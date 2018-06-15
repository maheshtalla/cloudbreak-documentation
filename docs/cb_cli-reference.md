## Cloudbreak CLI reference  

This section will help you get started with the Cloudbreak CLI after you have [installed and configured it](https://hortonworks.github.io/cloudbreak-documentation/latest/).


### Command structure

The CLI command can contain multiple parts. The first part is a set of global options. The next part is the command. The next part is a set of command options and arguments which could include sub-commands.

<pre>cb [global options] command [command options] [arguments...]</pre>


### Command output

You can control the output from the CLI using the --output argument. The possible output formats include:

* JSON (`json`)
* YAML (`yaml`)
* Formatted table (`table`)

For example:

<pre>cb cluster list --output json</pre>

<pre>cb clusters list --output yaml</pre>

<pre>cb cluster list --output table</pre>


### Commands



#### blueprint create 

Adds a new blueprint from a file or from a URL.

**Sub-commands**

**`from-url`** Creates a blueprint by downloading it from a URL location  
**`from-file`** Creates a blueprint by reading it from a local file

**Required options**

**`from-url`** 

**`--name <value>`** Name for the blueprint  
**`--url <value>`** URL location of the Ambari blueprint JSON file

**`from-file`** 

**`--name <value>`** Name for the blueprint  
**`--file <value>`** Location of the Ambari blueprint JSON file on the local machine


**Options**

**`--description <value>`**  Description of the resource  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]  
**`--profile <value>`** Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  
**`--public`**   Public in account  

**Examples**

Adds a blueprint from a URL:

<pre>cb blueprint create from-url --url https://someurl.com/test.bp --name test1</pre>

Adds a blueprint from a local file:

<pre>cb blueprint create from-file --file /Users/test/Documents/blueprints/test.bp --name test2</pre>













#### blueprint delete

Deletes an existing blueprint.

**Required options**

**`--name <value>`** Blueprint name 

**Options**

[comment]: <> (This also has "--output option", which I believe should not be there.)

**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]  
**`--profile <value>`** Selects a config profile to use [$CB_PROFILE]   
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  

**Examples**

<pre>cb blueprint delete --name "testbp"</pre>
















#### blueprint describe

Describes an existing blueprint.

**Required options**

**`--name <value>`** Blueprint name 

**Options**

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]  
**`--profile <value>`** Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  

**Examples**

<pre>cb blueprint describe --name "Data Science: Apache Spark 2.1, Apache Zeppelin 0.7.0"
{
  "Name": "Data Science: Apache Spark 2.1, Apache Zeppelin 0.7.0",
  "Description": "Data Science: Apache Spark 2.1, Apache Zeppelin 0.7.0",
  "HDPVersion": "2.6",
  "HostgroupCount": "3",
  "Tags": "DEFAULT"
}</pre>















#### blueprint list

Lists available blueprints.

**Required options**

Nome

**Options**

**`--output <value>`** Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]   
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]  
**`--profile <value>`** Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  

**Examples**

<pre>cb blueprint list
[
  {
    "Name": "EDW-Analytics: Apache Hive 2 LLAP, Apache Zeppelin",
    "Description": "Useful for EDW analytics using Hive LLAP",
    "HDPVersion": "2.6",
    "HostgroupCount": "3",
    "Tags": "DEFAULT"
  },
  {
    "Name": "Data Science: Apache Spark 2, Apache Zeppelin",
    "Description": "Useful for data science with Spark and Zeppelin",
    "HDPVersion": "2.6",
    "HostgroupCount": "3",
    "Tags": "DEFAULT"
  },
  {
    "Name": "EDW-ETL: Apache Hive, Apache Spark 2",
    "Description": "Useful for ETL data processing with Hive and Spark",
    "HDPVersion": "2.6",
    "HostgroupCount": "3",
    "Tags": "DEFAULT"
  },
  {
    "Name": "Flow Management: Apache NiFi",
    "Description": "Useful for data-flow management with Apache NiFi",
    "HDPVersion": "3.1",
    "HostgroupCount": "2",
    "Tags": "DEFAULT"
  },
  {
    "Name": "my-hdf-test",
    "Description": "",
    "HDPVersion": "3.1",
    "HostgroupCount": "3",
    "Tags": "USER_MANAGED"
 }
]</pre>










#### cloud availability-zones

Lists all availability zones available in the specified cloud provider region. 

**Required options**

**`--credential <value>`**  Name of the credential  
**`--region <value>`**   Name of the region  
   
**Options**

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]   
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]   
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]   

**Examples**

Lists availability zones in the us-west-2 (Oregon) region on the AWS account identified by the credential called "aws-cred":

<pre>cb cloud availability-zones --credential aws-cred --region us-west-2
[
  {
    "Name": "us-west-2a"
  },
  {
    "Name": "us-west-2b"
  },
  {
    "Name": "us-west-2c"
  }
]</pre>


  
  
  
  
  
  


