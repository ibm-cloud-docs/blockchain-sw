---

copyright:
  years: 2018, 2020
lastupdated: "2020-02-21"

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

# Deploying {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2
{: #deploy-ocp}


You can use the following instructions to deploy the {{site.data.keyword.blockchainfull}} Platform v2.1.2 onto a Kubernetes cluster that is running on OpenShift Container Platform 3.11, 4.1, or 4.2. The {{site.data.keyword.blockchainfull_notm}} Platform uses a [Kubernetes Operator](https://www.openshift.com/learn/topics/operators){: external} to install the {{site.data.keyword.blockchainfull_notm}} Platform console on your cluster and manage the deployment and your blockchain nodes. When the {{site.data.keyword.blockchainfull_notm}} Platform console is running on your cluster, you can use the console to create blockchain nodes and operate a multicloud blockchain network.
{:shortdesc}

The following diagram shows the steps that a cluster administrator needs to take on the OpenShift Container Platform to deploy the {{site.data.keyword.blockchainfull_notm}} Platform.

![{{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 deployment overview](../images/OCP_deploy_flow.svg "{{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 deployment overview"){: caption="Figure 1. Deployment process on OpenShift"  caption-side="bottom"}

## Resources required
{: #deploy-ocp-resources-required}

Ensure that your OpenShift cluster has sufficient resources for the {{site.data.keyword.blockchainfull_notm}} console and for the blockchain nodes that you create. The amount of resources that are required can vary depending on your infrastructure, network design, and performance requirements. To help you deploy a cluster of the appropriate size, the default CPU, memory, and storage requirements for each component type are provided in this table. Your actual resource allocations are visible in your blockchain console when you deploy a node and can be adjusted at deployment time or after deployment according to your business needs.

| **Component** (all containers) | CPU**  | Memory (GB) | Storage (GB) |
|--------------------------------|---------------|-----------------------|------------------------|
| **Peer**                       | 1.1           | 2.8                   | 200 (includes 100 GB for peer and 100 GB for state DB)|
| **CA**                         | 0.1           | 0.2                   | 20                     |
| **Ordering node**              | 0.35          | 0.7                   | 100                    |
| **Console**                    | 1.2           | 2.4                   | 10                     |
| **Operator**                   | 0.1           | 0.2                   | 0                      |
{: caption="Table 1. Default resource allocations" caption-side="bottom"}
** These values can vary slightly. Actual VPC allocations are visible in the blockchain console when a node is deployed.  

If you are deploying the OpenShift platform on {{site.data.keyword.cloud_notm}}, you must create a 4x16 cluster with multiple nodes. This cluster provides sufficient resources to get started by deploying a basic network. Note that one core in OpenShift is equivalent to one CPU (or vCPU) in {{site.data.keyword.blockchainfull_notm}} Platform.
{: important}

## Browsers
{: #deploy-ocp-browsers}
The {{site.data.keyword.blockchainfull_notm}} Platform console has been successfully tested on the following browsers:

- Chrome: Version 78.0.3904.70 (Official Build) (64-bit)
- Firefox (non-ESR): Version 69.0.1
- Safari: Version 13.0 (14608.1.49)
- Edge: v44.17763.1.0

## Storage
{: #deploy-ocp-storage}

{{site.data.keyword.blockchainfull_notm}} Platform requires persistent storage for each CA, peer, and ordering node that you deploy, in addition to the storage required by the {{site.data.keyword.blockchainfull_notm}} console. The {{site.data.keyword.blockchainfull_notm}} Platform console uses [dynamic provisioning](https://docs.openshift.com/container-platform/4.2/install_config/persistent_storage/dynamically_provisioning_pvs.html#basic-spec-definition){: external} to allocate storage for each blockchain node that you deploy by using a pre-defined storage class. You have the opportunity to choose your persistent storage from the available storage options for the OpenShift Container Platform.

Before you deploy the {{site.data.keyword.blockchainfull_notm}} Platform console, you must create a storage class with enough backing storage for the {{site.data.keyword.blockchainfull_notm}} console and the nodes that you create. You can set this storage class to the default storage class of your Kubernetes cluster or create a new class that is used by the {{site.data.keyword.blockchainfull_notm}} Platform console. If you are using a multizone cluster in OpenShift Container Platform, then you must configure the default storage class for each zone. After you create the storage class, run the command `kubectl patch storageclass` to set the storage class of the multizone region to be the default storage class.

If you prefer not to choose a persistent storage option, the default storage class of your OpenShift project is used. `Host-local` volume storage is not supported.
{: note}

## Get your entitlement key
{: #deploy-ocp-entitlement-key}

When you purchase the {{site.data.keyword.blockchainfull_notm}} Platform from PPA, you receive an entitlement key for the software is associated with your MyIBM account. You need to access and save this key to deploy the platform.

1. Log in to [MyIBM Container Software Library](https://myibm.ibm.com/products-services/containerlibrary){: external} with the IBMid and password that are associated with the entitled software.

2. In the Entitlement keys section, select **Copy key** to copy the entitlement key to the clipboard. Save this value for use later during deployment.

## Before you begin
{: #deploy-ocp-prerequisites}

1. The {{site.data.keyword.blockchainfull_notm}} Platform can be installed only on the [OpenShift Container Platform 3.11, 4.1, or 4.2](https://docs.openshift.com/container-platform/3.11/welcome/index.html){: external}.

2. You need to install and connect to your cluster by using the [OpenShift Container Platform CLI](https://docs.openshift.com/container-platform/4.2/cli_reference/get_started_cli.html#installing-the-cli){: external} to deploy the platform. If you are using an OpenShift cluster that was deployed with the {{site.data.keyword.IBM_notm}} Kubernetes Service, use these instructions to [Install the OpenShift Origin CLI](/docs/openshift?topic=openshift-openshift-cli#cli_oc).

## Log in to your OpenShift cluster
{: #deploy-ocp-login}

Before you can complete the next steps, you need to log in to your cluster by using the OpenShift CLI. You can log in to your cluster by using the OpenShift web console.

1. Open the OpenShift web console. If you are using the {{site.data.keyword.IBM_notm}} Kubernetes Service, you can go to your cluster by using the [{{site.data.keyword.cloud_notm}} dashboard](https://cloud.ibm.com/kubernetes/clusters){: external}. In the upper right corner of the cluster overview page, click **OpenShift web console**.

2. From the web console, click the dropdown menu in the upper right corner and then click **Copy Login Command**. Paste the copied command in your terminal window.

The command looks similar to the following example:
```
oc login https://c100-e.us-south.containers.cloud.ibm.com:31394 --token=<TOKEN>
```
If the command is successful, you can see the list of the projects in your cluster in your terminal by running the following command:
```
oc get pods
```
{:codeblock}

If successful, you can see the pods that are running in your default namespace:
```
docker-registry-7d8875c7c5-5fv5j    1/1       Running   0          7d
docker-registry-7d8875c7c5-x8dfq    1/1       Running   0          7d
registry-console-6c74fc45f9-nl5nw   1/1       Running   0          7d
router-6cc88df47c-hqjmk             1/1       Running   0          7d
router-6cc88df47c-mwzbq             1/1       Running   0          7d
```

When you connect to your cluster by using the OpenShift CLI, you also connect by using the `kubectl` CLI. You can find the same pods by running the equivalent `kubectl` command:
```
kubectl get pods
```
{:codeblock}

## Create a new project
{: #deploy-ocp-project}

After you connect to your cluster, create a new project for your deployment of {{site.data.keyword.blockchainfull_notm}} Platform. You can create a new project by using the OpenShift web console or OpenShift CLI. The new project needs to be created by a cluster administrator.

If you are using the CLI, create a new project by the following command:
```
oc new-project <PROJECT_NAME>
```
{:codeblock}

Replace `<PROJECT_NAME>` with the name of your project.

It is required that you create a new OpenShift project for each blockchain network that you deploy with the {{site.data.keyword.blockchainfull_notm}} Platform. For example, if you plan to create different networks for development, staging, and production, then you need to create a unique project for each environment. Each project creates a new Kubernetes namespace. Using a separate namespace provides each network with separate resources and allows you to set unique access policies for each network. You need to follow these deployment instructions to deploy a separate operator and console for each project.
{: important}

When you create a new project, a new namespace is created with the same name as your project. You can verify that the existence of the new namespace by using the `oc get namespace` command:
```
$ oc get namespace
NAME                                STATUS    AGE
blockchain-project                  Active    2m
```

You can also use the CLI to find the available storage classes for your namespace. If you created a new storage class for your deployment, that storage class must be visible in the output in the following command:
```
oc get storageclasses
```
{:codeblock}

## Add security and access policies
{: #deploy-ocp-scc}

The {{site.data.keyword.blockchainfull_notm}} Platform requires specific security and access policies to be added to your project. The contents of a set of `.yaml` files are provided here for you to copy and edit to define the security policies for your project. You must save these files to your local system and then add them your project by using the OpenShift CLI. These steps need to be completed by a cluster administrator. Also, be aware that the peer `init` and `dind` containers that get deployed are required to run in privileged mode.

### Apply the Security Context Constraint

Copy the security context constraint object below and save it to your local system as `ibp-scc.yaml`. Edit the file and replace `<PROJECT_NAME>` with the name of your project.

```yaml
allowHostDirVolumePlugin: true
allowHostIPC: true
allowHostNetwork: true
allowHostPID: true
allowHostPorts: true
allowPrivilegeEscalation: true
allowPrivilegedContainer: true
allowedCapabilities:
- NET_BIND_SERVICE
- CHOWN
- DAC_OVERRIDE
- SETGID
- SETUID
- FOWNER
apiVersion: security.openshift.io/v1
defaultAddCapabilities: null
fsGroup:
  type: RunAsAny
groups:
- system:cluster-admins
- system:authenticated
kind: SecurityContextConstraints
metadata:
  name: <PROJECT_NAME>
readOnlyRootFilesystem: false
requiredDropCapabilities: null
runAsUser:
  type: RunAsAny
seLinuxContext:
  type: RunAsAny
supplementalGroups:
  type: RunAsAny
volumes:
- "*"
```
{:codeblock}

After you save and edit the file, run the following commands to add the file to your cluster and add the policy to your project. Replace `<PROJECT_NAME>` with your project.
```
oc apply -f ibp-scc.yaml
oc adm policy add-scc-to-user <PROJECT_NAME> system:serviceaccounts:<PROJECT_NAME>
```
{:codeblock}

If the command is successful, you can see a response that is similar to the following example:
```
securitycontextconstraints.security.openshift.io/blockchain-project created
scc "blockchain-project" added to: ["system:serviceaccounts:blockchain-project"]
```

### Apply the ClusterRole

Copy the following text to a file on your local system and save the file as `ibp-clusterrole.yaml`. This file defines the required ClusterRole for the PodSecurityPolicy. Edit the file and replace `<PROJECT_NAME>` with the name of your project.

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

If successful, you can see a response that is similar to the following example:
```
clusterrole.rbac.authorization.k8s.io/blockchain-project created
scc "blockchain-project" added to groups: ["system:serviceaccounts:blockchain-project"]
```

### Apply the ClusterRoleBinding

Copy the following text to a file on your local system and save the file as `ibp-clusterrolebinding.yaml`. This file defines the ClusterRoleBinding. Edit the file and replace `<PROJECT_NAME>` with the name of your project.

```yaml
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: <PROJECT_NAME>
subjects:
- kind: ServiceAccount
  name: default
  namespace: <PROJECT_NAME>
roleRef:
  kind: ClusterRole
  name: <PROJECT_NAME>
  apiGroup: rbac.authorization.k8s.io
```
{:codeblock}

After you save and edit the file, run the following commands. Replace `<PROJECT_NAME>` with your project.
```
oc apply -f ibp-clusterrolebinding.yaml
oc adm policy add-cluster-role-to-user <PROJECT_NAME> system:serviceaccounts:<PROJECT_NAME>
```
{:codeblock}

If successful, you can see a response that is similar to the following example:
```
clusterrolebinding.rbac.authorization.k8s.io/blockchain-project created
cluster role "blockchain-project" added: "system:serviceaccounts:blockchain-project"
```

## Create a secret for your entitlement key
{: #deploy-ocp-docker-registry-secret}

After you purchase the {{site.data.keyword.blockchainfull_notm}} Platform, you can access the [My IBM dashboard](https://myibm.ibm.com/dashboard/){: external} to obtain your entitlement key for the offering. You need to store the entitlement key on your cluster by creating a [Kubernetes Secret](https://kubernetes.io/docs/concepts/configuration/secret/){: external}. Using a Kubernetes secret allows you to securely store the key on your cluster and pass it to the operator and the console deployments.

Run the following command to create the secret and add it to your OpenShift Project:
```
kubectl create secret docker-registry docker-key-secret --docker-server=cp.icr.io --docker-username=cp --docker-password=<KEY> --docker-email=<EMAIL> -n <PROJECT_NAME>
```
{:codeblock}
- Replace `<KEY>` with your entitlement key.
- Replace `<EMAIL>` with your email address.
- Replace `<PROJECT_NAME>` with the name of your OpenShift Container Platform project.

The name of the secret that you are creating is `docker-key-secret`. This value is used by the operator to deploy the offering in future steps. If you change the name of any of secrets that you create, you need to change the corresponding name in future steps.
{: note}

## Deploy the {{site.data.keyword.blockchainfull_notm}} Platform operator
{: #deploy-ocp-operator}

The {{site.data.keyword.blockchainfull_notm}} Platform uses an operator to install the {{site.data.keyword.blockchainfull_notm}} Platform console. You can deploy the operator on your cluster by adding a custom resource to your project by using the OpenShift CLI. The custom resource pulls the operator image from the Docker registry and starts it on your cluster.

Copy the following text to a file on your local system and save the file as `ibp-operator.yaml`. If you changed the name of the Docker key secret, then you need to edit the field of `name: docker-key-secret`.
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ibp-operator
  labels:
    release: "operator"
    helm.sh/chart: "ibm-ibp"
    app.kubernetes.io/name: "ibp"
    app.kubernetes.io/instance: "ibpoperator"
    app.kubernetes.io/managed-by: "ibp-operator"
spec:
  replicas: 1
  strategy:
    type: "Recreate"
  selector:
    matchLabels:
      name: ibp-operator
  template:
    metadata:
      labels:
        name: ibp-operator
        release: "operator"
        helm.sh/chart: "ibm-ibp"
        app.kubernetes.io/name: "ibp"
        app.kubernetes.io/instance: "ibpoperator"
        app.kubernetes.io/managed-by: "ibp-operator"
      annotations:
        productName: "IBM Blockchain Platform"
        productID: "54283fa24f1a4e8589964e6e92626ec4"
        productVersion: "2.1.2"
    spec:
      hostIPC: false
      hostNetwork: false
      hostPID: false
      serviceAccountName: default
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: beta.kubernetes.io/arch
                operator: In
                values:
                - amd64
      imagePullSecrets:
        - name: docker-key-secret
      containers:
        - name: ibp-operator
          image: cp.icr.io/cp/ibp-operator:2.1.2-20200213-amd64
          command:
          - ibp-operator
          imagePullPolicy: Always
          securityContext:
            privileged: false
            allowPrivilegeEscalation: false
            readOnlyRootFilesystem: false
            runAsNonRoot: false
            runAsUser: 1001
            capabilities:
              drop:
              - ALL
              add:
              - CHOWN
              - FOWNER
          livenessProbe:
            tcpSocket:
              port: 8383
            initialDelaySeconds: 10
            timeoutSeconds: 5
            failureThreshold: 5
          readinessProbe:
            tcpSocket:
              port: 8383
            initialDelaySeconds: 10
            timeoutSeconds: 5
            periodSeconds: 5
          env:
            - name: WATCH_NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
            - name: POD_NAME
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name
            - name: OPERATOR_NAME
              value: "ibp-operator"
            - name: CLUSTERTYPE
              value: OPENSHIFT
          resources:
            requests:
              cpu: 100m
              memory: 200Mi
            limits:
              cpu: 100m
              memory: 200Mi
```
{:codeblock}

Then, use the `kubectl` CLI to add the custom resource to your project.

```
kubectl apply -f ibp-operator.yaml -n <PROJECT_NAME>
```
{:codeblock}

You can confirm that the operator deployed by running the command `kubectl get deployment -n <PROJECT_NAME>`. If your operator deployment is successful, then you can see the following tables with four ones displayed. The operator takes about a minute to deploy.
```
NAME           READY     UP-TO-DATE   AVAILABLE   AGE
ibp-operator   1/1       1            1           46s
```

## Deploy the {{site.data.keyword.blockchainfull_notm}} Platform console
{: #deploy-ocp-console}

When the operator is running on your namespace, you can apply a custom resource to start the {{site.data.keyword.blockchainfull_notm}} Platform console on your cluster. You can then access the console from your browser. Note that you can deploy only one console per OpenShift project.

Save the custom resource definition below as `ibp-console.yaml` on your local system. If you changed the name of the entitlement key secret, then you need to edit the field of `name: docker-key-secret`.

```yaml
apiVersion: ibp.com/v1alpha1
kind: IBPConsole
metadata:
  name: ibpconsole
spec:
  license: accept
  serviceAccountName: default
  email: "<EMAIL>"
  password: "<PASSWORD>"
  registryURL: cp.icr.io/cp
  imagePullSecret: "docker-key-secret"
  networkinfo:
    domain: <DOMAIN>
  storage:
    console:
      class: default
      size: 10Gi
```
{:codeblock}

You need to specify the external endpoint information of the console in the `ibp-console.yaml` file:
- Replace `<DOMAIN>` with the name of your cluster domain. You can find this value by using the OpenShift web console. Use the dropdown menu next to **OpenShift Container Platform** at the top of the page to switch from **Service Catalog** to **Cluster Console**. Examine the url for that page. It will be similar to `console.xyz.abc.com/k8s/cluster/projects`. The value of the domain then would be `xyz.abc.com`, after removing `console` and `/k8s/cluster/projects`.

You need to provide the user name and password that is used to access the console for the first time:
- Replace `<EMAIL>` with the email address of the console administrator.
- Replace `<PASSWORD>` with the password of your choice. This password also becomes the default password of the console until it is changed.

You also need to make additional edits to the file depending on your choices in the deployment process:
- If you changed the name of your Docker key secret, change corresponding value of the `imagePullSecret:` field.
- If you created a new storage class for your network, provide the storage class that you created to the `class:` field.

If you are running OpenShift on Azure, you also need to change the storage class from `default` to `azure-standard`, unless you created your own storage class.
{: tip}

Because you can only run the following command once, you should review the [Advanced deployment options](#console-deploy-ocp-advanced) in case any of the options are relevant to your configuration, before you install the console.  For example, if you are deploying your console on a multizone cluster, you need to configure that before you run the following step to install the console.
{: important}

After you update the file, you can use the CLI to install the console.
```
kubectl apply -f ibp-console.yaml -n <PROJECT_NAME>
```
{:codeblock}

Replace `<PROJECT_NAME>` with the name of your project. Before you install the console, you might want to review the advanced deployment options in the next section. The console can take a few minutes to deploy.

### Advanced deployment options
{: #console-deploy-ocp-advanced}

Before you deploy the console, you can edit the `ibp-console.yaml` file to allocate more resources to your console or use zones for high availability in a multizone cluster. To take advantage of these deployment options, you can use the console resource definition with the `resources:` and `clusterdata:` sections added:

```yaml
apiVersion: ibp.com/v1alpha1
kind: IBPConsole
metadata:
  name: ibpconsole
  spec:
    license: accept
    serviceAccountName: default
    proxyIP:
    email: "<EMAIL>"
    password: "<PASSWORD>"
    registryURL: cp.icr.io/cp
    imagePullSecret: "docker-key-secret"
    networkinfo:
        domain: <DOMAIN>
    storage:
      console:
        class: default
        size: 10Gi
    clusterdata:
      zones:
    resources:
      console:
        requests:
          cpu: 500m
          memory: 1000Mi
        limits:
          cpu: 500m
          memory: 1000Mi
      configtxlator:
        limits:
          cpu: 25m
          memory: 50Mi
        requests:
          cpu: 25m
          memory: 50Mi
      couchdb:
        limits:
          cpu: 500m
          memory: 1000Mi
        requests:
          cpu: 500m
          memory: 1000Mi
      deployer:
        limits:
          cpu: 100m
          memory: 200Mi
        requests:
          cpu: 100m
          memory: 200Mi
```
{:codeblock}

- You can use the `resources:` section to allocate more resources to your console. The values in the example file are the default values allocated to each container. Allocating more resources to your console allows you to operate a larger number of nodes or channels. You can allocate more resources to a currently running console by editing the resource file and applying it to your cluster. The console will restart and return to its previous state, allowing you to operate all of your exiting nodes and channels.
  ```
  kubectl apply -f ibp-console.yaml -n <PROJECT_NAME>
  ```
  {:codeblock}

- If you plan to use the console with a multizone Kubernetes cluster, you need to add the zones to the `clusterdata.zones:` section of the file. When zones are provided to the deployment, you can select the zone that a node is deployed to using the console or the APIs. As an example, if you are deploying to a cluster across the zones of dal10, dal12, and dal13, you would add the zones to the file by using the format below.
  ```yaml
  clusterdata:
    zones:
      - dal10
      - dal12
      - dal13
  ```
  {:codeblock}

  When you finish editing the file, apply it to your cluster.
  ```
  kubectl apply -f ibp-console.yaml -n <PROJECT_NAME>
  ```
  {:codeblock}

Unlike the resource allocation, you cannot add zones to a running network. If you have already deployed a console and used it to create nodes on your cluster, you will lose your previous work. After the console restarts, you need to deploy new nodes.    
{: Important}

### Use your own TLS Certificates (Optional)

The {{site.data.keyword.blockchainfull_notm}} Platform console uses TLS certificates to secure the communication between the console and your blockchain nodes and between the console and your browser. You have the option of creating your own TLS certificates and providing them to the console by using creating a Kubernetes secret. If you skip this step, the console creates its own self-signed TLS certificates during deployment.

You can use a Certificate Authority or tool to create the TLS certificates for the console. The TLS certificate needs to include the hostname of the console and the proxy in the subject name or the alternative domain names. The console and proxy hostname are in the following format:

**Console hostname:** ``<PROJECT_NAME>-ibpconsole-console.<DOMAIN>``  
**Proxy hostname:** ``<PROJECT_NAME>-ibpconsole-proxy.<DOMAIN>``

- Replace `<PROJECT_NAME>` with the name of the OpenShift project that you created.
- Replace `<DOMAIN>` with the name of your cluster domain. You can find this value by using the OpenShift web console. Use the dropdown menu next to **OpenShift Container Platform** at the top of the page to switch from **Service Catalog** to **Cluster Console**. Examine the url for that page. It will be similar to `console.xyz.abc.com/k8s/cluster/projects`. The value of the domain then would be `xyz.abc.com`, after removing `console` and `/k8s/cluster/projects`.

Navigate to the TLS certificates that you plan to use on your local system. Name the TLS certificate `tlscert.pem` and the corresponding private key `tlskey.pem`. Run the following command to create the Kubernetes secret and add it to your OpenShift project. The TLS certificate and key need to be in PEM format.
```
kubectl create secret generic console-tls-secret --from-file=tls.crt=./tlscert.pem --from-file=tls.key=./tlskey.pem -n <PROJECT_NAME>
```
{:codeblock}

After you create the secret, add the following field to the `spec:` section of `ibp-console.yaml` with one indent added, at the same level as the `resources:` and `clusterdata:` sections of the advanced deployment options. You must provide name of the TLS secret that you created to the field:
```
tlsSecretName: console-tls-secret
```
{:codeblock}

When you finish editing the file, you can apply it to your cluster to provide new TLS certificates to a deployed console. After the console restarts, the UI returns to its previous state, allowing you to operate all of your exiting nodes and channels.
```
kubectl apply -f ibp-console.yaml -n <PROJECT_NAME>
```
{:codeblock}

### Verifying the console installation

You can confirm that the operator deployed by running the command `kubectl get deployment -n <PROJECT_NAME>`. If your console deployment is successful, you can see `ibpconsole` added to the deployment table, with four ones displayed. The console takes a few minutes to deploy. You might need to click refresh and wait for the table to be updated.
```
NAME           READY     UP-TO-DATE   AVAILABLE   AGE
ibp-operator   1/1       1            1           10m
ibpconsole     1/1       1            1           4m
```

The console consists of four containers that are deployed inside a single pod:
- `optools`: The console UI.
- `deployer`: A tool that allows your console to communicate with your deployments.
- `configtxlator`: A tool used by the console to read and create channel updates.
- `couchdb`: An instance of CouchDB that stores the data from your console, including your authorization information.

If there is an issue with your deployment, you can view the logs from one of the containers inside the pod. First, run the following command to get the name of the console pod:
```
kubectl get pods -n <PROJECT_NAME>
```
{:codeblock}

Then, use the following command to get the logs from one of the four containers listed above:
```
kubectl logs -f <pod_name> <container_name> -n <PROJECT_NAME>
```
{:codeblock}

As an example, a command to get the logs from the UI container would look like the following example:
```
kubectl logs -f ibpconsole-55cf9db6cc-856nz optools -n blockchain-project
```
{:codeblock}

## Log in to the console
{: #deploy-ocp-log-in}

You can use your browser to access the console by browsing to the console URL:

```
https://<PROJECT_NAME>-ibpconsole-console.<DOMAIN>
```

- Replace `<PROJECT_NAME>` with the name of the OpenShift project that you created.
- Replace `<DOMAIN>` with the name of your cluster domain. You passed this value to the `DOMAIN:` field of the `ibp-console.yaml` file.

Your console URL looks similar to the following example:
```
https://blockchain-project-ibpconsole-console.xyz.abc.com
```

You can also find your console URL by logging in to your OpenShift cluster and running the following command. Replace `<PROJECT_NAME>` with the name of your project:
```
oc get routes -n <PROJECT_NAME>
```
In the output of the command, you can see the URLs for the proxy and the console. You need to add `https://` to the beginning console URL to to access the console. You do not need to add a port to the URL.

In your browser, you can see the console log in screen:
- For the **User ID**, use the value you provided for the `email:` field in the `ibp-console.yaml` file.
- For the **Password**, use the value you encoded for the `password:` field in the `ibp-console.yaml` file. This password becomes the default password for the console that all new users use to log in to the console. After you log in for the first time, you will be asked to provide a new password that you can use to log in to the console.

Ensure that you are not using the ESR version of Firefox. If you are, switch to another browser such as Chrome and log in.
{: important}

The administrator who provisions the console can grant access to other users and restrict the actions they can perform. For more information, see [Managing users from the console](/docs/blockchain-sw?topic=blockchain-sw-console-icp-manage#console-icp-manage-users).

## Next steps
{: #console-deploy-ocp-next-steps}

When you access your console, you can view the **nodes** tab of your console UI. You can use this screen to deploy components on the cluster where you deployed the console. See the [Build a network tutorial](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-build-network#ibp-console-build-network) to get started with the console. You can also use this tab to operate nodes that are created on other clouds. For more information, see [Importing nodes](/docs/blockchain-sw?topic=blockchain-sw-ibp-console-import-nodes#ibp-console-import-nodes).

To learn how to manage the users that can access the console, view the logs of your console and your blockchain components, see [Administering your console](/docs/blockchain-sw?topic=blockchain-sw-console-icp-manage#console-icp-manage).
