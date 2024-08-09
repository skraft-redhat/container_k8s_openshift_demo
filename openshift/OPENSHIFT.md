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