#### cloud regions  

Lists the available cloud provider regions. 

**Required options**

**`--credential <value>`**  Name of the credential  
   
**Options**

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]   
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]  
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]   
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]   

**Examples**

Lists regions available on the AWS account identified by the credential called "aws-cred":

<pre>cb cloud regions --credential aws-cred
[
  {
    "Name": "ap-northeast-1",
    "Description": "Asia Pacific (Tokyo)"
  },
  {
    "Name": "ap-northeast-2",
    "Description": "Asia Pacific (Seoul)"
  },
  ...</pre>





    

#### cloud volumes

Lists the available cloud provider volume types. 

**Sub-commands**

**`aws`**     Lists the available aws volume types  
**`azure`**   Lists the available azure volume types  
**`gcp`**     Lists the available gcp volume types  

**Required options**

None
   
**Options**

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]    
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]   
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]   

**Examples**

Lists volumes available on AWS:

<pre>cb cloud volumes aws
[
  {
    "Name": "ephemeral",
    "Description": "Ephemeral"
  },
  {
    "Name": "gp2",
    "Description": "General Purpose (SSD)"
  },
  {
    "Name": "st1",
    "Description": "Throughput Optimized HDD"
  },
  {
    "Name": "standard",
    "Description": "Magnetic"
  }
]</pre>





  
 

#### cloud instances 
 
Lists the available cloud provider instance types.

**Required options**

**`--credential <value>`**  Name of the credential  
**`--region <value>`**   Name of the region  
  

**Options**

**`--availability-zone <value>`**  Name of the availability zone   
**`--output <value>`**  	Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]  
**`--profile <value>`**  	Selects a config profile to use [$CB_PROFILE]   
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]    

**Examples**

Lists instance types available in the us-west-2 (Oregon) region on the AWS account identified by the credential called "aws-cred":

<pre>cb cloud instances --credential aws-cred --region us-west-2

  {
    "Name": "c3.2xlarge",
    "Cpu": "8",
    "Memory": "15.0",
    "AvailabilityZone": "us-west-2b"
  },
  {
    "Name": "c3.4xlarge",
    "Cpu": "16",
    "Memory": "30.0",
    "AvailabilityZone": "us-west-2b"
  },
  ...</pre>








#### cluster change-ambari-password

Changes Ambari password.

**Required options**

**`--name <value>`**  Cluster name  
**`--old-password <value>`** Old Ambari password   
**`--new-password <value>`**  New Ambari password    
**`--ambari-user <value>`**  Ambari user   

**Options**

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]  
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]    

**Examples**

Changes password for Ambari user called "admin" for a cluster called "test1234":

<pre>cb cluster change-ambari-password --name test1234 --old-password 123456 --new-password Ambari123456 --ambari-user admin</pre>










#### cluster create 

Creates a new cluster based on a JSON template. 

**Required options**

**`--cli-input-json <value>`**  User provided file in JSON format  

**Options**

**`--name <value>`**  Name for the cluster  
**`--description <value>`**  Description of resource   
**`--input-json-param-password <value>`**  Password for the cluster and Ambari   
**`--wait`**  Wait for the operation to finish. No argument is required   
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]   
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]     
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]    
**`--public`**   Public in account  

**Examples**

Creates a cluster called "testcluster" based on a local JSON file called "mytemplate.json" located in the /Users/test/Documents directory:   

<pre>cb cluster create --name testcluster --cli-input-json /Users/test/Documents/mytemplate.json</pre>

 











#### cluster delete

Deletes an existing cluster. 

**Required options**

**`--name <value>`**  Cluster name

**Options**

**`--force`**  Force the operation  
**`--wait`**  Wait for the operation to finish. No argument is required  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]  
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]   
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]   

**Examples**

<pre>cb cluster delete --name test1234</pre>













#### cluster describe 

Describes an existing cluster.

**Required options**

**`--name <value>`**  Cluster name

**Options**

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]   
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]  
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]   

**Examples**

Returns a JSON file describing an existing cluster called "test1234":

<pre>./cb cluster describe --name test1234</pre>

The command returns JSON output which due to space limitation was not captured in the example.











#### cluster generate-template

Generates a provider-specific cluster template in JSON format.

**Sub-commands**

**`aws new-network`** Generates an AWS cluster JSON template with new network  
**`aws existing-network`** Generates an AWS cluster JSON template with existing network  
**`aws existing-subnet`** Generates an AWS cluster JSON template with existing network and subnet  

**`azure new-network`** Generates an Azure cluster JSON template with new network  
**`azure existing-subnet`** Generates an Azure cluster JSON template with existing network and subnet  

**`gcp new-network`** Generates an GCP cluster JSON template with new network  
**`gcp existing-network`** Generates an GCP cluster JSON template with existing network   
**`gcp existing-subnet`** Generates an GCP cluster JSON template with existing network and subnet  
**`gcp legacy-network`** Generates an GCP cluster JSON template with legacy network without subnets    
 
