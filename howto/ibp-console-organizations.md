---

copyright:
  years: 2019, 2020
lastupdated: "2020-03-23"

keywords: organizations, MSPs, create an MSP, MSP JSON file, consortium, system channel

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


# Managing organizations
{: #ibp-console-organizations}

<div style="background-color: #f4f4f4; padding-left: 20px; border-bottom: 2px solid #0f62fe; padding-top: 12px; padding-bottom: 4px; margin-bottom: 16px; font-family: 'IBM Plex Sans', 'Helvetica Neue', Arial, sans-serif;">
  <p style="line-height: 10px;">
    <strong>Running a different version of IBM Blockchain Platform?</strong> Switch to version
    <a href="https://cloud.ibm.com/docs/blockchain-sw-213?topic=blockchain-sw-213-ibp-console-organizations">2.1.3</a>
    </p>
</div>

You can use the {{site.data.keyword.blockchainfull}} Platform console to create a formal organization definition known as a Membership Services Provider (MSP). Your organization's MSP definition allows other members of the blockchain consortium to verify the identity of your nodes and applications. Your MSP definition also contains your organization's admin certificates.

You can also use the console to manage which organizations are members of your network. The administrator of the ordering service can use the organizations tab to add members to the blockchain [consortium](/docs/blockchain-sw?topic=blockchain-sw-glossary#glossary-consortium). Members of the consortium can then use the console to add members to new or existing channels.

![{{site.data.keyword.blockchainfull_notm}} Platform console organizations tab](../images/console_organizations_tab.png "{{site.data.keyword.blockchainfull_notm}} Platform console organizations tab"){: caption="Figure 1. You can use the organizations panel to create, import, and manage organization MSP definitions" caption-side="bottom"}

**Target audience:** This topic is designed for network operators who are responsible for creating, monitoring, and managing the blockchain network.


## Understanding MSPs
{: #console-organizations-about-msp}

The {{site.data.keyword.blockchainfull_notm}} Platform is based on Hyperledger Fabric and builds permissioned blockchain networks. Participants need to be known to the network before they can submit transactions and interact with the assets on the ledger. Fabric recognizes identity through a group of invited organizations, known as the consortium. Organizations in the consortium are able to issue valid credentials to their members and let them become participants in the network. The participants can then operate blockchain nodes and submit transactions from client applications.

Each organization in the consortium needs to operate its own Certificate Authority, known as the root CA. This Certificate Authority (or its intermediate CAs) creates all the identities belonging to your organization and issues each identity a signing certificate and private key. These keys are signed by the CA and used by the members of your organization to sign and verify their actions. Joining the consortium allows other organizations to recognize your CA's signature, and verify that your peers and applications are valid participants. For more information about membership in Hyperledger Fabric, see the [Membership concept topic](https://hyperledger-fabric.readthedocs.io/en/release-1.4/membership/membership.html){: external} in the Fabric documentation.

Before your organization can join a consortium, it needs to create an organization definition known as a **Membership Services Provider (MSP)**. The MSP contains the following information:
- A certificate signed by your **root Certificate Authority**. This certificate is used to verify the identity of your nodes, channels, and applications.
- A certificate signed by your **TLS CA**. A root TLS certificate allows your peers to participate in cross organization gossip, which is necessary to take advantage of the [Private data collections](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html#what-is-a-private-data-collection){: external} and [Service Discovery](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external} features of Hyperledger Fabric.
- The **MSP ID**. The MSP ID is the formal name of your organization within the consortium. You need to remember the MSP ID for your nodes and applications.
- The **Admin certificates** of your **Organization Admin** identities. These certificates are passed to the Ordering Service and are used to verify which identities in your organization are allowed to create or edit channels. When you use your console to create an ordering node or peer, the admin certificates inside the MSP are deployed within the new node, making your organization admin identities your **Peer or Orderer Admins** as well. You can use these identities to operate your node, such as by installing a smart contract on a peer or joining a peer to a channel, from your console or a client application.

## Managing MSPs in the console
{: #ibp-console-organizations-manage}

Navigate to the **Organizations** tab. You can use this tab to [create an MSP definition](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-organizations#console-organizations-create-msp) by using a Certificate Authority that exists in your console. You can also use this tab to [import an MSP](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-organizations#console-organizations-import-msp) that was created by another organization.

You can view all of the MSPs that you have created or imported under **Available Organizations**. You can use the MSP definitions in the organizations tab for important functions within your console:
- If you are creating peer or orderer nodes, the MSP of your organization is used to provide admin certificates to the new node.
- If you are the admin of an ordering node, you can use the MSPs to [add new organizations to the consortium](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-organizations#console-organizations-add-consortium).
- If you are a member of the consortium, you can import the MSPs of other consortium members into your console and then add the members to new or existing channels.

You can click on an MSP definition in the organizations tab to view all of the nodes in the console that belong to each organization. Because each node was deployed with the administrator certificate from the MSP definition inside, this panel allows you to keep track of which nodes are managed by which organization administrator.


## Creating an MSP for your organization
{: #console-organizations-create-msp}

Use the **Organizations** tab to generate an MSP definition for your organization. When you click **Create MSP**, a side panel opens that allows you to enter all the necessary information: your Root CA, the MSP ID, and your organization administrators.

- Use the **MSP details** section of the panel to select your organizations MSP ID. This ID is the formal name that other members of the network will use to refer to your organization. **Save your MSP ID.**

- Use the **Root Certificate Authority details** section to select your organization's root CA. This is the CA that you used (or will use) to create all of the node and application identities that belong to your organization. If you use intermediate CAs, this is the CA that you used to create those CAs. Select your root CA from the list of CAs managed by using your console. If you created a CA using the console, selecting a root CA will also create a root TLS Certificate.

- You can also use the **Root Certificate Authority details** section to generate one of your organizations admin certificates. Before creating your organization MSP definition, you need to register your org and node admins with your root CA. You then need to complete the following steps in order to use these identities to operate your network:

  1. Create a signing certificate and private key for each admin identity.
  2. Provide the signing certificate of each admin identity in the MSP definition.
  3. Add the identity to your console wallet. The nodes or channels that are created by the console uses the certificates from the MSP to check who is a valid administrator. As a result, the same signing certificate and private key pair that you used to add an admin cert to the MSP needs to be stored inside your console wallet.

  You can use the **Create MSP definition** panel to complete these actions for one of your admin identities. After you select your root CA, complete the following steps in the **Generate organization admin certificate** section:
  1. Enter the Enroll ID and Enroll secret of an admin identity that is registered with your root CA. After you enter the Enroll ID and Enroll secret, choose a name to display the identity in your console wallet.
  2. Click **Generate**. This generates a certificate and private key and automatically add the keys to your console wallet. You can then find your admin identity in your wallet by using the name that you selected on this panel. These keys are only stored in your browser local storage. Therefore, if you change browsers, they will not be in your console wallet. You will need to export the identity from your original browser and import them into the console wallet of your new browser.
  3. Then, click **Export** to download the key pair to your file system and secure them.

- The **Administrators (Optional)** section of the side panel contains the signing certificates keys of your admins. The certificate that you generated by using the **Generate organization admin certificate** section can be found in the first row of the **admin certificate** field. If you want to use multiple admin identities to operate your network, you can paste the certificates of additional node or organization admins into **admin certificate** fields.

Because your admin certs are passed to your nodes and channels by using the MSP definition, you need to ensure that each of your node and org admin certificates are stored in the MSP. When you use the console to create an orderer, peer, or channel, you need to **Associate** one of the identities you exported in your console wallet with the admin certificates that were provided to the MSP definition. When you encounter an **Associate identity** section or panel, select an identity that you generated and saved to the wallet when creating the MSP definition.

After you have selected your root CA, MSP ID, and created your admin certs, click **Create MSP definition** to create the MSP definition. It is now listed as an organization in the Organizations tab. You will use the MSP definition when you deploy your nodes and join a blockchain consortium.

## Updating an organization MSP definition
{: #ibp-console-govern-update-msp}

It is possible that you need to update an organization MSP definition, such as the display name or the certificates that are included inside the MSP.  

It is recommended that you do not change the `msp_id` field as this might cause breaking changes to your network.
{: important}

- Simply export the existing MSP definition from the **Organizations** tab and edit the generated JSON file to modify the display name, the existing certificates, or add new certificates.
- Click your MSP definition in the **Organizations** tab to open it, click the **Settings** icon, and then click **Add file** to upload the new JSON file.
- Click **Update MSP definition**. The changes are effective immediately.

If you need to add a new admin certificate to an existing organization MSP definition, refer to the following tasks:
- Add the certificate of an additional member of the peer's organization who can list and install a smart contract on a peer. See this section for [instructions](#ibp-console-organizations-new-admins).
- Add the certificate of an additional member of the channel's operator organization who can instantiate a smart contract on an **existing** channel and can submit and approve updates to the channel. See this section for [instructions](#ibp-console-organizations-new-admins-existing-channel).

### Adding a new peer admin certificate
{: #ibp-console-organizations-new-admins}

Because peer administrators can change over time, you will need to update your peer admin certificates. You can add new peer administrators by updating the peer's organization MSP definition to include the admin certificates of additional admin identities. Recall that a peer admin identity is required to install a smart contract on a peer and list the smart contracts that are already installed on the peer.

When you update an MSP organization definition with a new admin cert, the associated peer nodes are also updated with the modified MSP definition. This update process includes an automatic restart of the associated peer nodes which means they will be unavailable for a brief period of time while they restart. If client applications are sending transactions to the peers during this period, the transactions might need to be resubmitted. Note that imported peer nodes, or peers that were not created by using your console, will not be updated.
{: important}

#### Before you begin
{: #ibp-console-organizations-new-admins-before}

In order to complete this process, you need to register and enroll the new peer admin identity with the same CA that the existing peer admin was registered with.

1. Follow the steps to [register a new peer admin identity](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-identities#ibp-console-identities-register).
2. Follow the steps to [enroll the new admin identity](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-identities#ibp-console-identities-enroll) which generates the Certificate and private key for the new admin identity.  Be sure to download the generated Certificate and private key PEM files to your file system and add the identity to your console wallet.
3. You need to convert the Certificate string from PEM format to base64 format by running the following command:

```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
cat <certificate.pem> | base64 $FLAG
```
{: codeblock}

Replace `<certificate.pem>` with the name of the Certificate PEM file that you downloaded.

#### Updating the peer organization MSP definition
{: #ibp-console-organizations-new-admins-steps}

1. Open the **Nodes** tab in your console.
2. Locate the peer node that requires the admin certificate update. The peer organization MSP name is listed on the tile. Make a note of the MSP name.
3. Open the **Organizations** tab.
4. Locate the MSP tile for the peer and click the **Export** icon.
5. Open the downloaded MSP JSON file in a text editor.
6. Edit the `admins` element. Paste the new base64-encoded certificate string, that you generated in the previous section, to the end of the list of comma-separated admin certificates.
7. Save your changes.
8. In the **Organizations** tab, open the MSP tile for the peer and click the **Settings** icon.
9. In the side panel, click **Add file** and select the updated MSP JSON file.
10. Click **Update MSP definition**.

### Adding a new channel admin certificate
{: #ibp-console-organizations-new-admins-existing-channel}

Follow these instructions when you need to add a new admin certificate of another member from the same organization who can instantiate a smart contract on an existing channel and can submit and approve channel updates.  This task requires you to download and edit the existing MSP definition and add the new admin certificate and then update the MSP for the channel member.

If you need to add a new organization as a channel admin, see this topic on [Updating a channel configuration](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-govern#ibp-console-govern-update-channel).
{: note}

As with all channel updates, this update needs to be performed by a channel operator and the change will follow the channel update approval process according to the policy that was configured when the channel was created.  

1. In the **Organizations** tab, locate the existing organization MSP definition that you want to update and click its tile to open it.
2. Click **Export** to save the definition to a JSON file on your file system and open file in a text editor.
3. Edit the `admins` element. Paste the new admin certificate to the end of the list of comma-separated admin certificates.  
4. Open the MSP definition in the console and click the **Settings** icon.  
5. Click **Add file** and browse to the updated JSON file.
6. Click **Update MSP definition**.
7. Open the channel to be updated in the console.
8. Click **Channel details** and scroll down to **Channel members**.
9. Click the channel member that you want to update.
10. With the **Existing MSP ID tab** selected browse to the updated MSP.
11. Click **Update MSP definition**.

## Manually building an MSP JSON file
{: #console-organizations-build-msp}

**This option is for advanced users only who are familiar with how certificates are used in blockchain identity management.**

If you prefer to use certificates for your peer or ordering service from an **external CA**, one that is not hosted by {{site.data.keyword.IBM_notm}}, you need to build an MSP definition JSON file that represents the peer or ordering service organization MSP definition.

Note that all certificates must be encoded in base64 format.
{:important}

You can convert the contents of your certificate file, `<cert.pem>` from `PEM` format into a base64 string by running the following command on your local machine:

```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
cat <cert.pem> | base64 $FLAG
```
{:codeblock}


Create a JSON file by using the following format:

```json
{
    "name": "<organization_name>",
    "msp_id": "<organization_id>",
    "type": "msp",
    "root_certs": [
        "<root_certs>"
    ],
     "intermediate_certs": [
         "<intermediate_certs>"
     ],
    "admins": [
        "<admins>"
    ],
    "tls_root_certs": [
        "<tls_root_certs>"
    ],
    "tls_intermediate_certs": [
        "<tls_intermediate_certs>"
    ],
    "host_url": "<host_url>"
}
```
{:codeblock}

- **organization_name**: Specify any name to be used to identify this MSP definition in the console.
- **organization_id**: Specify an ID that is used to represent this MSP internally in the console.
- **root_certs**: (Optional) Paste in an array that contains one or more root certificates from the external CA in `base64` format. You must provide either a CA root certificate or an intermediate CA certificate, you can also provide both.
- **intermediate_certs**: (Optional) Paste in an array that contains one or more certificates from the external intermediate CA in `base64` format. You must provide either a CA root certificate or an intermediate CA certificate, you can also provide both.
- **admins**: Paste in the signing certificate of the organization admin in `base64` format.
- **tls_root_certs**: (Optional) Paste in an array that contains one or more root certificates from the TLS CA in `base64` format. You must provide either an external TLS CA root certificate or an external intermediate TLS CA certificate, you can also provide both.
- **tls_intermediate_certs**: (Optional) Paste in an array that contains one or more certificates from the intermediate TLS CA in `base64` format. You must provide either an external TLS CA root certificate or an external intermediate TLS CA certificate, you can also provide both.
- **host_url**: Specify the URL of the blockchain console. This URL is used for signature collection.

The following additional fields are also available in your MSP definition but are not required:
- **organizational_unit_identifiers**: A list of Organizational Units (OU) that valid members of this MSP should include in their X.509 certificate. This is an optional configuration parameter that is used when multiple organizations leverage the same root of trust and intermediate CAs, and they have reserved an OU field for their members. An organization is often divided up into multiple organizational units (OUs), each of which has a certain set of responsibilities. For example, the ORG1 organization might have both ORG1-MANUFACTURING and ORG1-DISTRIBUTION OUs to reflect these separate lines of business. When a CA issues X.509 certificates, the OU field in the certificate specifies the line of business to which the identity belongs. See this topic in the Fabric documentation on [Identity Classification](https://hyperledger-fabric.readthedocs.io/en/latest/msp.html#identity-classification){: external} for more information.  
- **fabric_node_OUs**: Fabric-specific OUs that enable identity classification. `NodeOUs` contain information on how to distinguish clients, peers, and orderers based on their OU. If the check is enforced, by setting Enabled to true, the MSP considers an identity valid if it is an identity of a `client`, a `peer` or an `orderer`. An identity should have only one of these special OUs. See this topic for an example of [how to specify the `fabric_node_OU` in an MSP](https://hyperledger-fabric.readthedocs.io/en/latest/discovery-cli.html#configuration-query){: external} in the Fabric Service Discovery documentation.
- **revocation_list**: A list of certificates that are no longer valid. For X.509-based identities, these identifiers are pairs of strings known as Subject Key Identifier (SKI) and Authority Key Identifier (AKI), and are checked whenever the X.509 certificate is being used to make sure that the certificate has not been revoked. See this topic in the Fabric documentation for more information about [Certificate Revocation Lists](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/users-guide.html?highlight=revocation%20list#revoking-a-certificate-or-identity){: external}.

For example, your JSON file would look similar to:

```json
{
    "name": "Org1 MSP",
    "msp_id": "org1msp",
    "type": "msp",
    "root_certs": [
        "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGakNDQWIyZ0F3SUJBZ0lVZFhUcWNZVVhRS3U3WHVQWmcxUHBsekpFVFlNd0NnWUlLb1pJemowRUF3SXcKYURFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTS0K"
    ],
    "admins": [
        "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlDWHpDQ0FnYWdBd0lCQWdJVVp6NFdQdWwxRXRVOUNIcTl4NFg0Y2QwakNpNHdDZ1lJS29aSXpqMEVBd0l3DQphREVMTUFrR0ExVUVCaE1DVlZNeEZ6QVZCZ05WQkFnVERrNXZjblJvSUVOaGNtOXNhVzVoTVJRd0VnWURWUVFLDQpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbFLS0tLS0NCg=="
    ],
    "tls_root_certs": [
        "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNHRENDQWI2Z0F3SUJBZ0lVTzVhWU9WbjNwTkRMZGVLTFlIanRIUEtNTnY4d0NnWUlLb1pJemowRUF3SXcKWFRFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVE0d0RBWamd5TURRM01EQmFNRjB4Q3pBSkJnTlZCQVlUQWxWVE1SY3cKRlFZRFZRUUlFdztLS0K"
    ],
    "host_url": "https://d0db6db7-optools.uss02.blockchain.cloud.ibm.com"
}
```
{:codeblock}

Save this definition as your MSP definition `JSON` file.  


You have constructed an MSP definition, which defines the organization for your peer or ordering service nodes, and uses certificates from an external CA. You can now return to the instructions that describe [How to use certificates from an external CA with your peer or ordering node](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-govern-components#ibp-console-govern-third-party-ca).

## Importing an MSP
{: #console-organizations-import-msp}

Only the orderer admin can add new organizations to the consortium. If you are the orderer admin, you will need to collect the MSP definitions of all the organizations who have been invited to the consortium and import the MSPs into the console. You can then add the MSPs to the ordering service, by using the orderer node.

After an administrator creates an MSP definition, they can use the Organizations tab to download the MSP in JSON format to their local file system. They can then send you the MSP JSON file in an out of band operation. Navigate to the **Organizations** tab and use **Import MSP Definition** to import the MSP file into your console. Once an MSP definition is visible in the **Available organizations** section, you can then navigate to your orderer node to [add the organization to the consortium](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-organizations#console-organizations-add-consortium).

## Adding an organization to a consortium
{: #console-organizations-add-consortium}

The consortium of organizations is hosted by the ordering service.

If you are the administrator of the ordering service, you can use the console to add an organization to the consortium. Navigate to the **Nodes** tab and click on the ordering node. On the ordering node panel, under **Consortium members**, click **Add organization**. This opens a side panel that allows you to select from the list of available MSP definitions that you have [imported into your organizations tab](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-organizations#console-organizations-import-msp). You can also use the **Upload JSON** option to import the MSP definition file created by another org directly.

## Creating and editing a channel
{: #console-organizations-create-channel}

After an organization is added to the consortium, the organization can use the ordering service to create a new channel or can be added to an existing channel. The information that allows you to participate in a channel, such as joining your peers to the channel, instantiating smart contracts, and submitting transactions, is provided by using the MSP definitions.

After your organization is added to a consortium, you can create a channel by using the following steps:

1. Import the ordering node that hosts the consortium into your console. You do not need to be an administrator of the ordering node; but you need to provide the orderer node name and endpoint information in your console.
2. Import the MSPs of organizations that you want to add to the new channel into your console in the **Organizations** tab. **Note** that organizations need to be added to the consortium before they can be added to a channel.
3. Navigate to the **Channels** tab and click **Create channel**. This opens a side panel that allows you to specify the channel name, membership, and channel policies. You can add any organizations that have been added to the consortium to the new channel.

For more information about these steps, see [creating a channel](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-build-network#ibp-console-build-network-create-channel1) in the **Build a network** tutorial.

### Updating an MSP in a channel definition
{: #console-organizations-update-channel}

Over time you might need to update the certificates in an MSP definition that is already associated with a channel. When that situation occurs follow these steps to update an organization's MSP definition in the channel:  

1. Navigate to the **Channels** tab in your console.
2. Click the channel that contains the organization MSP that you want to update and open it.
3. Click the **Channel details** tab.
4. Click the associated channel member's tile that you want to update.
5. If you have not already imported the updated MSP definition into the console, you can upload the file here. **Note:** This action will not update the associated MSP definition in the Organizations tab. If you have already updated the MSP definition in the Organizations tab of the console, you can select it from the drop-down list.
