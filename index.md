---

copyright:
  years: 2019, 2020
lastupdated: "2020-03-23"

keywords: Kubernetes, IBM Cloud Private, OCP, OpenShift Container Platform, IBM Blockchain Platform

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

# Getting started with {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2
{: #get-started-console-ocp}

<div style="background-color: #f4f4f4; padding-left: 20px; border-bottom: 2px solid #0f62fe; padding-top: 12px; padding-bottom: 4px; margin-bottom: 16px; font-family: 'IBM Plex Sans', 'Helvetica Neue', Arial, sans-serif;">
  <p style="line-height: 10px;">
    <strong>Running a different version of IBM Blockchain Platform?</strong> Switch to version
    <a href="https://test.cloud.ibm.com/docs/blockchain-sw-213?topic=blockchain-sw-213-get-started-console-ocp">2.1.3</a>
    </p>
</div>

{{site.data.keyword.blockchainfull}} Platform provides a managed and full stack blockchain-as-a-service (BaaS) offering that allows you to deploy blockchain components on any Kubernetes v1.14 - v1.16 container platform on x86_64.  This offering is ideal for the customers who want to deploy their components, store their data, or run their workloads on their own infrastructure or across public and private clouds for security, risk mitigation, preference, or compliance reasons. Clients can build, operate, and grow their blockchain networks with an offering that can be used from development through production.
{:shortdesc}

Your entitlement includes the flexible management console for deploying and managing your blockchain network. Use the console or {{site.data.keyword.blockchainfull_notm}} Platform APIs to build a consortium of organizations to easily transact on the same network, regardless of each client's cloud preference. By installing the {{site.data.keyword.blockchainfull_notm}} Platform console, users can create components in their clusters and connect them to components deployed on other clusters, forming a distributed, multi-organizational blockchain network.

Experienced Hyperledger Fabric customers who prefer to deploy and manage their containers can download and use the peer, CA, orderer, and smart contract container images without the management console.

For more information on where you can run {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2, see [Supported Platforms](/docs/blockchain-sw?topic=blockchain-sw-console-ocp-about#console-ocp-about-prerequisites).

The **{{site.data.keyword.blockchainfull_notm}} Platform v2.1.2** is purchased as a stand-alone entitlement. After purchase, you can access the [My IBM dashboard](https://myibm.ibm.com/dashboard/){: external} to obtain your entitlement key for the offering. This key is required to deploy the release or download the container images.

With this offering, you are responsible for purchasing and provisioning your own Kubernetes cluster.
{: important}

## Already have an {{site.data.keyword.blockchainfull_notm}} Platform v2.1.x network and want to upgrade?
{: #get-started-console-ocp-upgrade}

Check out the following topics for instructions on how to upgrade:
- [Upgrading your console and components on the OpenShift Container Platform](/docs/blockchain-sw?topic=blockchain-sw-upgrade-ocp)
- [Upgrading your console and components on Kubernetes or {{site.data.keyword.cloud_notm}} Private 3.2.1](/docs/blockchain-sw?topic=blockchain-sw-upgrade-k8)
- [Upgrading the {{site.data.keyword.blockchainfull_notm}} images](/docs/blockchain-sw?topic=blockchain-sw-blockchain-images#blockchain-images-upgrade)

## Is {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 suitable for you?
{: #get-started-console-ocp-suitable}

{{site.data.keyword.blockchainfull_notm}} Platform provides different offerings that allow you to deploy your network in the environment of your choice. You need to decide if you want to deploy the {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 or if you want to use the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}.

| |{{site.data.keyword.blockchainfull_notm}} Platform for anywhere (v2.1.2) | {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} |
|----|---|----|
| Where do you want to deploy the platform?|  Multiple Kubernetes distributions on a private, public or hybrid multicloud | An {{site.data.keyword.IBM_notm}} Kubernetes Service cluster on {{site.data.keyword.cloud_notm}} |  
| What are my deployment options? | <ul><li> Full platform </li> <li> Only {{site.data.keyword.blockchainfull_notm}} images ** </li> </ul>| <ul><li> Full platform </li> </ul>
| How is it billed? |Contact us for [pricing](/docs/blockchain-sw?topic=blockchain-sw-ibp-sw-pricing) |[$0.29 USD per allocated CPU hour](/docs/blockchain?topic=blockchain-ibp-saas-pricing)  |
| Can I connect with Peers in other clouds? |  Yes| Yes |
| Can my data center be on-premises and behind a firewall? | Yes| No |
| Can I use a console UI to deploy and manage my blockchain components? | Yes | Yes |
| Are APIs available for node management? | Yes | Yes |
| Does the product integrate with the {{site.data.keyword.blockchainfull_notm}} Platform VSCode extension to develop and test my smart contracts?| Yes | Yes |
| I am an experienced Fabric customer. Are peer, CA, orderer, and smart contract images available and supported? | Yes | No |
| Where can I learn more? |See [About {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2](/docs/blockchain-sw?topic=blockchain-sw-console-ocp-about#console-ocp-about-offers)  | See [About {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/blockchain?topic=blockchain-ibp-console-overview#ibp-console-overview-capabilities) |
{: caption="Table 1. Which offering is right for your business?" caption-side="bottom"}
** {{site.data.keyword.blockchainfull_notm}} images - Your purchase of {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 includes an entitlement to images for peer, CA, ordering service, and smart contract containers that are signed by IBM. The images are based on the open source Hyperledger Fabric code base and contain a number of enhancements for stability and serviceability. The images are bundled with support from IBM. The {{site.data.keyword.blockchainfull_notm}}  images do not include the {{site.data.keyword.blockchainfull_notm}} management console or operator. This offering is intended for experienced Fabric users with existing Fabric deployments. For more information, see [Using the IBM Blockchain images](/docs/blockchain-sw?topic=blockchain-sw-blockchain-images).

### Developer Tools
{: ##get-started-console-ocp-dev-tools}

Are you a developer? Check out the [**{{site.data.keyword.blockchainfull_notm}} Platform Extension for VS Code**](/docs/blockchain-sw?topic=blockchain-sw-develop-vscode#develop-vscode), a free IDE that provides an explorer and commands accessible from the command palette for developing smart contracts quickly.

## Before you begin
{: #get-started-console-ocp-set-up-ocp}

1. Review the [Supported Platforms](/docs/blockchain-sw?topic=blockchain-sw-console-ocp-about#console-ocp-about-prerequisites).

2. Install a Kubernetes cluster and log in to your cluster. If you are using OpenShift, see the [OpenShift Container Platform CLI](https://docs.openshift.com/container-platform/4.2/cli_reference/openshift_cli/getting-started-cli.html){: external} to deploy the CLI. If you are using an OpenShift cluster that was deployed with the {{site.data.keyword.IBM_notm}} Kubernetes Service, use these instructions to [Install the OpenShift Origin CLI](/docs/openshift?topic=openshift-openshift-cli#cli_oc).

3. If you are _not_ running the platform on Red Hat OpenShift Container Platform, Red Hat Open Kubernetes Distribution, or {{site.data.keyword.cloud_notm}} Private, you need to setup the nginx ingress controller and it needs to be running in [SSL passthrough mode](https://kubernetes.github.io/ingress-nginx/user-guide/tls/#ssl-passthrough){: external}.

4. Get the entitlement key from your MyIBM account in order to install the platform. For more information, see [Get your entitlement key](/docs/blockchain-sw?topic=blockchain-sw-deploy-ocp#deploy-ocp-entitlement-key).

## Step one: Install the {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-console-ocp-step-two-deploy-console}

The {{site.data.keyword.blockchainfull_notm}} Platform uses a [Kubernetes Operator](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/){: external} to install the {{site.data.keyword.blockchainfull_notm}} Platform console on your cluster and manage the deployment and your blockchain nodes. A cluster administrator can use your entitlement key to deploy the platform on any Kubernetes Cluster

-  If you are deploying the {{site.data.keyword.blockchainfull_notm}} Platform on the OpenShift Container Platform, you can deploy the platform by using the steps in the [Deploying {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 on the OpenShift Container Platform](/docs/blockchain-sw/howto?topic=blockchain-sw-deploy-ocp#deploy-ocp).

- If you are deploying the {{site.data.keyword.blockchainfull_notm}} Platform on another Kubernetes distribution, such as Rancher or {{site.data.keyword.cloud_notm}} Private, use the steps described in the [Deploying {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 on Kubernetes](/docs/blockchain-sw/howto?topic=blockchain-sw-deploy-k8#deploy-k8).

- If you are an experienced Hyperledger Fabric customer and prefer to only use the {{site.data.keyword.blockchainfull_notm}} images, see [Using the {{site.data.keyword.blockchainfull_notm}} images](/docs/blockchain-sw?topic=blockchain-sw-blockchain-images). Consult the Fabric documentation for further deployment, configuration, and management instructions. The rest of the steps in this topic do not apply when using the images.

## Step two: Grant console access to other users
{: #get-started-console-ocp-step-four-add-console-admin}

After the platform is deployed, the console administrator can log in to the console with the email address and password that was provided during console deployment. The password that was provided becomes the default password of the console, and is used by all new users to log in to the console for the first time. The administrator can then add new users to the console, allowing others to log in and start working with {{site.data.keyword.blockchainfull_notm}} nodes. The administrator can also set a new default password. To learn more, see [Managing users from the console](/docs/blockchain-sw?topic=blockchain-sw-console-icp-manage#console-icp-manage-users).

## Step three: Use the console to create your components
{: #get-started-console-ocp-build-network}

After you deploy the console, you can use it to create, operate, and govern {{site.data.keyword.blockchainfull_notm}} components on your Kubernetes cluster. To get started with building a blockchain network by using the management console, go to the [Build a network tutorial](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-build-network#ibp-console-build-network).

## Step four: Connect networks across clouds
{: #get-started-console-ocp-import-nodes}

You can use your console to operate components that are running on other clusters. First, you need to export the component information to a JSON file from the console where the component was originally deployed. Then, you can import the node JSON file into the console that is deployed on your local cluster and manage the components across clouds. For more information, see [Importing nodes](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-import-nodes#ibp-console-import-nodes).
