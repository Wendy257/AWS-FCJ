---
title: "Summary & Clean up"
weight: 56
chapter: false
pre: " <b> 5.6. </b> "
---

## 5.6.1 Summary

We planned and scoped AWS Jewelry Web as a secure, cost-aware e-commerce stack:

- **Frontend**: React on S3 + CloudFront with ACM TLS and Route 53 domain.
- **Backend**: Node.js API on Lightsail; DB on Lightsail (MySQL/Postgres); Secrets Manager for DB password/bucket name.
- **Identity**: Cognito User Pool for signup/login and API token verification.
- **Media**: S3 private bucket; uploads via presigned PUT; CloudFront reads objects.
- **Observability**: CloudWatch structured logs for API + business events; optional export to S3/Athena for deeper analysis.

Success criteria: <2s page load via CDN, stable API under expected traffic, secure DB operations, stable Cognito auth, secure uploads, and complete API logging.

---

## 5.6.2 Key takeaways

- Keep secrets out of code: Secrets Manager + IAM least privilege on the API role.
- Performance from the edge: CloudFront + S3 + ACM; minimal backend hops for static content.
- Reliability first: log everything meaningful (auth, uploads, CRUD, product events) and alarm on error/latency thresholds.
- Cost focus: Lightsail for predictable spend; CloudWatch retention tuned; optional analytics only when needed.

---

## 5.6.3 Clean-up checklist

- S3 + CloudFront: tear down the distribution and bucket objects if the site is decommissioned.
- Cognito: delete the User Pool if no longer used.
- Lightsail: snapshot or terminate API/DB instances; release static IPs.
- Secrets Manager: remove `DB_PASSWORD` and `APP_CONFIG` secrets.
- CloudWatch: delete log groups/alarms; disable any export tasks.
- Route 53/ACM: remove records/certs no longer in use.