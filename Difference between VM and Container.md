https://substack.com/home/post/p-169447424?source=queue

Note
Search Substack


The Cloud Handbook





The Cloud Handbook
📦 Containers vs 🔲 VMs
What are they, fundamental difference, architecture, use cases and more.
Kisan Tamang
Aug 05, 2025
When working in the cloud, there are two technologies you can not miss. And these two technologies absolutely dominate Cloud. They are containers and virtual machines (VMs).

Whether you're deploying jut a simple web app or orchestrating complex microservices, you'll encounter both of these fundamental technologies. Understanding when and how to use each one is crucial for any cloud practitioner.

Both containers and VMs solve the same core problem—they help you run applications in isolated environments—but they do it in fundamentally different ways.

In todya’s issue, we'll learn what makes each technology unique, when to use them, and how they fit into modern cloud architectures.

Let’s get started…

🔲 What Are Virtual Machines (VMs)?

A virtual machine is basically a complete computer system running inside another computer. Think of a VM like a full apartment in a building—it comes complete with its own kitchen, bathroom, electricity, and everything else needed to function independently. Each apartment (VM) is completely separate from the others, with its own resources and utilities.

Key Components of a VM
Hypervisor: This is the "landlord" that manages all the apartments (VMs) in the building (physical server). It allocates resources like CPU, memory, and storage to each VM. Popular hypervisors include VMware vSphere, Microsoft Hyper-V, and open-source options like KVM or VirtulaBox.

Guest Operating System: Each VM runs its own complete operating system—whether that's Windows, Linux, or any other OS. This is like each apartment having its own electrical system and plumbing.

Application Layer: On top of the guest OS, you install and run your applications, just like you would on a physical machine.

The best part of VMs is complete isolation. If one VM crashes or gets compromised, it doesn't affect the others. However, this comes at a cost—each VM requires significant resources to run its own operating system. When a VM resources are idel, another VM can not borrow that resources.

📦 What are Containers?

A container takes a different approach to isolation than VMs. Instead of being like a full apartment, a container is more like a room in a shared house. It's compact, lightweight, and shares some common utilities (like heating and water) with other rooms, but your personal belongings and space remain private.

Key Components of Containers
Container Engine: This is the shared house manager—tools like Docker or containerd that handle creating, running, and managing containers. The container engine sits on top of the host operating system and manages all containers.

Shared OS Kernel: Unlike VMs, all containers on a host share the same operating system kernel. This is like all rooms in the house sharing the same heating system and foundation—much more efficient than each room having its own.

Application and Dependencies: Each container packages an application along with all its dependencies (libraries, configuration files, etc.) into a single, portable unit. It's like packing everything you need for your room into a suitcase that you can move anywhere.

This approach makes containers incredibly lightweight and fast to start, but they sacrifice some isolation compared to VMs since they share the host OS kernel.

To learn more about Containers, read my previous issue. This issue deep dives how containers work under the hood.

📦 How Containers work? Deep dive into Containerization.
📦 How Containers work? Deep dive into Containerization.
Kisan Tamang and Sagar Lama
·
May 14, 2024
Read full story
And to learn how containerize react application, read this isssue.

🐳 How does Docker Work, Dockerize sample React Application, and Multi-stage Docker build. 🚀
🐳 How does Docker Work, Dockerize sample React Application, and Multi-stage Docker build. 🚀
Kisan Tamang and Sagar Lama
·
May 28, 2024
Read full story
⚖️ Key Differences: Containers vs VMs

VMs vs Containers
The table below shows the main differences between containers and VMs.


Differences between VMs and Containers
Real-World Examples
Virtual Machine
AWS EC2: Amazon's flagship compute service where you rent virtual servers

VMware vSphere: Enterprise virtualization platform used in data centers

Microsoft Azure Virtual Machines: Cloud-based VMs for Windows and Linux workloads

Container
Docker: The most popular container platform for packaging and running applications

Kubernetes: Container orchestration platform that manages containers at scale

AWS ECS/EKS: Amazon's container services for running Docker containers

When to Use What?
Use Virtual Machines When:
You need strong isolation: VMs provide complete separation between workloads, making them ideal for multi-tenant environments or when running untrusted code.

You run different operating systems: If you need to run Windows applications alongside Linux services, VMs are your only option.

You work with legacy systems: Older applications that were designed for specific OS configurations often work better in VMs where you can replicate their exact environment.

You have long-running, stable workloads: VMs are perfect for applications that run continuously and don't need frequent updates or scaling.

