---
title: "Protect Your Docker Socket!"
date: 2018-03-02T10:48:14+01:00
tags: ["Docker", "DevOps", "TLS", "Security", "Scaleway" ]
draft: false
---

By default, when you install and start Docker on Linux, client access Docker-Engine on local host with unix socket (/var/run/docker.sock), which by default is only accessible by the root user. Exposed REST API with root access to Linux kernel (cgroups, kernel namespaces, etc.)!!!??? No authentication or authorization!!!???

So, how to access Docker-Engine over network (tcp) and how to secure this communication using TLS?

### Method nr 1 - The Hard Way
Follow the steps in [Protect the Docker daemon socket doc](https://docs.docker.com/engine/security/https/). Deployment and use of TLS/SSL is easy ;-)

### Method nr 2 - The Popular Way
Sick of googling every time you need a self signed certificate? Check [OMGWTFSSL Cert Generator](https://hub.docker.com/r/paulczar/omgwtfssl/)

### Method nr 3 - The Easiest Way (IMHO)
[Docker-Machine](https://docs.docker.com/machine/overview/)... It can be used to create Docker host on various platforms locally or in a cloud environment. You can control your Docker hosts with it as well. Docker-Machine also has the option to run everything over TLS. Let's take a look at how we deploy to a cloud environment of our choosing - Docker host in [Scaleway](https://www.scaleway.com):

```
docker-machine --native-ssh create \
  --driver generic \
  --generic-ip-address <docker_host_ip> \
  --generic-ssh-user=root \
  --generic-ssh-key=<full_path_to_your_id_rsa> \
  --engine-storage-driver overlay2 \
  <docker_host_name>

```
##### Enjoy the magic!
