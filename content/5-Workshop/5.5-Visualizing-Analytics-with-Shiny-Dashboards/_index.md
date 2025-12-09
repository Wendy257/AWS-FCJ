---
title: "Visualizing Analytics with Shiny Dashboards"
weight: 55
chapter: false
pre: " <b> 5.5. </b> "
---

## 5.5.1 Pragmatic visualization path

Given the project scope, start with what is already available:

- **CloudWatch Log Insights**: saved queries for latency, 5xx/4xx, auth failures, product event counts (`product_view`, `add_to_cart`, `checkout_*`).
- **Athena (optional, if exports enabled)**: run SQL over exported JSON logs to build simple funnels and product popularity views.
- **QuickSight (optional)**: lightweight dashboards over Athena; keep SPICE size small.

---

## 5.5.2 Suggested CloudWatch queries

- **Reliability**: error rate, p95 latency by route; top failing endpoints; auth failures.
- **Behavior**: count by `eventName`; product popularity (`productId`, `category`); checkout completion rate.
- **Uploads**: success vs failure for presigned PUT (filter by `statusCode` and `path`).

Save queries and pin to a CloudWatch dashboard for quick ops visibility.

---

## 5.5.3 If using Athena/QuickSight

1) Export CloudWatch logs to S3 daily.  
2) Create an external table over the JSON; partition by date.  
3) Example metrics:
   - Events over time, event mix by userLoginState.
   - Product views vs add-to-cart vs checkout_complete.
   - Error rate and p95 latency by route.
4) In QuickSight, build 3 visuals:
   - KPI tiles (total events, error rate, p95 latency).
   - Funnel (product_view → add_to_cart → checkout_start → checkout_complete).
   - Top products (views and carts).

---

## 5.5.4 Access and security

- Keep the export bucket private; limit Athena/QuickSight access to project admins.
- Use IAM permission boundaries: read-only access for analysts, write for ops only where needed.
- No public dashboard endpoints; share via AWS console access or exported PDF if required.