---
title: Quickstart – Update the JavaScript application on a Microsoft Azure Managed CCF resource
description: Learn to update the JavaScript application on a Microsoft Azure Managed CCF resource
author: msmbaldwin, msftsettiy
ms.author: mbaldwin, settiy
ms.date: 09/10/2023
ms.service: managed-ccf
ms.topic: how-to
---

# Quickstart: Update the JavaScript application

With Managed CCF, it is simple and quick to update an appliation when new functionality is introduced or when bugs fixes are available. This tutorial builds on the Managed CCF resource created in the [Quickstart: Create an Azure Managed CCF resource](quickstart-portal.md) tutorial.

## Prerequisites

[!INCLUDE [Prerequisites](./includes/proposal-prerequisites.md)]

## Download the service identity

[!INCLUDE [Download Service Identity](./includes/service-identity.md)]

## Update the application

[!INCLUDE [Mac instructions](./includes/macos-instructions.md)

> [!Note]
> This tutorial assumes that the updated application bundle is created using the instructions available [here](https://microsoft.github.io/CCF/main/build_apps/js_app_bundle.html) and saved to set_js_app.json.
> Updating an application does not affect the JavaScript runtime options and the users, if any, in the application.

[!INCLUDE [Deploy an application](./includes/deploy-update-application.md)]

4. When the command completes, the application is updated and ready to accept transactions.

## Next steps

- [Microsoft Azure Managed CCF overview](overview.md)
- [Quickstart: View application logs in Log Analytics](quickstart-enable-log-analytics.md)
- [Quickstart: Deploy an Azure Managed CCF application](quickstart-deploy-application.md)