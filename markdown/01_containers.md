# Docker container orchestration with OpenShift

<!-- .slide: data-background="img_theme/topic_background.png" -->

---

## What are containers?

---

<!-- .slide: data-background="img/old-time-cargo.jpg" -->

---

<!-- .slide: data-background="img/old-time-cargo-2.jpg" -->

---

<!-- .slide: data-background="img/modern-cargo.jpg" -->

---

<!-- .slide: data-background="img/train-cargo.jpg" -->

Note:

* Image source: https://commons.wikimedia.org/wiki/File:CN_2416_GE_C40-8M.jpg
* Author: Nate Beal
* License: [CC BY 2.0](https://creativecommons.org/licenses/by/2.0/deed.en)

---

<!-- .slide: data-background="img/truck-cargo.jpg" -->

Note:

* Image source: https://commons.wikimedia.org/wiki/File:Intermodal_Transport_by_Truck.JPG
* Author: Joseph Madden (Joedamadman)
* License: [CC BY 3.0](https://creativecommons.org/licenses/by/3.0/deed.en)

---

## Containers

* Standardize software deployment
* Build once, run anywhere
* Everything needed to run an app in a single package

---

## Containers in Linux

* A container shares its kernel with its host and other containers, but can have its own
  * File systems
  * Networking stack
  * Resource quota
  * User IDs
  * Process tree
  * etc.
* Can mix and match different isolation features of the kernel

---

## What containers enable

* Programs with conflicting requirements can run on the same server
* Packaging into images
* Security hardening
* Run "Ubuntu" on CentOS or vice versa

---

<br/>
<br/>

## Containers ≠ light weight virtual machines

---

![VMs vs. containers](img/vm_vs_container.png "VMs vs. containers")

---
<br/>
<br/>

## Containers ≠ Docker

---

## What is Docker?

* Product from Docker Inc.
* Provides an **abstraction layer** for kernel container features
* Main features
   * Runtime for containers (runC)
   * Image format for containers
   * CLI for running and managing containers and images
   * Dockerfile files for building images

---

## Alternatives

* rkt
* Intel Clear Containers
* LXC
* Singularity (mainly for HPC)

---

## Docker registries

* A Docker **registry** is a server that stores Docker images
* The default one when using Docker's CLI is the **Docker Hub**
* There are many others:
  * [Red Hat Container Catalog](https://access.redhat.com/containers/)
  * [quay.io](https://quay.io/)
  * [Amazon Elastic Container Registry](https://aws.amazon.com/ecr/)
  * [Google Container Registry](https://cloud.google.com/container-registry/)
* You can host one yourself, too

---

## Image security considerations

* Image security is still more of a wild west compared to package security with
  package managers like yum and apt
* Docker Hub images are not automatically up to date and secure
* *Anyone* can upload images to Docker Hub

---

## Image source considerations

* There are pros and cons to all image sources
  * The Red Hat Container Catalog most likely has better images on average than
    Docker Hub, but of course the selection is more limited
  * Docker Hub has a very large selection of images - some good, some bad
* **Image security is not inherently better or worse than package security -
  it's all about the processes of your package/image provider**

---

## Picking a good image

**Picking a good base image is one of the most important things for good
container security.**
<!-- .element: class="fragment" data-fragment-index="0" -->

1. Is there a readymade image from a reputable source?
<!-- .element: class="fragment" data-fragment-index="1" -->
  * Docker Hub and RHCC both have security information these days
  <!-- .element: class="fragment" data-fragment-index="1" -->
  * Example: NGINX image from Red Hat vs. NGINX image from Docker Hub
  <!-- .element: class="fragment" data-fragment-index="1" -->

2. If no readymade image available, take a reputable image as a basis and build
   on top of that using a custom Dockerfile.
   <!-- .element: class="fragment" data-fragment-index="2" -->
  * Resist the temptation to pick a readymade image that some random person on
    the Internet uploaded to Docker Hub!
    <!-- .element: class="fragment" data-fragment-index="2" -->

---

## Docker commands

**Run** container:
```
docker run -it ubuntu
```
**List** running containers:
```
docker ps
```

---

## Docker commands

**Pull** container image from a **registry**:
```
docker pull ubuntu
```
**Tag** container image:
```
docker tag docker.io/alpine my-registry.example.com/alpine
```
**Push** container image to private registry:
```
docker push my-registry.example.com/alpine
```
**Build** container image from Dockerfile:
```
docker build . -t my-image:latest
```

---

## Demo

[https://labs.play-with-docker.com](https://labs.play-with-docker.com)

You can follow along on your own computer if you have Docker Hub credentials.

Note:
```
mkdir starwars
vim Dockerfile

FROM alpine

ENTRYPOINT [ "/usr/bin/telnet", "towel.blinkenlights.nl" ]

docker build . -t starwars:latest
docker images
docker run -it --detach --name starwars starwars
docker ps
clear
docker attach starwars
```
