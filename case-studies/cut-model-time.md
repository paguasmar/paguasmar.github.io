---
layout: page
title: "How I Cut Model Training Time by 93% with Snowflake-Powered MLOps"
subtitle: "How MLOps Saved $1.8M in ARR for a SaaS Giant"
share-title: "Data Nautical | How I Cut Model Training Time by 93% with Snowflake-Powered MLOps"

---

<!--
Tags:
#MLOps #Snowflake #DataEngineering #MachineLearning #AI #CaseStudy #DataScience #Automation #TechLeadership #SaaS #ChurnPrediction
-->

[Pedro Marques](https://www.linkedin.com/in/paguasmar/)

When I joined a 3,000+ employee SaaS firm and discovered the churn prediction model was taking 5 hours to train monthly with only 46% precision—costing +$10M in annual revenue loss—I knew we needed robust MLOps to turn it around. Here's how 3 months of focused work boosted efficiency by 93% and positioned us to save $1.8M in ARR.

Found this useful so far? Follow me on Medium for more practical insights on technical leadership and automation solutions.

## The Problem: Repeated Delays

Picture this: manual notebook runs, no automation, and precision hovering at 46% on outdated data. The team faced:

1. 5-hour training times blocking iterations.
2. Data drift causing unstable predictions (e.g., jumps from 40 to 70 at-risk customers).
3. No CI/CD or monitoring, leading to delays and missed +$10M churn opportunities.

This stemmed from limited ML experience and basic tracking in tables—wasting data scientists time on routine tasks instead of strategic revenue protection.

![](/imgs/case-studies/cut-model-time/meme_pizza_party.png)

## The Solution: Optimized MLOps Pipeline
![](/imgs/case-studies/cut-model-time/architecture_mlops_infra.svg)

I leveraged Snowflake-native tools for a scalable solution:

- Snowflake ML for core modeling, as it integrated seamlessly with existing data.
- Snowpark for parallel processing, chosen over single-threaded notebooks to slash runtime.
- PSI/KSA metrics in monitoring, building on Snowflake's features for drift detection.
- Git for versioning, ensuring traceability across releases.
- Snowflake tasks for automation, aligning with upcoming CI/CD needs.
- Model registry for persistence, decoupling training from inference.

Key Features:

- Automated monthly refreshes.
- Feature reduction from 23 to 21 without metric loss.
- Drift monitoring with alerts.
- Weighted average for stable predictions

## The Implementation Journey

The project kicked off with a deep dive into the existing model: analyzing 18 features, running experiments, and collaborating across teams. I phased it for quick wins:

1. Refactor code and migrate to Snowpark.
2. Implement monitoring and versioning.

### Overcoming Model Maintainer Departure

Early hurdles included mainly permissions delays. Mid-project, the Lead Data Scientist's sudden departure forced me to pivot from pure infrastructure work to a 70/30 split—70% on model performance amid manual updates and looming deadlines. Overwhelmed by the Chief Data Officer's frustrations in daily meetings, I simplified by shifting to bi-quarterly releases and prioritizing runtime reductions. This turned chaos into success: a 93% faster model, fixed degradations via monitoring, and €1.8M in protected ARR, laying the groundwork for scalable MLOps.

When feature experiments showed degradation on fresh data (precision dropping to 46%), I adjusted training windows quarterly—balancing accuracy with business timelines. This iterative approach avoided overengineering, delivering value amid the handover.

The refactor halved implementation changes through code consolidation and top-level parameters, enabling single-point updates that saved half a day and simplified automation.

Dealing with similar MLOps scaling challenges? Contact us today!

[Contact Us](/contact){: .btn .btn-primary }

### Building Trust Through Collaboration

Cross-team friction arose from misaligned expectations—e.g., sales questioning prediction swings. I focused on transparency: sharing SHAP values for explainability and looping in the Team Lead for stakeholder meetings. By documenting everything and aligning on metrics, we built confidence, turning initial delays into a reusable framework for other models. When the Chief Data Officer reviewed the 93% reduction and 30-39% metric gains, he noted the results "looked pretty good," boosting morale amid ongoing pressures.

With the model grinding through 5-hour trainings—limiting us to one experiment per day and stalling iterations—I tested parallelization locally using n_jobs=-1 in XGBoost, slashing time by 95%. But in Snowflake? Zero improvement—frustrating dead end after dead end. Late one night, buried in docs, I discovered joblib's parallel_backend with threading. Heart racing, I tweaked the code and ran it: Boom—93% reduction to 20 minutes on production data. Suddenly, we could run 20 experiments daily, unlocking 30% precision and 39% recall gains, turning months of delays into rapid progress toward $1.8M ARR savings.

## The Results: Beyond Time Savings

The impact was transformative:

- Training time: ↓93% (5 hours to 20 minutes).
- Precision: ↑30% (0.46 to 0.60); Recall: ↑39% (0.36 to 0.50).
- $1.8M potential ARR protected in July predictions.
- Inference time: Eliminated 5-hour retrain overhead per prediction via registry, enabling on-demand runs.
- Delivered 1 month early, enabling 24 experiments per day vs. 1.

Moreover, this optimized pipeline now forms the foundation for future models reusing the XGBoost efficiency gains, CI/CD automation, and monitoring framework—extending the $1.8M ARR protection potential across initiatives without rebuilding from scratch
## Key Learnings

- Prioritize high-impact optimizations early for quick wins.
- Use monitoring to catch degradation proactively.
- Document versioning rigorously to handle team transitions smoothly.

## The Takeaway

This churn project reinforced that MLOps isn't just technical—it's about delivering measurable business value amid constraints. While we hit delays on full automation, the 93% efficiency gain and improved metrics set a foundation for Q4 full-blown ML Infrastructure. Key lessons: Balance innovation with practicality, collaborate transparently, and always tie efforts to ROI. As we scale, this work's reusability—serving as a blueprint for new models with shared optimizations and infrastructure—ensures sustained AI maturity at the company.