**`openstack new-network`** Generates an OS cluster JSON template with new network   
**`openstack existing-network`** Generates an OS cluster JSON template with existing network   
**`openstack existing-subnet`** Generates an OS cluster JSON template with existing network and subnet   

**Examples**

<pre>cb cluster generate-template aws new-network</pre>










#### cluster generate-reinstall-template

Generates a cluster template that you can use to reinstall the cluster if installation went fail. 



**Required options**

**`--blueprint-name <value>`**  Name of the blueprint 


**Options**

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]    
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]   

**Examples**

<pre>cb cluster generate-reinstall-template --blueprint-name "EDW-ETL: Apache Hive 1.2.1, Apache Spark 2.1"</pre>







#### cluster list

Lists all clusters which are currently associated with the Cloudbreak instance.

**Required options**

None

**Options**

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]    
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  

**Examples**

Lists available clusters: 

<pre>cb cluster list
[
  {
    "Name": "test1234",
    "Description": "",
    "CloudPlatform": "AZURE",
    "StackStatus": "UPDATE_IN_PROGRESS",
    "ClusterStatus": "REQUESTED"
  }
]</pre>

Lists available clusters, with output in a table format:

<pre>cb cluster list --output table
+----------+-------------+---------------+--------------------+---------------+
|   NAME   | DESCRIPTION | CLOUDPLATFORM |    STACKSTATUS     | CLUSTERSTATUS |
+----------+-------------+---------------+--------------------+---------------+
| test1234 |             | AZURE         | UPDATE_IN_PROGRESS | REQUESTED     |
+----------+-------------+---------------+--------------------+---------------+</pre>













#### cluster repair

Repairs a cluster if cluster installation failed by removing, or removing and replacing failed nodes. You must specify the cluster name and the host group with the failed nodes.  

**Required options**

**`--name <value>`**  Cluster name  
**`--host-groups <value>`** Comma separated list of host groups where the failed nodes should be repaired  

**Options**

**`--remove-only`** The failed nodes will be removed (rather than "repaired" by removing and replacing)   
**`--wait`**  Wait for the operation to finish. No argument is required  
**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT] 
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]    
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]   

**Examples**

<pre>cb cluster repair --name test1234 --host-groups master,worker</pre>











#### cluster retry

Retries the process if cluster or stack provisioning failed.

**Required options**

**`--name <value>`**  Cluster name

**Options**

**`--wait`**  Wait for the operation to finish. No argument is required  
**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT] 
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]    
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]   

**Examples**

<pre>cb cluster retry --name test1234</pre>










#### cluster scale

Scales a cluster by adding or removing nodes.

**Required options**

**`--name <value>`**  Cluster name  
**`--group-name <value>`**  Name of the group to scale  
**`--desired-node-count <value>`**  Desired number of nodes  

**Options**

**`--wait`**  Wait for the operation to finish. No argument is required   
**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]    
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]   

**Examples**

<pre>cb cluster scale --name test1234 --group-name worker --desired node-count 3</pre>








#### cluster start

Starts a cluster which has previously been stopped.

**Required options**

**`--name <value>`**  Cluster name

**Options**

**`--wait`**  Wait for the operation to finish. No argument is required  
**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]    
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]   

**Examples**

<pre>cb cluster start --name test1234</pre>








#### cluster stop

Stops a cluster.

**Required options**

**`--name <value>`**  Cluster name

**Options**

**`--wait`**  Wait for the operation to finish. No argument is required  
**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]    
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  

**Examples**

<pre>cb cluster stop --name test1234</pre>








#### cluster sync

Synchronizes a cluster with the cloud provider.

**Required options**

**`--name <value>`**  Cluster name

**Options**

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]    
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  


**Examples**

<pre>cb cluster sync --name test1234</pre>










#### configure

Configures the Cloudbreak server address and credentials used to communicate with this server.

**Required options**

**`--server <value>`**  Server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**	 User name (e-mail address) [$CB_USER_NAME]  

**Options**

**`--password <value>`**  Password [$CB_PASSWORD]  
**`--profile <value>`**  Select a config profile to use [$CB_PROFILE]   
**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  

**Examples**

This example configures the server address with username and password:

<pre>cb configure --server https://ec2-11-111-111-11.compute-1.amazonaws.com --username admin@hortonworks.com --password MySecurePassword123</pre>

This example configures the server address with username but without a password:

<pre>cb configure --server https://ec2-11-111-111-11.compute-1.amazonaws.com --username admin@hortonworks.com</pre>









#### credential create 

Creates a new Cloudbreak credential.

**Sub-commands**

