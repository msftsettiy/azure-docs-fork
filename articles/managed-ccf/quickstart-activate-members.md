---
title: Quickstart – Activate Members in a Microsoft Azure Managed CCF resource
description: Learn to activate the members in a Azure Managed CCF resource
author: msmbaldwin, msftsettiy
ms.author: mbaldwin, settiy
ms.date: 09/08/2023
ms.service: managed-ccf
ms.topic: quickstart
ms.custom: devx-track-azurecli
---

# Quickstart: Activate members in an Azure Managed CCF resource

In this quickstart tutorial, you will learn how to activate the member(s) in a Managed CCF resource. This tutorial builds on the Managed CCF resource created in the [Quickstart: Create an Azure Managed CCF resource](quickstart-portal.md) tutorial.

## Prerequisites

- Python 3+.
- Install the latest version of the [CCF Python package](https://pypi.org/project/ccf/).

## Download the service identity

[!INCLUDE [Download Service Identity](./includes/service-identity.md)]

## Activate Member(s)

A new member who gets registered in Managed CCF is not yet able to participate in the governance operations. To do so, the new member should first acknowledge that they are satisfied with the state of the service (for example, after auditing the current constitution and the nodes currently trusted).

1. First, the new member should update and retrieve the latest state digest. In doing so, the new member confirms that they are satisfied with the current state of the service.

```Bash
$ curl https://confidentialbillingapp.confidential-ledger.azure.com/gov/ack/update_state_digest -X POST --cacert service_cert.pem --key member0_privk.pem --cert member0_cert.pem --silent | jq > request.json
$ cat request.json
{
    "state_digest": <...>
}
```
2. The new member should sign the state digest using the ccf_cose_sign1 utility.

```Bash
$ ccf_cose_sign1 --ccf-gov-msg-type ack --ccf-gov-msg-created_at `date -Is` --signing-key member0_privk.pem --signing-cert member0_cert.pem --content request.json | \
  curl https://confidentialbillingapp.confidential-ledger.azure.com/gov/ack --cacert service_cert.pem --data-binary @- -H "content-type: application/cose"
```

3. After the command completes, the new member becomes active and can take part in the governance operations. You can view the members in the resource using the following command.

[!INCLUDE [View members](./includes/view-members.md)]

## Next steps

- [Microsoft Azure Managed CCF overview](overview.md)
- [Quickstart: Create an Azure Managed CCF resource](quickstart-portal.md)
- [Quickstart: Deploy an Azure Managed CCF application](quickstart-deploy-application.md)