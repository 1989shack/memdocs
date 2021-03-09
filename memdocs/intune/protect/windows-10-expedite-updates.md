---
# required metadata

title: Use Intune to expedite Windows 10 quality updates - Azure | Microsoft Docs
description: Use Microsoft Intune policy to expedite the installation of Windows 10 updates on managed devices as soon as possible.
keywords:
author: brenduns
ms.author: brenduns
manager: dougeby
ms.date: 03/22/2021
ms.topic: how-to
ms.service: microsoft-intune
ms.subservice: protect
ms.localizationpriority: high
ms.technology:

# optional metadata

#ROBOTS:
#audience:

ms.reviewer: davguy;bryanke
ms.suite: ems
search.appverid: MET150
#ms.tgt_pltfrm:
#ms.custom:
ms.collection: M365-identity-device-management
---

# Expedite Windows 10 quality updates in Intune

*This feature is in public preview.*

With Windows 10 quality updates policy, you can expedite the install of the most recent Windows 10 quality updates as quickly as possible on devices you manage with Microsoft Intune. Deployment of expedited updates is done without the need to pause or edit your existing monthly servicing policies. For example, you might expedite a specific update to mitigate a security threat when your normal update process wouldn’t deploy the update for some time.

Not all updates can be expedited. Currently, only Windows 10 quality updates that can be expedited are available to deploy with Windows 10 quality updates policy. To manage regular monthly quality updates, use [Windows 10 update rings policies](../protect/windows-10-update-rings.md).

## How expedited updates work

With expedited updates, you can speed installation of quality updates like the most recent *patch Tuesday* release or an out-of-band security update for a zero-day flaw.

To speed installation, expedite updates uses available services, like WNS and http://channels, to deliver the message to devices that there's an expedited update to install. This process enables devices to start the download and install of an expedited update as soon as possible, without having to wait for the device to check in for updates.

The actual time that a device starts to update depends on the device being online, its scan timing, whether communication channels to the device are functioning, and other factors like cloud-processing time.

- For each expedite update policy you select a single update to deploy based on its release date. By using the release date, you don’t have to create separate policies to deploy different instances of that update to devices that have different versions of Windows 10, like 1809, 1909, and so on.

- Windows Update evaluates the specified update and then ensures each device that receives the expedite update policy installs the instance that applies to the device.

