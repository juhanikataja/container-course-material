# Exercise 5 - Applying what you've learned so far

## Prerequisites

Completed exercises 0-4.

## Learning objectives

* You are able to apply what you've learned so far about OpenShift to modify
  an existing site.

## Description

In this exercise the objective is to take the template from exercise 4 and
modify it so that instead of NGINX, the site is hosted using Apache. You can
refer to the previous exercises, other course material and the documentation for
Kubernetes and OpenShift to help you along.

Some hints:
* The official "httpd" image will try to bind port 80 and thus won't work with
  OpenShift.
* You can look for images in the
  [Red Hat Container Catalog](https://access.redhat.com/containers/).
  * Some images here may require a Red Hat subscription
* The CentOS project also maintains images compatible with OpenShift:
  [CentOS in the Docker Hub](https://hub.docker.com/u/centos/)
* The default location where Apache looks for web content is not the same as it
  was with NGINX - you should take this into account when mounting volumes and
  copying data into the site. You can find out what path to use by looking at
  the documentation for the image in whichever registry you choose to use.
* Use `oc rsh` and `oc debug` to log in to the running or dead containers and
  inspect how the volume mounts look like inside the container.

## Relevant documentation

* [Red Hat Container Catalog](https://access.redhat.com/containers/)
* [CentOS in the Docker Hub](https://hub.docker.com/u/centos/)
* [Kubernetes: Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
* [Kubernetes: Persistent volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
* [Kubernetes: Services](https://kubernetes.io/docs/concepts/services-networking/service/)
* [OpenShift: Routes](https://docs.openshift.org/3.6/architecture/networking/routes.html)
* [OpenShift: Copying Files to or from a Container](https://docs.openshift.org/latest/dev_guide/copy_files_to_container.html)
* [OpenShift Origin: Creating New Applications](https://docs.openshift.org/3.6/dev_guide/application_lifecycle/new_app.html)

## Steps

1. You can use an existing project or create a new one. If you decide to use an
   existing project, make sure you don't try to create any API objects whose
   name clashes with objects already in the project.

2. Make a copy of the **Template** file from the previous exercise and modify it
   so that it uses Apache instead of NGINX.

3. Add the newly modified **Template** to OpenShift.

4. Create an Apache site with `oc new-app`.

5. Upload the `index.html` file from exercise 3 to the new site.

6. You should see the same site as before in your browser, but this time served
   by Apache instead.
