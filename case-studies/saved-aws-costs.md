---
layout: page
title: "AWS Cost Optimization: How I Saved $714/Month in AWS Costs in Just 8 Hours"
subtitle: A Data Engineer's Guide to Turning 8 Hours into $8,568 Annual AWS Savings
share-title: "Pedro Marques Data Consulting | AWS Cost Optimization: How I Saved $714/Month in AWS Costs in Just 8 Hours"
description: "Learn how a Data Engineer reduced AWS costs by $8,568 Annual through Secret Manager optimization and resource management. Real-world case study with actionable steps for CTOs and Engineering Leaders."

---

<!--
Tags:
#AWSOptimization #CloudCosts #DataEngineering #DevOps #CloudArchitecture
-->

[Pedro Marques](https://www.linkedin.com/in/paguasmar/)

## TL;DR

**Problem**: AWS costs spiked 55% due to idle resources and inefficient API calls.
**Solution**:
- Eliminated unused test environments
- Implemented Secret Manager caching
- 8 hours of engineering work ($336)

**Impact**:
- $714 monthly savings ($8,568/year)
- 99.67% reduction in API calls
- 10.4% faster system performance
- 2,450% ROI

---

"Have you seen the staging account costs?"

That Slack message from my manager, accompanied by an article about cloud cost consciousness, kicked off what would become an interesting journey into AWS cost optimization. What started as a routine investigation would lead to not just significant cost savings ($714/month), but an unexpected performance breakthrough in our data platform.

The numbers were compelling: a 2,450% ROI achieved in just 8 hours of focused work, transforming $336 of engineering time into $8,568 of annual savings. But the real story isn't in the numbers - it's in how I got there.

![A popular meme showing a cartoon dog sitting at a table drinking coffee, surrounded by flames in a burning room. The dog is wearing a small hat and has a calm, slightly forced smile. Above the scene is text reading 'AWS costs increasing 55%' and below is the dog's famous quote 'This is fine.' The meme ironically portrays maintaining composure while ignoring an obviously dangerous situation, representing how teams sometimes overlook escalating cloud costs until they become a serious problem.](/imgs/case-studies/saved-aws-costs/meme_this_is_fine.jpg)
## AWS Cost Optimization Strategy

The numbers were concerning: our staging AWS bill had spiked 55%. As the only engineer tasked with investigating this, I faced a delicate balance: find quick savings without disrupting our 37 analysts who relied on these systems daily.

The [AWS Cost Explorer](https://aws.amazon.com/blogs/aws-cloud-financial-management/tips-and-tricks-for-exploring-your-data-in-aws-cost-explorer-part-2/) on the staging account revealed three concerning patterns:

![Stacked bar chart showing AWS costs breakdown by service from October to December. Total costs increased from $1,062 in October to $1,558 in December, a 55% increase. The chart shows four services: Idle MWAA (green), Idle SFTP (red), SecretManager API Requests (blue), and Other costs (grey). December shows significant increases in idle resources, with MWAA costing $130, SFTP $172, and SecretManager API requests spiking to $90, while base costs ('Other') remained relatively stable around $1,000 across all months.](/imgs/case-studies/saved-aws-costs/aws_services_cost_dist_by_month.png)

The chart showed partial-month costs:

- Idle [MWAA environment](https://aws.amazon.com/managed-workflows-for-apache-airflow/): $130
- Idle [SFTP server](https://aws.amazon.com/aws-transfer-family/): $172
- [Secret Manager](https://aws.amazon.com/secrets-manager/) API calls: $90

However, projecting these costs forward painted a more concerning picture. If left unchecked, our monthly costs would have been:

- Idle MWAA (staging): $215/month
- Idle SFTP (staging): $223/month
- Secret Manager API Requests (staging): $138/month
- Secret Manager API Requests (production): $138/month

Total projected monthly waste: $714

## The Two-Part Cost-Cutting Strategy

### 1. Quick Wins: Eliminating Idle Resources (61% of Savings)

The quick wins were clear:

- **Amazon MWAA Test Environment ($215/month)**: A development environment cloned directly from the console, sitting idle for over a month. After confirming with the development team that it was no longer needed, I terminated it. [Learn more about MWAA pricing and best practices](https://docs.aws.amazon.com/mwaa/latest/userguide/best-practices.html)
- **AWS Transfer Family SFTP Server ($223/month)**: A testing SFTP server with zero bytes transferred for over a month. Instead of letting it run continuously, I implemented a "create-test-delete" workflow with the development team. [AWS Transfer Family pricing details](https://aws.amazon.com/aws-transfer-family/pricing/)

But the real challenge lay in the Secret Manager API calls.
### 2. Secret Manager Cost Reduction Techniques (39% of Savings)

The next step tackled a hidden cost problem: the systems were making too many AWS Secrets Manager API calls, which added up to $276/month ($138 in staging and $138 in production). [See AWS Secrets Manager pricing](https://aws.amazon.com/secrets-manager/pricing/)

The root cause? Airflow's default behavior:

- Airflow parses DAG code every 30 seconds.
- Each parse triggers multiple Secret Manager calls.
- Result: 38,735 API calls per hour.

After several failed attempts, I discovered a relatively new feature: `secrets.use_cache`. Released just two years ago, it wasn't widely documented, but it looked promising. [Official Airflow configuration documentation](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html#use-cache)

```terraform
airflow_configuration_options = {
	"secrets.use_cache" = true
}
```

This reduced the client API calls to just 128 per hour - a 99.67% reduction.

However, implementing it wasn't straightforward. The implementation hit a roadblock when I found version conflicts in our [Terraform](https://www.terraform.io/) code. I had three options:

1. Spend two hours upgrading Terraform, but risk breaking the data platform used by 37 analysts.
2. Wait three days for the Tech Lead to return from vacation.
3. Use an older, stable version to fix the problem right away.

Since the Tech Lead owned the platform code and was on vacation, I played it safe and used the older version. Yes, I could have done the upgrade in two hours, but breaking someone else's system while they're away didn't feel right.

This wasn't just about the technical fix - it was about respecting team ownership and keeping our systems reliable. Sometimes the best technical solution isn't the best team solution.

Looking to optimize your AWS infrastructure costs? Book a free consultation to discuss your specific needs.

[Contact Us](/contact){: .btn .btn-primary }

## Cloud Cost Optimization Results

### Financial Results

- Immediate monthly savings projection: $714
    - Staging environment: $576 ($215 + $223 + $138)
    - Production environment: $138
- Annual cost avoidance: $8,568
- Implementation cost: $336 (8 hours)
- ROI: 2,450%

### Unexpected Performance Improvements

Beyond cost savings, my optimization delivered performance benefits:

- 10.4% reduction in [DAG parsing time](https://medium.com/@weih1214/deep-into-airflow-dag-processing-958c24c13dcb) (from 0.471 to 0.4222 seconds)
- Improved Airflow scheduler efficiency
- Better scalability for future workflows

## Key Takeaways: AWS Infrastructure Optimization

1. Early intervention is crucial - catching issues mid-month allowed the client to prevent full-month charges
2. Direct communication with resource owners before any changes
3. Small, focused engineering efforts can deliver outsized ROI when properly targeted.
4. The best optimizations improve both costs and performance

How do you balance quick cost-saving wins with long-term architectural improvements in your organization?