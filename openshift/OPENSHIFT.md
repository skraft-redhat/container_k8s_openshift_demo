# OpenShift Capabilities

1. First let's introduce the GUI which makes the whole administration as well as the developer workflows much easier and more transparent

2. Comfortably, we can build on the deploments that we have made before with basic Kubernetes. Let's have a look at them.

3. But let's show how OpenShift can support the whole life-cycle, literally from the Source-code

In fact, the process from a source (git) to a deployed image involves several tasks:
- Building the source to executables (e.g. mvn package)
- Choosing a base image
- Providing a Dockerfile / Containerfile
- Providing multiple Kubernetes manifests

All these steps usually require tooling which needs to be maintained. With OpenShift, the whole environment is captured in a container image, the so-called *Builder image*. Comfortably, OpenShift is packaged with builder images for the most common programming languages and frameworks.

Let's explore this with Go.

In our git repository, we have a very simple Go-application in the folder */hello-world-golang*.

If we want to have this deployed as a container, we would need to follow the above steps. Namely, first having a go compiler, then writing a Dockerfile, then building the container image, then writing Kubernetes manifests and then applying them to the cluster.

In OpenShift, this is much, much easier.

Just, in the "Developer" perspective, click on "+Add" and choose the option "Import from Git". Then provide the Git Repository and the rest will be chosen and created automatically.

![Screenshot "Import from Git"](images/ImportFromGit.png)

As you can see, OpenShift is smart enough to automatically choose the right Builder Image. Which is in our case Go 1.18 based on the Universal Base Image 7 (= free distribution of Red Hat Enterprise Linux 7).

![Screenshot "Builder Image Detected"](images/BuilderImageDetected.png)

Now, if you start this (by presssing "Create"), several things will happen:

- The Builder Image will be instantiated and will take care of building the application and the container image
- All the Manifests will be created (deployment, service, route)
- Once the build is finished, it will be automatically deployed

Great!!!

But the support for building container images goes beyond the one-time creation. If we are updating the source code, OpenShift provides a fully automated solution to run through the whole process again.

The only thing, we need to do is to connect the GitHub repository with the OpenShift BuildConfig. This is accomplished, by adding a WebHook to Git which is triggered by each commit. The WebHook will then iniate the OpenShift Build.

1. Get the URL of the "Start Trigger"

- In the "Admin" perspective, go to "Builds -> BuildConfigs"

- Select the BuildConfig object that was automatically created for the Go application.

- Scroll down and copy the **Generic WebHook** URL with Secret


2. Create the Webhook

- In the GitHub repository, go to Settings -> Webhooks

- Fill out the form

    Payload URL: The copied URL from OpenShift

    Content Type: application/json

    SSL Verification: Disable

3. Now, you can change the source code and push it to the git repo.

4. You should see that a new build has been automatically created (e.g. look in the topology view or show the new build object). After the build has finished a new version is deployed.


One last thing that we want to show is one of the sweet spots of Kubernetes / OpenShift, which is smooth upgrading of applications. This actually allows to have multipe upgrades during business hours and generally reducing the risk.

The concepts for this are:
- Rolling Upgrades: allowing for zero-down time
- Canary deployments: only upgrading a small portion (e.g. 10%) to test a new version 
- Blue/Green Deployments: having multiple versions of an application running simultaneously. 

The easiest to show are rolling upgrades.

- Scale up the number of replicas to any number above 1.

- Explain the different parameter to configure the rolling upgrade (Max unavaible, Max surge)

- Start another upgrade as described above

- Watch how K8s performs the upgrade 