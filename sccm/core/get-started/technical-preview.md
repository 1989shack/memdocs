---
title: "Technical Preview for System Center Configuration Manager | Microsoft Docs"
description: "Learn about the Technical Preview release that let's you test-drive new functionality and capabilities in System Center Configuration Manager."
ms.custom: na
ms.date: 3/24/2017
ms.prod: configuration-manager
ms.reviewer: na
ms.suite: na
ms.technology:
  - configmgr-other
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9ce0a8cb-f96c-4e41-834c-59ceb54ce44a
caps.latest.revision: 157
author: Brenduns
ms.author: brenduns
manager: angrobe

---
# Technical Preview for System Center Configuration Manager

*Applies to: System Center Configuration Manager (Technical Preview)*

**Welcome to the System Center Configuration Manager Technical Preview**. This topic provides details about the evolving preview release that introduces new functionality and capabilities we are working on. With each version of the technical preview, new features are introduced that are not included in the current branch of System Center Configuration Manager at the time the technical preview version is made available. These features might eventually be included in an update to the current branch release, but before we finalize the features and add them, we want you to have a chance to try them out and give us feedback.  

 Because this is a technical preview, details and functionality are subject to change.  

 This topic contains information that applies to all versions of the Technical Preview, and also  lists each new capability (or feature) along with the  Technical Preview version in which the capability first appears, like version 1701 for January of 2017. The details for these capabilities are detailed in  separate topics dedicated to each preview version.  

 For information about what's new in the current branch of Configuration Manager, see [What's new in System Center Configuration Manager](/sccm/core/plan-design/changes/what-has-changed-from-configuration-manager-2012).



##  <a name="bkmk_reqs"></a> Requirements and limitations for the Technical Preview  

> [!IMPORTANT]     
>  The Technical Preview is licensed for use only in a lab environment.  Microsoft may not provide support services and certain features may not be available in the Preview software. Additionally, the Preview software may have reduced or different security, privacy, accessibility, availability and reliability standards relative to commercially provided software.  

 For most product prerequisites, use the information in the [Supported configurations for System Center Configuration Manager](../../core/plan-design/configs/supported-configurations.md). The following exceptions apply to the Technical Preview releases:  

-   Each install remains active for 90 days before it becomes inactive.  

-   English is the only language supported.  

-   Only a stand-alone primary site is supported. There is no support for a central administration site, multiple primary sites, or secondary sites.  

-   Only the following versions of SQL Server are supported:  

    -   SQL Server 2016 (with no Service Pack, and later)
    -   SQL Server 2014 (with no Service Pack, and later)
    -   SQL Server 2012 (with Service Pack 2, or later)


-   The site supports up to 10 clients, which must run one of the following:  

      -   Windows 10  
      -   Windows 8.1  
      -   Windows 8  
      -   Windows 7  


-   Only the following install flags (switches) are supported:  

    -   **/silent**  
    -   **/testdbupgrade**  

-   When applicable, additional limitations or requirements are included with details for each specific version of the Technical Preview  

-   There is no support for migration to or from this preview build.  

-   There is no support for upgrade to this preview build.  

