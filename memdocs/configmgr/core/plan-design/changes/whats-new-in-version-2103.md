---
title: What's new in version 2103
titleSuffix: Configuration Manager
description: Get details about changes and new capabilities introduced in version 2103 of Configuration Manager current branch.
ms.date: 03/26/2021
ms.prod: configuration-manager
ms.technology: configmgr-core
ms.topic: conceptual
ms.assetid: 8c65ba1a-152e-467a-ac56-c46efe2e7f0d
author: mestew
ms.author: mstewart
manager: dougeby 
---

# What's new in version 2103 of Configuration Manager current branch

*Applies to: Configuration Manager (current branch)*

Update 2103 for Configuration Manager current branch is available as an in-console update. Apply this update on sites that run version 1910 or later. <!-- baseline only statement:--> When installing a new site, it's also available as a baseline version. This article summarizes the changes and new features in Configuration Manager, version 2103.

> [!NOTE]
> To better align with other releases within Microsoft Endpoint Manager, starting this year the current branch version names will be 2103, 2107, and 2111. They will still release every four months, and release at the same time of the year.

Always review the latest checklist for installing this update. For more information, see [Checklist for installing update 2103](../../servers/manage/checklist-for-installing-update-2103.md). After you update a site, also review the [Post-update checklist](../../servers/manage/checklist-for-installing-update-2103.md#post-update-checklist).

To take full advantage of new Configuration Manager features, after you update the site, also update clients to the latest version. While new functionality appears in the Configuration Manager console when you update the site and console, the complete scenario isn't functional until the client version is also the latest.

> [!TIP]
> To get notified when this page is updated, copy and paste the following URL into your RSS feed reader:
> `https://docs.microsoft.com/api/search/rss?search=%22what%27s+new+in+version+2103+-+Configuration+Manager%22&locale=en-us`

## Microsoft Endpoint Manager tenant attach

### Required application deployments display in Microsoft Endpoint Manager admin center
<!--8795301-->
Applications targeted to a device or a user with a required deadline will now show in the **Applications** view for a tenant attached device in Microsoft Endpoint Manager admin center. This improvement allows you to review when application installations are expected to occur on a device. The **An administrator must approve a request for this application on the device** option is no longer required to be set on the device available deployment for applications to be listed in the admin center.


## Cloud-attached management


## Desktop Analytics

For more information on the monthly changes to the Desktop Analytics cloud service, see [What's new in Desktop Analytics](../../../desktop-analytics/whats-new.md).


## Site infrastructure

### Simplified CMPivot permissions requirements
<!--7898885-->
We've simplified the CMPivot permissions requirements. The following changes have been made:
- CMPivot no longer requires **SMS Scripts** read permission
   - The [administration service](../../../develop/adminservice/overview.md) still requires this permission when falling back to use the SMS Provider when a 503 (Service Unavailable) error is received, as seen in the CMPivot.log
- The **default scope** permission isn’t required.

### Allow exclusion of organizational units (OU) from Active Directory User Discovery
<!--5193509-->
You can now exclude OUs from [Active Directory User Discovery](../../servers/deploy/configure/configure-discovery-methods.md#bkmk_config-adud).

### Prerequisite rule for deprecated Azure Monitor connector

<!--8269855-->

We continue to see broad adoption of native Azure Monitor log query groups as customers shift more of their workloads to the cloud. Because of this reason, starting in November 2020, the Configuration Manager feature to synchronize collections to Azure Monitor is deprecated.

When you update to this release, a new prerequisite check warns about the presence of the [Log Analytics connector for Azure Monitor](/azure/azure-monitor/platform/collect-sccm?context=%2fmem%2fconfigmgr%2fcore%2fcontext%2fcore-context). (This feature is called the *OMS Connector* in the Azure Services wizard.) This connector is deprecated, and will be removed from the product in a future release. At that time, this check will be an error that blocks upgrade.

### Manage aged distribution point messages

<!--8561493-->

In some environments, a small number of error status messages for a distribution point can cause the entire server status to show an error state. This behavior could be due to a known issue when failing to distribute content. To help you better manage the state of distribution points, this release includes the following improvements:

- Delete a specific distribution point status message. Go to the **Monitoring** workspace, expand **Distribution Status**, and select **Distribution Point Configuration Status**. Select a server, and switch to the **Details** tab in the bottom pane. Select a message, and then in the ribbon, select **Delete**.

- Delete aged status messages for a specific distribution point. In the same **Distribution Point Configuration Status** node, select a server. In the ribbon, select **Delete Aged Status Messages**. Then choose a timeframe for messages to delete: older than 30 days, older than 90 days, older than one year.

- Configure a site maintenance task to **Delete Aged Distribution Point Messages**. For more general information on tasks, see [Maintenance tasks](../../servers/manage/maintenance-tasks.md).

## Client management

### Software Center notifications display with logo

<!--4993167-->

If you enable Software Center customizations, then notifications on Windows 10 devices display the logo that you configure in client settings. This change helps users to trust these notifications.

### Improved user experience and security with Software Center custom tabs

<!--8655543-->

Since current branch version 1906, you can add up to five custom tabs to Software Center. These custom tabs let you give your users easy access to common web apps and other sites. Previously, to display websites Software Center used the Windows built-in Internet Explorer browser control.

Starting in this release, Software Center can now use the Microsoft Edge WebView2 browser control. The WebView2 browser control provides improved security and user experience. For example, more websites should work with these custom tabs without displaying script errors or security warnings.

### Client setting for displaying Software Center custom tabs

<!--9142301-->

Technical preview version 2012 included an improved user experience and security with Software Center custom tabs. It required that you manually deploy the Microsoft Edge WebView2 browser control.

Starting in this release, you can now configure a client setting to use the WebView2 runtime.

### Improvements to Support Center

<!--8272488-->

When you troubleshoot software update deployments, you might ask the following questions:

- Are any updates missing or failing?
- Are specific updates deployed to a device?
- What are the error codes for a deployment?
- Why didn't the device reboot during a maintenance window?
- What's the current state of an update?

Support Center already shows updates that are targeted to the device but not yet installed. Now in this release, select **All Updates** on the **Content** tab of Support Center to show all updates targeted to the device. To help you troubleshoot, this list includes information about whether the update is installed or missing.

Also on the **Content** tab, select **Maintenance Windows** to show the available maintenance windows for the device.

### Changes to Support Center

<!--8693068-->

Support Center is now split into the following tools:

- **Support Center Client Data Collector**: Collects data from a device to view in the Support Center Viewer. This separate tool encompasses the existing Support Center action to [Collect selected data](../../support/support-center-ui-reference.md#collect-selected-data).

- **Support Center Client Tools**: The other Support Center troubleshooting functionality, except for **Collect selected data**.

The following tools are still a part of Support Center:

- **Support Center Viewer**
- **Support Center OneTrace**
- **Support Center Log File Viewer**

### OneTrace support for jump lists

<!--6991505-->

[Support Center OneTrace](../../support/support-center-onetrace.md) now supports jump lists for recently opened files. Jump lists let you quickly go to previously opened files, so you can work faster.

There are now three methods to open recent files in OneTrace:

- Windows taskbar jump list
- Windows Start menu recently opened list
- In OneTrace from **File** menu or **Recently opened** tab.


## Collections


## Application management

### Disable application deployments

<!--8354812-->

You can now disable application deployments. Other objects already have similar behaviors:

- Software update deployments: Disable the deployment
- Phased deployments: Suspend the phase
- Package: Disable the program
- Task sequence: Disable the task sequence
- Configuration baseline: Disable the baseline

For device-based deployments, when you disable the deployment or object, use the client notification action to **Download Computer Policy**. This action immediately tells the client to update its policy from the site. If the deployment hasn't already started, the client receives the updated policy that the object is now disabled.


## OS deployment

### Windows 10 Servicing dashboard changes
<!--3555940-->
We've simplified the Windows 10 Servicing dashboard to make it more relevant. The new **Quality Update Versions** chart displays the top five revisions of Windows 10 across your devices. The **Latest Feature Update** chart shows the number of devices that installed the latest feature update. The **Windows 10 Usage** chart, showing the distribution of Windows 10 major releases, was renamed to **Feature Update Versions**. Servicing plan and Windows 10 ring information were removed from the dashboard.

### Deploy a feature update with a task sequence

<!--3555906-->

You can now upgrade a client's Windows OS by using a feature update deployed with a task sequence. This integration combines the simplicity of Windows servicing with the flexibility of task sequences. Servicing uses a single ESD file that you synchronize through the software update point. This process simplifies the need to manually get, import, and maintain the Windows image content used with a standard task sequence to upgrade Windows. The size of the ESD file is generally smaller than the WIM image file.

### Task sequence error shows more check readiness details

<!--8888218-->

The task sequence progress can now display more information about readiness checks. If a task sequence fails because the client doesn't meet the requirements configured in the **Check readiness** task sequence step, the user can now see more details about the failed prerequisites. They still see the common "task sequence error" message, but can now select an option to **Inspect**. This action shows the checks that failed on the device.

### Encryption algorithm to capture and restore user state

<!--9171505-->

The task sequence steps to **Capture User State** and **Restore User State** always encrypt the USMT state store. Previously, Configuration Manager configured USMT to use the 3DES algorithm. Starting in this release, both steps now use the highest supported encryption algorithm, **AES 256**.

### Improvements to OS deployment

This release includes the following improvements to OS deployment:

- Task sequence conditions now include a **not like** operator. This operator applies to task sequence variable conditions. It's also used in the [Set Dynamic Variable](../../../osd/understand/task-sequence-steps.md#BKMK_SetDynamicVariables) task sequence step.<!--8764365-->

- This release fixes issues with the [SMSTSDisableStatusRetry](../../../osd/understand/task-sequence-variables.md#SMSTSDisableStatusRetry) task sequence variable.<!-- 8727206 -->

- The [Check Readiness](../../../osd/understand/task-sequence-steps.md#BKMK_CheckReadiness) task sequence step now also checks free space on disks without partitions.<!-- 8751864  -->

- The following PowerShell cmdlets now have an **Index** parameter:<!-- 8559060 -->

  - [New-CMOperatingSystemImage](/powershell/module/configurationmanager/new-cmoperatingsystemimage): When you run this cmdlet with the new **Index** parameter, it creates a new single-index image file in the same source folder.
  - [New-CMOperatingSystemInstaller](/powershell/module/configurationmanager/new-cmoperatingsysteminstaller) (alias **New-CMOperatingSystemUpgradePackage**): When you run this cmdlet with the new **Index** parameter, it replaces the original image file in the source folder with a single-index image file.

- The following new cmdlets are available to get the list of existing hardware IDs in the site database:<!-- 8702570 -->

  - **Get-CMDuplicateHardwareIdGuid**
  - **Get-CMDuplicateHardwareIdMacAddress**

  These new cmdlets supplement the existing cmdlets to add and remove duplicate IDs. For more information, see [Version 1910 PowerShell release notes](/powershell/sccm/1910-release-notes#new-cmdlets).


## Protection


## Software updates

## Approved scripts for orchestration groups
<!--6991647-->
You can now select from scripts that have already been approved when configuring pre and post-scripts for an [orchestration group](../../../sum/deploy-use/orchestration-groups.md). When in the **Create Orchestration Group Wizard**, you'll see a new page called **Script Picker**. Select your pre and post scripts from your list of scripts that are already approved. You can still add scripts manually on the pre and post-script pages. Additionally, you can also edit scripts that you pre-populated from the **Script Picker**.


## Configuration Manager console

### Get console extensions from the Community hub

<!--8116426-->

The [Community hub](../../servers/manage/community-hub.md) now supports sharing extensions to the Configuration Manager console. When you get an extension from the hub, it's available in a new **Console extensions** node in the console. Getting an extension from the hub doesn't make it immediately available. First, an administrator has to approve the extension for the site. Then console users can install the extension to their local console.

### Console extension installation
<!--3555909-->
You can now download console extensions from the [Community hub](../../servers/manage/community-hub.md) and have it applied to all consoles connected to a hierarchy. This improvement allows you to start managing the approval and installation of console extensions used in your environment. In this technical preview, only [Right Click Tools (Community Edition) from Recast Software](https://www.recastsoftware.com/enterprise?utm_source=microsoft&utm_medium=referral&utm_campaign=commhub) is available for download and installation. This version of the Right Click Tools extension isn't a final production version. This extension is for technical preview environments only and will expire on April 1, 2021.

### Access the top queries shared in the Community hub from CMPivot
<!--7137169-->

You can now access the top CMPivot queries shared in the Community hub from on-premises CMPivot. By leveraging pre-created CMPivot queries shared by the broader community, CMPivot users gain access to a wider variety of queries. On-premises CMPivot accesses the Community hub and returns a list of the top downloaded CMPivot queries. Users can review the top queries, customize them, and then run on-demand. This improvement gives a wider selection of queries for immediate usage without having to construct them and also allows information sharing on how to build queries for future reference.

### Community hub support for application content

<!--7983035-->

This release continues to iterate on the scenario to share apps via the [Community hub](../../servers/manage/community-hub.md). Previously you could share just the definition of the app. Another hub user could download the app's XML metadata, and create it in their site. But to actually deploy the app, they would then need to locate the app's content.

### Improvements to the product lifecycle dashboard

<!--8160460-->

This release includes improvements to the [product lifecycle dashboard](../../clients/manage/asset-intelligence/product-lifecycle-dashboard.md) to make it more actionable for you.

- Customize the timeframe on the charts for your preference.
- Search, sort, and filter the data.
- View a list of devices with products that are near or at end of support, and you need to update.


## Content management

### Improvements to client data sources dashboard


## PowerShell

### Update PowerShell help

<!-- will need to say something about the changes here...
You can now use the [Update-Help](/powershell/module/microsoft.powershell.core/update-help) cmdlet to get the latest information for the Configuration Manager PowerShell module. This content is the same as what's published on docs.microsoft.com for the [ConfigurationManager module](/powershell/module/configurationmanager/).

For more information, see [Configuration Manager PowerShell cmdlets: Update help](/powershell/sccm/overview#update-help).
 -->


## Deprecated features

Learn about support changes before they're implemented in [removed and deprecated items](deprecated/removed-and-deprecated.md).

The following features are now deprecated:

<!-- - The [collection evaluation viewer](../../support/ceviewer.md)<!-- 8509484 -->



<!--
As first announced in version 1906, version 2103 drops support for the following client OS versions:  

- Windows CE 7.0
- Windows 10 Mobile
- Windows 10 Mobile Enterprise
 -->

## Other updates

<!--
Starting with this version, the following features are no longer [pre-release](../../servers/manage/pre-release-features.md):

### Azure Active Directory user group discovery](../../servers/deploy/configure/configure-discovery-methods.md#bkmk_azuregroupdisco)<!--3611956
-->

For more information on changes to the Windows PowerShell cmdlets for Configuration Manager, see [version 2103 release notes](/powershell/sccm/2103-release-notes).

<!-- For more information on changes to the administration service REST API, see [Administration service release notes](../../../develop/adminservice/release-notes.md). -->

<!-- Aside from new features, this release also includes additional changes such as bug fixes. For more information, see [Summary of changes in Configuration Manager current branch, version 2103](https://support.microsoft.com/help/4599442).
 -->
<!--
The following update rollup (4517869) is available in the console starting on October 1, 2019: [Update rollup for Configuration Manager current branch, version 1906](https://support.microsoft.com/help/4517869).
-->

<!-- ### Hotfixes

The following additional hotfixes are available to address specific issues:

| ID | Title | Date | In-console |
|---------|---------|---------|---------|
| [4594177](https://support.microsoft.com/help/4594177) | Client notifications sent to all collection members in Configuration Manager current branch, version 2103 | January 12, 2021 | Yes |
 -->

## Next steps

At this time, version 2103 is released for the early update ring. To install this update, you need to opt in. For more information, see [Early update ring](../../servers/manage/checklist-for-installing-update-2103.md#early-update-ring).

<!-- As of December 11, 2020, version 2103 is globally available for all customers to install. -->

When you're ready to install this version, see [Installing updates for Configuration Manager](../../servers/manage/updates.md) and [Checklist for installing update 2103](../../servers/manage/checklist-for-installing-update-2103.md).

> [!TIP]
> To install a new site, use a baseline version of Configuration Manager.
>
> Learn more about:
>
> - [Installing new sites](../../servers/deploy/install/installing-sites.md)
> - [Baseline and update versions](../../servers/manage/updates.md#bkmk_Baselines)

For known significant issues, see the [Release notes](../../servers/deploy/install/release-notes.md).

After you update a site, also review the [Post-update checklist](../../servers/manage/checklist-for-installing-update-2103.md#post-update-checklist).
