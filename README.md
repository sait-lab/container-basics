# Container Basics

<img src="./README.assets/shipping.container.jpg" alt="shipping.container" style="zoom:50%;" />

[How Shipping Containers Revolutionized Global Trade](https://www.linkedin.com/pulse/how-shipping-containers-revolutionized-global-trade-david-conway-ycwyc)

## Installing Docker

There are plenty of ways and places to get Docker up and running. You can do it on Windows, Mac, or Linux. Whether you want it in the cloud, on-prem, or right on your laptop with VMware Workstation or VirtualBox. 

<details>
  <summary>Play with Docker (PWD)</summary>
https://labs.play-with-docker.com/ is a free 4-hour Docker playground which allows users to run Docker commands in a matter of seconds. It gives the experience of having a free Alpine Linux Virtual Machine in browser, where you can build and run Docker containers and even create clusters.
</details>

<details>
  <summary>Install Docker Engine on Ubuntu</summary>
  https://docs.docker.com/engine/install/ubuntu/ <br>
  https://docs.docker.com/engine/install/linux-postinstall/
</details>
Installing Docker Engine directly on Linux is often better for learners compared to using Docker Desktop for a few reasons:

1. **Simplicity**: Linux simplifies Docker installation and usage because Docker was originally designed for Linux. Learners can avoid the complexities and potential issues associated with running Docker through a virtualization layer.
2. **Command Line Proficiency**: Using Docker Engine on Linux encourages learners to become more comfortable with the command line, which is an essential skill for DevOps and development roles.
3. **Access to Native Linux Features**: Docker on Linux can leverage native kernel features directly, providing a more seamless and integrated experience.
4. **Community and Support**: The Docker community and many tutorials are heavily Linux-focused. Learners using Linux will find it easier to follow along with these resources and seek help when needed.

The demos in this document run on a Ubuntu 24.04 LTS VM.

```
ubuntu@docker-host:~$ lsb_release  -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 24.04 LTS
Release:	24.04
Codename:	noble
```

```
ubuntu@docker-host:~$ docker info
Client: Docker Engine - Community
 Version:    26.1.4
 Context:    default
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.14.1
    Path:     /usr/libexec/docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.27.1
    Path:     /usr/libexec/docker/cli-plugins/docker-compose
......
```

## Docker Concepts

<img src="./README.assets/docker-components.jpeg" alt="docker-components" style="zoom:33%;" />

Credit: https://www.slideshare.net/slideshow/docker-41045742/41045742

### What is a container?

Excerpt from https://docs.docker.com/guides/docker-concepts/the-basics/what-is-a-container/:

> Without getting too deep, a VM is an entire operating system with its own kernel, hardware drivers, programs, and applications. Spinning up a VM only to isolate a single application is a lot of overhead. 
> 
> A container is simply an isolated process with all of the files it needs to run. If you run multiple containers, they all share the same kernel, allowing you to run more applications on less infrastructure.

> [!NOTE]  
> **Using VMs and containers together**: Quite often, you will see containers and VMs used together. As an example, in a cloud environment, the provisioned machines are typically VMs. However, instead of provisioning one machine to run one application, a VM with a container runtime can run multiple containerized applications, increasing resource utilization and reducing costs.

Run a container:

```
docker run -d -p 8080:80 docker/welcome-to-docker
```
The output from this command is the full container ID. 
```
Unable to find image 'docker/welcome-to-docker:latest' locally
latest: Pulling from docker/welcome-to-docker
96526aa774ef: Pull complete
740091335c74: Pull complete
da9c2e764c5b: Pull complete
ade17ad21ef4: Pull complete
4e6f462c8a69: Pull complete
1324d9977cd2: Pull complete
1b9b96da2c74: Pull complete
5d329b1e101a: Pull complete
Digest: sha256:eedaff45e3c78538087bdd9dc7afafac7e110061bbdd836af4104b10f10ab693
Status: Downloaded newer image for docker/welcome-to-docker:latest
ea219fee063f826773e85ade36ccd3e4ac654aa4b5de2ed3b42374fe745f8886
```
Run `docker ps` to verify the container ID.
```
ubuntu@docker-host:~$ docker ps
CONTAINER ID   IMAGE                      COMMAND                  CREATED         STATUS         PORTS                  NAMES
ea219fee063f   docker/welcome-to-docker   "/docker-entrypoint.…"   8 seconds ago   Up 8 seconds   0.0.0.0:8080->80/tcp   elated_chatterjee
```
> [!NOTE]  
> `docker run` is an alias of `docker container run`. https://docs.docker.com/reference/cli/docker/container/run/

### What is an image?

Excerpt from https://docs.docker.com/guides/docker-concepts/the-basics/what-is-an-image/:

> A container image is a standardized package that includes all of the files, binaries, libraries, and configurations to run a container.

Excerpt from https://docs.vmware.com/en/VMware-vSphere/8.0/vsphere-vm-administration/GUID-E9EAF7AC-1C08-441A-AB80-0BAA1EAF9F0A.html:

> VM Templates are primary copies of virtual machines that you can use to deploy virtual machines that are customized and ready for use. Templates promote consistency throughout your vSphere environment. You can use the content library to store and manage templates of virtual machines and vApps.

A vSphere VM Template is a stopped VM. A container image is a stopped container. 

Search for images using the [`docker search`](https://docs.docker.com/reference/cli/docker/search/) command:

```
docker search nginx
```

```
NAME                               DESCRIPTION                                     STARS     OFFICIAL
nginx                              Official build of Nginx.                        19963     [OK]
unit                               Official build of NGINX Unit: Universal Web …   32        [OK]
nginx/nginx-ingress                NGINX and  NGINX Plus Ingress Controllers fo…   92
nginxinc/nginx-unprivileged        Unprivileged NGINX Dockerfiles                  152
nginx/nginx-prometheus-exporter    NGINX Prometheus Exporter for NGINX and NGIN…   42
nginxinc/nginx-s3-gateway          Authenticating and caching gateway based on …   6
nginx/unit                         This repository is retired, use the Docker o…   63
nginx/nginx-ingress-operator       NGINX Ingress Operator for NGINX and NGINX P…   2
nginxinc/amplify-agent             NGINX Amplify Agent docker repository           1
nginx/nginx-quic-qns               NGINX QUIC interop                              1
nginxinc/ingress-demo              Ingress Demo                                    4
nginxproxy/nginx-proxy             Automated nginx proxy for Docker containers …   139
nginxproxy/acme-companion          Automated ACME SSL certificate generation fo…   134
nginx/unit-preview                 Unit preview features                           0
bitnami/nginx                      Bitnami container image for NGINX               189
bitnami/nginx-ingress-controller   Bitnami container image for NGINX Ingress Co…   34
nginxproxy/docker-gen              Generate files from docker container meta-da…   17
bitnami/nginx-exporter             Bitnami container image for NGINX Exporter      5
nginxinc/mra-fakes3                                                                0
ubuntu/nginx                       Nginx, a high-performance reverse proxy & we…   114
nginxinc/ngx-rust-tool                                                             0
nginxinc/mra_python_base                                                           0
rancher/nginx-ingress-controller                                                   13
kasmweb/nginx                      An Nginx image based off nginx:alpine and in…   8
```

Pull an image using the [`docker pull`](https://docs.docker.com/reference/cli/docker/image/pull/) command.

```
docker pull docker/welcome-to-docker
```

You will see output like the following:

```
Using default tag: latest
latest: Pulling from docker/welcome-to-docker
96526aa774ef: Pull complete
740091335c74: Pull complete
da9c2e764c5b: Pull complete
ade17ad21ef4: Pull complete
4e6f462c8a69: Pull complete
1324d9977cd2: Pull complete
1b9b96da2c74: Pull complete
5d329b1e101a: Pull complete
Digest: sha256:eedaff45e3c78538087bdd9dc7afafac7e110061bbdd836af4104b10f10ab693
Status: Downloaded newer image for docker/welcome-to-docker:latest
docker.io/docker/welcome-to-docker:latest
```

> Each of line represents a different downloaded layer of the image. Remember that each layer is a set of filesystem changes and provides functionality of the image.

List your downloaded images using the [`docker image ls`](https://docs.docker.com/reference/cli/docker/image/ls/) or `docker images` command:

```
docker image ls
```

```
REPOSITORY                 TAG       IMAGE ID       CREATED        SIZE
docker/welcome-to-docker   latest    c1f619b6477e   7 months ago   18.6MB
```

List the image's layers using the [`docker image history`](https://docs.docker.com/reference/cli/docker/image/history/) command:

```
docker image history docker/welcome-to-docker
```

```
IMAGE          CREATED        CREATED BY                                      SIZE      COMMENT
c1f619b6477e   7 months ago   COPY /app/build /usr/share/nginx/html # buil…   1.6MB     buildkit.dockerfile.v0
<missing>      8 months ago   /bin/sh -c #(nop)  CMD ["nginx" "-g" "daemon…   0B
<missing>      8 months ago   /bin/sh -c #(nop)  STOPSIGNAL SIGQUIT           0B
<missing>      8 months ago   /bin/sh -c #(nop)  EXPOSE 80                    0B
<missing>      8 months ago   /bin/sh -c #(nop)  ENTRYPOINT ["/docker-entr…   0B
<missing>      8 months ago   /bin/sh -c #(nop) COPY file:9e3b2b63db9f8fc7…   4.62kB
<missing>      8 months ago   /bin/sh -c #(nop) COPY file:57846632accc8975…   3.02kB
<missing>      8 months ago   /bin/sh -c #(nop) COPY file:3b1b9915b7dd898a…   298B
<missing>      8 months ago   /bin/sh -c #(nop) COPY file:caec368f5a54f70a…   2.12kB
<missing>      8 months ago   /bin/sh -c #(nop) COPY file:01e75c6dd0ce317d…   1.62kB
<missing>      8 months ago   /bin/sh -c set -x     && addgroup -g 101 -S …   9.61MB
<missing>      8 months ago   /bin/sh -c #(nop)  ENV PKG_RELEASE=1            0B
<missing>      8 months ago   /bin/sh -c #(nop)  ENV NGINX_VERSION=1.25.3     0B
<missing>      8 months ago   /bin/sh -c #(nop)  LABEL maintainer=NGINX Do…   0B
<missing>      8 months ago   /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B
<missing>      8 months ago   /bin/sh -c #(nop) ADD file:756183bba9c7f4593…   7.34MB
```

This output shows you all of the layers, their sizes, and the command used to create the layer.

### What is a registry?

Excerpt from https://docs.docker.com/guides/docker-concepts/the-basics/what-is-a-registry/:
> An image registry is a centralized location for storing and sharing your container images. It can be either public or private. Docker Hub is a public registry that anyone can use and is the default registry.
> 
> While Docker Hub is a popular option, there are many other available container registries available today, including Amazon Elastic Container Registry(ECR), Azure Container Registry (ACR), and Google Container Registry (GCR). You can even run your private registry on your local system or inside your organization. For example, Harbor, JFrog Artifactory, GitLab Container registry etc.
> 
> While you're working with registries, you might hear the terms registry and repository as if they're interchangeable. Even though they're related, they're not quite the same thing.
> 
> A registry is a centralized location that stores and manages container images, whereas a repository is a collection of related container images within a registry. Think of it as a folder where you organize your images based on projects. Each repository contains one or more container images.

https://docs.docker.com/docker-hub/quickstart/

https://docs.docker.com/docker-hub/repos/





---

Todo List:

- [ ] Describe Docker components
- [ ] Docker lifecycle commands
- [ ] Docker networking
- [ ] Docker storage
