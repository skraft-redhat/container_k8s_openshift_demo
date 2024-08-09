# Show the basic capabilities of Kubernetes

1. Connect to a Kubernetes cluster:
In our case we are using OpenShift, but all what can be done with Kubernetes is also supported with OpenShift

2. Set the right context aka *Namespace* or *Project*:  
- Create the environment: `kubectl create namespace mystuff`
- Check that we are in the right environment: `kubectl config current-context`

3. Create resources declaratively:
Kubernetes primarily works with so-called *Manifests*. These are config-files which specify the **desired** state. It's then the obligation of Kubernetes to continously compare the *desired* with the *real* status and initiate remediation. 

Let's just deploy the image first.

- Deyploying the image: `kubectl apply -f deployment-1-replica.yaml`

- Let's see whether instances have already been started: 

`kubectl get deploy` and `kubectl get pods`

Now, let's show the beauty of *declarative configuration*:

First, by changing the declaration and let Kubernetes figure out how to reconcile.
`kubectl apply -f deployment-1-replica.yaml`

Second, by killing any of the instances ("pods") and see what happens.
- `kubectl get pods`

- Copy one of the pods id

- `kubectl delete pod [pod id]`

- Now, show again the state of the deployment and all instances. Obviously, the remediation has happened automatically and very fast. The only thing how you can notice is that one of the pods has a lower age.


Let's explore some advanced features from K8s:
- rule-based placement:
We have a Kubernetes cluster with 3 worker nodes. How does Kubernetes decide where to put the instances? Well, that's based on several rules.
`kubectl get pods --output=wide`

- Anti-affinity rules:
`kubectl apply -f deployment-with-anti-affinity.yaml`

- Readiness checks

- Requests and Limits





Great, that has worked. But how can this be accessed?

- We need to additionally create a *Service* object which acts as a Load Balancer. (In OpenShift, this will be even more easy - which we'll show later!):  
`kubectl expose deployment myapp --port=8080 --type=LoadBalancer`

This will give us a (cluster)-internal IP:   
`kubectl get services`

But as you can see, we don't get an external-IP!

With OpenShift that gets a lot easier, as we can also easily expose it externally and we will get to know the beautiful GUI.

Source: [Kubernetes Tutorial @ Red Hat Developer Page](https://redhat-scholars.github.io/kubernetes-tutorial/kubernetes-tutorial/kubectl.html)

