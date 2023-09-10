---
title: Quickstart – Deploy a JavaScript application to a Microsoft Azure Managed CCF resource
description: Learn to deploy a JavaScript application to a Microsoft Azure Managed CCF resource
author: msmbaldwin, msftsettiy
ms.author: mbaldwin, settiy
ms.date: 09/08/2023
ms.service: managed-ccf
ms.topic: quickstart
ms.custom: devx-track-azurecli
---

# Quickstart: Deploy a JavaScript application to an Azure Managed CCF resource

In this quickstart tutorial, you will learn how to deploy an application to a Managed CCF resource. This tutorial builds on the Managed CCF resource created in the [Quickstart: Create an Azure Managed CCF resource](quickstart-portal.md) tutorial.

## Prerequisites

[!INCLUDE [Prerequisites](./includes/proposal-prerequisites.md)]

## Download the service identity

[!INCLUDE [Download Service Identity](./includes/service-identity.md)]

## Deploy the application

[!INCLUDE [Mac instructions](./includes/macos-instructions.md)]

> [!Note]
> This tutorial assumes that the JavaScript application bundle is created using the instructions available [here](https://microsoft.github.io/CCF/main/build_apps/js_app_bundle.html).

[!INCLUDE [Deploy an application](./includes/deploy-update-application.md)]

4. When the command completes, the application is deployed and ready to accept transactions.  

## Next steps

- [Microsoft Azure Managed CCF overview](overview.md)
- [Quickstart: Update the JavaScript runtime options](quickstart-update-runtime-options.md)
- [Quickstart: Deploy an Azure Managed CCF application](quickstart-deploy-application.md)