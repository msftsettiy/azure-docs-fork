---
title: Build and deploy a JavaScript application in a local development environment
description: Learn to build and deploy a JavaScript application in a local development environment
author: msmbaldwin, msftsettiy
ms.author: mbaldwin, settiy
ms.date: 09/09/2023
ms.service: managed-ccf
ms.topic: how-to
ms.custom: devx-track-azurecli
---

In this turorial, you will learn how to build, deploy and test a JavaScript confidential application in the local development environment. We will deploy a 1-node CCF network locally and view the logs. It uses the virtual CCF image which does not provide the hardware-backed confidentiality of the Trusted Execution Environments(TEE). It is recommended only for development purposes and not for production workloads.

## Prerequisites

[!INCLUDE [Prerequisites](./includes/proposal-prerequisites.md)]
- [Install CCF](https://github.com/Microsoft/CCF/releases)

> [!Note]
> This tutorial uses the banking sample app available at [Banking Application](https://github.com/microsoft/ccf-app-samples/tree/main/banking-app) for demostration. However, the steps explained in this tutorial applies to any other JavaScript application targetting CCF. 

> In this tutorial, we use [Visual Studio Code](https://code.visualstudio.com/) as the IDE. But, any IDE with support for Nodejs and JavaScript application development can be used.

## Build

1. Follow the detailed instructions available [here](https://microsoft.github.io/CCF/main/build_apps/js_app_ts.html) to setup the application folders and the dependencies.

2. Build the application. The application bundle is created at dist/set_js_app.json.

```bash
$:~/ccf-app-samples/banking-app/dist$make build
build

> build
> del-cli -f dist/ && rollup --config && cp app.json dist/ && node build_bundle.js dist/


src/endpoints/all.ts → dist/src...
created dist/src in 1.3s
Writing bundle containing 8 modules to dist/bundle.json
$:~/ccf-app-samples/banking-app/dist$ ls -ltr dist
total 40
drwxr-xr-x 4 settiy settiy  4096 Sep 11 10:20 src
-rw-r--r-- 1 settiy settiy  3393 Sep 11 10:20 app.json
-rw-r--r-- 1 settiy settiy 16146 Sep 11 10:20 set_js_app.json
-rw-r--r-- 1 settiy settiy 16061 Sep 11 10:20 bundle.json
```
## Deploy a 1-node CCF network

4. Run the sandbox.sh script to start a 1-node CCF network and deploy the JavaScript application bundle that was created in the previous step. 

```bash
$:~/ccf-app-samples/banking-app$ sudo /opt/ccf_virtual/bin/sandbox.sh --js-app-bundle ~/ccf-app-samples/banking-app/dist/
Setting up Python environment...
Python environment successfully setup
[10:40:37.516] Virtual mode enabled
[10:40:37.517] Starting 1 CCF node...
[10:40:41.488] Started CCF network with the following nodes:
[10:40:41.488]   Node [0] = https://127.0.0.1:8000
[10:40:41.489] You can now issue business transactions to the libjs_generic application
[10:40:41.489] Loaded JS application: /home/demouser/ccf-app-samples/banking-app/dist/
[10:40:41.489] Keys and certificates have been copied to the common folder: /home/demouser/ccf-app-samples/banking-app/workspace/sandbox_common
[10:40:41.489] See https://microsoft.github.io/CCF/main/use_apps/issue_commands.html for more information
[10:40:41.490] Press Ctrl+C to shutdown the network
```

5. The member certificate and the private key is available at /workspace/sandbox_0. The application log is available at /workspace/sandbox_0/out.

```bash
$:~/ccf-app-samples/banking-app$ ls -ltr workspace/sandbox_0
total 108
lrwxrwxrwx 1 root root    79 Sep 11 11:50 0.config.json -> /home/demouser/ccf-app-samples/banking-app/workspace/sandbox_common/0.config.json
lrwxrwxrwx 1 root root    45 Sep 11 11:50 libjs_generic.virtual.so -> /opt/ccf_virtual/lib/libjs_generic.virtual.so
lrwxrwxrwx 1 root root    27 Sep 11 11:50 cchost -> /opt/ccf_virtual/bin/cchost
-rw-r--r-- 1 root root   656 Sep 11 11:50 member0_cert.pem
-rw-r--r-- 1 root root   451 Sep 11 11:50 member0_enc_pubk.pem
-rw-r--r-- 1 root root   594 Sep 11 11:50 validate.js
-rw-r--r-- 1 root root 45285 Sep 11 11:50 actions.js
-rw-r--r-- 1 root root   278 Sep 11 11:50 resolve.js
-rw-r--r-- 1 root root     0 Sep 11 11:50 err
-rw-r--r-- 1 root root   278 Sep 11 11:50 apply.js
-rw-r--r-- 1 root root     5 Sep 11 11:50 node.pid
-rw-r--r-- 1 root root    42 Sep 11 11:50 0.rpc_addresses
-rw-r--r-- 1 root root    44 Sep 11 11:50 0.node_address
-rw-r--r-- 1 root root   664 Sep 11 11:50 service_cert.pem
-rw-r--r-- 1 root root   676 Sep 11 11:50 0.pem
drwxr-xr-x 2 root root  4096 Sep 11 11:50 0.ledger
drwxr-xr-x 2 root root  4096 Sep 11 11:50 0.snapshots
-rw-r--r-- 1 root root  7255 Sep 11 11:51 out
```

6. At this point, we created a local CCF network with a member and deployed the application. The network endpoint is https://127.0.0.1:8000. The member can participate in governance operations like updating the application or adding more members by submitting a proposal.

```Bash
$:~/ccf-app-samples/banking-app$ curl -k  https://127.0.0.1:8000/node/version | jq
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    73  100    73    0     0   6083      0 --:--:-- --:--:-- --:--:--  6083
{
  "ccf_version": "ccf-4.0.7",
  "quickjs_version": "2021-03-27",
  "unsafe": false
}
```

## Next steps

- [Microsoft Azure Managed CCF overview](overview.md)
- [How to: Manage members](how-to-manage-members.md)
- [How to: Update a Managed CCF application](how-to-update-application.md)
- [How to: Update the JavaScript runtime options](how-to-update-js-runtime-options.md)