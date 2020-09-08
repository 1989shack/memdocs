---
title: Tenant attach - Resource explorer (preview) in the admin center
titleSuffix: Configuration Manager
description: "View hardware inventory for uploaded Configuration Manager devices from resource explorer in the admin center."
ms.date: 09/08/2020
ms.topic: conceptual
ms.prod: configuration-manager
ms.technology: configmgr-core
ms.assetid: 963dda08-87b8-4e80-90a7-25625efe8861
manager: dougeby
author: mestew
ms.author: mstewart
---

# <a name="bkmk_hinv"></a> Tenant attach: View hardware inventory with resource explorer in Microsoft Endpoint Manager admin center (preview)
<!--cm 6479284 in 7220536 pubpreview Sept 8, 2020-->
*Applies to: Configuration Manager (current branch)*

> [!Important]
> - This information relates to a preview feature which may be substantially modified before it's commercially released. Microsoft makes no warranties, express or implied, with respect to the information provided here.

Microsoft Endpoint Manager is an integrated solution for managing all of your devices. Microsoft brings together Configuration Manager and Intune into a single console called **Microsoft Endpoint Manager admin center**. From the Microsoft Endpoint Management admin center, you can view hardware inventory for uploaded Configuration Manager devices by using resource explorer.

   :::image type="content" source="media/6479284-resource-explorer.png" alt-text="Resource explorer in Microsoft Endpoint Manager admin center" lightbox="media/6479284-resource-explorer.png":::

## Prerequisites

The following items are required to use resource explorer from the admin center:

- All of the prerequisites for [Tenant attach: ConfigMgr client details](client-details.md).
- A minimum of Configuration Manager version 2006 and the corresponding version of the console installed.
- Upgrade the target devices to the latest version of the Configuration Manager client.

## Permissions

The user account needs the following permissions:

- The **Read** permission for the device's **Collection** in Configuration Manager.
- The **Admin User** role for the Configuration Manager Microservice application in Azure AD.
  - Add the role in Azure AD from **Enterprise applications** > **Configuration Manager Microservice** > **Users and groups** > **Add user**. Groups are supported if you have Azure AD premium.

## <a name="bkmk_launch"></a> Launch resource explorer

1. In a browser, navigate to [https://endpoint.microsoft.com](https://endpoint.microsoft.com).
1. Select **Devices** then **All Devices**.
1. Select a device that is synced from Configuration Manager via [tenant attach](device-sync-actions.md).
1. Select **Resource explorer (preview)** to view hardware inventory.
1. Search for or select a class to retrieve information from the client.

   :::image type="content" source="media/6479284-resource-explorer-details.png" alt-text="Resource explorer with the motherboard class selected" lightbox="media/6479284-resource-explorer-details.png":::

## Next steps

[Troubleshoot resource explorer](troubleshoot-resource-explorer.md)