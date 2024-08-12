# Instructions for the demo preparation

1. Clone the git repo

   `git clone https://github.com/skraft-redhat/container_k8s_openshift_demo`

2. Build the container image


- Go to the source directory:  
`cd container_k8s_openshift_demo/source`

- Package the source code:  
`./mvnw package -Dnative`

- Build the container image:  
`podman build -f src/main/docker/Dockerfile.native -t quay.io/stephan_kraft/hello-world-demo .`

- (optional) Run the image:  
`podman run -i --rm -p 8080:8080 quay.io/stephan_kraft/hello-world-demo`

In a browser (or with curl) try to access http://localhost:8080/hello

3. Push the container image:  
`podman push quay.io/stephan_kraft/hello-world-demo`

4. Make the container image public:

In the Quay Web Console, go to the Repository -> Settings -> Repository Visibility.
Press "Make Public".

(optional) create another container image with another tag => will be used later for rolling upgrade demo.

5. Connect to the OpenShift Cluster:  
`oc login -u [USER] -p [PASSWORD] https://api.[Cluster Domain]`

6. Open VS Codium 

7. Open Quay.io

8. (optional) cleanup local container images:
- `podman rmi --all --force`

- `podman images`