**`aws role-based`**  Creates a new AWS credential  
**`aws key-based`**  Creates a new AWS credential  
**`azure app-based`** Creates a new app-based Azure credential  
**`gcp`** Creates a new gcp credential  
**`openstack keystone-v2`** Creates a new OpenStack credential  
**`openstack keystone-v3`** Creates a new OpenStack credential 

**Required options**

**`aws role-based`** 

**`--name <value>`**  Name for the credential   
**`--role-arn <value>`** IAM Role ARN of the role used for Cloudbreak credential  

**`aws key-based`** 

**`--name <value>`**  Name for the credential    
**`--access-key <value>`**  AWS Access Key  
**`--secret-key <value>`**  AWS Secret Key  

**`azure app-based`** 

**`--name <value>`**  Name for the credential    
**`--subscription-id <value>`**  Subscription ID from your Azure Subscriptions  
**`--tenant-id <value>`**  Directory ID from your Azure Active Directory > Properties      
**`--app-id <value>`**  Application ID of your app from your Azure Active Directory > App Registrations            
**`--app-password <value>`**  Your application key from app registration's Settings > Keys  

**`gcp`** 

**`--name <value>`**  Name for the credential    
**`--project-id <value>`**  Project ID from your GCP account                        
**`--service-account-id <value>`**  Your GCP Service account ID from IAM & Admin > Service accounts                 
**`--service-account-private-key-file <value>`**  P12 key from your GCP service account  

**`openstack keystone-v2`**  

**`--name <value>`**  Name for the credential    
**`--tenant-user <value>`**  OpenStack user name     
**`--tenant-password <value>`**  OpenStack password  
**`--tenant-name <value>`**  OpenStack tenant name      
**`--endpoint <value>`**   OpenStack endpoint   

**`openstack keystone-v3`** 

**`--name <value>`**  Name for the credential   
**`--tenant-user <value>`**  OpenStack user name     
**`--tenant-password <value>`**  OpenStack password  
**`--user-domain <value>`**  OpenStack user domain      
**`--endpoint <value>`**   OpenStack endpoint  


**Options**

**`--description <value>`**  Description of the resource  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]    
**`--profile <value>`** Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  
**`--public`**   Public in account  

Additionally, the following option is available for OpenStack Keystone2 and Keystone3:

**`--facing <value>`** API facing. One of: public, admin, internal

Additionally, the following options are available for OpenStack Keystone3:

**`--project-domain-name <value>`**  OpenStack project domain name    
**`--project-name <value>`**  OpenStack project name           
**`--domain-name <value>`**  OpenStack domain name    
**`--keystone-scope <value>`** OpenStack keystone scope. One of: default, domain, project   


**Examples**

Creates a role-based credential on AWS:

<pre>cb credential create aws role-based --name my-credential1 --role-arn arn:aws:iam::517127065441:role/CredentialRole</pre>

Creates a key-based credential on AWS:

<pre>cb credential create aws key-based --name my-credential2 --access-key ABDVIRDFV3K4HLJ45SKA --secret-key D89L5pOPM+426Rtj3curKzJEJL3lYoNcP8GvguBV</pre>

Creates an app-based credential on Azure:

<pre>cb credential create azure app-based --name my-credential3 --subscription-id b8e7379e-568g-55d3-na82-45b8d421e998 --tenant-id  c79n5399-3231-65ba-8dgg-2g4e2a40085e --app-id 6d147d89-48d2-5de2-eef8-b89775bbfcg1 --app-password 4a8hBgfI52s/C8R5Sea2YHGnBFrD3fRONfdG8w7F2Ua=</pre>

Creates a credential on Google Cloud:

<pre>cb credential create gcp --name my-credential4 --project-id test-proj --service-account-id test@test-proj.iam.gserviceaccount.com --service-account-private-key-file /Users/test/3fff57a6f68e.p12</pre>


Creates a role-based credential on OpenStack with Keystone-v2:

<pre>cb credential create openstack keystone-v2 --name my-credential5 --tenant-user test --tenant-password MySecurePass123 --tenant-name test --endpoint http://openstack.test.organization.com:5000/v2.0</pre>










#### credential delete

Deletes an existing Cloudbreak credential.

**Required options**

**`--name <value>`**  Credential name 

**Options**

**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]    
**`--profile <value>`** Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  

**Examples**

<pre>cb credential delete --name test-cred</pre>








#### credential describe

Describes an existing credential.

**Required options**

**`--name <value>`**  Credential name 

**Options**
  
**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]    
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]    
**`--profile <value>`** Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  

**Examples**

<pre>cb credential describe --name testcred
{
  "Name": "testcred",
  "Description": "",
  "CloudPlatform": "AZURE"
}</pre>












#### credential list

Lists existing Cloudbreak credentials.

**Required options**

None

**Options**

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]    
**`--profile <value>`** Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  

**Examples**

Lists credentials:

<pre>cb credential list 
[
  {
    "Name": "testcred",
    "Description": "",
    "CloudPlatform": "AZURE"
  }
]
</pre>

