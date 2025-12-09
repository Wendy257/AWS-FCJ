---
title: "Proposal"
 
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# **Jewelry E-Commerce Platform**  
## **Cloud-Based Online Sales System Using React, .NET, and MySQL on AWS Lightsail**  



# AWS First Cloud AI Journey – **Project Plan**


# T1VN – FPT University – Video Share

# 09/12/2025

# Table of Contents

**[1 BACKGROUND AND MOTIVATION	3](#background-and-motivation)**

[1.1 EXECUTIVE SUMMARY	3](#executive-summary)

[1.2 PROJECT SUCCESS CRITERIA	3](#heading=)

[1.3 ASSUMPTIONS	3](#heading=)

[**2 SOLUTION ARCHITECTURE / ARCHITECTURAL DIAGRAM	4**](#heading=)

[2.1 TECHNICAL ARCHITECTURE DIAGRAM	4](#technical-architecture-diagram)

[2.2 TECHNICAL PLAN	4](#technical-plan)

[2.3 PROJECT PLAN	5](#heading=)

[2.4 SECURITY CONSIDERATIONS	5](#security-considerations)

[**3 ACTIVITIES AND DELIVERABLES	6**](#activities-and-deliverables)

[3.1 ACTIVITIES AND DELIVERABLES	6](#activities-and-deliverables-1)

[3.2 OUT OF SCOPE	8](#out-of-scope)

[3.3 PATH TO PRODUCTION	8](#path-to-production)

[**4 EXPECTED AWS COST BREAKDOWN BY SERVICES	9**](#expected-aws-cost-breakdown-by-services)

[**5 TEAM	10**](#team)

[**6 RESOURCES & COST ESTIMATES	10**](#resources-&-cost-estimates)

[**7 ACCEPTANCE	11**](#acceptance)

# **BACKGROUND AND MOTIVATION** 

## 1. **EXECUTIVE SUMMARY**

The AWS Jewelry Web project entails the development of a comprehensive Jewelry E-commerce Platform. The system architecture comprises a backend and database infrastructure hosted on AWS Lightsail, coupled with a React-based frontend deployed via Amazon S3 and CloudFront. This architecture is designed to ensure scalability, high security, and operational cost optimization by leveraging essential yet highly effective Cloud services.

**The system provides main features such as:**

* Jewelry product management.  
* Product image uploading capabilities.  
* Shopping cart functionality.  
* User registration and authentication via AWS Cognito.  
* Backend API operations on Lightsail with MySQL/Postgres data storage.  
* Content delivery acceleration and international standard SSL processing via CDN.

## 2.**PROJECT SUCCESS CRITERIA**

* **Performance:** Website load time under 2 seconds for international access, facilitated by CloudFront CDN.  
* **Stability:** Backend operates stably on Lightsail under actual traffic conditions.  
* **Data Integrity:** Secure database operations with rapid retrieval speeds.  
* **User Management:** Stable and secure user management via Amazon Cognito.  
* **Security:** Secure image uploading processes via Amazon S3.  
* **Monitoring:** Comprehensive API logging through Amazon CloudWatch.

## 3.**ASSUMPTIONS**

* **Traffic Volume:** Moderate traffic levels (less than 100,000 requests/month).  
* **Scaling:** No requirement for advanced autoscaling configurations.  
* **Domain:** Domain name is pre-acquired or will be purchased via Amazon Route 53\.  
* **Competency:** The development team possesses proficiency in Node.js and React.

# **SOLUTION ARCHITECTURE / ARCHITECTURAL DIAGRAM**

## 1.**TECHNICAL ARCHITECTURE DIAGRAM** 

![Architecture](/images/imageworkshop.png)

## 2.**TECHNICAL PLAN** 

* **Frontend:** Hosted on Amazon S3 with Amazon CloudFront CDN and HTTPS enabled via AWS Certificate Manager (ACM).  
* **Backend API:** .NET runtime environment on AWS Lightsail.  
* **Database:** MySQL/PostgreSQL hosted on AWS Lightsail.  
* **Authentication:** Amazon Cognito User Pool.  
* **Image Storage:** Amazon S3.  
* **Logging:** Amazon CloudWatch.  
* **Secrets Management:** AWS Secrets Manager.

## 3.**PROJECT PLAN**

The implementation timeline is estimated to range from 6 to 12 weeks, contingent upon the final project scope.

## 4.**SECURITY CONSIDERATIONS** 

* **Access Control:** S3 private access configured, granting read permissions exclusively to CloudFront.  
* **Credential Management:** API utilizes private keys stored securely in AWS Secrets Manager.  
* **Encryption:** Full system-wide HTTPS implementation.  
* **Authentication Security:** User login protected by Amazon Cognito.

# **ACTIVITIES AND DELIVERABLES** 

## 1.**ACTIVITIES AND DELIVERABLES** 

| Project Phase | Timeline | Activities | Deliverables / Milestones | MD |
| :---: | :---: | ----- | ----- | :---: |
| **Assessment** | Week 1 | \-  Analyze the Jewelry Web system requirements. \- Draft the Architecture Diagram. **\-**  Design the Database Schema (Lightsail MySQL/Postgres). \-  Identify necessary secrets (DB password, bucket name). | \- Architecture Diagram \- DB Schema \- List of Secrets | **5** |
| **Setup Base Infrastructure** | Week 2 | \-  Configure S3 hosting and React build. \-  Configure CloudFront CDN and ACM SSL. \-  Map domain via Route 53\. \-  Provision Lightsail API instance. \-  Provision Lightsail Database. \-  Configure Cognito Login. \-  Create Secrets Manager entries:   + Secret 1: DB\_PASSWORD   + Secret 2: APP\_CONFIG (bucket-name) \-  Assign IAM roles to API instances for secret retrieval permissions. \-  Enable CloudWatch logs. | \- Functional Frontend CDN \- Operational Backend API \- Database connectivity established \- Functional Cognito login \- Secrets Manager populated with DB password & bucket name | **7** |

| Setup Component 1 – Backend API | Week 3 | \- Implement API logic to retrieve DB password from Secrets Manager. \- Implement API logic to retrieve bucket names from Secrets Manager. \- Implement image upload to S3 (via presigned URL). \- Develop CRUD operations for jewelry products. \- Integrate Cognito authentication. \- Implement log transmission to CloudWatch. | \- Stable API operations \- Successful image upload functionality \- Successful Login functionality \- Hardcoded configurations fully replaced by Secrets Manager | 7 |
| :---: | :---: | :---- | :---- | :---: |
| **Setup Component 2 – Frontend React** | Week 4 | \- Develop Jewelry Shop UI. \- Implement Cognito Login interface. \- Implement jewelry image upload interface. \- Fetch data from API. \- Build and deploy to S3 \+ CloudFront. | \- Complete User Interface (UI) \- API Integration established | **7** |
| **Testing &  Go-live** | Week 5 | \- Integration testing (FE ↔ BE ↔ S3 ↔ DB). \- Security testing (IAM \+ Secrets Manager). \- End-to-end testing. | \- Test Report \- Security checklist | **5** |
| **Handover** | Week 6 | \- Provide instruction on using Secrets Manager for updating bucket names/DB passwords. \- Transfer AWS account ownership. \- Provide Runbook Documentation. | \- Comprehensive Runbook \- Project Closure | **5** |

## 

## 2.**OUT OF SCOPE** 

* AI/Machine Learning features.  
* Complex E-commerce functionalities.  
* Advanced image processing capabilities.  
* Multi-region deployment or Disaster Recovery (DR) sites.  
* Complex administrative systems.  
* Integration with third-party systems.

## 3.**PATH TO PRODUCTION**

* Operational Excellence Optimization.  
* Secrets Management – Production Hardening.  
* Extended Error Handling.  
* Deployment & Production Verification.  
* Disaster Recovery Plan.  
* Production Handover.

# **EXPECTED AWS COST BREAKDOWN BY SERVICES** 

| Service Name | Upfront cost | Monthly cost | Region |
| :---- | :---- | :---- | :---- |
| Amazon S3 | 0.00 USD |  0.26 USD | Asia Pacific (Singapore) |
| Amazon CloudFront (CDN cho FE) | 0.00 USD | 0.17 USD | Asia Pacific (Singapore) |
| AWS ACM | 0.00 USD | 0 USD | Asia Pacific (Singapore) |
| Amazon Route 53 | 0.00 USD | 0.50–1.00 USD | Asia Pacific (Singapore) |
| AWS Lightsail – Database  | 0.00 USD | 10–15 USD | Asia Pacific (Singapore) |
| Amazon Cognito | 0.00 USD | 2.00 USD | Asia Pacific (Singapore) |
| AWS Secrets Manager   | 0.00 USD | 0.40 USD | Asia Pacific (Singapore) |
| Amazon CloudWatch | 0.00 USD | 0.30 USD | Asia Pacific (Singapore) |
| AWS Lightsail – API Server | 0.00 USD | 5 \- 10 USD | Asia Pacific (Singapore) |

# **TEAM** 

**Partner Executive Sponsor**

| Name | Title | Description | Email / Contact Info |
| :---- | :---- | :---- | :---- |
|  |  |  |  |

**Project Stakeholders**

| Name | Title | Stakeholder for | Email / Contact Info |
| :---- | :---- | :---- | :---- |
|  |  |  |  |

**Partner Project Team**

| Name | Title | Role | Email / Contact Info |
| :---- | :---- | :---- | :---- |
| Nguyễn Duy Hiếu  | Product Owner | Project Manager (BE) | Hieundse185047@fpt.edu.vn |
| Lưu Ngọc Ngân Giang | Software Developer | Developer (BE) | luungocngangiang25@gmail.com |
| Nguyễn Huy Hoàng  | Software Developer | Developer (FE) | Hoangnhse185092@fpt.edu.vn |
| Trần Hồ Phương Khanh | Software Developer | Developer (FE) | khanhthpse185070@fpt.edu.vn |
| Tăng Khanh Nhi | Software Developer | Developer (FE) | tangkhanhnhi111@gmail.com |

**Project Escalation Contacts**

| Name | Title | Role | Email / Contact Info |
| :---- | :---- | :---- | :---- |
|  |  |  |  |

# **RESOURCES & COST ESTIMATES** 
| Resource | Responsibility | Rate (USD) / Hour |
| :---- | :---- | ----- |
| Solution Architects \[number of assigned headcount\] | \- Design and architect the AWS infrastructure ecosystem, incorporating services such as S3, CloudFront, Lightsail, ACM, Route 53, and Secrets Manager \- |12|
| Solution Architects \[number of assigned headcount\] |\-Provide strategic consultancy regarding security protocols and operational cost optimization.|12|
| Solution Architects \[number of assigned headcount\] |\- Conduct comprehensive reviews of deployment procedures to ensure adherence to industry best practices.|12| 
| Engineers \[number of assigned headcount\] | \- Execute the provisioning and deployment of core infrastructure components, including S3, CloudFront, Route 53, ACM, and Lightsail. |6|
| Engineers \[number of assigned headcount\] |\- Establish Continuous Integration/Continuous Deployment (CI/CD) pipelines and facilitate the deployment of frontend assets to Amazon S3. |6|
| Engineers \[number of assigned headcount\] |\- Deploy and configure the Node.js API runtime environment within the AWS Lightsail infrastructure.|6| 
| Engineers \[number of assigned headcount\] |\- Configure secure credential management via AWS Secrets Manager and establish logging mechanisms using Amazon CloudWatch. | 6 |
| Other (Please specify) | \- Perform rigorous post-deployment system verification and integration testing.|0|



\* Note: Refer to section “activities & deliverables” for the list of project phases

| Project Phase | Solution Architects | Engineers | Other  (Please specify) | Total Hours |
| :---: | :---: | :---: | :---: | :---: |
| S3 \+ CloudFront | 1 | 2 |  | 3 |
| Lightsail API \+ DB | 1 | 4 |  | 4 |
| Cognito | 1 | 2 |  | 5 |
| Logging & Monitoring | 1 | 1 |  | 3 |
| Total Hours | 4 | 12 |  | 15 |
| Total Cost |  |  |  |  |

Cost Contribution distribution between Partner, Customer, AWS:

| Party | Contribution (USD) | % Contribution of Total |
| :---- | :---- | :---- |
| Customer |  |  |
| Partner |  |  |
| AWS |  |  |

# **ACCEPTANCE** 

Project is complete when: 9/12/2025

Website runs stably on real domain.

API fully connects to DB.

Upload product images works.

CloudWatch log & Cognito login works.

Accepted by customer/stakeholder.


## File TEMPLETE DOCX: [DOWLOAD Proposal (DOCX)](https://drive.google.com/drive/folders/1TLXOU4XDvSqv1hfWYhXhWilc5G53iN2H?usp=sharing)
 