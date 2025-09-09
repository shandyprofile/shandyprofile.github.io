---
title: Introduction to Cloud Computing
description: >-
  Introduction to Cloud Computing
author: [shandy]
date: 2025-09-05
categories: [Web Development with Python, Web Development with Python]
tags: [Web Development with Python]
sort_index: 101
# pin: true
# media_subpath: '/posts/01'
---
[Mooc 1: Introduction to Cloud Computing](https://www.coursera.org/learn/introduction-to-cloud)

## Introduction to Cloud Computing

- Core concepts of cloud computing.
- Foundational knowledge required for understanding cloud computing from a business perspective as also for becoming a cloud practitioner.
- Definition and essential characteristics of cloud computing, its history, the business case for cloud computing, and emerging technology usecases enabled by cloud.
- Introduce to some of the prominent service providers of our times (e.g. AWS, Google, IBM, Microsoft, etc.) the services they offer, and look at some case studies of cloud computing across industry verticals.
- Learn about the various cloud service models (IaaS, PaaS, SaaS) and deployment models (Public, Private, Hybrid) and the key components of a cloud infrastructure (VMs, Networking, Storage - File, Block, Object, CDN).
- Cover emergent cloud trends and practices including - Hybrid Multicloud, Microservices, Serverless, DevOps, Cloud Native and Application Modernization.
- The basics of cloud security, monitoring, and different job roles in the cloud industry.
- Created your own account on IBM Cloud and gained some hands-on experience by provisioning a cloud service and working with it.

## Introduction to Cloud Computing 

NIST Definition of Cloud Computing

To establish a common understanding, the US National Institute of Standards and Technology (NIST) defines cloud computing as:

A model for enabling convenient, on-demand network access to a shared pool of configurable computing resources (such as networks, servers, storage, applications, and services) that can be rapidly provisioned and released with minimal management effort or service provider interaction.

This cloud model is built upon:
- Five essential characteristics
- Four deployment models
- Three service models

In this section, we will focus on the five essential characteristics.

---

### Five essential characteristics of cloud computing

| Characteristic | Definition | Meaning | Example | Benefit |
| - | - | - | - | - |
| **On-Demand Self-Service** | Users can provision computing resources automatically whenever needed, without human interaction. | Users have full control to access and manage resources instantly. | Creating a new virtual machine on AWS in minutes, like withdrawing cash from an ATM 24/7.  | Saves time, increases autonomy, eliminates delays in infrastructure provisioning. |
| **Broad Network Access** | Resources are available over the network and accessed through standard devices. | Services can be reached from anywhere using Internet or Intranet, on multiple devices. | Accessing Google Docs from a laptop, tablet, or smartphone while traveling. | Enables mobility, remote work, and device independence. |
| **Resource Pooling** | Provider’s resources are pooled to serve multiple customers using a multi-tenant model. | Customers share the same infrastructure, but resources are dynamically allocated. | AWS data centers serve thousands of businesses worldwide, regardless of server location.   | Reduces cost through economies of scale, improves efficiency of resource utilization. |
| **Rapid Elasticity** | Resources can be scaled up or down quickly based on demand. | System adapts automatically to workload fluctuations. | E-commerce site scaling up servers during Black Friday sales, then scaling down afterward. | Ensures performance during peak times, prevents waste when demand decreases. |
| **Measured Service** | Resource usage is monitored and billed according to actual consumption (pay-as-you-go). | Customers pay only for what they use, similar to utility billing. | Paying for 500GB of cloud storage on Amazon S3; higher bill if usage grows to 2TB. | Cost transparency, optimized spending, avoids overpaying for unused resources. |

  Video: Definition and Essential Characteristics of Cloud Computing - Expert Overview 

---

### Expert Perspectives

1. Cloud Native Developer

For a cloud native developer, cloud computing boils down to two key ideas: on-demand and self-service.

- Developers should be able to log into a portal, request the exact amount of compute, storage, or platform services they need, and get them instantly.
- No waiting for procurement, no delays from infrastructure teams—just immediate access.
- This applies across the stack: infrastructure-as-a-service (IaaS), platform-as-a-service (PaaS), and software-as-a-service (SaaS).

2. Application Developer

Another expert views cloud computing primarily through the lens of deployment and scale.
- It enables code or infrastructure to be deployed anywhere in the world, with the ability to support large-scale, distributed systems.
- Key characteristics emphasized here include high availability, resiliency, and scalability, which are crucial for modern applications that need to serve users globally.

3. Cloud Services Specialist

From a services perspective, cloud computing means delivering a broad range of computing services—servers, databases, networking, analytics—over the Internet.
- Attributes highlighted include flexible cost structures, global reach, faster deployment, higher productivity, and greater performance and security.
- Businesses benefit from faster time to market and the ability to experiment without heavy upfront investment.

4. Data & Machine Learning Engineer

For data-focused professionals, cloud computing represents virtually unlimited access to compute, storage, and machine learning capacity.
- Instead of waiting for servers to be ordered, installed, and configured in a data center, resources are ready within seconds.
- Cloud providers handle physical security and availability, leaving users to focus on applications, data, and models.

5. Enterprise Architect

An enterprise architect describes cloud computing as a suite of API-driven, software-defined services for managing compute and networking resources.
- Cloud is not just about infrastructure; it is about programmable, automated control.

- Key attributes include:
  - Elasticity: systems grow or shrink based on demand, like adding capacity for holiday e-commerce traffic.
  - Dynamic cost models: shifting from large capital expenses (hardware purchases) to operational expenses (pay-as-you-go).

6. Cloud Adoption Strategist

From a strategic viewpoint, cloud computing is not all-or-nothing. It exists on a spectrum.
- Organizations can adopt it gradually—for example, using cloud only for authentication services like social logins (Google, Facebook).
- Or they can fully embrace cloud by hosting their entire application infrastructure there.
- The point is that almost everything today relies on some form of cloud computing, whether small or large.

7. Pragmatic Engineer

One engineer shares a common joke: “Cloud computing is just using someone else’s computer over the Internet.”
- While there’s truth in that, cloud computing is much more.
- It provides levels of availability, consistency, and scalability that most organizations could not achieve with their own hardware.
- Most importantly, it abstracts away many operational burdens, letting teams focus on what matters most to their business.

8. High Performance Computing Advocate

Finally, for those working in high performance computing (HPC), the cloud is revolutionary.
- Companies can access clusters of powerful machines on demand.
- They only pay for them while they are in use.
- This has democratized innovation, enabling even smaller companies to harness resources once available only to large research labs or tech giants.

---

### Cloud Adoption Strategy: Key Considerations
Key Considerations for Cloud Adoption

- Infrastructure and Workloads
- Building and operating traditional data centers is expensive.
- Cloud computing reduces costs with low upfront investment and a pay-as-you-go model.

However, not all workloads are cloud-ready; some may need re-engineering before migration.

SaaS and Development Platforms

Organizations should evaluate whether subscribing to SaaS applications is more cost-effective than buying off-the-shelf software and managing upgrades.

Cloud enables faster deployment: applications can be live in hours versus weeks or months in traditional environments.

Productivity gains come from features like cloud dashboards, real-time statistics, and active analytics.

Benefits of Cloud Adoption

Cloud benefits can be grouped into three broad categories: Flexibility, Efficiency, and Strategic Value.

1. Flexibility

Services can be scaled up or down to meet demand.

Applications can be customized, and services accessed anywhere with an Internet connection.

Organizations can choose their level of control (IaaS, PaaS, SaaS).

Built-in features like encryption and API keys enhance data security.

2. Efficiency

Applications can be brought to market quickly without managing infrastructure.

Data and applications are accessible from almost any device.

Hardware failures don’t cause data loss thanks to cloud backups.

Organizations save on server/equipment costs by paying only for what they use.

3. Strategic Value

Cloud services provide access to cutting-edge technologies.

They reduce the burden of infrastructure management, allowing businesses to focus on innovation and strategic priorities.

Cloud adoption creates a competitive advantage by enabling faster, smarter business operations.

Challenges and Risks

While the opportunities are substantial, cloud adoption also raises important challenges:

Data security risks: potential loss or unavailability of data leading to business disruption.

Governance and sovereignty: questions around who controls data and where it is stored.

Legal, regulatory, and compliance issues: ensuring cloud use complies with industry and government standards.

Lack of standardization: evolving cloud technologies may not always integrate smoothly.

Choosing the right models: selecting deployment (public, private, hybrid, community) and service models (IaaS, PaaS, SaaS).

Vendor dependency: finding the right cloud service provider and avoiding lock-in.

Business continuity & disaster recovery: ensuring resilience in case of outages.

  Video: Key cloud service providers and their services

  Reading: Lesson Summary

LESSON 2: Business case for Cloud Computing

  Video: Cloud adoption - No longer a choice

  Video: Expert Viewpoints: Cloud Adoption Benefits and Usecases

  Video: Cloud adoption - Some case studies

  Reading: Lesson Summary

LESSON 3: Emerging Technologies accelerated by cloud

  Video: Internet of Things on the cloud

  Video: Artificial Intelligence on the cloud

  Video: Blockchain and Analytics on the cloud

  Reading: Lesson Summary

  Practice Assessment

  Graded Assessment