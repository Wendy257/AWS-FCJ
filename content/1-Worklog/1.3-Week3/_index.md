---
title: "Week 3 Worklog"
 
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---



### Week 3 Objectives:

* Differentiate between Site-to-Site VPN and Client-to-Site VPN use cases.
* Describe AWS Direct Connect benefits, components (Hosted/Dedicated Connections), and latency.
* Identify the function of ELB as a managed service for traffic distribution and health checking.
* Recognize that NLB supports static IP and high performance, while ALB supports path-based routing.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - VPN Site-to-Site Connection <br>&emsp; + Concept: Hybrid model connecting Data Center to AWS VPC <br>&emsp; + Components: Virtual Private Gateway (AWS side) & Customer Gateway (Client side) <br>&emsp; + Understanding connectivity across different IP ranges <br>                                                                                                   | 22/09/2025 | 22/09/2025      |    <https://cloudjourney.awsstudygroup.com/> |
| 2   | - VPN Client-to-Site & Direct Connect <br>&emsp;  + Client-to-Site: Use cases and AWS Marketplace solutions (e.g., Cisco Meraki)  <br>&emsp; + AWS Direct Connect: Benefits (Low latency 20-30ms) <br>&emsp; + Differentiating Hosted Connections (via Partners like Viettel, FPT) vs. Dedicated Connections <br>                                   | 23/09/2025 | 23/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Elastic Load Balancing (ELB) Fundamentals <br>&emsp;  + Concept: Managed service for traffic distribution (EC2/Containers)  <br>&emsp; + Key Features: Health checks & Access Logs (S3 integration) <br>&emsp; + Sticky Session: Concept and importance for maintaining user state <br> | 24/09/2025 | 24/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Application Load Balancer (ALB) & Network Load Balancer (NLB) <br>&emsp; + ALB (Layer 7): HTTP/HTTPS protocols and Path-based routing (/mobile vs /desktop) <br>&emsp; + NLB (Layer 4): TCP/TLS protocols, Static IP support, and High performance handling <br>&emsp;    <br>                            | 24/09/2025 | 24/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Other Load Balancers & Review <br>&emsp; + Classic Load Balancer (CLB - Layer 4/7) overview <br>&emsp; + Gateway Load Balancer (GLB - Layer 3) & GENEVE protocol <br>&emsp; + Review: Compare VPN vs. Direct Connect & ALB vs. NLB                      | 25/09/2025 | 25/09/2025      | <https://cloudjourney.awsstudygroup.com/> |


### Week 3 Achievements:

* Hybrid Connectivity Mastery: Understood how to connect Data Centers to AWS using Site-to-Site VPN (Virtual Private Gateway) or AWS Direct Connect (low latency, 20-30ms).
* VPN Strategy: Differentiated between Site-to-Site (infrastructure connecting) and Client-to-Site VPN (user access) use cases.
* ELB Proficiency: Distinguished between Application Load Balancer (Layer 7, path-based routing) and Network Load Balancer (Layer 4, static IP support, high performance).
* Traffic Management: Learned to use Sticky Sessions to maintain user state and Health Checks to ensure system reliability.