Lists credentials, with output formatted in a table format:

<pre>cb credential list --output table
+---------+-------------+---------------+
|  NAME   | DESCRIPTION | CLOUDPLATFORM |
+---------+-------------+---------------+
| armcred |             | AZURE         |
+---------+-------------+---------------+</pre>













#### credential modify 

Modifies an existing Cloudbreak credential. 

> The `--name` parameter is used to identify the credential that is being modified, and therefore its value cannot be modified.    


**Sub-commands**

**`aws role-based`**  Modifies an AWS role-based credential  
**`aws key-based`**  Modifies an AWS key-based credential  
**`azure app-based`** Modifies an app-based Azure credential  
**`gcp`** Modifies a Google Cloud credential  
**`openstack keystone-v2`** Modifies an OpenStack v2 credential  
**`openstack keystone-v3`** Modifies an  OpenStack v3 credential 

**Required options**

**`aws role-based`** 

**`--name <value>`**  Credential name  
**`--role-arn <value>`** IAM Role ARN of the role used for Cloudbreak credential  

**`aws key-based`** 

**`--name <value>`**  Credential name  
**`--access-key <value>`**  AWS Access Key  
**`--secret-key <value>`**  AWS Secret Key  

**`azure app-based`** 

**`--name <value>`**  Credential name  
**`--subscription-id <value>`**  Subscription ID from your Azure Subscriptions  
**`--tenant-id <value>`**  Directory ID from your Azure Active Directory > Properties      
**`--app-id <value>`**  Application ID of your app from your Azure Active Directory > App Registrations            
**`--app-password <value>`**  Your application key from app registration's Settings > Keys  

**`gcp`** 

**`--name <value>`**  Credential name   
**`--project-id <value>`**  Project ID from your GCP account                        
**`--service-account-id <value>`**  Your GCP Service account ID from IAM & Admin > Service accounts                 
**`--service-account-private-key-file <value>`**  P12 key from your GCP service account  

**`openstack keystone-v2`**  

**`--name <value>`**  Credential name  
**`--tenant-user <value>`**  OpenStack user name     
**`--tenant-password <value>`**  OpenStack password  
**`--tenant-name <value>`**  OpenStack tenant name      
**`--endpoint <value>`**   OpenStack endpoint   

**`openstack keystone-v3`** 

**`--name <value>`**  Credential name  
**`--tenant-user <value>`**  OpenStack user name     
**`--tenant-password <value>`**  OpenStack password  
**`--user-domain <value>`**  OpenStack user domain      
**`--endpoint <value>`**   OpenStack endpoint  


**Options**

**`--description <value>`**  Description of the resource  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]    
**`--profile <value>`** Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]    

Additionally, the following option is available for OpenStack Keystone2 and Keystone3:

**`--facing <value>`** API facing. One of: public, admin, internal

Additionally, the following options are available for OpenStack Keystone3:

**`--project-domain-name <value>`**  OpenStack project domain name    
**`--project-name <value>`**  OpenStack project name           
**`--domain-name <value>`**  OpenStack domain name    
**`--keystone-scope <value>`** OpenStack keystone scope. One of: default, domain, project  


**Examples**

Modifies a role-based AWS credential:

<pre>cb credential modify aws role-based --name my-credential1 --role-arn arn:aws:iam::517127065441:role/CredentialRole</pre>

Modifies a key-based AWS credential:

<pre>cb credential modify aws key-based --name my-credential2 --access-key ABDVIRDFV3K4HLJ45SKA --secret-key D89L5pOPM+426Rtj3curKzJEJL3lYoNcP8GvguBV</pre>

Modifies an app-based Azure credential:

<pre>cb credential modify azure app-based --name my-credential3 --subscription-id b8e7379e-568g-55d3-na82-45b8d421e998 --tenant-id  c79n5399-3231-65ba-8dgg-2g4e2a40085e --app-id 6d147d89-48d2-5de2-eef8-b89775bbfcg1 --app-password 4a8hBgfI52s/C8R5Sea2YHGnBFrD3fRONfdG8w7F2Ua=</pre>

Modifies a Google Cloud credential:

<pre>cb credential modify gcp --name my-credential4 --project-id test-proj --service-account-id test@test-proj.iam.gserviceaccount.com --service-account-private-key-file /Users/test/3fff57a6f68e.p12</pre>


Modifies a role-based OpenStack credential which uses Keystone-v2:

<pre>cb credential modify openstack keystone-v2 --name my-credential5 --tenant-user test --tenant-password MySecurePass123 --tenant-name test --endpoint http://openstack.test.organization.com:5000/v2.0</pre>










__________________________________

#### database create

Registers an existing external database with Cloudbreak.

**Sub-commands**  

