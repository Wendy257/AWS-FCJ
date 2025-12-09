---
title: "Project Proposal"
date: 2025-11-10
weight: 1
chapter: false
pre: " <b> 2. </b> "
---
# **Jewelry E-Commerce Platform**  
## **Cloud-Based Online Sales System Using React, .NET, and MySQL on AWS Lightsail**  

---

## **1. Project Overview**

Jewelry E-Commerce Platform is a modern online retail system built on a cloud infrastructure, designed for small and medium-sized jewelry businesses. The project aims to help these enterprises transition from traditional sales models to a secure, flexible, and automated digital environment.  

The platform integrates a ReactJS front-end, a .NET Core backend hosted on Amazon Lightsail, and a MySQL database for managing products, users, and orders efficiently.  

Static assets such as product images and web content are stored on Amazon S3 and globally distributed through Amazon CloudFront, ensuring optimal speed and security. Amazon Cognito handles user authentication, while Amazon CloudWatch, AWS CloudTrail, and Lightsail Snapshots provide monitoring, auditing, and disaster recovery.  

This project delivers a cost-effective, easy-to-operate, and scalable e-commerce solution tailored for small and medium jewelry businesses.  

### **Project Objectives**
- Develop a responsive, user-friendly jewelry e-commerce website that works seamlessly across devices.  
- Centralize management of products, inventory, and orders.  
- Ensure ≥99.9% uptime through automated backup and recovery.  
- Maintain infrastructure costs below USD 65/month using Lightsail and AWS Free Tier resources.  

### **Business Value**
Small jewelry shops often face challenges related to high infrastructure costs and limited technical expertise. This solution helps:  
- Reduce operational costs with predictable Lightsail pricing.  
- Automate repetitive tasks, improving efficiency.  
- Enhance brand trust through system stability and robust data protection.  

---

## **2. Problem Analysis**

### **2.1 Current Situation**

The jewelry retail market is rapidly shifting to online channels, driven by consumer demand for personalization, transparency, and high-quality digital experiences — especially among younger generations. However, most existing e-commerce systems face critical limitations:

- Poor user experience: Slow page loading, outdated design, and lack of interactive features such as AR try-on.  
- Lack of trust: Customers hesitate to purchase high-value items (gold, diamonds) due to concerns over data security and authenticity.  
- Insecure, outdated infrastructure: Many legacy systems store passwords or customer data in plain text, making them vulnerable to attacks and limiting scalability.  

---

### **2.2 Key Challenges**

- **Speed and reliability:**  
  Jewelry relies heavily on high-resolution imagery and interactive content (e.g., 360° views, AR try-on). Without a global CDN, loading such content becomes slow and unreliable, resulting in high cart abandonment rates.  

- **Security and fraud risks:**  
  E-commerce platforms are prime targets for cyberattacks. Without a Web Application Firewall (WAF), systems are exposed to SQL injection, cross-site scripting, and data breaches. Storing credentials directly in code poses severe risks of unauthorized access.  

- **Data loss and recovery issues:**  
  Transactions and inventory records must be 100% accurate. Local storage risks permanent data loss if hardware fails. Without durable storage like Amazon S3, essential data such as invoices and product images could become irretrievable.  

- **Limited monitoring and control:**  
  Without centralized monitoring tools such as CloudWatch, operators can only detect issues after customers report them, increasing MTTR (Mean Time to Recovery) and damaging user trust.  

---

### **2.3 Stakeholder Impact**

| **Stakeholder** | **Key Impact** |
|------------------|----------------|
| Customers | Gain confidence through fast, transparent, and secure online shopping. |
| Operations Team | Easier system monitoring, with automated backup and recovery processes. |
| Developers | Faster development through modular architecture with API Gateway and Lightsail. |
| Business Owners | Continuous business operations, reduced data loss risk, and stronger competitive advantage. |

---

### **2.4 Business Consequences**

- **Revenue loss:** Slow performance and poor UX reduce conversions and increase cart abandonment.  
- **Reputation and compliance risks:** Data breaches (from missing WAF or Secrets Manager) can result in heavy penalties and long-term brand damage — especially in the luxury goods sector.  
- **Higher operational costs:** Lack of automated monitoring and backup increases manpower and recovery time.  
- **Limited scalability:** Legacy systems fail to adapt to fast market growth, causing missed business opportunities.  

---

## **3. Solution Architecture**

![Platform Architecture Diagram](/images/2-Proposal/AWS_Architecture.jpg)

### **3.1 Architecture Overview**
The proposed architecture is a high-performance e-commerce platform deployed on the AWS Cloud (Singapore Region) using a separated SPA/microservices design. This enables dynamic processing and high scalability:

