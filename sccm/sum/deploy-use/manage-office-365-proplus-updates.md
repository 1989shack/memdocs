---
title: Manage Office 365 ProPlus updates | Microsoft Docs
description: "Configuration Manager synchronizes Office 365 client updates from the WSUS catalog to the site server to make updates available to deploy to clients."
keywords:
author: dougebyms.author: dougebymanager: angrobe
ms.date: 05/31/2017
ms.topic: article
ms.prod: configuration-manager
ms.service:
ms.technology:
 - configmgr-sum
ms.assetid: eac542eb-9aa1-4c63-b493-f80128e4e99b
---

# Manage Office 365 ProPlus with Configuration Manager*Applies to: System Center Configuration Manager (Current Branch)*
Configuration Manager synchronizes Office 365 client updates and makes them available to deploy to clients with Office 365 installed. Beginning in Configuration Manager version 1610, you can review Office 365 client information from the Office 365 Client Management dashboard.

Beginning in Configuration Manager version 1602, Configuration Manager has the ability to manage Office 365 client updates by using the software update management workflow. When Microsoft publishes a new Office 365 client update to the Office Content Delivery Network (CDN), Microsoft also publishes an update package to Windows Server Update Services (WSUS). After Configuration Manager synchronizes the Office 365 client update from the WSUS catalog to the site server, the update is available to deploy to clients.

Beginning in version 1702, you can start the Office 365 Installer from the Office 365 Client Management dashboard to make the initial Office 365 App install experience easier. The wizard lets you configure Office 365 installation settings, download files from Office Content Delivery Networks (CDNs), and create and deploy a script application with the content.

## Office 365 Client Management dashboard  
Starting in Configuration Manager version 1610, the Office 365 Client Management dashboard is available in the Configuration Manager console. To view the dashboard, go to **Software Library** > **Overview** > **Office 365 Client Management**.

The dashboard displays charts for the following:

