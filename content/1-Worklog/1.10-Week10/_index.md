---
title: "Week 10 Worklog"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objectives:

* **Infrastructure as Code (IaC):** Learn to provision resources using code (CloudFormation).
* **Configuration Management:** Use AWS Systems Manager (Parameter Store) for managing configuration secrets.
* **Cognito Authentication:** Implement user sign-up and sign-in flows for web applications.
* **Project Backend Development:** Begin core API implementation for the Jewelry Store.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 1 | - AWS CloudFormation Basics <br> - **Practice:** <br>&emsp; + Write a YAML template to launch the Project VPC and Database | 03/11/2025 | 03/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 2 | - AWS Systems Manager (Parameter Store) <br> - **Practice:** <br>&emsp; + Store database credentials securely <br>&emsp; + Retrieve secrets in backend code programmatically | 04/11/2025 | 04/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Amazon Cognito (User Pools) <br> - **Practice:** <br>&emsp; + Create a User Pool for Jewelry Store Customers <br>&emsp; + Implement Register/Login API endpoints | 05/11/2025 | 05/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - **Team Project (Backend):** Database Design <br> - **Practice:** <br>&emsp; + Design Schema for 'Products' and 'Categories' <br>&emsp; + Provision RDS or DynamoDB tables via CloudFormation | 06/11/2025 | 06/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - **Team Project (Backend):** Product API <br> - **Practice:** <br>&emsp; + Develop CRUD APIs for Jewelry Products (GET/POST /products) <br>&emsp; + Test with Postman | 07/11/2025 | 07/11/2025 | <https://cloudjourney.awsstudygroup.com/> |

### Week 10 Achievements:

* Provisioned the project's foundational infrastructure (Network + DB) using CloudFormation templates.
* Secured backend configuration using Systems Manager Parameter Store instead of hardcoding secrets.
* Implemented secure user authentication using Amazon Cognito for the Jewelry Store application.
* Delivered the initial "Product Catalog" APIs for the frontend team to consume.