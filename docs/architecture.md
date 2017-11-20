## Architecture  

**Cloudbreak deployer** installs Cloudbreak components on a VM. Once these components are deployed, you can use **Cloudbreak application** or Cloudbreak CLI to create, manage, and monitor clusters. 

### Cloudbreak Deployer Architecture

**Cloudbreak deployer** installs Cloudbreak components on a VM. It includes the following components:

| Component | Description |
|---|---|
| **Cloudbreak Application** | Cloudbreak application is built on the foundation of cloud provider APIs and Apache Ambari. | 
| **Uluwatu** | This is Cloudbreak web UI, which can be used to create, manage, and monitor clusters. |
| **Cloudbreak CLI** | This is Cloudbreak's command line tool, which can be used to create, manage, and monitor clusters. | 
| **Identity** | This is Cloudbreak's OAuth identity server implementation, which utilizes UAA. |
| **Sultans** | This is Cloudbreak's user management system. | 
| **Periscope** | This is Cloudbreak's autoscaling application, which is responsible for automatically increasing or decreasing the capacity of the cluster when your pre-defined conditions are met. |
 

### Cloudbreak Application Architecture 

The Cloudbreak application is a web application that communicates with the cloud provider account to create cloud resources on your behalf. Once the cloud resources are in place, Cloudbreak uses Apache Ambari to deploy and configure the cluster on cloud VMs. Once your cluster is deployed, you can use Cloudbreak to monitor and manage the cluster. 

<a href="../images/arch-app.png" target="_blank" title="click to enlarge"><img src="../images/arch-app.png" width="500" title="Cloudbreak architecture"></a> 

Cloudbreak application is built on the foundation of cloud provider APIs and Apache Ambari:

* Cloudbreak uses **Apache Ambari** to provision, manage, and monitor HDP clusters. 

    Ambari **blueprints** are a declarative definition of a cluster. With a blueprint, you can specify stack, component layout, and configurations to materialize an HDP cluster instance via Ambari REST API, without having to use the Ambari cluster install wizard. 
    
* Cloudbreak uses **cloud provider APIs** to create cloud resources required for the HDP clusters. 

    In order to authenticate with the cloud provider, you must create a [Cloudbreak credential](#cloudbreak-credential) for that provider. Next, you can use the create a cluster wizard in the Cloudbreak web UI (or its equivalent in the CLI) to define cloud resources which will be created for your cluster (networks, security groups, VMs and storage, and so on).  
    

#### Ambari Blueprints

**Ambari blueprints** are a declarative definition of a cluster. With a blueprint, you can specify stack, component layout, and configurations to materialize an HDP cluster instance via Ambari REST API, without having to use the Ambari cluster install wizard.  

Ambari blueprints are specified in JSON format. After you provide the blueprint to Cloudbreak, the host groups in the JSON will be mapped to a set of instances when starting the cluster, and the specified services and components will be installed on the corresponding nodes.

Cloudbreak includes a few default blueprints and allows you to upload your own blueprints.

<a href="../images/arch-blue.png" target="_blank" title="click to enlarge"><img src="../images/arch-blue.png" width="500" title="How Cb uses Ambari blueprints"></a> 

**Related Links**  
[Blueprints](security.md#identity-management)  
[Apache documentation](https://cwiki.apache.org/confluence/display/AMBARI/Blueprints) (External)  


#### Cloudbreak Credential

**Cloudbreak credential** allows Cloudbreak to authenticate with the cloud provider and create resources on your behalf. This is typically done via assigning a specific IAM role to Cloudbreak which allows Cloudbreak to perform certain actions within your cloud provider account.

After launching Cloudbreak, you must create a Cloudbreak credential. Only after you have completed that step you can start creating clusters. 

<a href="../images/arch-cred.png" target="_blank" title="click to enlarge"><img src="../images/arch-cred.png" width="500" title="How Cb uses Cloudbreak Credential"></a> 

**Related Links**  
[Identity Management](security.md#identity-management)  

