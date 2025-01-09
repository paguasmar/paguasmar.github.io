---
layout: page
title: How I Saved 103 Hours of Analyst Time per Month with a Simple Automation Solution
subtitle: Transforming Manual Query Runs into an Efficient Scheduled System
share-title: Pedro Marques Data Consulting | How I Saved 103 Hours of Analyst Time per Month
---

[Pedro Marques](https://www.linkedin.com/in/paguasmar/)

When I found out that Tripadvisor analysts were spending over 100 hours monthly on repetitive query tasks, I knew we had to find a better way. Here's how a month's project transformed my client team's efficiency and delivered an impressive 1095% ROI.

Found this useful so far? Follow me on Medium for more practical insights on technical leadership and automation solutions.

## The Problem: Manual Query Hell

Picture this: 37 analysts, each running multiple queries daily, spending 10 minutes per query on manual tasks. They would:

1. Write and execute SQL queries in Snowflake.
2. Export results to CSV.
3. Upload files to internal tools.

The impact? Skilled analysts were spending 103 hours each month on routine tasks instead of working on important marketing campaigns that could generate hundreds of millions of dollars in revenue.

![Pedro Águas Marques profile picture](/imgs/case-studies/scheduled-query-tool/drake.png){:height="280px"}

## The Solution: Scheduled Query Tool

![Pedro Águas Marques profile picture](/imgs/case-studies/scheduled-query-tool/architecture_query_tool.png)

I built a solution using:

- AWS, as the team already had most of its services with this cloud provider.
- Amazon API Gateway instead of ALB, because the API is expected to receive less than 1,000 requests per month.
- Amazon Lambda, because the function should not run longer than the Lambda timeout limit (15 minutes).
- Snowflake for data retrieval, which was an obvious choice since the client data was already stored there.
- Airflow, because the team was already using it and I had 3 years of experience with it.
- The existing internal tools UI, so the new Scheduled Query Tool would be just another page within it, allowing me to reuse all the security features of the main internal UI.

Key Features:

- Automate query runs.
- Queries run on multiple days of the week.
- Detailed execution logging in Grafana.
- Add another page for it in the internal tools UI.

## The Implementation Journey

The project started with a research phase. I identified:

- 34 important weekly queries.
- Query manual execution took on average 10 minutes.
- Mandatory features through analyst workshops.
- Integration requirements with existing systems.

I divided the project into 2 parts to get quick feedback:

1. Run query instantly feature.
2. Run query scheduled functionality.

### Overcoming Integration Challenges

What looked like an easy task turned out to be difficult. The original plan was simple: upload CSV files to Amazon S3 so another system could use them. However, I found out that there was a problem because the other system needed extra information additional metadata with each file - details I wasn't aware of when I started.

Because I needed to finish quickly, I first suggested a solution that would take three weeks. The client didn't like this idea because they thought I was overengineering. Things got better when the client asked a basic question about how big the files could be. After talking with the analysts, I found out that limiting files to 100MB would work well for everyone. This simple solution helped me avoid making big changes to the system.

Dealing with similar integration challenges? Contact us today!

[Contact Us](/contact){: .btn .btn-primary }
### Building Trust Through Collaboration

I had more problems with the other system. When queries started failing, I didn't blame the other system owner. Instead, I showed them clear information about what went wrong and asked if we could work together to fix it. This helped us work well as a team instead of having arguments.
## The Results: Beyond Time Savings

The numbers speak for themselves:

- 103 hours saved monthly.
- $80,340 annual cost savings.
- 1095% ROI.
- Implementation cost: $6,720.

But the real victory was in the analysts' reactions: 
"WOW this is awesome! Thank you!" - Analyst 
"This looks great - thanks" - Analyst's Manager

## Key Learnings

- Testing with connected systems early in the process helps avoid unexpected problems later.
- Working with analysts helped create an easy-to-use interface.
- Making good use of existing security systems and tools.

## The Takeaway

This project taught me valuable lessons about finding the right balance between technical quality and practical solutions. The project was overall successful, but I learned several important things:

- Getting feedback from users early is very important, even when they point out problems with your design.
- A simple solution (such as setting a 100MB limit) can sometimes work better than a complex one.
- It's just as important to build good relationships with team members and stakeholders as it is to create the right technical solution.
- Being flexible when plans need to change.
