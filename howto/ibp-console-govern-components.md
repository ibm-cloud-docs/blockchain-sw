---

copyright:
  years: 2019, 2020
lastupdated: "2020-06-18"

keywords: network components, Kubernetes, OpenShift, allocate resources, batch timeout, reallocate resources, LevelDB, CouchDB, ordering nodes, ordering, add and remove, governance

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

# Component governance
{: #ibp-console-govern-components}

<div style="background-color: #f4f4f4; padding-left: 20px; border-bottom: 2px solid #0f62fe; padding-top: 12px; padding-bottom: 4px; margin-bottom: 16px;">
  <p style="line-height: 10px;">
    <strong>Running a different version of IBM Blockchain Platform?</strong> Switch to version
    <a href="https://cloud.ibm.com/docs/blockchain-sw-213?topic=blockchain-sw-213-ibp-console-adv-deployment">2.1.3</a>,
    <a href="https://cloud.ibm.com/docs/blockchain-sw-25?topic=blockchain-sw-25-ibp-console-adv-deployment">2.5</a>
    </p>
</div>

After creating CAs, peers, and ordering nodes, you can use the console to update these components in a variety of ways.
{:shortdesc}

**Target audience:** This topic is designed for network operators who are responsible for creating, monitoring, and managing their components in the blockchain network.

## LevelDB vs CouchDB
{: #ibp-console-govern-components-level-couch}

During the creation of a peer, it is possible to choose between two state database options: LevelDB and CouchDB. Recall that the state database keeps the latest value of all of the keys (assets) stored on the blockchain. For example, if a car has been owned by Varad and then Joe, the value of the key representing the ownership of the car would be "Joe".

Because it can be useful to perform rich queries against the state database (for example, searching for every red car with an automatic transmission owned by Joe), users will often choose a Couch database, which stores data as JSON objects. LevelDB, on the other hand, only stores information as key-value pairs, and can therefore not be queried in this way. Users must keep track of block numbers and query the blocks directly (or within a range of block numbers), and parse the information. However, LevelDB is also faster than CouchDB, though it does not support database indexing (which helps performance).

This support for rich queries is why **CouchDB is the default database** unless a user selects the **State database selection** box during the process of adding a peer selects **LevelDB** on the subsequent tab.

Because the data is modeled differently in a Couch database than in a Level database, **the peers in a channel must all use the same database type**. If data written for a Level database is rejected by a Couch database (which can happen, as CouchDB keys have certain formatting restrictions as compared to LevelDB keys), a state fork would be created between the two ledgers. Therefore, **take extreme care when joining a channel to know the database type supported by the channel**. It might be necessary to create a new peer using the appropriate database type and join it to the channel. Note that the database type cannot be changed after a peer has been deployed.
{:important}

## Ordering node configurations
{: #ibp-console-govern-components-suggested-ordering-node-configurations}

While it's possible to use the console to build a configuration of any number of ordering nodes (no configuration is explicitly restricted), some numbers provide a better balance between cost and performance than others. The reason for this lies in satisfying the needs of high availability (HA) and in understanding the Raft concept of the "quorum", the minimum number of nodes that must be available (out of the total number) for the ordering service to process transactions.

In Raft, a **majority of the total number of nodes** is needed to form a quorum. In other words, if you have one node, you need that node available to have a quorum, because the majority of one is one. Similarly, if you have two nodes, you will need both available, since the majority of two is two (for this reason, a configuration of two nodes is discouraged; there is no advantage to a two node configuration). In a similar vein, the majority of three is two, the majority of four is three, the majority of five is three, and so on.

While satisfying the quorum will make sure the ordering service is functioning, production networks also have to think about deployment configurations that are highly available (in other words, configurations in which the loss of a certain number of nodes can be tolerated by the system). Typically, this means tolerating two nodes failing: one node going down during a normal maintenance cycle, and another going down for any other reason (such as a power outage or error).

This is why, by default, the console offers two options: one node or five nodes. Recall that the majority of five is three. This means that in a five node configuration, the loss of two nodes can be tolerated. If your configuration features four nodes, only one node can be down for any reason before **another** node going down means a quorum has been lost and the ordering service will stop processing transactions.

## How the console interacts with your Kubernetes cluster
{: #ibp-console-govern-components-iks-console-interaction}

It is the network operator's responsibility to monitor CPU, memory, and storage usage and ensure that adequate resources are available **before** attempting to create or resize a node.
{:important}

Because your instance of the {{site.data.keyword.blockchainfull_notm}} Platform console and your Kubernetes cluster do not communicate directly about the resources that are available, the process for deploying or resizing components by using the console must follow this pattern:

1. **Size the deployment that you want to make**. The **Resource allocation** panels for the CA, peer, and ordering node in the console offer default CPU, memory, and storage allocations for each node. You may need to adjust these values according to your use case. If you are unsure, start with default allocations and adjust them as you understand your needs. Similarly, the **Resource reallocation** panel displays the existing resource allocations.

  For a sense of how much storage and compute you will need in your cluster, refer to the chart after this list, which contains the current defaults for the peer, orderer, and CA.

2. **Check whether you have enough resources in your Kubernetes cluster**.  If you do not have enough space in your cluster to deploy or resize resources, you will need to increase the size of your cluster. Check the documentation of your cloud provider to learn how to scale clusters. If you have enough space in your cluster, you can continue with step 3.
3. **Use the console to deploy or resize your node**. If your Kubernetes pod is large enough to accommodate the new size of the node, the reallocation should proceed smoothly. If the worker node that the pod is running on is running out of resources, you can add a new larger worker node to your cluster and then delete the existing working node.

| **Component** (all containers) | CPU**  | Memory (GB) | Storage (GB) |
|--------------------------------|---------------|-----------------------|------------------------|
| **Peer**                       | 1.1           | 2.8                   | 200 (includes 100GB for peer and 100GB for state database)|
| **CA**                         | 0.1           | 0.2                   | 20                     |
| **Ordering node**              | 0.35          | 0.7                   | 100                    |


**  Actual VPC allocations are visible in the blockchain console when a node is deployed.  

If you plan to deploy a five node Raft ordering service, note that the total of your deployment will increase by a factor of five, a total of 1.75 CPU, 3.5 GB of memory, and 500 GB of storage for the five Raft nodes. A 4 CPU Kubernetes single worker node cluster is minimally recommended to allow enough CPU for the Raft cluster.

For cases when a user wants to minimize charges without stopping or deleting a node, it is possible to scale the node down to a minimum of 0.001 CPU (1 milliCPU). Note that the node will not be functional when using this amount of CPU.

While the figures in this topic endeavor to be precise, be aware that there are times when a node may not deploy even when it appears that you have enough space in your cluster. Make sure to reference your Kubernetes dashboard to see when components deploy and for error messages when they don't. In cases where a component doesn't deploy for a lack of resources, even if there seems to be enough space in the cluster, you will likely have to deploy additional cluster resources for the component to deploy.
{:tip}

## Allocating resources
{: #ibp-console-govern-components-allocate-resources}



Resources can be set during the creation of a node by clicking on the **Resource allocation** box while the node is being created. If this box is not checked, the default resource allocations, which can be seen below, will be used.

The **Resource allocation** panel in the console provides default values for the various fields that are involved in creating a node. These values are chosen because they represent a good way to get started. However, every use case is different. While this topic will provide guidance for ways to think about these values, it ultimately falls to the user to monitor their nodes and find sizings that work for them. Therefore, barring situations in which users are certain that they will need values different from the defaults, a practical strategy is to use these defaults at first and adjust them later. For an overview of performance and scale of Hyperledger Fabric, which the {{site.data.keyword.blockchainfull_notm}} Platform is based on, see [Answering your questions on Hyperledger Fabric performance and scale](https://www.ibm.com/blogs/blockchain/2019/01/answering-your-questions-on-hyperledger-fabric-performance-and-scale/){: external}.

All of the containers that are associated with a node have **CPU** and **memory**, while certain containers that are associated with the peer, ordering node, and CA also have **storage**. For more information about storage, see [storage](/docs/blockchain-sw?topic=blockchain-sw-deploy-ocp#deploy-ocp-storage).

You are responsible for monitoring your CPU, memory and storage consumption in your cluster. If you do happen to request more resources for a blockchain node than are available, the node will not start. However, existing nodes will not be affected.  For information about how to increase the CPU, memory, and storage, consult the documentation of your cloud provider.
{:note}

Every node has a gRPC web proxy container that bootstraps the communication layer between the console and a node. This container has fixed resource values and is included on the Resource allocation panel to provide an accurate estimate of how much space is required on your Kubernetes cluster in order for the node to deploy. Because the values for this container cannot be changed, we will not discuss the gRPC web proxy in the following sections.

### Certificate Authorities (CAs)
{: #ibp-console-govern-components-CA}

Unlike peers and ordering nodes, which are actively involved in the transaction process, CAs are involved only in the registration and enrollment of identities, and in the creation of an MSP. This means that they require less CPU and memory. To stress a CA, a user would need to overwhelm it with requests (likely using APIs and a script), or have issued so many certificates that the CA runs out of storage. Under typical operations, neither of these things should happen, though as always, these values should reflect the needs of a particular use case.

The CA has only one associated container that we can adjust:

* **CA container**: Encapsulates the internal CA processes, such as registering and enrolling nodes and users, as well as storing a copy of every certificate it issues.

#### Sizing a CA during creation
{: #ibp-console-govern-components-CA-sizing-creation}

As we noted in our section on [How the console interacts with your Kubernetes cluster](#ibp-console-govern-components-iks-console-interaction), it is recommended to use the defaults for these peer containers and adjust them later when it becomes apparent how they are being utilized by your use case.

| Resources | Condition to increase |
|-----------------|-----------------------|
| **CA container CPU and memory** | When you expect that your CA will be bombarded with registrations and enrollments. |
| **CA storage** | When you plan to use this CA to register a large number of users and applications. |



#### Creating highly available CAs
{: #ibp-console-govern-components-CA-HA}

For information about creating highly available CAs through the use of replica sets, see [how to configure CA replica sets](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-ha-ca).


### Peers
{: #ibp-console-govern-components-peers}

The peer has only three associated containers that can be adjusted:

- **Peer container**: Encapsulates the internal peer processes (such as validating transactions) and the blockchain (in other words, the transaction history) for all of the channels it belongs to. Note that the storage of the peer also includes the smart contracts installed on the peer.
- **CouchDB container**: Where the state databases of the peer are stored. Recall that each channel has a distinct state database.
- **Smart contract container**: Recall that during a transaction, the relevant smart contract is "invoked" (in other words, run). Note that all smart contracts that you install on the peer will run in a separate container inside your smart contract container, which is known as a Docker-in-Docker container.

The peer also includes a container for the **Log Collector** that pipes the logs from the smart contract container to the peer container. Similar to the gRPC web proxy container, you cannot adjust the compute for this container.

#### Sizing a peer during creation
{: #ibp-console-govern-components-peers-sizing-creation}

As we noted in our section on [How the console interacts with your Kubernetes cluster](#ibp-console-govern-components-iks-console-interaction), it is recommended to use the defaults for these peer containers and adjust them later when it becomes apparent how they are being utilized by your use case.

| Resources | Condition to increase |
|-----------------|-----------------------|
| **Peer container CPU and memory** | When you anticipate a high transaction throughput right away. |
| **Peer storage** | When you anticipate installing many smart contracts on this peer and to join it to many channels. Recall that this storage will also be used to store smart contracts from all channels the peer is joined to. Keep in mind that we estimate a "small" transaction to be in the range of 10,000 bytes (10k). As the default storage is 100G, this means as many as 10 million total transactions will fit in peer storage before it will need to be expanded (as a practical matter, the maximum number will be less than this, as transactions can vary in size and the number does not include smart contracts). While 100G might therefore seem like much more storage than is needed, keep in mind that storage is relatively inexpensive, and that the process for increasing it is more difficult (require command line) than increasing CPU or memory. |
| **CouchDB container CPU and memory** | When you anticipate a high volume of queries against a large state database. This effect can be mitigated somewhat through the use of [indexes](https://hyperledger-fabric.readthedocs.io/en/release-1.4/couchdb_as_state_database.html#couchdb-indexes){: external}. Nevertheless, high volumes might strain CouchDB, which can lead to query and transaction timeouts. |
| **CouchDB (ledger data) storage** | When you expect high throughput on many channels and don't plan to use indexes. However, like the peer storage, the default CouchDB storage is 100G, which is significant. |
| **Smart contract container CPU and memory** | When you expect a high throughput on a channel, especially in cases where multiple smart contracts will be invoked at once. You should also increase the resource allocation of your peers if your smart contracts are written in JavaScript or TypeScript.|

### Ordering nodes
{: #ibp-console-govern-components-ordering-nodes}

Occasionally, a five node ordering service will be deleted by the Kubernetes garbage collector because it considers the nodes a resource that needs to be cleaned up. This process is both random and unrecoverable --- if the ordering service is deleted, all of the channels hosted on it are permanently lost. To prevent this, the `ownerReferences` field in the configuration of each ordering node must be removed **as soon as possible** to prevent your ordering service from being randomly deleted. For the steps about how to pull the configuration file, remove `ordererReferences`, and apply the change, see [Known issues](/docs/blockchain-sw?topic=blockchain-sw-sw-known-issues#sw-known-issues-ordering-service-delete).
{:important}

Because ordering nodes neither maintain the State DB nor host smart contracts, they require fewer containers than peers do. But they do host the blockchain (the transaction history) because the blockchain is where the channel configuration is stored, and the ordering service must know the latest channel configuration to perform its role.

Similar to the CA, an ordering node has only one associated container that we can adjust (if you are deploying a five-node ordering service, you will have five separate ordering node containers, as well as five separate gRPC containers):

* **Ordering node container**: Encapsulates the internal orderer processes (such as validating transactions) and the blockchain for all of the channels it hosts.

#### Sizing an ordering node during creation
{: #ibp-console-govern-components-orderer-sizing-creation}

As we noted in our section on [How the console interacts with your Kubernetes cluster](#ibp-console-govern-components-iks-console-interaction), it is recommended to use the defaults for these orderer containers and adjust them later as it becomes apparent how they are being utilized.

| Resources | Condition to increase |
|-----------------|-----------------------|
| **Ordering node container CPU and memory** | When you anticipate a high transaction throughput right away. |
| **Ordering node storage** | When you anticipate that this ordering node will be part of an ordering service on many channels. Recall that the ordering service keeps a copy of the blockchain for every channel they host. The default storage of an ordering node is 100G, same as the container for the peer itself. |

If an ordering service is overstressed, it might hit timeouts and start dropping transactions, requiring transactions to be resubmitted. This causes much greater harm to a network than a single peer struggling to keep up. In a Raft ordering service configuration, an overstressed leader node might stop sending heartbeat messages, triggering a leader election, and a temporary cessation of transaction ordering. Likewise, a follower node might miss messages and attempt to trigger a leader election where none is needed.
{:important}


## Reallocating resources
{: #ibp-console-govern-components-reallocate-resources}

Resizing a node requires the containers to be rebuilt, which can cause a delay in the functioning of the node.
{:important}



Third party tools such as [Sysdig](https://sysdig.com){: external} can be used to help monitor the usage in your cluster. If you determine that a worker node is running out of resources, you can add a new larger worker node to your cluster and then delete the existing working node.
{:note}


While it takes less effort to deploy enough resources to your Kubernetes cluster from the start and therefore be able deploy and expand resources without having to increase the resources in your cluster, the bigger the deployment, the more it will cost. Users need to consider their options carefully and recognize the tradeoffs that they are making regardless of the option that they choose.




You can scale your cluster by monitoring your nodes and following the instructions available from your cloud provider to add more nodes, larger nodes, or increasing the size of the nodes, depending on the options available in your cloud provider. The method you will use to increase storage will depend on the storage class you chose for your cluster. Note that if you are about to exhaust the storage on your peer or ordering node, you might need to deploy a new peer or ordering node with more storage and let it sync via your other components on the same channels.





## Using certificates from an external CA with your peer or ordering service
{: #ibp-console-govern-third-party-ca}


Currently, it is not possible to create a five node Raft ordering service using certificates entered into the console as described in this section. You can still use the normal register and enroll flow described in [Build a network](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-build-network#ibp-console-build-network-use-CA-org1) to create this type of ordering service.
{:important}


Instead of using an {{site.data.keyword.blockchainfull_notm}} Platform Certificate Authority as your peer or ordering service's CA, you can use certificates from an external CA, one that is not hosted by {{site.data.keyword.IBM_notm}}, as long as the CA issues certificates in [X.509](https://hyperledger-fabric.readthedocs.io/en/release-1.4/identity/identity.html#digital-certificates){: external} format.

### Before you begin
{: #ibp-console-govern-third-party-ca-prereq}

1. You need to gather the following certificate information and save it to individual files that can be uploaded to the console.   
**Note:** The certificates inside the files can be in either `PEM` format or `base64 encoded` format.
 * **Peer or ordering service identity certificate** This is the signing certificate from your external CA that the peer or ordering service will use.
 * **Peer or ordering service identity private key**  This is your private key corresponding to the signed certificate from your third-party CA that this peer or ordering service will use.
 * **Peer or ordering service organization MSP definition** You must manually generate this file using instructions provided in [Manually building an MSP JSON file](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-organizations#console-organizations-build-msp).
 * **TLS CA certificate** This is the public signing certificate created by your external TLS CA that will be used by this peer or ordering service.
  * **TLS CA private key** This is the private key corresponding to the signed certificate from your TLS CA that will be used by this peer or ordering service for secure communications with other members on the network.
 * **TLS CA root certificate** (Optional) This is the root certificate of your external TLS CA. You must provide either a TLS CA root certificate or an intermediate TLS CA certificate, you may also provide both.
 * **Intermediate TLS certificate**: (Optional) This is the TLS certificate if your TLS certificate is issued by an intermediate TLS CA. Upload the intermediate TLS CA certificate. You must provide either a TLS CA root certificate or an intermediate TLS CA certificate, you may also provide both.
 * **Peer or ordering service admin identity certificate** This is the signing certificate from your external CA that the admin identity of this peer or ordering service will use. This certificate is also known as your peer or ordering service admin identity key.
 * **Peer or ordering service admin identity private key**  This is the private key corresponding to the signed certificate from your external CA that the admin identity of this peer or ordering service will use.

2. Import the generated peer or ordering service organization MSP definition file into the console, by clicking the **Organizations** tab followed by **Import MSP definition**.

Now you have the choice of creating a peer or single-node ordering service node, or  a five node ordering service.

### Option 1: Create a new peer or single-node ordering service using certificates from an external CA
{: #ibp-console-govern-create-peer-orderer-third-party-ca-}

You can skip to **Option 2** if you want to create a new five node ordering service. The following instructions are only for creating a peer or single-node ordering service using certificates from your external CA.
{:note}

Now that you have gathered all the necessary certificates, you are ready to create a peer or ordering service which uses those certificates. Follow these instructions to create the peer or single-node ordering service with certificates from an external CA.

1. On the **Nodes** tab click **Add peer** or **Add ordering service**.
2. Make sure the option to **Create** the peer or ordering service is selected. Then click **Next**.
3. After entering a display name for the node, select the option to use an external CA.
4. Step through the panels and upload the files corresponding to the certificate and private key you gathered.
5. Ensure you select the peer or ordering service organization MSP definition that you imported into the console from the drop-down list.
6. On the last step when you are asked to associate an identity with your peer or ordering service, you need to click **New identity**.
7. Specify any value as the **Display name** for this identity. The display name will be visible in the Wallet after you create the node.
8. In the **Certificate** field, upload the file that contains  the **Peer or ordering service admin identity certificate**.
9. In the **Private key** field, upload the file that contains  the **Peer or ordering service admin identity private key**.
10. Review the information on the Summary page and click **Add peer** or **Add ordering service**.

### Option 2: Create a five node ordering service using certificates from an external CA
{: #ibp-console-govern-create-five-node}

 You  have the additional option of deploying a five node ordering service that uses the Raft consensus protocol.  Before you deploy a five node ordering service, you need to build a JSON file that contains all of the certificates for the five nodes by using the following instructions:

#### Create the certificates JSON file
{: #ibp-console-govern-create-certs-file}

The required certificates JSON file contains an array of five `msp` entries, where each array element contains the certificates for one of the ordering nodes.  You must specify unique certificates for each node, do not reuse certificates across the different ordering nodes. The certificates in the `component` section represent the certificates for the node itself, while the `tls` section includes the certificates issued by the TLS CA.  

- **keystore**: The private key for this node
- **signcerts**: The public key (also known as a signing certificate or enrollment certificate) assigned by the CA for this node.
- **cacerts**: The certificate of the root CA.
- **admincerts**: The certificate of the admin users of the node. This might also be the admin of the organization.
- **intermediatecerts**: If your network includes multi-level CAs, paste in the certificate of the intermediate CA. If you did not use an intermediate certificate, you can leave this field blank.

Using the certificates you gathered above, paste in the corresponding certificate in the fields below, in base64 format.

You can convert the contents of your certificate file, `<cert.pem>` from `PEM` format into a base64 string by running the following command on your local machine:

```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-d"; else echo "-b 0"; fi)
cat <cert.pem> | base64 $FLAG
```
{:codeblock}

```json
[
    {
        "msp": {
            "component": {
                "keystore": [“<cert>“],
                "signcerts": [“<cert>“],
                "cacerts": [“<cert>“],
                "admincerts": [“<cert>“],
                "intermediatecerts": [“<cert>“]
            },
            "tls": {
                "keystore": [“<cert>“],
                "signcerts": [“<cert>“],
                "cacerts": [“<cert>“],
                "intermediatecerts": [“<cert>“]
            }
        }
    },
    {
        "msp": {
            "component": {
                "keystore": [“<cert>“],
                "signcerts": [“<cert>“],
                "cacerts": [“<cert>“],
                "admincerts": [“<cert>“],
                "intermediatecerts": [“<cert>“]
            },
            "tls": {
                "keystore": [“<cert>“],
                "signcerts": [“<cert>“],
                "cacerts": [“<cert>“],
                "intermediatecerts": [“<cert>“]
            }
        }
    },
    {
        "msp": {
            "component": {
                "keystore": [“<cert>“],
                "signcerts": [“<cert>“],
                "cacerts": [“<cert>“],
                "admincerts": [“<cert>“],
                "intermediatecerts": [“<cert>“]
            },
            "tls": {
                "keystore": [“<cert>“],
                "signcerts": [“<cert>“],
                "cacerts": [“<cert>“],
                "intermediatecerts": [“<cert>“]
            }
        }
    },
    {
        "msp": {
            "component": {
                "keystore": [“<cert>“],
                "signcerts": [“<cert>“],
                "cacerts": [“<cert>“],
                "admincerts": [“<cert>“],
                "intermediatecerts": [“<cert>“]
            },
            "tls": {
                "keystore": [“<cert>“],
                "signcerts": [“<cert>“],
                "cacerts": [“<cert>“],
                "intermediatecerts": [“<cert>“]
            }
        }
    },
    {
        "msp": {
            "component": {
                "keystore": [“<cert>“],
                "signcerts": [“<cert>“],
                "cacerts": [“<cert>“],
                "admincerts": [“<cert>“],
                "intermediatecerts": [“<cert>“]
            },
            "tls": {
                "keystore": [“<cert>“],
                "signcerts": [“<cert>“],
                "cacerts": [“<cert>“],
                "intermediatecerts": [“<cert>“]
            }
        }
    }
]
```
{:codeblock}

Save this definition as a `JSON` file.

#### Create the ordering service and use the certificates from the external CA for each ordering node
{: #ibp-console-govern-create-five-node-os}

Now that you have created a JSON file with all of the certificates for the ordering nodes, you are ready to create the ordering service.

1. On the **Nodes** tab click **Add ordering service**.
2. Make sure the option to **Create** an ordering service is selected. Then click **Next**.
3. Enter a single **Display name** for the five ordering nodes. The display name that you provide will be the prefix for each ordering node name and a number will be appended to it.
4. In **Number of ordering nodes**, select **Five ordering nodes**. Then select **External Certificate Authority configuration** and click **Next**.
5. Click **Add file** to upload the JSON file that contains all of the certificates.
6. Select the **Organization MSP** definition that you imported.
7.  On the next panel, you have the opportunity to configure resource allocation for the nodes. The selections you make here are applied to all five ordering nodes.  If you want to learn more about how to allocate resources to your node, see this topic on [Allocating resources](#ibp-console-govern-components-allocate-resources).
8. Review the summary and click **Add ordering service**.

### Consideration when using openSSL to generate certificates
{: #ibp-console-govern-third-party-openssl}

When using openssl to generate ECDSA 256 SHA-2 certificates, the string `EC` is inserted into the `BEGIN PRIVATE KEY` and `END PRIVATE KEY` header and footer. The `EC` causes a failure in the console if you later try to create an identity using the certificate pem file.

For example:
```
-----BEGIN EC PRIVATE KEY-----
MHcCAQEEINLMBxWNS+KfENOAZDbvwJxib+1FXaWIa9xuvyJjQNoAoGCCqGSM49
AwEHoUQDQgAEB49vPZw7Chp7xMLOg0n/L5D235rFhH+tu8CIGdj4Rwg3d6B1CW
NGggmidf1wrdYcHphq1LrT2ft4RkwR0w==
-----END EC PRIVATE KEY-----
```

Removing the `EC` from the header and footer resolves the problem:

```
-----BEGIN PRIVATE KEY-----
MHcCAQEEINLMBxWNS+KfENOAZDbvwJxib+1FXaWIa9xuvyJjQNoAoGCCqGSM49
AwEHoUQDQgAEB49vPZw7Chp7xMLOg0n/L5D235rFhH+tu8CIGdj4Rwg3d6B1CW
NGggmidf1wrdYcHphq1LrT2ft4RkwR0w==
-----END PRIVATE KEY-----
```

Now, you can import the `.PEM` file when you create an identity.

### What's next
{: #ibp-console-govern-third-party-ca-next}

You have gathered all of your peer or ordering service certificates from your third-party CA, created their corresponding organization MSP definition and created a peer or ordering service. If you are following along in the tutorials, you can return to the next step.
- If you created the peer node, the next step is to [Create the node that orders transactions](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-build-network#ibp-console-build-network-create-orderer).
- If you created the node to join an existing network, the next step is to [Add your organization to list of organizations that can transact](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-join-network#ibp-console-join-network-add-org2).
- If you created an ordering service, the next step is to [Create a channel](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-channel#ibp-console-build-network-create-channel).
