# 3.13 - Interview Questions Master List

## 🎯 Objective
This section compiles all the critical interview questions from Chapter 3 to help you prepare for AWS Cloud Engineer, DevOps, and Solutions Architect interviews.

---

### 🟢 Basic Questions

1. **What is Amazon EC2?**
   *Answer:* Elastic Compute Cloud. It provides resizable, virtual compute servers in the AWS cloud.

2. **What does "Elastic" mean in EC2?**
   *Answer:* The ability to automatically scale compute capacity up/down (vertical) or out/in (horizontal) based on application demand.

3. **What is an AMI?**
   *Answer:* Amazon Machine Image. It's a template containing the OS, software, and configuration required to launch an EC2 instance.

4. **Which port is used for SSH, and which port for RDP?**
   *Answer:* SSH uses Port 22 (for Linux). RDP uses Port 3389 (for Windows).

5. **What is a Security Group?**
   *Answer:* A virtual firewall acting at the instance level to control inbound and outbound traffic.

---

### 🟡 Intermediate Questions

6. **Explain the difference between a Region and an Availability Zone.**
   *Answer:* A Region is a geographical area (like Mumbai) containing multiple isolated locations. An Availability Zone is one of those isolated locations within a region, consisting of one or more physical data centers.

7. **How does an Elastic IP differ from a Public IP?**
   *Answer:* A standard Public IP changes if the instance is stopped and started. An Elastic IP is a static IPv4 address that persists and remains attached to your account until you explicitly release it.

8. **Compare On-Demand, Reserved, and Spot pricing.**
   *Answer:* On-Demand: Pay per hour/second, no commitment. Reserved: Commit for 1-3 years for massive discounts. Spot: Bid on unused capacity for the deepest discount, but the instance can be terminated by AWS with 2 minutes' notice.

9. **What is the difference between EBS and Instance Store?**
   *Answer:* EBS is persistent, network-attached block storage. Instance Store is ephemeral, physically attached storage (if the instance stops, data is lost, but it provides incredible I/O speed).

10. **Can you deny an IP address using a Security Group?**
    *Answer:* No. Security Groups only support ALLOW rules. To deny a specific IP, you must use a Network ACL (NACL) at the subnet level.

---

### 🔴 Scenario-Based / Advanced Questions

11. **You want your EC2 instances to fetch data from an S3 bucket. What is the most secure way to do this?**
    *Answer:* Attach an IAM Role with S3 permissions to the EC2 instance. Do NOT generate IAM user access keys and store them on the server.

12. **A developer launched an EC2 instance in a public subnet, but it cannot be reached from the internet. What three things should you check?**
    *Answer:* 
    1) Does it have a Public IP or Elastic IP?
    2) Is the Security Group configured to allow inbound traffic on the desired port?
    3) Does the VPC Route Table have a route to the Internet Gateway (IGW)?

13. **You created a Custom AMI in `us-east-1`, but your colleague in `eu-west-1` cannot see it. Why?**
    *Answer:* AMIs are Region-specific. You must copy the AMI from `us-east-1` to `eu-west-1`, which will give it a new AMI ID in the destination region.

14. **A production database running on `gp2` EBS volume is experiencing severe lag. CloudWatch shows it's maxing out IOPS. Without downtime, how can you fix this?**
    *Answer:* You can modify the EBS volume type on the fly. Change the volume from `gp2` to `io1` or `io2` (Provisioned IOPS) and specify the exact IOPS required. The modification happens in the background without instance downtime.

15. **Your web application receives heavy traffic during the day and almost zero at night. Which pricing model would you use for the baseline load, and which for the dynamic load?**
    *Answer:* Use Reserved Instances or Savings Plans for the baseline load (the minimum servers you always need running). Use On-Demand or Spot Instances paired with Auto Scaling for the dynamic daytime load.
