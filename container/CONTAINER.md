# Showcase the basic workflows of pulling and running containers

> **_NOTE:_** A very nice tool to manage multiple terminals on one screen is *tmux*. This can be started on a bash terminal, the most basic commands are:
%b + %      => adds a vertical terminal pane
%b + "      => adds a horizonatl terminal pane
%b + Arrows => jump from one pane to another


1. Search in container image registries, e.g.:
`podman search hello-world-demo`

(Remark: Podman searches in all registries listed in `/etc/containers/registries.conf`)

2. Show container image registry:
https://quay.io/repository/stephan_kraft/hello-world-demo

3. Pull the container image:  
`podman pull quay.io/stephan_kraft/hello-world-demo`

3a. (optional) Show the images and their size:
`podman images ls`

4. Run the image:  
`podman run -i --rm -p 8080:8080 quay.io/stephan_kraft/hello-world-demo`

5. Access the endpoint: 
In a browser (or with curl) try to access http://localhost:8080

6. Container basics:
As discussed, container are also virtualization, but on a different level than Virtual Machines (VMs). They are running as processes on a (Linux) Kernel and make use of basic Kerne functionality, like:
- CGroups: in order to limit resource consumption
- Namespaces: in order to limit what a Container can see (e.g. Network, Process,...)
- chroot: in order to limit the view on the file system

You can just exec into a container, e.g. with `podman exec -it [Container Name] [Command, eg. bash]`

Show the differences outside and inside the container with commands like:
- `cat /etc/os-release`
- Show that most of the sys-admin utilities don't work in the container, e.g. `ps`, `netstat`, etc. (Security!)

7. What happens if the container is killed?

- Find out the Process ID: The pid is not so easy to find as podman starts container - due to security reasons - with a different user id. What you can do:
`podman container inspect -f "{{.PidFile}}" [Container ID]` to find the file where podman writes the pid to.

- Kill the process `kill -9 [pid]`

Alternatively, you can just delete the contaimer with `podman rm -f [container id]`

(Remark: It might be that the process still polutes the port. In that case, you can find out with `netstat -anop | grep 8080`)

8. Scaling the container:
If you run the "podman run" command again, it fails. Why? Because you can't connect two processes to the same port.

Discussion:
- If container dies, there is no self-healing
- How to scale? (Starting another container would have a conflicting port)
- How to manage multi-node environments

**=> Kubernetes to the rescue**