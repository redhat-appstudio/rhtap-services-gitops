# rhtap-services gitops repo

This repo contains gitops definitions for services needed for testing RHTAP/TSSC like Jenkins, Artifactory, Hive, ...

## Deployment

Steps to deploy these services on new cluster:
1. Edit `./envfile` - fill out the secrets needed.
2. Run `./create_secrets.sh`. This will create the secrets on your cluster
3. Run `./bootstrap.sh`. 
    * This script installs Opehsift Gitops and creates initial app-of-apps.

## Development

### Adding new component

* Create your component `Application` CR in `./app-of-apps` folder. 
* Create new folder in `./components` folder holding all the resources of your new component.
