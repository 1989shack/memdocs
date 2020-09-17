---
title: Troubleshoot scripts for devices uploaded to the admin center
titleSuffix: Configuration Manager
description: "Troubleshooting scripts for Configuration Manager tenant attach"
ms.date: 09/18/2020
ms.topic: troubleshooting
ms.prod: configuration-manager
ms.technology: configmgr-core
ms.assetid: a0eb1e8f-2c85-4f48-a0b5-945b3e5f63f3
manager: dougeby
author: mestew
ms.author: mstewart
---

# Troubleshoot Scripts (preview) for devices uploaded to the admin center
<!--6024392-->
*Applies to: Configuration Manager (current branch)*

Use the following to troubleshoot Scripts in the Microsoft Endpoint Manager admin center:

> [!Important]
> This information relates to a preview feature which may be substantially modified before it's commercially released. Microsoft makes no warranties, express or implied, with respect to the information provided here.

## Common issues

### <a name="bkmk_aad"></a> The necessary configuration is missing in Azure Active Directory

**Error message:** The necessary configuration is missing in Azure Active Directory. Make sure to attach the Configuration Manager site to your Azure tenant, and assign the proper user role in Azure AD.

**Possible cause:** The user account is likely missing the **Admin User** role for the Configuration Manager Microservice application in Azure AD. Add the role in Azure AD from **Enterprise applications** > **Configuration Manager Microservice** > **Users and groups** > **Add user**. Groups are supported if you have Azure AD premium. Changes to this permission can take up to an hour to take effect.

### <a name="bkmk_403"></a> Unable to get Scripts information

**Error message:** Unable to get Scripts information. Make sure Azure AD and AD user discovery are configured and the user account accessing tenant attach features from the Microsoft Endpoint Manager admin center is discovered by both. Verify that the user has proper permissions in Configuration Manager.

**Possible causes:** Typically, this error is caused by an issue with the admin account. Below are the most common issues with the administrative user account:

1. Use the same account to sign in to the admin center. The on-premises identity must be synchronized with and match the cloud identity.
1. Verify the account has **Read** permission for the device's **Collection** in Configuration Manager.
1. Verify the account has **Read Resource** permission for the device's **Collection** in Configuration Manager.
1. Make sure that Configuration Manager has discovered the administrative user account you're using to access the tenant attach features within Microsoft Endpoint Manager admin center. In the Configuration Manager console, go to the **Assets and Compliance** workspace. Select the **Users** node, and find your user account.

    If your account isn't listed in the **Users** node, check the configuration of the site's [Active Directory User discovery](../core/servers/deploy/configure/about-discovery-methods.md#bkmk_aboutUser).

1. Verify the discovery data. Select your user account. In the ribbon, on the **Home** tab select **Properties**. In the properties window, confirm the following discovery data:

    - **Azure Active Directory Tenant ID**: This value should be a GUID for the Azure AD tenant.
    - **Azure Active Directory User ID**: This value should be a GUID for this account in Azure AD.
    - **User Principal Name**: The format of this value is user@domain. For example, `jqpublic@contoso.com`.

    If the Azure AD properties are empty, check the configuration of the site's [Azure AD user discovery](../core/servers/deploy/configure/about-discovery-methods.md#azureaddisc).


### <a name="bkmk_noinfo"></a> Unable to get device information

**Error message:** Unable to get device information. Make sure Azure AD and AD user discovery are configured and the user is discovered by both. Verify that the user has proper permissions in Configuration Manager.

**Possible cause:** Make sure that Configuration Manager has discovered the administrative user account you're using to access the tenant attach features within Microsoft Endpoint Manager admin center. In the Configuration Manager console, go to the **Assets and Compliance** workspace. Select the **Users** node, and find your user account.

   If your account isn't listed in the **Users** node, check the configuration of the site's [Active Directory User discovery](../core/servers/deploy/configure/about-discovery-methods.md#bkmk_aboutUser).

1. Verify the discovery data. Select your user account. In the ribbon, on the **Home** tab select **Properties**. In the properties window, confirm the following discovery data:

    - **Azure Active Directory Tenant ID**: This value should be a GUID for the Azure AD tenant.
    - **Azure Active Directory User ID**: This value should be a GUID for this account in Azure AD.
    - **User Principal Name**: The format of this value is user@domain. For example, `jqpublic@contoso.com`.

    If the Azure AD properties are empty, check the configuration of the site's [Azure AD user discovery](../core/servers/deploy/configure/about-discovery-methods.md#azureaddisc).


### <a name="bkmk_1603"></a> Unexpected error occurred

**Error message:** Unexpected error occurred

#### Possible cause

Verify the account has **Read Resource** permission for the device's **Collection** in Configuration Manager.

#### Error code 500 with an unexpected error occurred message

If you see `System.Security.SecurityException` in the **AdminService.log**, verify that your user principal name (UPN) discovered by [Active Directory User discovery](../core/servers/deploy/configure/about-discovery-methods.md#bkmk_aboutUser) isn't set to a cloud UPN rather than an on-premises UPN. An empty UPN value is also acceptable as it means the Active Directory discovered domain name is used. If you see cloud-only UPN (example: onmicrosoft.com) that's not valid domain UPN (contoso.com), you have an issue and may need to go [set the UPN suffix in Active Directory](/office365/enterprise/prepare-a-non-routable-domain-for-directory-synchronization#add-upn-suffixes-and-update-your-users-to-them).


#### Other possible causes of unexpected errors

Unexpected errors are typically caused by either [service connection point](../core/servers/deploy/configure/about-the-service-connection-point.md), [administration service](../develop/adminservice/overview.md), or connectivity issues.

1. Verify the service connection point has connectivity to the cloud using the **CMGatewayNotificationWorker.log**.
1. Verify the administrative service is healthy by reviewing the SMS_REST_PROVIDER component from site component monitoring on the central site.
1. IIS must be installed on provider machine. For more information, see [Prerequisites for the administration service](../develop/adminservice/overview.md#prerequisites).


### <a name="bkmk_version"></a> Configuration Manager doesn't meet the minimum version prerequisite

**Error message:** Configuration Manager doesn't meet the minimum version prerequisite.

**Possible causes:** Your Configuration Manager sites are not running the minimum version of Configuration Manager 2006 with [KB4580678 - Tenant attach rollup for Configuration Manager current branch, version 2006](https://support.microsoft.com/help/4580678) installed. Verify the following:
 - Configuration Manager version 2006 or higher is installed.
 - That [KB4580678](https://support.microsoft.com/help/4580678) on sites running Configuration Manager version 2006.
 - That every site in the hierarchy meets the minimums listed above.

## Known issues

[!INCLUDE [Known issues shared across tenant attach features](includes/known-issues-shared.md)]
