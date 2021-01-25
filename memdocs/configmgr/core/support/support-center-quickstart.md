---
title: Support Center quickstart
titleSuffix: Configuration Manager
description: Quickly capture the state of a Configuration Manager client for troubleshooting.
ms.date: 01/20/2021
ms.prod: configuration-manager
ms.technology: configmgr-core
ms.topic: conceptual
ms.assetid: 5cb41e2b-4c79-4da9-a432-ff869c0870f8
author: mestew
ms.author: mstewart
manager: dougeby


---

# Support Center quickstart guide

*Applies to: Configuration Manager (current branch)*

Support Center has powerful capabilities including troubleshooting and real-time log viewing. It can also be used in just a few minutes to capture the state of a Configuration Manager client computer. This ability includes accessing remote clients.

Create a complete *troubleshooting bundle* file (.zip) that captures the client state. The bundle doesn't only contain log files. It can include other types of data such as registry settings and client configurations. Provide the bundle to a support technician who uses Support Center Viewer.



## Prerequisites

- Local administrative rights to a Configuration Manager client  

- The Support Center installer. This file is on the site server at `cd.latest\SMSSETUP\Tools\SupportCenter\SupportCenterInstaller.msi`. For more information, see [Support Center - Install](support-center.md#install).  



## Step 1: Create a data bundle on a local client

1. Install Support Center on the Configuration Manager client.  

1. Go to the **Start** menu, in the **Microsoft Endpoint Manager** group, select **Support Center**.  

    > [!NOTE]
    > The above Start menu path is for versions from November 2019 (version 1910) or later. In earlier versions, the folder name is **Microsoft System Center**.

1. On the Home tab of the ribbon, select **Collect Selected Data**. By default, Support Center only collects the minimum data set: log files, client configuration, and operating system.  

1. Save the troubleshooting bundle file (.zip) to a folder on the computer. By default, the file name is similar to the following example: `Support_c885cdfed3c7482bba4f9e662978ec07.zip`.  



## Step 2: View the data bundle using Support Center Viewer

1.  Start **Support Center Viewer**. This action can happen on any computer on which you install Support Center.  

2.  Select **Open bundle**, browse to the bundle file, and select **Open**.  

3.  After Support Center Viewer processes the file, switch to each available tab. View the types of data that Support Center collects by default:  

    - **Configuration**  

        - Configuration Manager client configuration  

        - Operating system  

        - Computer  

        - Services  

        - Network adapters  

    - **Logs**: Choose one or more entries in the list, and select **Open**. This action opens the selected log files in Log Viewer. Use this feature to look up error codes, and use advanced filters to help you more quickly analyze log files.  



## Collect more data

Beyond these basic capabilities, Support Center can also collect a wide variety of other client state information. Open **Support Center** and select **Collect All Data**. This process typically lasts several minutes, even on newer computers. Support Center collects the following additional data:

- **Policy**: Configuration Manager policy settings, including both the requested policy configuration and the actual policy configuration  

- **Certificates**: Public key information for client certificates. Support Center doesn't collect certificate private keys.  

- **Client registry**: Collects client configuration information from the registry. Support Center only collects Configuration Manager registry information.  

- **Client WMI**: Client configuration information from WMI. Support Center doesn't collect client policy.  

- **Troubleshooting**: Real-time troubleshooting data to help diagnose common client problems with Active Directory, management points, networking, policy assignments, and registration.  
    > [!NOTE]  
    > This data type isn't supported when you make a remote connection to another client.

- **Debug dumps**: Perform debug dump of client and related processes. Debug dumps can be large. Only enable this option when troubleshooting issues with client performance.  

    > [!WARNING]  
    > Collecting debug dumps will cause data bundles to become very large (in some cases, several hundred MB).  
    >  
    > Debug dumps contain may contain sensitive information, including passwords, cryptographic secrets, or user data. Debug dumps should only be collected on the recommendation of Microsoft Support personnel. Data bundles that contain debug dumps should be handled carefully to protect them from unauthorized access.  
    >  
    > This data type isn't supported when you make a remote connection to another client.
