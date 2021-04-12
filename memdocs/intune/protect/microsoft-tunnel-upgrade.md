---
title: Upgrade the Microsoft Tunnel Gateway server software - Azure | Microsoft Docs
description: Understand how Microsoft Tunnel Gateway upgrades to new versions of the tunnel software for Microsoft Intune.
keywords:
author: brenduns
ms.author: brenduns
manager: dougeby
ms.date: 04/26/2021
ms.topic: how-to
ms.service: microsoft-intune
ms.subservice: protect
ms.localizationpriority: high
ms.technology:

# optional metadata

#ROBOTS:
#audience:

ms.reviewer: tycast
ms.suite: ems
search.appverid: MET150
#ms.tgt_pltfrm:
ms.custom: intune-azure
ms.collection: M365-identity-device-management
---

# Upgrade Microsoft Tunnel for Microsoft Intune

Microsoft Tunnel, a VPN gateway solution for Microsoft Intune, periodically receives [software upgrades](#microsoft-tunnel-update-history) which must install on the tunnel servers to keep them in support. To stay in support, servers must run the most recent release, or at most be one version behind. The information in this article explains the upgrade process, upgrade controls, and status reports you can use to understand the software version of tunnel servers, when upgrades are available, and how to control when upgrades happen.

Intune handles the upgrade of all servers assigned to each tunnel site for you, upgrading all servers in the site one at a time. This is referred to as an upgrade cycle for the site. While a server is upgrading, the Microsoft Tunnel on the server isn't available for use. By upgrading a single server at a time at each site, disruptions to users are minimized when a site includes multiple servers.
During an upgrade cycle:

- Intune begins by upgrading one server in the site. This can start as soon as 10 minutes after the release becomes available.
- If a server was off, upgrade begins after the server turns on.
- After a successful upgrade of one server at a site, Intune waits a short period of time before starting the upgrade of the next server.

## Upgrade controls

To help control when Intune begins the upgrade cycle, configure the following settings at each site. You can configure these when [creating a new site](../protect/microsoft-tunnel-configure.md#create-a-site), or by editing the properties of an existing site:

- **Automatically upgrade servers at this site**
- **Limit server upgrades to maintenance window**

### Automatically upgrade servers at this site

This setting determines if an upgrade cycle for the site can begin automatically, or if an admin must explicitly approve the upgrade before the cycle can begin.

- **Yes** *(default)* – When set to *Yes*, the site automatically upgrade servers as soon as possible after a new tunnel version becomes available. Upgrades begin without further admin intervention.

  If you set a maintenance window for the site, the upgrade cycle begins between the windows start and end time. If no maintenance window is set, the upgrade cycle starts as soon as possible.

- **No** – When set to *No*, Intune won’t upgrade servers until an Admin explicitly chooses to begin the upgrade cycle.

  After upgrade is approved for a site with a maintenance window, the upgrade cycle begins between the windows start and end time. If there is no maintenance window, the upgrade cycle starts as soon as possible.

  > [!IMPORTANT]  
  > When you configure site for manual upgrades, periodically review the [Health check](#view-tunnel-server-status) tab to understand when newer versions of Microsoft Tunnel are available to install. The report also identifies when the current tunnel version at the site is out of support.

### Limit server upgrades to maintenance window

This setting is used to define a maintenance window for the site.

When configured for site, the server upgrade cycle can begin only during the configured period. However, one begun, the cycle continues to update servers one-by-one until all servers assigned to the site complete the upgrade.

- **No** *(default)* – No maintenance window is set. Sites configured to upgrade automatically will do so as soon as possible. Sites configured to require explicit action to start the upgrade will do so as soon as possible *after* the upgrade is approved.

- **Yes** – Set a maintenance window. The window limits when a server upgrade cycle can begin at the site. The maintenance window doesn’t define when individual servers assigned to the site might start to upgrade.

  Sites configured to upgrade automatically will start the upgrade cycle only during the configured period. Sites configured to require the admin to approve the upgrade before beginning, will do during the next maintenance window *after* the upgrade is approved.

  When set to *Yes*, configure the following options:

  - **Time zone** – The time zone you select determines when the maintenance window starts and ends on all servers in the site, regardless of the time zone of individual servers.
  - **Start time** – Specify the earliest time that the upgrade cycle can start, based on the time zone you selected.
  - **End time** - Specify the latest time that upgrade cycle can start, based on the time zone you selected. Upgrade cycles that start before this time will continue to run and can complete after this time.

## View tunnel server status

You can view information about the status of Microsoft Tunnel servers, including the version of Microsoft Tunnel on a server.

For sites that do not support automatic upgrade, you can also view when upgrades to a new version are available.

Sign in to [Microsoft Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431) > **Tenant administration** > **Microsoft Tunnel Gateway** > **Health status**.  Select a server and then open the **Health check** tab to view the following information about it:

- **Server version** - The status of the Tunnel Gateway Server software, in relation to the most recent version.

  - **Healthy** - Up to date with the most recent software version. 
  - **Warning** - One version behind.
  - **Unhealthy** - Two or more versions behind, and out of support.

When a server doesn’t run the most recent software version, plan to install an available upgrade to keep the Microsoft Tunnel in support.

## Manage upgrades

Upgrade installation depends on how a site is configured.

### Sites that support automatic upgrade

When a site [supports automatic upgrades](#automatically-upgrade-servers-at-this-site) of servers, no administrative action is necessary. When the upgrade starts depends on the configuration of a maintenance window for the site:

- **Without a maintenance window**: Upgrade start as soon as possible after a new version of Microsoft Tunnel becomes available.

- **With a maintenance window**: Upgrades start during the next maintenance window after a new version of Microsoft Tunnel becomes available.

### Sites that don't support automatic upgrade

When a site [doesn’t support automatic upgrade](#automatically-upgrade-servers-at-this-site), new versions of Microsoft Tunnel must be explicitly approved for the site before servers will upgrade.

Use the [Health check](#view-tunnel-server-status) tab to understand when newer versions of Microsoft Tunnel are available to install. The report also identifies when the current tunnel version at the site is out of support.

#### To approve installation of an upgrade

1. Sign in to [Microsoft Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431) > **Tenant administration** > **Microsoft Tunnel Gateway** > **Sites**.

2. Select the site with an **Upgrade type** of **Manual**.

3. On the site’s properties, select **Upgrade servers**.

After you choose to upgrade servers, Intune starts the process to do so, which cannot be canceled. The time that upgrades begin at the site depends on the configuration of maintenance windows for the site.

## Microsoft Tunnel update history

Updates for the Microsoft Tunnel release periodically. When a new version is available, read about the changes here.

After an update releases, it rolls out to tenants over the following days. This means new updates might not be available for your tunnel servers for a few days.

The Microsoft Tunnel version for a server isn’t available in the Intune UI at this time. Instead, run the following command on the Linux server that hosts the tunnel to identify the hash values of  *agentImageDigest* and *serverImageDiegest*: `cat /etc/mstunnel/images_configured`

### March 29, 2021

Image hash values:

- **agentImageDigest**: sha256:7ff81ebec9d129558cf07ba1d044d4051dbfaf9eb75cc91500a11f4ef0cb447e

- **serverImageDigest**: sha256:56dc303c67735bad243b2dc8644cc3d1e5318aa963be05a9a685ec6bcbb41c4e

Changes in this release:

- Minor bug fixes and enhancements

- ### January 19, 2021

Image hash values:

- **agentImageDigest**: sha256:227557e71b197c5c26baeed7633e5f89b476bbb8eb23fc82dec260890d5145f1

- **serverImageDigest**: sha256:70026dc3585db871f419d25066e655902af732286b0537512d53e1f0897cc423

Changes in this release:

- Support for Red Hat Enterprise Linux 8.
- Extraneous logging suppressed.

### October 29, 2020

Image hash values:

- **agentImageDigest**: sha256:ba48de2c746a68286d15985f807702c60004131368a4a6a50ceab0f04653031a

- **serverImageDigest**: sha256:a60d778664f7f3ba28d363ec783014d9fc2eda6cc5f6057a1eab8635928e7b07

Changes in this release:

- Fixes for logging. [View the Microsoft Tunnel system logs](../protect/microsoft-tunnel-monitor.md#view-microsoft-tunnel-logs).
- Additional bug fixes.

### October 12, 2020

Image hash values:

- **agentImageDigest**: sha256:d168e416591d94d6a02b64e5dde8709e2d5a44261d67036caafcb55b12912ca5

- **serverImageDigest**: sha256:8b50257a94b9825915cb6a77ed49cfb3e5c6f68da9ae0272cdf8e49cff3d342e

Changes in this release:

- Microsoft Tunnel now [logs](../protect/microsoft-tunnel-monitor.md#view-microsoft-tunnel-logs) operational and monitoring details to Linux server logs in the *syslog* format.
- Various bug fixes.

### September 23, 2020

The initial public preview release of Microsoft Tunnel.

<!-- Archive of past releases
-->

## Next steps

[Reference for Microsoft Tunnel](../protect/microsoft-tunnel-reference.md)
