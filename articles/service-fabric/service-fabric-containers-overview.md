---
title: Overview of Service Fabric and containers | Microsoft Docs
description: An overview of Service Fabric and the use of containers to deploy microservice applications. This article provides an overview of how containers can be used and the available capabilities in Service Fabric.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''

ms.assetid: c98b3fcb-c992-4dd9-b67d-2598a9bf8aab
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/24/2016
ms.author: mfussell

---
# Preview: Service Fabric and containers
> [!NOTE]
> This feature is in preview for Linux and is not currently available on Windows Server 2016. This will be in preview for Windows Server on the next release of Azure Service Fabric after Windows Server 2016 GA and supported in the subsequent release after that.
> 
> 

## Introduction
Azure Service Fabric is an [orchestrator](service-fabric-cluster-resource-manager-introduction.md) of services across a cluster of machines. Services can be developed in many ways, from using the [Service Fabric programming models](service-fabric-choose-framework.md) to deploying [guest executables](service-fabric-deploy-existing-app.md). By default, Service Fabric deploys and activates these services as processes. Processes provide the fastest activation and highest density usage of the resources in a cluster. Service Fabric can also deploy services in container images. Importantly, you can mix services in processes and services in containers in the same application. You get the best of both worlds, depending on your scenario.

## What are containers?
Containers are encapsulated, individually deployable components that run as isolated instances on the same kernel to take advantage of virtualization that an operating system provides. This means that each application and its runtime, dependencies, and system libraries run inside a container with full, private access to the container's own isolated view of operating system constructs. Along with portability, this degree of security and resource isolation is the main benefit for using containers with Service Fabric, which otherwise runs services in processes.

Containers are a virtualization technology that virtualizes the underlying operating system from applications. Containers provide an immutable environment for applications to run with varying degrees of isolation. Containers run directly on top of the kernel and have an isolated view of the file system and other resources. Compared to virtual machines, containers have the following advantages:

* **Small in size**: Containers use a single storage space and smaller deltas for each layer to increase efficiency.
* **Fast boot-up time**: Containers don’t have to boot an operating system, so they can be up and available much faster, typically in seconds, than virtual machines.
* **Portability**: A containerized application image can be ported to run in the cloud, on premises, inside virtual machines, or directly on physical machines.
* **Resource governance**: A container can limit the physical resources that it can consume on its host.

## Container types
Service Fabric supports the following types of containers.

### Docker containers on Linux
Docker provides high-level APIs to create and manage containers on top of Linux kernel containers. Docker Hub is a central repository to store and retrieve container images.

For a walkthrough about how to do this, read [Deploy a Docker container to Service Fabric](service-fabric-deploy-container-linux.md).

### Windows Server containers
Windows Server 2016 provides two different types of containers that differ in the level of provided isolation. Windows Server containers and Docker containers are similar because both have namespace and file system isolation but share the kernel with the host they are running on. On Linux, this isolation has traditionally been provided by cgroups and namespaces, and Windows Server containers behave similarly.

Windows Hyper-V containers provide more isolation and security because each container does not share the operating system kernel with other containers or with the host. With this higher level of security isolation, Hyper-V containers are particularly targeted at hostile, multitenant scenarios.

For a walkthrough about how to do this, read [Deploy a Windows container to Service Fabric](service-fabric-deploy-container.md).

The following figure shows the different types of virtualization and isolation levels available in the operating system.
![Service Fabric platform][Image1]

## Scenarios for using containers
Here are typical examples where a container is a good choice:

* **IIS lift and shift**: If you have existing [ASP.NET MVC](https://www.asp.net/mvc) apps that you want to continue to use, put them in a container instead of migrating them to ASP.NET Core. These ASP.NET MVC apps are dependent on Internet Information Services (IIS). You can package these into container images from the precreated IIS image and deploy them with Service Fabric. See [Container Images on Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/quick_start/quick_start_images) for information about how to create IIS images.
* **Mix containers and Service Fabric microservices**: Use an existing container image for part of your application. For example, you might use the [NGINX container](https://hub.docker.com/_/nginx/) for the web front end of your application and stateful services built with Reliable Services for the more intensive back-end computation. An example of this scenario includes gaming applications.
* **Reduce impact of "noisy neighbors" services**: You can use the resource governance ability of containers to restrict the resources that a service uses on a host. If services might consume a lot of resources and affect the performance of others (such as a long-running, query-like operation), consider putting these services into containers that have resource governance.

## Service Fabric support for containers
Service Fabric currently supports deployment of Docker containers on Linux and Windows Server containers on Windows Server 2016. Support for Hyper-V containers will be added in a future release.

In the Service Fabric [application model](service-fabric-application-model.md), a container represents an application host in which multiple service replicas are placed. The following scenarios are supported for containers:

* **Guest containers**: This scenario is identical to the [guest executable scenario](service-fabric-deploy-existing-app.md) where you can deploy existing applications in a container. Examples include Node.js, JavaScript, or any code (executables).
* **Stateless services inside containers**: These are stateless services that use the Reliable Services and the Reliable Actors programming models. This is currently only supported on Linux. Support for stateless services in Windows containers will be added in future release.
* **Stateful services inside containers**: These are stateful services that use the Reliable Actors programming model on Linux. Reliable Services are not currently supported on Linux.  Support for stateful services in Windows containers will be added in future release.

Service Fabric has several container capabilities that help you build applications that are composed of microservices that are containerized. These are called containerized services. The capabilities include:

* Container image deployment and activation.
* Resource governance.
* Repository authentication.
* Container port to host port mapping.
* Container-to-container discovery and communication.
* Ability to configure and set environment variables.

## Next steps
In this article, you learned about containers, that Service Fabric is a container orchestrator, and that Service Fabric has features that support containers. As a next step, we will go over examples of each of the features to show you how to use them.

[Deploy a Windows container to Service Fabric on Windows Server 2016](service-fabric-deploy-container.md)

[Deploy a Docker container to Service Fabric on Linux](service-fabric-deploy-container-linux.md)

[Image1]: media/service-fabric-containers/Service-Fabric-Types-of-Isolation.png
