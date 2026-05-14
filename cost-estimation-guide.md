# Cost Estimation Guide

Back-of-the-envelope calculations to estimate infrastructure costs before you build.

## Cloud Pricing Models

Most cloud services charge based on one or more of:

| Dimension | What it means | Example |
|---|---|---|
| Compute | Time your server runs | $0.01/hour per VM |
| Storage | How much data you store | $0.023/GB/month (S3) |
| Data transfer | Data leaving the cloud (egress) | $0.09/GB |
| Requests | Number of API calls | $0.0004 per 1,000 requests |
| Database | Instance size + storage | $0.034/hour (small RDS) |

**Rule of thumb:** Egress (outbound data) is often the hidden cost. Inbound is usually free.

## Back-of-the-Envelope Calculation

### Example: Image sharing app

**Assumptions:**
- 10,000 users
- Each uploads 5 photos/month
- Average photo size: 2 MB
- Each photo viewed 20 times

**Storage cost:**
```
Photos/month = 10,000 × 5 = 50,000
Storage/month = 50,000 × 2 MB = 100 GB
Cost (S3) = 100 GB × $0.023 = $2.30/month
```

**Data transfer cost:**
```
Views/month = 50,000 × 20 = 1,000,000
Transfer/month = 1,000,000 × 2 MB = 2,000 GB
Cost = 2,000 GB × $0.09 = $180/month
```

**Compute (small instance):**
```
1 × t3.medium = ~$30/month
```

**Total estimate: ~$212/month**

## Common Pricing References (AWS)

| Service | Rough cost | Notes |
|---|---|---|
| EC2 t3.micro | ~$7.50/month | 1 vCPU, 1 GB RAM |
| EC2 t3.medium | ~$30/month | 2 vCPU, 4 GB RAM |
| RDS (small Postgres) | ~$25/month | Includes storage |
| S3 storage | $0.023/GB/month | Standard tier |
| CloudFront (CDN) | $0.085/GB | First 10 TB |
| Lambda | $0.20 per 1M requests | + compute time |
| SQS | $0.40 per 1M requests | |
| DynamoDB | $1.25 per 25 RCUs | Pay-per-request also available |

## Cost Saving Tips

1. **Start small** — use the smallest instance that works, scale up when needed
2. **Use free tiers** — AWS, GCP, and Azure all offer generous free tiers for new accounts
3. **Reserved instances** — up to 70% cheaper if you can commit to 1-3 years
4. **Auto-scaling** — don't pay for idle capacity
5. **Use managed services** — often cheaper than running your own (when you factor in your time)
6. **Monitor usage** — set billing alerts early, check cost dashboards weekly
7. **S3 lifecycle policies** — move old data to cheaper storage tiers automatically

## Quick Estimation Template

For any project, estimate these numbers:

| Question | Your estimate |
|---|---|
| How many users? | |
| How many requests per user per day? | |
| Average request/response size? | |
| How much data stored per user? | |
| Read vs write ratio? | |
| Peak traffic multiplier? | × |

Then multiply: `users × actions × data_size × cost_per_unit = monthly cost`