- Number of Office 365 clients
- Office 365 client versions
- Office 365 client languages
- Office 365 client channels     
For more information, see [Overview of update channels for Office 365 ProPlus](https://technet.microsoft.com/library/mt455210.aspx).
<!--- - Automatic deployment rules with Office 365 apps (have Office 365 Client selected in the set of available products). --->

<!---You can take the following actions on the dashboard:
- --->

At the top of the dashboard, use the **Collection** drop-down setting to filter the dashboard data by members of a specific collection.

### Display data in the Office 365 Client Management dashboard
The data that is displayed in the Office 365 Client Management dashboard comes from hardware inventory. Hardware inventory must be enabled and you must select the **Office 365 ProPlus Configurations** hardware inventory class before data is displayed in the dashboard.
#### To display data in the Office 365 Client Management dashboard
1. Enable hardware inventory, if it is not yet enabled. For details, see [Configure hardware inventory](\sccm\core\clients\manage\configure-hardware-inventory).
2. In the Configuration Manager console, navigate to **Administration** > **Client Settings** > **Default Client Settings**.  
3. On the **Home** tab, in the **Properties** group, click **Properties**.  
4. In the **Default Client Settings** dialog box, click **Hardware Inventory**.  
5. In the **Device Settings** list, click **Set Classes**.  
6. In the **Hardware Inventory Classes** dialog box, select **Office 365 ProPlus Configurations**.  
7.  Click **OK** to save your changes and close the **Hardware Inventory Classes** dialog box.  
The Office 365 Client Management dashboard will start displaying data as hardware inventory is reported.

## Deploy Office 365 updates with Configuration Manager
Use the following steps to deploy Office 365 updates with Configuration Manager:

1.  [Verify the requirements](https://technet.microsoft.com/library/mt628083.aspx) for using Configuration Manager to manage Office 365 client updates in the **Requirements for using Configuration Manager to manage Office 365 client updates** section of the topic.  

2.  [Configure software update points](../get-started/configure-classifications-and-products.md) to synchronize the Office 365 client updates. Set **Updates** for the classification and select **Office 365 Client** for the product. You might have to synchronize software updates at least one time before the Office 365 Client product is available for you to choose. You must synchronize software updates after you configure the software update points to use the **Updates** classification.
3.  Enable Office 365 clients to receive updates from Configuration Manager. You can do this by using Configuration Manager client settings or use group policy. Use one of the following methods to enable the client:  
    - Method 1: Beginning in Configuration Manager version 1606, you can use the Configuration Manager client setting to manage the Office 365 client agent. After you configure this setting and deploy Office 365 updates, the Configuration Manager client agent communicates with the Office 365 client agent to download Office 365 updates from a distribution point and install them. Configuration Manager takes inventory of Office 365 ProPlus Client settings.
      1.  In the Configuration Manager console, click **Administration** > **Overview** > **Client Settings**.  

      2.  Open the appropriate device settings to enable the client agent. For more information about default and custom client settings, see [How to configure client settings in System Center Configuration Manager](../../core/clients/deploy/configure-client-settings.md).  

      3.  Click **Software Updates** and select **Yes** for the **Enable management of the Office 365 Client Agent** setting.  

    - Method 2: [Enable Office 365 clients to receive updates](https://technet.microsoft.com/library/mt628083.aspx#BKMK_EnableClient) from Configuration Manager by using the Office Deployment Tool or Group Policy.  

4. [Deploy the Office 365 updates](deploy-software-updates.md) to clients.   

> [!Important]
> When you have an Office 365 client with more than one language, and download updates for fewer languages, the updates will fail to install. For example, let's say you have an Office 365 client with en-us and de-de. On the site server, you dowload and deploy only en-us content for an applicable Office 365 update. When the user starts the installation for this update from Software Center, the update will hang while downloading the content. You must download and deploy updates in the same languages as the Office 365 clients.  


## Add other languages for Office 365 update downloads
Beginning in Configuration Manager version 1610, you can add support for Configuration Manager to download updates for any languages supported by Office 365 regardless of whether they are supported in Configuration Manager.
> [!IMPORTANT]  
> Configuring additional Office 365 update languages is a site-wide setting. After you add the languages by using the following procedure, all Office 365 updates will be downloaded in those languages as well as the languages that you select on the Language Selection page in the Download Software Updates or Deploy Software Updates wizards.

### To add support to download updates for additional languages
Use the following procedure on the central administration site, or stand-alone primary site, where the software update point site system role is installed.
1. From a command prompt, type *wbemtest* as an administrative user to open the Windows Management Instrumentation Tester.
2. Click **Connect**, and then type *root\sms\site_&lt;siteCode&gt;*.
3. Click **Query**, and then run the following query:
   *select &#42; from SMS_SCI_Component where componentname ="SMS_WSUS_CONFIGURATION_MANAGER"*  
![WMI query](..\media\1-wmiquery.png)
4. In the results pane, double-click the object with the site code for the central administration site or stand-alone primary site.
5. Select the **Props** property, click **Edit Property**, and then click **View Embedded**.
![Property editor](..\media\2-propeditor.png)
6. Starting at the first query result, open each object until you find the one with **AdditionalUpdateLanguagesForO365** for the **PropertyName** property.
7. Select **Value2** and click **Edit Property**.  
![Edit the Value2 property](..\media\3-queryresult.png)
8. Add additional languages to the **Value2** property and click **Save Property**.  
For example, pt-pt (for Portuguese - Portugal), af-za (for Afrikaans - South Africa), nn-no (for Norwegian (Nynorsk) - Norway), etc.  
![Add languages in Property Editor](..\media\4-props.png)  
9. Click **Close**, click **Close**, click **Save Property**, click **Save Object** (if you click **Close** here the values are discarded), click **Close**, and then click **Exit** to exit the Windows Management Intstrumentation Tester.
10. In the Configuration Manager console, go to **Software Library** > **Overview** > **Office 365 Client Management** > **Office 365 Updates**.
11. Now, when you download Office 365 updates, the updates will be downloaded in the language that you select in the wizard and the languages that you configured in this procedure. To verify that the updates downloaded in those languages, go to the package source for the update and look for files with the language code in the filename.  
![Filenames with additional languages](..\media\5-verification.png)


## Change the update channel after you enable Office 365 clients to receive updates from Configuration Manager
To change the update channel after you enable Office 365 clients to receive updates from Configuration Manager, you can use group policy to distribute a registry key value change to Office 365 clients . Change the **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Office\ClickToRun\Configuration\CDNBaseUrl** registry key to use one of the following values:

- Current Channel:  
  **CDNBaseUrl** = http&#58;//officecdn.microsoft.com/pr/492350f6-3a01-4f97-b9c0-c7c6ddf67d60

- Deferred Channel:  
  **CDNBaseUrl** = http&#58;//officecdn.microsoft.com/pr/7ffbc6bf-bc32-4f92-8982-f9dd17fd3114

- First Release for Current Channel:  
  **CDNBaseUrl** = http&#58;//officecdn.microsoft.com/pr/64256afe-f5d9-4f86-8936-8840a6a4f5be

- First Release for Deferred Channel:  
  **CDNBaseUrl** = http&#58;//officecdn.microsoft.com/pr/b8f9b850-328d-4355-9145-c59439a0c4cf

## Deploy Office 365 apps  
Beginning in version 1702, you can start the Office 365 Installer from the Office 365 Client Management dashboard to make the initial Office 365 App installation experience easier. The wizard lets you configure Office 365 installation settings, download files from Office Content Delivery Networks (CDNs), and create and deploy a script application for the files.

This is especially helpful because Office 365 updates are not applicable for clients without Office 365 installed. Before version 1702, to install Office 365 apps for the first time on clients, you would need to manually download Office 365 Deployment Tool (ODT) and the Office 365 installation source files, including all of the language packs that you need, and generate the Configuration.xml that specifies the correct Office version and channel. Then, you would need to create and deploy either a legacy package or a script application for clients to install the Office 365 apps.

> [!NOTE]
> - The computer that runs the Office 365 Installer must have Internet access.  
> - The user that runs the Office 365 Installer must have **Read** and **Write** access to the content location share is provided in the wizard.
> - If you receive a 404 download error, copy the following files to the user %temp% folder:
>    - [releasehistory.xml](http://officecdn.microsoft.com.edgesuite.net/wsus/releasehistory.cab)
>    - [o365client_32bit.xml](http://officecdn.microsoft.com/pr/wsus/ofl.cab)  
> - After you create and deploy Office 365 applications using the Office 365 Installer, Configuration Manager will not manage the Office updates by default. To enable Office 365 clients to receive updates from Configuration Manager, see [Deploy Office 365 updates with Configuration Manager](#deploy-office-365-updates-with-configuration-manager).

### To deploy Office 365 apps to clients from the Office 365 Client Management dashboard
1. In the Configuration Manager console, navigate to **Software Library** > **Overview** > **Office 365 Client Management**.
2. Click **Office 365 Installer** in the upper-right pane. The Office 365 Client Installation Wizard opens.
3. On the **Application Settings** page, provide a name and description for the app, enter the download location for the files, and then click **Next**. Note that the location must be specified in the form &#92;&#92;*server*&#92;*share*.
4. On the **Import Client Settings** page, choose whether to import the Office 365 client settings from an existing XML configuration file or to manually specify the settings, and then click **Next**.  

    When you have an existing configuration file, enter the location for the file and skip to step 7. Note that the location must be specified in the form &#92;&#92;*server*&#92;*share*&#92;*filename*.XML.
    > [!IMPORTANT]    
    > The XML configuration file must contain only [languages supported by the Office 365 ProPlus client](https://technet.microsoft.com/library/cc179219&#40;v=office.16&#41;.aspx).

5. On the **Client Products** page, select the Office 365 suite that you use, select the applications that you want to include, select any additional Office products that should be included, and then click **Next**.
6. On the **Client Settings** page, choose the settings to include, and then click **Next**.
7. On the **Deployment** page, choose whether to deploy the application, and then click **Next**.  
If you choose not to deploy the package in the wizard, skip to step 9.
8. Configure the remainder of the wizard pages as you would for a typical application deployment. For details, see [Create and deploy an application](/sccm/apps/get-started/create-and-deploy-an-application).
9. Complete the wizard.
10. You can deploy or edit the application just as you would with any other application in Configuration Manager from **Software Library** > **Overview** > **Application Management** > **Applications**.   

> [!IMPORTANT]
> The Office 365 app that you create and deploy by using the Office 365 Application Wizard in Configuration Manager is not automatically managed by Configuration Manager until you enable the **Enable management of the Office 365 Client Agent** software updates client agent setting. For details, see [About client settings](/sccm/core/clients/deploy/about-client-settings).

>[!NOTE]
>After you deploy Office 365 apps, you can create automatic deployment rules to maintain the apps. To create an automatic deployment rule for Office 365 apps, click **Create an ADR** from the Office 365 Client Management dashboard, and select **Office 365 Client** when you choose the product. For more information, see [Automatically deploy software updates](/sccm/sum/deploy-use/automatically-deploy-software-updates).

<!--- You can create an Office 365 app without using the Office 365 Installation Wizard. To do this, you use the Office 2016 Deployment Tool (ODT) to download Office installation source files to a network share, generate Configure.xml that specifies the correct Office version and channel, and so on. Then, create an app for the files using the normal app management process.
> [!Note]
> The Office 365 Installation Wizard was introduced in Configuration Manager version 1702 and provides an easy way to create Office 365 apps.

- [Download the Office 2016 Deployment Tool](http://aka.ms/ODT2016) from the Microsoft Download Center.  
- Review the [configuration options for the Office Deployment Tool](https://technet.microsoft.com/library/jj219426.aspx).

You can create an application just as you would with any other application in Configuration Manager from **Software Library** > **Overview** > **Application Management** > **Applications**. For details, see [Create and deploy an application](/sccm/apps/get-started/create-and-deploy-an-application).
--->

<!--- ## Next steps
Use the Office 365 Client Management dashboard in Configuration Manager to review Office 365 client information and deploy Office 365 apps. For details, see [Manage Office 365 apps](manage-office-365-apps.md). --->
