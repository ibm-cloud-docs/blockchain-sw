---

copyright:
  years: 2019, 2020
lastupdated: "2020-02-11"

keywords: OpenShift, IBM Blockchain Platform console, deploy, resource requirements, storage, parameters

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

# Upgrading your console and components
{: #upgrade-ocp}

You can upgrade the {{site.data.keyword.blockchainfull}} Platform without disrupting a running network. Because the platform is deployed by using a Kubernetes operator, you can pull the latest {{site.data.keyword.blockchainfull_notm}} Platform images from the {{site.data.keyword.IBM_notm}} Entitlement registry without having to reinstall the platform. You can use these instructions to upgrade to the {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2.
{:shortdesc}

## {{site.data.keyword.blockchainfull_notm}} Platform overview
{: #upgrade-ocp-platform-overview}

You can upgrade to the {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 from any previous release of the {{site.data.keyword.blockchainfull_notm}} Platform. The table provides an overview of past releases.

| Version | Release date | Image tags | New features |
|----|----|----|----|
| [{{site.data.keyword.blockchainfull_notm}} Platform v2.1.2](/docs/blockchain-sw?topic=blockchain-sw-whats-new#whats-new-12-17-2019) | 17 December 2019 | **Console and tools** <ul><li>2.1.2-20191217-amd64</li><li>2.1.2-20200213-amd64</li></ul> **Fabric nodes** <ul><li>1.4.4-20191217-amd64</ul> **CouchDB** <ul><li>2.3.1-20191217-amd64</ul> | **Fabric Version Upgrade** <ul><li>Fabric version 1.4.4</ul> **Additional platforms** <ul><li>Platform can be deployed on the OpenShift Container Platform 4.1 and 4.2</ul> **Improvements to the Console UI** <ul><li>Simplified component creation flows</li><li>Zone selection for ordering nodes</li><li>Add peer to a channel from Channels tab</li><li>Anchor peer during join</li><li>Export/Import all</ul> |
| [{{site.data.keyword.blockchainfull_notm}} Platform v2.1.1]( /docs/blockchain-sw?topic=blockchain-sw-whats-new#whats-new-11-08-2019)| 8 November 2019 | **Console and tools** <ul><li>2.1.1-20191217-amd64</ul> **Fabric nodes** <ul><li>1.4.3-20191217-amd64</ul> **CouchDB** <ul><li>2.3.1-20191217-amd64</ul> | **Additional platforms** <ul><li>Platform can be deployed on Kubernetes v1.14 - v1.16</li><li>Platform can be deployed on {{site.data.keyword.cloud_notm}} Private 3.2.1</li></ul> |
| [{{site.data.keyword.blockchainfull_notm}} Platform v2.1.0](/docs/blockchain-sw?topic=blockchain-sw-whats-new#whats-new-9-24-2019) | 24 September 2019 | **Console and tools** <ul><li>2.1.0-20190918-amd64</ul> **Fabric nodes** <ul><li>1.4.3-20190918-amd64</ul> **CouchDB** <ul><li>2.3.1-20190918-amd64</ul> | **Fabric Version Upgrade** <ul><li>Fabric version 1.4.3</ul> **Additional platforms** <ul><li>Platform can be deployed on the OpenShift Container Platform 3.11</ul> |
{: caption="Table 1. {{site.data.keyword.blockchainfull_notm}} Platform versions" caption-side="bottom"}

If you are using {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0 or v2.1.1, you cannot access the console from the Chrome browser on Mac OS Catalina when the console is deployed with the default configuration that uses self-signed certificates. For more information on how you can resolve this problem, see [Chrome browser on Mac OS Catalina](/docs/blockchain-sw?topic=blockchain-sw-sw-known-issues#sw-known-issues-catalina) in Known Issues.
{:note}

## Upgrading platforms
{: #upgrade-ocp-platform}

If you are using {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0 or V2.1.1 on the OpenShift Container Platform 3.11, you can upgrade your network to run on OpenShift Container Platform 4.2. Because the {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0 or v2.1.1 cannot run on OpenShift Container Platform 4.x, you need to upgrade your blockchain network before you upgrade your cluster. First, follow the steps to [upgrade your network to the {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2](#upgrade-ocp-steps). You can then migrate your OpenShift cluster from 3.11 to 4.2. You cannot migrate your OpenShift cluster from 3.11 to 4.1. For more information, see [Migrating OpenShift Container Platform 3.7 to 4.2](https://docs.openshift.com/container-platform/4.2/migration/migrating-3-4/migrating-openshift-3-to-4.html#migrating-openshift-3-to-4).

## Upgrade to the {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2
{: #upgrade-ocp-steps}

You can upgrade an {{site.data.keyword.blockchainfull_notm}} Platform network by using the following steps:

1. [Update the ClusterRole](#upgrade-ocp-clusterrole)
2. [Upgrade the {{site.data.keyword.blockchainfull_notm}} Platform operator](#upgrade-ocp-operator)
3. [Use your console to upgrade your running blockchain nodes](#upgrade-ocp-nodes)

After you upgrade the {{site.data.keyword.blockchainfull_notm}} Platform operator, the operator will automatically upgrade the console that is deployed on your OpenShift project. You can then use the upgraded console to upgrade your blockchain nodes.

You need to complete these steps for each network that that runs on a separate project. If you experience any problems, see the instructions for [rolling back an upgrade](#upgrade-ocp-rollback). If you deployed your network behind a firewall, without access to the external internet, see the separate set of instructions for [Upgrading the {{site.data.keyword.blockchainfull_notm}} Platform behind a firewall](#upgrade-ocp-nodes).

You can continue to submit transactions to your network while you are upgrading your network. However, you cannot use the console to deploy new nodes, install or instantiate smart contracts, or create new channels during the upgrade process.

### Roll back an upgrade
{: #upgrade-ocp-rollback}

When you upgrade your operator, the operator saves the secrets, deployment spec, and network information of your console before attempting to upgrade the console. If your upgrade fails for any reason, the {{site.data.keyword.IBM_notm}} Support can roll back your upgrade and restore your previous deployment by using the information on your cluster. If you need to roll back your upgrade, you can submit a support case from the [mysupport](https://www.ibm.com/support/pages/support-ibm-blockchain-platform-v21x){: external} page.

You can roll back an upgrade after you use the console to operate your network. However, after you use the console to upgrade your blockchain nodes, you can no longer roll back your console to a previous version of the platform.

## Before you begin

To upgrade your network, you need to [retrieve your entitlement key](/docs/blockchain-sw?topic=blockchain-sw-deploy-ocp#deploy-ocp-entitlement-key) from the My {{site.data.keyword.IBM_notm}} Dashboard, and [create a Kubernetes secret](/docs/blockchain-sw?topic=blockchain-sw-deploy-ocp#deploy-ocp-docker-registry-secret) to store the key on your OpenShift project. If the Entitlement key secret was removed from your cluster, or if your key is expired, then you need to download another key and create a new secret.

## Step One: Update the ClusterRole
{: #upgrade-ocp-clusterrole}

You need to update the ClusterRole that is applied to your project. Copy the following text to a file on your local system and save the file as `ibp-clusterrole.yaml`. Edit the file and replace `<PROJECT_NAME>` with the name of your project.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  creationTimestamp: null
  name: <PROJECT_NAME>
rules:
- apiGroups:
  - apiextensions.k8s.io
  resources:
  - persistentvolumeclaims
  - persistentvolumes
  - customresourcedefinitions
  verbs:
  - '*'
- apiGroups:
  - "*"
  resources:
  - pods
  - services
  - endpoints
  - persistentvolumeclaims
  - persistentvolumes
  - events
  - configmaps
  - secrets
  - ingresses
  - roles
  - rolebindings
  - serviceaccounts
  - nodes
  - routes
  - routes/custom-host
  verbs:
  - '*'
- apiGroups:
  - ""
  resources:
  - namespaces
  - nodes
  verbs:
  - get
- apiGroups:
  - apps
  resources:
  - deployments
  - daemonsets
  - replicasets
  - statefulsets
  verbs:
  - '*'
- apiGroups:
  - monitoring.coreos.com
  resources:
  - servicemonitors
  verbs:
  - get
  - create
- apiGroups:
  - apps
  resourceNames:
  - ibp-operator
  resources:
  - deployments/finalizers
  verbs:
  - update
- apiGroups:
  - ibp.com
  resources:
  - '*'
  - ibpservices
  - ibpcas
  - ibppeers
  - ibpfabproxies
  - ibporderers
  verbs:
  - '*'
- apiGroups:
  - ibp.com
  resources:
  - '*'
  verbs:
  - '*'
- apiGroups:
  - config.openshift.io
  resources:
  - '*'
  verbs:
  - '*'
```
{:codeblock}

After you save and edit the file, run the following commands. Replace `<PROJECT_NAME>` with your project.
```
oc apply -f ibp-clusterrole.yaml
oc adm policy add-scc-to-group <PROJECT_NAME> system:serviceaccounts:<PROJECT_NAME>
```
{:codeblock}

## Step two: Upgrade the {{site.data.keyword.blockchainfull_notm}} operator
{: #upgrade-ocp-operator}

You can upgrade the {{site.data.keyword.blockchainfull_notm}} operator by fetching the operator deployment spec from your OpenShift project. When the operator upgrade is complete, the operator will upgrade your console and download the latest images for your blockchain nodes.

Log in to your cluster by using the OpenShift CLI. Because each {{site.data.keyword.blockchainfull_notm}} network runs in a different project, you must switch to each OpenShift project and upgrade each network separately. Go to the OpenShift project of the network that you want to upgrade. Replace `<PROJECT_NAME>` with the name of your project.
```
oc project <PROJECT_NAME>
```
{:codeblock}

When you are operating from your project, run the following command to download the operator deployment spec to your local file system:
```
kubectl get deployment ibp-operator -o yaml > operator.yaml
```
{:codeblock}

Open `operator.yaml` in a text editor and save a new copy of the file as `operator-upgrade.yaml`. Open `operator-upgrade.yaml` in a text editor. You need to update the `image:` field with the updated version of the operator image. You can find the name and tag of the latest operator image below:
```
cp.icr.io/cp/ibp-operator:2.1.2-20200213-amd64
```
{:codeblock}

You also need to edit the `env:` section of the file. Find the following lines in `operator-upgrade.yaml`:
```
- name: ISOPENSHIFT
  value: "true"
```
{:codeblock}

Replace this section with the following lines at the same indentation:
```
- name: CLUSTERTYPE
  value: OPENSHIFT
```
{:codeblock}

When you are finished editing the file, the `env:` section would look similar to the following:
```
env:
- name: WATCH_NAMESPACE
  valueFrom:
    fieldRef:
      apiVersion: v1
      fieldPath: metadata.namespace
- name: POD_NAME
  valueFrom:
    fieldRef:
      apiVersion: v1
      fieldPath: metadata.name
- name: OPERATOR_NAME
  value: ibp-operator
- name: CLUSTERTYPE
  value: OPENSHIFT
```
{:codeblock}

If you created a new Kubernetes secret to store your entitlement key, you also need to update the image pull secret in your operator spec:
```
imagePullSecrets:
  - name: <NEW_SECRET>
```
{:codeblock}

Save the file on your local system. You can then issue the following command upgrade your operator:
```
kubectl apply -f operator-upgrade.yaml
```
{:codeblock}

You can use the `kubectl get deployment ibp-operator -o yaml` command to confirm that the command updated the operator spec.

After you apply the `operator-upgrade.yaml` operator spec to your OpenShift project, the operator will restart and pull the latest image. The upgrade takes about a minute. While the upgrade is taking place, you can still access your console UI. However, you cannot use the console to install and instantiate chaincode, or use the console or the APIs to create or remove a node.

You can check that the upgrade is complete by running `kubectl get deployment`. If the upgrade is successful, then you can see the following tables with four ones displayed for your operator and your console.
```
NAME           READY     UP-TO-DATE   AVAILABLE   AGE
ibp-operator   1/1       1            1           1m
ibpconsole     1/1       1            1           4m
```

If you experience a problem while you are upgrading the operator, go to this [troubleshooting topic](/docs/blockchain-sw?topic=blockchain-sw-ibp-v2-troubleshooting#ibp-v2-troubleshooting-deployment-cr) for a list of commonly encountered problems. You can run the command to apply the original operator file, `kubectl apply -f operator.yaml` to restore your original operator deployment.

## Step three: Upgrade your blockchain nodes
{: #upgrade-ocp-nodes}

After you upgrade your console, you can use the console UI to upgrade the nodes of your blockchain network. Browse to the console UI open the nodes overview tab. You can find the **Patch available** text on a node tile if there is an update available for the component. You can install this patch whenever you are ready. These patches are optional, but they are recommended. You cannot patch nodes that were imported into the console.

Apply patches to nodes one at a time. Your nodes are unavailable to process requests or transactions while the patch is being applied. Therefore, to avoid any disruption of service, you need to ensure that another node of the same type is available to process requests whenever possible. Installing patches on a node takes about a minute to complete and when the update is complete, the node is ready to process requests.
{:important}

To apply a patch to a node, open the node tile and click the **Install patch** button. You cannot patch nodes that you imported to the console.

## Upgrading the {{site.data.keyword.blockchainfull_notm}} Platform behind a firewall
{: #upgrade-ocp-firewall}

If you deployed the {{site.data.keyword.blockchainfull_notm}} Platform behind a firewall, without access to the external internet, you can upgrade your network by using the following steps:

1. [Pull the latest {{site.data.keyword.blockchainfull_notm}} Platform images](#upgrade-ocp-images-firewall)
2. [Update the ClusterRole](#upgrade-ocp-clusterrole-firewall)
3. [Upgrade the {{site.data.keyword.blockchainfull_notm}} Platform operator](#upgrade-ocp-operator-firewall)
4. [Use your console to upgrade your running blockchain nodes](#upgrade-ocp-nodes-firewall)

You can continue to submit transactions to your network while you are upgrading your network. However, you cannot use the console to deploy new nodes, install or instantiate smart contracts, or create new channels during the upgrade process.

### Before you begin
{: #upgrade-ocp-begin-firewall}

To upgrade your network, you need to [retrieve your entitlement key](/docs/blockchain-sw?topic=blockchain-sw-deploy-ocp-firewall#deploy-ocp-entitlement-key-firewall) from the My {{site.data.keyword.IBM_notm}} Dashboard, and [create a Kubernetes secret](/docs/blockchain-sw?topic=blockchain-sw-deploy-ocp#deploy-ocp-docker-registry-secret) to store the key on your OpenShift project. If the Entitlement key secret was removed from your cluster, or if your key is expired, then you need to download another key and create a new secret.

### Step one: Pull the latest {{site.data.keyword.blockchainfull_notm}} Platform images
{: #upgrade-ocp-images-firewall}

To upgrade your network, download the latest set of {{site.data.keyword.blockchainfull_notm}} Platform images and push them to a docker registry that you can access from behind your firewall.

Use the following command to log in to the {{site.data.keyword.IBM_notm}} Entitlement Registry:
```
docker login --username cp --password <KEY> cp.icr.io
```
{:codeblock}

- Replace `<KEY>` with your entitlement key.

After you log in, use the following command to pull the images for {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2:
```
docker pull cp.icr.io/cp/ibp-operator:2.1.2-20200213-amd64
docker pull cp.icr.io/cp/ibp-init:2.1.2-20200213-amd64
docker pull cp.icr.io/cp/ibp-peer:1.4.4-20191217-amd64
docker pull cp.icr.io/cp/ibp-orderer:1.4.4-20191217-amd64
docker pull cp.icr.io/cp/ibp-ca:1.4.4-20191217-amd64
docker pull cp.icr.io/cp/ibp-dind:1.4.4-20191217-amd64
docker pull cp.icr.io/cp/ibp-console:2.1.2-20200213-amd64
docker pull cp.icr.io/cp/ibp-grpcweb:2.1.2-20200213-amd64
docker pull cp.icr.io/cp/ibp-utilities:1.4.4-20191217-amd64
docker pull cp.icr.io/cp/ibp-couchdb:2.3.1-20191217-amd64
docker pull cp.icr.io/cp/ibp-deployer:2.1.2-20200213-amd64
docker pull cp.icr.io/cp/ibp-fluentd:2.1.2-20200213-amd64
```
{:codeblock}

After you download the images, you must change the image tags to refer to your docker registry. Replace `<LOCAL_REGISTRY>` with the url of your local registry and run the following commands:
```
docker tag cp.icr.io/cp/ibp-operator:2.1.2-20200213-amd64 <LOCAL_REGISTRY>/ibp-operator:2.1.2-20200213-amd64
docker tag cp.icr.io/cp/ibp-init:2.1.2-20200213-amd64 <LOCAL_REGISTRY>/ibp-init:2.1.2-20200213-amd64
docker tag cp.icr.io/cp/ibp-peer:1.4.4-20191217-amd64 <LOCAL_REGISTRY>/ibp-peer:1.4.4-20191217-amd64
docker tag cp.icr.io/cp/ibp-orderer:1.4.4-20191217-amd64 <LOCAL_REGISTRY>/ibp-orderer:1.4.4-20191217-amd64
docker tag cp.icr.io/cp/ibp-ca:1.4.4-20191217-amd64 <LOCAL_REGISTRY>/ibp-ca:1.4.4-20191217-amd64
docker tag cp.icr.io/cp/ibp-dind:1.4.4-20191217-amd64 <LOCAL_REGISTRY>/ibp-dind:1.4.4-20191217-amd64
docker tag cp.icr.io/cp/ibp-console:2.1.2-20200213-amd64 <LOCAL_REGISTRY>/ibp-console:2.1.2-20200213-amd64
docker tag cp.icr.io/cp/ibp-grpcweb:2.1.2-20200213-amd64 <LOCAL_REGISTRY>/ibp-grpcweb:2.1.2-20200213-amd64
docker tag cp.icr.io/cp/ibp-utilities:1.4.4-20191217-amd64 <LOCAL_REGISTRY>/ibp-utilities:1.4.4-20191217-amd64
docker tag cp.icr.io/cp/ibp-couchdb:2.3.1-20191217-amd64 <LOCAL_REGISTRY>/ibp-couchdb:2.3.1-20191217-amd64
docker tag cp.icr.io/cp/ibp-deployer:2.1.2-20200213-amd64 <LOCAL_REGISTRY>/ibp-deployer:2.1.2-20200213-amd64
docker tag cp.icr.io/cp/ibp-fluentd:2.1.2-20200213-amd64 <LOCAL_REGISTRY>/ibp-fluentd:2.1.2-20200213-amd64
```
{:codeblock}

You can use the `docker images` command to check that the new tags were added. You can then push the images with the new tags to your docker registry. Log in to your registry by using the following command:
```
docker login --username <USER> --password <LOCAL_REGISTRY_PASSWORD> <LOCAL_REGISTRY>
```
{:codeblock}

- Replace `<USER>` with your username
- Replace `<LOCAL_REGISTRY_PASSWORD>` with the password to your registry.
- Replace `<LOCAL_REGISTRY>` with the url of your local registry.

Then, run the following command to push the images. Replace `<LOCAL_REGISTRY>` with the url of your local registry.
```
docker push <LOCAL_REGISTRY>/ibp-operator:2.1.2-20200213-amd64
docker push <LOCAL_REGISTRY>/ibp-init:2.1.2-20200213-amd64
docker push <LOCAL_REGISTRY>/ibp-peer:1.4.4-20191217-amd64
docker push <LOCAL_REGISTRY>/ibp-orderer:1.4.4-20191217-amd64
docker push <LOCAL_REGISTRY>/ibp-ca:1.4.4-20191217-amd64
docker push <LOCAL_REGISTRY>/ibp-dind:1.4.4-20191217-amd64
docker push <LOCAL_REGISTRY>/ibp-console:2.1.2-20200213-amd64
docker push <LOCAL_REGISTRY>/ibp-grpcweb:2.1.2-20200213-amd64
docker push <LOCAL_REGISTRY>/ibp-utilities:1.4.4-20191217-amd64
docker push <LOCAL_REGISTRY>/ibp-couchdb:2.3.1-20191217-amd64
docker push <LOCAL_REGISTRY>/ibp-deployer:2.1.2-20200213-amd64
docker push <LOCAL_REGISTRY>/ibp-fluentd:2.1.2-20200213-amd64
```
{:codeblock}

After you complete these steps, you can use the following instructions to deploy the {{site.data.keyword.blockchainfull_notm}} Platform with the images in your registry.

### Step two: Update the ClusterRole
{: #upgrade-ocp-clusterrole-firewall}

You need to update the ClusterRole that is applied to your project. Copy the following text to a file on your local system and save the file as `ibp-clusterrole.yaml`. Edit the file and replace `<PROJECT_NAME>` with the name of your project.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  creationTimestamp: null
  name: <PROJECT_NAME>
rules:
- apiGroups:
  - apiextensions.k8s.io
  resources:
  - persistentvolumeclaims
  - persistentvolumes
  - customresourcedefinitions
  verbs:
  - '*'
- apiGroups:
  - "*"
  resources:
  - pods
  - services
  - endpoints
  - persistentvolumeclaims
  - persistentvolumes
  - events
  - configmaps
  - secrets
  - ingresses
  - roles
  - rolebindings
  - serviceaccounts
  - nodes
  - routes
  - routes/custom-host
  verbs:
  - '*'
- apiGroups:
  - ""
  resources:
  - namespaces
  - nodes
  verbs:
  - get
- apiGroups:
  - apps
  resources:
  - deployments
  - daemonsets
  - replicasets
  - statefulsets
  verbs:
  - '*'
- apiGroups:
  - monitoring.coreos.com
  resources:
  - servicemonitors
  verbs:
  - get
  - create
- apiGroups:
  - apps
  resourceNames:
  - ibp-operator
  resources:
  - deployments/finalizers
  verbs:
  - update
- apiGroups:
  - ibp.com
  resources:
  - '*'
  - ibpservices
  - ibpcas
  - ibppeers
  - ibpfabproxies
  - ibporderers
  verbs:
  - '*'
- apiGroups:
  - ibp.com
  resources:
  - '*'
  verbs:
  - '*'
- apiGroups:
  - config.openshift.io
  resources:
  - '*'
  verbs:
  - '*'
```
{:codeblock}

After you save and edit the file, run the following commands. Replace `<PROJECT_NAME>` with your project.
```
oc apply -f ibp-clusterrole.yaml
oc adm policy add-scc-to-group <PROJECT_NAME> system:serviceaccounts:<PROJECT_NAME>
```

### Step three: Upgrade the {{site.data.keyword.blockchainfull_notm}} operator
{: #upgrade-ocp-operator-firewall}

You can upgrade the {{site.data.keyword.blockchainfull_notm}} operator by fetching the operator deployment spec from your OpenShift project. You can then update the spec with the latest operator image that you pushed to your local registry. When the operator upgrade is complete, the operator will download the images that you pushed to your local registry and upgrade your console.

Log in to your cluster by using the OpenShift CLI. Because each {{site.data.keyword.blockchainfull_notm}} network runs in a different project, you must switch to each OpenShift project and upgrade each network separately. Go to the OpenShift project of the network that you want to upgrade. Replace `<PROJECT_NAME>` with the name of your project.
```
oc project <PROJECT_NAME>
```
{:codeblock}

When you are operating from your project, run the following command to download the operator deployment spec to your local file system:
```
kubectl get deployment ibp-operator -o yaml > operator.yaml
```
{:codeblock}

Open `operator.yaml` in a text editor and save a new copy of the file as `operator-upgrade.yaml`. Open `operator-upgrade.yaml` a text editor. You need to update the `image:` field with the updated version of the operator image:
```
<LOCAL_REGISTRY>/ibp-operator:2.1.2-20200213-amd64
```
{:codeblock}

You also need to edit the `env:` section of the file. Find the following lines in `operator-upgrade.yaml`:
```
- name: ISOPENSHIFT
  value: "true"
```
{:codeblock}

Replace the values above with the following lines at the same indentation:
```
- name: CLUSTERTYPE
  value: OPENSHIFT
```

When you are finished editing the file, the `env:` section would look similar to the following:
```
env:
- name: WATCH_NAMESPACE
  valueFrom:
    fieldRef:
      apiVersion: v1
      fieldPath: metadata.namespace
- name: POD_NAME
  valueFrom:
    fieldRef:
      apiVersion: v1
      fieldPath: metadata.name
- name: OPERATOR_NAME
  value: ibp-operator
- name: CLUSTERTYPE
  value: OPENSHIFT
```

If you created a new Kubernetes secret to store your entitlement key, you also need to update the image pull secret in your console spec:
```
imagePullSecrets:
  - name: <NEW_SECRET>
```
{:codeblock}

Save the file on your local system. You can then issue the following command upgrade your operator:
```
kubectl apply -f operator-upgrade.yaml
```
{:codeblock}

You can use the `kubectl get deployment ibp-operator -o yaml` command to confirm that the command updated the operator spec.

After you apply the `operator-upgrade.yaml` operator spec to your OpenShift project, the operator will restart and pull the latest image. The upgrade takes about a minute. While the upgrade is taking place, you can still access your console UI. However, you cannot use the console to install and instantiate chaincode, or use the console or the APIs to create or remove a node.

You can check that the upgrade is complete by running `kubectl get deployment`. If the upgrade is successful, then you can see the following tables with four ones displayed for your operator and your console.
```
NAME           READY     UP-TO-DATE   AVAILABLE   AGE
ibp-operator   1/1       1            1           1m
ibpconsole     1/1       1            1           4m
```

If you experience a problem while you are upgrading the operator, go to this [troubleshooting topic](/docs/blockchain-sw?topic=blockchain-sw-ibp-v2-troubleshooting#ibp-v2-troubleshooting-deployment-cr) for a list of commonly encountered problems.

If your console experiences an image pull error, you may need to update the console deployment spec with the local registry that you used to download the images.
```
NAME                          READY     STATUS              RESTARTS   AGE
ibp-operator-b9446759-6tmls   1/1       Running             0          1m
ibpconsole-57ff4bcbb7-79dhn   0/4       Init:ErrImagePull   0          1m
```
This can happen if you have changed your regsitry URL between deployments. Run the following command to download the deployment spec of the console:
```
kubectl get ibpconsole ibpconsole -o yaml > console.yaml
```
{:codeblock}

Then add the URL of your local registry to the `spec:` section of `console.yaml`. Replace `<LOCAL_REGISTRY>` with the url of your local registry:
```
spec:
  registryURL: <LOCAL_REGISTRY>
```
{:codeblock}

Save the updated file as `console-upgrade.yaml` on your local system. You can then issue the following command upgrade your console:
```
kubectl apply -f console-upgrade.yaml
```
{:codeblock}

### Step four: Upgrade your blockchain nodes
{: #upgrade-ocp-nodes-firewall}

After you upgrade your console, you can use the console UI to upgrade the nodes of your blockchain network. For more information, see [Upgrade your blockchain nodes](#upgrade-ocp-nodes).
