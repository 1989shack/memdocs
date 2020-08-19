---
title: Apps in Company Portal
titleSuffix: Configuration Manager
description: Provide a consistent user experience for co-managed devices to use the Company Portal app.
ms.date: 08/21/2020
ms.prod: configuration-manager
ms.technology: configmgr-comanage
ms.topic: how-to
ms.assetid: 26456bb7-f46b-4d8d-bb0b-e3fd9a52fe14
author: aczechowski
ms.author: aaroncz
manager: dougeby
---

# Use the Company Portal app on co-managed devices

*Applies to: Configuration Manager (current branch)*

<!--CMADO-3601237,INADO-4297660-->

Starting in version 2006, the Company Portal is now the cross-platform app portal experience for Microsoft Endpoint Manager. By configuring co-managed devices to also use the Company Portal, you can provide a consistent user experience on all devices.

The Company Portal supports the following actions:

- Launch the Company Portal app on co-managed devices and sign in with Azure Active Directory (Azure AD) single sign-on (SSO).
- View available and installed Configuration Manager apps in the Company Portal alongside Intune apps.
- Install available Configuration Manager apps from the Company Portal and receive installation status information.

:::image type="content" source="media/3601237-company-portal.png" alt-text="Company Portal with app from Configuration Manager" lightbox="media/3601237-company-portal.png":::

The behavior of the Company Portal depends upon your co-management workload configuration:

| Workload | Setting | Behavior |
|----------|---------|----------|
| Client apps | **Configuration Manager** | You can see only Configuration Manager client apps |
| Client apps | **Pilot Intune** or **Intune** | You can see both Configuration Manager and Intune client apps |
| Office Click-to-run apps | **Configuration Manager** | You can see only Configuration Manager Office click-to-run apps |
| Office Click-to-run apps | **Pilot Intune** or **Intune** | You can see only Intune Office click-to-run apps |

For more information, see the following articles:

- [Diagram for app workloads](workloads.md#diagram-for-app-workloads)

- [How to switch Configuration Manager workloads to Intune](how-to-switch-workloads.md)

## Prerequisites

- Configuration Manager current branch version 2006 or later

- Windows 10, version 1803 or later:

  - Enrolled to [co-management](how-to-enable.md)

  - Access to [internet endpoints for Intune](../../intune/fundamentals/intune-endpoints.md)

- The user accounts that sign in to these devices require the following configurations:

  - An Azure AD identity

  - Assigned an Intune license

## Configure and deploy

### Configuration Manager client settings

To make sure that users only receive notifications from Company Portal, configure Configuration Manager client settings. In the **Software Center** group of device settings, change **Select the user portal** to **Company Portal**.

### Deploy the Company Portal app

- Users can manually install the Company Portal app from the [Microsoft Store](https://www.microsoft.com/p/company-portal/9wzdncrfj3pz?activetab=pivot:overviewtab).

- To require the app on co-managed devices, the deployment process depends upon the state of the [Client apps](workloads.md#client-apps) co-management workload:

  - If the client apps workload is with Configuration Manager, [create and deploy an application with Configuration Manager](../apps/get-started/create-and-deploy-an-application.md). Download the offline Company Portal app from the [Microsoft Store for Business](https://www.microsoft.com/business-store).

  - If the client apps workload is with Intune, you can deploy it via Configuration Manager or [add the Windows 10 Company Portal app by using Microsoft Intune](../../intune/apps/store-apps-company-portal-app.md).

For more information on branding the Company Portal for your organization, see [How to customize the Intune Company Portal app](../../intune/apps/company-portal-app.md).

## Use the Company Portal

1. Launch the Company Portal from the Start menu. The currently signed-in user is automatically signed in to the Company Portal based on their Azure AD identity.

1. Select the **Apps** page. You should see Configuration Manager apps in the list.

1. Select one of the apps deployed from Configuration Manager.

    - The **Overview** tab shows details about the app, such as size, version, and date published.

    - To see that Configuration Manager is the management service for this app, switch to the **Additional information** tab.

    - To install the app, select **Install**. The Company Portal shows installation status, and you'll see a notification when it completes.

    - If the app is already installed, select **Uninstall** to remove the app.

    - Select the ellipsis (`...`) for additional actions, such as **Repair** and **Share**.

        :::image type="content" source="media/3601237-company-portal-app-details.png" alt-text="Configuration Manager app with details in Company Portal" lightbox="media/3601237-company-portal-app-details.png":::

    - After you install a Configuration Manager web app, select the ellipsis menu, then select **Open in Browser** to launch the web app.

    - If a Configuration Manager application fails to install with a known error code, select the failed status link to search on the error code.

If you change the client setting for Company Portal, when a user selects a Configuration Manager notification, it launches the Company Portal. If the notification is for a scenario the Company Portal doesn't support, selecting the notification launches Software Center.

To help troubleshoot issues with installation of Configuration Manager apps, go to the **Help & Support** section in Company Portal. When you use the **Get help** option, you can send Configuration Manager log files as part of the request.

## Frequently asked questions (FAQ)

### Does Company Portal support applications deployed as software updates from Configuration Manager?

Yes. If you deploy an app using the software updates feature, the client treats it the same whether the user experience is Company Portal or Software Center.

### Can users repair, uninstall, and update Configuration Manager apps in Company Portal?

Yes. If you configure the Configuration Manager app to support these additional actions, Company Portal supports repair, uninstall, and update.

## Known issues

The following features of Software Center aren't currently available in the Company Portal:

- Uninstall or repair apps
- Some app information, for example if a restart is required or the estimated time to install
- [App groups](../apps/deploy-use/create-app-groups.md)

Other known issues:

- If you deploy the same app from both Configuration Manager and Intune, it appears twice in the Company Portal.
- When you search Company Portal, Intune apps always display before Configuration Manager apps.
- When you use the option to **Get help** in the Company Portal, logs to assist troubleshooting aren't sent to Microsoft.

## Next steps

[How to switch Configuration Manager workloads to Intune](how-to-switch-workloads.md)

[How to customize the Intune Company Portal app](../../intune/apps/company-portal-app.md)
