# IBM Cloud Foundry Deploy

GitHub action for deploying applications to IBM Cloud Foundry.

## Prerequisites

- [IBM Cloud account](https://cloud.ibm.com)
- [IBM Cloud Foundry app](https://cloud.ibm.com/cloudfoundry/overview)
- [IBM Cloud API key](https://cloud.ibm.com/iam/apikeys)

## Setup

### Parameters

**DISCLAIMER**: It's recommended that you use [GitHub encrypted secrets](https://docs.github.com/en/actions/reference/encrypted-secrets) to keep your sensitive data safe.

* IBM_CLOUD_API_KEY

  The IBM Cloud API key is necessary to authenticate this GitHub Action with IBM. Details on how to create an API key can be found [here](https://cloud.ibm.com/docs/account?topic=account-userapikey#create_user_key).

* IBM_CLOUD_CF_API

  The IBM Cloud Foundry API endpoint is used to connect and to deploy the application. A list of supported regions and API Endpoints can be found [here](https://cloud.ibm.com/docs/cloud-foundry-public?topic=cloud-foundry-public-endpoints).

* IBM_CLOUD_CF_SPACE

  Apps in an IBM Cloud Foundry Enterprise Environment are scoped within specific spaces. A space exists within a specific organziation.
A guide on how to create organzizations and spaces can be found [here](https://cloud.ibm.com/docs/cloud-foundry?topic=cloud-foundry-create_orgs).

* IBM_CLOUD_CF_ORG

  The Cloud Foundry organization name in which the `IBM_CLOUD_CF_SPACE` exists.

### Workflow Example

```yml
name: Deploy to IBM Cloud Foundry
on:
  push:
    branches: [ master ]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Deploy to IBM Cloud Foundry
        # You may pin to the exact commit or the version.
        uses: IBM/cloudfoundry-deploy@master
        with:
          IBM_CLOUD_API_KEY: ${{ secrets.IBM_CLOUD_API_KEY }}
          IBM_CLOUD_CF_API: ${{ secrets.IBM_CLOUD_CF_API }}
          IBM_CLOUD_CF_ORG: ${{ secrets.IBM_CLOUD_CF_ORG }}
          IBM_CLOUD_CF_SPACE: ${{ secrets.IBM_CLOUD_CF_SPACE }}
```

### Application manifest

This action uses a manifest file to deploy the application. Manifests provide consistency, reproducibility and can be used for customizations like number of instances, disk space limit, and memory limit. For a full list of attributes you can specify in an application manifest, see [App Manifest Attribute Reference](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest-attributes.html).

### Example

Create a `manifest.yml` file in the root directory of your application with the follogin content:

```yml
---
applications:
- name: YOUR-APP
```

Where `YOUR-APP` is the name of your application.
