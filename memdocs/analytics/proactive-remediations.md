---
title: Tutorial - Proactive remediations
titleSuffix: Configuration Manager
description: A tutorial on using Proactive remediations to enhance the user 
ms.date: 06/25/2019
ms.prod: configuration-manager
ms.technology: configmgr-analytics
ms.topic: tutorial
ms.assetid: 1426b6e4-fd29-4a59-b6be-2302281118b0
author: mestew
ms.author: mstewart
manager: dougeby

# Customer intent: As a Microsoft Endpoint Manager administrator, I want to enable and use Proactive remediations in Endpoint analytics so that I can fix common support issues before end-users notice issues.
---

# Tutorial: Proactive remediations

Proactive remediations in Endpoint analytics helps you fix common support issues before end-users notice issues. Use Proactive remediations to help increase your [User experience score](enroll-intune.md#bkmk_view).


In this tutorial, you learn how to:  

> [!div class="checklist"]  
> * Review prerequisites for Proactive remediations
> * Deploy a built-in script package
> * Deploy a custom script package
> * Monitor the script packages  

## <a name="bkmk_prs"></a> About Proactive remediations

Proactive remediations are script packages that can detect and fix common support issues on a user's device before they even realize there's a problem. These remediations can help reduce support calls. You can create your own script package, or deploy one of the script packages we've written and used in our environment for reducing support tickets.

Each script package consists of a detection script, a remediation script, and metadata. Through Intune, you'll be able to deploy these script packages and see reports on their effectiveness. We're actively developing new script packages and would like to know your experiences using them. Reach out to your Endpoint analytics preview contact if you have any feedback on the script packages.

## <a name="bkmk_prereq"></a> Prerequisites

- Devices enrolled into Endpoint analytics.
   - For more information, see [Enroll Configuration Manager managed devices](enroll-configmgr.md) and [Enroll Intune managed devices](enroll-intune.md).

Whether enrolling devices via Intune or Configuration Manager, Proactive remediation scripting has the following requirements:
- Devices must be Azure AD joined or hybrid Azure AD joined and meet one of the following conditions:
- A Windows 10 Enterprise, Professional, or Education device that is managed by Intune
- A [co-managed](../configmgr/comanage/overview.md) device running Windows 10 Enterprise, version 1607 or later with the [Client apps workload](../configmgr/comanage/workloads.md#client-apps) pointed to Intune.
- A [co-managed](../configmgr/comanage/overview.md) device running Windows 10 Enterprise, version 1903 or later with the [Client apps workload](../configmgr/comanage/workloads.md#client-apps) pointed to Configuration Manager.

### Licensing

Proactive remediations also requires the [licensing for Endpoint analytics](enroll-intune.md#bkmk_prereq) and one of the following licenses for the enrolled devices:

- Windows 10 Enterprise E3 or E5 (included in Microsoft 365 F3, E3, or E5)
- Windows 10 Education A3 or A5 (included in Microsoft 365 A3 or A5)
- Windows Virtual Desktop Access E3 or E5

### Permissions

- For Proactive remediations, the user needs permissions appropriate to their role under the **Device configurations** category. Permissions in the **Endpoint Analytics** category aren't needed if the user only uses Proactive remediations.

- An [Intune Service Administrator](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#intune-service-administrator-permissions) is required to confirm licensing requirements before using proactive remediations for the first time.

- The PowerShell execution policy on the device can't be set to **Restricted** or **AllSigned**. For more information, see [PowerShell execution policies](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_execution_policies#powershell-execution-policies).


### Script requirements

If the option **Enforce script signature check** is enabled in the [Settings](#bkmk_prs_deploy) page of creating a script package, then make sure that the scripts are:
- Encoded in UTF-8 not UTF-8 BOM
- Scripts have line breaks indicated by `LF` and not `CR LF`, which is the Windows default.
   - `LF` is the default line break for Unix. For more information, see [Encoding and line endings](https://docs.microsoft.com/visualstudio/ide/encodings-and-line-breaks?view=vs-2019).
   - Currently, the encoding and line breaks are a known issue.


## Deploy built-in script packages

There are built-in script packages you can use to get started with Proactive remediations. The **Microsoft Intune Management Extension** service gets the scripts from Intune and runs them. The scripts are rerun every 24 hours by default. The following built-in script packages just need to be assigned:

- **Update stale Group Policies** – Stale Group Policies can lead to helpdesk tickets related to connectivity and internal resource access.
- **Restart Office Click-to-run service** – When the Click-to-run service is stopped, Office apps fail to start leading to helpdesk calls.

To assign the script package:

1. From the **Proactive remediations** node, select one of the built-in script packages.
1. Select **Properties**, then next the **Assignments** heading, select **Edit**.
1. Choose the groups you want to **Assign to** and any **Excluded groups** for the script package.
1. If you would like to change the schedule, click the ellipses and choose **Edit** to specify your settings then **Apply** to save them.
1. When you're done, select **Review + save**.

## <a name="bkmk_prs_deploy"></a> Create and deploy custom script packages

The **Microsoft Intune Management Extension** service gets the scripts from Intune and runs them. The scripts are rerun every 24 hours. You can copy the provided scripts and deploy them, or you can create your own script packages. To deploy script packages, follow the instructions below:

### <a name="bkmk_prs_ps1"></a> Copy the provided detection and remediation scripts

1. Copy the scripts from the [PowerShell scripts](powershell-scripts.md#bkmk_ps_scripts) article.
    - Script files whose names start with `detect` are detection scripts. Remediation scripts start with `remediate`.
    - For a description of the scripts, see the [Script descriptions](powershell-scripts.md#bkmk_scripts).
1. Save each script using the provided name. The name is also in the comments at the top of each script.
    - You can use a different script name, but it won't match the name listed in the [Script descriptions](powershell-scripts.md#bkmk_scripts).

### Deploy the script packages

1. Go to the **Proactive remediations** node in the console.
1. Click the **Create script package** button to create a script package.
     [![Endpoint analytics Proactive remediations page. Select the create link.](media/proactive-remediations-create.png)](media/proactive-remediations-create.png#lightbox)
1. In the **Basics** step, give the script package a **Name** and optionally, a **description**. The **Publisher** field can be edited, but defaults to your tenant name. **Version** can't be edited.
1. On the **Settings** step, copy the text from the provided scripts or put your own scripts into the **Detection script** and **Remediation script** fields.
   - You need the corresponding detection and remediation script to be in the same package. For example, the `Detect_Expired_User_Certificates.ps1` detection script corresponds with the `Remediate_Expired_User_Certificates.ps1` remediation script.
       [![Endpoint analytics Proactive remediations script settings page.](media/proactive-remediations-script-settings.png)](media/proactive-remediations-script-settings.png#lightbox)
1. Finish the options on the **Settings** page with the following recommended configurations:
   - **Run this script using the logged-on credentials**: This setting is dependent on the script. For more information, see the [Script descriptions](powershell-scripts.md#bkmk_scripts).
   - **Enforce script signature check**: No
   - **Run script in 64-bit PowerShell**: No
1. Click **Next** then assign any **Scope tags** you need.
1. In the **Assignments** step, select the device groups to which you want to deploy the script package.
1. Complete the **Review + Create** step for your deployment.


## Monitor your script packages

1. Under **Reporting** > **Endpoint analytics - Proactive remediations**, you can see an overview of your detection and remediation status.
       [![Endpoint analytics Proactive remediations report, overview page.](media/proactive-remediations-report-overview.png)](media/proactive-remediations-report-overview.png#lightbox)
1. Click on **Device status** to get status details for each device in your deployment.
       [![Endpoint analytics Proactive remediations device status.](media/proactive-remediations-device-status.png)](media/proactive-remediations-device-status.png#lightbox)

## Next steps

- Get the [PowerShell scripts](powershell-scripts.md) for Proactive remediations.
- View [Recommended software](recommended-software.md).
- View [Startup performance](startup-performance.md)
