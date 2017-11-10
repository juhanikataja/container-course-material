 # Terminology

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

## Persistent volume

A **PersistentVolume** stores state. Claimed via **PersistentVolumeClaim**.

![PersistentVolumeClaim](img/persistentvolumeclaim.png "PersistentVolumeClaim")

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

## More information

**[https://kubernetes.io/docs/concepts/](https://kubernetes.io/docs/concepts/)**
