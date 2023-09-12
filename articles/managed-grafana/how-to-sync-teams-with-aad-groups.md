---
title: Sync Grafana teams with Azure Active Directory groups
description: Learn how to set up Grafana teams using Azure Active Directory groups in Azure Managed Grafana
ms.service: managed-grafana
ms.topic: how-to
author: mcleanbyron
ms.author: mcleans
ms.date: 9/11/2023
--- 

# Sync Grafana teams with Azure Active Directory groups (preview)

In this guide, you learn how to use Azure Active Directory (Azure AD) groups with [Grafana Team Sync](https://grafana.com/docs/grafana/latest/setup-grafana/configure-security/configure-team-sync/) (Azure AD group sync) to set dashboard permissions in Azure Managed Grafana. Grafana allows you to control access to its resources at multiple levels. In Managed Grafana, you use the built-in Azure RBAC roles for Grafana to define access rights users have. These permissions are applied to all resources in your Grafana workspace by default. You can't, for example, grant someone edit permission to only one particular dashboard with RBAC. If you assign a user to the Grafana Editor role, that user can make changes to any dashboard in your Grafana workspace. Using Grafana's [granular permission model](https://grafana.com/docs/grafana/latest/setup-grafana/configure-security/configure-team-sync/), you can elevate or demote a user's default permission level for specific dashboards (or dashboard folders).

Setting up dashboard permissions for individual users in Managed Grafana is a little tricky. Managed Grafana stores the user assignments for its built-in RBAC roles in Azure AD. For performance reasons, it doesn't automatically synchronizes the user assignments to Grafana workspaces. Users in these roles don't show up in Grafana's  **Configuration** UI until they've signed in once. You can only grant users extra permissions after they appear in the Grafana user list in **Configuration**. Azure AD group sync gets around this issue. With this feature, you create a *Grafana team* in your Grafana workspace linked with an Azure AD group. You then use that team in configuring your dashboard permissions. For example, you can grant a viewer the ability to modify a dashboard or block an editor from being able to make changes. You don't need to manage the team's member list separately since its membership is already defined in the associated Azure AD group.

> [!IMPORTANT]
> Azure AD group sync is currently in preview. See the [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) for legal terms that apply to Azure features that are in beta, preview, or otherwise not yet released into general availability.

## Set up Azure AD group sync

To use Azure AD group sync, you add a new team to your Grafana workspace and link it to an existing Azure AD group through its group ID. Follow these steps to set up an Azure AD-backed Grafana team.

1. In the Azure portal, open Azure Active Directory.
1. Select **Groups** and find the Azure AD group you plan to use.
1. Select the group name in the search result.
1. Copy the value in the *Object ID* field. You need to provide this information later.

    :::image type="content" source="media/azure-ad-group-sync/find-azure-ad-group-object-id.png" alt-text="Screenshot of the Azure Active Directory. Finding Azure AD group's object ID.":::

1. Open your Azure Managed Grafana workspace.
1. Select **Administration > Teams**.
1. Select **New Team**, enter a name for the team, and select **Create**.

    :::image type="content" source="media/azure-ad-group-sync/create-new-grafana-team.png" alt-text="Screenshot of the Grafana platform. Creating a new Grafana team.":::

1. Select the **External group sync** tab, and **Add group**.
1. Enter the Azure AD group object ID obtained before, and select **Add group** to link to that group.

    :::image type="content" source="media/azure-ad-group-sync/link-azure-ad-group.png" alt-text="Screenshot of the Grafana platform. Configuring Grafana team to sync with Azure AD group.":::

## Remove Azure AD group sync

If you no longer need a Grafana team, follow these steps to delete it, which also removes the link to the Azure AD group.

1. In the Azure portal, open your Azure Managed Grafana workspace.
1. Select **Administration > Teams**.
1. Select the **X** button to the right of a team you're deleting.

    :::image type="content" source="media/azure-ad-group-sync/remove-azure-ad-group-sync.png" alt-text="Screenshot of the Grafana platform. Removing a Grafana team.":::

1. Select **Delete** to confirm.

## Next steps

In this how-to guide, you learned how to set up Grafana teams backed by Azure AD groups. To learn how to use teams to control access to dashboards in your workspace, see [Manage dashboard permissions](https://grafana.com/docs/grafana/latest/administration/user-management/manage-dashboard-permissions/).

