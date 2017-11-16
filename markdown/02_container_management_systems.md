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
- now developed by Google, Red Hat, CoreOS, ...
- open source, one of the biggest and most active projects in
  GitHub  
- Google Container Engine (GKE) implements K8s API
- some notable workloads
  - GitHub frontend
  - Pokémon Go
- [kubernetes.io](https://kubernetes.io/)

Note:

(TODO: add picture from CNCF showing K8s contribution activity)

---

## OpenShift

- from Red Hat
- "Enterprise Kubernetes for Developers" 
- open source ([github.com/openshift](https://github.com/openshift))
- batteries included
  - application templates
  - fairly complete Web UI
  - CD/CI
- [www.openshift.com](https://www.openshift.com)
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

## OpenShift versions

- OpenShift Origin
  - community supported
  - run on CentOS-7, Fedora, Atomic Host
- Red Hat OpenShift Container Platform    
  - Red Hat support, runs on supported OS stacks 
- OpenShift operated by Red Hat in a public cloud
  - OpenShift Online - shared
  - OpenShift Dedicated    

---

## OpenShift platform development model

- development is very open 
  - https://github.com/openshift/origin/issues
  - https://trello.com/atomicopenshift
- based on a stable K8s version, a few releases behind from bleeding edge
- ahead of K8s in some areas
  - security, multitenancy
- RH contributes to upsteam K8s releases, then
  later switches to using upstream features in OpenShift

---

## OpenShift releases

- Currently (Nov 2017) we are on 3.6.1
- up to 1.5 there was two versioning schemes 
  - 3.x for commercial
  - 1.x for Origin
- minor number shows K8s release (3.6 -> 1.6) 
- in the future OpenShift could be just K8s extensions 
  ([source: commons briefing #98](
  https://blog.openshift.com/openshift-commons-briefing-98-kubernetes-release-1-8-update/))

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
- masters are the only ones that talk to etcd (state)

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

---

## Security in OpenShift - runtime privileges

- Containers can request additional capabilities
- accounts can be given rights to use (for example)
  - host network stack
  - fixed UIDs
  - root UID
- these privileges are abstracted as Security Context Constraints (SCC) 
- in a shared environment, access to these is mostly reserved to system components 
