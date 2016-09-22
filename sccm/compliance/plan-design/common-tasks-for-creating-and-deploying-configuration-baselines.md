---
title: "Common tasks for creating and deploying configuration baselines with System Center Configuration Manager | System Center Configuration Manager"
ms.custom: na
ms.date: 2015-12-08
ms.prod: configuration-manager
ms.reviewer: na
ms.suite: na
ms.technology: 
  - configmgr-other
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4bb6afeb-d267-4f9b-ade2-26e5400c223b
caps.latest.revision: 6
caps.handback.revision: 0
author: robstackmsft

---
# Common tasks for creating and deploying configuration baselines with System Center Configuration Manager
This topic contains common scenarios to help you learn about how to create and deploy System Center Configuration Manager configuration baselines.  
  
 If you are already familiar with compliance settings, detailed documentation about all the features you use can be found in the [How to create configuration baselines in System Center Configuration Manager](../../compliance/deploy-use/create-configuration-baselines.md) and [How to deploy configuration baselines in System Center Configuration Manager](../../compliance/deploy-use/deploy-configuration-baselines.md) topics.  
  
 Before you start, read [Get started with compliance settings in System Center Configuration Manager](../../compliance/get-started/get-started-with-compliance-settings.md) to learn some basics about compliance settings, and also read [Plan for and configure compliance settings in System Center Configuration Manager](../../compliance/plan-design/plan-for-and-configure-compliance-settings.md) to implement any necessary prerequisites.  
  
## Create a configuration baseline  
 In this example, you've created a configuration item for only Windows 10 PCs that run the Configuration Manager client.  
  
 This configuration item enforces a required password of at least 6 characters on Windows 10 PCs. The configuration item is named **Windows 10 Password Enforcement**.  
  
 In the following procedure, you'll learn how to add this configuration item to a configuration baseline to get it ready for deployment.  
  
#### To create the configuration baseline  
  
1.  In the Configuration Manager console, click **Assets and Compliance**.  
  
2.  In the **Assets and Compliance** workspace, expand **Compliance Settings**, and then click **Configuration Baselines**.  
  
3.  On the **Home** tab, in the **Create** group, click **Create Configuration Baseline**.  
  
4.  In the **Create Configuration Baseline** dialog box, configure the following:  
  
    -   **Name** - Enter **Windows 10 Passwords** (or another name of your choice)  
  
5.  Click **Add** > **Configuration Items**.  
  
6.  In the **Add Configuration Items** dialog box, select the **Windows 10 Password Enforcement** configuration item that you previously created, then click **Add**.  
  
7.  Click OK to close the **Add Configuration Items** dialog box and return to the **Create Configuration Baseline** dialog box which should appear similar to this screenshot:  
  
     ![Create Configuration Baseline dialog box](../../compliance/plan-design/media/Create-Configuration-Baseline.png "Create)  
  
8.  Click **OK** to close the **Create Configuration Baseline** dialog box.  
  
 You can now see the configuration baseline you just created in the **Configuration Baselines** node of the Configuration Manager console.  
  
## Deploy a configuration baseline  
 In this example, you'll deploy the configuration baseline you created in the previous procedure to a collection of computers.  
  
#### To deploy the configuration baseline  
  
1.  In the Configuration Manager console, click **Assets and Compliance**.  
  
2.  In the **Assets and Compliance** workspace, expand **Compliance Settings**, and then click **Configuration Baselines**.  
  
3.  From the list of configuration baselines, select **Windows 10 Passwords**.  
  
4.  On the **Home** tab, in the **Deployment** group, click **Deploy**.  
  
5.  In the **Deploy Configuration Baselines** dialog box, configure the following:  
  
    -   **Selected configuration baselines** - Ensure that the **Windows 10 Passwords** configuration baseline was automatically added to this list.  
  
    -   **Remediate noncompliant rules when supported** - Check this box to ensure that if the correct settings are not present on targeted devices, then they will be remediated by Configuration Manager.  
  
    -   **Collection** - Click **Browse** to choose the collection of computers on which the configuration baseline will be evaluated and remediated for compliance. In this example, the configuration baseline was deployed to the built-in **All Desktop and Server Clients** collection.  
  
        > [!TIP]  
        >  Don't worry if the collection you choose contains computers or devices that don't run Windows 10. As long as you configured supported platforms in the configuration item you created, only Windows 10 PCs will be evaluated for compliance.  
  
    -   If required, configure the schedule by which the configuration baseline will be evaluated. Otherwise, keep the default of **7 Days**.  
  
6.  The dialog box will now look something like:  
  
     ![Deploy configuration baselines dialog box](../../compliance/plan-design/media/Deploy-configuration-baselines.png "Deploy)  
  
7.  Click **OK** to close the **Deploy Configuration Baselines** dialog box and create the deployment.  
  
 If you want to take a quick look at compliance statistics for this deployment, in the **Monitoring** workspace, click **Deployments**. At the bottom of the screen, you'll see a **Compliance Statistics** chart.  
  
 For more detailed information about how to monitor configuration baselines, see [How to monitor compliance settings in System Center Configuration Manager](../../compliance/deploy-use/monitor-compliance-settings.md)  
  
## See Also  
 [Common tasks for managing compliance with System Center Configuration Manager](../../compliance/plan-design/common-tasks-for-managing-compliance.md)