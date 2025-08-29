# rhtap-services gitops repo

This repo contains gitops definitions for services needed for testing RHTAP/TSSC like Jenkins, Artifactory, Hive, ...

## Deployment

Steps to deploy these services on new cluster:
1. Edit `./envfile` - fill out the secrets needed.
2. Run `./create_secrets.sh`. This will create the secrets on your cluster
3. Run `./bootstrap.sh`. 
    * This script installs Opehsift Gitops and creates initial app-of-apps.

### Artifactory - After deployed on new cluster perform the following to setup.
1. Login to Artifactory-JCR console and complete "Inital Setup"
   Perform the following command to get console url
```
$ echo "https://$(kubectl -n artifactory get route artifactory-ui -o jsonpath='{.spec.host}')"
```
The default username and password for the built-in administrator user is:
admin/password

Accept EULA, fill in a new password and all others can be skipped in the navigation window when you first login to Artifactory-JCR console.

2. Create a Local Docker Repository

Go to `Administration` -> `Repositories` -> `Create a Repository` -> `Local`, Select Package Type `Docker`, fill in `Repository Key` field with `rhtap`, finally click button `Create Local Repository`.

3. Set Me Up - Docker Repository

Go to `Application` -> `JFrog Container Registry` -> `Artifacts`, Click the Docker Repository created in step  `2. Create a Local Docker Repository`, Click `Set Me Up` to set up a generic client

When you input JFrog account password, it generates an identity token and Docker auth contenxt, like below

```
{
        "auths": {
                "https://artifactory-web-artifactory-jcr.apps.rosa.xjiang0212416.jgt9.p3.openshiftapps.com" : {
                        "auth": "xxxxxxxxxxxxxxx",
                        "email": "youremail@email.com"
                }
        }
}
```
<span style="color:red"> Notice: Once `Set Me Up` is done , don't click it again, otherwise the token will be changed.</span>

4. Verify pushing image to Artifactory
Perform the following command to get registry hostname.

```
$ kubectl -n artifactory get route artifactory -o jsonpath='{.spec.host}'
```

Push image to `rhtap` repository on Artifactory server

```
$ podman tag docker.io/mysql:latest <registry hostname>/rhtap/mysql:latest
$ <Copy the Docker auth content into a file, for instance auth.json>
$ podman push --authfile <auth.json> <registry hostname>/rhtap/mysql:latest
```

## Development

### Adding new component

* Create your component `Application` CR in `./app-of-apps` folder. 
* Create new folder in `./components` folder holding all the resources of your new component.


