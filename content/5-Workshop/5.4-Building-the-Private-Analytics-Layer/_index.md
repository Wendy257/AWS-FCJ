---
title: "Building the Private Analytics Layer"
weight: 54
chapter: false
pre: " <b> 5.4. </b> "
---

## 5.4.1 Minimal analytics approach (fits scope/cost)

- **Source of truth**: CloudWatch structured API logs (events and request metrics).
- **Storage**: Keep logs in CloudWatch with sensible retention (e.g., 30–90 days).
- **Export path**: Enable CloudWatch → S3 export for ad-hoc analysis when needed.
- **Query**: Use CloudWatch Log Insights for quick views; use Athena on exported S3 logs for deeper analysis.

This avoids a separate DWH while still giving behavior and reliability visibility.

---

## 5.4.2 Optional light ETL (if needed)

- Set an **export task** (daily) from CloudWatch Logs to an S3 bucket (`logs/<yyyy>/<mm>/<dd>/`).
- In Athena, create an external table over the exported JSON; partition by date.
- Derive a single `events` view with key fields: `eventName`, `userId`, `sessionId`, `productId`, `statusCode`, `latencyMs`, `timestamp`.
- Keep costs low by:
  - Compressing exports (GZIP).
  - Dropping high-cardinality noise fields.
  - Restricting retention in S3 (lifecycle to Glacier/expire after X days).

---

## 5.4.3 Security and access

- **No public analytics endpoints**: exports stay in a private S3 bucket.
- **IAM least privilege**:
  - API instance role: Secrets Manager read, CloudWatch logs write, S3 put for exports (if used).
  - CloudFront only reads from the image bucket; uploads remain private via presigned PUT.
- **Secrets**: DB password + bucket name stored in Secrets Manager; rotate via runbook.
- **Encryption**: HTTPS everywhere; S3 bucket encryption enabled; export bucket private.

---

## 5.4.4 Operational checks

- Alarms on API 5xx rate, p95 latency, auth failures.
- Periodic validation:
  - CloudWatch export jobs succeed (if enabled).
  - Cognito JWT verification still passes after config changes.
  - Presigned upload permissions stay scoped (content-type, key prefix).

---

## 5.4.5 Future extension (beyond current scope)

- Introduce a small RDS read-replica or a dedicated analytics DB if queries grow.
- Add Lambda transform to normalize events into an `events` table (daily batch).
- Add BI tooling (QuickSight) on top of Athena or the analytics DB for richer dashboards.