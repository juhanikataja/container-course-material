# Exercise 1 - Creating an app from source code

## Prerequisites

Exercise 0 completed.

## Learning objectives

* Learn how to use the source-to-image -mechanism to build an application
  directly from source code

## Description

In this exercise we learn how to build an application using only source code. Then 
we modify the source and update the application. We are letting OpenShift build
the application containers for us by using the source-to-image (s2i) mechanism.

The application is a small Python web server, based on [Bottle framework](https://bottlepy.org/).

## Relevant documentation

* [OpenShift Origin: Creating New Applications](https://docs.openshift.org/3.6/dev_guide/application_lifecycle/new_app.html)
* [OpenShift Origin: Source-to-image builds](https://docs.openshift.org/3.6/architecture/core_concepts/builds_and_image_streams.html#source-build)
* [OpenShift Origin: Routes](https://docs.openshift.org/3.6/architecture/networking/routes.html)

## Steps

1. Create a project

    For this exercise, we want to create a new project so we don't clash with the
    objects from other exercises:
    ```bash
    oc new-project <new project name>
   ```
    
2. Clone the application source code.  
    
    Clone a repository with a simple skeleton app by running
    
    ```bash
    git clone https://github.com/tourunen/simple-bottle.git
    cd simple-bottle
    ```
    
    The source tree should look like this:
    ```
    src
    ├── app.py              # main application, detected by s2i
    ├── requirements.txt    # python package requirements
    └── static              # static www content
        ├── css             # subdirectory for stylesheets
        │   └── styles.css  # a rather minimal stylesheet
        └── index.html      # the main page for the application
    ```
    
3. Create and deploy the application

    We first use `oc new-app` to create a bunch of objects for us.  
    ```bash
    oc new-app . --context-dir src --name ex1
    ```
    
    We can track the build process by specifying the **BuildConfig** name. The other
    alternative is to invoke `oc logs` on the build pod directly, but as the
    **BuildConfig** name does not change from build to build, this is more handy.
    ```bash
    oc logs -f bc/ex1
    ```

    Check all the objects that were created by the single command
    ```bash
    oc get all
    ```
    
    We can also list all the events that were triggered.
    ```bash
    oc get events
    ```

    We then expose it over HTTPS by creating a **Route**
     * the type is 'edge', meaning it is TLS terminated by OpenShift's router
     * we redirect all HTTP traffic to HTTPS port with `--insecure-policy` option.
    ```bash
    oc create route edge ex1-route --insecure-policy='Redirect' --service ex1
    oc get route ex1-route
    ```
    
    Finally, we can open our app in a browser and check the default page and
    /healthz -output.

4. Modify the source code and trigger a **Build**.
    Edit app.py or index.html to your liking, then start a build from our local sources:
    ```bash
    oc start-build ex1 --from-dir . --follow
    ```
    Check that the changes are visible in browser.

## Bonus exercises

When you have completed the other exercises, you can optionally continue with
these:

1. Fork the repository in GitHub, create the app from the fork

2. Trigger a build after you push changes to the code in GitHub

3. Setup web hooks to trigger a build

    See [Triggering Builds](https://docs.openshift.org/latest/dev_guide/builds/triggering_builds.html#github-webhooks)
    in OpenShift developer guide. You can also use the WebUI and navigate to `Builds -> ex1 ->
    Configuration` to get the WebHook URLs.
