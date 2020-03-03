---

copyright:
  years: 2019

lastupdated: "2020-03-03"

keywords: getting started tutorials, create a CA, enroll, register, create an MSP, wallet, create a peer, create ordering service, Raft, join a network, system channel

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

# Join a network tutorial
{: #ibp-console-join-network}

{{site.data.keyword.blockchainfull}} Platform is a blockchain-as-a-service offering that enables you to develop, deploy, and operate blockchain applications and networks. You can learn more about blockchain components and how they work together by visiting the [Blockchain component overview](/docs/blockchain-sw?topic=blockchain-sw-blockchain-component-overview#blockchain-component-overview). This tutorial is the second part in the [sample network tutorial series](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-build-network#ibp-console-build-network-sample-tutorial) and describes how to create nodes in the {{site.data.keyword.blockchainfull_notm}} Platform console and connect them a blockchain consortium hosted in another cluster.
{:shortdesc}

**Target audience:** This topic is designed for network operators who are responsible for creating, monitoring, and managing the blockchain network.  



If you have not already deployed your {{site.data.keyword.blockchainfull_notm}} Platform console to a Kubernetes cluster, see [Getting started with {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2](/docs/blockchain-sw?topic=blockchain-sw-get-started-console-ocp).  

You need to pay close attention to the resources at your disposal when you choose to deploy nodes and create channels. It is your responsibility to manage your Kubernetes cluster and deploy additional resources if necessary. For more information about component sizings and how the console interacts with your Kubernetes Service cluster, see [Allocating resources](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-govern-components#ibp-console-govern-components-iks-console-interaction).


## Sample network tutorial series
{: #ibp-console-join-network-structure}

You are currently on the second part of our three-part tutorial series. This tutorial guides you through the process of using the console to create and join a peer node to an existing {{site.data.keyword.blockchainfull_notm}} Platform network. 

* [Build a network tutorial](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-build-network#ibp-console-build-network) guides you through the process of hosting a network by creating an ordering service and a peer.
* **Join a network tutorial** (Current tutorial) Guides you through the process of joining an existing network by creating a peer and joining it to a channel.
* [Deploy a smart contract on the network](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-smart-contracts#ibp-console-smart-contracts) provides information on how to write a smart contract and deploy it on your network.

You can use the steps in these tutorials to build a network with multiple organizations in one cluster for the purposes of development and testing. Use the **Build a network** tutorial if you want to form a blockchain consortium by creating an ordering service and adding organizations. Use the **Join a network** tutorial to connect a peer to the network. Following the tutorials with different consortium members allows you to create a truly **distributed** blockchain network.

This tutorial is meant to show how to join a peer to an **existing** network. It presumes that an ordering service has already been created using another {{site.data.keyword.blockchainfull_notm}} Platform console (it is possible to join peers created in the {{site.data.keyword.blockchainfull_notm}} Platform to any network running Hyperledger Fabric based components using the Fabric APIs or the CLI, but we will not show that process here). If you don't have an existing network to join, visit the [Build a network tutorial](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-build-network#ibp-console-build-network) to learn how to create one. The **Join a network** tutorial takes you through the steps to create the following `Org2` blockchain components, highlighted in the blue box:
![Join network structure](../images/ibp2-join-network.svg "Join network structure"){: caption="Figure 1. Join network structure" caption-side="bottom"}  

Perform the steps in the **Join a network** tutorial to create the following components and complete the following actions:

* **One peer organization** `Org2`  
  Create the Org2 Membership Services Provider (MSP) definition which defines the organization `Org2`.
* **One peer** `Peer Org2`   
  The ledger, `Ledger x` in the illustration above, is maintained by distributed peers. The peer is deployed with [Couch DB](https://hyperledger-fabric.readthedocs.io/en/release-1.4/couchdb_as_state_database.html){: external} as the state database.
* **One Certificate Authority (CA)** `Org2 CA`
  A CA is the node that issues certificates to all organization admins as well as to nodes associated with an organization. We will create one CA for the peer organization `Org2`.
* **Joining one channel**
  The tutorial describes how to join the channel that was created by the [Build a network tutorial](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-build-network#ibp-console-build-network).

Throughout this tutorial we supply **recommended values** for some of the fields in the console. This allows the names and identities to be easier to recognize in the tabs and drop-down lists. These values are not mandatory, but you will find them helpful, especially since you will have to remember certain values, especially the IDs and secrets of registered users, you input at a previous step. If you forget these values, you will have to register additional users for your admins and components. We provide a table of the recommended values after each task and recommend that if you do not use sample values that you record the values you do use somewhere as you proceed through the tutorial.
{:tip}

## Step one: Create a peer organization and a peer
{: #ibp-console-join-network-create-ca-org2}

For each organization that you want to create with the console, you should deploy at least one CA. A CA is the node that issues certificates to all network participants (peers, ordering services, clients, admins, and so on). These certificates, which include a signing certificate and private key, allow network participants to communicate, authenticate, and ultimately transact. These CAs will create all of the identities and certificates that belong to your organization, in addition to defining the organization itself. You can then use those identities to deploy nodes, create admin identities, and submit transactions. For more information about your CA and the identities that you will need to create, see [Managing identities](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-identities#ibp-console-identities).

In this tutorial, we will create one organization. Therefore, we will need to create **one CA**.

### Creating your peer organization CA
{: #ibp-console-join-network-create-CA-org2CA}

Your CA issues the certificates and private keys for your organization's admins, client applications, and nodes. These identities are not managed by {{site.data.keyword.IBM_notm}} and the keys are not stored in the console. They are only stored in your browser local storage. Therefore, make sure to export your identities and the MSP of your organization. If you attempt to access the console from a different machine or a different browser, you will need to import these identities and organization definitions.
{:important}

Perform the following steps from your console:  

1. Navigate to the **Nodes** tab on the left and click **Add Certificate Authority**. The side panels will allow you to customize the CA that you want to create and the organization that this CA will issue keys for.
2. In this tutorial, we're creating nodes, so make sure the option to **Create a Certificate Authority** is selected. Then click **Next**.
3. Use the side panel to give your CA a **display name**. Our recommended value for this CA is `Org2 CA`. Then give your CA admin credentials by specifying a **CA administrator enroll ID** of `admin` and a secret of `adminpw`.
4. The **Advanced deployment options** can be safely ignored for purposes of this tutorial. For more information about these options, see the link below.
   * [Database and replica sets](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-build-ha-ca#ibp-console-build-ha-ca-create) (Creating an HA CA)
   * [Kubernetes zone selection](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-ha#ibp-console-ha-multi-zone) (Multizone HA) This option is only visible when your cluster is configured with multiple zones.
   * [Resource allocation](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-govern-components#ibp-console-govern-components-allocate-resources)
5. Review the Summary page, then click **Add Certificate Authority**. 

**Task: Creating the peer organization CA**

  | **Field** | **Display name** | **Enroll ID** | **Secret** |
  | ------------------------- |-----------|-----------|-----------|
  | **Create CA** | Org2 CA  | admin | adminpw |
  {: caption="Table 1. Creating the peer organization CA" caption-side="bottom"}

After you deploy the CA, you will use it when you create your organization MSP, register users, and to create your entry point to a network, the **peer**.


Advanced users may already have their own CA, and not want to create a new CA in the console. If your existing CA can issue certificates in `X.509` format, you can use certificates from your own third-party CA instead of creating new certificates here. The CA should sign using ECDSA and the defaults should be set to use P256 curve. See this topic on [Using certificates from an external CA with your peer or ordering node](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-govern-components#ibp-console-govern-third-party-ca) for more information.

### Associating the CA admin identity
{: #ibp-console-join-network-ca-admin}

Each CA is created with a CA admin identity. You can use the admin to register new users with your CA and generate certificates. Before you can use the console to operate your CA, you need to associate the CA admin identity with the CA node itself.

Depending on your cluster type, deployment of the CA can take up to ten minutes. When the CA is first deployed (or when the CA is otherwise unavailable), the box in the tile for the CA will be grey box. When the CA has successfully deployed and is running, this box will be green, indicating that it is "Running" and can be operated from the console. Before proceeding with the steps below, you must wait until the CA status is "Running". If the grey box stops blinking, you can try reloading the page in your browser to refresh the status.
{:important}

Once the CA is running, as indicated by the green box in the tile, complete the following steps:

1. Click the `Org2 CA` tile in the **Nodes** tab. Then click **Associate identity** on the CA overview panel.
2. On the side panel that opens, provide an **Enroll ID** of `admin` and an **Enroll secret** of `adminpw`. For the **Identity display name**, you can use the default value of `Org2 CA Admin`.
3. Click **Associate identity** to add the identity into your console Wallet and associate the admin identity with your CA.

After setting the CA admin identity, you will be able to see the table of registered users in the CA overview panel.

**Task: Associate identity**

  |  **Field** | **Display name** | **Enroll ID** | **Secret** |
  | ------------------------- |-----------|-----------|-----------|-----------|
  | **Enroll ID** |  Org2 CA Admin  | admin | adminpw |
  {: caption="Table 2. Associate the CA admin identity" caption-side="bottom"}

You can view the CA admin identity in your console wallet by clicking on the **Wallet** in the left navigation. Click the identity to view the certificate and private key of the CA admin. The identity is not stored in your console or managed by {{site.data.keyword.IBM_notm}}. It is only stored in local browser storage. If you change browsers, you will need to import this identity into your Wallet to be able to operate the CA. Click **Export identity** to download the identity and keys.

**Task: Check your Wallet**

  | **Field** |  **Display name** | **Description** |
  | ------------------------- |-----------|----------|
  | **Identity** | Org2 CA Admin | Org2 CA admin identity |
  {: caption="Table 3. Check your Wallet" caption-side="bottom"}

### Using your CA to register identities
{: #ibp-console-join-network-use-CA-org2}

Each node or application that you want to create needs certificates and private keys to participate in the blockchain network. You also need to create admin identities for these nodes and applications so that you can manage them from the console. We will create one CA and use it to create two identities:

* **An organization admin**: This identity allows you to operate nodes using the platform console.
* **A peer identity**: This is the identity of the peer itself. Whenever a peer performs an action (for example, endorsing a transaction) it will sign using its certificate.

Once you have associated the CA admin, you can use the CA tile to create these identities by completing the following steps:

1. Click the `Org2 CA` tile and ensure the `admin` identity that you created for the CA is visible in the table. Then click the **Register User** button.
2. First we'll register the organization admin, which we can do by giving an **Enroll ID** of `org2admin` and a **secret** of `org2adminpw`. Then set the `Type` for this identity as `admin`.  You can ignore the **Maximum enrollments** field. If you want to learn more about enrollments, see [Registering identities](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-identities#ibp-console-identities-register). Click **Next**.
3. This tutorial does not configure attributes on identities, see [Registering identities](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-identities#ibp-console-identities-register) if you want to learn more. Click **Register user**.
4. After the organization admin has been registered, repeat this same process for the identity of the peer (also using the `Org2 CA`). For the peer identity, give an enroll ID of `peer2` and a secret of `peer2pw`. This is a node identity, so select `peer` as the **Type**. You can ignore the **Maximum enrollments** field and, on the next panel, do not assign any **Attributes**, as before.

Registering these identities with the CA is only the first step in **creating** an identity. You will not be able to use these identities until they have been **enrolled**. For the `org2admin` identity, this will happen during the creation of the MSP, which we will see in the next step. In the case of the peer, it happens during the creation of the peer.
{:note}

**Task: Register users**

  |  **Field** | **Description** | **Enroll ID** | **Secret** | **Type**
  | ------------------------- |-----------|-----------|-----------|-----------|
  | **Create CA**  | CA admin | admin | adminpw | client |
  | **Register users** |  Org2 MSP Admin  | org2admin | org2adminpw | admin |
  | | Peer identity |  peer2 | peer2pw | peer|
  {: caption="Table 4. Using your CA to register user" caption-side="bottom"}

### Creating the peer organization MSP
{: #ibp-console-join-network-create-peers-org2}

Now that we have created the peer's CA and used it to **register** our organization identities, we need to create a formal definition of the peer's organization, which is known as the Membership Services Provider (MSP) definition. Many peers can belong to an organization. **You do not need to create a new organization every time you create a peer.** Because this is the first time that we go through the tutorial, we will create the MSP ID for this organization. During the process of creating the MSP, we are going to generate certificates for the `org2admin` identity and add them to our Wallet.

1. Navigate to the **Organizations** tab in the left navigation and click **Create MSP definition**.
2. Enter `Org2 MSP` as the organization MSP display name and `org2msp` and as the MSP ID. If you want to specify your own MSP ID in this field, make sure to review the instructions in the tool tip.
3. Under **Root Certificate Authority details**, specify the CA you used to register the identities in the previous step. If this is your first time through this tutorial, you should only see one: `Org2 CA`.
4. The **Enroll ID** and **Enroll secret** fields below this will auto populate with the enroll ID and secret for the first user that you created with your CA: `admin` and `adminpw`. However, using this identity would make your organization the same identity as your CA identity, which for security reasons is not recommended. Instead, select the enroll ID you created for your organization admin from the drop-down list, `org2admin`, and enter its associated secret, `org2adminpw`. Then, give this identity a display name, `Org2 MSP Admin `. Note: the default display name for this identity is the name of your MSP and the word "Admin"  that will be reflected in the default.
5. Click the **Generate** button to enroll this identity as the admin of your organization and export the identity to the Wallet, where it will be used when creating the peer and creating channels.
6. Click **Export** to export the admin certificates to your file system. As we said above, this identity is not stored in your cluster or managed by {{site.data.keyword.IBM_notm}}. It is only stored in local browser storage. If you change browsers, you will need to import this identity into your Wallet to be able to administer the peer.
7. Click **Create MSP definition**.

Exporting your organization admin identity is important because you are responsible for managing and securing these certificates. If you switch browsers, you will need to import this admin identity otherwise you will not be able to operate Org2.
{:important}

**Task: Create the peer organization MSP definition**

  |  | **Display name** | **MSP ID** | **Enroll ID**  | **Secret** |
  | ------------------------- |-----------|-----------|-----------|-----------|
  | **Create Organization** | Org2 MSP | org2msp |||
  | **Root CA** | Org2 CA ||||
  | **Org Admin Cert** | |  | org2admin | org2adminpw |
  | **Identity** | Org2 MSP Admin |||||
  {: caption="Table 5. Create the peer organization MSP definition" caption-side="bottom"}

After you have created the MSP, you should be able to see the peer organization admin in your **Wallet**, which can be accessed by clicking on the **Wallet** in the left navigation.

**Task: Check your Wallet**

  | **Field** |  **Display name** | **Description** |
  | ------------------------- |-----------|----------|
  | **Identity** | Org2 MSP Admin  | Org2 identity |
  {: caption="Table 6. Check your Wallet" caption-side="bottom"}

For more information about MSPs, see [managing organizations](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-organizations#ibp-console-organizations).

### Creating a peer
{: #ibp-console-join-network-peer-create}

After you have [created a CA](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-join-network#ibp-console-join-network-create-CA-org2CA), used it to register identities, and created the [peer organization MSP](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-join-network#ibp-console-join-network-create-peers-org2), you're ready to create a peer.

#### What role do peers play?
{: #ibp-console-join-network-peer-role}

It's important to remember that organizations themselves do not maintain ledgers. Peers do. Organizations also use peers to sign transaction proposals and approve channel configuration updates. Because having at least two peers on a channel makes them highly available, having at least two peers joined to a channel is considered a best practice for production level implementations. In this tutorial, we'll only show the process for creating a single peer.

From a resource allocation perspective, it is possible to join the same peers to multiple channels. The design of the peer ensures that the data from one channel cannot pass to another through the peer. However, because the peer will store a separate ledger for each channel, it is necessary to ensure that the peer has enough processing power and storage to handle the transaction and data load.

#### Deploying your peer
{: #ibp-console-join-network-deploy-peer-role}

Use your console to perform the following steps:

1. On the **Nodes** page, click **Add peer**.
2. Make sure the option to **Create a peer** is selected. Then click **Next**.
3. Give your peer a **Display name** of `Peer Org2`.
4. The **Advanced deployment options** can be safely ignored for purposes of this tutorial. For more information about these options, see the links below.
   * [State database selection](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-govern-components#ibp-console-govern-components-level-couch)
   * [Kubernetes zone selection](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-ha#ibp-console-ha-multi-zone) (Multizone HA) This option is only visible when your cluster is configured for multizone support.
   * [External Certificate Authority configuration](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-govern-components#ibp-console-govern-third-party-ca)
   * [Resource allocation](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-govern-components#ibp-console-govern-components-allocate-resources)
5. Click **Next**.
6. On the next screen
   * Select `Org2 CA`, as this is the CA you used to register the peer identity.
   * Select the **Enroll ID** for the peer identity that you created for your peer from the drop-down list, `peer2`, and enter its associated **secret**, `peer2pw`.
   * Then, select `Org2 MSP` from the drop-down list
   * The **TLS Certificate Signing Request (CSR) hostname** is an option available to advanced users who want specify a custom domain name that can be used to address the peer endpoint. Custom domain names are not a part of this tutorial, so leave the **TLS CSR hostname** blank for now.
   * Click **Next**.
7. The last side panel asks you to **Associate an identity** to make it the admin of your peer. For the purpose of this tutorial, make your organization admin, `Org2 MSP Admin`, the admin of your peer as well. It is possible to register and enroll a different identity with the `Org2 CA` and make that identity the admin of your peer, but this tutorial uses the `Org2 MSP Admin` identity.
8. Review the summary and click **Add peer**.  


**Task: Deploying a peer**

|  | **Display name** | **MSP ID** | **Enroll ID** | **Secret** |
| ------------------------- |-----------|-----------|-----------|-----------|
| **Create Peer** | Peer Org2 | org2msp |||
| **CA** | Org2 CA ||||
| **Peer Identity** | |  | peer2 | peer2pw |
| **Administrator certificate** | org2msp ||||
| **Associate identity** | Org2 MSP Admin  |||||
{: caption="Table 7. Deploying a peer" caption-side="bottom"}

In a production scenario, it is recommended to deploy three peers to each channel. This is to allow one peer to go down (for example, during a maintenance cycle) and still maintain highly available peers. To deploy more than one peer for an organization, use the same CA you used to register your first peer identity. In this tutorial, that would be `Org2 CA`. Then, register a new peer identity using a distinct enroll ID and secret. For example, `org2secondpeer` and `org2secondpeerpw`. Then, when creating the peer, give this enroll ID and secret. As this peer is still associated with Org2, choose `Org2 CA`, `Org2 MSP`, and `Org2 MSP Admin ` from the drop-down lists. You may choose to give this new peer a different admin, which can be registered and enrolled with `Org2 CA`, but this optional. This tutorial series will only show the process for creating a single peer for each peer organization.
{:tip}

## Step two: Join the consortium hosted by the ordering service
{: #ibp-console-join-network-add-org2}

Because the channel we are joining, `channel1`, has already been created, it is not strictly necessary for Org2 to become a member of the consortium before being joined to the channel. Instead, it is possible for the MSP representing Org2 to be sent to Org1 out of band using the process described in [Export your organization information](#ibp-console-join-network-add-org2-remote). Org1, the sole administrator of `channel1`, can then perform a channel configuration update to add Org2 to the channel using the process described in [Step three: Add the peer's organization to an existing channel](#ibp-console-join-network-add-channel). After which the JSON representing the ordering service can be exported following the process described in [Export the ordering service](#ibp-console-join-network-export-ordering-service) and imported following the process described in [Import the ordering service from another cluster](#ibp-console-join-network-import-remote-orderer) However, because Org2 cannot create a channel until its MSP is joined to the consortium, we will show that process here.
{:important}

As we noted earlier, a peer organization must be known to the ordering service before it can create a channel (this is also known as joining the "consortium", the list of organizations known to the ordering service). This is because channels are, at a technical level, **messaging paths** between peers through the ordering service. Just as a peer can be joined to multiple channels without information passing from one channel to another, so too can an ordering service have multiple channels running through it without exposing data to organizations on other channels.

Because only ordering service admins can add peer organizations to the consortium, you need to either:

* **Be** the ordering service admin. In which case you can add the peer organization you created to the consortium directly.
* **Send** MSP information to the ordering service admin, who will add your organization to their consortium.

In the next step, we will show how to add your peer organization to a consortium hosted by an ordering service in another {{site.data.keyword.blockchainfull_notm}} Platform service instance. The tutorial assumes that you have created the ordering service and can follow every step yourself. If the ordering service was created in another console, ensure that the ordering service admin follows the steps marked with the blue "tip" box at the top.

If you followed the Build a network tutorial or if your console already includes the ordering service, you can skip ahead to [Add the peer's organization to the ordering service](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-join-network#ibp-console-join-network-add-org2-local). You can then continue with [Step three: Add the peer's organization to an existing channel](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-join-network#ibp-console-join-network-add-channel).

### Export your organization information
{: #ibp-console-join-network-add-org2-remote}

To send your MSP to the console where the ordering service was created:

1. Navigate to the **Organizations** tab. You can see the organizations available for export are listed under **Available organizations**. Click the **Export** button inside the `Org2 MSP` organization tile to download the JSON configuration file that represents the MSP of the peer organization. Note: you must be an admin of the **peer organization**, meaning you have the peer organization admin identity in your Wallet.
2. Send this file to the ordering service admin in an out of band operation.

### Import the organization definition
{: #ibp-console-join-network-import-remote-msp}

This step needs to be completed by an ordering service admin.
{:tip}

Once the MSP representing Org2 has been received, an administrator of the ordering service must import the JSON file:

- Navigate to the **Organizations** tab, click the **Import MSP definition** button, and select the JSON file that represents the `Org2 MSP` peer organization MSP definition. You can leave the `I have an administrator identity for the MSP definition` checkbox unchecked because the admin identity is not required here.

### Add the peer's organization to the ordering service consortium
{: #ibp-console-join-network-add-org2-local}

This step needs to be completed by an ordering service admin.
{:tip}

After the MSP representing Org2 has been imported, use these steps to add the peer organization to the consortium hosted by the ordering service. Only ordering service admins, those who are listed under **Ordering service administrators** in the **Details** tab of the ordering service, can add peer organizations to the consortium. You must have the ordering service organization admin identity in your Wallet to perform this task.

1. Navigate to the **Nodes** tab.
2. Scroll down to the ordering service you want to use and click the tile to open it.
3. Under **Consortium Members**, click **Add organization**.
4. From the drop-down list, select `Org2 MSP`, as this is the MSP that represents `Org2`.
5. Click **Add organization**.

If you followed the **Build a network** tutorial or if your console already includes the ordering service and a channel, you can now skip ahead to [Step three: Add the peer's organization to an existing channel](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-join-network#ibp-console-join-network-add-channel).

### Export the ordering service
{: #ibp-console-join-network-export-ordering-service}

This step needs to be completed by the ordering service admin.
{:tip}

Complete the following steps to **Export** the ordering service before it can be imported by the peer organization:

1. Navigate to the ordering service inside the **Nodes** tab. Click the **Export** button beneath the ordering service name to download a JSON configuration file.
2. Send this file to the peer organization in an out of band operation. The peer organization administrator can then use the configuration file to add the ordering service to the console.

### Import the ordering service from another cluster
{: #ibp-console-join-network-import-remote-orderer}

Complete the following steps to **import** the ordering service into your console:

1. Navigate to the **Nodes** page and click **Add ordering service**.
2. Click the option to **Import an existing ordering service**.
3. Then click the **Add file** button to select the JSON that represents the ordering service. If the JSON representing the ordering service was created before October 2nd, 2019, you will be asked to specify a **Service location** for this ordering service.
4. When you are asked to associate an identity for the ordering service nodes, select the peer organization identity you have locally (note that this "association" step is for administration of the ordering nodes themselves and will not make this identity one of the "ordering service administrators" mentioned in the [Add the peer's organization to the consortium](#ibp-console-join-network-add-org2-local) above. In this tutorial, this local peer organization identity would be `Org2 MSP Admin `. Click **Add ordering service**.

When this process is complete, it is possible for `Org2` to create a channel hosted on the `Ordering Service`.

## Step three: Add the peer's organization to an existing channel
{: #ibp-console-join-network-add-channel}

This step needs to be completed by an admin of Org1.
{:tip}

Now the orderer admin can add the peer organization to the channel:
1. Navigate to the **Channels** tab, click `channel1`.
2. Click the  **Settings** icon to update the channel and add the peer organization to the channel.
3. In the **Organizations** section, open the `Select a channel member` drop-down list and select the peer organization MSP, `Org2 MSP`.
4. Click **Add** and then assign permissions for that organization. We recommend you make them an `Operator` so they can update the channel.
5. Under **Channel update policy**, select `1 out of 2`, meaning only one of the two organizations needs to approve updates to the channel.
6. In the **Channel Updater MSP** drop-down list (under the **Channel updater organization** heading) ensure that `Org1 MSP` is selected.
7. In the **Identity** drop-down list, ensure that `Org1 MSP Admin ` is selected.
8. When you are ready, click **Send proposal**.

After the admin of the ordering service has joined your peer organization to the channel you can now join your peers to the channels that are hosted by the ordering service.

## Step Four: Join your peer to the channel
{: #ibp-console-join-network-join-peer-org2}

We are almost done. Your peer can now be joined to an existing channel. You need to get the `channel name`, out of band, from an organization that is a member of the channel. In the **Build a network** tutorial, we created a channel named `channel1`. If you are not already there, navigate to the **Channels** tab in the left navigation.

Perform the following steps from your console:

1. Click the **Join channel** button to launch the side panels.
2. Select your ordering service named `Ordering Service` and click **Next**.
3. Enter the name of the channel you want to join, `channel1` and click **Next**.
4. Select which peers you want to join the channel. For purposes of this tutorial, click `Peer Org2`.
5. Leave the tab for **Make anchor peer** selected. It is a best practice for each organization to have at least one anchor peer on each channel, as anchor peers bootstrap the inter-organizational communication that enables features like [Private Data](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html){: external} and [Service Discovery](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external} to work. While it is only necessary to have one anchor peer on each channel, it does not hurt to make all peer anchor peers. The only downside will be a short term increase in the stress on your communication layer when new organizations join their peers to the channel, as these peers are designed to contact every anchor peer in every organization to find out about the peers belonging to that organization. Note that you can also make a peer an anchor peer later through the **Channels** tab.
6. Click **Join channel**.

If you plan to leverage the Hyperledger Fabric [Private Data](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html){: external} or [Service Discovery](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external} features, you must configure anchor peers in your organizations from the **Channels** tab. For more information about how to configure anchor peers for private data by using the **Channels** tab in console, see [Private data](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data).

In this tutorial, we are only creating and joining a single peer to the channel. As a result, you don't have to worry about a conflict between the database type used by your peer (which in this tutorial is CouchDB) and any other peers on the channel. However, in a production scenario, a best practice will be to ensure that the peer you are joining to this channel uses the same database type as other peers on the channel. For more information, see [LevelDB vs CouchDB](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-govern-components#ibp-console-govern-components-level-couch).
{:important}

## Creating a channel
{: #ibp-console-join-network-create-channel}

In this tutorial, we will presume that users will not be attempting to edit any of the advanced options. 
{:important}

Because your organization is now a member of the consortium, you also have the ability create a new channel. First, you will need to import the MSP definition of other channel members. Use the steps below to create a channel with two members: the `Org1` that was created in the [Build a network tutorial](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-build-network#ibp-console-build-network) and the `Org2` that was created in the steps above.

### Export your organization information
{: #ibp-console-join-network-add-org2-export-info}

This step needs to be completed by an administrator of Org1.
{:tip}

`Org1` needs to send you their organization MSP definition before you can add them to the channel. You need to be a member of the consortium hosted by the ordering service before can be added to the channel. In order to follow these steps you need to be the admin of the **peer organization**, meaning you have the peer organization admin identity in your Wallet:

First, navigate to the **Organizations** tab. You can see the organizations available for export are listed under **Available organizations**. Click the **Export** button inside the organization tile to download the JSON configuration file that represents the MSP of the peer organization.

Then, send this file to the consortium member that will create the channel in an out of band operation.

### Import the organization definition
{: #ibp-console-join-network-create-channel-import-msp}

You then need to import the MSP definition of `Org1` into your console, which you can do by navigating to the **Organizations** tab, clicking the **Import MSP definition** button, and selecting the JSON file that represents the peer organization MSP definition.



#### Creating the channel
{: #ibp-console-join-network-channels-create}

Perform the following steps from your console:

1. Click **Create channel**. A side panel will open.
2. Give the channel a **name**, `channel2`. Make a note of this value, as you will need to share it with anyone who wants to join this channel.
3. Select `Ordering Service` from the drop-down list.
4. Choose the **Organizations** who will be a part of this channel. Since we have two organizations, select and add `Org1 MSP (org1msp)` and then `Org2 MSP (org2msp)`. Make at least both organizations an **Operator**. Note: do not use the `Ordering Service MSP` here.
5. Choose a **Channel update policy** for the channel. This is the policy that will dictate how many organizations will have to approve updates to the channel configuration. You can select `1 out of 2`. As you add organizations to the channel, you should change this policy to reflect the needs of your use case. A sensible standard is to use a majority of organizations. For example, `3 of 5`.
6. Specify any **Access control** limitations you want to make. Note: this is an **advanced option**. If you set the access to a resource to a particular organization, it will restrict access to that resource for every organization. For example, if the default access to a particular resource is the `Readers` of all organizations, and that access is changed to the `Admin` of `Org2`, then **only** the admin of Org2 will have access to that resource. Because access to certain resources is fundamental to the smooth operation of a channel, it is highly recommended to make access control decisions carefully. If you decide to limit access to a resource, make sure that the access to that resource is added, as needed, for each organization.
7. Select the **Channel creator organization**. Since you are operating the tutorial as `Org2`, and have the `Org2 MSP Admin ` certificates in your console wallet, select `Org2 MSP` from the drop-down list. Likewise, choose `Org2 MSPAdmin ` as the identity creating the channel.

When you are ready, click **Create channel**. You are taken back to the Channels tab and you can see a pending tile of the channel that you just created.

**Task: Create a channel**

  |  **Field** | **Name** |
  | ------------------------- |-----------|
  | **Channel name** | channel2 |
  | **Ordering Service** | Ordering Service |
  | **Organizations** | Org2 MSP |
  | **Channel update policy** | 1 out of 2 |
  | **Access control list** | None |
  | **Channel creator organization** | Org2 MSP |
  | **Identity** | Org2 MSP Admin |
  {: caption="Table 8. Create a channel" caption-side="bottom"}



## Next steps
{: #ibp-console-join-network-next-steps}

After you have joined your peer to a channel, use the following steps to deploy a smart contract and begin submitting transactions to the blockchain:

- [Deploy a smart contract on your network](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-smart-contracts#ibp-console-smart-contracts) using the console.
- After you have installed and instantiated your smart contract, you can [submit transactions using your client application](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-smart-contracts#ibp-console-smart-contracts-connect-to-SDK).
- Use [the commercial paper sample](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-app#ibp-console-app-commercial-paper) to deploy an example smart contract and submit transactions from sample application code.

