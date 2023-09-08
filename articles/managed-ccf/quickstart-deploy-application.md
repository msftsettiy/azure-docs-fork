---
title: Quickstart – Deploy an application in a Microsoft Azure Managed CCF resource
description: Learn to deploy an application in a Microsoft Azure Managed CCF resource
author: msmbaldwin, msftsettiy
ms.author: mbaldwin, settiy
ms.date: 09/08/2023
ms.service: managed-ccf
ms.topic: quickstart
ms.custom: devx-track-azurecli
---

# Quickstart: Deploy an application to an Azure Managed CCF resource

In this quickstart tutorial, you will learn how to deploy an application to a Managed CCF resource. This tutorial builds on the Managed CCF resource created in the [Quickstart: Create an Azure Managed CCF resource](quickstart-portal.md) tutorial.

## Prerequisites

- Python 3+.
- Install the latest version of the [CCF Python package](https://pypi.org/project/ccf/).

## Download the service identity

The identity of a Managed CCF resource is represented by a certificate. It is used to establish a TLS connection with the resource nodes and validate the server identity. Download the certificate and save it to service_cert.pem.

```Bash
$ curl https://identity.confidential-ledger.core.azure.com/ledgerIdentity/confidentialbillingapp --silent | jq ' .ledgerTlsCertificate' | xargs echo -e > service_cert.pem
```
## Deploy the application

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

3. After the command completes, the new member becomes active and can take part in the governance operations. You can check the state of the members using the following command.

```Bash
$ curl --cacert service_cert.pem https://confidentialbillingapp.confidential-ledger.azure.com/gov/members | jq
{
  "710c4d7ce6a70a89137b39170cd5c48f94b4756a66e66b2242370111c1c47564": {
    "cert": "-----BEGIN CERTIFICATE-----\nMIIB9zCCAX2gAwIBAgIQW20I1iR...l8Uv8rRce\n-----END CERTIFICATE-----",
    "member_data": {
      "is_operator": true,
      "owner": "Microsoft Azure"
    },
    "public_encryption_key": "-----BEGIN PUBLIC KEY-----\nMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMI...n3QIDAQAB\n-----END PUBLIC KEY-----\n",
    "status": "Active"
  },
  "f9ea379051e5292b538ff2a3dc97f1bb4d5046f12e2bdbf5b8e3acc4516f34e3": {
    "cert": "-----BEGIN CERTIFICATE-----\nMIIBuzCCAUKgAwIBAgIURuSESLma...yyK1EHhxx\n-----END CERTIFICATE-----",
    "member_data": {
      "group": "",
      "identifier": "member0"
    },
    "public_encryption_key": null,
    "status": "Active"
  }
}
```

## Next steps

- [Microsoft Azure Managed CCF overview](overview.md)
- [Quickstart: Create an Azure Managed CCF resource](quickstart-portal.md)
- [Quickstart: Deploy an Azure Managed CCF application](quickstart-deploy-application.md)