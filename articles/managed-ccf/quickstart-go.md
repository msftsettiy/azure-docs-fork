---
title: Quickstart – Azure SDK for Go library for Microsoft Azure Managed Confidential Consortium Framework
description: Learn to use the Azure SDK for Go library for Microsoft Azure Managed Confidential Consortium Framework
author: msmbaldwin, msftsettiy
ms.author: mbaldwin, settiy
ms.date: 09/11/2023
ms.service: managed-ccf
ms.topic: quickstart
ms.custom: devx-track-python, mode-api
---

# Quickstart: Create a Microsoft Azure Managed Confidential Consortium Framework using the .NET SDK

Microsoft Azure Managed Confidential Consortium Framework (Managed CCF) is a new and highly secure service for deploying confidential applications. For more information on Azure Manaegd CCF, and for examples use cases, see [About Microsoft Azure Managed Confidential Consortium Framework](overview.md).

In this quickstart, you learn how to create a Managed CCF resource using the .NET client management library.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[API reference documentation](https://learn.microsoft.com/en-us/dotnet/api/overview/azure/resourcemanager.confidentialledger-readme?view=azure-dotnet) | [Library source code](https://github.com/Azure/azure-sdk-for-net/tree/main/sdk/confidentialledger/Azure.ResourceManager.ConfidentialLedger) | [Package (NuGet)](https://www.nuget.org/packages/Azure.ResourceManager.ConfidentialLedger/1.1.0-beta.2)

## Prerequisites

- An Azure subscription - [create one for free](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- .NET versions [supported by the Azure SDK for .NET](https://www.nuget.org/packages/Azure.ResourceManager.ConfidentialLedger/1.1.0-beta.2#dependencies-body-tab).

## Setup

### Create new .NET console app

1. In a command shell, run the following command to create a project named `managedccf-app`:

    ```dotnetcli
    dotnet new console --name managedccf-app
    ```

1. Change to the newly created *managedccf-app* directory, and run the following command to build the project:

    ```dotnetcli
    dotnet build
    ```

    The build output should contain no warnings or errors.
    
    ```console
    Build succeeded.
     0 Warning(s)
     0 Error(s)
    ```

### Install the package

Install the Azure Managed CCF client library for .NET with [NuGet][client_nuget_package]:

```dotnetcli
dotnet add package Azure.ResourceManager.ConfidentialLedger --version 1.1.0-beta.2
```

For this quickstart, you'll also need to install the Azure SDK client library for Azure Identity:

```dotnetcli
dotnet add package Azure.Identity
```

### Create a resource group

[!INCLUDE [Create resource group](../../includes/powershell-rg-create.md)]

### Register the resource provider

[!INCLUDE [Register the resource provider](includes/register-provider.md)]

### Create members

[!INCLUDE [Create members](includes/create-member.md)]

## Create the JavaScript application

### Use the Management plane client library

The management plane library (azure/arm-confidentialledger) allows operations on Managed CCF resources, such as creation and deletion, listing the resources associated with a subscription, and viewing the details of a specific resource. The following piece of code creates and views the properties of a Managed CCF resource.

Add the following directives to the top of *Program.cs*:

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using Azure;
using Azure.Core;
using Azure.Identity;
using Azure.ResourceManager;
using Azure.ResourceManager.ConfidentialLedger;
using Azure.ResourceManager.ConfidentialLedger.Models;
using Azure.ResourceManager.Resources;
```

### Authenticate and create a client

In this quickstart, logged in user is used to authenticate to Azure Managed CCF, which is the preferred method for local development. This example uses ['DefaultAzureCredential()'](/dotnet/api/azure.identity.defaultazurecredential) class from [Azure Identity Library](/dotnet/api/overview/azure/identity-readme), which allows to use the same code across different environments with different options to provide identity.

```csharp
// get your azure access token, for more details of how Azure SDK get your access token, please refer to https://learn.microsoft.com/en-us/dotnet/azure/sdk/authentication?tabs=command-line
TokenCredential cred = new DefaultAzureCredential();
```
Create an ARM client and authenticate using the token credential.

```csharp
// authenticate your client
ArmClient client = new ArmClient(cred);
```

### Create a Managed CCF resource

```csharp
// this example assumes you already have this ResourceGroupResource created on azure
// for more information of creating ResourceGroupResource, please refer to the document of ResourceGroupResource
string subscriptionId = "0000000-0000-0000-0000-000000000001";
string resourceGroupName = "contoso-rg";
ResourceIdentifier resourceGroupResourceId = ResourceGroupResource.CreateResourceIdentifier(subscriptionId, resourceGroupName);
ResourceGroupResource resourceGroupResource = client.GetResourceGroupResource(resourceGroupResourceId);

// get the collection of this ManagedCcfResource
ManagedCcfCollection collection = resourceGroupResource.GetManagedCcfs();

// invoke the operation
string appName = "confidentialbillingapp";
ManagedCcfData data = new ManagedCcfData(new AzureLocation("SouthCentralUS"))
{
    Properties = new ManagedCcfProperties()
    {
        MemberIdentityCertificates =
        {
            new ConfidentialLedgerMemberIdentityCertificate()
            {
                Certificate = "-----BEGIN CERTIFICATE-----MIIBsjCCATigA...LjYAGDSGi7NJnSkA-----END CERTIFICATE-----",
                Encryptionkey = "",
                Tags = BinaryData.FromObjectAsJson(new Dictionary<string, object>()
                {
                    ["additionalProps1"] = "additional properties"
                }),
            }
        },
        DeploymentType = new ConfidentialLedgerDeploymentType()
        {
            LanguageRuntime = ConfidentialLedgerLanguageRuntime.JS,
            AppSourceUri = new Uri(""),
        },
        NodeCount = 3,
    },
    Tags =
    {
        ["additionalProps1"] = "additional properties",
    },
};

ArmOperation<ManagedCcfResource> lro = await collection.CreateOrUpdateAsync(WaitUntil.Completed, appName, data);
ManagedCcfResource result = lro.Value;

// the variable result is a resource, you could call other operations on this instance as well
// but just for demo, we get its data from this resource instance
ManagedCcfData resourceData = result.Data;
// for demo we just print out the id
Console.WriteLine($"Succeeded on id: {resourceData.Id}");
```
### View the properties of a Managed CCF resource

The following piece of code retrives the Managed CCF resource that was created above and prints it's properties. 

```csharp
// this example assumes you already have this ResourceGroupResource created on azure
// for more information of creating ResourceGroupResource, please refer to the document of ResourceGroupResource
string subscriptionId = "0000000-0000-0000-0000-000000000001";
string resourceGroupName = "contoso-rg";
ResourceIdentifier resourceGroupResourceId = ResourceGroupResource.CreateResourceIdentifier(subscriptionId, resourceGroupName);
ResourceGroupResource resourceGroupResource = client.GetResourceGroupResource(resourceGroupResourceId);

// get the collection of this ManagedCcfResource
ManagedCcfCollection collection = resourceGroupResource.GetManagedCcfs();

// invoke the operation
string appName = "confidentialbillingapp";
ManagedCcfResource result = await collection.GetAsync(appName);

// the variable result is a resource, you could call other operations on this instance as well
// but just for demo, we get its data from this resource instance
ManagedCcfData resourceData = result.Data;
// for demo we just print out the id
Console.WriteLine($"Succeeded on id: {resourceData.Id}");
```

### List the Managed CCF resources in a Resource Group

The following piece of code retrieves the Managed CCF resources in a resource group.

```csharp
// this example assumes you already have this ResourceGroupResource created on azure
// for more information of creating ResourceGroupResource, please refer to the document of ResourceGroupResource
string subscriptionId = "0000000-0000-0000-0000-000000000001";
string resourceGroupName = "contoso-rg";
ResourceIdentifier resourceGroupResourceId = ResourceGroupResource.CreateResourceIdentifier(subscriptionId, resourceGroupName);
ResourceGroupResource resourceGroupResource = client.GetResourceGroupResource(resourceGroupResourceId);

// get the collection of this ManagedCcfResource
ManagedCcfCollection collection = resourceGroupResource.GetManagedCcfs();

// invoke the operation and iterate over the result
await foreach (ManagedCcfResource item in collection.GetAllAsync())
{
    // the variable item is a resource, you could call other operations on this instance as well
    // but just for demo, we get its data from this resource instance
    ManagedCcfData resourceData = item.Data;
    // for demo we just print out the id
    Console.WriteLine($"Succeeded on id: {resourceData.Id}");
}

Console.WriteLine($"Succeeded");
```

### List the Managed CCF resources in a subscription

The following piece of code retrieves the Managed CCF resources in a subscription.

```csharp
// this example assumes you already have this SubscriptionResource created on azure
// for more information of creating SubscriptionResource, please refer to the document of SubscriptionResource
string subscriptionId = "0000000-0000-0000-0000-000000000001";
ResourceIdentifier subscriptionResourceId = SubscriptionResource.CreateResourceIdentifier(subscriptionId);
SubscriptionResource subscriptionResource = client.GetSubscriptionResource(subscriptionResourceId);

// invoke the operation and iterate over the result
await foreach (ManagedCcfResource item in subscriptionResource.GetManagedCcfsAsync())
{
    // the variable item is a resource, you could call other operations on this instance as well
    // but just for demo, we get its data from this resource instance
    ManagedCcfData resourceData = item.Data;
    // for demo we just print out the id
    Console.WriteLine($"Succeeded on id: {resourceData.Id}");
}

Console.WriteLine($"Succeeded");
```

## Clean up resources

Other Managed CCF articles can build upon this quickstart. If you plan to continue on to work with subsequent quickstarts and tutorials, you may wish to leave these resources in place.

Otherwise, when you're finished with the resources created in this article, use the Azure CLI [az group delete](/cli/azure/group?#az-group-delete) command to delete the resource group and all its contained resources.

```azurecli
az group delete --resource-group contoso-rg
```

## Next steps

In this quickstart, you created a Managed CCF resource by using the Azure Python SDK for Confidential Ledger. To learn more about Azure Managed CCF and how to integrate it with your applications, continue on to the articles below.

- [Microsoft Azure Managed CCF overview](overview.md)
- [Quickstart: Activate members](activate-members.md)
- [Quickstart: Azure CLI](quickstart-cli.md)
