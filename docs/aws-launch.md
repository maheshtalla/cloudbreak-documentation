## Launching Cloudbreak on AWS

Before launching Cloudbreak on AWS, review and meet the prerequisites. Next, launch a VM using a Cloudbreak Amazon Machine Image, access the VM, and then start Cloudbreak. Once Cloudbreak is running, log in to the Cloudbreak UI and create a Cloudbreak credential. 

### Meet the Prerequisites

Before launching Cloudbreak on AWS, you must meet the following prerequisites.

#### AWS Account 

In order to launch Cloudbreak on AWS, you must log in to your AWS account. If you don't have an account, you can create one at [https://aws.amazon.com/](https://aws.amazon.com/).

#### AWS Region

Decide in which AWS region you would like to launch Cloudbreak. The following AWS regions are supported: 

| Region Name | Region | 
|---|---|
| EU (Ireland) | eu-west-1 |
| EU (Frankfurt) | eu-central-1 |	
| US East (N. Virginia) | us-east-1 |
| US West (N. California) | us-west-1 |	
| US West (Oregon) | us-west-2 | 
| South America (São Paulo) | sa-east-1 | 
| Asia Pacific (Tokyo) | ap-northeast-1	|  
| Asia Pacific (Singapore) | ap-southeast-1 | 
| Asia Pacific (Sydney) | ap-southeast-2 | 

Clusters created via Cloudbreak can be in the same or different region as Cloudbreak; when you launch a cluster, you select the region in which to launch it.

**Related Links**  
[AWS Regions and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html) (External)   

#### SSH Key Pair

Import an existing key pair or generate a new key pair in the AWS region which you are planning to use for launching Cloudbreak and clusters. You can do this using the following steps.

**Steps** 

1. Navigate to the Amazon EC2 console at https://console.aws.amazon.com/ec2/.  
2. Check the region listed in the top right corner to make sure that you are in the correct region.  
3. In the left pane, find **NETWORK AND SECURITY** and click **Key Pairs**.   
4. Do one of the following:
    * Click **Create Key Pair** to create a new key pair. Your private key file will be automatically downloaded onto your computer. Make sure to save it in a secure location. You will need it to SSH to the cluster nodes. You may want to change access settings for the file using `chmod 400 my-key-pair.pem`.  
    * Click **Import Key Pair** to upload an existing public key and then select it and click **Import**. Make sure that you have access to its corresponding private key.    

You need this SSH key pair to SSH to the Cloudbreak instance and start Cloudbreak. 

**Related Links**  
[Creating a Key Pair Using Amazon EC2](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair) (External)  

#### Authentication 

[comment]: <> (I cold use some help clarifying this section.)

Before you can start using Cloudbreak for provisioning clusters, you must select a way for Cloudbreak to authenticate with your AWS account and create resources on your behalf. There are two ways to do this: 

* **Key-based**: This is a simpler option which does not require additional configuration at this point. It requires that you provide your AWS access key and secret key pair in the Cloudbreak web UI later. All you need to do now is check your AWS account and ensure that you can access this key pair.

* **Role-based**: This requires that you or your AWS admin create an IAM role to allow Cloudbreak to assume AWS roles (the "AssumeRole" policy).

#####**Option 1: Key-based Authentication**

If you are using key-based authentication for Cloudbreak on AWS, you must be able to provide your AWS access key and secret key pair. Cloudbreak will use these keys to launch the resources. You must provide the access and secret keys later in the Cloudbreak web UI later when creating a credential. 

If you choose this option, all you need to do at this point is check your AWS account and make sure that you can access this key pair. You can generate new access and secret keys from the **IAM Console** > **Users**. Next, select a user and click on the **Security credentials** tab:

<a href="../images/aws-iam_security_creds.png" target="_blank" title="click to enlarge"><img src="../images/aws-iam_security_creds.png" width="650" title="IAM Console"></a> 