**`mysql`**  Registers a MySQL database configuration  
**`oracle11`**  Registers an Oracle 11 database configuration  
**`oracle12`**  Registers an Oracle 12 database configuration  
**`postgres`**  Registers a Postgres database configuration  

 

**Required options**

**`--name <value>`**   Name for the database     
**`--db-username <value>`**  Username for the JDBC connection  
**`--db-password <value>`**  Password for the JDBC connection  
**`--url <value>`**  JDBC connection URL in the form of jdbc:db-type://address:port/db  
**`--type <value>`**   Name if the service that will use the database (AMBARI, DRUID, HIVE, OOZIE, RANGER, SUPERSET, or other custom type)    

If using MySQL and Oracle, the **`--connector-jar-url value <value>`** parameter is required in all cases except the following: If you are using a custom image and you already placed the JAR file on the machine, then this parameter is not required.


**Options**

**`--description <value>`**  Description for the database       
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]   
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]   
**`--public`**   Public in account  


**Examples**

Registers an existing Postgres database called "test-postgres" with Cloudbreak:

<pre>cb database create postgres --name testpostgres  --type HIVE --url jdbc:postgresql://test-db.cic6nusrpqec.us-west-2.rds.amazonaws.com:5432/testdb --db-username testuser --db-password MySecurePassword123</pre>

The connection URL includes three components db-type://address:port/db: 

* Database type "jdbc:postgresql"  
* Endpoint "test-db.cic6nusrpqec.us-west-2.rds.amazonaws.com:5432"  
* Port "5432"  
* Database name "testdb"  

Registers an existing MySQL database called "testmysql" with Cloudbreak:  

<pre>cb database create mysql --name testmysql --type OOZIE --url jdbc:mysql://test-db.cic6nusrpqec.us-west-2.rds.amazonaws.com:5432/testdb  --db-username test --db-password test --connector-jar-url http://example-page/driver-file.JAR</pre> 
  







 
#### database delete

Unregisters a previously registered database with Cloudbreak. It does not delete the database instance. 

**Required options**

**`--name <value>`**  Database registration name     

**Options**

**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]  
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]

**Examples**

<pre>cb database delete --name testdatabase</pre>

















#### database list

Lists all available database registrations.

**Required options**

None

**Options**

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]  
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]

**Examples**

Lists existing database registrations:

<pre>cb database list</pre>

Lists existing database registrations, with output presented in a table format:

<pre>cb database list --output table</pre>
















#### database test

Test database connection.

 
**Sub-commands**  


**`by-name`**    Tests a stored database configuration identified by its name  
**`by-params`**  Tests database connection parameters   

**Required options**  

**`by-name`**  

**`--name <value>`**  Database registration name    

**`by-params`**  

**`--db-username <value>`**  Username to use for the JDBC connection  
**`--db-password <value>`**  Password to use for the JDBC connection  
**`--url <value>`**  JDBC connection URL in the form of jdbc:db-type://address:port/db  
**`--type <value>`**  Type of database (the service name that will use the database  

**Options**

**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]    
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  

**Examples**

Tests connection to a previously registered database called "testpostgres":

<pre>database test --name testpostgres</pre>

Tests connection to a database based on connection parameters provided: 

<pre>cb database test by-params --type HIVE --url jdbc:postgresql://test-db.cic6nusrpqec.us-west-2.rds.amazonaws.com:5432/testdb --db-username testuser --db-password MySecurePassword123</pre> 











#### imagecatalog create   

Registers a new custom image catalog based on the URL provided.  


**Required options**  

**`--name <value>`**  Name for the image catalog  
**`--url <value>`**   URL location of the image catalog JSON file  

**Options**

**`--description <value>`**  Description for the recipe   
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]    
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  
**`--public`**   Public in account  

**Examples**

Registers an image catalog called "mycustomcatalog" which is available at https://example.com/myimagecatalog.json: 

<pre>cb imagecatalog create --name mycustomcatalog --url https://example.com/myimagecatalog.json</pre> 

  








 

#### imagecatalog delete

Deletes a previously registered custom image catalog. It does not delete any cloud provider resources that you created as a prerequisite for creating the Cloudbreak credential.    

**Required options**  

**`--name <value>`**  Image catalog name    

**Options**

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]    
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  

**Examples**

Deletes an image catalog called "mycustomcatalog":

<pre>cb imagecatalog delete --name mycustomcatalog</pre> 










#### imagecatalog images 

Lists images from the specified image catalog available for the specified cloud provider.   

**Sub-commands**  

**`aws`**         Lists available aws images     
**`azure`**       Lists available azure images     
**`gcp`**         Lists available gcp images    
**`openstack`**   Lists available openstack images    


**Required options**

**`--imagecatalog <value>`**  Name of the imagecatalog     

   
**Options**

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]    
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]     
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]   
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  

**Examples**  

Returns date, description, Ambari version, and image ID for all AWS images from an image catalog called "myimagecatalog":

