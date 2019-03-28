---
title: Software updates maintenance
titleSuffix: "Configuration Manager"
description: "To maintain updates in Configuration Manager, you can schedule the WSUS cleanup task, or you can run it manually."
author: mestew
ms.date: 03/27/2019
ms.topic: conceptual
ms.prod: configuration-manager
ms.technology: configmgr-sum
ms.assetid: 4b0e2e90-aac7-4d06-a707-512eee6e576c
manager: dougeby
ms.author: mstewart
ms.collection: M365-identity-device-management
---
# Software updates maintenance

*Applies to: System Center Configuration Manager (Current Branch)*

You can schedule and run WSUS cleanup tasks from the Configuration Manager console from the Software Update Point Component properties. When you first select to run the WSUS cleanup task, it will run after the next software updates synchronization.  

## To schedule and run the WSUS cleanup job

Schedule the WSUS cleanup job by running the following steps:

1. In the Configuration Manager console, navigate to **Administration** > **Overview** > **Site Configuration** > **Sites**.
2. Select the site at the top of your Configuration Manager hierarchy.

3. Click **Configure Site Components** in the **Settings** group, and then click **Software Update Point** to open Software Update Point Component Properties.  

4. Review the **Supersedence behavior**. Modify the behavior if needed.

   ![supersedence behavior screenshot](media/sccm-supersedence-behavior.PNG)

5. Click the **Supersedence Rules** tab, select **Run WSUS cleanup wizard**. In version 1806, the option is renamed to **Run WSUS cleanup after synchronization**.

6. Click **OK** (Click **Close** if you're running version 1806).

## WSUS cleanup behavior in version 1802 and earlier

Before Configuration Manager version 1806, the WSUS cleanup option runs the following item:

- The **Expired updates** option from the WSUS cleanup wizard on the top-level site's WSUS server only.

  ![WSUS expired update cleanup screenshot](media/wsus-cleanup-expired.PNG)

- A cleanup for software update configuration items in the Configuration Manager database occurs every seven days and removes unneeded updates from the console.
  - This cleanup won't remove expired updates from the Configuration Manager console if they're currently deployed.

Additional maintenance is still needed on the top-level WSUS database and all other WSUS databases in the environment. For more information and instructions, see [The complete guide to Microsoft WSUS and Configuration Manager SUP maintenance](https://support.microsoft.com/help/4490644/complete-guide-to-microsoft-wsus-and-configuration-manager-sup-maint/) blog post.

## WSUS cleanup behavior starting in version 1806

Starting version 1806, the WSUS cleanup option occurs after every sync and does the following cleanup items:
<!--1357898 -->

- The **Expired updates** option for WSUS servers on CAS and primary sites.
  - WSUS servers for secondary sites, don't run WSUS cleanup for expired updates.
- Configuration Manager builds a list of superseded updates from its database. The list is based on the supersedence behavior in the Software Update Point component properties.
  - The update configuration items meeting the supersedence behavior criteria are expired in the Configuration Manager console.
  - The updates are declined in WSUS for CAS and primary sites but not for secondary sites.
- A cleanup for software update configuration items in the Configuration Manager database occurs every seven days and removes unneeded updates from the console.
  - This cleanup won't remove expired updates from the Configuration Manager console if they're currently deployed.

> [!NOTE]
> The "Months to wait before a superseded update is expired" is based on the creation date of the superseding update. For example, if you use 2 months for this setting, then updates that have been superseded will be declined in WSUS and expired in Configuration Manager when the superceding update is 2 months old.

All WSUS Maintenance needs to be run manually on secondary site WSUS databases. The following **WSUS Server Cleanup Wizard** options aren't run on the CAS and primary sites:

- Unused updates and update revisions
- Computers not contacting the server
- Unneeded update files

  For more information and instructions, see [The complete guide to Microsoft WSUS and Configuration Manager SUP maintenance](https://support.microsoft.com/help/4490644/complete-guide-to-microsoft-wsus-and-configuration-manager-sup-maint/) blog post.

## WSUS cleanup behavior starting in version 1810

Starting version 1810, you can specify supersedence rules for feature updates separately from non-feature updates in the Software Update Point component properties. The WSUS cleanup option occurs after every sync and does the following cleanup items:
<!--2839349,3098809, 2977644-->

- The **Expired updates** option for WSUS servers on CAS, primary, and secondary sites.
- Configuration Manager builds a list of superseded updates from its database. The list is based on the supersedence behavior in the Software Update Point component properties.
  - The update configuration items meeting the supersedence behavior criteria are expired in the Configuration Manager console.
  - The updates are declined in WSUS for CAS, primary, and secondary sites.
- A cleanup for software update configuration items in the Configuration Manager database occurs every seven days and removes unneeded updates from the console.
  - This cleanup won't remove expired updates from the Configuration Manager console if they're currently deployed.

> [!NOTE]
> The "Months to wait before a superseded update is expired" is based on the creation date of the superseding update. For example, if you use 2 months for this setting, then updates that have been superseded will be declined in WSUS and expired in Configuration Manager when the superceding update is 2 months old.

The following **WSUS Server Cleanup Wizard** options aren't run on the CAS, primary, and secondary sites:

- Unused updates and update revisions
- Computers not contacting the server
- Unneeded update files

  For more information and instructions, see [The complete guide to Microsoft WSUS and Configuration Manager SUP maintenance](https://support.microsoft.com/help/4490644/complete-guide-to-microsoft-wsus-and-configuration-manager-sup-maint/) blog post.

## Updates cleanup log entries

You can verify this cleanup by reviewing the wsyncmgr.log for the following entries:

- The decline of superseded updates in WSUS is complete when you see this log entry: `Cleanup processed <number> total updates and declined <number>`
- The WSUS cleanup is starting when you see this entry: `Calling WSUS Cleanup.`
- The WSUS cleanup for expired updates is complete when you see this entry: `Successfully completed WSUS Cleanup.`
- The Configuration Manager expired updates configuration items cleanup is starting when you see this entry: `Deleting old expired updates...`
- The Configuration Manager expired updates configuration items cleanup is complete when you see this entry: `Deleted <number> expired updates total`
