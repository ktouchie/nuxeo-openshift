# Openshift PS projects packaging


***N.B.: Work In Progress***

*For now, this section is very early stage. It is not available for a proper usage yet. Contributions and pull requests would be very welcome but in the future.*


## State of art

Today at Nuxeo, thanks to some Nuxeo engineers (Damien), we can provide Cloud development environments managed in OpenShift. We have already used them for a few PS projects. Indeed, we have an OpenShift template that can provide:

- 2 configurable environments (DEV and UAT), with a Mongo database and ElasticSearch
- a CD pipeline.


## Limitations

This OpenShift template doesn't enable to run a CI chain nor perform a Release directly via OpenShift. Also, it relies on s2i (source-to-image) which causes heavy Docker images.


## Goal

The goal is to be able to provide full Cloud development environments managed in OpenShift, including:

- 2 configurable environments (DEV and UAT)
- a CI+CD pipeline
- a Release pipeline.

This could possibly be packaged within an OpenShift template or an Ansible PlayBook Bundle, in order to be available when creating future OpenShift projects.


## Content

As a starting point, this folder gathers the YAML scripts to use to build this target packaging.

**Services:**

- a Jenkins service: It is based on the jenkins-ephemeral-template, uses additional plugins (maven-plugin, pipeline-maven, file-operations...), specific maven settings and git version.

**Resources:**

- a Config Map for Maven settings
- a Config Map for Git config
- a Secret for Github SSH key
- a Secret for Connect Credentials
- a Secret for Studio username and password
- a Secret for System username and password
- a Secret for instance.clid file

**Images and builds:**

- a Docker base image: The input base Nuxeo image (9.10)
- a base image Docker build: It takes as input the base image, then it installs librairies (imagemagick...)
- an app image Docker build: It takes as input the previously built Nuxeo image as well as a marketplace package zip, then it installs the marketplace package on the Nuxeo image
- a Docker app Image: The output final image that will have the marketplace package installed and that will be deployed

**Pipelines:**

- a CI/CD pipeline: It defines a Github webhook, it checks out a Github repository (with Github credentials), runs a Maven build (with Maven settings and Studio credentials), generates a marketplace package zip, launches the app image Docker build, deploys the output image to DEV (and UAT) environment. *(In the future: it may include functional testing)*

- a release pipeline: It checks out a Github repository (with Github credentials), sets up the Jenkins workspace (by handling maven settings, ssh key, instance.clid, gitconfig), downloads the Nuxeo Devtools python release script (and its required resources), launches a release prepare and release perform.

## JIRA

See [https://jira.nuxeo.com/browse/NXCT-57](https://jira.nuxeo.com/browse/NXCT-57) for status and more details.


## Contributors

Estele Giuly (egiuly@nuxeo.com)


