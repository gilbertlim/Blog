---
title: "[Docker] VM, Docker, Kubernetes 구조"
category: Docker
---

<br><br><br>

Virtual Machine, Docker, Kubernetes의 구조에 대해서 알아본다.

# VM (Virtual Machine)

가상머신: OS 단위 격리

<img src="/assets/images/posts/docker-kubernetes/vm.jpg" alt="" width="75%" />

# Docker

도커: Process 단위 격리

<img src="/assets/images/posts/docker-kubernetes/docker.jpg" alt="" width="75%" />

# Docker (Mac OS)

Docker Engine은 Linux Kernel을 사용한다.

따라서 Mac OS에서는 Linux VM 위에 Docker Engine이 올라간다.

<img src="/assets/images/posts/docker-kubernetes/docker-macos.jpg" alt="" width="75%" />

# Kubernetes

Host OS(Node)가 여러 개 일 때 한 번에 관리하기 위한 Orchestration Tool

동일 Cluster의 Node들은 같은 Network 상에 놓여져 있다.

<img src="/assets/images/posts/docker-kubernetes/kubernetes.jpg" alt="" width="75%" />
