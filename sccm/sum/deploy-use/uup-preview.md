---
title: UUP Preview
titleSuffix: Configuration Manager
description: Instructions for preview of UUP integration
ms.date: 02/19/2019
ms.prod: configuration-manager
ms.technology: configmgr-sum
ms.topic: conceptual
ms.assetid: 0b0da585-0096-410b-8035-6b7a312f37f5
author: aczechowski
ms.author: aaroncz
manager: dougeby
ROBOTS: NOINDEX
ms.collection: M365-identity-device-management
---

# UUP private preview instructions

> [!Note]  
> This information relates to a preview feature which may be substantially modified before it's commercially released. Microsoft makes no warranties, express or implied, with respect to the information provided here.  

## Benefits

### Feature updates

Feature updates with the Windows 10 Unified Update Platform (UUP) are designed to alleviate multiple problems that customers have with servicing today. Try UUP feature updates, including:

- Upgrade straight to the latest security compliance level, you no longer need to install security updates immediately after upgrading to be compliant. Each month a new feature update will be published to include the latest cumulative security update. You won't need to redownload or distribute majority of the feature update content each month, only the security update component, which is also shared with the cumulative update.

- All Features on Demand (FODs) and language packs should be preserved and not lost during the upgrade process.

- Feature updates with UUP support express installation files, enabling clients to reduce the amount of content each client must download.

