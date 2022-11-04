![Node.js CI](https://github.com/nodeshift-starters/nodejs-health-check/workflows/ci/badge.svg)
[![Coverage Status](https://coveralls.io/repos/github/nodeshift-starters/nodejs-health-check/badge.svg?branch=main)](https://coveralls.io/github/nodeshift-starters/nodejs-health-check?branch=main) 

Using Openhsift Binary Builds to Package NodeJS Applications into Images Using S2I Images with Opesnhift

## Relevenat Links
* [Creating images from source code with source-to-image](https://access.redhat.com/documentation/en-us/openshift_container_platform/4.11/html/images/creating-images#images-create-s2i_create-images)
* [Images](https://access.redhat.com/documentation/en-us/openshift_container_platform/4.11/html/images/index)
* [Creating applications using the CLI](https://access.redhat.com/documentation/en-us/openshift_container_platform/4.11/html-single/building_applications/index#creating-applications-using-cli)
* [Building Applications](https://access.redhat.com/documentation/en-us/openshift_container_platform/4.11/html/building_applications/index)


## Running The Example

You can run this example as node processes on your localhost, as pods on a local
[OpenShift Local](https://developers.redhat.com/products/openshift-local/overview) installation.

### Localhost

To run the basic application on your local machine, just run the commands bellow:

```
$ npm install
$ npm start
```

This will launch the application on port 8080.

Other options:

* `npm run dev` same as `npm start` but with pretty output log.
* `npm run dev:debug` shows debug information. 

### OpenShift

#### Using Openshift Builds Directly

An OpenShift cluster should be available, and you should be logged in with a currently
active project. A buildconfig will be created and then the source will be piped to a
new build with the resulting image being used as the image for a new application deployment.

```sh
$ oc login -u <username> # Login
$ oc project <project_name> # Set the current project to deploy to
$ oc get is -n openshift | grep nodejs # Retrieve the current image streams in the openshift project and filter on those for nodejs
$ oc new-build --name <build_and_image_name> --binary -i <source_S2I_image> -e NPM_MIRROR=<npm_mirror_url> # Create a buildconfig and imagestream for the resultant image
$ oc start-build <build_and_image_name> --from-dir=./
```
* Where <username> is the username of an Openshift account that has access to the destination project
* Where <project_name> is the name of the destination project
* Where <build_and_image_name> is the name that will be used for the buildConfig and the resulting image
* Where <source_S2I_image> is the image stream location of the base nodejs S2I image, in the openshift directory, i.e: openshift/nodejs:14-ubi8
* (Optional) Where <npm_mirror_url> is the URL to a local / corporate NPM mirror

[Source-to-Image for NodeJS](https://github.com/sclorg/s2i-nodejs-container)
[Environment variables for Source-to-Image for NodeJS 16](https://github.com/sclorg/s2i-nodejs-container/tree/master/16)

#### Using NodeShift Plugin

An OpenShift cluster should be available, and you should be logged in with a currently
active project. Then run the `npm run openshift` command.

```sh
$ oc login -u developer # Login
$ oc project <project_name> # Set the current project to deploy to
$ npm run openshift # Deploys the example app
```

### OpenTelemetry with OpenShift Distributed Tracing Platform

This [link](./OTEL.md) contains instructions on how to install the 
OpenShift Distributed Tracing Platform and enable tracing.
