# Hands-on in the OpenShift web UI

<!-- .slide: data-background="img_theme/topic_background.png" -->

---

## 1. Rocket.Chat

1. Create a new project and give it a unique name
2. Find the Rocket.Chat template in the app catalog (it is under the JS group)
3. Select the Rocket.Chat template and click Create
4. While the app is launching, you can explore the project view and see what has been created there
5. Click "Monitoring -> Events - View details" on the upper right corner to see how the deployment is progressing
6. Find the link to the application on the overview page once the app is running

---

## 2. Source to image (S2I)

1. Create a new project and give it a unique name
2. Select JS -> Node.js + MongoDB (Persistent)
3. Set "Git Repository URL" to `https://github.com/tourunen/gallery.git`
4. Click Create

---

## 3. Autoscaling (step 1)

In this exercise we test [Horizontal Pod Autoscaling](https://docs.openshift.org/latest/dev_guide/pod_autoscaling.html)

1. Create a new project and give it a unique name
2. Select Python -> Python 3.5 

---

## 3. Autoscaling (step 2)

3. Set 
  * Name to "primetime"
  * Git Repository URL to "https://github.com/tourunen/primetime.git"
  * Click on "show advanced options"
  * Set Context Dir to "src"
  * Tick Secure route
4. Click Create

---

## 3. Autoscaling (step 3)

5. On project overview, see that the app runs
6. Click "Applications -> Deployments -> primetime"
7. On the top right corner, click "Actions -> Add autoscaler"
8. Set Max Pods to "4"
9. Click Save

---

## 3. Autoscaling (testing)

Open the application in a browser, generate some primes. At the same time,
watch the CPU consumption and Pod count on the front page. 

You can lower the Autoscaler CPU % target to make it more sensible to load in
"Applications -> Deployments -> primetime -> Actions -> Edit Autoscaler".

---

## 3. Autoscaling (end note) 

The router keeps client sessions on the same Pod. To achieve load balancing for a 
single end web client, one could refactor the application so that the CPU heavy 
operations are done in a separate service, called by the web tier.
