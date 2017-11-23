# OpenShift exercises

## Introduction

This directory contains exercises that you can do to learn how to use OpenShift
via the command line interface.

## Preparations

There are a number of files here that you will need access to while working on
the exercises. It is easiest to clone this whole repository so that you get all
the files on your computer before starting to work on the exercises. You can
find the link to clone this repository on the main page of the repository. Run
this on the command line to clone the repo:

```bash
git clone --depth=1 https://github.com/Digipalvelutehdas/container-course-material.git
rm -rf container-course-material/.git
```

Note that we are making a shallow clone (`--depth=1`) and removing the .git
directory. If the .git directory is not removed, this may interfere with some
exercises due to the way `oc` works: it may confuse the course material
repository as something to use for builds.

## Instructions

The exercises are meant to be completed in order, though in some cases you may
skip exercises. Each exercise lists prerequisites that you can look at to see if
you've completed the necessary steps to start the exercise.

Both OpenShift and Kubernetes have good documentation that you can refer to
while working on the exercises:

  * [Kubernetes documentation](https://kubernetes.io/docs/home/)
  * [OpenShift documentation](https://docs.openshift.org/latest/welcome/index.html)

Notice that you should refer to the documentation for the correct version of
Kubernetes/OpenShift. Once you've completed exercise 0, you can get version
information with the `oc version` command.
