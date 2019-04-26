---
title: Deployment plans in Desktop Analytics
titleSuffix: Configuration Manager
description: Learn about deployment plans in Desktop Analytics.
ms.date: 04/22/2019
ms.prod: configuration-manager
ms.technology: configmgr-other
ms.topic: conceptual
ms.assetid: 0f369f3a-f251-4f34-9302-1bdc6ea5d139
author: aczechowski
ms.author: aaroncz
manager: dougeby
ROBOTS: NOINDEX
ms.collection: M365-identity-device-management
---

# About deployment plans in Desktop Analytics

> [!Note]  
> This information relates to a preview service which may be substantially modified before it's commercially released. Microsoft makes no warranties, express or implied, with respect to the information provided here.  

Desktop Analytics collects and analyzes device, application, and driver data in your organization. Based on this analysis and your input, you can use the service to create deployment plans for Windows 10. Deployment plans have the following features:  

- Automatically recommend which devices to include in pilots  

- Identify compatibility issues and suggest mitigations  

- Assess the health of the deployment before, during, and after updates  

- Track the progress of your deployment  

As part of your deployment plan, you do the following actions:  

- Define what versions of Windows 10 you want to deploy  

- Choose what groups of devices to which you want to deploy  

- Create readiness rules for the deployment  

- Define the importance of your apps  

- Choose pilot devices based on automatic recommendations  

- Decide how to fix issues with apps based on recommendations from Desktop Analytics  

Desktop Analytics refreshes deployment plan data daily. Any changes you make might not appear for 24 hours. Such changes include assigning importance to an app, or choosing a device to include in a pilot.  

If you connect Desktop Analytics to Configuration Manager, select your collections in the deployment plans. This integration then lets you deploy Windows to a collection based on the Desktop Analytics data.

If you don't use Configuration Manager, you can create groups in Log Analytics. For more information, see [Computer groups in Log Analytics log searches](https://docs.microsoft.com/azure/log-analytics/log-analytics-computer-groups).



## Readiness rules

The following readiness rules are available in deployment plans:

- Whether your devices automatically receive drivers from Windows Update. If devices receive the driver updates from Windows Update, then any driver issues identified as part of readiness assessment are automatically marked as **Ready**.  

- Low install count threshold for your Windows apps. If an app is installed on a higher percentage of computers than this threshold, the deployment plan marks the app as **Noteworthy**. This tag means you can decide how important it is to test during the pilot phase.  



## Importance

As part of the deployment plan, set the *importance* of the apps. Desktop Analytics detects these apps as installed on the target devices. This setting helps Desktop Analytics determine which devices it includes in the pilot phase of the deployment.

If an app is installed on less than 2% of the targeted devices, it's marked **Low install count**. Two percent is the default value. You can adjust the threshold in the readiness settings from 0% to 10%. Desktop Analytics automatically marks these apps as **Ready to upgrade**.  

For apps, choose an importance of **Critical**, **Important**, or **Not important**. If you mark one as critical or important, Desktop Analytics includes in the pilot deployment some devices with that app. The service includes in the pilot more instances of a critical app. If you mark an app as not important, Desktop Analytics automatically sets it to **Ready to upgrade**.



## Pilot devices

Desktop Analytics combines your importance information with the global pilot settings. It then creates a recommendation for which devices should be part of the pilot deployment. The recommended pilot deployment includes devices with different hardware configurations and one or more instances of all the critical and important apps. If an app is marked critical, the service recommends more devices with that app in the pilot.



## Deployment plans in Configuration Manager

After creating a deployment plan, use Configuration Manager to deploy the products. Once the deployment starts, Desktop Analytics monitors the deployment based on the settings in the plan.


## Next steps

- [Learn about Desktop Analytics assets](/sccm/desktop-analytics/about-assets): devices, drivers, and apps  

- [Learn about security and feature updates](/sccm/desktop-analytics/about-updates)  

- [Create a deployment plan](/sccm/desktop-analytics/create-deployment-plans)  