- **Static Frontend Delivery:** React frontend files (HTML, CSS, JS) are stored on S3 and served via CloudFront CDN for global low-latency delivery.  
- **Dynamic Backend Processing:** Business logic and data (catalogs, carts, orders) are handled through a .NET API hosted on Lightsail, exposed securely via API Gateway.  

---

### **3.2 AWS Services Used**

| **Category** | **AWS Service** | **Primary Function** |
|---------------|----------------|----------------------|
| Networking & Edge | Route 53, CloudFront, WAF, ACM | DNS routing, CDN delivery, web protection, SSL management |
| Compute & API | API Gateway, Lightsail | API endpoint management and backend application hosting |
| Identity & Access | Cognito, Secrets Manager | Authentication/authorization and secure credential management |
| Storage & Database | S3, Lightsail MySQL | Static asset storage and relational database for transactional data |
| Resilience & Backup | AWS Backup, S3 Versioning, Glacier, Lightsail Snapshots | Automated backups, archiving, and data version control |
| Monitoring & Audit | CloudWatch, CloudTrail | Real-time performance tracking and API activity auditing |

---

### **3.3 Component Design**

- **Presentation Layer (Frontend):** Static React site hosted on S3, distributed via CloudFront. AWS WAF provides edge-level protection from common exploits.  
- **Application Layer (Backend):** API Gateway validates Cognito tokens, manages API requests, and enforces throttling. Lightsail Instance (Ubuntu) runs the .NET Core API for handling business logic (orders, products, payments).  
- **Data Layer:** MySQL Database on Lightsail manages all relational data; credentials stored securely in Secrets Manager. Amazon S3 stores user uploads, product images, and other static assets.  

---

### **3.4 Security Architecture**

- **Edge Protection:** WAF filters malicious requests; ACM enforces HTTPS encryption.  
- **User Authentication:** All login and registration are handled by Cognito with secure token validation.  
- **Infrastructure Security:** Secrets Manager ensures sensitive credentials are never stored in plain text.  
- **Auditing:** CloudTrail records every API call for compliance and traceability.  

---

### **3.5 Scalability and Observability**

- Global scalability via CloudFront caching and distribution.  
- Unlimited storage growth on S3 for media assets.  
- Resource monitoring through CloudWatch metrics, enabling proactive scaling decisions.  

---

## **4. Technical Implementation Plan**

| **Phase** | **Duration** | **Goal** | **Deliverables** | **Success Criteria** |
|------------|---------------|-----------|------------------|----------------------|
| 1. Infrastructure Setup | Week 1–2 | Configure AWS environment | S3, CloudFront, Cognito, Route 53, SSL | Stable environment |
| 2. Backend Development | Week 3–5 | Build .NET API & MySQL schema | CRUD functions, database | Backend operational |
| 3. Frontend Integration | Week 6–7 | Connect React SPA to API | Login, UI pages | UI functional |
| 4. Image Upload Module | Week 8–9 | Enable S3 upload | Successful upload test | Pass |
| 5. Monitoring & Backup | Week 10–11 | Configure CloudWatch & Snapshots | Alerts and auto-backup | System stable |
| 6. Testing & Deployment | Week 12–14 | QA and final release | Demo and documentation | Full system stable |

---

## **5. Roadmap & Key Milestones**

The project will run for 14 weeks (September–December 2025), divided into six Agile–Scrum sprints.

| **Sprint** | **Deliverable** | **Success Criteria** |
|-------------|-----------------|----------------------|
| Sprint 1 – Foundation | AWS setup (Lightsail, S3, CloudFront, Cognito, Route53) | React website served via HTTPS; Cognito login test successful |
| Sprint 2 – Backend & DB | .NET API and MySQL schema | CRUD operations working correctly |
| Sprint 3 – Frontend | React pages integrated with API | Product and cart display functioning |
| Sprint 4 – Media | S3 upload integration | Image delivery via CDN |
| Sprint 5 – Checkout & Payment | Complete order flow | Checkout and order confirmation functional |
| Sprint 6 – Testing & Monitoring | Fully functional system | CloudWatch logs and backups verified |

---

## **6. Estimated Budget**

