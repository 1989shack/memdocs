---
# required metadata

title: Upgrade analytics | System Center Configuration Manager
description: Integrate Upgrade Analytics with Configuration Manager. Access upgrade compatibility data in your admin console. Target devices for upgrade or remediation.
keywords:
author: brenduns
ms.author: brenduns
manager: angerobe
ms.date: 12/3/2017
ms.topic: article
ms.prod: configuration-manager
ms.service:
ms.technology:
  - configmgr-client
ms.assetid: 68407ab8-c205-44ed-9deb-ff5714451624


# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
#ms.reviewer: maayan
#ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Integrate Upgrade Analytics with System Center Configuration Manager

Upgrade Analytics enables you to assess and analyze device readiness and compatibility with Windows 10, to allow easier and smoother upgrades. Integrate Upgrade Analytics with Configuration Manager to access client upgrade compatibility data in the Configuration Manager admin console. You'll then be able to target devices for upgrade or remediation from the device list.

Upgrade Analytics is a solution in the Microsoft Operations Management Suite (OMS). You can read more about Upgrade Analytics in [Get started with Upgrade Analytics](https://technet.microsoft.com/itpro/windows/deploy/upgrade-analytics-get-started).

## Configure clients

There are several configuration steps that you have to take to ensure that your clients can provide data to Upgrade Analytics:

-  Configure client telemetry settings as described in [Configure Windows telemetry in your organization](https://technet.microsoft.com/itpro/windows/manage/configure-windows-telemetry-in-your-organization).
-  Install the KBs described in the *Deploy the compatibility update and related KBs *section of [Get started with Upgrade Analytics](https://technet.microsoft.com/itpro/windows/deploy/upgrade-analytics-get-started).

	> [!NOTE]
	> You can download a script to automate many of the client setup tasks. See the *Run the Upgrade Analytics deployment script* section of [Get started with Upgrade Analytics](https://technet.microsoft.com/itpro/windows/deploy/upgrade-analytics-get-started) for information about the script.

## Create a connection to Upgrade Analytics

### Prerequisites

- In order to add the connection, your Configuration Manager environment must first configure a [service connection point](/sccm/core/servers/deploy/configure/about-the-service-connection-point) in an [online mode](https://azure.microsoft.com/en-us/documentation/articles/resource-group-create-service-principal-portal/). When you add the connection to your environment, it will also install the Microsoft Monitoring Agent on the machine running this site system role.
- Register Configuration Manager as a “Web Application and/or Web API” management tool, and get the [client ID from this registration](https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/).
- Create a client key for the registered management tool in Azure Active Directory.
- In the Azure Management Portal, provide the registered web app with permission to access OMS, as described in [Provide Configuration Manager with permissions to OMS](https://azure.microsoft.com/en-us/documentation/articles/log-analytics-sccm/#provide-configuration-manager-with-permissions-to-oms).

    > [!IMPORTANT]
	> When configuring permission to access OMS, be sure to choose the **Contributor** role, and assign it permissions to the resource group of the registered app.

### Create the connection

1.  In the Configuration Manager console, choose **Administration** > **Cloud Services** > **Upgrade Analytics Connector** > **Create Connection to Upgrade Analytics** to start the **Add Upgrade Analytics Connection wizard**.
3.  On the **Azure Active Directory** screen, provide **Tenant**, **Client ID**, and **Client Secret Key**, then select **Next**.
4.  On the **Upgrade Analytics** screen, provide your connection settings by filling in your **Azure subscription**, **Azure resource group**, and **Operations Management Suite workspace**.
5.  Verify your connection settings on the **Summary** screen, then select **Next**.

	> [!NOTE]
	> You must connect Upgrade Analytics to the top-tier site in your hierarchy. If you connect Upgrade Analytics to a standalone primary site and then add a central administration site to your environment, you must delete and recreate the OMS connection within the new hierarchy.

### Complete Upgrade Analytics tasks  

After you've create the connection in Configuration Manager, perform these tasks, as described in [Get started with Upgrade Analytics](https://technet.microsoft.com/itpro/windows/deploy/upgrade-analytics-get-started).  

1. Add the UpgradeAnalytics service to the OMS workspace.  
2. Generate a commercial ID.  
3. Subscribe to Upgrade Analytics.   

## Use the Upgrade Analytics deployment script  

You can automate many of the Upgrade Analytics tasks and troubleshoot data sharing issues with the Microsoft **Upgrade Analytics deployment script**.  
The Upgrade Analytics deployment script does the following:  

- Sets commercial ID key + CommercialDataOptIn + RequestAllAppraiserVersions keys.  
- Verifies that user computers can send data to Microsoft.  
- Checks whether the computer has a pending restart.   
- Verifies that the latest version of KB package 10.0.x is installed (requires 10.0.14913 or subsequent releases).  
- If enabled, turns on verbose mode for troubleshooting.  
- Initiates the collection of the telemetry data that Microsoft needs to assess your organization’s upgrade readiness.  
- If enabled, displays the script’s progress in a cmd window, providing you visibility into issues (success or fail for each step) and/or writes to log file.  

### To run the Upgrade Analytics deployment script:  

1. Download the [Upgrade Analytics deployment script](https://go.microsoft.com/fwlink/?LinkID=822966&clcid=0x409) and extract UpgradeAnalytics.zip. The files in the **Diagnostics** folder are necessary only if you plan to run the script in troubleshooting mode.  
2. Edit these parameters in RunConfig.bat:  
- Storage location for log information. Example: %SystemDrive%\UADiagnostics. You can store log information on a remote file share or a local directory. If the script is blocked from creating the log file for the given path, it creates the log files in the drive with the Windows directory.  
- Commercial ID key.  
- By default, the script sends log information to both the console and the log file. To change the default behavior, use one of the following options:  
	- logMode = 0 log to console only  
	- logMode = 1 log to file and console  
	- logMode = 2 log to file only  
    - For troubleshooting, set **isVerboseLogging** to **$true** to generate log information that can help with diagnosing issues. By default, **isVerboseLogging** is set to **$false**. Ensure the Diagnostics folder is installed in the same directory as the script to use this mode.  
	- Notify users if they need to restart their computers. By default, this is set to off.  

3. After you finish editing the parameters in RunConfig.bat, run the script as an administrator.  


## View Microsoft Upgrade Analytics properties in Configuration Manager  

1.  In the Configuration Manager console, navigate to **Cloud Services**, then choose **OMS Connector** to open the **OMS Connection Properties** page.  

2.  Within this page there are two tabs:
  * The **Azure Active Directory** tab shows your **Tenant**, **Client ID**, **Client secret key expiration**, and allows you to **Verify** your **Client secret key** if it has expired.
  * The **Upgrade Analytics** tab shows your **Azure subscription**, **Azure resource group**, and **Operations Management Suite workspace**.

## View and use the upgrade information

After you've integrated Upgrade Analytics with Configuration Manager, you can view the analysis of your clients' upgrade readiness and then take action.

1. In the Configuration Manager console, choose **Monitoring** > **Overview** > **Upgrade Analytics**.
2. Review the data, which includes the upgrade readiness state and the percent of Windows devices that are reporting telemetry.
3. You can filter the dashboard to view data for devices in specific collections.
4. You can view the devices in a particular readiness state, and create a dynamic collection for those devices so that you can upgrade those devices if ready, or take action to bring them to a readiness state.
