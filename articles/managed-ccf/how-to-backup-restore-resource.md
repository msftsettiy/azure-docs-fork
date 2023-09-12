---
title: How to Perform a Disaster Recovery
description: Learn how to backup and restore a Azure Managed CCF resource.
services: managed-ccf
author: msmbaldwin,pallabpaul
ms.service: managed-ccf
ms.topic: how-to
ms.date: 09/07/2023
ms.author: pallabpaul,mbaldwin
#Customer intent: As a developer, I want to know how to perform a backup and restore of my Managed CCF app so that I can can access backups of my app files and restore my app in another region in the case of a disaster recovery.
---

# Perform a Disaster Recovery

In this article you'll learn to perform backup of your Managed CCF resource into an Azure Storage Fileshare and restore it back into a specific region. The article will use the commands found at the [Managed CCF's REST API Docs](https://review.learn.microsoft.com/en-us/rest/api/documentation-preview/managed-ccf?view=azure-rest-preview&branch=result_openapiHub_production_6d83a8760fc1).

## Prerequisites

- Install [Az CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli).
- An Azure Storage Account.

## Generating a Bearer Token

A Bearer Token is needed to use the Managed CCF REST API. Execute the following command and save the generated Bearer Token.

```bash
$az account get-access-token –subscription <subscription id>
```

## Generating a SAS URI

Backups will be stored in your own Azure Storage Fileshare to allow you to have full ownership and access of your Managed CCF app data. Both the backup and restore jobs will require you to provide a SAS URI to allow temporary read/write access to your Fileshare. Please follow these steps to generate a SAS URI that can be used with the backup and restore jobs.

1. Go to the Azure Storage Account you would like to use for the Managed CCF backup.

2. Go to the `Security + networking` -> `Shared access signature` blade.

3. Generate a SAS URI with the following configuration:

:::image type="content" source="./media/cedr-sas-uri.png" alt-text="Screenshot of the Azure portal in a web browser, showing the required SAS Generation configuration.":::

4. Save the `File service SAS URL`.

## Performing a Backup

Performing a backup of your Managed CCF app will create a Fileshare in the provided Storage Account. This backup Fileshare can then be used to restore your Managed CCF app at a later time.

Follow these steps to perform a backup:

1. [Generate and save a bearer token](#generating-a-bearer-token) generated for the subscription that your Managed CCF app is located in.

2. [Generate a SAS URI](#generating-a-sas-uri) for the Storage Account you would like the backup Fileshare to be stored in.

3. Execute the following curl command to trigger a backup:

```bash
curl --request POST 'https://management.azure.com/subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.ConfidentialLedger/ManagedCCFs/<app_name>/backup?api-version=2023-06-28-preview' \
--header 'Authorization: Bearer <bearer_token>' \
--header 'Content-Type: application/json' \
--data-raw '{
  "sasUri": "<sas_uri>",
  "restoreRegion": "<restore_region>"
}'
```

Please provide the proper `subscription_id`, `resource_group` and `app_name` associated with the Managed CCF app. In addition, add the generated `bearer_token` and `sas_uri`. The `restore_region` field is optional and should be added if you would like to eventually restore your Managed CCF app in a different region.

4. A Fileshare will be created in the provided Azure Storage Account with the naming convention `<mccf_app_name>-<timestamp>`.

## Exploring Backup Files

Once a backup completes successfully, you should be able to view your backup stored in your Azure Storage Fileshare Account.

:::image type="content" source="./media/cedr-backup-fileshare.png" alt-text="Screenshot of the Azure portal in a web browser, showing a sample Fileshare folder structure.":::

Please refer to the following documents to explore the backup files:

- [Understanding your Ledger and Snapshot Files](https://microsoft.github.io/CCF/main/operations/ledger_snapshot.html)
- [Viewing your Ledger and Snapshot Files](https://microsoft.github.io/CCF/main/audit/python_library.html)

## Performing a Restore

This will restore your Managed CCF app using a provided backup Fileshare. It will restore the instance to the state and transaction it was at when it was backed up.

> [!IMPORTANT]
> The restore job will expect a backup that is less than 90 days old and will throw an error if the backup is older. The restore job also expects the original Managed CCF instance to be deleted before a restore takes place and will throw an error if the original instance still exists. Please [delete your original Managed CCF instance](https://learn.microsoft.com/en-us/cli/azure/confidentialledger/managedccfs?view=azure-cli-latest#az-confidentialledger-managedccfs-delete) if you have not done so already.

Follow these steps to perform a restore:

1. [Generate and save a bearer token](#generating-a-bearer-token) generated for the subscription that your Managed CCF app is located in.

2. [Generate a SAS URI](#generating-a-sas-uri) for the Storage Account that your backup Fileshare to be used is located in.

3. Execute the following curl command to trigger a restore:

```bash
curl --request POST 'https://management.azure.com/subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.ConfidentialLedger/ManagedCCFs/<app_name>/restore?api-version=2023-06-28-preview' \
--header 'Authorization: Bearer <bearer_token>' \
--header 'Content-Type: application/json' \
--data-raw '{
  "sasUri": "<sas_uri>",
  "restoreRegion": "<restore_region>",
  "fileShareName": "<fileshare_name>"
}'
```

Please provide the proper `subscription_id`, `resource_group` and `app_name` associated with the Managed CCF app. 

> [!NOTE]
> The `app_name` should be the same name as the original backed up ledger.

In addition, add the generated `bearer_token` and `sas_uri`. The `restore_region` is required to determine which region the ledger should be restored to and the `fileshare_name` is required to determine which backup is to be used.

4. Your Managed CCF instance will be restored in the provided restore region using the provided backup fileshare name.

## Next steps

- [Quickstart: Azure portal](quickstart-portal.md)
- [Quickstart: Azure CLI](quickstart-python.md)