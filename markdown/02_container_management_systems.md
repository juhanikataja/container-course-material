# Container management systems

<!-- .slide: data-background="img_theme/topic_background.png" -->

---

<br>
<br>

## Containers ≠ happiness

If run manually, you are tempted to start treating containers
as pets. To herd the cattle, we need a management system.

---

## Docker Swarm

* From Docker Inc.
* Takes care of 
  * scheduling containers on multiple hosts
  * networking, storage (to an extent)
* Bundled to Docker 1.12 and up 
* Same API as Docker engine -> same tools
* [docs.docker.com/engine/swarm/](https://docs.docker.com/engine/swarm/)
* Docker Platform also [supports Kubernetes](https://www.docker.com/kubernetes)

---

## Kubernetes (K8s)

* From Cloud Native Computing Foundation (CNCF)
* Originally developed by Google, donated 2015
* Now developed by Google, Red Hat, CoreOS, ...
* Open source (Apache License 2.0)
  * [github.com/kubernetes/](https://github.com/kubernetes/)
  * one of the biggest and most active projects in GitHub
  * see [CNCF Blog](https://www.cncf.io/blog/2017/06/05/30-highest-velocity-open-source-projects/)

![k8s logo](img/kubernetes_logo_helm.png)

---

## Kubernetes

* Google Container Engine (GKE) implements K8s API
* Some notable workloads
  * [GitHub frontend](https://githubengineering.com/kubernetes-at-github/)
  * [Pokémon Go](https://cloudplatform.googleblog.com/2016/09/bringing-Pokemon-GO-to-life-on-Google-Cloud.html)
  * [Philips lightbulbs](https://www.bloomberg.com/features/2017-kubernetes/)
* [kubernetes.io](https://kubernetes.io/)

![k8s logo](img/kubernetes_logo_helm.png)

---

## Kubernetes

* Currently (Nov 2017) at version 1.8
* Next releases will bring better support for batch workloads
* Several commercial distributions available from
  * CoreOS, Cisco, Canonical, Red Hat... 
  * See [K8s certified distributions](https://kubernetes.io/partners/#dist) 

![k8s logo](img/kubernetes_logo_helm.png)

Note:
* upcoming
  * generic device plugin mechanism
  * CPU pinning/architecture awareness
  * hugepages (maybe in 1.8 already?)

---

## OpenShift

> "Enterprise Kubernetes for Developers"
* From Red Hat
* Open source ([github.com/openshift](https://github.com/openshift))
* Batteries included
  * application templates
  * fairly complete Web UI
  * CI/CD pipelines
* [www.openshift.com](https://www.openshift.com)

![openshift logo](img/openshift_logo_small.png)

---


<br>
<br>

## Standing on the shoulders of giants

---

<!-- .slide: data-background="img/openshift_logo.png" -->

---

<!-- .slide: data-background="img/kubernetes_logo.png" -->

---

<!-- .slide: data-background="img/picard_as_locutus.jpg" -->
<!-- (image source: Wikipedia, under fair use) -->

---

## OpenShift versions

* OpenShift Origin
  * community supported
  * run on CentOS-7, Fedora, Atomic Host
* Red Hat OpenShift Container Platform    
  * Red Hat support, runs on supported OS stacks
* OpenShift operated by Red Hat in a public cloud
  * OpenShift Online - shared
  * OpenShift Dedicated    

![openshift logo](img/openshift_logo_small.png)

---

## OpenShift platform development model

* Development is very open
  * https://github.com/openshift/origin/issues
  * https://trello.com/atomicopenshift
* Based on a stable K8s version, a few releases behind from bleeding edge
* Ahead of K8s in some areas
  * security, multitenancy
* RH contributes actively to upsteam K8s releases, then
  later switches to using upstream features in OpenShift

![openshift logo](img/openshift_logo_small.png)

---

## OpenShift releases

* Currently (Nov 2017) we are on 3.6.1
* Up to 1.5 there was two versioning schemes
  * 3.x for commercial
  * 1.x for Origin
* Minor number shows K8s release (3.6 -> 1.6)
* In the future OpenShift could be just K8s extensions
  ([source: commons briefing #98](
  https://blog.openshift.com/openshift-commons-briefing-98-kubernetes-release-1-8-update/))

![openshift logo](img/openshift_logo_small.png)

---

## Motivation: SEP

* Why container orchestration system instead of XYZ

---

## Somebody Else's Problem

* Network isolation - SEP
* Container and project isolation - SEP
* Process keepalive - SEP
* Capacity management - SEP
* CI/CD orchestration - SEP
* Service location - SEP
* HA load balancing - SEP
* Storage - SEP
* DNS name for the application - SEP
* Server certificates - SEP
* Authorization for delegating management rights - SEP

---

## Services and other useful stuff (with demos)

* Application templates
* Docker registry
  * accessible from outside
* WebUI for Docker registry
* Container resource usage monitoring
  * CPU, memory, network
* Terminal access to running containers
  * no host login required
* Centralized logging

---

## Security in OpenShift - infrastructure

* Infra nodes (masters, etcd, lb, glusterfs) do not run user processes
* User nodes have limited access to infra
* Masters are the only ones that talk to etcd (state)

---

## Security in OpenShift - isolation

* Isolation: based on Linux kernel
* (ClearContainers: KVM)
* SELinux contexts per project
* User ID per project
  * taken from high range
  * no overlaps
* Extra process capabilities are dropped
* No user namespaces yet, thus no (local) root in containers
* Network: SDN per project
* Storage: SELinux labeling (for supported storage types)

---

## Security in OpenShift - runtime privileges

* Containers can request additional capabilities
* Accounts can be given rights to use (for example)
  * host network stack
  * fixed UIDs
  * root UID
* These privileges are abstracted as Security Context Constraints (SCC)
* In a shared environment, access to these is mostly reserved to system components

---

## Security in OpenShift - other aspects

* Container images
  * Bundled templates use images from Red Hat (RHEL/CentOS) 
  * Users may run on whatever images they want
* Developers do not need shell access to the hosts

Note:

Do consider twice before using non-official images. 

---

## Comparison to other platforms

|                           | Virtual hosting | IaaS Cloud  | Container cloud   |
| ------------------------- | --------------- | ----------- | ----------------- |         
| **Model**                 | pet VMs         | cattle VMs  | cattle containers |
| **Unit**                  | VM              | VM          | container         |
| **Avail for single unit** | high            | medium      | low               |
| **Automatic recovery**    | yes             | DIY         | yes               |
| **Level of abstraction**  | low             | low         | high              |
| **Ease of scaling**       | low             | medium      | trivial           |
| **Non-Linux workloads**   | yes             | yes         | hard              |
