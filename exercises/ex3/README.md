# Exercise 3 - Wrapping API objects into a Template

## Prerequisites

Exercises 0-2 completed.

## Learning objectives

* Encapsulating individual API objects into an application **Template**

## Description

> **Disclaimer:** The method we use to add content to our site in this exercise is
> not something you should do for a real site. Here we simply do it to
> demonstrate the usage of volumes and the `oc rsync` command in a way that is
> hopefully easy to understand if you have some familiarity with web servers.
> For a real site, you should put everything needed to run the site in the
> container images as part of the build process. Runtime state should be stored
> on persistent storage. See
> [The Twelve-Factor App: V. Build, release, run](https://12factor.net/build-release-run).

So far we've been creating API objects individually. These API objects have
constituted an app when put together. Instead of creating API objects one by
one, there is another way. You can pack things needed to run an app into an
application **Template**. In this exercise we will learn how to create your own
templates.

We will deploy the same NGINX application as in the previous two exercises, but
this time we will create it from a template that contains all the API objects.
We will also look into making the template configurable so we can launch copies
of the application with different settings.

The architecture is the same as it is in exercise 2.

## Relevant documentation

* [OpenShift Origin: Creating New Applications](https://docs.openshift.org/3.6/dev_guide/application_lifecycle/new_app.html)

## Steps

1. For this exercise, we want to create a new project so we don't clash with the
   objects with the same names from the previous exercises:
   ```bash
   oc new-project <new project name>
   ```

2. Have a look at the `nginx-template.yaml` file in the exercise directory. It
   contains the API objects from the previous exercises all in one file. The
   format of the file is like this:
   ```yaml
   apiVersion: v1
   kind: Template
   metadata:
     name: template-name
     annotations: # optional
       description: "Some description" # optional
       tags: "tag1,tag2" # optional
       iconClass: "icon-something" # optional
   objects:
     <api objects go here>
   parameters:
     <parameters go here>
   ```
   Notice that instead of a **Deployment** API object, we are creating a
   **DeploymentConfig** API object instead. This is to make the template work
   with the `oc new-app` command. It looks like there is a bug in the command
   that it doesn't recognize the **Deployment** type yet. **DeploymentConfig**
   is a native OpenShift type while **Deployment** is a Kubernetes type that is
   based on OpenShift's **DeploymentConfig** type. The APIs of the two types are
   very similar.

3. Create a new **Template** in your project using the `nginx-template.yaml`
   file:
   ```bash
   oc create -f nginx-template.yaml
   ```

4. Have a look at what information you can see on **Templates** when listing
   them:
   ```bash
   oc get templates
   ```
   Notice that the **Template** takes four parameters, one of which (the image)
   does not have a default value and must be specified each time.

5. Launch the application from the **Template**:
   ```bash
   oc new-app nginx-site -p NGINX_IMAGE=<openshift compatible nginx image>
   ```
   You can use the same image as in the previous exercises.

6. If you look at the status of your **Pods** now, you'll see none of them are
   ready. This is because the site is missing content as was the case in the
   previous exercise. If you access the URL of the site, you will get an
   "Application is not available" error from OpenShift.

7. As in the previous exercise, copy some content into your site:
   ```bash
   # Get one of the pods' name
   oc get pods
   # Copy over content
   oc rsync ../ex2/html/ <pod name>:/usr/share/nginx/html/
   ```

8. Get the URL of the site from the **Route**:
   ```bash
   oc get routes
   ```

9. If you access the site now, you should see the same content as in exercise 2.

10. Other things to try:
    * See how the items created look like in the web UI
    * Recreate the app from the web interface:
       * Delete the current project
       * Create a new project
       * In the application catalog, select "Import YAML / JSON"
       * Click "Browse" and select the **Template** file
       * Click "Create", select "Save template"
       * Click "Add to Project"
       * Find your **Template** under "Uncategorized"
       * Select the "nginx-site" **Template**
       * Fill in an image and click "Create"
