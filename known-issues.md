---

copyright:
  years: 2019, 2020
lastupdated: "2020-09-21"

keywords: catalina, chrome, external CA, TLS, orderer, error

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

# Known issues
{: #sw-known-issues}

<div style="background-color: #6fdc8c; padding-left: 20px; padding-right: 20px; border-bottom: 4px solid #0f62fe; padding-top: 12px; padding-bottom: 4px; margin-bottom: 16px;">
  <p style="line-height: 20px;">
    <strong>Important: You are not looking at the latest product documentation.  Make sure you are reading the documentation that matches the version of the software that you are using. Switch to product version </strong>
    <a href="https://cloud.ibm.com/docs/blockchain-sw-213?topic=blockchain-sw-213-sw-known-issues">2.1.3</a>,
    <a href="https://cloud.ibm.com/docs/blockchain-sw-25?topic=blockchain-sw-25-sw-known-issues">2.5 (latest)</a>
    </p>
</div>

## Ordering service randomly disappears
{: #sw-known-issues-ordering-service-delete}

Occasionally, ordering service nodes are deleted by the Kubernetes garbage collector because it considers the nodes a resource that needs to be cleaned up. This process is both random and unrecoverable --- if the ordering service is deleted, all of the channels hosted on it are permanently lost.

To prevent this, the `ownerReferences` field must be removed from the configuration file of each ordering node. **Complete these steps as soon as possible** to prevent your ordering service from being deleted.
{: important}

First, get the list of orderers in the namespace:

```
kubectl  get ibporderers.ibp.com -n <NAMESPACE>
```
Replace
- `<NAMESPACE>` with the name of your Kubernetes namespace or OCP project.

You'll see output similar to:

```
NAME            AGE
orderera        5m
ordereranode1   5m
ordereranode2   5m
ordereranode3   5m
ordereranode4   5m
ordereranode5   5m
```

The ordering nodes with `node` in the name are the actual nodes of your ordering service. This example assumes a five node ordering service as this naming convention is given to five ordering nodes when they are deployed. All of these nodes must be edited. If you only deployed a single node ordering service you will only need to edit the one node, for example `ordereranode1`.

First, pull the YAML configuration file of one of the nodes:

```
kubectl get ibporderers.ibp.com -n <NAMESPACE> <NAME_OF_NODE> -o yaml > <NAME_OF_NODE>.yaml
```
Replace:
- `<NAMESPACE>` with the name of your Kubernetes namespace or OCP project.
- `<NAME_OF_NODE>` with the name of the node you are updating from the list above. For example: `ordereranode1`.

Open the YAML file in an editor of your choice. It should look similar to this:

```yaml
apiVersion: ibp.com/v1alpha1
kind: IBPOrderer
metadata:
  creationTimestamp: "2020-03-21T17:26:54Z"
  generation: 1
  labels:
    app: ordereranode1
    app.kubernetes.io/instance: ibporderer
    app.kubernetes.io/managed-by: ibp-operator
    app.kubernetes.io/name: ibp
    creator: ibp
    parent: orderera
  name: ordereranode1
  namespace: ibp212
  ownerReferences:
  - apiVersion: ibp.com/v1alpha1
    blockOwnerDeletion: true
    controller: true
    kind: IBPOrderer
    name: orderera
    uid: 2166e032-6b99-11ea-8dcc-9a49e6636cea
  resourceVersion: "67999182"
  selfLink: /apis/ibp.com/v1alpha1/namespaces/ibp212/ibporderers/ordereranode1
  uid: 28d93feb-6b99-11ea-8dcc-9a49e6636cea
spec:
  clusterSize: 1
```

Remove the `ordererReferences` section. It will look similar to the following:

```yaml
ownerReferences:
  - apiVersion: ibp.com/v1alpha1
    blockOwnerDeletion: true
    controller: true
    kind: IBPOrderer
    name: orderera
    uid: 2166e032-6b99-11ea-8dcc-9a49e6636cea
```

The resulting YAML file will look similar to this:

```yaml
apiVersion: ibp.com/v1alpha1
kind: IBPOrderer
metadata:
  creationTimestamp: "2020-03-21T17:26:54Z"
  generation: 1
  labels:
    app: ordereranode1
    app.kubernetes.io/instance: ibporderer
    app.kubernetes.io/managed-by: ibp-operator
    app.kubernetes.io/name: ibp
    creator: ibp
    parent: orderera
  name: ordereranode1
  namespace: ibp212
  resourceVersion: "67999182"
  selfLink: /apis/ibp.com/v1alpha1/namespaces/ibp212/ibporderers/ordereranode1
  uid: 28d93feb-6b99-11ea-8dcc-9a49e6636cea
spec:
  clusterSize: 1
```

Save this file and apply it back to your cluster:

```
kubectl apply -f <NAME_OF_NODE>.yaml
```
Replace
- `<NAME_OF_NODE>` with the name of the node you are updating from the list above. For example: `ordereranode1`.

Repeat this process for all ordering nodes in the ordering service. Note that you should complete this process before upgrading to v2.1.3 or higher.

## Chrome browser on Mac OS Catalina
{: #sw-known-issues-catalina}

The console will not work in the Chrome browser on Mac OS Catalina when the {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0 or v2.1.1 is deployed with the default configuration that uses self-signed certificates. There are three ways to resolve this problem:

1.  Use a different [supported browser](/docs/blockchain-sw?topic=blockchain-sw-deploy-ocp#deploy-ocp-browsers) with Catalina.
2. Use your own [TLS certificates when deploying on OpenShift Container Platform](/docs/blockchain-sw?topic=blockchain-sw-deploy-ocp#use-your-own-tls-certificates-optional-) or [TLS certificates when deploying on Kubernetes or {{site.data.keyword.cloud_notm}} Private](/docs/blockchain-sw?topic=blockchain-sw-deploy-k8#use-your-own-tls-certificates-optional-).
3. Run the following commands to generate a new key and certificate pair for the console that will fix the problem.
   - Run the following command to get the pod that corresponds to the ibp console:
      ```
      kubectl get po
      ```
      {: codeblock}
   - Exec into the pod by running the command:
      ```
      kubectl get po <pod-name> -c optools bash
      ```
      {: codeblock}
   - Delete the console key and certificate by running the command:
      ```
      rm -f /certs/tls.key rm -f /certs/tls.crt
      ```
      {: codeblock}
   - Delete the console pod which causes it to restart by running the command:
      ```
      kubectl delete po <pod-name>
      ```
      {: codeblock}
    When the pod restart completes, you should now be able to login to your console URL from a Chrome Browser.

## Exporting an ordering node shows error for missing TLS certs
{: #sw-known-issues-orderer-tls-cert-error}

When attempting to export an ordering node, you might see the following error: `Some ordering service TLS certificates could not be retrieved at this time and will not be included in the exported information.`

You can safely ignore this error, as the certificates the console is trying to pull are only needed for a feature that is not currently enabled.

## Unable to create ordering node using external CA certificates
{: #sw-known-issues-external-CA-cant-create}

Currently, it is not possible to create ordering nodes using certificates entered into the console as described in [Component governance](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-govern-components#ibp-console-govern-third-party-ca).

It is still possible to create components using the normal register and enroll flows in the console.
