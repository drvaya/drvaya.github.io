---
layout: post
title:  "Multi-Tenant Solutions on Kubernetes Ecosystem"
author: dharam
categories: [ tutorial ]
tags: [yellow]
image: assets/images/KZ-MultiTenK8S.png
description: "Understanding the approaches towards implementing a Multi-Tenant solution for Kubernetes workloads."
featured: true
---

# Multi-Tenant Solutions on Kubernetes Ecosystem

## Overview

### What is a Multi-Tenant Application / What is Multi-Tenancy ?

In simpler terms, any technical architecture that uses a single logical unit or instance of the Application in order to serve multiple tenants is termed as Multi-Tenant Application. Here the definition of Tenant can vary as per use-cases, in some instances it can be a Customer, or Department, Domain or even simply put Users.

### Why is Multi-Tenancy preferred ?

When it comes to SaaS applications, it becomes essential to have a scalable infrastructure, however at the same time maintain a robust level of isolation especially when it comes to data. This becomes crucial as most SaaS applications would be managing customer data.

## Analysis

### Architecture Patterns in Multi-Tenancy

Typically Multi-tenant applications have evolved into the following 3 architecture patterns -

a) Database-per-Tenant

b) Schema-per-Tenant

c) Shared Database (Discriminator Column for Tenants)

Each of the above approaches has its own pros-cons. Choosing the right pattern will depend on several factors based on the use-case under consideration. Typically, the first 2 approaches are suitable for SaaS applications that require the highest level of isolation between data. However, it also needs to be understood that these approaches don't provide the best scalability in terms of infrastructure esp. in data heavy applications. In that case, the third option using the Discriminator Column becomes suitable. The downside to this approach is the fact that it provides the least isolation, so lets say something breaks in your database, all tenants (customers) get affected.

Sometimes, depending on the business model - a trade-off needs to be managed and teams might end up using a Hybrid approach depending on the segmentation/category of users.

## Multi-Tenancy in Kubernetes

Multi-tenancy in Kubernetes implies that a cluster alongwith its control plane are shared by multiple users, workloads, or applications (Tenets). It can be of types -

### Soft Multi-Tenancy

Soft multi-tenancy is a form of multi-tenancy that does not provide a strong isolation of the tenants, rather it's only based on isolation using namespaces.

### Hard Multi-Tenancy
For Saas based workload, this is something that provides a strict isolation between clusters and prevents any negative situations arising out of any rogue tenant/users.

## Kubernetes based workloads

Based on the scope of this document/task, given that an application is deployed on the Kubernetes ecosystem, here are the possible approaches and technical insights on achieving multi-tenancy.

In case of K8S, the level of multi-tenancy will be at the following components or layers of the ecosystem (hierarchical) - Cluster, Namespace, Node, Pod and Container. K8S natively does not provide adequate capabilities to achieve multi-tenancy, while it gets managed for small clusters and data, as the application ecosystem expands, things do get complicated.

![](https://lh3.googleusercontent.com/2sJyTmnAPoWt_2jKanPvpDVqPWwOd22BiHK2VpNiyxMqmyJ-AlVTHBOFTSXkNuVacxjuPSlrR6OVh-IgXUcC4X01ZacIqSmxEI-uHi8tWRBF6d45N3MblJw1ODbxHJ5Okzud5-uC=s0)

When it comes to SaaS workloads, the tenants are the 'instances' of application per-customer as well as the SaaS control plane. Application instances can be managed on the basis of their namespaces, wherein end-users would actually be interacting with a SaaS interface that indirectly interacts with the K8S control plane thus maintaining the required isolation/abstraction.

  
 
## Considerations/Challenges in implementation of Multi-Tenancy using K8S

#### User Management

Any multi-tenant implementation should be capable of handling any existing SSO based authentication or User Management.

#### ResourceSharing

Tenants will be sharing the same infrastructure and the multi-tenancy should be capable of providing a fair resource management, this can be achieved using native policies like Limit Range or Resource Quotas.

#### Isolation

One of the important considerations is the degree of isolation that any multi-tenancy can provide. While namespace provides the basic level of isolation, optimized policies around Networking will make the isolation more secure and resilient to any cross-tenant attacks too. (Istio Service Mesh is something to be considered in the long run/next phase to manage this, but for now the strategy should focus on 'one thing at a time').

| Framework/ Product 	| Kiosk                                                                                                                                     	| Loft                                                                                                                      	| vCluster                                                                        	| Capsule                                                                                                                                 	|
|--------------------	|-------------------------------------------------------------------------------------------------------------------------------------------	|---------------------------------------------------------------------------------------------------------------------------	|---------------------------------------------------------------------------------	|-----------------------------------------------------------------------------------------------------------------------------------------	|
| Features           	| Accounts & Account Users  Self-Service Namespace  Namespace Templates Lacks a few security features (Namespace capabilities used by loft) 	| Accounts & Account Users  Self-Service Namespace  Virtual Clusters Secure Cluster sharing Supports all major public cloud 	| Accounts & Account Users  Self-Service Namespace  Virtual Clusters (OS by Loft) 	| Namespace level isolation Users can be configured as needed Lacks native security and networking features. Lacks adequate documentation 	|
| Learning Curve     	| High                                                                                                                                      	| Medium                                                                                                                    	| High                                                                            	| Low                                                                                                                                     	|
| Integrations       	| OS - Does not natively support, but can work with custom integrations                                                                     	| Natively supports CI/CD, Monitoring, Prometheus and quite a number of apps.                                               	| (Originally developed by Loft - Supports major required integrations)           	| OS - Needs to be built on top of Capsule                                                                                                	|
| Commercial Aspects 	| Open-Source                                                                                                                               	| Open-source /Enterprise                                                                                                   	| Open-Source                                                                     	| Open-Source                                                                                                                             	|

## Loft Architecture and related Details

  
  

Loft consists of several components:

-   API Gateway: The gateway decides based on incoming requests how to route them. They are either routed to an external Kubernetes cluster or the local Loft Kubernetes API Server, or are handled internally.
    
-   Kubernetes API Server: The local Kubernetes API server contains the business logic of Loft and introduces a new API group management loft.sh.
    
-   Kubernetes Operator: This component watches for changes to User, Team and Cluster objects in the management cluster.
    
The Loft pod itself is stateless and everything that is stored is stored within Kubernetes custom resource definitions. Most communication (except some OIDC and authentication routes) is done via Kubernetes requests that access the local Loft Kubernetes API server.

  
  

![](https://lh3.googleusercontent.com/yA42voP6oaDNiaIwSKs2P1nvexHBGPQrzRzOVg5tMcaM2mx25PT4UC-RoVGhwoC2lOACW2NXR53YmJWNbX7cXZrmViy31qyCfsBx6czJ0Ma-QV8U_SxyVhvoObv_mXN2UNXduQYn=s0)

## *For a cost efficient solution, vCluster also can be considered. However in the long-run loft.sh is more suitable from the user ecosystem perspective.*
