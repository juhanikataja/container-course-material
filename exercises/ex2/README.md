# Exercise 2 - Deploying NGINX

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

The architecture of the resulting app will look like this:

![Exercise 2 architecture](ex2-arch.png)

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
   platform. You can get more information about why a **Pod** is not working
   correctly with the `oc logs` command:
   ```bash
   oc logs <name of a pod>
   ```

5. You will need to replace the NGINX image with one that is compatible with
   OpenShift. You can use either `oc edit deployment <deployment name>` or `oc
   replace -f <modified deployment yaml>` to achieve this. Modify the
   **Deployment** you created in an earlier step to change the image. You can list
   your **Deployments** with `oc get deployments`. You can use this image (all you
   need to do is to enter this string in the *image* field of the **Deployment**):
   `docker.io/rlaurika/openshift-nginx`. If you have
   access to Docker, you can get a Dockerfile for this image by the NGINX folks
   here: [nginxinc/openshift-nginx](https://github.com/nginxinc/openshift-nginx).
   Note that this is by no means something you have to do with every image when
   using OpenShift, but it's good to be aware that some images do require some
   changes.

6. Inspect the status of the **Pods** once more. They should end up in the
   "Running" state after a small wait.

7. The default NGINX image uses port 80 by default, but the one for OpenShift
   uses 8080 instead, so you will need to change the port exposed by the **Pods**
   in the **Deployment** (adjust the *containerPort* setting). The *targetPort*
   setting in the **Service** references the port in the **Pods** by name, so it
   does not need to be updated. If you ever do need to modify a **Service**, note
   that they are immutable, so you cannot use `oc edit` or `oc replace -f`. You
   need to use `oc replace --force -f`. This deletes and recreates the **Service**.

8. If all went well, you should now be able to access NGINX. Check the address
    from the **Route** you created in an earlier step:
    ```bash
    oc get route nginx-route
    ```
    If you access the URL listed under HOST/PORT in your browser, you should see
    the text "Welcome to nginx!"
