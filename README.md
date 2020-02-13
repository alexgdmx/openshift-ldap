# openshift-ldap
1. The LDAP authentication provider expects the server certificate to be provided via a ConfigMap in the **openshift-config** project. The expected key for the certificate is **ca.crt**.
```bash
oc create configmap XXXXX-ldap-ca-cert --from-file=ca.crt=$HOME/ldap-ca.crt -n openshift-config
```
2. It also expects the bind password to be provided via a secret. The expected key is **bindPassword**.
```bash
oc create secret generic XXXXX-ldap-secret --from-literal=bindPassword=<your-password-value> -n openshift-config
```
3. Finally create the LDAP Provider (use oc apply to replace the default provider with name **cluster**):
```bash
oc apply -f $HOME/XXXXX-ldap.yaml
```
Sample output:
```bash
Warning: oc apply should be used on resource created by either oc create --save-config or oc apply
oauth.config.openshift.io/cluster configured
```
4. Validate that your config is there:
```bash
oc get oauths
```
5. Test the connection.
```bash
oc login -u <ldap-user> -p <your-password>
```
Sample output:

```bash
Login successful.

You don't have any projects. You can try to create a new project, by running

    oc new-project <projectname>
```
