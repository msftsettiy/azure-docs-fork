---
title: Quickstart – Update the JavaScript runtime options on a Microsoft Azure Managed CCF resource
description: Learn to update the JavaScript runtime options on a Microsoft Azure Managed CCF resource
author: msmbaldwin, msftsettiy
ms.author: mbaldwin, settiy
ms.date: 09/08/2023
ms.service: managed-ccf
ms.topic: how-to
ms.custom: devx-track-azurecli
---

# Quickstart: Update the runtime options of the JavaScript execution engine on a Managed CCF resource

Sometimes it is necessary to update the runtime options of the CCF JavaScript interpreter to extend the request execution duration or update the heap or stack allocation size. In this howto guide, you will learn to update the runtime settings. This tutorial builds on the Managed CCF resource created in the [Quickstart: Create an Azure Managed CCF resource](quickstart-portal.md) tutorial.

## Prerequisites

[!INCLUDE [Prerequisites](./includes/proposal-prerequisites.md)]

## Download the service identity

[!INCLUDE [Download Service Identity](./includes/service-identity.md)]

## Update the runtime options

[!INCLUDE [Mac instructions](./includes/macos-instructions.md)]

1. Prepare a **set_js_runtime_options.json** file and submit it using the command below.

```Bash
$ cat set_js_runtime_options.json
{
  "actions": [
    {
      "name": "set_js_runtime_options",
      "args": {
        "max_heap_bytes": 1024,
        "max_stack_bytes": 1024,
        "max_execution_time_ms": 5000, // increase the request execution time
        "log_exception_details": false,
        "return_exception_details": false
      }
    }
  ]
}

$ proposalid=$( (ccf_cose_sign1 --content set_js_runtime_options.json --signing-cert member0_cert.pem --signing-key member0_privk.pem --ccf-gov-msg-type proposal --ccf-gov-msg-created_at `date -Is` | curl https://confidentialbillingapp.confidential-ledger.azure.com/gov/proposals -H 'Content-Type: application/cose' --data-binary @- --cacert service_cert.pem | jq -r ‘.proposal_id’) )
```
2. The next step is to accept the proposal by submitting a vote. 

[!INCLUDE [Submit a vote](./includes/submit-vote.md)]

3. Repeat the above step for every member in the Managed CCF resource.

4. After the proposal is accepted, the runtime options will be applied to the subsequent requests.

## Next steps

- [Microsoft Azure Managed CCF overview](overview.md)
- [Quickstart: View application logs in Log Analytics](quickstart-enable-log-analytics.md)
- [Quickstart: Deploy an Azure Managed CCF application](quickstart-deploy-application.md)