<pre>./cb imagecatalog images aws --imagecatalog cloudbreak-default
[
  {
    "Date": "2017-10-13",
    "Description": "Cloudbreak official base image",
    "Version": "2.6.0.0",
    "ImageID": "44b140a4-bd0b-457d-b174-e988bee3ca47"
  },
  {
    "Date": "2017-11-16",
    "Description": "Official Cloudbreak image",
    "Version": "2.6.0.0",
    "ImageID": "3c7598a4-ebd6-4a02-5638-882f5c7f7add"
  }
]</pre>












  
               
#### imagecatalog list 

Lists default and custom image catalogs registered with Cloudbreak instance.   

**Required options**  

None  

**Options**  

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]   
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]    
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  

**Examples**

Lists existing image catalogs:  

<pre>cb  imagecatalog list 
[
  {
    "Name": "mycustomcatalog",
    "Default": false,
    "URL": "https://example.com/imagecatalog.json"
  },
  {
    "Name": "cloudbreak-default",
    "Default": true,
    "URL": "https://s3-eu-west-1.amazonaws.com/cloudbreak-info/v2-dev-cb-image-catalog.json"
  }
]</pre>


 










#### imagecatalog set-default

Sets the specified image catalog as default.  

**Required options**   

**`--name <value>`**  Image catalog name     

**Options**

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]    
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  

**Examples**

Sets "mycustomcatalog" as default:  

<pre>imagecatalog set-default --name mycustomcatalog</pre>

 













#### ldap create

Registers an existing LDAP with Cloudbreak.
 

**Required options**

**`--name <value>`**  Name for the LDAP       
**`--ldap-server <value>`**  Address of the LDAP server (e.g. ldap://10.0.0.1:384)  
**`--ldap-domain <value>`**   LDAP domain (e.g. ad.cb.com)  
**`--ldap-bind-dn <value>`**  LDAP bind DN (e.g. CN=Administrator,CN=Users,DC=ad,DC=cb,DC=com)  
**`--ldap-bind-password <value>`**  LDAP bind password  
**`--ldap-directory-type <value>`**   LDAP directory type (LDAP or ACTIVE_DIRECTORY)   
**`--ldap-user-search-base <value>`**   LDAP user search base (e.g. CN=Users,DC=ad,DC=cb,DC=com)  
**`--ldap-user-name-attribute <value>`**   LDAP user name attribute  
**`--ldap-user-object-class <value>`**   LDAP user object class  
**`--ldap-group-member-attribute <value>`** LDAP group member attribute  
**`--ldap-group-name-attribute <value>`**  LDAP group name attribute  
**`--ldap-group-object-class <value>`**  LDAP group object class  
**`--ldap-group-search-base <value>`**   LDAP group search base (e.g. OU=scopes,DC=ad,DC=cb,DC=com)  

**Options**

**`--ldap-admin-group <value>`**  LDAP group of administrators  
**`--description <value>`**  Description for the LDAP      
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]   
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  
**`--public`**   Public in account  












 
#### ldap delete

Deletes selected LDAP registration from Cloudbreak. It does not delete the LDAP. 

**Required options**

**`--name <value>`**  LDAP name      

**Options**

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]  
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]

**Examples**

<pre>cb ldap delete --name testldap</pre>

















#### ldap list

Lists all available LDAPs.

**Required options**

None

**Options**

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]  
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]

**Examples**

Lists existing LDAPs:

<pre>cb ldap list</pre>

Lists existing LDAPs, with output presented in a table format:

<pre>cb ldap list --output table</pre>














#### mpack create

Registers an existing management pack with Cloudbreak.
 

**Required options**

**`--name <value>`**  Name for the mpack        
**`--url <value>`**  URL that points to the location of the mpack tarball   

**Options**

**`--purge`**  Purge existing resources specified in purge-list   
**`--purge-list <value>`**  Comma-separated list of resources to purge (stack-definitions,service-definitions,mpacks). By default (stack-definitions,mpacks) will be purged  
**`--force`**  Force install management pack    
**`--description <value>`**  Description for the LDAP      
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]   
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  
**`--public`**   Public in account  

**Examples**

Registers a new mpack without purging:

<pre>cb mpack create --name test-hdp-search --url http://public-repo-1.hortonworks.com/HDP-SOLR/hdp-solr-ambari-mp/solr-service-mpack-3.0.0.tar.gz</pre>









 
#### mpack delete

Deletes selected management registration from Cloudbreak. It does not delete the management pack. 

**Required options**

**`--name <value>`**  Management pack name      

**Options**

**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]  
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]

**Examples**

<pre>cb mpack delete --name testmpack</pre>

















#### mpack list

Lists all available management packs.

**Required options**

None

**Options**

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]  
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]

**Examples**

Lists all currently registered management packs and provides information about each:

