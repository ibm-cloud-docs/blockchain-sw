---

copyright:
  years: 2019, 2020
lastupdated: "2020-02-17"
keywords: troubleshooting, debug, why, what does this mean, how can I, when I

subcollection: blockchain-sw

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# Troubleshooting
{: #ibp-v2-troubleshooting}

General problems may occur when using the console to manage nodes, channels, or smart contracts. In many cases, you can recover from these problems by following a few easy steps.
{:shortdesc}

This topic describes common issues that can occur when using the {{site.data.keyword.blockchainfull_notm}} Platform console.  

**Issues during Deployment**
- [My deployment fails when I try apply the security and access policies to my namespace](#ibp-v2-troubleshooting-deployment-policies)
- [My deployment fails when I try apply the custom resource definition of the console or operator](#ibp-v2-troubleshooting-deployment-cr)

**Issues with the Console**
- [Why are my console actions failing in my Chrome browser Version 77.0.3865.90 (Official Build) (64-bit)?](#ibp-v2-troubleshooting-chrome-v77)
- [Why am I not able to log in to the console from my Chrome browser on Mac OS Catalina?](#ibp-v2-troubleshooting-console-catalina)
- [Why is my channel creation failing or I am unable to add a new organization to my ordering service with the error "Unable to get system channel"?](#ibp-v2-troubleshooting-accept-tls)
- [When I hover over my node, the status is `Status unavailable`, what does this mean?](#ibp-v2-troubleshooting-status-unavailable)
- [When I hover over my node, the status is `Status undetectable`, what does this mean?](#ibp-v2-troubleshooting-status-undetectable)
- [Why did my smart contract installation, instantiation or upgrade fail?](#ibp-console-smart-contracts-troubleshoot-entry1)
- [Why is the smart contract that I installed on the peer not listed in the UI?](#ibp-console-build-network-troubleshoot-missing-sc)
- [My channel, smart contracts, and identities have disappeared from the console. How can I get them back?](/docs/blockchain-sw?topic=blockchain-sw-ibp-v2-troubleshooting#ibp-v2-troubleshooting-browser-storage)
- [Why am I getting the error `Unable to authenticate with the enroll ID and secret you provided` when I create a new organization MSP definition?](#ibp-v2-troubleshooting-create-msp)
- [Why am I getting the error `An error occurred when updating channel` when I try to add an organization to my channel?](#ibp-v2-troubleshooting-update-channel)
- [When I log in to my console, why am I getting a 401 unauthorized error?](#ibp-v2-troubleshooting-console-401)

**Issues with your Nodes**  

- [Why is my first invoke of a smart contract returning the following error: no suitable peers available to initialize from?](#ibp-v2-troubleshooting-smart-contract-anchor-peers)
- [Why are my node operations failing after I create my peer or ordering service?](#ibp-console-build-network-troubleshoot-entry1)
- [Why does my peer fail to start?](#ibp-console-build-network-troubleshoot-entry2)
- [Why are my transactions returning an endorsement policy error: signature set did not satisfy policy?](#ibp-v2-troubleshooting-endorsement-sig-failure)
- [How can I view my smart contract container logs?](#ibp-console-smart-contracts-troubleshoot-entry2)
- [Why are the transactions I submit from VS Code failing with a No endorsement plan available error?](#ibp-v2-troubleshooting-anchor-peer)
- [Why are the transactions I submit from VS Code failing with an endorsement failure?](#ibp-v2-troubleshooting-endorsement)


## My deployment fails when I try apply the security and access policies to my namespace
{: #ibp-v2-troubleshooting-deployment-policies}
{: troubleshoot}

When I try to deploy the {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 and apply the Security Context Constraint, clusterRole, or ClusterRoleBinding to my namespace, I encounter one of the following errors.

When I apply the file, I receive a user forbidden error:
{: tsSymptoms}

```
securitycontextconstraints.security.openshift.io "e-block2" is forbidden: User cannot get securitycontextconstraints.security.openshift.io at the cluster scope: no RBAC policy matched
```

This problem occurs when you are not a cluster administrator. You must have a cluster administrator role and apply the security and access policies to your namespace.
{: tsCauses}

When I try to apply resource file, I receive a parsing error:
{: tsSymptoms}

```
error: error parsing ibp-scc.yaml: error converting YAML to JSON: yaml: line 10: mapping values are not allowed in this context
```

This problem occurs when there is a problem with the indents in your file. Refer to the documentation for the correct format for the security and access files.
{: tsCauses}


## My deployment fails when I try apply the custom resource definition of the console or operator
{: #ibp-v2-troubleshooting-deployment-cr}
{: troubleshoot}

When I try to deploy the {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 and apply the custom resource definition of the operator or the console, I encounter one of the following errors:

When I apply the custom resource file, I receive an image pull or image pull back-off error:
{: tsSymptoms}

```
NAME                            READY     STATUS             RESTARTS   AGE
ibp-operator-769d94ffbc-w52n6   0/1       ImagePullBackOff   0          32s
```

This problem occurs when your deployment cannot pull the {{site.data.keyword.blockchainfull_notm}} Platform images from the {{site.data.keyword.IBM_notm}} Entitlement registry. This can happen because you provided an incorrect name of your secret to the `imagePullSecrets:` field, or if there was a problem with the url or the key that you provided to the entitlement key secret. This error can also occur if you supplied the wrong image tag to the file.
{: tsCauses}

When I try to apply the custom resource file, I receive a parsing error:
{: tsSymptoms}

```
error: error parsing console.yaml: error converting YAML to JSON: yaml: line 11: mapping values are not allowed in this context
```

This problem occurs when there is a problem with the indents in your file. Refer to the documentation for the correct format for the custom resource files of the console and operator.
{: tsCauses}


## Why are my console actions failing in my Chrome browser Version 77.0.3865.90 (Official Build) (64-bit)?
{: #ibp-v2-troubleshooting-chrome-v77}
{: troubleshoot}

The console has been working successfully, but requests have started to fail. For example, after I create an ordering service and open it I see the error: `Unable to get system channel. If you associated an identity without administrative privilege on the ordering service node, you will not be able to view or manage ordering service details.`
{: tsSymptoms}

This problem can be caused by a [bug](https://bugs.chromium.org/p/chromium/issues/detail?id=1006243){: external} introduced by the Chrome browser `Version 77.0.3865.90 (Official Build) (64-bit)` that causes actions from the browser to fail.
{: tsCauses}

To resolve this problem, open the console in a new browser tab in Chrome. Any identities that you saved in your console wallet will persist in the new browser tab. To avoid this problem you can upgrade your Chrome browser version. Ensure you have downloaded all of your wallet identities to your local machine before closing your browser. If this solution does not resolve your problem see [Why is my channel creation failing or I am unable to add a new organization to my ordering service with the error "Unable to get system channel"?](#ibp-v2-troubleshooting-accept-tls).
{: tsResolve}


## Why am I not able to log in to the console from my Chrome browser on Mac OS Catalina?
{: #ibp-v2-troubleshooting-console-catalina}
{: troubleshoot}

The console has been working successfully, but after I upgraded my Mac OS to Catalina, I can no longer log in to the console.
{: tsSymptoms}

There are three ways to resolve this problem:
{: tsResolve}
1.  Use a different [supported browser](/docs/blockchain-sw?topic=blockchain-sw-deploy-ocp#deploy-ocp-browsers) with Catalina.
2. Use your own [TLS certificates when deploying on OpenShift Contain Platform](/docs/blockchain-sw?topic=blockchain-sw-deploy-ocp#use-your-own-tls-certificates-optional-) or [TLS certificates when deploying on Kubernetes or {{site.data.keyword.cloud_notm}} Private](/docs/blockchain-sw?topic=blockchain-sw-deploy-k8#use-your-own-tls-certificates-optional-).
3. Run the following commands to generate a new key and certificate pair for the console that will fix the problem.
   1. Run the following command to get the pod that corresponds to the ibp console:
      ```
      kubectl get po
      ```
      {: codeblock}
   2. Exec into the pod by running the command:
      ```
      kubectl get po <pod-name> -c optools bash
      ```
      {: codeblock}
   3. Delete the console key and certificate by running the command:
      ```
      rm -f /certs/tls.key rm -f /certs/tls.crt
      ```
      {: codeblock}
   4. Delete the console pod which causes it to restart by running the command:
      ```
      kubectl delete po <pod-name>
      ```
      {: codeblock}
   When the pod restart completes, you should now be able to login to your console URL from a Chrome Browser.

## Why is my channel creation failing or I am unable to add a new organization to my ordering service with the error "Unable to get system channel"?
{: #ibp-v2-troubleshooting-accept-tls}
{: troubleshoot}

If you are not using your own TLS certificates to secure communications in your blockchain network, you need to accept the self-signed certificate that was generated for you. Otherwise, when you try to create a channel or add an organization to an ordering service the action fails. Channel creation fails with the error `An error occurred when creating channel. submit config update failed: grpc code ???= response contains no code or message`. Or, when you click on your ordering service, you see `Unable to get system channel. If you associated an identity without administrative privilege on the ordering service node, you will not be able to view or manage ordering service details`.
{: tsSymptoms}

This problem occurs when the blockchain console is deployed without the advanced deployment option to [use your own TLS certificates](/docs/blockchain-sw?topic=blockchain-sw-deploy-ocp#use-your-own-tls-certificates-optional-).
{: tsCauses}

To resolve this problem, you need to accept the self-signed certificate in your browser.
{: tsResolve}

1. Copy your console URL, for example `https://<PROJECT-NAME>-ibpconsole-console.abc.xyz.com:443`, where the value of `abc.xyz.com` depends on your cloud provider.
2. Replace `-console` with `-proxy`. The new URL looks similar to: `https://<PROJECT-NAME>-ibpconsole-proxy.abc.xyz.com:443`.
3. Open a new tab in your browser and paste in the new URL.
4. Accept the certificate.

You need to accept the certificate from this url to communicate with your nodes from your console and then log in as usual. When you switch to a new machine or a new browser, you need to repeat these steps.


## When I hover over my node, the status is `Status unavailable` or `Status unknown`, what does this mean?
{: #ibp-v2-troubleshooting-status-unavailable}
{: troubleshoot}

A CA, peer, or ordering node has a grey status box, meaning the status of the node is not available. Ideally, when you hover over any node, the node status should be `Running`.
{: tsSymptoms}

This problem can occur if the node is newly created and the deployment process has not completed. If the node is a CA and the status has been grey for more than a few minutes, then it is likely that the deployment process has failed. Peers and ordering nodes take longer to deploy, but this condition can also occur when the health checker that runs against the peer or ordering nodes cannot contact the node. The request for status can fail with a timeout error because the node did not respond within a specific time period, the node could be down, or network connectivity is down.
{: tsCauses}

If this is a new node, wait a few more minutes for the deployment to complete. You can try reloading the page in your browser to refresh the status. If the node is not new,
 [examine the associated node logs](/docs/blockchain-sw?topic=blockchain-sw-console-icp-manage#console-icp-manage-console-logs) for errors to determine the cause.
{: tsResolve}

## When I hover over my node, the status is `Status undetectable`, what does this mean?
{: #ibp-v2-troubleshooting-status-undetectable}
{: troubleshoot}

The node status in the tile for the peer or ordering node is yellow, meaning the status of the node cannot be detected. Ideally, when you hover over any node, the node status should be `Running`.
{: tsSymptoms}

This condition only occurs on peer and ordering nodes that were *imported* to the console and the health checker cannot run against the node. This status happens because an `operations_url` was not specified when the node was imported. An operations url is required for the node health checker to run. The node itself is likely `Running`, but because the operations url was not specified, its status cannot be determined.
{: tsCauses}

You can resolve this problem by performing the following steps:
 1. Click the node tile to open it.
 2. Click the **Settings** icon.
 3. Click **Associate identity**, view and make note of the identity that is associated with this node. Click **Cancel** to close this panel.
 4. Click **Export** to generate a `JSON` file for the node.
 5. Edit the generated `JSON` file and enter the `operations_url` for the node. The value of the `operations_url` depends on how it was configured as well as various network settings. This value needs to be provided by the network administrator who deployed the node.
 6. Click **Delete**. This step removes the imported node from the console, but does not delete the actual node.
 7. From the **Nodes** tab, click **Add Peer** or **Add ordering service** followed by **Import an existing peer** or **Import an existing Ordering service**.
 8. Click **Upload JSON** and browse to the JSON file you just edited. Click **Next**.
 9. Associate the same identity you noted in step three.
 10. Click **Add peer** or **Add ordering service**.
The health checker can now run against the node and report the status of the node.
{: tsResolve}



## Why did my smart contract installation, instantiation or upgrade fail?
{: #ibp-console-smart-contracts-troubleshoot-entry1}
{: troubleshoot}
{: support}

It is possible you may experience an error when installing, instantiating or upgrading a smart contract. For example, when you try to install a smart contract on a peer, it fails with the error `An error occurred when installing smart contract on peer.`
{: tsSymptoms}

You may receive this error if this version of the smart contract already exists on the peer, or if your peer is out of resources.
{: tsCauses}

- Open your Kubernetes dashboard and ensure the peer status is `Running`.
- Open the peer node and ensure the smart contract version does not already exist on the peer and try again with the proper version.
- If you are still experiencing problems after the node is up,  [check your node logs](/docs/blockchain-sw?topic=blockchain-sw-console-icp-manage#console-icp-manage-node-logs) for errors.
{: tsResolve}

## Why is the smart contract that I installed on the peer not listed in the UI?
{: #ibp-console-build-network-troubleshoot-missing-sc}
{: troubleshoot}

A smart contract was installed on a peer but when you click on the **Smart contracts** tab, it is not listed.
{: tsSymptoms}

This issue can occur when another user or application installs the smart contract onto the peer and you do not have the peer admin identity in your browser wallet.
{: tsCauses}

In order to view the smart contracts installed on a peer, you need to be a peer admin. Ensure that the peer admin identity exists in your browser wallet. If it does not, you need to import it into your console wallet.
{: tsResolve}

## My channel, smart contracts, and identities have disappeared from the console. How can I get them back?
{: #ibp-v2-troubleshooting-browser-storage}
{: troubleshoot}
{: support}

The identities in your wallet consist of a signing certificate and private key that allow you to manage your blockchain components. However, they are only stored in your browser local storage. You are responsible for securing and managing these identities. We recommend that you export them to your file system after you create them. Whenever you create a new node, you associate an identity from your wallet with the node. This admin identity is what allows you to manage the node. When you switch browsers or change to a browser on a different machine, these identities are no longer in your wallet. Therefore, you are unable to manage the components.
{: tsSymptoms}

One of the features of {{site.data.keyword.blockchainfull_notm}} Platform is that you are now responsible for securing and managing your certificates. Therefore, they are only persisted in the browser local storage to allow you to manage the component. If you are using a private browser window and then switch to another browser or non-private browser window, the identities that you created will be gone from your wallet in the new browser session. Therefore, it is required that you export the identities from the wallet in your private browser session to your file system. You can then import them into your non-private browser session if they are needed. Otherwise, there is no way to recover them.
{: tsCauses}

- Anytime when you create a new organization MSP definition, you generate keys for an identity that is allowed to administer the organization. Therefore, during that process you must click the **Generate** and then **Export** buttons to store the generated identity in your wallet and then save it to your file system as a JSON file.
- To resolve this problem in your browser, you need to import those identities and associate them with the corresponding node:
  - In the browser where you are experiencing the problem, click the **Wallet** tab followed by **Add identity** to import the JSON file into your wallet.
  - Click **Upload JSON** and browse to the JSON file you exported using the **Add files** button.
  - Click **Submit**.
  - Now open the peer or ordering service node that this identity was originally associated to, and click on the **Settings** icon.
  - Click the **Associate identity** button.
  - Select the identity you just imported to your console wallet from the drop-down list.
  - Click **Associate**.
- Repeat this process for each identity that was in the wallet of the original browser.
{: tsResolve}

This problem can also occur when the console has lost contact with your Kubernetes cluster on {{site.data.keyword.cloud_notm}}. This can happen if your console has not recently communicated with your cluster, or if you have made changes to your cluster that have overridden the settings used by the {{site.data.keyword.blockchainfull_notm}} platform.
{: tsCauses}

You can click the **Refresh your linked cluster** button to renew the communication between your console and the {{site.data.keyword.IBM_notm}} Kubernetes service cluster on {{site.data.keyword.cloud_notm}}. Click **{{site.data.keyword.cloud_notm}} Support** next to the **Launch the {{site.data.keyword.blockchainfull_notm}} Platform**. This will take you to the panel where you can click **Refresh your linked cluster**.
{: tsResolve}

## Why am I getting the error `Unable to authenticate with the enroll ID and secret you provided` when I create a new organization MSP definition?
{: #ibp-v2-troubleshooting-create-msp}
{: troubleshoot}
{: support}

When you attempt to create a new organization MSP definition from the Organizations tab, you experience the error `Unable to authenticate with the enroll ID and secret you provided`.
{: tsSymptoms}

This error occurs when the value that you specified for the enroll secret is not valid for the enroll ID that you selected in the `Generate organization admin certificate` section of the panel.
{: tsCauses}

Verify that you have selected the correct organization admin enroll ID from enroll ID drop down list and enter the correct value for the enroll secret.
{: tsResolve}

## Why am I getting the error `An error occurred when updating channel` when I try to add an organization to my channel?
{: #ibp-v2-troubleshooting-update-channel}
{: troubleshoot}

When you attempt to add another organization to a channel, the update fails with the message `An error occurred when updating channel`.
{: tsSymptoms}

This error occurs when the selected **Channel Updater MSP ID** on the **Update channel** panel is not an admin of the channel.
{: tsCauses}

On the **Update channel** panel, scroll down to the **Channel Updater MSP ID** and select the MSP ID that was specified when the channel was created or specify the MSP ID that is the admin of the channel.
{: tsResolve}

## When I log in to my console, why am I getting a 401 Unauthorized error?
{: #ibp-v2-troubleshooting-console-401}
{: troubleshoot}
{: support}

When I try to log in to my console, I am unable to access the console from my browser. If I check the browser logs, I can find the error 401 unauthorized.
{: tsSymptoms}

Your browser console session times out after **8 hours** of inactivity. If a session becomes inactive, the console will prevent the inactive user from performing any actions.
{: tsCauses}

If your session has become inactive, you can try simply refreshing your browser. If that does not work, close the browser including **all** tabs and windows. Open the URL again. You will be required to log in.

As a best practice, you should have already stored your certificates and identities on your file system. If you happen to be using an incognito window, all the certificates are deleted from the browser local storage when you close the browser. After you log in again you will need to re-import your identities and certificates.
{: note}

## Why is my first invoke of a smart contract returning the following error: no suitable peers available to initialize from?
{: #ibp-v2-troubleshooting-smart-contract-anchor-peers}
{: troubleshoot}
{: support}

When I try to invoke a smart contract from the Fabric SDK, the transaction fails and returns the following error:
{: tsSymptoms}

```
error: [Network]: _initializeInternalChannel: no suitable peers available to initialize from Failed to submit transaction: Error: no suitable peers available to initialize from
```

This error occurs if you have not configured an anchor peer on your channel. Unless you have manually updated your connection profile, your application needs to use the [Service Discovery](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external} feature to learn about the peers it needs to submit the transaction to.
{: tsCauses}

Use the following steps to [configure anchor peers on your channel](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-govern#ibp-console-govern-channels-anchor-peers).
Also, you should verify that you are submitting the transactions against the correct channel and organization MSP id.
{: tsResolve}

## Why are my node operations failing after I create my peer or ordering service?
{: #ibp-console-build-network-troubleshoot-entry1}
{: troubleshoot}

It is possible you may experience an error when managing an existing node. When that occurs, it is often useful to consult the node logs.

For example, when you try to operate the node, the action might fail.
{: tsSymptoms}

After creating a new peer or ordering service, depending on your cluster storage configuration, it may take a few minutes for the nodes to be ready for operation.
{: tsCauses}




Check your Kubernetes dashboard and ensure the peer or node status is `Running`. Then try your action again. If you are still experiencing problems after the node is up, [check your node logs](/docs/blockchain-sw?topic=blockchain-sw-console-icp-manage#console-icp-manage-node-logs) for errors.
{: tsResolve}


## Why does my peer fail to start?
{: #ibp-console-build-network-troubleshoot-entry2}
{: troubleshoot}

It is possible to experience this error under a variety of conditions.

The peer log includes `2019-02-06 19:43:24.159 UTC [main] InitCmd -> ERRO 001 Cannot run peer because cannot init crypto, folder “/certs/msp” does not exist`
{: tsSymptoms}

- This error can occur under the following conditions:
  - When you created the peer or ordering service organization MSP definition, you specified an enroll ID and secret that corresponds to an identity of type `peer` and not `client` or `admin`. It must be of type `client` or `admin`.
  - When you created the peer or ordering service organization MSP definition, you specified an enroll ID and secret that does not match the enroll ID or secret of the corresponding organization admin identity.
  - When you created the peer or ordering service, you specified the enroll ID and secret of an identity that is not type 'peer'.

- Open your peer or ordering service CA node and view the registered identities listed in the **Registered Users** table.
- Delete the peer or ordering service and recreate it, being careful to specify the correct enroll ID and secret.
- Note that before you create the peer or ordering service, you need to create an organization admin id, of type 'client'. Be sure to specify that same id as the enroll ID when you create the organization MSP definition. See these instructions for [registering peer identities](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-build-network#ibp-console-build-network-use-CA-org1) and these instructions for [registering orderer identities](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-build-network#ibp-console-build-network-use-CA-orderer).
{: tsResolve}

## Why are my transactions returning an endorsement policy error: signature set did not satisfy policy?
{: #ibp-v2-troubleshooting-endorsement-sig-failure}
{: troubleshoot}

When I invoke a smart contract to submit a transaction, the transaction returns the following endorsement policy failure:
{: tsSymptoms}

```
returned error: VSCC error: endorsement policy failure, err: signature set did not satisfy policy
```

If you have recently joined a channel and installed the smart contract, this error occurs if you have not added your organization to the endorsement policy. Because your organization is not on the list of organizations who can endorse a transaction from the smart contract, the endorsement from your peers is rejected by the channel. If you encounter this problem, you can change the endorsement policy by upgrading the smart contract. For more information, see [Specifying an endorsement policy](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-smart-contracts#ibp-console-smart-contracts-endorse) and [Upgrading a smart contract](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-smart-contracts#ibp-console-smart-contracts-upgrade).
{: tsCauses}

## How can I view my smart contract container logs?
{: #ibp-console-smart-contracts-troubleshoot-entry2}
{: troubleshoot}

You may need to view your smart contract, or chaincode, container logs to debug a smart contract issue.
{: tsSymptoms}


Follow these [instructions](/docs/blockchain-sw?topic=blockchain-sw-console-icp-manage#console-icp-manage-container-logs) to view your smart contract container logs.
{: tsResolve}




## Why are the transactions I submit from VS Code failing with a No endorsement plan available error?
{: #ibp-v2-troubleshooting-anchor-peer}
{: troubleshoot}

Transactions submitted from VS Code fail with an error similar to:
{: tsSymptoms}

```
Error submitting transaction: No endorsement plan available for {"chaincodes":[{"name":"hello-world"}]}
```

This error occurs if you are using the Fabric Service Discovery feature but did not configure any anchor peers on your channel.
{: tsCauses}

Follow step three of the [private data topic](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data) in the Deploy a smart contract tutorial to configure your anchor peers.
{: tsResolve}


## Why are the transactions I submit from VS Code failing with an endorsement failure?
{: #ibp-v2-troubleshooting-endorsement}
{: troubleshoot}

My smart contract endorsement proposals from my peer are failing from my client application with the endorsement error `error: [Channel.js]: Channel:<channel_name> received discovery error:failed constructing descriptor for chaincodes:<name:"chaincode-name">`
{: tsSymptoms}

Also in the endorsing peer logs I can see the error:
```
UTC [discovery] chaincodeQuery -> ERRO 23c Failed constructing descriptor for chaincode chaincodes:<name:"chaincode-name">,: cannot satisfy any principal combination
```
This error occurs when the peer's enroll id type does not match the smart contract endorsement policy that was configured when the smart contract was instantiated on the channel.
{: tsCauses}

The only way to resolve this error is to delete the peer and create a new one with an enroll id that has the correct type `peer`. You can use the enroll id and secret from an existing user of type `peer` from the peer's CA or register a new user with type `peer`. Follow the instructions in the [Build a network tutorial](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-build-network#ibp-console-build-network-create-peer-org1) to create a new peer identity with the correct type and peer.
{: tsResolve}


