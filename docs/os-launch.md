## Launching Cloudbreak on OpenStack

These steps describe how to launch Cloudbreak on OpenStack. This is the only deployment options available on OpenStack.  
Before launching Cloudbreak on OpenStack, review and meet the [prerequisites](os-pre.md). Next, follow the steps below. 


{!docs/common/vm-req.md!}

{!docs/common/vm-launch.md!} 


### Configure a self-signed certificate

If your OpenStack is secured with a self-signed certificate, you need to import that certificate into Cloudbreak, or else Cloudbreak won't be able to communicate with your OpenStack.

To import the certificate, place the certificate file in the `/certs/trusted/` directory, follow these steps.

**Steps**

1. Navigate to the `certs` directory (automatically generated).
2. Create the `trusted` directory.
3. Copy the certificate to the `trusted` directory.

Cloudbreak will automatically pick up the certificate and import it into its trust store upon start.


### Create Cloudbreak credential

Cloudbreak works by connecting your OpenStack account through this credential, and then uses it to create resources on your behalf. Before you can start provisioning cluster using Cloudbreak, you must create a [Cloudbreak credential](concepts.md#cloudbreak-credential).

**Steps**

1. In the Cloudbreak web UI, select **Credentials** from the navigation pane.

2. Click **Create Credential**.

3. Under **Cloud provider**, select "Google Cloud Platform".

    <a href="../images/cb_cb-os-cred.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-os-cred.png" width="650" title="Cloudbreak web UI"></a>

3. Select the keystone version.

4. Provide the  following information:

    For Keystone v2:

    | Parameter | Description |
|---|---|
| Name | Enter a name for your credential. |
| Description | (Optional) Enter a description. |
| User | Enter your OpenStack user name. |
| Password | Enter your OpenStack password. |
| Tenant Name | Enter the OpenStack tenant name. |
| Endpoint | Enter the OpenStack endpoint. |
| API Facing | (Optional) Select *public*, *private*, or *internal*. |

    For Keystone v3:

    | Parameter | Description |
|---|---|
| Keystone scope | Select the scope: default, domain, or project. |
| Name | Enter a name for your credential. |
| Description | (Optional) Enter a description. |
| User | Enter your OpenStack user name. |
| Password | Enter your OpenStack password. |
| User Domain | Enter your OpenStack user domain. |
| Endpoint | Enter the OpenStack endpoint. |
| API Facing | (Optional) Select *public*, *private*, or *internal*. |

[comment]: <> (Not sure what these params do: Keystone scope, User Domain)

4. Click **Create**.

5. Your credential should now be displayed in the **Credentials** pane.

    Congratulations! You have successfully launched Cloudbreak and created a Cloudbreak credential. Now you can use Cloudbreak to [create clusters](os-create.md).

<div class="next">
<a href="../os-create/index.html">Next: Create a Cluster</a>
</div>



