---
title: "Week 1 Worklog"
 
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---




### Tasks to be carried out this week:
| Day | Task | Reference Material |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------- |
| 1 | I. Introduction to Cloud Computing <br> &emsp; - Definition: Cloud computing is the on-demand delivery of IT resources over the Internet with pay-as-you-go pricing. <br> &emsp; - Key Benefits: Cost optimization, increased speed/agility, flexibility (easy scaling), and global reach. <br> &emsp; - AWS Differentiators: Market leadership, commitment to lower prices (economies of scale), and a customer-centric approach (Leadership Principles). | <https://youtu.be/b9pK1oG534Q?si=gR7O6gRMBqNt5u0R> |
| 2 | II. AWS Global Infrastructure <br> &emsp; - Core Components: Data centers, Availability Zones (AZs) (for fault isolation), and Regions (a collection of ≥ 3 AZs). <br> &emsp; - Edge Locations (Points of Presence — POPs): Secondary data centers for low-latency services like CloudFront (CDN), WAF, and Route 53 (DNS). <br> &emsp; - Region Selection Factors: Latency (e.g., Singapore for Vietnam), service availability (new features often appear first in US regions), and cost. | |
| 3 | III. AWS Management Tools <br> &emsp; - AWS Management Console: Web interface; importance of securing and limiting use of the Root User versus regular IAM users. <br> &emsp; - AWS CLI (Command Line Interface): Open-source tool for interacting with AWS services via commands (equivalent to API requests). <br> &emsp; - AWS SDK: Libraries that simplify application development by handling API lifecycle management (credentials, retries, data marshalling, etc.). | |
| 4 | IV. AWS Cost Optimization <br> &emsp; - Pricing Models: On-Demand, Reserved Instances / Savings Plans (long-term commitment), and Spot Instances (temporary resources). <br> &emsp; - Practices: Delete unused resources, schedule auto-shutdown for non-24/7 resources, and leverage serverless services where appropriate. <br> &emsp; - Cost Management Tools: Use AWS Budgets for alerts and automated actions, and implement Cost Allocation Tags to track spending by department/application. | |
| 5 | V. Working with AWS Support <br> &emsp; - Support Plans: Overview of the four main tiers — Basic, Developer, Business, and Enterprise. <br> &emsp; - Escalation: Option to briefly upgrade the support plan to expedite critical issue resolution when necessary. <br> &emsp; - Tools: Used the AWS Pricing Calculator for cost estimation and planning. | |

### Week 1 — Achievements

- Understood core cloud computing concepts and AWS's value proposition (benefits and differentiators).
- Identified AWS global infrastructure components (Regions, Availability Zones, Edge Locations) and region-selection considerations.
- Created and configured an AWS Free Tier account and installed/configured the AWS CLI.
- Learned to use the AWS Management Console, AWS CLI, and the role of AWS SDKs for application development.
- Gained awareness of cost-optimization techniques and tools (Pricing Calculator, Budgets, Cost Allocation Tags).
- Reviewed AWS support plans and escalation options for production issues.

### Week 1 — Difficulties / Challenges

- Initial unfamiliarity with the AWS CLI and IAM best practices (avoiding Root user use, managing credentials securely).
- Choosing the optimal region balancing latency, cost, and service availability.
- Understanding pricing models (On-Demand, Reserved/Savings Plans, Spot) and predicting costs accurately.
- Limited hands-on time to practice all topics in depth during the week.
- Ensuring secure handling of access keys and account configuration while following best practices.
