---
title: Quickstart – Azure SDK for Java for Microsoft Azure Managed Confidential Consortium Framework 
description: Learn to use the Azure SDK for Java library for Microsoft Azure Managed Confidential Consortium Framework
author: msmbaldwin, msftsettiy
ms.author: mbaldwin, settiy
ms.date: 09/11/2023
ms.service: managed-ccf
ms.topic: quickstart
ms.custom: devx-track-python, mode-api
---

# Quickstart: Create a Microsoft Azure Managed Confidential Consortium Framework resource using the Azure SDK for Java

Microsoft Azure Managed Confidential Consortium Framework (Managed CCF) is a new and highly secure service for deploying confidential applications. For more information on Azure Manaegd CCF, and for examples use cases, see [About Microsoft Azure Managed Confidential Consortium Framework](overview.md).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[API reference documentation](https://learn.microsoft.com/en-us/java/api/com.azure.resourcemanager.confidentialledger?view=azure-java-preview) | [Library source code](https://github.com/Azure/azure-sdk-for-java/tree/main/sdk/confidentialledger) | [Package (maven central repository)](https://central.sonatype.com/artifact/com.azure.resourcemanager/azure-resourcemanager-confidentialledger/1.0.0-beta.3)

## Prerequisites

- An Azure subscription - [create one for free](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Java Development Kit (KDK) versions that are [supported by the Azure SDK for Java](https://github.com/Azure/azure-sdk-for-java/blob/main/sdk/confidentialledger/azure-resourcemanager-confidentialledger/README.md).

## Setup

This quickstart uses the Azure Identity library, along with Azure CLI or Azure PowerShell, to authenticate user to Azure Services. Developers can also use Visual Studio or Visual Studio Code to authenticate their calls. For more information, see [Authenticate the client with Azure Identity client library](/python/api/overview/azure/identity-readme).

### Sign in to Azure

[!INCLUDE [Sign in to Azure](../../includes/confidential-ledger-sign-in-azure.md)]

### Install the dependencies

```Java
<dependency>
    <groupId>com.azure.resourcemanager</groupId>
    <artifactId>azure-resourcemanager-confidentialledger</artifactId>
    <version>1.0.0-beta.3</version>
</dependency>
```

### Create a resource group

[!INCLUDE [Create resource group](../../includes/powershell-rg-create.md)]

### Register the resource provider

[!INCLUDE [Register the resource provider](includes/register-provider.md)]

### Create members

[!INCLUDE [Create members](includes/create-member.md)]

## Create the Java application

The Azure SDK for Java library (azure-resourcemanager-confidentialledger) allows operations on Managed CCF resources, such as creation and deletion, listing the resources associated with a subscription, and viewing the details of a specific resource. The following piece of code creates and views the properties of a Managed CCF resource.

```java
import com.azure.core.management.AzureEnvironment;
import com.azure.core.management.exception.ManagementException;
import com.azure.core.management.profile.AzureProfile;
import com.azure.identity.DefaultAzureCredentialBuilder;
import com.azure.resourcemanager.confidentialledger.ConfidentialLedgerManager;
import com.azure.resourcemanager.confidentialledger.fluent.models.ManagedCcfInner;
import com.azure.resourcemanager.confidentialledger.models.DeploymentType;
import com.azure.resourcemanager.confidentialledger.models.LanguageRuntime;
import com.azure.resourcemanager.confidentialledger.models.ManagedCcfProperties;
import com.azure.resourcemanager.confidentialledger.models.MemberIdentityCertificate;
import java.util.*;

public class AzureJavaSdkClient {
    public static void main(String[] args) {
      try {
          AzureProfile profile = new AzureProfile("<tenant id>","<subscription id>", AzureEnvironment.AZURE);
          ConfidentialLedgerManager manager = ConfidentialLedgerManager.authenticate(new DefaultAzureCredentialBuilder().build(), profile);

          MemberIdentityCertificate member0 = new MemberIdentityCertificate()
              .withCertificate("-----BEGIN CERTIFICATE-----\nMIIBvjCCAUSgAwIBAgIUA0YHcPpUCtd...0Yet/xU4G0d71ZtULNWo\n-----END CERTIFICATE-----")
              .withTags(Map.of("Dept", "IT"));
          List<MemberIdentityCertificate> members = new ArrayList<MemberIdentityCertificate>();
          members.add(member0);

          DeploymentType deployment = new DeploymentType().withAppSourceUri("").withLanguageRuntime(LanguageRuntime.JS);
          ManagedCcfProperties properties = new ManagedCcfProperties()
              .withDeploymentType(deployment)
              .withNodeCount(5)
              .withMemberIdentityCertificates(members);

          ManagedCcfInner inner = new ManagedCcfInner().withProperties(properties).withLocation("southcentralus");

          // Send Create request
          manager.serviceClient().getManagedCcfs().create("contoso-rg", "confidentialbillingapp", inner);

          // Print the Managed CCF resource properties
          ManagedCcfInner app = manager.serviceClient().getManagedCcfs().getByResourceGroup("contoso-rg", "confidentialbillingapp");
          printAppInfo(app);

          // Delete the resource
          manager.serviceClient().getManagedCcfs().delete("contoso-rg", "confidentialbillingapp");
        } catch (ManagementException ex) {
            // The x-ms-correlation-request-id is located in the Header
            System.out.println(ex.getResponse().getHeaders().toString());
            System.out.println(ex);
        }
    }

    private static void printAppInfo(ManagedCcfInner app) {
        System.out.println("App Name: " + app.name());
        System.out.println("App Id: " + app.id());
        System.out.println("App Location: " + app.location());
        System.out.println("App type: " + app.type());
        System.out.println("App Properties Uri: " + app.properties().appUri());
        System.out.println("App Properties Language Runtime: " + app.properties().deploymentType().languageRuntime());
        System.out.println("App Properties Source Uri: " + app.properties().deploymentType().appSourceUri());
        System.out.println("App Properties NodeCount: " + app.properties().nodeCount());
        System.out.println("App Properties Identity Uri: " + app.properties().identityServiceUri());
        System.out.println("App Properties Cert 0: " + app.properties().memberIdentityCertificates().get(0).certificate());
        System.out.println("App Properties Cert tags: " + app.properties().memberIdentityCertificates().get(0).tags());
    }
}
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
- [Quickstart: Azure Portal](quickstart-portal.md)
- [Quickstart: Azure CLI](quickstart-cli.md)
