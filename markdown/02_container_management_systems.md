# Container management systems

---

## Docker Swarm

- from Docker Inc.
- takes care of scheduling containers on multiple hosts
- same API as Docker engine -> same tools
- [Swarm documentation](https://docs.docker.com/engine/swarm/)
- k8s support in beta

Note:

(TODO: rephrase and clarify Docker/Swarm/K8s relation/support)

---

## Kubernetes (K8s)

- from Cloud Native Computing Foundation (CNCF)
- originally developed by Google, donated 2015
- now developed by Google, RedHat, CoreOS, ...
- open source, one of the biggest and most active projects in
  GitHub  
- Google Container Engine (GKE) implements K8s API
- some notable workloads
  - GitHub frontend
  - Pokémon Go

- [https://kubernetes.io/](https://kubernetes.io/)

Note:

(TODO: add picture from CNCF showing K8s contribution activity)

---

## OpenShift

- from RedHat
- based on Kubernetes
- batteries included
    - application templates
    - fairly complete Web UI
    - CD/CI
- community supported and commercial versions available

---

# Standing on the shoulders of giants

---

<!-- .slide: data-background="img/openshift_logo.png" -->

---

<!-- .slide: data-background="img/kubernetes_logo.png" -->

---

<!-- .slide: data-background="img/picard_as_locutus.jpg" -->
<!-- (image source: Wikipedia, under fair use) -->

---

## OpenShift platform development model

- based on a stable K8s version, a few releases behind from bleeding edge
- ahead of K8s in some areas
  - security, multitenancy
- RH contributes to upsteam K8s releases, then
  later switches to using upstream features in OpenShift
- target: make K8s more extensible, so that
  OpenShift can run as plugins
  (source: OpenShift commons briefing for K8s 1.8)

Note:

(TODO: find specific link for briefing)

---

## Motivation: SEP

- why container orchestration system instead of XYZ

---

## Somebody Else's Problem

- network isolation - SEP
- container and project isolation - SEP
- process keepalive - SEP
- capacity management - SEP
- CI/CD orchestration - SEP
- service location - SEP
- HA load balancing - SEP
- storage - SEP
- DNS name for the application - SEP
- server certificates - SEP
- authorization for delegating management rights - SEP

---

## Services and other useful stuff (with demos)

- application templates
- Docker registry
  - accessible from outside
- WebUI for Docker registry
- container resource usage monitoring
  - CPU, memory, network
- terminal access to running containers 
  - no host login required 
- centralized logging

---

## Security in OpenShift - infrastructure

- infra nodes (masters, etcd, lb, glusterfs) do not run user processes 
- user nodes have limited access to infra
- masters are only ones that talk to etcd (state)

---

## Security in OpenShift - containers

- isolation: based on Linux kernel
- (ClearContainers: KVM)
- SELinux contexts per project
- User ID per project
  - taken from high range
  - no overlaps
- extra process capabilities are dropped
- no user namespaces yet, thus no (local) root in containers
- network: SDN per project
- storage: SELinux labeling (for supported storage types)

Note:

TODO: add SCC model

---

## Security in OpenShift - runtime privileges

- Containers can request additional capabilities
- accounts can be given rights to use (for example)
  - host network stack
  - fixed UIDs
  - root UID
- these privileges are abstracted as Security Context Constraints (SCC) 
- in a shared environment, access to these is mostly reserved to system components 