<pre>cb mpack list
[
  {
    "Name": "hdp-search-3",
    "Description": "",
    "URL": "http://public-repo-1.hortonworks.com/HDP-SOLR/hdp-solr-ambari-mp/solr-service-mpack-3.0.0.tar.gz",
    "Purge": "false",
    "PurgeList": "",
    "Force": "false"
  }
]</pre>



























#### proxy create

Registers an existing proxy with Cloudbreak.
 

**Required options**

**`--name <value>`**  Name for the proxy     
**`--proxy-host <value>`**  Hostname or IP address of the proxy  
**`--proxy-port <value>`**  Port of the proxy  

**Options**

**`--proxy-protocol <value>`**  Protocol for the proxy (http or https) (default: "http")  
**`--proxy-user <value>`**   User for the proxy if basic auth is required  
**`--proxy-password <value>`**  Password for the proxy if basic auth is required  
**`--description <value>`**  Description for the proxy      
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]   
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  
**`--public`**   Public in account  











 
#### proxy delete

Unregisters a previously registered proxy with Cloudbreak. It does not delete the proxy. 

**Required options**

**`--name <value>`**  Proxy registration name     

**Options**

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]  
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]

**Examples**

<pre>cb proxy delete --name testproxy</pre>

















#### proxy list

Lists all proxies that were previously registered with Cloudbreak.

**Required options**

None

**Options**

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]  
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]

**Examples**

Lists existing proxy registrations:

<pre>cb proxy list</pre>

Lists existing proxy registrations, with output presented in a table format:

<pre>cb proxy list --output table</pre>
















#### recipe create

Adds a new recipe from a file or from a URL.

**Sub-commands**

**`from-url`**  Creates a recipe by downloading it from a URL location  
**`from-file`**  Creates a recipe by reading it from a local file  

**Required options**

**`from-url`**  

**`--name <value>`**  Name for the recipe   
**`--execution-type <value>`**  Type of execution [pre-ambari-start, pre-termination, post-ambari-start, post-cluster-install]    
**`--url <value>`**  URL location of the Ambari blueprint JSON file  

  
**`from-file`** 

**`--name <value>`**  Name for the recipe  
**`--execution-type <value>`**  Type of execution [pre-ambari-start, pre-termination, post-ambari-start, post-cluster-install]   
**`--file <value>`**  Location of the Ambari blueprint JSON file  

**Options**

**`--description <value>`**  Description for the recipe   
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]  
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  
**`--public`**   Public in account  

**Examples**


Adds a new recipe called "test1" from a URL:

<pre>cb recipe create from-url --name "test1" --execution-type post-ambari-start --url http://some-site.com/test.sh</pre>

Adds a new recipe called "test2" from a file:

<pre>cb recipe create from-url --name "test2" --execution-type post-ambari-start --file /Users/test/Documents/test.sh</pre>









 
#### recipe delete

Deletes an existing recipe.

**Required options**

**`--name <value>`**  Recipe name  

**Options**

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]  
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]   
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  

**Examples**

<pre>cb recipe delete --name test</pre>













#### recipe describe

Describes an existing recipe.

**Required options**

**`--name <value>`**  Recipe name     

**Options**

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]  
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  


**Examples**

Describes a recipe called "test":

<pre>cb recipe describe --name test
{
  "Name": "test",
  "Description": "",
  "ExecutionType": "POST"
}</pre>

Describes a recipe called "test", with output presented in a table format:

<pre>cb describe-recipe --name test --output table
+------+-------------+----------------+
| NAME | DESCRIPTION | EXECUTION TYPE |
+------+-------------+----------------+
| test |             | POST           |
+------+-------------+----------------+</pre>













#### recipe list

Lists all available recipes.

**Required options**

None

**Options**

**`--output <value>`**  Supported formats: json, yaml, table (default: "json") [$CB_OUT_FORMAT]  
**`--server <value>`**  Cloudbreak server address [$CB_SERVER_ADDRESS]  
**`--username <value>`**  Cloudbreak user name (e-mail address) [$CB_USER_NAME]  
**`--password <value>`**  Cloudbreak password [$CB_PASSWORD]  
**`--profile <value>`**  Selects a config profile to use [$CB_PROFILE]  
**`--auth-type <value>`**  Authentication method to use. Values: oauth2, basic [$CB_AUTH_TYPE]  

**Examples**

Lists existing recipes:

<pre>cb recipe list
[
  {
    "Name": "test",
    "Description": "",
    "ExecutionType": "POST"
  }
]</pre>

Lists existing recipes, with output presented in a table format:

<pre>cb recipe list --output table
+------+-------------+-------------------+
| NAME | DESCRIPTION | EXECUTION TYPE    |
+------+-------------+-------------------+
| test |             | POST-AMBARI-START |
+------+-------------+-------------------+
</pre>










### Debugging

To use debugging mode, pass the `--debug` option. 


### Checking CLI Version

To check CLI version, use `cb --version`.