| **Service** | **Description** | **Estimated Monthly Cost (USD)** | **Notes** |
|--------------|----------------|----------------------------------|------------|
| Lightsail Instance (.NET API) | 2–4 vCPU, 4–8 GB RAM | $10–$40 | Recommended ≥$20 plan |
| Lightsail MySQL Database | 20–50 GB managed DB | $15–$50 | Better kept separate from instance |
| Amazon S3 | Store static files and images | $1–$5 | Includes request fees |
| Amazon CloudFront | CDN distribution | $1–$30 | First 1TB free/month |
| Route 53 + ACM | Domain and SSL | $1–$4 | ACM free; domain ~$12/year |
| Amazon Cognito | User management | $0–$10 | First 10k users free |
| CloudWatch + CloudTrail | Monitoring and logging | $1–$10 | Depends on log volume |
| Backups | Snapshots and versioning | $1–$10 | Weekly auto-backups recommended |

**Estimated total:** $30–$160/month (≈ $90–$480 for 3 months)

---

### **Cost Optimization Tips**

1. Use AWS Free Tier for Lightsail, S3, CloudFront, and Cognito.  
2. Deploy in ap-southeast-1 (Singapore) for minimal latency.  
3. Apply S3 Lifecycle Policies to move old files to Glacier Deep Archive.  
4. Enable billing alerts with AWS Cost Explorer and Budgets.  
5. Schedule weekly snapshots and enable MFA Delete on S3.  

---

## **7. Risk Assessment**

| **Risk ID** | **Description** | **Severity** | **Impact** |
|--------------|----------------|--------------|-------------|
| R1 | Lightsail instance failure | Medium | Temporary downtime |
| R2 | Database corruption | High | Loss of transactional data |
| R3 | Credential leakage | Medium | Unauthorized access risk |
| R4 | Traffic surge overload | Medium | Slow or unresponsive website |

---

### **7.1 Mitigation Strategies**

- R1: Daily Lightsail snapshots and detailed recovery procedures.  
- R2: Automated DB backups exported to S3 for redundancy.  
- R3: Secrets Manager enforced with key rotation.  
- R4: Optimize CloudFront caching and scale Lightsail when traffic spikes.  

---

### **7.2 Contingency Plan**

- System Recovery: Restore instance from latest snapshot within 4 hours.  
- Data Restoration: Recover MySQL backups stored in S3.  
- Continuity: Serve a maintenance page via S3 + CloudFront during downtime.  
- Security Incident: Rotate keys and investigate with CloudTrail logs.  

---

### **7.3 Risk Monitoring Plan**

- Operational Monitoring: CloudWatch alarms for CPU/network thresholds.  
- Security Review: Weekly CloudTrail audits for unusual API activities.  
- Quarterly Risk Review: Reassess risk matrix after major updates.  

---

## **8. Expected Outcomes**

### **8.1 Success Metrics (KPIs)**

**Technical KPIs**
- Frontend latency < 200ms (via CloudFront)  
- API response < 350ms (via API Gateway + Lightsail)  
- 99.9% uptime with automated recovery  
- 70%+ requests served from CDN cache  
- Zero major security incidents (Cognito + WAF)  
- RTO < 30 minutes, RPO = 0 for data protection  

**Business KPIs**
- 20–30% increase in conversions  
- 15–25% customer retention improvement  
- 25–40% infrastructure cost reduction  
- Lower transaction costs aligned with AWS value efficiency  

---

### **8.2 Short-Term Benefits (0–6 Months)**

- 40–70% faster page load, lower bounce rate  
- Reduced backend load with CDN caching  
- Stronger authentication via Cognito  
- Automated alerts and backups with CloudWatch  
- Faster feature deployment thanks to decoupled architecture  

---

### **8.3 Mid-Term Benefits (6–18 Months)**

- Lower storage cost via S3 → Glacier lifecycle  
- Stable API under high traffic  
- Cost forecasting through AWS Cost Explorer  
- Continuous performance tuning with CloudWatch dashboards  
- Reduced maintenance workload for developers  

---

### **8.4 Long-Term Value (18+ Months)**

- Fully scalable cloud-native platform, ready for mobile or marketplace expansion  
- AI/ML readiness for personalized jewelry recommendations  
- 80–90% storage cost reduction via Glacier archiving  
- Enterprise-grade security and compliance (IAM, WAF, CloudTrail)  
- Global reach with CloudFront Edge Network  
- Sustainable, resilient operations for long-term growth  

---

### **8.5 User Experience Enhancements**

- Faster image loading and product browsing  
- Smooth login and order tracking via Cognito  
- Reduced lag and downtime during peak hours  
- Increased customer trust through AWS-grade reliability  

---

### **8.6 Strategic Capabilities Gained**

- Cloud-native architecture, ready for microservices evolution  
- FinOps maturity with detailed cost tracking  
- Operational excellence via centralized monitoring and auditing  
- Future-ready infrastructure expandable to ECS, EKS, or RDS  
- High-level security compliance using IAM, Cognito, WAF, Secrets Manager  
- Solid foundation for data analytics and AI integration