If you choose this option, you can proceed to [Launch the VM](#launch-the-vm).
 

#####**Option 2: Role-based Authentication**

{!docs/common/aws-launch-authentication-role-based-intro.md!}

[comment]: <> (The notes below could be included in the table above as alternative steps, but I'm afraid that showing too many options can be confusing; especially that in order to perform these alternative steps, you must perform advanced configuration. So I am adding them as a side note.)

> The following alternative steps are available for the **CloudbreakRole**:

> Alternatively, you can generate the "CredentialRole" role later once your Cloudbreak VM is running and once you have [configured AWS CLI](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) on your VM, by running the `cbd aws generate-role` command. This command creates a role with the name "cbreak-deployer" (equivalent to the "CredentialRole"). To customize the name of the role, add `export AWS_ROLE_NAME=my-cloudbreak-role-name` (where "my-cloudbreak-role-name" is your custom role name) as a new line to your Profile.
 
> Alternatively, instead of attaching the "CloudbreakRole" role during the VM launch, you can assign the "CloudbreakRole" to an IAM user and then add the access and security key of that user to your 'Profile'.

You can create these roles in the **IAM console**, on the **Roles** page via the **Create Role** option. Detailed steps are provided below. 


**Create CloudbreakRole**

Use these steps to create CloudbreakRole. 

Use the following "AssumeRole" policy definition: 

<pre>{
  "Version": "2012-10-17",
  "Statement": {
    "Sid": "Stmt1400068149000",
    "Effect": "Allow",
    "Action": ["sts:AssumeRole"],
    "Resource": "*"
  }
}
</pre>  

[comment]: <> (Pasting this here because when I paste it in-between the steps, I can't get this to format correctly.) 

**Steps**

1. Navigate to the **IAM console** > **Roles** and click **Create Role**.

    <a href="../images/aws_role-00.png" target="_blank" title="click to enlarge"><img src="../images/aws_role-00.png" width="650" title="IAM Console"></a> 
    
2. In the "Create Role" wizard, select **AWS service** role type and then select any service. 

    <a href="../images/aws_role-10.png" target="_blank" title="click to enlarge"><img src="../images/aws_role-10.png" width="650" title="IAM Console"></a> 

3. When done, click **Next: Permissions** to navigate to the next page in the wizard.
    
4. Click **Create policy**.

    <a href="../images/aws_role-11.png" target="_blank" title="click to enlarge"><img src="../images/aws_role-11.png" width="650" title="IAM Console"></a>
    
5. Click **Select** next to "Create Your Own Policy".

    <a href="../images/aws_role-12.png" target="_blank" title="click to enlarge"><img src="../images/aws_role-12.png" width="650" title="IAM Console"></a>  

6. In the **Policy Name** field, enter "AssumeRole" and in the **Policy Document** paste the policy definition. You can either copy it from the section preceding these steps or download and copy it from [here](https://docs.hortonworks.com/HDPDocuments/Cloudbreak/Cb-doc-resources/AssumeRole.json).

    <a href="../images/aws_role-13.png" target="_blank" title="click to enlarge"><img src="../images/aws_role-13.png" width="650" title="IAM Console"></a>  
    
7. When done, click **Create Policy**.

8. Click **Refresh**. Next, find the "AsumeRole" policy that you just created and select it by checking the box.

    <a href="../images/aws_role-14.png" target="_blank" title="click to enlarge"><img src="../images/aws_role-14.png" width="650" title="IAM Console"></a> 
    
9. When done, click **Next: Review**.
    
9. In the **Roles name** field, enter role name, for example "CloudbreakRole". 

    <a href="../images/aws_role-15.png" target="_blank" title="click to enlarge"><img src="../images/aws_role-15.png" width="650" title="IAM Console"></a> 
    
10. When done, click **Create role** to finish the role creation process.
    

**Create CredentialRole**

Use these steps to create CredentialRole.

Use the following "cb-policy" policy definition: 

<pre>{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudformation:CreateStack",
                "cloudformation:DeleteStack",
                "cloudformation:DescribeStackEvents",
                "cloudformation:DescribeStackResource",
                "cloudformation:DescribeStacks"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:AllocateAddress",
                "ec2:AssociateAddress",
                "ec2:AssociateRouteTable",
                "ec2:AuthorizeSecurityGroupIngress",
                "ec2:CreateAddress",
                "ec2:CreateRoute",
                "ec2:CreateRouteTable",
                "ec2:CreateSecurityGroup",
                "ec2:CreateSubnet",
                "ec2:CreateTags",
                "ec2:CreateVpc",
                "ec2:ModifyVpcAttribute",
                "ec2:DeleteSubnet",
                "ec2:CreateInternetGateway",
                "ec2:CreateKeyPair",
                "ec2:DisassociateAddress",
                "ec2:DisassociateRouteTable",
                "ec2:ModifySubnetAttribute",
                "ec2:ReleaseAddress",
                "ec2:DescribeAddresses",
                "ec2:DescribeImages",
                "ec2:DescribeInstanceStatus",
                "ec2:DescribeInstances",
                "ec2:DescribeInternetGateways",
                "ec2:DescribeKeyPairs",
                "ec2:DescribeRouteTables",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeSubnets",
                "ec2:DescribeVpcs",
                "ec2:DescribeVpcAttribute",
                "ec2:ImportKeyPair"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:ListRolePolicies",
                "iam:GetRolePolicy",
                "iam:ListAttachedRolePolicies",
                "iam:ListInstanceProfiles",
                "iam:PutRolePolicy",
                "iam:PassRole",
                "iam:GetRole"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "autoscaling:CreateAutoScalingGroup",
                "autoscaling:CreateLaunchConfiguration",
                "autoscaling:DeleteAutoScalingGroup",
                "autoscaling:DeleteLaunchConfiguration",
                "autoscaling:DescribeAutoScalingGroups",
                "autoscaling:DescribeLaunchConfigurations",
                "autoscaling:DescribeScalingActivities",
                "autoscaling:DetachInstances",
                "autoscaling:ResumeProcesses",
                "autoscaling:SuspendProcesses",
                "autoscaling:UpdateAutoScalingGroup"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}</pre>

[comment]: <> (Pasting this here because when I paste it in-between the steps, I can't get this to format correctly.) 

**Steps**

1. Navigate to the **IAM console** > **Roles** and click **Create Role**.

    <a href="../images/aws_role0.png" target="_blank" title="click to enlarge"><img src="../images/aws_role0.png" width="650" title="IAM Console"></a> 
    
2. In the "Create Role" wizard, select **Another AWS account** role type. Next, provide the following:

    * In the **Account ID** field, enter your AWS account ID.
    * Under **Options**, check **Require external ID**.
    * In the **External ID**, enter "provision-ambari".

    <a href="../images/aws_role1.png" target="_blank" title="click to enlarge"><img src="../images/aws_role1.png" width="650" title="IAM Console"></a> 
    
3. When done, click **Next: Permissions** to navigate to the next page in the wizard.
    
4. Click **Create policy**.

    <a href="../images/aws_role2.png" target="_blank" title="click to enlarge"><img src="../images/aws_role2.png" width="650" title="IAM Console"></a>
    
5. Click **Select** next to "Create Your Own Policy".

    <a href="../images/aws_role3.png" target="_blank" title="click to enlarge"><img src="../images/aws_role3.png" width="650" title="IAM Console"></a> 
    
    
6. In the **Policy Name** field, enter "cb-policy" and in the **Policy Document** paste the policy definition.  You can either copy it from the section preceding these steps or download and copy it from [here](https://docs.hortonworks.com/HDPDocuments/Cloudbreak/Cb-doc-resources/cb-policy.json).

    <a href="../images/aws_role4.png" target="_blank" title="click to enlarge"><img src="../images/aws_role4.png" width="650" title="IAM Console"></a>  
    
7. When done, click **Create Policy**.

8. Click **Refresh**. Next, find the "cb-policy" that you just created and select it by checking the box.

    <a href="../images/aws_role5.png" target="_blank" title="click to enlarge"><img src="../images/aws_role5.png" width="650" title="IAM Console"></a> 
    
9. When done, click **Next: Review**.
    
10. In the **Roles name** field, enter role name, for example "CredentialRole". 

    <a href="../images/aws_role6.png" target="_blank" title="click to enlarge"><img src="../images/aws_role6.png" width="650" title="IAM Console"></a> 
    
11. When done, click **Create role** to finish the role creation process. 

Once you are done, you can proceed to [Launch the VM](#launch-the-vm).  

**Related Links**  
[Using Instance Profiles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html) (External)  
[Using an IAM Role to Grant Permissions to Applications](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html) (External)   


### Launch the VM  

Now that you've met the prerequisites, you can launch the Cloudbreak deployer VM available as a Community AMI.

**Steps**

1. On AWS, navigate to the EC2 Console.  

2. In the top right corner, select the region in which you want to launch Cloudbreak.  

    <a href="../images/aws-01.png" target="_blank" title="click to enlarge"><img src="../images/aws-01.png" width="650" title="EC2 Console"></a> 
    
3. From the left pane, select **INSTANCES** > **Instances**.  

4. Click on **Launch Instance**.

5. In *Step 1: Choose an Amazon Machine Image (AMI)*, select **Community AMIs** from the left pane. 

    <a href="../images/aws-02.png" target="_blank" title="click to enlarge"><img src="../images/aws-02.png" width="650" title="EC2 Console"></a> 
     
6. In the search box, enter the image name. The following Cloudbreak deployer images are available:

    | Region Name | Region | Community AMI |
|---|---|---|
| EU (Ireland) | eu-west-1 | ami-6edb5817 |
| EU (Frankfurt) | eu-central-1 |	 ami-665ecb09 |
| US East (N. Virginia) | us-east-1 | ami-82324cf8 |
| US West (N. California) | us-west-1 | ami-a53532c5 |
| US West (Oregon) | us-west-2 | ami-d88725a0 | 
| South America (São Paulo) | sa-east-1 | ami-a56627c9 |
| Asia Pacific (Tokyo) | ap-northeast-1	| ami-d40d84b2 |
| Asia Pacific (Singapore) | ap-southeast-1 | ami-02523e7e | 
| Asia Pacific (Sydney) | ap-southeast-2 | ami-afd726cd |

[comment]: <> (TO-DO: This table should be automatically generated.)

7. Click **Select**.  

    > The steps listed below only mention required parameters. You may optionally review and adjust additional parameters. 
  
8. In *Step2: Choose Instance Type*, choose an instance type. The minimum instance type which is suitable for Cloudbreak is **m3.large**. Minimum requirements are 8GB RAM, 10GB disk, 2 cores. When done, click **Next**.

    <a href="../images/aws-03.png" target="_blank" title="click to enlarge"><img src="../images/aws-03.png" width="650" title="EC2 Console"></a>   
    
9. (Perform this step only if you are using role-based authorization) In **Step 3: Configure Instance Details** > **IAM**, select the "CloudbreakRole" IAM role which you [created earlier](#authorization-for-cloudbreak).

10. In *Step 6: Configure Security Group*, open the following ports:
 
    * 22 (for access via SSH)  
    * 80 (for access via HTTP)   
    * 443 (for access via HTTPS). 

    <a href="../images/aws-04.png" target="_blank" title="click to enlarge"><img src="../images/aws-04.png" width="650" title="EC2 Console"></a> 
    
    When done, click **Review and Launch**.

12. In *Step 7: Review Instance Launch*, review the information carefully and then click **Launch**. 

13. When prompted select an existing key pair or create a new one. Next, acknowledge that you have access to the private key file and click **Launch Instance**. 

    <a href="../images/aws-05.png" target="_blank" title="click to enlarge"><img src="../images/aws-05.png" width="400" title="EC2 Console"></a>  
    
14. Click on the instance ID to navigate to the **Instances** view in your EC2 console. 

    <a href="../images/aws-06.png" target="_blank" title="click to enlarge"><img src="../images/aws-06.png" width="650" title="EC2 Console"></a>  


### SSH to the VM  

Now that your VM is ready, access it via SSH: 
  
* Use the private key from the key pair that you selected when launching the instance. 
* The SSH user is called "cloudbreak".
* You can obtain the host IP from the EC2 console > **Instances** view by selecting the instance, selecting the **Description** tab, and copying the value of the **Public DNS (IPv4) ** or **IPv4 Public IP** parameter.

On Mac OS X, you can SSH to the VM by running the following from the Terminal app: `ssh -i "your-private-key.pem" cloudnreak@instance_IP` where "your-private-key.pem" points to the location of your private key and "instance_IP" is the public IP address of the VM.

On Windows, you can use [PuTTy](http://www.putty.org/).


### Launch Cloudbreak Deployer 

After accessing the VM via SSH, launch Cloudbreak deployer using the following steps.  

**Steps**  

1. Navigate to the cloudbreak-deployment directory:

    <pre>cd /var/lib/cloudbreak-deployment/</pre>
    
    This directory contains configuration files and the supporting binaries for Cloudbreak deployer.
   
    
2.  Initialize your profile by creating a new file called `Profile` and adding the following content:

    <pre>export UAA_DEFAULT_SECRET=MY-SECRET
export UAA_DEFAULT_USER_PW=MY-PASSWORD
export UAA_DEFAULT_USER_EMAIL=MY-EMAIL</pre>  

    For example: 

    <pre>export UAA_DEFAULT_SECRET=MySecret123
export UAA_DEFAULT_USER_PW=MySecurePassword123
export UAA_DEFAULT_USER_EMAIL=dbialek@hortonworks.com</pre> 

    > You will need to provide the email and password when logging in to the Cloudbreak web UI and when using the Cloudbreak CLI. The secret will be used by Cloudbreak for authentication.  
    
3. Start the Cloudbreak application by using the following command:

    <pre>cbd start</pre>
    
    This will start the Docker containers and initialize the application. The first time you start the Coudbreak app, this also downloads of all the necessary docker images.
    
[Comment]: <> (Extra info which may not be needed here: The `cbd start` command includes the `cbd generate` command which applies the following steps: Creates the `docker-compose.yml` file, which describes the configuration of all the Docker containers needed for the Cloudbreak deployment. Creates the `uaa.yml` file, which holds the configuration of the identity server used to authenticate users with Cloudbreak.)

    Once the `cbd start` has finished, it returns the "Uluwatu (Cloudbreak UI) url" which you can later paste in your browser and log in to Cloudbreak web UI. 

5. Check Cloudbreak deployer version and health: 
    
    <pre>cbd doctor</pre>
    
5. Check Cloudbreak Application logs: 

    <pre>cbd logs cloudbreak</pre>
    
    You should see a message like this in the log: `Started CloudbreakApplication in 36.823 seconds.` Cloudbreak takes less than a minute to start. If you try to access the Cloudbreak UI before Cloudbreak started, you will get a "Bad Gateway" error or "Cannot connect to Cloubdreak" error.
    

### Access Cloudbreak UI

Log in to the Cloudbreak UI using the following steps.

**Steps**

1. You can log into the Cloudbreak application at `https://IPv4_Public_IP>/` or `https://Public_DNS`. For example `https://34.212.141.253` or `https://ec2-34-212-141-253.us-west-2.compute.amazonaws.com`. 

2. Confirm the security exception to proceed to the Cloudbreak web UI.

    The first time you access Cloudbreak UI, Cloudbreak will automatically generate a self-signed certificate, due to which your browser will warn you about an untrusted connection and will ask you to confirm a security exception.

3. The login page is displayed:

    <a href="../images/cb-ui.png" target="_blank" title="click to enlarge"><img src="../images/cb-ui.png" width="650" title="Cloudbreak web UI"></a>  

4. Log in to the Cloudbreak web UI using the credential that you configured in your `Profile` file when [launching Cloudbreak deployer](#launch-cloudbreak-deployer):

    * The username is the `UAA_DEFAULT_USER_EMAIL`     
    * The password is the `UAA_DEFAULT_USER_PW` 

5. Upon a successful login, you are redirected to the dashboard:

    <a href="../images/cb-ui1.png" target="_blank" title="click to enlarge"><img src="../images/cb-ui1.png" width="650" title="Cloudbreak web UI"></a>  


### Create Cloudbreak Credential

Before you can start creating clusters, you must first create a [Cloudbreak credential](architecture.md#cloudbreak-credential). Without this credential, you will not be able to create clusters via Cloudbreak. 

As part of the [prerequisites](#authentication), you had two options to allow Cloudbreak to authenticate with AWS and create resources on your behalf: key-based or role-based authentication. Depending on your choice, you must configure a key-based or role-based credential: 

* [Create Key-Based Credential](#create-key-based-credential)  
* [Create Role-Based Credential](#create-role-based-credential)

#### Create Key-Based Credential

To perform these steps, you must know your access and secret key. If needed, you or your AWS administrator can generate new access and secret keys from the **IAM Console** > **Users** > select a user > **Security credentials**. 

**Steps**

1. In the Cloudbreak web UI, select **Credentials** from the navigation pane. 

2. Click **Create Credential**. 

3. Under **Cloud provider**, select "Amazon Web Services":

    <a href="../images/cb-aws-cred-key.png" target="_blank" title="click to enlarge"><img src="../images/cb-aws-cred-key.png" width="650" title="Cloudbreak web UI"></a>  

3. Provide the following information:

    | Parameter | Description |
|---|---|
| Select Credential Type | Select **Key Based**. | 
| Name | Enter a name for your credential. |
| Description | (Optional) Enter a description. | 
| Access Key | Paste your access key. |
| Secret Access Key | Paste your secret key. |
 
4. Click **Create**.

5. Your credential should now be displayed in the **Credentials** pane.

    
    Congratulations! You've successfully launched Cloudbreak and create a Cloudbreak credential. Now it's time to [create a cluster](aws-create.md). 
 

#### Create Role-Based Credential

To perform these steps, you must know the **IAM Role ARN** corresponding to the "CredentialRole" (configured as a [prerequisite](#authorization-for-cloudbreak)).  

**Steps**

1. In the Cloudbreak web UI, select **Credentials** from the navigation pane. 

2. Click **Create Credential**. 

3. Under **Cloud provider**, select "Amazon Web Services":

    <a href="../images/cb-aws-cred-role.png" target="_blank" title="click to enlarge"><img src="../images/cb-aws-cred-role.png" width="650" title="Cloudbreak web UI"></a>  

3. Provide the following information:

    | Parameter | Description |
|---|---|
| Select Credential Type | Select **Role Based** (default value). | 
| Name | Enter a name for your credential. |
| Description | (Optional) Enter a description. | 
| IAM Role ARN | Paste the IAM Role ARN corresponding to the "CredentialRole" that you created earlier. For example `arn:aws:iam::315627065446:role/CredentialRole` is a valid IAM Role ARN. |


4. Click **Create**.

5. Your credential should now be displayed in the **Credentials** pane.

    Congratulations! You have successfully launched Cloudbreak and created a Cloudbreak credential. Now you can use Cloubdreak to [create clusters](aws-create.md). 
      

<div class="next">
<a href="../aws-create/index.html">Next: Create a Cluster</a>
</div>

