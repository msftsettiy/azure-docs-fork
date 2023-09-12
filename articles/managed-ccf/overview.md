---
title: Microsoft Azure Managed Confidential Consortium Framework
description: An overview of Azure Managed Confidential Consortium Framework, a highly secure service for deploying confidential application.
services: managed-confidential-consortium-framework
author: msmbaldwin,msftsettiy
ms.service: managed-ccf
ms.topic: overview
ms.date: 09/07/2023
ms.author: mbaldwin,settiy

---
# Microsoft Azure Managed Confidential Consortium Framework

Microsoft Azure Managed Confidential Consortium Framework (Managed CCF) is a new and highly secure service for deploying confidential applications. It runs exclusively on hardware-backed secure enclaves, a heavily monitored and isolated runtime environment which keeps potential attacks at bay. Furthermore, Azure Managed Confidential Consortium Framework runs on a minimalistic Trusted Computing Base (TCB), which ensures that no one⁠-not even Microsoft⁠-is "above" the service.

As the name suggests, Azure Managed Confidential Consortium Framework utilizes the [Azure Confidential Computing platform](../confidential-computing/index.yml) and the open-sourced [Confidential Consortium Framework](https://ccf.dev) as the underlying technology to provide a high integrity platform that is tamper-protected and evident. A Managed CCF instance spans across three or more identical machines, each of which run in a dedicated, fully attested hardware-backed enclave. The data integrity is maintained through a consensus-based blockchain.

The following are a few examples of use cases where a CCF application would bring-in value over a non-confidential application.

- Data for purpose
Parties can share key-value data for specific purposes by trusting only the hardware (TEEs). ​
Example: Build applications which selectively reveal aggregated information or selectively reveal raw data to authorized parties (e.g. regulator).​​

- Joint System Administration
Parties can run a confidential system with attested components to propose and approve changes without a single party holding all the power.
Example: Implement system as a CCF application or set of attested components that rely on CCF receipts.

- Transparent System Operation
Parties can operate a system where users can independently confirm that it is run correctly.​​
Example: Compile and share CCF source code for users to audit both the system’s governance and application code; and verify that their transactions are handled according to expectations.​

## Key Features

The key features of Azure Managed CCF are confidentiality, customizable governance, high availability, auditability and transparency.

:::image type="content" source="media/azure-mccf-key-features.png" alt-text="Key features":::

### Confidentiality

The nodes in an Azure Managed CCF run inside a hardware-based Trusted Execution Environment (TEE) which ensures that the data in use is protected and encrypted, while in RAM, and during computation. The following diagram shows how the code and data is protected while in use.

:::image type="content" source="media/tee-enclave-example.png" alt-text="TEE enclave example":::

### Customizable Governance

The participants, called members, share the responsibility for the network operationability which is established by a constitution. The shared governance model establishes trust and transparency among the members through a voting process which is public and auditable. The constitution is implemented as a set of JavaScript scripts which can be customized during the network creation and later. The following diagram shows how the members participate in a governance operation to either accept or reject a proposed action which is enforced by the constitution.

:::image type="content" source="media/governance.png" alt-text="Proposal governance":::

### High Availability

An Azure Managed CCF network is built on top of a network of distributed nodes that maintains an identical replica of the transactions. The platform is built from ground-up to be resilient to network and infrastructure instabilities and be tolerant to node disruptions. The platform guarantees highly availability and quick recoverability by distributing the network nodes across [Azure Availability Zones](https://learn.microsoft.com/en-us/azure/reliability/availability-zones-overview#availability-zones).

### Auditability and Transparency

The state of the network is auditable via receipts. A receipt is a signed proof that is associated with a transaction. The reciepts are verifiable offline and by third parties, equivalent to "this transaction produced this outcome at this position in the network". Together with a copy of the ledger, or other receipts, they can be used to audit the service and hold the consortium accountable. 

The governance operations and the associated public key value maps are stored in plain text. Customers are encouraged to download the ledger and verify it's integrity using open-sourced scripts that are shipped with CCF.

## Open-source CCF(IaaS) vs. Azure Managed CCF (PaaS)

Customers can build applications that target the Confidential Consortium Framework(CCF) and self host it. But it introduces common engineering problems like regular code maintenance and frequent hardware and software updates that are time and resource critical. Azure Managed CCF abstracts away the day-to-day operational tasks and allows the engineering team to stay focused on the business priorities. The following diagram shows the benefits of using Azure Managed CCF over self-hosting.

:::image type="content" source="media/paas-vs-iaas.png" alt-text="Azure Managed CCF vs open-source CCF":::

## Next steps

- [Quickstart: Azure portal](quickstart-portal.md)
- [Quickstart: Azure CLI](quickstart-python.md)
