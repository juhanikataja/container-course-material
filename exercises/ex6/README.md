# Exercise 6 - Build an app from a Dockerfile

## Prerequisites

You have logged into OpenShift with the `oc` tool.

## Learning objectives

* How to take an existing Docker project and run it in OpenShift

## Description

So far we have looked at how to create your own API objects in OpenShift and how
these API objects can be compiled into a **Template** for a complete site. The
goal has been to understand some of the building blocks of a site hosted on
OpenShift. In this exercise we are going to look at how OpenShift can
automatically generate these building blocks for you based on a Git repository.

## Relevant documentation

* [OpenShift Origin: Creating New Applications](https://docs.openshift.org/3.6/dev_guide/application_lifecycle/new_app.html)
* [OpenShift Origin: How Builds Work](https://docs.openshift.org/3.6/dev_guide/builds/index.html)
* [OpenShift Origin: Builds and Image Streams ](https://docs.openshift.com/container-platform/3.6/architecture/core_concepts/builds_and_image_streams.html)
* [OpenShift Origin: Routes](https://docs.openshift.org/3.6/architecture/networking/routes.html)
* [Kubernetes: Replication Controller](https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/)

## Steps

1. There is an example application hosted under the Digipalvelutehdas GitHub
   organisation here:
   [oso-course-docker-example](https://github.com/Digipalvelutehdas/oso-course-docker-example)
   Have a look at that repository to see what it contains.

2. Create a new app in OpenShift based on the repository:
   ```bash
   oc new-app https://github.com/Digipalvelutehdas/oso-course-docker-example.git
   ```

3. Have a look at what API objects were automatically created by OpenShift:
   ```bash
   oc get all
   ```
   Notice that there are some API object types that we haven't covered yet, like
   a BuildConfig, an ImageStream and a ReplicationController. You can find more
   about these API object types under the Relevant documentation section.

4. You can get more information about a given API object by specifying
   `-o yaml`, for example:
   ```bash
   oc get dc/oso-course-docker-example -o yaml
   ```

5. OpenShift has not created a **Route** for you yet, so the application is not
   yet accesible via a web browser. You can create the **Route** yourself based
   on the examples in the previous exercises or via the web interface. Once
   you have done that, you should be able to access the site with its familiar
   content in your favorite browser. Notice that this time the **Service**
   exposes port 8080 instead of port 80.