Use Containers When:
You want fast scaling: Containers can start in seconds, making them perfect for applications that need to scale up and down quickly based on demand.

You use microservices architecture: Containers excel at running small, focused services that work together to form larger applications.

You need developer agility: Containers make it easy to package applications with their dependencies, ensuring consistency across development, testing, and production environments.

You're building cloud-native applications: Modern applications designed for the cloud typically benefit from container's portability and efficiency.

☁️ Cloud Services
VM-Based Services:
AWS EC2: Virtual servers in the cloud with full control over the operating system

Azure Virtual Machines: Microsoft's cloud VM service with Windows and Linux options

Google Compute Engine: Google's infrastructure-as-a-service offering

VMware Cloud: Enterprise virtualization extended to public clouds

Container-Based Services:
AWS ECS (Elastic Container Service): Fully managed container orchestration service

AWS EKS (Elastic Kubernetes Service): Managed Kubernetes service

Azure AKS (Azure Kubernetes Service): Microsoft's managed Kubernetes offering

Google GKE (Google Kubernetes Engine): Google's container orchestration platform

Docker Desktop: Local development environment for containers

The Modern Trend: Containers and Kubernetes
The cloud industry has seen a significant shift toward containers in recent years, and for good reason. Containers have become central to modern DevOps practices and CI/CD pipelines because they solve several critical challenges:

Consistency Across Environments: The "it works on my machine" problem disappears when your application and all its dependencies are packaged together in a container.

Kubernetes: Kubernetes has emerged as the de facto standard for container orchestration, providing automated deployment, scaling, and management of containerized applications. It's like having a smart building manager that automatically adjusts resources based on demand.

Microservices Architecture: As companies break down monolithic applications into smaller, focused microservices, containers provide the perfect packaging mechanism. Each service can be developed, deployed, and scaled independently.

Cloud-Native Development: Major cloud providers have embraced containers as the foundation for serverless computing, with services like AWS Fargate and Google Cloud Run allowing you to run containers without managing the underlying infrastructure.

Many companies are migrating from VM-based architectures to container-based ones to gain agility, reduce costs, and improve deployment velocity. However, this doesn't mean VMs are obsolete—they still play crucial roles in hybrid architectures and specific use cases.

📚 Further Reading
To deepen your understanding of containers and VMs, here are some valuable resources:

Docker's Official Getting Started Guide

Kubernetes Documentation

Try Docker locally with Docker Desktop

Getting started with Amazon ECS

Azure Container Instances vs Virtual Machines

Google Cloud: Containers vs VMs

Books and Courses:

"Docker Deep Dive" by Nigel Poulton

"Kubernetes in Action" by Marko Lukša

Cloud provider certification courses (AWS, Azure, GCP)

Conclusion
Virtual machines and containers are both essential technologies in modern cloud computing, but they solve similar problems in fundamentally different ways. VMs provide complete isolation and are perfect for legacy systems, multi-OS environments, and workloads requiring strong security boundaries. Containers offer lightweight, portable, and fast-scaling solutions ideal for microservices, cloud-native applications, and modern DevOps practices.

While containers have gained tremendous popularity and are driving many modern cloud architectures, VMs still have their place in the ecosystem. The most successful cloud strategies often use both technologies, choosing the right tool for each specific use case.

As a cloud builder, understanding both containers and VMs—and knowing when to use each—is essential for designing efficient, scalable, and maintainable cloud architectures. The key is not to view them as competing technologies, but as complementary tools in your cloud computing toolkit.

📬 Get in touch
Liked this article? Feel free to drop ❤️ and restack with your friends.

If you have any feedbacks or questions 💬, comment below. See you in the next one.

You can find me on Twitter, Linkedin.

Sponsor The Cloud Handbook Newsletter
If you want to work with me or want to sponsor the newsletter, please email me at kisan.codes@gmail.com.

You can learn more about the Sponsorships here in details:

Sponsor The Cloud Handbook Newsletter!
Sponsor The Cloud Handbook Newsletter!
Kisan Tamang
·
Feb 17
Read full story
See you in the next one! Until then, keep building.

Thanks for reading The Cloud Handbook! We are a reader powered publication. So feel free to share it.

Share

Support The Cloud Handbook
By Kisan Tamang · Launched 2 years ago
Weekly Cloud, DevOps and System Design. Highly relevant for engineers, developers, and builders.
11 Likes
∙
1 Restack

11


1


