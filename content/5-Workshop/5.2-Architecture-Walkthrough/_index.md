---
title: "Architecture Walkthrough"
weight: 52
chapter: false
pre: " <b> 5.2. </b> "
---

![Architecture](/images/architecture.drawio.png)
<p align="center"><em>Figure: AWS Jewelry Web deployment (adapted from workshop layout).</em></p>

---

## 5.2.1 User & Frontend

- **Browser**: Shoppers browse catalog, product detail, cart, checkout.
- **CloudFront + S3 (static hosting)**: React build served globally with ACM TLS; Route 53 maps domain. CloudFront has origin access to S3.
- **Cognito**: User Pool issues JWTs for signup/login; React uses tokens to call the API.

Why it matters: Fast global delivery (<2s target), standard SSL, and auth tokens for protected API calls.

---

## 5.2.2 API & Data

- **Lightsail API (Node.js)**: CRUD for products, cart, and image upload orchestration. Runs with IAM role permission to read Secrets Manager.
- **Lightsail DB (MySQL/Postgres)**: Stores catalog, users, cart/orders. Private to the API instance.
- **Secrets Manager**: Holds DB password and bucket name (`DB_PASSWORD`, `APP_CONFIG`). API fetches at startup/first use.
- **CloudWatch Logs**: Captures API access/error logs; base for operational and light analytics queries.
- **S3 (media bucket)**: Private bucket for product images. Upload flow uses presigned URLs from the API; CloudFront reads images.

Security highlights: HTTPS everywhere, CloudFront-to-S3 access controlled, DB credentials never hardcoded, IAM least privilege on the API role.

---

## 5.2.3 Identity & Access

- **Amazon Cognito**: Signup/login, token issuance; API validates JWT for protected routes.
- **Route 53 + ACM**: Domain + TLS cert for CloudFront; optional custom domain for API if exposed.
- **IAM**: API instance role limited to Secrets Manager + CloudWatch; S3 bucket policy grants CloudFront read access; uploads only via presigned PUT.

---

## 5.2.4 Operations & Observability

- **CloudWatch**: API logs/metrics, alarms for error rates/latency.
- **Backup/DR (lightweight)**: DB snapshots via Lightsail schedule; S3 versioning optional.
- **Cost posture**: Lightsail for predictable monthly cost; S3/CF/Cognito/SM/CW minimal at stated traffic.

---

## 5.2.5 Delivery Phases (6–12 weeks from proposal)

1) **Assessment (Week 1)**: Requirements, architecture diagram, DB schema, secrets list.  
2) **Base infra (Week 2)**: S3 hosting, CloudFront+ACM, Route 53, Lightsail API/DB, Cognito setup, Secrets Manager entries, CloudWatch logging.  
3) **Backend (Week 3)**: Secrets fetch, presigned upload, CRUD, Cognito token verification, CloudWatch logs.  
4) **Frontend (Week 4)**: React shop UI, Cognito login UI, upload UI, API integration, deploy to S3+CF.  
5) **Testing & go-live (Week 5)**: FE↔API↔S3↔DB integration, security tests (IAM/SM), E2E testing.  
6) **Handover (Week 6)**: Secrets update runbook, ownership transfer, operations guide.