- Only devices that need the update receive the expedited update:
  - Windows Update doesn’t try to expedite the update for devices that already have a revision that’s equal to or greater than the update version.
  - For devices with a lower build version than the update, Windows Update confirms that the device still requires the update before installing it.

  > [!IMPORTANT]
  > In some scenarios, Windows Update can install an update that is more recent than the update you specify in expedite update policy. For more information about this scenario, see [About installing the latest applicable update](#identify-the-latest-applicable-update), later in this article.

- Expedite update policies ignore and override any quality [update deferral periods](/windows/client-management/mdm/policy-csp-update#update-deferqualityupdatesperiodindays) for the update version you deploy. You can configure quality updates deferrals by using Intune [Widows 10 update rings](../protect/windows-10-update-rings.md) and the setting for **Quality update deferral period**.

- When a restart is required to complete installation of the update, the policy helps to manage the restart. In the policy, you can configure a period that users have to restart a device before the policy forces an automatic restart. Users can also choose to schedule the restart or let the device try to find the best time outside of the devices *Active Hours*. Before reaching the restart deadline, the device displays notifications to alert device users about the deadline and includes options to schedule the restart.

  If a device doesn’t restart before the deadline, the restart can happen in the middle of the working day. For more information on restart behavior, see [Enforcing compliance deadlines for updates](/windows/deployment/update/wufb-compliancedeadlines).

## Prerequisites

The following are requirements to qualify for installing expedited quality updates with Intune:

**Supported Windows 10 versions**:

- Windows 10 versions that remain in support for Servicing, on x86 or x64 architecture

**Supported Windows 10 editions**:

- Professional
- Enterprise
- Pro Education
- Education

**Devices must**:

- Be [enrolled in Intune](../enrollment/device-enrollment.md) MDM, or be [co-managed](../../configmgr/comanage/overview.md) with the [Windows Update policies](../../configmgr/comanage/workloads.md#windows-update-policies) workload set to Intune.

- Have access to the following endpoints:

  - [Windows Update](/windows/privacy/manage-windows-1809-endpoints#windows-update)

    - *.prod.do.dsp.mp.microsoft.com
    - *.windowsupdate.com
    - *.dl.delivery.mp.microsoft.com
    - *.update.microsoft.com
    - *.delivery.mp.microsoft.com
    - tsfe.trafficshaping.dsp.mp.microsoft.com

  - WUfB-DS

    - devicelistenerprod.microsoft.com
    - login.windows.net
    - payloadprod*.blob.core.windows.net

  - [Windows Push Notification Services](/windows/uwp/design/shell/tiles-and-notifications/firewall-allowlist-config): *(Recommended, but not required. Without this access, devices might not expedite updates until their next daily check for updates.)*
    - *.notify.windows.com

- Be Azure Active Directory (AD) Joined, or Hybrid Azure AD Joined. Workplace Join isn't supported.

- Have installed the update described in [KB 4023057 - Update for Windows 10 Update Service components](https://support.microsoft.com/topic/kb4023057-update-for-windows-10-update-service-components-fccad0ca-dc10-2e46-9ed1-7e392450fb3a), or a more recent quality update for a Windows 10 version of 1809 or later. Both options will install the *Update Health Tools*. To confirm the presence of the Update Health Tools on a device, look for the folder **C:\Program Files\Microsoft Update Health Tools**.

  To confirm the presence of the Update Health tools, device administrators can use one of the following methods:  
  - On a device, review *Add Remove Programs* for **Microsoft Update Health Tools**
  - Run the following script in WMI:

    `Get-WmiObject -Class Win32_Product | Where-Object {$_.Name -match "Microsoft Update Health Tools"}`

    Example results:  
    :::image type="content" source="./media/windows-10-expedite-updates/example-wmi-query-results.png" alt-text="Example results for the WMI query":::

**Device settings**:

The following settings on devices should be configured as follows. Review and apply policies to devices to help avoid conflicts or configurations that can block installation of expedited updates. For more information about these settings, see [Policy CSP – Update](/windows/client-management/mdm/policy-csp-update).

| MDM Setting Name  |  Equivalent Group Policy Name       |
|-------------------|-------------------------------------|
| **AllowUpdateService** - This policy enables the admin to specify whether they want devices to get updates from the Windows Update endpoint directly, or if they want to get updates only from their specified intranet Microsoft update service location. </br></br> Expedite is only applicable to devices scanning Windows Update directly. </br></br> Use one of the following options: </br> - **Not configured** </br> - **1 Update Service is Allowed** (default)  |  **CorpWuURL** - Specify intranet Microsoft update service location. </br></br> Use one of the following options: </br> - The intranet update location as blank string, or **Not configured** </br> - **1 - Update Service is Allowed** (default)      |
|**AllowAutoUpdate** - This policy controls how the device downloads and installs updates. For a best experience, Microsoft recommends not configuring this value or configuring it to the default. Use of either configuration ensures that updates automatically download and install at a good time for the end user while having few negative impacts on compliance. </br></br> Expedite can only be effective when the device is automatically downloading and installing updates. If you require end-user interaction, such as notify to download or notify to install, you might be hurting your compliance goals. </br></br> Use one of the following options: </br> - **Not configured** </br> -**2 Auto install and restart** (default)  |  **AutoUpdateCfg** - Configure Automatic Updates. </br></br> Use one of the following options: </br> - **Not configured** </br> - **4 - Schedule an install time with *Scheduled install day* = 0** (every day), and ***Scheduled Install time*** **= Automatic**                                  |
| **BranchReadinessLevel** - This policy specifies which branch a device receives their updates from. </br></br> Use one of the following options: </br> - **Not configured** </br> - **16 - Semi-Annual Channel** (default)  |  **DeferFeatureUpdates** - Select when Preview Builds and Feature Updates are received. </br></br> Use one of the following options: </br> - **Not configured** </br> - **Semi-Annual Channel**      |
| **DisableDualScan** -This policy specifies whether the device gets its Windows Updates from Windows Update and all other updates from the specified server, or to get all updates from the specified server including Windows Updates. </br></br> For expedite to work, devices must scan against Windows Update directly for their Windows updates. </br></br> Use one of the following options: </br> - **Not configured** </br> -**0 – Allow scans against Windows Update**  |  **Disable Dual Scan** - Don't allow update deferral policies to cause scans against Windows Update. </br></br> Use one of the following options: </br> - **Not configured** </br>  - **0 - Allow scan against Windows Update**       |
| **RequireUpdateApproval** - This policy isn't supported for desktop devices. Instead of this policy, use the policy *Update CSP deferrals* or target versions to manage when and which updates are offered. </br></br> Set to: </br> - **Not configured**  |  *Not applicable*     |

## Create and assign an expedited quality update

1. Sign in to the [Microsoft Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431).

2. Select **Devices** > **Windows** > **Windows 10 quality updates (preview)** > **Create profile**.

   :::image type="content" source="./media/windows-10-expedite-updates/create-quality-update-profile.png" alt-text="Screen capture of the Create profile UI":::

3. In **Settings**, enter the following properties to identify this profile:

   - **Name**: Enter a descriptive name for the profile. Name your profiles so you can easily identify them later.

   - **Description**: Enter a description for the profile. This setting is optional but recommended.

4. In **Settings**, configure **Expedite installation of quality updates if device OS version less than**. Select the update that you want to expedite from the drop-down list. The list includes only the updates you can expedite.

   > [!TIP]
   > Optional Windows 10 quality updates can’t be expedited and won’t be available to select. Instead, use [Windows 10 update rings policies](../protect/windows-10-update-rings.md) to deploy them.

   :::image type="content" source="./media/windows-10-expedite-updates/select-update.png" alt-text="Screen capture of update selection UI":::

   When selecting an update:

   - Updates are identified by their release date, and you can select only one update per policy.

   - Updates that include the letter **B** in their name identify updates that released as part of a *patch Tuesday* event. The letter B identifies that the update released on the second Tuesday of the month.

   - Security updates for Windows 10 that release out of band from a *patch Tuesday* can be expedited. Instead of the letter B, *out-of-band* patch releases have different identifiers.

   - When the update deploys, Windows Update ensures that each device that receives the policy installs a version of the update that applies to that devices current Windows version, like version 1809, 2004, and so on.

   > [!TIP]
   > For more information, see the blog [Windows 10 update servicing cadence - Microsoft Tech Community](https://techcommunity.microsoft.com/t5/windows-it-pro-blog/windows-10-update-servicing-cadence/ba-p/222376).

5. In **Settings**, configure **Number of days to wait before forced reboot**. For this setting, select how soon after installing the update a device will automatically restart to complete the update installation. You can select from zero to two days. The automatic restart is canceled if a device manually restarts before the deadline. If an update doesn’t require a restart, this setting isn’t enforced.

   - A setting of **0 days** means that as soon as the device installs the update, the user is notified about the restart and has limited time to save their work.

   > [!IMPORTANT]
   > This experience can impact user productivity. Consider using it for those devices or updates that must complete and restart the device as soon as possible.

   - A setting of **1 day** or **2 days** provides device users flexibility to manage a restart before it’s forced. These settings correspond to an automatic restart delay of 24 or 48 hours after the update installs on the device.

     :::image type="content" source="./media/windows-10-expedite-updates/select-reboot-time.png" alt-text="Screen capture of selecting days before forced reboot":::

6. In **Assignments**, select **Add groups** and then select device or user groups to assign the policy.

7. In **Review + create**, select **Create**. After the policy is created, it deploys to assigned groups.

## Identify the latest applicable update

There are some scenarios when your policy to expedite an update results in the installation of a more recent update than specified in policy. This result occurs when the newer update includes and surpasses the specified update, and that newer update is available before a device checks in to install the update that's specified in the expedite update policy. A detailed [example](#example-of-installing-an-expedited-update) of this scenario is provided later in this article.

Installing the most recent quality update reduces disruptions to the device and user while applying the benefits of the intended update. This avoids having to install multiple updates, which each might require separate reboots.

A more recent update is deployed when the following conditions are met:

- The Windows build revision of a device is less than the Windows build version of the update specified in the policy.
- Before the device begins to install an expedited update:
  - One or more newer updates are released and available from Windows Update that support being expedited.
  - Those newer updates include the updates that were provided by the update you’ve specified in your policy.

- The device isn't targeted with a deferral policy that blocks installation of a more recent update. In this case, the most recently available update that isn't deferred is the update that installs.

While expedite update policies will override an update deferral for the update version that’s specified in the policy, they don’t override deferrals that are in place for any other update version.

### Example of installing an expedited update

The following sequence of events provides an example of how two devices, named *Test-1* and *Test-2*, install an update based on a *Windows 10 quality update policy* that's assigned to the devices.

1. Each month, Intune administrators deploy the most recent Windows 10 quality updates on the fourth Tuesday of the month. This period gives them two weeks after the patch Tuesday event to validate the updates in their environment before they force installation of the update.

2. On January 19, 2021, device *Test-1* and *Test-2* install the latest quality update from the patch Tuesday release on January 12. The next day, both devices are turned off by their users who are each leaving on vacation.

3. On the February 9, the Intune admin creates policy to expedite installation of the patch Tuesday release **02/09/2021 – 2021.02 B Security Updates for Windows 10** to help secure company devices against a critical threat that the update resolves. The expedite policy is assigned to a group of devices that includes both *Test-1* and *Test-2*. All devices in that group that are active receive and install the expedited update policy.

4. On the March 9 patch Tuesday event, a new quality update releases as **03/09/2021 – 2021.03 B Security Updates for Windows 10**. There are no critical issues that require an expedited deployment of this update, but admins do find a possible conflict. To provide time to review the possible issue, admins use a Windows 10 update ring policy to create a seven-day deferral policy. All managed devices are prevented from installing this update until March 14.

5. Now consider the following results for *Test-1* and *Test-2*, based on when each is turned back on:

   - **Test-1** - On March 12, *Test-1* is powered back on, connects to the network, and receives expedited update notifications:  
     1. Windows Update determines that *Test-1* still needs to expedite the update installation, per policy.
     2. Because the March 9 update supersedes the February update, Windows Update could install the March 9 update.
     3. There's an active deferral for the March update that won't expire until March 14.

     **Result**: With the deferral policy for the March update still active and blocking installation of that update, *Device-1* installs the February update as configured in policy.

   - **Test-2** - On March 20, *Test-2* is powered back on, connects to the network, and receives expedited update notifications:  
     1. Windows Update determines that *Test-2* still needs to expedite the update installation, per policy.
     2. Because the March 9 update supersedes the February update, Windows Update could install the March 9 update.
     3. There's no longer an active deferral for the March update.

     **Result**: With the deferral policy for the March update having expired, *Test-2* installs the more recent March update, skipping over the February update and installing a later update than was specified in policy.

## Manage policies to expedite quality updates

In the admin center, go to **Devices** > **Windows** > **Windows 10 quality updates** and select the policy that you want to manage. The policy opens to its **Overview** pane.

From this pane, you can:

- Select **Delete** to delete the policy from Intune. Deleting a policy removes it from Intune but won’t result in the update uninstalling if it has already completed installation. Windows Update will attempt to cancel any in-progress installations, but a successful cancellation of an in-progress install can’t be guaranteed.

- Select **Properties** to modify the deployment. On the *Properties* pane, select **Edit** to open the *Settings*, *Scope tags*, or *Assignments*, where you can then modify the deployment.

## Monitoring and reporting

After a policy has been created you can monitor results, update status, and errors from the following reports.

### Summary report

This report shows the current state of all devices in the profile and provides an overview of how many devices are in progress of installing an update, have completed the installation, or have an error.

1. Sign in to the [Microsoft Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431).

2. Select **Reports** > **Windows updates**.

3. In the main pane, Select the **Windows Expedited Updates** report.

4. Click the link **Select an expedited update profile**.

5. From the list of profiles that is shown on the right side of the page, select a profile to see results.

6. Select the **Generate report** button.

### Device report

This report can help you find devices with alerts or errors and can help you troubleshoot update issues.

1. Sign in to the [Microsoft Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431)

2. Select **Devices** > **Monitor**.

3. In the list of monitoring reports, scroll to the Software updates section and select **Expedited update failures**.

4. From the list of profiles that is shown on the right side of the page, select a profile to see results.

### Update states

|  Update State  |  Update SubState  |  Definition  |
|------------|------------------|-------------------|
| Pending    | Validating       | The device has been added to the policy in the service and validation that the device can be expedited has begun.  |
| Pending    | Scheduled        | Device has passed validation and will be expedited. |
| Offering   | OfferReady       | The expedite instructions have been sent to the device. |
| Installing | OfferReceived    | Device scanned against Windows Update and the update is applicable but hasn't yet begun to download. |
| Installing | DownloadStart    | The device has begun to download the update. |
| Installing | DownloadComplete | The device has downloaded the update. |
| Installing | InstallStart     | The device has begun to install the update. |
| Installing | InstallComplete  | The device has completed installing the update. Unless the update has an update error, the device should move quickly to *RestartRequired* or *UpdateInstalled*. |
| Installing | RestartRequired  | The installation is complete and requires a restart. |
| Installing | RestartInitiated | The device has begun a restart. |
| Installing | RestartComplete  | The device has completed the restart. |
| Installing | UpdateInstalled  | Update has successfully completed. |
 
## Next steps

- Configure [Widows 10 update rings](../protect/windows-10-update-rings.md)
- Configure [Windows 10 feature updates](../protect/windows-10-feature-updates.md)
- View [Windows 10 release information](/windows/release-information/)
