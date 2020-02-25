---

copyright:
  years: 2019, 2020
lastupdated: "2020-02-25"

keywords: FAQs, can I, upgrade, what version, peer ledger database, supported languages, why do I, regions

subcollection: blockchain-sw

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:faq: data-hd-content-type='faq'}
{:pre: .pre}


# FAQs
{: #ibp-v2-faq}

**General**   

- [What is the value of using {{site.data.keyword.blockchainfull_notm}} Platform over native Hyperledger Fabric?](#ibp-v2-faq-v2-IBP-Overview-1-7)
- [Where can a customer deploy the {{site.data.keyword.blockchainfull_notm}} Platform and how will {{site.data.keyword.IBM_notm}} support those deployment environments?](#ibp-v2-faq-sw-support)
- [Does {{site.data.keyword.blockchainfull_notm}} Platform v2.1.x run on OpenShift on {{site.data.keyword.cloud_notm}}?](#ibp-v2-faq-saas-ocp)
- [What version of Hyperledger Fabric is being used with {{site.data.keyword.blockchainfull_notm}} Platform?](#ibp-v2-faq-v2-Hyperledger-Fabric-3-1)
- [What database do the peers use for their ledger?](#ibp-v2-faq-v2-IBP-Overview-1-3)
- [What languages are supported for smart contracts?](#ibp-v2-faq-v2-IBP-Overview-1-4)
- [Do you support using certificates from non-IBM Certificate Authorities (CAs)?](#ibp-v2-faq-v2-external-certs)
- [I am currently using Hyperledger Fabric v1.4 and want to move to {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} or Multicloud. Can I continue to use Raft?](#ibp-v2-faq-migrate-raft)
- [Is it possible to deploy blockchain nodes to multiple clouds from a single blockchain console?](#ibp-v2-faq-multicloud)
- [How can I find the examples and tutorials within the VSCode extension?](#ibp-v2-faq-vscode-tutorials)
- [Is there a best practice for monitoring my blockchain resources?](#ibp-v2-faq-mon-res)
- [If service discovery is on, will an endorsement request be routed to any peer on the network?](#ibp-v2-faq-service-discovery)
- [Do ordering service Raft nodes use Transport Layer Security (TLS) for communication?](#ibp-v2-faq-raft-tls)
- [Can {{site.data.keyword.blockchainfull_notm}} Platform components interoperate with Hyperledger Fabric components on the same network? And vice versa? And what is the support policy for networks that include both {{site.data.keyword.blockchainfull_notm}} Platform components and open source components?](#ibp-v2-faq-interoperability)



## What is the value of using {{site.data.keyword.blockchainfull_notm}} Platform over native Hyperledger Fabric?
{: #ibp-v2-faq-v2-IBP-Overview-1-7}
{: faq}

Hyperledger Fabric is a powerful, versatile, pluggable, open source, distributed ledger technology capable of addressing a wide variety of use cases across many industries. {{site.data.keyword.blockchainfull_notm}} Platform is built on top of Fabric and includes integrated tools that provide end to end features for developers and network operators to develop, test, operate, monitor, and govern Hyperledger Fabric components by using an intuitive console UI. Quickly deploy an instance and use the streamlined console UI to [build a network](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-build-network), easily [install and instantiate smart contracts](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-smart-contracts), [govern your components](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-govern-components), and [govern your channel](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-govern). Interested in APIs? See the [{{site.data.keyword.blockchainfull_notm}} Platform API reference](https://cloud.ibm.com/apidocs/blockchain){: external}. With the {{site.data.keyword.blockchainfull_notm}} Platform, it is easy to extend a basic network, work with multicloud solutions, and receive {{site.data.keyword.IBM_notm}} worldwide support when needed. Finally, the {{site.data.keyword.blockchainfull_notm}} Platform provides additional security benefits that are essential for running an enterprise-grade production network.


## Where can a customer deploy the {{site.data.keyword.blockchainfull_notm}} Platform and how will {{site.data.keyword.IBM_notm}} support those deployment environments?]
{: #ibp-v2-faq-sw-support}

For an updated list of all the {{site.data.keyword.blockchainfull_notm}} Platform deployment options check out the [Supported Platforms](/docs/blockchain-sw?topic=blockchain-sw-console-ocp-about#console-ocp-about-prerequisites).

It is important to note that the {{site.data.keyword.blockchainfull_notm}} support only spans across Hyperledger Fabric based component issues for customers who have purchased the Blockchain Platform. Some examples include assistance in Fabric upgrades, chaincode instantiation, adding peers to channels, etc.

On the other hand, {{site.data.keyword.blockchainfull_notm}} will not provide deployment support for environments outside of Red Hat OpenShift. This means the customer is free to deploy into an environment of their choice from the list above, and should have the same {{site.data.keyword.blockchainfull_notm}} Platform experience as they would deploying into the {{site.data.keyword.cloud_notm}}. But it will be up to the customer to configure the network and address their own infrastructure related issues. Examples of these types of issues include failed deployment to a Kubernetes service, infrastructure capacity, custom firewall settings, etc.

## Does {{site.data.keyword.blockchainfull_notm}} Platform v2.1.x run on OpenShift on {{site.data.keyword.cloud_notm}}?
{: #ibp-v2-faq-saas-ocp}
{: faq}

Yes. The {{site.data.keyword.blockchainfull_notm}} Platform can be purchased and deployed in three ways on OpenShift:
- [{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/catalog/services/blockchain-platform){: external} is deployed and runs on [IBM Cloud](https://cloud.ibm.com/kubernetes/catalog/openshiftcluster){: external}.
- {{site.data.keyword.blockchainfull_notm}} Platform is also available as a software offering that can be deployed on Red Hat OpenShift and can run in all environments where OpenShift Container Platform (OCP) is supported. Read more about running OpenShift Container Platform [here](/docs/blockchain-sw?topic=blockchain-sw-console-ocp-about).
- Finally, experienced Hyperledger Fabric customers also have the option to download and use the peer, CA, orderer, and smart contract container [images](/docs/blockchain-sw?topic=blockchain-sw-blockchain-images).


## What version of Hyperledger Fabric is being used with {{site.data.keyword.blockchainfull_notm}} Platform?
{: #ibp-v2-faq-v2-Hyperledger-Fabric-3-1}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 uses Hyperledger Fabric v1.4.4 while {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0 and 2.1.1 use Hyperledger Fabric 1.4.3.

## What database do the peers use for their ledger?
{: #ibp-v2-faq-v2-IBP-Overview-1-3}
{: faq}
{: support}

You have the choice of either CouchDB or LevelDB when you configure your peer database. Because data is modeled differently in a Couch database than in a Level database, the peers in a channel must all use the same database type. See [LevelDB versus CouchDB](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-govern-components#ibp-console-govern-components-level-couch) to decide what is best for your business needs.

## What languages are supported for smart contracts?
{: #ibp-v2-faq-v2-IBP-Overview-1-4}
{: faq}
{: support}

The {{site.data.keyword.blockchainfull_notm}} Platform supports smart contracts that are written in  Node.js, Golang, or JavaScript, with future support for Java. The new Hyperledger Fabric programming model currently supports JavaScript, TypeScript, Java, and Go. If you are interested in preserving your existing application code, or by using Fabric SDKs for *Go*, you can still connect to your {{site.data.keyword.blockchainfull_notm}} Platform network by using the lower-level Fabric SDK APIs.

## Do you support using certificates from non-IBM Certificate Authorities?
{: #ibp-v2-faq-v2-external-certs}
{: faq}

Yes, you can bring your own certificates if they are issued by a CA that is X.509 compliant. The CA should sign by using ECDSA and the defaults should be set to use P256 curve. See this topic about [Using certificates from an external CA with your peer or ordering node](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-govern-components#ibp-console-govern-third-party-ca).

## I am currently using Hyperledger Fabric v1.4.x and want to move to {{site.data.keyword.blockchainfull_notm}} Platform v2.1.x. Can I continue to use Raft?
{: #ibp-v2-faq-migrate-raft}
{: faq}

Yes. The {{site.data.keyword.blockchainfull_notm}} Platform v2.1.x uses Raft consensus. All of the applications and smart contracts that you are using on Fabric 1.4.x are able to work on your {{site.data.keyword.blockchainfull_notm}} Platform network. However, no mechanism exists to migrate your ledger data from one network to another. Instead, you can reinstall your smart contract packages on your {{site.data.keyword.blockchainfull_notm}} Platform network. See also [Can IBM Blockchain Platform components interoperate with Hyperledger Fabric components on the same network?](/docs/blockchain-sw?topic=blockchain-sw-ibp-v2-faq#ibp-v2-faq-interoperability).

## Is it possible to deploy blockchain nodes to multiple clouds from a single blockchain console?
{: #ibp-v2-faq-multicloud}
{: faq}

You can not currently deploy blockchain nodes to multiple hosted cloud providers. However, you can use your console to operate a distributed multicloud network by importing nodes deployed by consoles on other clouds.

## How can I find the examples and tutorials within the VSCode extension?
{: #ibp-v2-faq-vscode-tutorials}
{: faq}

The {{site.data.keyword.blockchainfull_notm}} Platform extension provides guided tutorials within VS Code to help you get started. You can find these tutorials on the extension home page when the extension is first installed. However, you can return to the tutorials and the home page by going to the extensions tab. For more information, see [Guided tutorials in VS Code](/docs/blockchain-sw?topic=blockchain-sw-develop-vscode#develop-vscode-guided-tutorials).

## Is there a best practice for monitoring my blockchain resources?
{: #ibp-v2-faq-mon-res}
{: faq}
{: support}

You are responsible for the health monitoring and resource allocation of the blockchain nodes in your Kubernetes cluster. While requests against the nodes are being actively processed, you should be monitoring for spikes in resource consumption to avoid problems.  You can configure a monitoring tool, such as [Sysdig](https://sysdig.com/platform/){: external} with alert notifications for the nodes in your cluster.

You should be aware that JavaScript and TypeScript smart contracts require more resources than contracts written in Go. Therefore, when you are allocating resources to your peer node, it is important to allocate sufficient resources and monitor the peer containers to ensure adequate resources are available to the peer when smart contracts are instantiated on a channel and during transaction processing.
{: tip}

## If service discovery is on, will an endorsement request be routed to any peer on the network?
{: #ibp-v2-faq-service-discovery}
{: faq}

Yes, if you have the endorsement policy set to “ANY”. However, you do have the opportunity to bind the policy directly to an organization's peers. The service discovery information provided by the peer supplies two pieces of information, `Layouts` and `EndorsersByGroup`. With these two pieces of data, the SDK has the ability to send requests to peers in different organizations that meet the endorsement policy requirements. The node.js SDK provides default code that uses the `Layouts` and `EndoresersByGroup` and sends the requests to the appropriate peers to meet the endorsement policy requirements. This existing logic can be customized to meet the business needs.

## Do ordering service Raft nodes use Transport Layer Security (TLS) for communication?
{: #ibp-v2-faq-raft-tls}
{: faq}

Yes. The Raft ordering service nodes are configured to use TLS communication. TLS is embedded in the trust model of Hyperledger Fabric. By default, server-side TLS is enabled for all communications using TLS certificates. TLS is used to encrypt the communication between your nodes and as well as between your nodes and your applications. TLS prevents man-in-the-middle and session hijacking attacks. All {{site.data.keyword.blockchainfull_notm}} Platform components use TLS to communicate with each other.

## Can {{site.data.keyword.blockchainfull_notm}} Platform components interoperate with Hyperledger Fabric components on the same network? And vice versa? And what is the support policy for networks that include both {{site.data.keyword.blockchainfull_notm}} Platform components and open source components?
{: #ibp-v2-faq-interoperability}

Yes. Hyperledger Fabric networks consist of many distributed members owning one or more nodes. There are multiple deployment options:

* {{site.data.keyword.blockchainfull_notm}} Platform for IBM Cloud with console
* {{site.data.keyword.blockchainfull_notm}} Platform v2.1.x (Full Platform)  
* {{site.data.keyword.blockchainfull_notm}} Images
* Open source Hyperledger Fabric images or a non-IBM product

Containers deployed from any of the above sources can be connected on a single channel and transact. You can join IBM Blockchain Platform peers to any network running Hyperledger Fabric components. Similarly, you can invite Fabric peers to join channels hosted on an ordering service deployed on the IBM Blockchain Platform. Note that you will need to use Hyperledger Fabric APIs or the CLI. For more information about what is supported, see [Support for IBM Blockchain Platform v2.1.x](https://www.ibm.com/support/pages/support-ibm-blockchain-platform-v21x){: external}.