For more information on UUP, see the Windows blog post [An update on our Unified Update Platform (UUP)](https://blogs.windows.com/windowsexperience/2017/03/02/an-update-on-our-unified-update-platform-uup/).


### Cumulative updates

- Cumulative updates with UUP allow content for FODs and Language Packs to be distributed offline to enable end users to acquire them on demand without needing to go to the internet or tedious staging efforts by administrators.

- Cumulative updates with UUP include servicing stack updates with monthly cumulative security updates. This behavior solves difficulties with orchestrating these two updates. It makes sure that the servicing stack updates are in place to install cumulative updates without you needing to manage and orchestrate the relationships.



## Set up instructions

### 1. Send your WSUS ID to your UUP preview contact at Microsoft

In order to participate in the UUP private preview, you must share your WSUS ID with Microsoft so that we can whitelist your environment into the preview. Without this ID, you'll be unable to see any UUP updates until the preview goes public.

To retrieve your WSUS ID:

```PowerShell
$server = Get-WsusServer
$config = $server.GetConfiguration()
$config.ServerId

# also check MUUrl
$config.MUUrl
```

The **MUUrl** property should be `https://sws.update.microsoft.com`. To change it, see the resolution in the following support article: 
[WSUS synchronization fails with SoapException](https://support.microsoft.com/help/4482416/wsus-synchronization-fails-with-soapexception)


### 2. Update ConfigMgr

Make the following changes to your Configuration Manager site to support this UUP preview:


#### Diagnostics and usage data level
Consider increasing the Configuration Manager diagnostic and data usage level during this preview. The **Full** level helps Microsoft better analyze and troubleshoot issues with this new feature. For more information, see [Levels of diagnostic usage data collection for version 1810](/sccm/core/plan-design/diagnostics/levels-of-diagnostic-usage-data-collection-1810).


#### Update rollup for ConfigMgr 1810 (4486457)

1. Update the site with the update rollup for version 1810. For more information, see [Install in-console updates](/sccm/core/servers/manage/install-in-console-updates).  

2. Update clients.  

    - To simplify this process, consider using automatic client upgrade. For more information, see [Upgrade clients](/sccm/core/clients/manage/upgrade/upgrade-clients#automatic-client-upgrade).  

    - All clients you target UUP updates to must be upgraded to prevent **unnecessarily downloading around 6 GB** of unused content to the client.

For more information on this update, see [Update rollup for System Center Configuration Manager current branch, version 1810](https://support.microsoft.com/help/4486457).


<!-- 
#### 1812 Technical Preview
The 1812 Technical Preview is equivalent in supported UUP scenarios to the ConfigMgr 1810 UUP Hotfix (KB4482615).

The only note is that client upgrade of 1812 Technical Preview is broken from 1810.1 TP or 1811 TP. To work around this issue, you'll need to uninstall 1810.1 TP and 1811 TP clients, then install the 1812 TP client cleanly. All clients you target UUP updates to must be on 1812 Technical Preview (or later) to prevent **unnecessarily downloading around 6 GB** of unused content to the client.
 -->


### 3. Update Windows clients to supported versions

#### For express installation file sync
For express content, supported Windows versions include:

- **Windows 10 version 1809** with non-security cumulative update [KB4476976](https://support.microsoft.com/help/4476976/windows-10-update-kb4476976) (released 1/22) or later. This update is only available in the catalog and doesn't directly sync to WSUS. To import the update into your environment in order to deploy it, see [Import updates from the Microsoft Update Catalog](/sccm/sum/get-started/synchronize-software-updates#import-updates-from-the-microsoft-update-catalog).

- **Windows 10 version 1803** with [KB4284835](https://support.microsoft.com/help/4284835) (June 2017 cumulative security update) or later  

- **Windows 10 version 1709** with [KB4338825](https://support.microsoft.com/help/4338825) (July 2017 cumulative security update) or later  


#### For non-express installation file sync
For non-express content, an additional patch must be applied. This update is critical to prevent unnecessarily downloading around 6 GB of unused content to the client. Supported Windows versions include the following builds:

- **Windows 10 version 1809** with non-security cumulative update [KB4476976](https://support.microsoft.com/help/4476976/windows-10-update-kb4476976) (released 1/22) or later. This update is only available in the catalog and doesn't directly sync to WSUS. To import the update into your environment in order to deploy it, see [Import updates from the Microsoft Update Catalog](/sccm/sum/get-started/synchronize-software-updates#import-updates-from-the-microsoft-update-catalog).


- **Windows 10 version 1803** and **Windows 10 version 1709** clients must have a base cumulative update level plus the non-cumulative update:
    - Cumulative update
        - 1803: [KB4284835](https://support.microsoft.com/help/4284835) (June 2017 cumulative security update) through January 2019 cumulative security update, inclusive
        - 1709: [KB4338825](https://support.microsoft.com/help/4338825) (July 2017 cumulative security update) through January 2019 security cumulative update, inclusive
    - Non-cumulative update: This update is only available in the catalog and doesn't directly sync to WSUS. To import the update into your environment in order to deploy it, see [Import updates from the Microsoft Update Catalog](/sccm/sum/get-started/synchronize-software-updates#import-updates-from-the-microsoft-update-catalog).
        - 1803: [KB4483541](https://support.microsoft.com/help/4483541)
        - 1709: [KB4483530](https://support.microsoft.com/help/4483530)


### 4. Enable express installation on clients in client settings

The client setting to enable express installation must be set for UUP updates, regardless of whether express content is synced or not. This setting enables ConfigMgr to let the Windows Update Agent (WUA) determine the necessary content to download to clients, rather than having ConfigMgr download all content associated with the UUP update. This setting is required even for non-express scenarios as there's optional FOD and Language Pack content, resulting in a non-significant amount extra data that is not required by all clients associated with the update.

Enabling this setting won't affect server content downloads, only client download behaviors. It's important that you have the ConfigMgr and Windows client versions articulated above before enabling this setting if you didn't already have it enabled, as those versions fix some compatibility issues with approval of updates directly in WSUS and enable ConfigMgr to use this channel for UUP updates even if express content isn't synced.

To enable express installation on clients:

1. In the Configuration Manager Console, browse to **Administration** \ **Client Settings**  

2. Open the Properties of the Client Settings you wish to use or create a new one to deploy as appropriate.  

3. Under **Software Updates** group, set **Enable installation of express installation files on clients** to **Yes**

![Highlighting client setting in Software Updates group](../media/uup-preview-client-settings.png)


### 5. Make sure your ADRs are set as desired 

Before enabling sync of UUP updates, consider your automatic deployment rules (ADRs) and any other update infrastructure you have in place. If you don’t want these updates to automatically deploy as part of your existing ADRs and Servicing Plans, be sure to update your ADRs to filter them out, see [How to find synced UUP updates](#how-to-find-synced-uup-updates). Existing Servicing Plans will deploy non-UUP only by default, but you can update them to change this behavior.

Also consider if these updates will affect any of your compliance reports or other infrastructure just by syncing them and make any desired modifications in advance. For example, if you measure compliance across all products, you’ll now see both the UUP and non-UUP cumulative Windows 10 update as non-compliant or compliant therefore skewing your numbers.



## Enable UUP and start testing

### Select products and classifications to sync

Once you're ready to start syncing UUP updates and trying them out, and you have received word from Microsoft that we've enabled your WSUS to see the pilot, enable the new products.

1. [Synchronize Software Updates](/sccm/sum/get-started/synchronize-software-updates) to allow the new products to populate  

2. In the Configuration Manager console, browse to **Administration** \ **Site Configuration** \ **Sites**  

3. Select your top-level site, which is either a central administration site (CAS) or standalone primary  

4. Open **Configure Site Components** \ **Software Update Point**  

5. On the **Products** tab, once your WSUS server is added to the preview, two new products should appear. These products contain the preview UUP content.  

    - **Windows 10 UUP Preview**  
    - **Windows Server UUP Preview**  

6. On the **Classifications** tab, make sure to select:  

    - **Security Updates**: To see the UUP Cumulative Updates  
    - **Upgrades**: To see the UUP Feature Updates  

7. Synchronize software updates to see the new UUP updates


### How to find synced UUP updates

After you have synced UUP updates into your environment, you’ll want to find them in order to test. There are two easy ways to find the preview updates within the Configuration Manager console.

- Because these preview updates are in separate products, you can always use the product to filter or find these updates. Product filter has been added to Servicing Plans to enable you to select whether you want to deploy the UUP or non-UUP feature updates.  

- There's a new optional column, **Tag**, in the **All Software Updates** and **All Windows 10 Updates** nodes of Software Library, as well as a filter in ADRs. This field is set to **UUP** for UUP updates and blank for non-UUP updates.  


### Updates available during preview

- Windows 10 1809 Cumulative Updates
    - February security update (2/12)  

- Windows 10 1803 Cumulative Updates
    - December security update (12/11)
    - January security update (1/8)
    - January non-security update (1/15)
    - February security update (2/12)  

- Windows 10 1709 Cumulative Updates
    - December security update (12/11)
    - January security update (1/8)
    - January non-security update (1/15)
    - February security update (2/12)  

- Windows 10 1809 Feature Updates (from 1709 or 1803)
    - December (12/11) security update compliance
    - January (1/8) security update compliance
    - February security update compliance (2/12)  

- Windows 10 1803 Feature Updates (from 1709 or 1803)	
    - December security update compliance (12/11)
    - January security update compliance (1/8)
    - February security update compliance (2/12)  

If necessary, March and future security updates will continue to be published in all these areas for as long as UUP is still in preview (private or public). Once we complete preview, only Windows 10 Version 1809 Cumulative Updates and Feature Updates (from Windows 10 Version 1803) will be supported in production.


### Scenarios to try

#### Feature updates
- Upgrade straight to the security compliance level of your choosing  

- Upgrade with FODs and language packs installed before the upgrade, they're preserved through the upgrade  

- Optionally, enable syncing express installation files for feature updates to reduce the amount of content each client must download.  

    > [!Note]  
    > This client benefit comes at the cost of larger server download and distribution, as is the case for express installation files for cumulative updates.  

#### Cumulative updates
During the preview, keep clients compliant using the UUP type update for multiple consecutive updates to get the feel for ongoing expectations.

#### Content
The first update for each major version (1809, 1803, 1709), architecture, and language combination will appear to be large, in both number of files and disk space, compared to what you would have seen in non-UUP updates before. This extra content is primarily for all the FOD and language packs for cumulative updates. For feature updates, especially if express is enabled there's additional content that is large for that first update. 

However, the subsequent updates (both the cumulative updates and the monthly feature updates at higher compliance levels) the amount of new content that needs to be downloaded and distributed will be much smaller as all of the FOD and language pack content is intelligently shared across updates rather than redownloaded or redistributed. During the preview, in 1709 and 1803, this monthly download will be approximately equivalent to the size of the cumulative updates you see in non-UUP scenarios. However, in 1809, the story gets much better as the incremental download of the cumulative update is much smaller month to month. 

When you look at the total content downloaded and distributed over a 12-month period for non-express, 1803 without UUP should be approximately equivalent to 1809 with UUP, and after that tipping point the total content downloaded and distributed over the entire lifespan of the release is smaller in 1809 with UUP. For express, the tipping point is sooner as with 1809, express is only for feature updates not cumulative update, and therefore the large server content cost of express is only once per release rather than monthly.

#### Supported content channels
For the preview, test with what you use in your real enterprise environments. UUP will support all content channels, including:
- Windows Delivery Optimization
- Configuration Manager peer cache
- Windows BranchCache
- Deploy without downloading to server (no deployment package) to download straight from Microsoft Update, which if you're using, we recommended using delivery optimization in conjunction
- Third-party alternate content providers

For more information, see [Optimize Windows 10 update delivery](/sccm/sum/deploy-use/optimize-windows-10-update-delivery).


## Known issues

### Additional resources are required on WSUS
When synchronizing UUP updates, especially for the first time, additional resources are required on WSUS servers. In some cases, this behavior has blocked the update synchronization either with the top WSUS server or hierarchy sync to downlevel servers.

This issue manifests as a synchronization failure in WSUS. Configuration Manager also shows it as a synchronization failure.

#### Workaround
Make the following changes on your top level WSUS server or any parent WSUS servers in your hierarchy:
1. Increase the ServerSyncWebService timeout 

    1. Make a backup copy of `C:\Program Files\Update Services\WebServices\serversyncwebservice\web.config` with a different name  

    2. Open `C:\Program Files\Update Services\WebServices\serversyncwebservice\web.config` in Notepad  

    3. Modify **httpRunTime** by adding an **executionTimeout** attribute, for example:  

        `<httpRuntime maxRequestLength="4096" executionTimeout="3600" />`  

    4. Save the web.config to a different location. This step is required because the config file is generally non-editable without taking ownership of the file.  

    5. Then copy the modified web.config to the `C:\Program Files\Update Services\WebServices\serversyncwebservice` directory, replacing the older one.  

    6. From an elevated command prompt, restart IIS: `IISReset`  

        > [!Note]  
        > This action temporarily stops the IIS server

2. Increase the app pool memory for the WSUS server:  

    1. Go to IIS Manager > Application Pools > Select WsusPool and select **Advanced Settings** in the right pane.  

    2. Set both **Private Memory** and **Virtual Memory** limits to `0`.

