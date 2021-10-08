---
layout: post
title:  "Part 1 : Why Anthos ?"
author: dharam
categories: [ tutorial ]
tags: [yellow]
image: assets/images/KZ-P1-Anthos.png
description: "A blog series on understand and exploring Anthos from Google Cloud Platform"
featured: true
---



# Part 1: Why Anthos ?

As an enterprise decision maker the top 3 questions that would occur to you would be - Do you really need it ? What are the competing services ? and after you have made some opinion you would like to reinforce your understanding with a few use-cases. Exactly the points that are covered in this blog series that uncovers some facts and features around Anthos.

## Is there really a need ?

In my personal opinion, Google has always been more inclined to have a developer centric approach when it comes to products and services. However, with Cloud this notion definitely needed a change. To reap the maximum benefits of Cloud, its adoption at a larger scale makes a significant difference and Enterprises are the best customers when it comes to large scale implementations as well as the scale at which they can innovate. (That does not mean startups or small scale segments don't innovate - it's just that the budgets and capacity is fairly wide with deep pockets at Enterprises).

Although Cloud adoption at Enterprises has been slow, it's also because they are governed by a lot of factors like Implementation Impact, Scale, Compliance, Business Continuity, Transformation Vision to name a few. One important aspect that I would highlight is their capacity to expand into a Multi-Cloud realm. And to tap this segment of Enterprise business, Google has a clear vision. As a Cloud Platform, GCP has quite a diverse set of capabilities that might not exist with other players (and in some areas Google is catching up quickly).

From being Developer Centric to a paradigm shift into being Enterprise Centric, there was a gap that had to be addressed. Arrives Anthos - a possible one-stop solution that drives Enterprise Application Modernisation. Enterprises (or any Business) that has heavily forayed into either On-Premise base investments or other Public Cloud Providers - can now still make the best use of the potential of Google Cloud Platform.  
Anthos is a 'managed application platform that extends Google Cloud services' and engineering practices 'to your environments' so you can modernize apps faster and establish operational consistency across them.

Anthos is not a single product or service, rather consider it as a framework that caters to your Enterprise IT/Cloud needs. It is a strategic approach that helps you in Modernizing and Transforming your journey to Cloud, maintaining a focus on your business objectives.

*That's precisely the answer to the question - Is there really a need ?*
  

## What does the competition look like ?
 
*Competition would be the wrong term here - Each Cloud providers knows their strengths and in the end enterprises are going to tread the multi-cloud way.*

If you are a CIO/Enterprise Decision Maker reading this, you would have this natural question about a comparison against similar offerings from major Public Cloud players. 

Let's get straight into this, consider the top 3 Cloud Leaders (as per Gartner Insights) - Amazon WebServices (AWS), Microsoft's Azure and Google Cloud Platform (GCP). 

Before that let's define the thin line of difference between Hybrid Cloud and Multi-Cloud as they often get used interchangeably. Simply put, Hybrid Cloud would always include your Private Cloud (On-Premise) along with a Public Cloud Service; whereas Multi-Cloud is always two or more Public Cloud entities without a Private Cloud. Multi-Cloud architectures become Hybrid as soon as a Private Cloud entity comes into the picture.

Now let's head back into the offerings by the top 3 Clouds, do note that each of these public Clouds provides the services to fundamentally solve a similar problem, however the form/medium of this service differs to a large extent. These solutions that come closest to this problem are - AWS Outpost, Azure Stack and of course we are considering Google Anthos here.

AWS Outposts is essentially an actual Physical Hardware that is shipped to your location, take the analogy of a DTH connection - to use the service you get a physical Dish and a set-top box. This also means that you have the least control/flexibility on the hardware to be used, but you have AWS to manage the Infrastructure setup and Management for you. Also if you are a heavy consumer of Serverless components, then this is not an ideal service.

Next in line is Azure Stack - I can say that Microsoft being an Enterprise heavy service provider, provides quite a lot of variants in how your workload is distributed. There are variants like Azure Stack HCI and Azure Stack Hub -- here you are responsible for your hardware (On-Premise), while another variant in the form of Azure Stack Edge is essentially a hardware-as-a-service to give you more of a Cloud to Edge kind of experience.

Let's talk about Anthos now - What differentiates it from the above two solutions ?

One major differentiating factor is the capability of Anthos being natively Multi-Cloud Solution as well as with the capability to work into a Hybrid-Cloud architecture. This also means Anthos certainly has an edge when it comes to choosing an Enterprise Cloud Provider because it removes any kind of lock-in and works seamlessly with other Cloud Providers too. Also as an Enterprise, you are responsible for managing your own hardware (On-Premise), but now with an augmented capability - Anthos brings Google's Engineering services on your platform.

  

How is Anthos Multi-Cloud ready ? The answer at the core is Kubernetes, coming up on the next part in the series we talk about Anthos Components to understand it better.
  

## Use Cases

A lot said around products/services - where do we actually use this and under what kind of situations ? Typically, Anthos can be applied to any use-case that requires an Enterprise-grade container management service, with added capabilities to automate policy management and security; in a hybrid or multi-cloud environment. Was that too long - Let's simplify it.

***Consider a legacy Java Application intended to take a modernization journey to Cloud.***

 - Without rewriting any code of your application, make it running into
   Containers so that barriers to Cloud are minimal. 
 - Next would be deploying these containers into Anthos Clusters with a modern CI/CD
   approach.  
  - And lastly, the actual journey of modernization would be to gradually adopt new technical frameworks, microservices based approaches that make your applications more resilient and scalable.

Often, unknowingly or impulsively Enterprises take the reverse approach of breaking down their applications first into microservices then moving into Cloud - which may not be wrong in their business context - it would really depend on the overall Architecture and Business Vision.

That was a typical example of an Enterprise application modernization, let us consider a Multi-Cloud Scenario. Let us consider an e-Commerce retailer, as a part of the Tech Strategy - they have a Multi-Cloud approach. At the same time, you do not want to have any kind of vendor lock-in nor do you want to spend too much on upskilling your IT to manage multiple cloud workloads by having separate Cloud specific teams. The obvious solution would be to have a cloud-agnostic control plane that provides you visibility and control over your services across cloud platforms. And the choice here is Anthos - highly secure, reliable and cost-effective too - a balanced best-fit.


*Now that we have some context around Anthos and why/where it can be applied, let us try and understand some internal components of Anthos and what does its architecture look like - Coming up on Part 2 of this series.*