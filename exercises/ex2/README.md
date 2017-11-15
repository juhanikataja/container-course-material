# Exercise 2 - Adding storage, content and scaling up

## Prerequisites

The NGINX instance from exercise 1 is succesfully deployed and accessible via a
browser.

## Learning objectives

* Creating secure **Routes** to a **Service**
* Using persistent storage via **PersistentVolumeClaim** with **Pods**
* Scaling up **Deployments**

## Description

Now that we have a basic NGINX web server installation, we should look into
serving some content. We will look at storing data for our website on a
**PersistentVolume** that we will attach to our **Pods**. Since we will have
some real content now, we should make sure the **Route** to our web site is
secure. We will also see how to scale up our site to prepare for more visitors.

The architecture of the resulting app will look like this:

![Exercise 2 architecture](ex2-arch.png)

## Relevant documentation

* [Kubernetes: Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
* [Kubernetes: Persistent volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
* [Kubernetes: Services](https://kubernetes.io/docs/concepts/services-networking/service/)
* [OpenShift: Routes](https://docs.openshift.org/3.6/architecture/networking/routes.html)
* [OpenShift: Copying Files to or from a Container](https://docs.openshift.org/latest/dev_guide/copy_files_to_container.html)

## Steps

1. Before we add our content, let's first make the **Route** to NGINX secured by
   TLS. You can replace your existing **Route** using the `nginx-tls-route.yaml`
   file found in the directory for this exercise. You can compare the new
   **Route** definition with the old one to see what's changed:
   ```bash
   diff -u ex1/nginx-route.yaml ex2/nginx-tls-route.yaml
   ```

2. If you access the URL from the **Route** again, you'll be redirected to a
   secure version of the NGINX site. Have a look at the certificate information
   in your browser to get a clue into how this is implemented.

3. Create a **PersistentVolumeClaim** using the `web-content-pvc.yaml` file:
   ```bash
   oc create -f web-content-pvc.yaml
   ```
   Notice the "ReadWriteMany" access mode. This makes it possible to attach the
   same volume to multiple **Pods** at the same time. If the access mode was
   "ReadWriteOnce" instead, the volume would only be attachable to one **Pod**
   at a time. The supported access modes depend on the underlying storage.
   In this example we are using GlusterFS which supports ReadWriteMany.

4. Watch the **PersistentVolumeClaim** listing to see when the new claim enters
   the **Bound** state and is ready for use (the handy -w flag can be used with
   other commands as well):
   ```bash
   oc get pvc -w
   ```

5. Update the NGINX **Deployment** to attach the newly created volume to the
   **Pods**. Let's use `oc patch` this time to see how that works:
   ```bash
   oc patch deployment nginx-deployment -p "$(cat nginx-deployment-persistent.yaml)"
   ```
   This will amend the existing **Deployment** so that storage is added to each
   **Pod**. It will only touch the parts of the API object that are relevant.

6. If you look at the NGINX URL now, you should see "403 Forbidden" since there
   is no index page and directory listings are not allowed. This is because we
   have just replaced the content of the default html directory with an empty
   volume.

7. Deploy your fancy new website using `oc rsync` on one of the **Pods** using
   content from the html subdirectory of the exercise directory:
   ```bash
   # Get a name for one of the Pods
   oc get pods
   # Copy over the html/ dir to the Pod
   oc rsync html/ <pod name>:/usr/share/nginx/html/
   ```
8. You should now have some content that was very expensive to create on your
   web page!

9. This new content is sure to bring hordes of interested visitors to the site,
   so let's scale up the **Deployment** to accommodate for the increased
   traffic:
   ```bash
   oc scale --replicas=4 deployment nginx
   ```
   After running this, there will be four **Pods** running instead of two.
