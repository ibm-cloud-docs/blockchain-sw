---

copyright:
  years: 2019, 2020
lastupdated: "2020-11-02"

keywords: IBM Blockchain Platform, system requirements, Kubernetes, behind a firewall

subcollection: blockchain-sw

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: #swift .ph data-hd-programlang='swift'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vb.net: .ph data-hd-programlang='vb.net'}
{:video: .video}

# About {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2
{: #console-ocp-about}

<div style="background-color: #6fdc8c; padding-left: 20px; padding-right: 20px; border-bottom: 4px solid #0f62fe; padding-top: 12px; padding-bottom: 4px; margin-bottom: 16px;">
  <p style="line-height: 20px;">
    <strong>Important: You are not looking at the latest product documentation.  Make sure you are reading the documentation that matches the version of the software that you are using. Switch to product version </strong>
    <a href="/docs/blockchain-sw-213?topic=blockchain-sw-213-console-ocp-about">2.1.3</a>,
    <a href="/docs/blockchain-sw-25?topic=blockchain-sw-25-console-ocp-about">2.5 </a>,
    <a href="/docs/blockchain-sw-251?topic=blockchain-sw-251-console-ocp-about">2.5.1 (latest)</a>
    </p>
</div>

The {{site.data.keyword.blockchainfull}} Platform v2.1.2 enables a consortium of organizations to easily build and join a blockchain network on-premises, or on any private, public, or hybrid multicloud using Kubernetes. Customers can deploy their nodes on the cloud platform of their choice and connect to any {{site.data.keyword.blockchainfull_notm}} Platform network, whether it is deployed on your own Kubernetes cluster or with the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}. The {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 utilizes Hyperledger Fabric v1.4.4 and supports deployment on multiple Kubernetes distributions.
{:shortdesc}

Watch the following video for an introduction to blockchain and the {{site.data.keyword.blockchainfull}} Platform:

![Key concepts video](https://www.youtube.com/embed/AvQa1W38J4I){: video output="iframe" data-script="#video-transcript-key-concepts" id="youtubeplayer" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen}

## Video script
{: #video-transcript-key-concepts}
{: notoc}

By now you’ve probably heard of the IBM Blockchain Platform, the leading permissioned enterprise blockchain solution in the world. But what is a permissioned blockchain? And what is the IBM Blockchain Platform? The modern world is interwoven and interactive place.

But under the surface it’s still following some pretty old rules.

Jerry here on the left uses a different record keeping system than Door2Door Logistics on the right, which means they have to spend a lot of time figuring out what the truth is before they can make a deal.

This process is not just slow, it’s vulnerable. A successful hack or other problem can mean records are lost forever. As a result, businesses sacrifice efficiency for security and lock their records away.

But what if businesses shared their records, and shared the burden of protecting them? What if Jerry’s Modern Fabrics and Door2Door Logistics and their business partners never had to spend time arguing over who’s right because every time an asset moves from one to the other everyone’s records updated at the same time? And what if those records, once written, could never been changed?

This network, leveraging what’s called Distributed Ledger Technology, already exists. It’s an open blockchain network like Bitcoin. But there’s a problem. Businesses don’t necessarily want the records of their transactions shared with everyone, especially in a network like Bitcoin where users are unknown. In some industries, it’s actually illegal to share data that way.

What Jerry’s Modern Fabrics and Door2Door Logistics need is a permissioned blockchain like IBM Blockchain Platform, where businesses can form networks with known, established partners and still take advantage of the robustness and efficiency of blockchains. But this too creates a problem. Who owns this network? Who runs it? The answer is: no one does.

Once an IBM Blockchain network has been established, its rules and practices are managed collectively, mimicking the kind of consensus process that governs the way transactions themselves are approved and written to the ledgers in the network.

But the IBM Blockchain Platform doesn’t just stop with permissions and identities, users also have the ability to create channels where a few members of a network can get together and transact privately.

Additionally, private data collections can be established, which allows a few channel members to share certain transactions just with each other, without needing a whole separate channel.

Because components are hosted in clusters that are owned and controlled by users, the IBM Blockchain Platform is naturally compliant with data residency rules.

All of these processes are managed through an award winning UI we call the console, which, along with custom APIs, makes the powerful open-source Hyperledger Fabric blockchain painless to use.

The console integrates seamlessly with the rest of the IBM Blockchain Platform suite, including a powerful VS Code extension which allows users to create and test smart contracts and applications and then package and install them on production networks.

Because the console can run on both IBM Cloud and any cloud supported by Red Hat’s Open Shift, the console can run nearly everywhere and consoles in different clouds can connect to each other and to nodes deployed on Hyperledger Fabric.

But how many CAs, which create identities and define organizations, do I need?

How many peers, which host ledgers and have smart contracts installed on them, should I deploy on a channel to make sure I have no downtime?

Because you only pay for the compute you use, it’s painless to transition from pilot programs to full production networks using the IBM Blockchain Platform. The IBM Garage is here to help assist you in finding the right configuration for every use case.

The world is moving too fast to keep doing things the old way. Go to cloud dot IBM dot com today and check out the IBM blockchain platform.

## What {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 offers
{: #console-ocp-about-offers}

The {{site.data.keyword.blockchainfull_notm}} Platform provides a flexible management platform that runs on Kubernetes. The offering includes an award-winning management console that allows you to easily deploy blockchain components, build a multicloud blockchain network, and perform network management and maintenance.

This offering includes two deployment options:  

**Full platform**

Includes the operator, management console, peer, CA, orderer, and smart contract container images. The {{site.data.keyword.blockchainfull_notm}} Platform management console can be used to create all of the fundamental components of a Hyperledger Fabric network: a Certificate Authority (CA), an ordering service, and peers, on your local cluster. You can also use your console to operate a distributed multicloud network by importing nodes deployed by other consoles. For more information about the building blocks of Hyperledger Fabric networks, see the [blockchain component overview](/docs/blockchain-sw?topic=blockchain-sw-blockchain-component-overview).

**{{site.data.keyword.blockchainfull_notm}} images**  

For experienced Hyperledger Fabric customers, a purchase of {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 includes an entitlement to the peer, CA, orderer, and smart contract container images that are signed and supported by {{site.data.keyword.IBM_notm}}. These images are based on the open source Hyperledger Fabric code base and contain a number of enhancements for stability and serviceability. The images are bundled with support from {{site.data.keyword.IBM_notm}}. The {{site.data.keyword.blockchainfull_notm}} Platform management console or operator are not among the images included in this entitlement. For more information, see [{{site.data.keyword.blockchainfull_notm}} images for Hyperledger Fabric](/docs/blockchain-sw?topic=blockchain-sw-blockchain-images#blockchain-images).

The {{site.data.keyword.blockchainfull_notm}} Platform includes the following key features:

**BUILD ---- Integrated developer experience**
- **Easily code** your smart contracts in Node.js, Golang, or JavaScript, write client applications using the new {{site.data.keyword.blockchainfull_notm}} VS Code extension, leverage **SDK integration** with the user interface console, and learn from our rich tutorials and samples.
- **Simplified DevOps** allows you to move from development to test to production in a single environment by scaling up your Kubernetes resources to add more components.
- **Kubernetes service integration.** Leverage services such as Grafana and Prometheus for logging and Kibana for monitoring.
- **Up-to-date Fabric key features**. Leverage the latest features of Hyperledger Fabric v1.4.4:
  - [Raft ordering service](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#raft){: external}
  - [Private data collections](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data) that provide increased data privacy by ensuring that ledger data is shared to only authorized peers via the gossip protocol.
  - [Service discovery](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external}, allowing you to dynamically discover and update how your application interacts with your network.
  - [Channel access control lists](https://hyperledger-fabric.readthedocs.io/en/release-1.4/access_control.html){: external} that allow you additional control the governance of your channels and smart contracts.

**OPERATE --- Total control of your deployments**
- **Deploy only the components you need**. Connect a peer to multiple channels and networks, or host an ordering service that business partners can connect to.
- **Maintain complete control of your identities**. Store and manage the keys that are used to administer your nodes in your own secure environment.
- **Unified operation**. The {{site.data.keyword.blockchainfull_notm}} Platform console allows you to deploy and manage all of your organizations and nodes in **one console** without having to rely on {{site.data.keyword.IBM_notm}} or other vendors to manage your ordering nodes or Certificate Authority. You can also add or remove members from a blockchain consortium, create and join channels, and install and instantiate smart contracts from your console.
- **Host or join a network**. Deploy peers that are hosted in your cluster to multiple channels on multiple clouds, or invite other organizations to join your consortium or channels while the organizations manage their nodes independently across infrastructures.
- **Manage access** of the users who can administer or monitor your nodes.
- **Direct access to the logs** of your nodes from your Kubernetes service. Use any supported third-party service to extract and analyze your logs.
- **Interact directly with your pods** using your Kubernetes service.
- **Dynamic signature collection** that allows better control over collaborative governance over channel configurations.

**GROW --- Scalability and flexibility**
- **Choose your compute**. You have the flexibility to decide the amount of CPU, memory, and storage you want to provision in your Kubernetes cluster. For more information, see [How the console interacts with your Kubernetes cluster](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-govern-components#ibp-console-govern-components-iks-console-interaction).
- **Scale** up and down the resources in your Kubernetes cluster, paying for only what you need. For more information, see [Pricing](/docs/blockchain-sw?topic=blockchain-sw-ibp-sw-pricing).
- **Disaster recovery and multizone high availability (HA).** This ability duplicates your node deployment across zones, enabling zero down time of your components and disaster recovery (DR).
- **Run Anywhere**. Thanks to the **unified codebase** of the {{site.data.keyword.blockchainfull_notm}} Platform console, it is possible to run your components on any Kubernetes v1.14 - v1.16 container platform on x86_64.
- **Connect to other Fabric networks**: Join {{site.data.keyword.blockchainfull_notm}} Platform peers to any network running Hyperledger Fabric components. Similarly, you can invite Fabric peers to join channels hosted on an ordering service deployed on the {{site.data.keyword.blockchainfull_notm}} Platform. Note that you will need to use Hyperledger Fabric APIs or the CLI.  

Have questions and want to speak to an {{site.data.keyword.blockchainfull_notm}} Platform expert? [Schedule a consult](https://www.ibm.com/cloud/blockchain-platform/developer?schedulerform){: external} now to learn more about how blockchain can transform your business.

## Supported Platforms
{: #console-ocp-about-prerequisites}

The {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 can be deployed using the following Kubernetes distributions on the following Platforms:

| Kubernetes distribution | Supported hardware Platforms | Tested Platforms |
|----|----|----|
| **Red Hat OpenShift Container Platform (OCP) 3.11, 4.1, and 4.2 and OpenShift Kubernetes Distribution (OKD) 3.11:** |Any x86_64 hardware running:  <ul><li>OCP 3.11 that meet the [system and environment requirements](https://docs.openshift.com/container-platform/3.11/install/prerequisites.html){: external}. </li> <li> [OCP 4.1 available platforms](https://docs.openshift.com/container-platform/4.1/architecture/architecture-installation.html#available-platforms_architecture-installation){: external} </li> <li>[OCP 4.2 available platforms](https://docs.openshift.com/container-platform/4.2/architecture/architecture-installation.html#available-platforms_architecture-installation){: external} </li></ul> | <ul> <li> Red Hat OpenShift 3.11 Cluster on {{site.data.keyword.cloud_notm}} </li> <li> Red Hat OpenShift 3.11 Cluster on Microsoft Azure <li> Red Hat OpenShift 4.1  <br><br> See these articles on [OpenShift Container Platform 3.x Tested Integrations](https://access.redhat.com/articles/2176281){: external} or [OpenShift Container Platform 4.x Tested Integrations](https://access.redhat.com/articles/4128421){: external} (Red Hat subscription required to view content). Scroll down to the bottom of the article to the section titled 'Tested Platforms' to view the list of cloud providers tested on OpenShift Container Platform 3.11, 4.1, or 4.2. </li></ul>
| **{{site.data.keyword.cloud_notm}} Private 3.2.1** | <ul><li>x86_64</li><li>LinuxONE on s390x</li></ul>| <ul><li>Intel (x86)</li><li>LinuxONE (s390x)</ul>|
| **Kubernetes v1.14 - v1.16** | <ul><li>x86_64 </li></ul> | <ul><li>Kubernetes</li> <br>The platform has also been tested on Rancher v2.3.2 and Azure Kubernetes Service (AKS).<br><br>**Note:** This offering is not supported on the {{site.data.keyword.cloud_notm}} Kubernetes Service. If you want to run the {{site.data.keyword.blockchainfull_notm}} Platform on the {{site.data.keyword.cloud_notm}} Kubernetes Service, use the [IBM Blockchain Platform for IBM Cloud offering](/docs/blockchain?topic=blockchain-ibp-v2-deploy-iks). </ul>|
{: caption="Table 1. Supported Platforms" caption-side="bottom"}


If you are not running the platform on Red Hat OpenShift Container Platform, Red Hat Open Kubernetes Distribution, or {{site.data.keyword.cloud_notm}} Private then you need to setup the nginx ingress controller and it needs to be running in [SSL passthrough mode](https://kubernetes.github.io/ingress-nginx/user-guide/tls/#ssl-passthrough){: external}.
{: important}

## License and pricing
{: #console-ocp-about-license}

Your {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 entitlement includes both the full platform and the {{site.data.keyword.blockchainfull_notm}} images.

The entitlement does not include a Kubernetes distribution. You must procure that separately. And if you plan to use {{site.data.keyword.cloud_notm}} Private, you need to purchase a separate entitlement.
{: note}

After purchasing an entitlement, you can access your [My IBM dashboard](https://myibm.ibm.com/dashboard/){: external} to obtain your entitlement key for the offering. This key is required to deploy the release. Note that if you choose this option you are responsible for provisioning your own Kubernetes cluster.

For more information, see [Pricing](/docs/blockchain-sw?topic=blockchain-sw-ibp-sw-pricing).

## Considerations and limitations
{: #console-ocp-about-considerations}

- This offering does not include an entitlement to Red Hat OpenShift.
- You are responsible for the management of health monitoring, logging, and resource usage of your blockchain components.
- Users of this offering must manage their own security and infrastructure. The {{site.data.keyword.blockchainfull_notm}} Platform does not provision or provide those services.
- Persistent storage is required. Host-local storage volumes are not supported.
- You must have the cluster admin role in order to deploy the product.
- The console creates nodes based on the Hyperledger Fabric v1.4.4 node images.
- You can deploy only one {{site.data.keyword.blockchainfull_notm}} Platform console per Kubernetes namespace. If you plan to create multiple blockchain networks, for example to create different environments for development, staging, and production, you should create a unique project or namespace for each environment.
- You cannot upgrade a deployed network from {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud to {{site.data.keyword.blockchainfull_notm}} Platform v2.1.x.
- {{site.data.keyword.blockchainfull_notm}} Platform is not supported on OpenShift Online.
- There is no free trial at this time. Customers who are interested in exploring the functionality should try the offering on [{{site.data.keyword.cloud_notm}}](/docs/blockchain?topic=blockchain-ibp-console-overview){: external}.
- You cannot access the {{site.data.keyword.blockchainfull_notm}} Platform console using the Chrome browser on Mac OS Catalina when the {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0 or v2.1.1 is deployed with the default configuration that uses self-signed certificates. See [Known issues](/docs/blockchain-sw?topic=blockchain-sw-sw-known-issues#sw-known-issues-catalina) for a list of workarounds.

## Installing {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2
{: #console-ocp-about-install}

The {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 uses a [Kubernetes Operator](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/){: external} to install the {{site.data.keyword.blockchainfull_notm}} Platform console on your cluster and manage the deployment of your nodes. When you purchase the {{site.data.keyword.blockchainfull_notm}} Platform license from Passport Advantage Online, you receive a token that provides access to {{site.data.keyword.IBM_notm}} Entitlement registry. You can use your token with the commands and files that are provided in the installation guide to automatically download the Docker images and start the operator and console on your cluster. When you are ready to get started, see [Deploying {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 on the OpenShift Container Platform](/docs/blockchain-sw?topic=blockchain-sw-deploy-ocp). If you are deploying the platform on other Kubernetes distributions, see [Deploying {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 on Kubernetes](/docs/blockchain-sw?topic=blockchain-sw-deploy-k8).

It is also possible to deploy the {{site.data.keyword.blockchainfull_notm}} Platform behind a firewall, without having access to the public internet. For more information, see [Deploying IBM Blockchain Platform v2.1.2 on the OpenShift Container Platform behind a firewall](/docs/blockchain-sw?topic=blockchain-sw-deploy-ocp-firewall). Otherwise, for other Kubernetes distributions see [Deploying IBM Blockchain Platform v2.1.2 on Kubernetes behind a firewall](/docs/blockchain-sw?topic=blockchain-sw-deploy-k8-firewall).

## Security Considerations
{: #console-ocp-about-security}

Because these components are deployed on your own infrastructure, you are responsible for managing their security. This includes important areas of security, such as Identity and Access Management, key management, and data encryption. Review the following topic on [Security](/docs/blockchain-sw?topic=blockchain-sw-ibp-security) for the list of considerations.

## Getting support
{: #console-ocp-about-support}

For more information about how to get support on {{site.data.keyword.blockchainfull_notm}} Platform, in addition to free blockchain developer resources and support forums that you can use to troubleshoot problems, see [Getting support](/docs/blockchain-sw?topic=blockchain-sw-blockchain-support#blockchain-support).

## Next steps
{: #console-ocp-about-next-steps}

When you are ready to learn how to deploy an instance of the {{site.data.keyword.blockchainfull_notm}} Platform to your Kubernetes cluster see [Getting started with {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2](/docs/blockchain-sw?topic=blockchain-sw-get-started-console-ocp).
