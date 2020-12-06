---
title: Use the Microsoft Tunnel VPN solution for Microsoft Intune - Azure | Microsoft Docs
description: Learn about the Microsoft Tunnel Gateway, a VPN server for Intune that runs on Linux. With the Microsoft Tunnel, cloud-based devices you manage with Intune can reach your on-premises infrastructure. 
keywords:
author: brenduns
ms.author: brenduns
manager: dougeby
ms.date: 12/02/2020
ms.topic: how-to
ms.service: microsoft-intune
ms.subservice: protect
ms.localizationpriority: high
ms.technology:

# optional metadata

#ROBOTS:
#audience:

ms.reviewer: lacranda
ms.suite: ems
search.appverid: MET150
#ms.tgt_pltfrm:
ms.custom: intune-azure
ms.collection: M365-identity-device-management
---

# Microsoft Tunnel for Microsoft Intune

Microsoft Tunnel is a VPN gateway solution for Microsoft Intune. The tunnel allows access to on-premises resources from iOS/iPadOS and Android Enterprise devices using modern authentication and Conditional Access. This article explains how the tunnel works, including prerequisites to use it, and its architecture.

*Microsoft Tunnel is in public preview*.

When you have your prerequisite configurations in place and are ready to install the tunnel, see [Configure the Microsoft Tunnel](../protect/microsoft-tunnel-configure.md).

## Overview of Microsoft Tunnel

Microsoft Tunnel Gateway installs to a Docker container that runs on a Linux server. The Linux server can be a physical box in your on-premises environment, or a virtual machine that runs on-premises or in the cloud. After the tunnel installs, you use Intune VPN profiles for iOS or Android to direct your devices to use the tunnel for connections to your corporate network and resources. When the tunnel is hosted in the cloud, you’ll need to use a solution like Azure ExpressRoute to extend your on-premises network to the cloud.

Through the Microsoft Endpoint Manager admin center, you’ll:

- Download the Microsoft Tunnel installation script that you’ll run on the Linux servers.
- Configure aspects of Microsoft Tunnel Gateway like IP addresses, DNS servers, and ports.
- Deploy VPN profiles to devices to direct them to use the tunnel.
- Deploy the Microsoft Tunnel apps to your devices.

Through the Microsoft Tunnel app, iOS/iPadOS and Android Enterprise devices:

- Use Azure Active Directory (Azure AD) to authenticate to the tunnel.
- Are evaluated against your Conditional Access policies. If the device isn’t compliant, then it won’t have access to your VPN server or your on-premises network.

To connect to the tunnel, devices use the Microsoft Tunnel app, which is available from the iOS/iPadOS or Android app stores.

You can install multiple Linux servers to support Microsoft Tunnel, and combine servers into logical groups called *Sites*. Each server can join a single Site. When you configure a Site, you’re defining a connection point for devices to use when they access the tunnel. Sites require a *Server configuration* that you’ll define and assign to the Site. The Server configuration is applied to each server you add to that Site, simplifying the configuration of additional servers.

To direct devices to use the tunnel, you create and deploy a VPN policy for Microsoft Tunnel. This policy is a device configuration VPN profile that uses the Microsoft Tunnel for its connection type.

Features of the VPN profiles for the tunnel include:

- A friendly name for the VPN connection that your end users will see.
- The Site that the VPN client connects to.
- Per-app VPN configurations that define which apps the VPN profile is used for, and if it's always-on or not. When always-on, the VPN will automatically connect and is used only for the apps you define. If no apps are defined, the always-on connection provides tunnel access for all network traffic from the device.
- Manual connections to the tunnel when a user launches the VPN and selects *Connect*.
- On-demand VPN rules that allow use of the VPN when conditions are met for specific FQDNs or IP addresses. (iOS/iPadOS)
- Proxy support (iOS/iPadOS, Android 10+)

Server configurations include:

- IP address range – The IP addresses that are assigned to devices that connect to a Microsoft Tunnel.
- DNS servers – The DNS server devices should use when they connect to the server.
- DNS suffix search.
- Split tunneling rules – Up to 500 rules shared across include and exclude routes. For example, if you create 300 include rules, you can then have up to 200 exclude rules.
- Port – The port that Microsoft Tunnel Gateway listens on.

