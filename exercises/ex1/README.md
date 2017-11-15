# Exercise 1 - Deploying NGINX

## Prerequisites

* Terminal open with a login session to OpenShift using `oc`
* Project created and in use (check with `oc status`)

## Learning objectives

* Basic command line usage
  * Creating API objects
  * Getting information about existing API objects
  * Updating existing API objects
* Security restrictions related to OpenShift containers compared to plain Docker
  containers

## Description

The exercise directory contains a set of YAML files that describe a very simple
stateless deployment of NGINX. You can have a look at them with your favorite
editor to see what they contain. We will use these files to set up NGINX and see
that we can access the newly create NGINX application via a web browser.

## Relevant documentation

* [Kubernetes: Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
* [Kubernetes: Services](https://kubernetes.io/docs/concepts/services-networking/service/)
* [OpenShift: Routes](https://docs.openshift.org/3.6/architecture/networking/routes.html)

## Steps

1. Create a new NGINX **Deployment** using the nginx-deployment.yaml file:
   ```bash
   oc create -f nginx-deployment.yaml
   ```
   This is going to create a **ReplicaSet** with two **Pods** that run NGINX.

2. Create a **Service** for the **Deployment**:
   ```bash
   oc create -f nginx-service.yaml
   ```
   This will create a stable IP and DNS name for the **Pods** in the
   **Deployment**.

3. Create a **Route** to the newly created **Service**:
   ```bash
   oc create -f nginx-route.yaml
   ```
   This will create a URL in the app domain where the NGINX app is reachable.

4. Inspect the status of the **Pods** created by the **Deployment**:
   ```bash
   oc get pods
   ```
   Notice that the **Pods** are not working correctly. This is because of
   security restrictions in OpenShift that enable it to be a multi-tenant cloud
   platform.

5. You will need to replace the NGINX image with one that is compatible with
   OpenShift. You can use either `oc edit` or `oc replace -f` to achieve this.
   If you have access to Docker, you can get a Dockerfile by the NGINX folk
   here:
   [nginxinc/openshift-nginx](https://github.com/nginxinc/openshift-nginx).
   If you don't have access to Docker, you can also get the image at
   `docker-registry.rahti-int.csc.fi/oso-course/openshift-nginx`. Note that this
   is by no means something you have to do with every image when using
   OpenShift, but it's good to be aware that some images do require some
   changes.

6. Inspect the status of the **Pods** once more. They should now be running.

7. The default NGINX image uses port 80 by default, but the one for OpenShift
   uses 8080 instead, so you will need to change the port exposed by the pods in
   the **Deployment** and the **targetPort** setting in the **Service**. Note
   that **Services** are immutable, so you cannot use `oc edit` or
   `oc replace -f`. You need to use `oc replace --force -f` when you update the
   **Service**. This deletes and recreates the **Service**.

8. If all went well, you should now be able to access NGINX. Check the address
    from the **Route** you created in an earlier step:
    ```bash
    oc get route nginx-route
    ```
    If you access the URL listed under HOST/PORT in your browser, you should see
    the text "Welcome to nginx!"
