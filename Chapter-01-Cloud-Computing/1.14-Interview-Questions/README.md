# 1.14 Master Interview Questions 🎤

Welcome to the Master Interview Questions bank for **Chapter 1: Cloud Computing**. These questions are categorized by difficulty and format to help you prepare for AWS Cloud Practitioner, Solutions Architect, and general DevOps interviews.

---

## 🟢 Basic Level Questions
*These questions test your fundamental understanding of vocabulary and concepts.*

<details>
<summary>1. What is Cloud Computing?</summary>
The delivery of computing services (servers, storage, databases, networking) over the internet. It offers faster innovation, flexible resources, and economies of scale, typically utilizing a pay-as-you-go pricing model.
</details>

<details>
<summary>2. What are the three primary deployment models of Cloud Computing?</summary>
Public Cloud, Private Cloud, and Hybrid Cloud.
</details>

<details>
<summary>3. Name the 5 essential characteristics of Cloud Computing according to NIST.</summary>
1. On-demand self-service
2. Broad network access
3. Resource pooling
4. Rapid elasticity
5. Measured service
</details>

<details>
<summary>4. What is a Data Center?</summary>
A secure physical facility that houses massive amounts of computing infrastructure, including servers, storage, networking equipment, power backups, and cooling systems.
</details>

<details>
<summary>5. What does CapEx and OpEx stand for?</summary>
CapEx: Capital Expenditure (Upfront investment in physical assets).
OpEx: Operational Expenditure (Ongoing cost for running a product/service, like pay-as-you-go billing).
</details>

<details>
<summary>6. What is a Hypervisor?</summary>
A software layer that enables virtualization. It sits between the physical hardware and the operating systems, allowing multiple isolated Virtual Machines to run on a single physical host.
</details>

<details>
<summary>7. What is the difference between a Type 1 and Type 2 Hypervisor?</summary>
Type 1 (Bare Metal) is installed directly on the physical hardware (e.g., VMware ESXi). Type 2 (Hosted) is installed as an application on top of an existing host OS (e.g., VirtualBox).
</details>

<details>
<summary>8. Define a Virtual Machine (VM).</summary>
A software-based emulation of a physical computer that runs its own Guest Operating System and applications in isolation from other VMs on the same hardware.
</details>

<details>
<summary>9. What is meant by "High Availability"?</summary>
A system design approach that ensures an agreed level of operational performance (uptime) for a higher than normal period, usually by eliminating single points of failure through redundancy.
</details>

<details>
<summary>10. What is AWS, and who is its parent company?</summary>
Amazon Web Services (AWS) is the world's most comprehensive and broadly adopted cloud platform. Its parent company is Amazon.
</details>

---

## 🟡 Intermediate Level Questions
*These questions require you to explain the "why" and "how" behind the concepts.*

<details>
<summary>11. Why did traditional IT infrastructure lead to resource wastage?</summary>
Because companies practiced a "one application per server" model for isolation. Since servers are powerful, the application might only use 10-15% of the CPU/RAM, leaving the rest of the capacity wasted, even though the company paid for 100% of the hardware and power.
</details>

<details>
<summary>12. How does virtualization solve the problem of low resource utilization?</summary>
By using a hypervisor to safely run multiple isolated Virtual Machines on a single physical server. This allows a company to consolidate 10 physical servers running at 10% capacity into 1 physical server running 10 VMs at 80% capacity.
</details>

<details>
<summary>13. Explain the concept of Multi-Tenancy.</summary>
It is an architecture in which a single instance of a software application (or underlying physical hardware in the case of cloud) serves multiple customers (tenants). The tenants share the physical resources, but the hypervisor keeps their data completely isolated and secure.
</details>

<details>
<summary>14. Why is a Hybrid Cloud considered the most complex deployment model?</summary>
Because IT teams must manage two entirely different environments (On-Premise Private Cloud and AWS Public Cloud). They must ensure seamless, secure networking between them, synchronize data, and maintain consistent security and compliance policies across both boundaries.
</details>

<details>
<summary>15. How do "Economies of Scale" apply to AWS pricing?</summary>
AWS operates data centers at a massive, unprecedented scale. This allows them to negotiate massive discounts on hardware, electricity, and internet bandwidth. They pass these savings to the customer, making it cheaper for a company to rent from AWS than to buy their own servers.
</details>

<details>
<summary>16. What is Cloud Repatriation, and why does it happen?</summary>
The process of moving workloads from the public cloud back to an on-premise data center. It usually happens when a massive enterprise reaches a point where their workloads are so large and predictable that buying hardware (CapEx) becomes cheaper over a 5-year period than paying monthly cloud rental fees (OpEx).
</details>

<details>
<summary>17. Describe how a Hypervisor provides Isolation.</summary>
The hypervisor acts as an impenetrable barrier between VMs. It manages memory and CPU scheduling so that if VM A gets a virus, crashes its kernel, or attempts to consume 100% of the RAM, the hypervisor intervenes to protect the physical hardware and ensures VM B continues to run completely unaffected.
</details>

---

## 🔴 Scenario-Based Questions
*These simulate real-world architectural decisions you will face on the job.*

<details>
<summary>18. Scenario: You are the CTO of a fast-growing startup. Your traffic fluctuates wildly. Sometimes you have 100 users, sometimes you have 100,000. Your CFO wants to buy $50,000 worth of physical servers to handle the peak load. How do you respond?</summary>
I would advise against it. Buying servers for peak load means during low traffic, 90% of our $50,000 investment is sitting idle (wasted CapEx). Instead, we should use the Public Cloud. Its "Rapid Elasticity" will allow us to automatically scale up to handle the 100,000 users, and instantly scale down when traffic drops, utilizing the Pay-As-You-Go model to only pay for what we use.
</details>

<details>
<summary>19. Scenario: A government hospital wants to create a patient portal. The frontend needs to handle massive global traffic, but by law, the patient medical records cannot reside on shared, public infrastructure. Architect a high-level solution.</summary>
I would architect a Hybrid Cloud solution. The frontend web servers and load balancers will be deployed in the Public Cloud (AWS) to handle the massive traffic spikes and utilize edge locations for speed. The patient database will be deployed in a Private Cloud (On-Premise data center). The two environments will communicate via a highly secure, encrypted connection (like AWS Direct Connect or a site-to-site VPN).
</details>

<details>
<summary>20. Scenario: Your development team wants to test a new application on Linux, Windows, and macOS simultaneously. They ask you to purchase three new physical laptops. How can you solve this faster and cheaper?</summary>
I would use the Public Cloud. I can instantly provision three Virtual Machines (EC2 instances) running Linux, Windows, and macOS. The team can run their tests for a few hours, and then we will terminate the instances. The total cost will be a few dollars (OpEx) instead of thousands of dollars (CapEx), and they can start testing in 5 minutes instead of waiting days for laptops to ship.
</details>

<details>
<summary>21. Scenario: A legacy banking application takes 3 weeks to deploy a new feature because the hardware team has to rack servers, the networking team configures switches, and the OS team installs Linux. How does Cloud Computing solve this?</summary>
Cloud computing provides "On-Demand Self-Service." Instead of opening tickets with hardware and networking teams, a developer can use code or a web console to provision the entire infrastructure (servers, networks, OS) in minutes via APIs. The cloud provider has already handled all the physical racking, cabling, and power.
</details>
