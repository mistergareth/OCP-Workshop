# 3 Users & Roles

## 3.1 Inspect Configuration

At this time, we have not configured any additional user accounts or roles.  So our next activity will be to do just that.

The OCP parameters we used at installation automatically configured the cluster for user credentials to be defined in **/etc/origin/master/openshift-passwd** and managed  by the commandline utility **htpasswd** (ie: httpd-tools).  This configuration is defined in **/etc/origin/master/master-config.yaml** on the host **master.example.com**.

### Inspect the master-config.yaml

Use SSH to connect to master.example.com as user *root*

```
: [root@workstation ~]#

ssh master.example.com
```

If you are not familiar with grep utility, the parameter `-6` will provide the 6 lines above and 6 lines below the requested matched.  Thus we can easily inspect the entire stanza defining the IdentityProviders. 

```
: [root@master ~]#

grep -6 htpasswd_auth /etc/origin/master/master-config.yaml
```

Your results should look like this.  Pay attention to `file: /etc/origin/master/htpasswd` and `kind: HTPasswdPasswordIdentityProvider`.

<em>

```
grantConfig:
  method: auto
identityProviders:
- challenge: true
  login: true
  mappingMethod: claim
  name: htpasswd_auth
  provider:
    apiVersion: v1
    file: /etc/origin/master/htpasswd
    kind: HTPasswdPasswordIdentityProvider
masterCA: ca-bundle.crt
masterPublicURL: https://master.example.com:8443
```
</em>

This confirms that the cluster is properly configured for the purposes of this workshop.

## 3.2 Configure User *admin*

### Add User

Add the user *admin* with password *redhat*

```
: [root@master ~]#

htpasswd -b /etc/origin/master/htpasswd admin redhat
```

### Assign role *cluster-admin*

Provide the *admin* user with the *cluster-admin* role

```
: [root@master ~]#
    
oc adm policy add-cluster-role-to-user cluster-admin admin
```

### Use the new credential

Now you can use this new credential to log into Openshift.  Remember that you declared the password for user *admin* above when you ran `htpasswd`.

```
: [root@master ~]#

oc login -u admin
```

# End of Unit

[Return to Index](https://github.com/xtophd/OCP-Workshop/tree/master/documentation "OCP-Workshop Index")
