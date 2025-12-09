---
title : "Objectives & Scope"
weight : 51 
chapter : false
pre : " <b> 5.1. </b> "
---

### Business Context

The AWS Jewelry Web project delivers a jewelry e-commerce site with:

- React SPA hosted on **S3 + CloudFront** (ACM-managed HTTPS, Route 53 domain).
- **.net API** on **AWS Lightsail** serving product/catalog/cart operations.
- **Lightsail MySQL/Postgres** for transactional data.
- **Amazon Cognito** for signup/login and API authorization.
- **Amazon S3** for product image storage via presigned uploads.
- **CloudWatch** for API logging and **Secrets Manager** for DB password + bucket config.

Traffic assumption: <100k requests/month with no advanced autoscaling required. Focus is reliability, secure media handling, and fast global delivery.

---

### Objectives (Workshop Outcomes)

- **Architecture clarity**: Explain how S3+CloudFront, Lightsail API/DB, Cognito, and Secrets Manager fit together for a small-but-secure e-commerce stack.
- **Hands-on delivery**: Stand up CDN-hosted frontend, API tied to Secrets Manager, Cognito auth, secure image uploads to S3, and end-to-end logging in CloudWatch.
- **Success criteria (from proposal)**:
  - Page load <2s globally (CloudFront + S3).
  - Stable Lightsail API under expected load.
  - Secure DB operations with fast queries.
  - Cognito signup/login stable.
  - Image uploads secure to S3.
  - API logging visible in CloudWatch.

---

### Scope

- Core catalog CRUD, shopping cart basics.
- Cognito-based authentication and token verification on API.
- Presigned S3 uploads for product images.
- CDN + HTTPS via CloudFront/ACM; Route 53 domain mapping.
- CloudWatch logging/metrics; Secrets Manager for DB password + bucket name.
- Timeline guidance: 6–12 weeks across assessment → infra → backend → frontend → testing → handover.

---

### Out of Scope (per proposal)

- AI/ML features, advanced e-commerce flows, complex image processing.
- Multi-region/DR, complex admin portals, third-party integrations.
- Advanced autoscaling or sophisticated CI/CD beyond basic deploys.