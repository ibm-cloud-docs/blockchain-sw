---

copyright:
  years: 2019, 2020
lastupdated: "2020-02-07"

keywords: IBM Blockchain Platform, administrate, add user, remove user, password, APIs, authentication, view logs

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
{:curl: #curl .ph data-hd-programlang='curl'}

# Administering your console
{: #console-icp-manage}




After you install the console on your cluster, you can use the console to add or remove console users and access APIs that allow you to operate your network and govern your console. You can also access and customize the logs of your console.
{:shortdesc}


## Managing users from the console
{: #console-icp-manage-users}

The user who provisions the {{site.data.keyword.blockchainfull_notm}} Platform console is considered the console administrator. The administrator can then add and grant other users access to the console from the **Users** tab. Every user that accesses the console must be assigned an access policy with a user role defined. The policy determines what actions the user can perform within the console. By default, the console administrator is given the **Manager** role for the console. Other users can be assigned with **Manager**, **Writer**, or **Reader** roles when a console manager adds them to the console. Note that the users can also be managed with  [APIs](/docs/blockchain-sw?topic=blockchain-sw-ibp-v2-apis#console-icp-manage-users-apis).

### Role to permission mapping
{: #console-icp-manage-role-mapping}

| Role | Permissions |
|--------|----------|
| Manager | As a Manager, you have permissions beyond the Writer role. You can do everything a Reader and Writer can do as well as: <ul><li>Provision new components such as CAs, peers, and ordering services, by using the console or APIs.</li><li>Delete provisioned components by using the console or APIs.</li><li>Add/remove users and change user access policies.</li><li>Change console logging levels by using the console or APIs.</li><li>Restart the console by using an API.</li></ul> |
| Writer | As a Writer, you have permissions beyond the Reader role, including: <ul><li>Import components by using the console or APIs.</li><li>Remove imported components by using the console or APIs.</li><li>Register users on a CA.</li><li> Add or remove notifications by using the console or APIs.</li></ul>  |
| Reader | As a reader, you can perform read-only actions including: <ul><li>View console UI.</li><li>View console log.</li><li>Export components.</li><li>Issue any GET API.</li></ul> | |
{: caption="Table 1. Role to permissions mapping table" caption-side="bottom"}

Permissions are cumulative. If you select a **Manager** role, the user will also be able to perform all **Writer** and **Reader** actions, and you are not required to additionally check those roles. Likewise, a user with the **Writer** role will be able to perform all of the actions in the **Reader** role.

### Managing a user's password
{: #console-icp-manage-user-pw}




The password that you provided to the [console custom resource definition](/docs/blockchain-sw?topic=blockchain-sw-deploy-ocp#deploy-ocp-console) becomes the default password for the console. All users need to use this password when they log in to the console for the first time. The console manager can also specify a new default password. In the **Users** tab of the console, click the **Update configuration** link under your authentication service tile name. In the right panel, you can view or update the default password for new users of the console.


You need to share the default password, or the default password that you reset, with the users so that they can log in to the console. They will be required to change the password upon their first login.
{:note}

Users with the manager role can reset passwords for other users. In the **Users** tab of the console, click the three vertical dots at the end of the row for a specific user, and then click **Reset password**. The console resets that user's password to the default password, which you can check and confirm in the right panel. Users with any role can change their own password at any time. Click the avatar icon in the upper right corner, and then click **Change password**.

After you add new users to the console, the users might not be able to view all the nodes, channels, or chaincode, which other users deploy. To work with these components, each user needs to import the associated identities into their own console wallet. For more information, see [Storing identities in your console wallet](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-identities#ibp-console-identities-wallet).
{:important}

### Modifying a user's role
{: #console-icp-manage-reset-user-pw}

A user with a manager role can update the roles of other console users and provide them access to more or fewer console permissions. In the **Users** tab of the console, click the three vertical dots at the end of the specific user row, and then click **Update authenticated user**. Select the new role for the user in the right panel. The user's role will be updated when they log in to the console the next time.

### Deleting a user from the console
{: #console-icp-manage-icp-remove-user}

If you are a user with a manager role, you can remove a user's access to the console. In the **Users** tab of the console, click the checkbox at the beginning of the specific user row. Then, click the **Trash** can icon at the top of the table, next to the **Add new users** button. After you remove the user from the table, the user cannot log in to the console again.



## Viewing your logs
{: #icp-console-manage-logs}

When you use the {{site.data.keyword.blockchainfull_notm}} Platform console, you might need to view logs to debug an issue.

### Viewing your console logs
{: #console-icp-manage-console-logs}

You can easily access the console logs if you need to debug problems that you encounter when you use the console or operate your nodes. You can also set the logging level to increase or decrease the amount of logs that the console collects. The console logs are collected separately from the [node logs](#console-icp-manage-node-logs), which are collected by your Kubernetes cluster.  

Navigate to the **Settings** tab in the console browser to change the logging settings. The console logs are collected from two separate sources:

  * **Client logging:** These logs are collected when commands are sent from your browser to the console.
  * **Server logging:** These logs are collected when the console sends commands to your nodes and from the console deployment. These logs include the Hyperledger Fabric logging output.

Set the number of collected logs by using the drop-down list under each log type. For example, **Error** and **Warning** collect the least amount of logs, while **Debug** and **All** collect the most.

You can view only the console logs if you are logged in as a console administrator. To view the logs from the **Settings** tab, replace the word `settings` in the browser URL with `api/v1/logs`. For example, if your console url is `localhost:3001/settings`, you can view your logs by navigating to `localhost:3001/api/v1/logs`. Client and server logs are collected in separate files. The most recent logs are at the top of the page.

### Viewing your node logs
{: #console-icp-manage-node-logs}




Component logs can be viewed from the command line by using the [kubectl CLI commands](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_kubectl.html){: external} or through [Kibana](https://www.elastic.co/products/kibana){: external} which is included in the OpenShift Container Platform.


- Use the `kubectl logs` command to view the container logs inside the pod. Follow the instructions to [Install the kubectl cli](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_kubectl.html){: external} if you have not already done so. If you are unsure of your pod name, run the following command to view your list of pods.

  ```
  kubectl get pods
  ```
  {:codeblock}

  Then, run the following command to retrieve the logs for the node container that resides inside the pod:

  ```
  kubectl logs -f <pod_name> -c <node>
  ```
  {:codeblock}

  Replace `<pod_name>` with the name of your pod from the command output above.  
  Replace `<node>` with `ca`, `peer`, or `orderer` to view the logs for your node.  
  Replace `<node>` with  `chaincode-logs` to view the logs for your smart contracts.

  
  You can also run the command `kubectl logs -f <pod_name>` to get a list of all of the containers that are running inside a pod. The response to the command is an error message similar to the following:
  ```
  Error from server (BadRequest): a container name must be specified for pod peer1org1-7b4b6687dc-7n4fz, choose one of: [dind peer proxy chaincode-logs couchdb] or one of the init containers: [init]
  ```
  

  For more information about the `kubectl logs` command, see [Kubernetes documentation](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs){: external}.




- Alternatively, if you are running on OpenShift Container Platform, you can access the logs from the OpenShift Web Browser.

  **Note:** When you view your logs in Kibana, you might receive the response `No results found`. This condition can occur if your cluster uses your worker node IP address as its hostname. To resolve this problem, remove the filter that begins with `node.hostname.keyword` at the top of the panel and the logs will become visible.


### Viewing your smart contract container logs
{: #console-icp-manage-container-logs}

If you encounter issues with your smart contract, you can view the smart contract, or chaincode, container logs to debug an issue. You can run the following command to view the smart contract container logs:




```
kubectl  logs -f <peer_ped> -c chaincode-logs
```
{:codeblock}


Replace `<peer_pod>` with the name of the peer pod where the chaincode is running. Use the command `kubectl get po` to get the list of running pods.