-   There is no support for upgrade to a production build (current branch) from this preview build. However, when updates are available for a preview version,  you can find and install them from the **Updates and Servicing** node of the Configuration Manager console. For a video of the in-console upgrade process, see [Installing ConfigMgr Update Packages](https://www.youtube.com/embed/KBd_EGFbUT8) on youtube.com.  

##  <a name="bkmk_install"></a> Install and update the Technical Preview  
 The System Center Configuration Manager Technical Preview is distinct from the current release of System Center Configuration Manager.  

 To use the technical preview you must first install a **baseline version** of the technical preview build. After installing a baseline version, you then use **in-console updates** to bring your installation up to date with the most recent preview version.     Typically, new versions of the Technical Preview are available each month.

Each preview release is supported up until three successive releases are available. Meaning, when version 1702 releases, version 1610 would no longer be in support, but versions 1611, 1612, and 1701 would remain in support. However, if the last baseline would fall out of support (like vesion 1610), it is still supported for installing a new Technical Preview site, so long as you then update that intsall to a supported version.

> [!TIP]  
>  When you install  an update to the  technical preview, you  update your preview installation to that new technical preview version.    A technical preview installation  never has the option to upgrade to a current branch installation, nor  receive updates from the current branch release.  

 **Active baseline versions of the Technical Preview:**  
 You can install a baseline version for up to 1 year after its release.

-   **Technical Preview 1610** - The Configuration Manager Technical Preview 1610 is available as both an in-console update for the Configuration Manager Technical Preview, and as a new baseline version that is available from the [TechNet Evaluation Center](https://www.microsoft.com/en-us/evalcenter/evaluate-system-center-configuration-manager-and-endpoint-protection-technical-preview) website.




##  <a name="BKMK_TPFeedback"></a> Providing feedback  
 We would love to hear your feedback about our technical previews. To submit feedback about the capabilities in each preview, follow the link to our feedback form on the [Configuration Manager feedback program](https://connect.microsoft.com/ConfigurationManagervnext/Feedback) page on the Microsoft Connect site.  

 And, if you have ideas about new features you would like to see, we want to know that as well. To submit new ideas and to vote on the ideas submitted by others, [visit our user voice page](http://configurationmanager.uservoice.com).  

<!--   ##  <a name="bdmk_tpknownissues"></a> General changes introduced in Technical Previews    -->




##  <a name="bkmk_tpCaps"></a> Capabilities delivered in technical previews  
 The following are the  capabilities delivered  with each Configuration Manager technical preview release.  Capabilities that are available beginning in a version of the technical preview remain available in later versions. Similarly, capabilities that have been added to the System Center Configuration Manager Release (current branch) remain  available in subsequent technical previews.  Click through to the content for each preview version to learn more about a specific capability.  

 |Capability|Technical Preview version|Current Branch version|  
 |----------------|---------------------|--------------------|
 |Deploy volume-purchased iOS apps to device collections|[Tech Preview 1703](capabilities-in-technical-preview-1703.md#deploy-volume-purchased-ios-apps-to-device-collections)|![Not added](media/Red_X.gif)|
 |PFX certificates for Configuration Manager Windows client computers|[Tech Preview 1703](capabilities-in-technical-preview-1703.md#pfx-certificates-for-configuration-manager-windows-client-computers)|![Not added](media/Red_X.gif)|
 |Configure Azure Services wizard|[Tech Preview 1703](capabilities-in-technical-preview-1703.md#configure-azure-services-wizard)|![Not added](media/Red_X.gif)|
 |New compliance settings for iOS devices|[Tech Preview 1702](capabilities-in-technical-preview-1702.md#new-compliance-settings-for-ios-devices)|![Not added](media/Red_X.gif)|
 |Create PFX certificates with S/MIME support|[Tech Preview 1702](capabilities-in-technical-preview-1702.md#create-pfx-certificates-with-s-mime-support)|![Not added](media/Red_X.gif)|
 |Check for running executable files before installing an application|[Tech Preview 1702](capabilities-in-technical-preview-1702.md#check-for-running-executable-files-before-installing-an-application)|![Not added](media/Red_X.gif)|
 |Send feedback from the Configuration Manager console | [Tech Preview 1702](capabilities-in-technical-preview-1702.md#send-feedback-from-the-configuration-manager-console)    |![Not added](media/Red_X.gif)  |
 |Changes for Updates and Servicing  | [Tech Preview 1702](capabilities-in-technical-preview-1702.md#changes-for-updates-and-servicing)  |![Not added](media/Red_X.gif) |
 |Peer Cache improvements  | [Tech Preview 1702](capabilities-in-technical-preview-1702.md#peer-cache-improvements) |![Not added](media/Red_X.gif)|
 |Use Azure Active Directory  | [Tech Preview 1702](capabilities-in-technical-preview-1702.md#azurediscovery) |![Not added](media/Red_X.gif)|
 |Conditional access device compliance policy improvements | [Tech Preview 1702](capabilities-in-technical-preview-1702.md#conditional-access-device-compliance-policy-improvements) |![Not added](media/Red_X.gif)|
 |Antimalware client version alert | [Tech Preview 1702](capabilities-in-technical-preview-1702.md#antimalware-client-version-alert) |![Not added](media/Red_X.gif)|
 |Compliance assessment for Windows Update for Business updates | [Tech Preview 1702](capabilities-in-technical-preview-1702.md#compliance-assessment-for-windows-update-for-business-updates) |![Not added](media/Red_X.gif)|
 |Improvements to Software Center settings and notification messages for high-impact task sequences| [Tech Preview 1702](capabilities-in-technical-preview-1702.md#improvements-to-software-center-settings-and-notification-messages-for-high-impact-task-sequences) |![Not added](media/Red_X.gif)|
 |Android for Work support| [Tech Preview 1702](capabilities-in-technical-preview-1702.md#android-for-work-support) |![Not added](media/Red_X.gif)|
 |Boundary groups improvements for software update points | [Tech Preview 1701](capabilities-in-technical-preview-1701.md#boundary-groups-improvements-for-software-update-points)    |![Not added](media/Red_X.gif)  |
 |Hardware inventory collects UEFI information | [Tech Preview 1701](capabilities-in-technical-preview-1701.md#hardware-inventory-collects-uefi-information)|![Not added](media/Red_X.gif)  |
 |Improvements to operating system deployment| [Tech Preview 1701](capabilities-in-technical-preview-1701.md#improvements-to-operating-system-deployment)|![Not added](media/Red_X.gif)  |
 |Host software updates on cloud-based distribution points| [Tech Preview 1701](capabilities-in-technical-preview-1701.md#host-software-updates-on-cloud-based-distribution-points)|![Not added](media/Red_X.gif) |
 |Validate device health attestation data via management points| [Tech Preview 1701](capabilities-in-technical-preview-1701.md#validate-device-health-attestation-data-via-management-points)|![Not added](media/Red_X.gif) |
 |OMS connector for Microsoft Azure Government cloud |[Tech Preview 1701](capabilities-in-technical-preview-1701.md#use-the-oms-connector-for-microsoft-azure-government-cloud) |![Not added](media/Red_X.gif) |
 |Android and iOS versions are no longer targetable in creation wizards |[Tech Preview 1701](capabilities-in-technical-preview-1701.md#android-and-ios-versions-are-no-longer-targetable-in-creation-wizards-for-hybrid-mdm) |![Not added](media/Red_X.gif) |
 |OData endpoint data access |[Tech Preview 1612](capabilities-in-technical-preview-1612.md#odata-endpoint-data-access)|![Not added](media/Red_X.gif)|
 |Data Warehouse Service point |[Tech Preview 1612](capabilities-in-technical-preview-1612.md#the-data-warehouse-service-point)|![Not added](media/Red_X.gif)|
 |Content library cleanup tool |[Tech Preview 1612](capabilities-in-technical-preview-1612.md#content-library-cleanup-tool)|![Not added](media/Red_X.gif)|
 |Improvements for in-console search |[Tech Preview 1612](capabilities-in-technical-preview-1612.md#improvements-for-in-console-search)|![Not added](media/Red_X.gif)|
 |Prevent installation of an application if a specified program is running|[Tech Preview 1612](capabilities-in-technical-preview-1612.md#prevent-installation-of-an-application-if-a-specified-program-is-running)|![Not added](media/Red_X.gif)|
 |New Windows Hello for Business notification for end users|[Tech Preview 1612](capabilities-in-technical-preview-1612.md#new-windows-hello-for-business-notification-for-end-users)|![Not added](media/Red_X.gif)|
 |Windows Store for Business support in Configuration Manager|[Tech Preview 1612](capabilities-in-technical-preview-1612.md#windows-store-for-business-support-in-configuration-manager)|![Not added](media/Red_X.gif)|
 |Return to previous page when a task sequence fails|[Tech Preview 1612](capabilities-in-technical-preview-1612.md#return-to-previous-page-when-a-task-sequence-fails)|![Not added](media/Red_X.gif)|
 |Express installation files support for Windows 10 updates|[Tech Preview 1612](capabilities-in-technical-preview-1612.md#express-installation-files-support-for-windows-10-updates)|![Not added](media/Red_X.gif)|
 |Azure Active Directory onboarding|[Tech Preview 1612](capabilities-in-technical-preview-1612.md#azure-active-directory-onboarding)|![Not added](media/Red_X.gif)|
|Change to configuring multi-factor authentication for device enrollment|[Tech Preview 1612](capabilities-in-technical-preview-1612.md#change-to-configuring-multi-factor-authentication-for-device-enrollment)|![Not added](media/Red_X.gif)|
|Pre-cache content for deployments and task sequences |[Tech Preview 1611](capabilities-in-technical-preview-1611.md#pre-cache-content-for-available-deployments-and-task-sequences)|![Not added](media/Red_X.gif)|
 |Windows Defender configuration settings|[Tech Preview 1610](capabilities-in-technical-preview-1610.md#windows-defender-configuration-settings)|[Version 1610](/sccm/compliance/deploy-use/create-configuration-items-for-windows-8.1-and-windows-10-devices-managed-without-the-client)|
 |Request policy sync from administrator console|[Tech Preview 1610](capabilities-in-technical-preview-1610.md#request-policy-sync-from-administrator-console)|[Version 1610](/sccm/mdm/deploy-use/sync-intune-device)|
 |Additional security role support for All Corporate-owned Devices node|[Tech Preview 1610](capabilities-in-technical-preview-1610.md#additional-security-role-support)|[Version 1610](/sccm/mdm/understand/whats-new-in-hybrid-mobile-device-management#new-hybrid-features-in-november-2016)|
 |Conditional access for Windows 10 VPN profiles|[Tech Preview 1610](capabilities-in-technical-preview-1610.md#conditional-access-for-windows-10-vpn-profiles)|[Version 1610](/sccm/protect/deploy-use/create-vpn-profiles#configure-the-authentication-method-for-the-vpn-profile)|
 |Filter by content size in automatic deployment rules|[Tech Preview 1610](capabilities-in-technical-preview-1610.md#filter-by-content-size-in-automatic-deployment-rules)|[Version 1610](/sccm/core/plan-design/changes/whats-new-in-version-1610#filter-by-content-size-in-automatic-deployment-rules) |
 |Improved functionality for required software dialogs|[Tech Preview 1610](capabilities-in-technical-preview-1610.md#improved-functionality-for-required-software-dialogs)|[Version 1610](/sccm/apps/deploy-use/deploy-applications)|
 |Deny previously approved application requests|[Tech Preview 1610](capabilities-in-technical-preview-1610.md#deny-previously-approved-application-requests)|[Version 1610](/sccm/apps/deploy-use/deploy-applications)|
 |Exclude clients from automatic upgrade|[Tech Preview 1610](capabilities-in-technical-preview-1610.md#exclude-clients-from-automatic-upgrade)|[Version 1610](/sccm/core/clients/manage/upgrade/exclude-clients-windows)|
 |Improvements to Endpoint Protection|[Tech Preview 1609](capabilities-in-technical-preview-1609.md#improvements-to-endpoint-protection)|![Not added](media/Red_X.gif)|
 |Increased number of enrolled devices|[Tech Preview 1609](capabilities-in-technical-preview-1609.md#increased-number-of-enrolled-devices)|![Not added](media/Red_X.gif)|
 |Additional Apple DEP settings|[Tech Preview 1609](capabilities-in-technical-preview-1609.md#additional-apple-dep-settings)|[Version 1610](/sccm/mdm/deploy-use/ios-device-enrollment-program-for-hybrid)|
 |Enhancements to Windows Store for Business integration with Configuration Manager|[Tech Preview 1609](capabilities-in-technical-preview-1609.md)|[Version 1610](/sccm/apps/deploy-use/manage-apps-from-the-windows-store-for-business)|
 |New compliance settings for configuration items|[Tech Preview 1609](capabilities-in-technical-preview-1609.md#new-compliance-settings-for-configuration-items)|[Version 1610](/sccm/compliance/deploy-use/create-configuration-items)|
 |Integration with Upgrade Analytics|[Tech Preview 1609](capabilities-in-technical-preview-1609.md#integration-with-upgrade-analytics)|[Version 1610](/sccm/core/clients/manage/upgrade/upgrade-analytics)|
 |Native Connection Types for Windows 10 VPN Hybrid Profiles|[Tech Preview 1609](capabilities-in-technical-preview-1609.md#native-connection-types-for-windows-10-vpn-hybrid-profiles)|![Not added](media/Red_X.gif)|
 |Improvements for boundary groups|[Tech Preview 1609](capabilities-in-technical-preview-1609.md#improvements-for-boundary-groups)|[Version 1610](/sccm/core/servers/deploy/configure/define-site-boundaries-and-boundary-groups#BKMK_BoundaryGroups)|
 |Office 365 Client Management dashboard|[Tech Preview 1609](capabilities-in-technical-preview-1609.md#office-365-client-management-dashboard)|[Version 1610](/sccm/sum/deploy-use/manage-office-365-proplus-updates#office-365-client-management-dashboard)|
 |Deploy Office 365 apps to clients|[Tech Preview 1609](capabilities-in-technical-preview-1609.md#deploy-office-365-apps-to-clients)|![Not added](media/Red_X.gif)|
 |Improvements to BIOS to UEFI conversion|[Tech Preview 1609](capabilities-in-technical-preview-1609.md#BKMK_UEFIConversion)|[Version 1610](/sccm/osd/deploy-use/task-sequence-steps-to-manage-bios-to-uefi-conversion)|
 |Intune compliance charts|[Tech Preview 1609](capabilities-in-technical-preview-1609.md#intune-compliance-charts)|[Version 1610](/sccm/protect/deploy-use/create-compliance-policy#monitor-the-compliance-policy)|
 |Improvements to the Prepare ConfigMgr Client for Capture task sequence step|[Tech Preview 1608](capabilities-in-technical-preview-1608.md#improvements-to-the-prepare-configmgr-client-for-capture-task-sequence-step)|[Version 1610](/sccm/osd/understand/task-sequence-steps#BKMK_PrepareConfigMgrClientforCapture)|
 |Improvements to Software Center|[Tech Preview 1608](capabilities-in-technical-preview-1608.md#improvements-to-software-center)|[1610](/sccm/core/plan-design/changes/whats-new-in-version-1610#general-improvements-to-software-center)|
 |Improvements to Asset Intelligence|[Tech Preview 1608](capabilities-in-technical-preview-1608.md#improvements-to-asset-intelligence)|![Not added](media/Red_X.gif)|
 |Remote control keyboard translation|[Tech Preview 1608](capabilities-in-technical-preview-1608.md#remote-control-keyboard-translation)|![Not added](media/Red_X.gif)|
 |Improvements to the Windows 10 Edition Upgrade Policy|[Tech Preview 1607](capabilities-in-technical-preview-1607.md#dmp_edition)|[Version 1610](/sccm/compliance/deploy-use/upgrade-windows-version)|
 |Customizable Branding for Software Center Dialogs|[Tech Preview 1607](capabilities-in-technical-preview-1607.md#customizable-branding-for-software-center-dialogs)|![Not added](media/Red_X.gif)|  
 |Multiple device management points for On-premises Mobile Device Management|[Tech Preview 1606](capabilities-in-technical-preview-1606.md#dmp_onprem)|[Version 1606](/sccm/core/plan-design/changes/whats-new-in-version-1606#on-premises-mobile-device-management)|
 |Automatically categorize devices into collections|[Tech Preview 1606](capabilities-in-technical-preview-1606.md#dmp_category)|[Version 1606](/sccm/core/clients/manage/collections/automatically-categorize-devices-into-collections) |
 |Enforcement grace period for required application and software update deployments|[Tech Preview 1606](capabilities-in-technical-preview-1606.md#dmp_grace)|[Version 1610](/sccm/apps/deploy-use/deploy-applications)|
 |Using Configuration Manager as a Managed Installer with Device Guard|[Tech Preview 1606](capabilities-in-technical-preview-1606.md#dmp_devg)|![Not added](media/Red_X.gif)|
 |Cloud management gateway (formerly Cloud Proxy Service)|[Tech Preview 1606](capabilities-in-technical-preview-1606.md#cloud_proxy) | [Version 1610](/sccm/core/clients/manage/plan-cloud-management-gateway)|  
 |Manage the Office 365 client agent in Configuration Manager|[Tech Preview 1606](capabilities-in-technical-preview-1606.md#manage_o365) |[Version 1606](/sccm/core/plan-design/changes/whats-new-in-version-1606#software-updates)|  
 |The OSDPreserveDriveLetter task sequence variable has been deprecated|[Tech Preview 1606](capabilities-in-technical-preview-1606.md#osdpreservedriveletter) |[Version 1606](/sccm/osd/understand/task-sequence-built-in-variables) |
 |Changes for the Updates and Servicing Node|[Tech Preview 1606](capabilities-in-technical-preview-1606.md#updatesandservicing)|[Version 1606](/sccm/core/plan-design/changes/whats-new-in-version-1606#updates-and-servicing) |
 |Per-app VPN for Windows 10 devices|[Tech Preview 1605](capabilities-in-technical-preview-1605.md#BKMK_PerAppVPN)|[Version 1606](/sccm/protect/deploy-use/create-vpn-profiles)|  
 |Improvements to the Install software updates task sequence|[Tech Preview 1605](capabilities-in-technical-preview-1605.md#BKMK_InstallSU)|[Version 1606](/sccm/core/plan-design/changes/whats-new-in-version-1606#operating-system-deployment)|  
 |Improvements to the Prepare ConfigMgr Client for Capture task sequence step |[Tech Preview 1605](capabilities-in-technical-preview-1605.md#BKMK_PrepareConfigMgrClient)|[Version 1610](/sccm/osd/understand/task-sequence-steps#BKMK_PrepareConfigMgrClientforCapture) |
 |Grace period for required application deployments |[Tech Preview 1605](capabilities-in-technical-preview-1605.md#BKMK_Grace)|![Not added](media/Red_X.gif)|  
 |New experience for remote device actions |[Tech Preview 1605](capabilities-in-technical-preview-1605.md#BKMK_Remote)|[Version 1606](/sccm/core/plan-design/changes/whats-new-in-version-1606#device-configuration-and-protection)|  
 |Windows Store for Business apps |[Tech Preview 1605](capabilities-in-technical-preview-1605.md#BKMK_WSFB)|[Version 1606](/sccm/apps/deploy-use/manage-apps-from-the-windows-store-for-business)|  
 |General improvements for volume-purchased apps|[Tech Preview 1605](capabilities-in-technical-preview-1605.md#BKMK_VPP2)|[Version 1606](/sccm/core/plan-design/changes/whats-new-in-version-1606)|  
 |Enterprise Data Protection (EDP)|[Tech Preview 1605](capabilities-in-technical-preview-1605.md#BKMK_VPP)|[Version 1606](/sccm/core/plan-design/changes/whats-new-in-version-1606)|  
 |End users can install apps from the Company Portal |[Tech Preview 1605](capabilities-in-technical-preview-1605.md#BKMK_End)|![Not added](media/Red_X.gif)|  
 |New tabs for Updates and Operating Systems in Software Center|[Tech Preview 1605](capabilities-in-technical-preview-1605.md#BKMK_SW1)|[Version 1606 ](/sccm/core/plan-design/changes/whats-new-in-version-1606#application-management)|  
 |Service a server group |[Tech Preview 1605](capabilities-in-technical-preview-1605.md#BKMK_ServerGroups)|[Version 1606](/sccm/sum/deploy-use/service-a-server-group)|   
 |Support for Windows Defender Advanced Threat Protection service |[Tech Preview 1605](capabilities-in-technical-preview-1605.md#BKMK_ATP)|[Version 1606](/sccm/protect/deploy-use/windows-defender-advanced-threat-protection)|  
 |New restart options for Windows 10 clients after software update installation|[Tech Preview 1605](capabilities-in-technical-preview-1605.md#BKMK_RestartOptions)|[Version 1606](/sccm/sum/plan-design/plan-for-software-updates#restart-options-for-Windows-10-clients-after-software-update-installation)|  
 |On-premises Device Health Attestation |[Tech Preview 1605](capabilities-in-technical-preview-1605.md#BKMK_DHA)|[Version 1606](/sccm/core/servers/manage/health-attestation)|  
 |Pre-declare corporate-owned devices with IMEI or iOS serial number|[Tech Preview 1605](capabilities-in-technical-preview-1605.md#BKMK_IMEI)|[Version 1606](/sccm/mdm/deploy-use/predeclare-devices-with-hardware-id)|  
 |Manage volume-purchased apps from the Windows Store for Business| [Tech Preview 1604](capabilities-in-technical-preview-1604.md#BKMK_WindowsVPP)|[Version 1606](/sccm/apps/deploy-use/manage-apps-from-the-windows-store-for-business)|  
 |Improvements to Microsoft Passport for Work management|[Tech Preview 1604](capabilities-in-technical-preview-1604.md#BKMK_PFW)|[Version 1606](/sccm/core/plan-design/changes/whats-new-in-version-1606#device-configuration-and-protection)|  
 |Option for clients to switch to a new software update point|[Tech Preview 1604](capabilities-in-technical-preview-1604.md#bkmk_switchsup)|[Version 1606](/sccm/sum/plan-design/plan-for-software-updates#BKMK_ManuallySwitchSUPs)|  
 |Client settings to manage Client Cache Settings and client Peer Cache |[Tech Preview 1604](capabilities-in-technical-preview-1604.md#bkmk_peercache)|Client Settings: [Version 1606](/sccm/core/plan-design/changes/whats-new-in-version-1606#administration)<br/>Peer Cache: [Version 1610](/sccm/core/plan-design/hierarchy/client-peer-cache)|  
 |Support for Passport for Work as a KSP |[Tech Preview 1604](capabilities-in-technical-preview-1604.md#bkmk_passport)|[Version 1606](/sccm/protect/deploy-use/create-certificate-profiles)|  
 |On-premises Device Health Attestation|[Tech Preview 1604](capabilities-in-technical-preview-1604.md#bkmk_onpremdha)|[Version 1606](/sccm/core/servers/manage/health-attestation)|  
 |SmartLock setting for Android devices|[Tech Preview 1604](capabilities-in-technical-preview-1604.md#BKMK_Smart)|[Version 1606](/sccm/compliance/deploy-use/create-configuration-items-for-android-and-samsung-knox-devices-managed-without-the-client#android-and-samsung-knox-configuration-item-settings-reference)|  
 |Improvements to Software Center|[Tech Preview 1603](capabilities-in-technical-preview-1603.md#BKMK_SC1603)|[Version 1606](/sccm/core/plan-design/changes/whats-new-in-version-1606#application-management)|  
 |Improvements to remote control|[Tech Preview 1603](capabilities-in-technical-preview-1603.md#BKMK_RC1603)|[Version 1606](/sccm/core/plan-design/changes/whats-new-in-version-1606#remote-control)|  
 |Customize the RamDisk TFTP block size and window size on PXE-enabled distribution points|[Tech Preview 1603](capabilities-in-technical-preview-1603.md#BKMK_RamDiskTFTP)|[Version 1606](/sccm/core/plan-design/changes/whats-new-in-version-1606#operating-system-deployment)|  


 When all features from a technical preview release are available in the minimum supported version of the Current Branch, details for that preview version are removed from this table.


## See Also  
[What's new in System Center Configuration Manager](/sccm/core/plan-design/changes/whats-new-incremental-versions)  
 [Introduction to System Center Configuration Manager](../../core/understand/introduction.md)
