# Showcase the basic workflows of pulling and running containers

1. Show container image registry:
https://quay.io/repository/stephan_kraft/hello-world-demo

2. Pull the container image:  
`podman pull quay.io/stephan_kraft/hello-world-demo`

3. Run the image:  
`podman run -i --rm -p 8080:8080 quay.io/stephan_kraft/hello-world-demo`

4. Access the endpoint: 
In a browser (or with curl) try to access http://localhost:8080/hello

Discussion:
- If container dies, there is no self-healing
- How to scale? (Starting another container would have a conflicting port)
- How to manage multi-node environments

**=> Kubernetes to the rescue**