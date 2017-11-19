# Kubernetes architecture

<!-- .slide: data-background="img_theme/topic_background.png" -->

---

## Basics

* Fundamental model: Master-Slave
* State in [etcd](https://github.com/coreos/etcd)
  * Distributed reliable key-value store
* Access to state through Master API only
* No SPOFs (exception: minimal deployments)

---

## Controllers make it tick

> "A controller is a reconciliation loop that drives actual cluster state toward the desired cluster state"

Source: Wikipedia

---

## Controller behaviour 

* An **Actor** (User, Controller, ...) **changes object state**
  * e.g API call to Master to change the number of replicas
* Related **Controller is triggered** 
  * it **compares** the **target state** to the **actual state** in the cluster
  * performs **actions** that drive the state **towards the target state**
* Actions may trigger additional controllers (e.g. DC -> RC -> Pod)

---

<br/>

## How does our OpenShift deployment look like

---

![Openshift deployment](img/openshift_deployment.png)

Note:
* SDN: OpenVSwitch, can be Flannel, Calico, ...
* Show description for the cluster we are running on
* Show our deployment pipeline
