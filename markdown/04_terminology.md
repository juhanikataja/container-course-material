# Terminology

---

## K8s architecture
* master-slave model
* state in etcd (usually clustered)

---

## K8s principles

"A controller is a reconciliation loop that drives actual cluster state toward the desired cluster state"

Source: wikipedia

---

## K8s principles

* controllers
* target state
* current state
* actions
---

## How does our OpenShift deployment look like

---

![Openshift deployment](img/openshift_deployment.png)

---

Note:

TODO: Rahti-int description, our deployment pipeline

---

### API object

* Described in YAML or JSON
* Represents an abstraction of a concept
* Examples:
  * Pod
  * Service
  * Deployment
  * ReplicaSet

---

### Label

* A key value pair like `app: nginx`
* Many things can have a label attached to them
* Can be used for selecting a set of API objects

---

### Pod

A collection of one or more containers and volumes with a common IP.

![Pods](img/pods.png "Pods")

---
### Pod

* smallest unit to schedule
* has resource limits
  * memory
  * CPU
* subject to readiness and health checks (per container)

---

## Storage (persistent volumes, claims, model)

A **PersistentVolume** stores state. Claimed via **PersistentVolumeClaim**.

![PersistentVolumeClaim](img/persistentvolumeclaim.png "PersistentVolumeClaim")

---

## Persistent volume
* Global object (lives outside namespaces)
* Can be created by administrators
* Can be created dynamically by Dynamic Volume Provisioning
* Obtained by user by creating a PersistentVolumeClaim  

---

## Persistent volume claim
* Created in project namespace
* properties
  * size
  * access mode (ReadOnly, ReadWriteOnce, ReadWriteMany)
  * storage class
* StorageClass -objects provide a way to select PV properties
  * backed by SSD, backed by spinning rust, mirrored, erasure coded,...

---

### Persistence model

* Pods can use scratch space local to the node it is running on
* **Any data that should persist has to be written to a volume**
* System is free to delete and respawn Pods
* Volume follows Pod when it is rescheduled to a different host

---

### Service

Provides a stable virtual IP and port for a set of pods.

![Service](img/service.png "Service")

---

### ReplicaSet

Ensure *n* copies of a pod are running.

![ReplicaSet](img/rc.png "ReplicaSet")

---

### Deployment

Manages rolling updates.

![Deployment](img/deployment.png "Deployment")

---

### Route/Ingress

* Provides a way to access Services externally
* Implemented by HAProxy pods ("Routers") in OpenShift
* Maps traffic for a given DNS name to a set of Pods backing a Service
* Optionally terminates SSL

---

### Namespace/Project

* User objects live in their namespaces/projects
* Services are accessible to Pods in the same namespace
* Users can have different roles/rights (globally or locally to the namespace)
  * admin, self-provisioner, basic-user, cluster-reader   
* admin of a namespace (=You!) can add others as collaborators

---

## More information

**[https://kubernetes.io/docs/concepts/](https://kubernetes.io/docs/concepts/)**
