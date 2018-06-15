## Create Cloudbreak credential

Cloudbreak works by connecting your GCP account through this credential, and then uses it to create resources on your behalf. Before you can start provisioning cluster using Cloudbreak, you must create a [Cloudbreak credential](https://hortonworks.github.io/cloudbreak-documentation/latest/).  

**Prerequisites**

In order to launch clusters on GCP via Cloudbreak, you must have a service account that Cloudbreak can use to create resources. If you do not have one, refer to [Prerequisites: Service account](https://hortonworks.github.io/cloudbreak-documentation/latest/). 

**Steps**

1. In the Cloudbreak web UI, select **Credentials** from the navigation pane. 

2. Click **Create Credential**. 

3. Under **Cloud provider**, select "Google Cloud Platform":

    <img src="../images/cb_cb-gcp-cred.png" width="650" title="Cloudbreak web UI">  

3. Provide the following information:

    | Parameter | Description |
|---|---|
| Key type | Select JSON or P12. Since activating service accounts with P12 private keys has been deprecated in the Cloud SDK, we recommend using JSON. |
| Name | Enter a name for your credential. |
| Description | (Optional) Enter a description. | 
| Project Id | (Only required for P12 key type) Enter the project ID. You can obtain it from your GCP account by clicking on the name of your project at the top of the page and copying the **ID**. |
| Service Account Email Address | (Only required for P12 key type) "Service account ID" value for your service account created in prerequisites. You can find it on GCP at **IAM & Admin** > **Service accounts**. |
| Service Account Private Key | Upload the key that you created in the prerequisites when creating a service account. |

4. Click **Create**.

5. Your credential should now be displayed in the **Credentials** pane.

    Congratulations! You have successfully launched Cloudbreak and created a Cloudbreak credential. Now you can use Cloudbreak to create clusters. 

