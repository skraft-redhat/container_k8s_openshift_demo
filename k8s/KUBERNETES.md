# Show the basic capabilities of Kubernetes

1. Connect to a Kubernetes cluster:
In our case we are using OpenShift, but all what can be done with Kubernetes is also supported with OpenShift

2. Set the right context aka *Namespace* or *Project*:  
- Create the environment: `kubectl create namespace [Project Name]`
- Check that we are in the right environment: `kubectl config set-context --current --namespace [Project Name]`

3. Create resources declaratively:
Kubernetes primarily works with so-called *Manifests*. These are config-files which specify the **desired** state. It's then the obligation of Kubernetes to continously compare the *desired* with the *real* status and initiate remediation. 

Let's just deploy the image first.

- Have a look at a simple yaml

- Deyploying the image: `kubectl apply -f deployment-1-replica.yaml`

- Let's see whether instances have already been started: 

`kubectl get deploy` and `kubectl get pods`

Now, let's show the beauty of *declarative configuration*:

First, by changing the declaration and let Kubernetes figure out how to reconcile.
`kubectl apply -f deployment-1-replica.yaml`

Second, by just changing the configuration, e.g. to have more replicas.
`kubectl apply -f deployment-8-replica.yaml`


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


Great, that has worked. But there are still some challenges that we have not adressed:

- How can we expose these pods to the outside world?

- How are actually the images built?

- And then, we actually need a a lot of other services to complement the Kubernetes cluster, e.g. image registry, monitoring, an overlay network, etc


With OpenShift all of these issues are solved in an *opinionated* way. Still, with OpenShift everything is open source. 


Source: [Kubernetes Tutorial @ Red Hat Developer Page](https://redhat-scholars.github.io/kubernetes-tutorial/kubernetes-tutorial/kubectl.html)