Site configuration includes:

- A public IP address or FQDN, which is the connection point for devices that use the tunnel. This address can be for an individual server or the IP or FQDN of a load-balancing server.
- The Server configuration that is applied to each server in the Site.

You assign a server to a Site at the time you install the tunnel software on the Linux server. The installation uses a script that you can download from within the admin center. After starting the script, you’ll be prompted to configure its operation for your environment, which includes specifying the Site the server will join.

To use the Microsoft Tunnel, devices will need to install the Microsoft Tunnel app. You get the app from the iOS/iPadOS or Android app stores and deploy it to users.

## Configure prerequisites for Microsoft Tunnel

The following sections detail prerequisites for the Linux server that hosts the tunnel software, and for your network.  

> [!NOTE]
> Microsoft Tunnel isn’t supported with Azure Government cloud environments.

### Linux server

Set up a Linux based virtual machine or a physical server on which Microsoft Tunnel Gateway will install.

- **Linux distribution** - The following are supported:

  - CentOS 7.4+(CentOS 8+ isn’t supported)
  - Red Hat (RHEL) 7.4+ (RHEL 8+ isn't supported)
  - Ubuntu 18.04
  - Ubuntu 20.04

- **Size the Linux server**: Use the following guidance to meet your expected use:

  |# Devices | # CPUs | Memory GB | # Servers | # Sites | Disk Space GB |
  |----------|--------|-----------|-----------|---------|---------------|
  | 1,000    | 4      | 4         | 1         | 1       | 30            |
  | 2,000    | 4      | 4         | 1         | 1       | 30            |
  | 5,000    | 8      | 8         | 2         | 1       | 30            |
  | 10,000   | 8      | 8         | 3         | 1       | 30            |
  | 20,000   | 8      | 8         | 4         | 1       | 30            |
  | 40,000   | 8      | 8         | 8         | 1       | 30            |

  Support scales linearly. While each Microsoft Tunnel supports up to 64,000 concurrent connections, individual devices can open multiple connections.

- **CPU**: 64-bit AMD/Intel processor.

- **Install Docker**:  Install Docker version 19.03 CE or later.

  Microsoft Tunnel requires Docker on the Linux server to provide support for containers. Containers provide a consistent execution environment, health monitoring and proactive remediation, and a clean upgrade experience.

  For information about installing and configuring Docker, see:

  - [Install Docker Engine on CentOS]( https://docs.docker.com/engine/install/centos/)
  - [Install Docker Engine on Red Hat Enterprise Linux 7]( https://docs.docker.com/engine/install/centos/)
    > [!NOTE]
    > The current version of Docker that’s available for download for Red Hat Enterprise Linux 7 is not supported with Microsoft Tunnel. Therefore, the preceding link directs you to the CentOS download and installation instructions. The CentOS distribution and installation instructions for Docker are supported on Red Hat Enterprise Linux 7 for Microsoft Tunnel and should be used until an updated distribution for Red Hat Enterprise Linux 7 is available.
  - [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
  
<!-- RHEL 7 distro isn't a supported Docker version at this time:  [Using Docker on Red Hat Enterprise Linux 7](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/7.0_release_notes/sect-red_hat_enterprise_linux-7.0_release_notes-linux_containers_with_docker_format-using_docker) -->

- **Transport Layer Security (TLS) certificate**: The Linux server requires a trusted TLS certificate to secure the connection between devices and the Tunnel Gateway server. You’ll add the TLS certificate, including the full trusted certificate chain, to the server during installation of the Tunnel Gateway.

  - The TLS certificate used to secure the Tunnel Gateway endpoint must have the IP address or FQDN of the Tunnel Gateway server in the SAN.

  - TLS certificate can't have an expiration date longer than two years. If the date is longer than two years, it won't be accepted on iOS devices.

  - Use of wildcards has limited support. For example, **\*.contoso.com** is supported. **cont\*.com** isn’t supported.

  - During installation of the Tunnel Gateway server, you must copy the entire trusted certificate chain to your Linux server. The installation script provides the location where you copy the certificate files and prompts you to do so.

  - If you use a TLS certificate that's not publicly trusted, you must push the entire trust chain to devices using an Intune *Trusted certificate* profile.

  - The TLS certificate can be in **PEM** or **pfx** format.

- **TLS version**: By default, connections between Microsoft Tunnel clients and servers use TLS 1.3. When TLS 1.3 isn’t available, the connection can fallback to use TLS 1.2.

### Network

We recommend using two Network Interface controllers (NICs) per Linux server to improve performance, though use of two is optional.

- **NIC 1** - This NIC handles traffic from your managed devices and should be on a public network with public IP address.  This IP address is the address that you configure in the *Site configuration*. This address can represent a single server or a load balancer.

- **NIC 2** - This NIC handles traffic to your on-premises resources and should be on your private internal network without network segmentation.

If you run Linux as a VM in a cloud, you’ll need to ensure the server can access to your on-premises network. For example, if your VM runs in Azure, you can use [Azure ExpressRoute](/azure/expressroute/expressroute-introduction) or something similar to provide access. If you run the server in a VM on-premises, ExpressRoute isn't necessary.

If you choose to add a load balancer, consult your vendors documentation for configuration details. Take into consideration network traffic and firewall ports specific to Intune and the Microsoft Tunnel.

### Firewall

By default, the Microsoft Tunnel and server use the following ports:

**Inbound ports**:

- TCP 443 – Required by Microsoft Tunnel.
- UDP 443 – Required by Microsoft Tunnel.
- TCP 22 – Optional. Used for SSH/SCP to the Linux server.

**Outbound ports**:

- TCP 443 – Required to access Intune services. Required by Docker to pull images.
- TCP – 80 – Required to access Intune services.

When you create a Server configuration for the tunnel, you can specify a different port than the default of 443. If you specify a different port, be sure to configure firewalls to support your configuration.

**Additional requirements**:

- The Tunnel shares the same requirements as [Network endpoints for Microsoft Intune](../fundamentals/intune-endpoints.md), with the addition of port TCP 22.

- Configure firewall rules to support the configurations detailed in  [Microsoft Container Registry (MCR) Client Firewall Rules Configuration](https://github.com/microsoft/containerregistry/blob/master/client-firewall-rules.md).

### Proxy

You can use a proxy server with Microsoft Tunnel. The following considerations can help you configure the Linux server and your environment for success:

- If you use an internal proxy, you might need to configure the Linux host to use your proxy server by using environment variables. To use the variables, edit the **/etc/environment** file on the Linux server, and adding the following lines:

  `http_proxy=[address]`  
  `https_proxy=[address]`

- Authenticated proxies aren't supported.

- The proxy can’t perform break and inspect. This is because the Linux server uses TLS mutual authentication when connecting to Intune.

- Configure Docker to use the proxy to pull images. To do so, edit the **/etc/systemd/system/docker.service.d/http-proxy.conf** file on the Linux server and add the following lines:

  ```
  [Service]
  Environment="HTTP_PROXY=http://your.proxy:8080/"
  Environment="HTTPS_PROXY=http://your.proxy:8080/"
  Environment="NO_PROXY=127.0.0.1,localhost
  ```

  > [!NOTE]  
  > Microsoft Tunnel doesn’t support Azure AD App Proxy, or similar proxy solutions.

### Platforms

Devices must be enrolled to Intune to be supported with Microsoft Tunnel. Only the following device platforms are supported:

- iOS/iPadOS
- Android Enterprise:
  - Fully Managed
  - Corporate-Owned Work Profile
  - Personally-Owned Work profile

  > [!NOTE]
  > *Android Enterprise dedicated* devices aren't supported by the Microsoft Tunnel.

The following functionality is supported by all platforms:

- Azure Active Directory (Azure AD) authentication to the Tunnel using either username and password, or certificates.
- Per-app support.
- Manual full-device tunnel through a Tunnel app, where the user launches VPN and selects *Connect*.
- Split tunneling. However, on iOS split tunneling rules are ignored when your VPN profile uses *per app VPN*.

Support for a Proxy is limited to the following platforms:

- Android 10 and later
- iOS/iPadOS

## Run the readiness tool

Before you start a server install, we recommend you download and run the **mst-readiness** tool. The tool is a script that runs on your Linux server and does the following actions:

- Confirms that your network configuration allows Microsoft Tunnel to access the required Microsoft endpoints.  
- Validates that the Azure Active Directory (Azure AD) account you’ll use to install Microsoft Tunnel has the required roles to complete enrollment.

> [!IMPORTANT]
> The readiness tool doesn't validate inbound ports, which is a common misconfiguration. After the readiness tool runs, review the [firewall prerequisites](#firewall) and manually validate your firewalls pass inbound traffic.

The mst-readiness tool has a dependency on **jq**, a command-line JSON processor. Before you run the readiness tool, ensure **jq** is installed. For information about how to get and install **jq**, see the documentation for the version of Linux that you use.

To use the readiness tool:

1. Get the readiness tool by using one of the following methods:
   - Download the tool directly by using a web browser.  Go to https://aka.ms/microsofttunnelready to download a file named **mst-readiness**.
   - Sign in to [Microsoft Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431) > **Tenant administration** > **Microsoft Tunnel Gateway**, select the **Servers** tab, select **Create** to open the *Create a server* pane, and then select **Download readiness tool**.  
   - Use a Linux command to get the readiness tool directly. For example, you can use **wget** or **curl** to open the link https://aka.ms/microsofttunnelready.

      For example, to use **wget** and log details to *mst-readiness* during the download, run `wget --output-document=mst-readiness https://aka.ms/microsofttunnelready`

   You can run the script from any Linux server that is on the same network as the server you plan to install, allowing network admins to run it and troubleshoot network issues independently.

2. To validate your network configuration, run the script as **root**. For example, you might use the following command line: `sudo chmod +x ./mst-readiness network`

   The script runs the following actions and reports on success or error for both:
   - Tries to connect to each Microsoft endpoint the tunnel will use.
   - Checks that the required ports are open in your firewall.

3. To validate that the account you’ll use to install Microsoft Tunnel has the required roles and permissions to complete enrollment, run the script with the following command line: `./mst-readiness account`

   The script prompts you to use a different machine with a web browser, which you use to authenticate to Azure AD and to Intune. The tool will report success or an error.

For more information about this tool, see [Reference for mst-cli](../protect/microsoft-tunnel-reference.md#mst-cli-command-line-tool-for-microsoft-tunnel-gateway) in the reference article for Microsoft Tunnel article.

## Use Conditional Access with the Microsoft Tunnel

When you use Conditional Access with Intune, you can create policies to gate device access to Microsoft Tunnel Gateway.

### Provision your tenant

Before you can configure Conditional Access policies for the tunnel, you must enable your tenant to support Microsoft Tunnel for Conditional Access. To enable your tenant, you run a PowerShell script that modifies your tenant to add **Microsoft Tunnel Gateway** as a cloud app that you can then select as part of a Conditional Access policy.  This process requires the use of the Azure Active Directory PowerShell module.

1. [Download and install](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0&preserve-view=true) the **AzureAD PowerShell module**.

2. Download the PowerShell script named **mst-CA-readiness.ps1** from [aka.ms/mst-ca-provisioning](https://aka.ms/mst-ca-provisioning).

3. Using credentials that have the Azure Role permissions [equivalent to **Application Administrator**](/azure/active-directory/users-groups-roles/directory-assign-admin-roles#application-administrator-permissions), run the script from any location in your environment, to provision your tenant.

   > [!CAUTION]
   > During the Microsoft Tunnel preview, *mst-CA-readiness.ps1* is an unsigned script. To enable an unsigned script to run, use the following command: **Set-ExecutionPolicy -executionPolicy Unrestricted**. Use of this command can reduce security in your environment. Therefore, if you use the command to enable use of *mst-CA-readiness.ps1*, plan to restore a stronger level of PowerShell security to your environment after the your use of the readiness script is complete. For more information, see [set-executionpolicy](/powershell/module/microsoft.powershell.security/set-executionpolicy?preserve-view=true&view=powershell-7) in the PowerShell documentation.
   >
   > In a future update, *mst-CA-readiness.ps1* will be signed, which will remove the need to set ExecutionPolicy to *Unrestricted*.


   The script modifies your tenant by creating a service principle with the following details:

   - App ID: 3678c9e9-9681-447a-974d-d19f668fcd88
   - Name: Microsoft Tunnel Gateway

   The addition of this service principle is required so you can select the tunnel cloud app while configuring Conditional Access policies. It's also possible to use Graph to add the service principle information to your tenant.  
4. After the script completes, you can use your normal process to create Conditional Access policies.

To create policies for Conditional Access, see [Create a device-based Conditional Access policy](../protect/create-conditional-access-intune.md).

### Create Conditional Access policy to limit access to Microsoft Tunnel

If you choose to configure Conditional Access policy to limit user access, we recommend configuring this policy after you provision your tenant to support the Microsoft Tunnel Gateway cloud app, but before you install the Tunnel Gateway.

1. Sign in to [Microsoft Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431) > **Endpoint Security** > **Conditional access** > **New policy**.
2. Specify a name for this policy.
3. To configure user and group access, below *Assignments*, select **Users and groups**.
   1. Select **Include** > **All users**.  
   2. Next, select **Exclude** and configure the groups you want to *grant access to*, and then save the user and Group configuration.
4. Under **Cloud apps or actions** > **Select apps**, select the **Microsoft Tunnel Gateway app**.
5. Below *Access controls*, select **Grant**, select **Block access**, and then save the configuration.  
6. Set **Enable policy** to **On**.
7. Select **Create**.

## Architecture

The Microsoft Tunnel Gateway runs in Docker containers that run on Linux servers.  

![Drawing of the Microsoft Tunnel Gateway architecture](./media/microsoft-tunnel-overview/tunnel-architecture.png)
  
**Components**:  
- **A** – Microsoft Intune.
- **B**- Azure Active Directory (AD).
- **C** – Linux server with Docker.
  - **Ci** - Microsoft Tunnel Gateway.
  - **Cii** – Management Agent.
  - **Ciii** – Authentication plugin – Authorization plugin, which authenticates with Azure AD.
- **D** – Public facing IP or FQDN of the Microsoft Tunnel. This can represent a load balancer.
- **E** – Mobile Device Management (MDM) enrolled device.
- **F** – Firewall
- **G** – Internal Proxy Server (optional).
- **H** – Corporate Network.
- **I** – Public internet.

**Actions**:  
- **1** - Intune administrator configures *Server configurations* and *Sites*, Server configurations are associated with Sites.
- **2** - Intune administrator installs Microsoft Tunnel Gateway and the authentication plugin authenticates Microsoft Tunnel Gateway with Azure AD. Microsoft Tunnel Gateway server is assigned to a site.
- **3** - Management Agent communicates to Intune to retrieve your server configuration policies, and to send telemetry logs to Intune.  
- **4** - Intune administrator creates and deploys VPN profiles and the Tunnel app to devices.  
- **5** - Device authenticates to Azure AD. Conditional Access policies are evaluated.  
- **6** - With split tunnel:  
  - **6a** - Some traffic goes directly to the public internet.  
  - **6b** - Some traffic goes to your public facing IP address for the Tunnel.  
- **7** - The Tunnel routes traffic to your internal proxy (optional) and your corporate network.

**Additional details**:

- Conditional Access is done in the VPN client and based on the cloud app *Microsoft Tunnel Gateway*. Non-compliant devices won’t receive an access token from Azure AD and can't access the VPN server. For more information about using Conditional Access with Microsoft Tunnel, see [Use Conditional Access with the Microsoft Tunnel](#use-conditional-access-with-the-microsoft-tunnel).

- The Management Agent is authorized against Azure AD using Azure app ID/secret keys.

- The Tunnel Gateway server uses NAT to provide addresses to VPN clients that are connecting to the corporate network.
  
## Next steps

[Install and configure Microsoft Tunnel](microsoft-tunnel-configure.md)