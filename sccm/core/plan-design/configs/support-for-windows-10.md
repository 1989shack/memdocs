---
title: "Support for Windows 10 | Microsoft Docs"
description: "Learn about the Windows 10 versions that are supported as clients or for OSD with System Center Configuration Manager."
ms.custom: na
ms.date: 05/09/2017
ms.prod: configuration-manager
ms.reviewer: na
ms.suite: na
ms.technology:
  - configmgr-other
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a1626a65-da22-49e0-9564-d2f752ea3f4b
caps.latest.revision: 5
author: brenduns  
ms.author: brenduns
manager: angrobe
---
# Support for Windows 10 for System Center Configuration Manager  

*Applies to: System Center Configuration Manager (Current Branch)*


 This topic details the versions of Windows 10 that you can use with the different versions of System Center Configuration Manager Current Branch. This includes:
 -  Windows 10 as Configuration Manager client.
 -  The Windows 10 Windows Assessment and Deployment Kit (ADK).

## Windows 10 as a client
Configuration Manager attempts to provide support as a client for each new Windows 10 version as soon as possible after that Windows version releases. Because the products have separate development and release schedules, the support that Configuration Manager provides depends on when the versions and branches of each product release.

For example, a Configuration Manager version will drop from the matrix after [support for that version](/sccm/core/servers/manage/current-branch-versions-supported) ends. Similarly, support for Windows 10 versions like the Enterprise 2015 LTSB or 1607 (CBB) will drop from the matrix when they are removed from the Configuration Manager supported configurations list. See [Deprecated operating systems](/sccm/core/plan-design/changes/removed-and-deprecated-features#deprecated-operating-systems) for more information.

-   The following information supplements [Supported operating systems for clients and devices](/sccm/core/plan-design/configs/supported-operating-systems-for-clients-and-devices).
-   If you use the Long-Term Servicing Branch of Configuration Manager, see [Supported Configurations for the Long-Term Servicing Branch](/sccm/core/understand/supported-configurations-for-ltsb).

|Windows 10 versions                    |Configuration Manager 1606          |Configuration Manager 1610          |    Configuration Manager 1702 |
|---------------------|-----|-----|-----|
|Enterprise 2015 LTSB                   |![Supported](media/green_check.png) |![Supported](media/green_check.png) |![Supported](media/green_check.png) |
|1507 <br />(*see editions*)            |![Supported](media/green_check.png) |![Supported](media/green_check.png) |![Supported](media/green_check.png) |
|1511 (CB), (CBB)<br />(*see editions*) |![Supported](media/green_check.png) |![Supported](media/green_check.png) |![Supported](media/green_check.png) |
|Enterprise 2016 LTSB                   |![Supported](media/green_check.png) |![Supported](media/green_check.png) |![Supported](media/green_check.png) |
|1607 (CB)	<br />Anniversary Update<br />(*see editions*)      |![Backwards compatible](media/blue_compat.png) |![Supported](media/green_check.png) |![Supported](media/green_check.png) |
|1607 (CBB)	<br />Anniversary Update<br />(*see editions*)      |![Not supported](media/Red_X.png)   |![Supported](media/green_check.png) |![Supported](media/green_check.png) |
|1703 (CB)	<br />Creators Update<br />(*see editions*)      |![Not supported](media/Red_X.png)   |![Not supported](media/Red_X.png) |![Backwards compatible](media/blue_compat.png) |


**Editions:** Enterprise, Pro, Education, Pro Education   

|Key|
|--|
|![Supported](media/green_check.png) = **Supported**  |
|![Not supported](media/blue_compat.png)  = **Backwards compatible** - This means that existing client management features (hardware inventory, software inventory, software updates, etc.) should work with the new Windows 10 Current Branch build. Any known issues or caveats will be documented. <br><br>This approach gives you the ability to deploy and manage new Windows 10 CB builds on day one with application compatibility support without requiring a new Configuration Manager update version. |
|![Supported](media/Red_X.png) = **Not supported**|


## Windows 10 ADK
When you deploy operating systems with Configuration Manager, the [Windows ADK is an external dependency](/sccm/osd/plan-design/infrastructure-requirements-for-operating-system-deployment) that is required.

The following table list the versions of the Windows 10 ADK that you can use with different versions of Configuration Manager.

|Windows 10 ADK versions |Configuration Manager 1606 |Configuration Manager 1610  |Configuration Manager 1702 |
|--------------------|-----|-----|-----|
|1507  |![Not supported](media/Red_X.png)         |![Not supported](media/Red_X.png)  |![Not supported](media/Red_X.png)|
|1511  |![Backwards compatible](media/blue_compat.png)|![Not supported](media/Red_X.png)  |![Not supported](media/Red_X.png)|
|1607  |![Supported](media/green_check.png)       |![Supported](media/green_check.png)|![Backwards compatible](media/blue_compat.png) |
|1703  |![Not supported](media/Red_X.png)         |![Not supported](media/Red_X.png)  |![Supported](media/green_check.png) |  

|Key|
|--|
|![Supported](media/green_check.png) = **Supported** - Windows recommends using the Windows ADK that matches the version of Windows you are deploying. For example, use the Windows ADK for Windows 10 version 1703 when deploying Windows 10 version 1703.  |
|![Backwards compatible](media/blue_compat.png)  = **Backward compatible** - This combination is not tested but should work. Any known issues or caveats will be documented. |
|![Supported](media/Red_X.png) = **Not supported**|
