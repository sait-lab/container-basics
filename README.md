# Container Basics

<img src="./README.assets/shipping.container.jpg" alt="shipping.container" style="zoom:50%;" />

[How Shipping Containers Revolutionized Global Trade](https://www.linkedin.com/pulse/how-shipping-containers-revolutionized-global-trade-david-conway-ycwyc)

## Installing Docker

There are plenty of ways and places to get Docker up and running. You can do it on Windows, Mac, or Linux. Whether you want it in the cloud, on-prem, or right on your laptop with VMware Workstation or VirtualBox. 

<details>
  <summary>Play with Docker</summary>
[Play with Docker (PWD)](https://labs.play-with-docker.com/) is a free 4-hour Docker playground which allows users to run Docker commands in a matter of seconds. It gives the experience of having a free Alpine Linux Virtual Machine in browser, where you can build and run Docker containers and even create clusters.
</details>

<details>
  <summary>Install Docker Engine on Ubuntu</summary>
  https://docs.docker.com/engine/install/ubuntu/
  https://docs.docker.com/engine/install/linux-postinstall/
</details>


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

