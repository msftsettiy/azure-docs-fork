---
title: View application logs in Azure Monitor
description: Learn to view the application logs in Azure Monitor
author: msmbaldwin, msftsettiy
ms.author: mbaldwin, settiy
ms.date: 09/09/2023
ms.service: managed-ccf
ms.topic: how-to
ms.custom: devx-track-azurecli
---

# Quickstart: View the application logs in Azure Monitor

In this turotial, you will learn how to view the application logs in Azure Monitor by creating a Log Analytics workspace. This tutorial builds on the Managed CCF resource created in the [Quickstart: Create an Azure Managed CCF resource](quickstart-portal.md) tutorial. Logs are essential pieces of information to understand, analyze and optimize the workings and performance of an application. 

The logs from your JavaScript application and from the Managed CCF resource can be viewed in Azure Monitor by creating a Log Analytics workspace.

## Create the Log Analytics workspace 

1. Follow the instructions at [Create a workspace](../azure-monitor/logs/quick-create-workspace.md) to create a workspace.

2. After the workspace is created, make a note of the Resource Id from the properties page. 

:::image type="content" source="media/how-to/log-analytics-workspace-properties.png" alt-text="Screenshot that shows the properties of a Log Analytics workspace screen.":::

3. Navigate to the Managed CCF resource and make a note of the Resource Id from the properties page.

:::image type="content" source="media/how-to/mccf-resource-properties-screen.png" alt-text="Screenshot that shows the properties of a Managed CCF resource screen.":::

## Link the Log Analytics workspace to the Managed CCF resource

1. After the workspace is created, it must be linked with the Managed CCF resource. After linking, it takes a few minutes for the logs to appear in the workspace.

```azurecli
> az login
> az monitor diagnostic-settings create --name confidentialbillingapplogs --resource <Resource Id of the Managed CCF resource> --workspace <Resource Id of the workspace> --logs [{\"category\":\"applicationlogs\",\"enabled\":true,\"retentionPolicy\":{\"enabled\":false,\"days\":0}}]
```

2.  Open the Logs page. Navaigate to the Queries tab and group the queries by Resource type from the drop down. Navigate to the 'Azure Managed CCF' resource and run the 'CCF application errors' query. Remove the 'Level' filter to view all the logs. 

:::image type="content" source="media/how-to/log-analytics-logs.png" alt-text="Screenshot that shows the Managed CCF resource query in the Log Analytics screen.":::

## Next steps

- [Microsoft Azure Managed CCF overview](overview.md)
- [Quickstart: Deploy an Azure Managed CCF application](quickstart-deploy-application.md)