# Exercise 2

## Prerequisites

The NGINX instance from exercise 1 is succesfully deployed and accessible via a
browser.

## Learning objectives

* Creating secure **Routes** to a **Service**
* Using persistent storage with **Pods**

## Description

Now that we have a basic NGINX web server installation, we should look into
serving some content. We will look at storing data for our website on a
**PersistentVolume** that we will attach to our **Pods**. Since we will have
some real content now, we should make sure the **Route** to our web site is
secure.

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
3. Update the NGINX **Deployment** so that there is a **PersistentVolumeClaim**
   for storing website data.
