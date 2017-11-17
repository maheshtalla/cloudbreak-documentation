
## Upgrade Cloudbreak

### Update Cloudbreak Deployer 

To upgrade Cloudbreak to the newest version, perform the following steps.

In Cloudbreak 2.1.0, you need sudo access in order to run cbd commands. Run `sudo su` to get sudo access. 

[Comment]: <> (This is only for 2.1.0)

**Steps**

1. On the VM where Cloudbreak is running, navigate to the directory where your Profile file is located:

    <pre>cd /var/lib/cloudbreak-deployment/</pre>

2. Stop all of the running Cloudbreak components:

    <pre>cbd kill</pre>
    
3. Update Cloudbreak deployer:

    <pre>cbd update</pre>
    
3. Update the `docker-compose.yml` file with new Docker containers needed for the cbd:

    <pre>cbd regenerate</pre>
    
4. If there are no other Cloudbreak instances that still use old Cloudbreak versions, remove the obsolete containers:

    <pre>cbd util cleanup</pre>
    
5. Check the health and version of the updated cbd:

    <pre>cbd doctor</pre>
    
6. Start the new version of the cbd:

    <pre>cbd start</pre>
    
    Cloudbreak needs to download updated docker images for the new version, so this step may take a while.

In addition, if you have any clusters running, you must update them using the following steps. 

### Update Existing Clusters

[comment]: <> (TO-DO: Maybe the sentence below should say "Upgrading from version 1.4.0 **or newer** to the newest version"??)

Upgrading from version 1.4.0 to the newest version does not require any manual modification from the users.

Upgrading from version 1.3.0 to the newest version requires that you update existing clusters. To update existing clusters, run the following commands on the `cbgateway` node of the cluster.

**Steps**

1. Update the version of the Salt-Bootsrap tool on the nodes:
    <pre>salt '*' cmd.run 'curl -Ls https://github.com/sequenceiq/salt-bootstrap/releases/download/v0.1.2/salt-bootstrap_0.1.2_Linux_x86_64.tgz | tar -zx -C /usr/sbin/ salt-bootstrap'</pre>
    
2. Trigger restart of the tool on the nodes:

    <pre>salt '*' service.dead salt-bootstrap</pre>

    > To check the version of the Salt-Bootsrap on the nodes, use `salt '*' cmd.run 'salt-bootstrap --version'`
    