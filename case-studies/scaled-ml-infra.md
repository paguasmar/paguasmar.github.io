---
layout: page
title: "How I Scaled MLOps Infrastructure for 3 Models in One Week with CI/CD"
subtitle: Safeguarding $1.9M Monthly Churn Insights Through Automated Validation and Gates
share-title: "Data Nautical | How I Scaled MLOps Infrastructure for 3 Models in One Week with CI/CD"

---

<!--
Tags:
#AWSOptimization #CloudCosts #DataEngineering #DevOps #CloudArchitecture
-->

[Pedro Marques](https://www.linkedin.com/in/paguasmar/)

## TL;DR

**Problem**: Manual ML cycles took 2-3 weeks with 100% deployment rejections, trust erosion from overlooked business rules, and inefficient 30-min monthly manual checks from stakeholders.

**Solution**: 
- Built CI/CD pipeline in 1 week with:
	- automated stages (tests, build, train, business rules validation)
	- branching (feat/* → dev → prod → main)
	- approval gates (PR approvals, manual prod deploy).

**Impact**:
- Eliminated deployment rejections (avoided 9-day fixes)
- Saved 30-min monthly manual checks from stakeholders.
- protected $1.9M monthly churn
- enabled scaling to 3+ models

---

"I don't have trust on the new model because it overlooked the business rules"

That phrase from the Chief Data Officer kicked off what would become an interesting journey into robust ML deployments.

## The Problem: Trust-Eroding Deployment Process
Imagine pushing a model update, only for manual business rules validation to reveal fatal flaws like zero overlap with prior predictions, leading to 100% rejections. Stakeholders wasted 30 minutes monthly on Excel overlap checks, while 2-3 week cycles delayed quarterly deploys.

The impact? One incident eroded trust, with 9-day fixes jeopardizing 1.9M € monthly churn insights.

## The Solution: Automated MLOps Pipeline with Guardrails

![](/imgs/case-studies/scaled-ml-infra/architecture_cicd.jpg)

To address the manual bottlenecks and trust issues, I designed and implemented a complete CI/CD pipeline in just one week, tailored for a solo developer managing a lightweight 20MB dataset while enabling scalability to 3+ models. Built on Azure DevOps for orchestration and Snowflake for ML operations, it ensures reproducible results via data snapshots and business rule checks. Here's how it works in three key phases:

1. **Core Pipeline Stages for Automation**: Integrated ML-specific stages into Azure DevOps YAML for end-to-end efficiency:
    
    - **Tests (Unit & Integration)**: Automated pytest runs for code functionality and input/output checks, catching errors early.
    - **Build & Train**: Packages the XGBoost model code and automates training/registration in Snowflake's Model Registry.
    - **Validation**: Runs business rules like overlap ratio checks against prior predictions, failing the pipeline if thresholds aren't met - eliminating manual Excel work.
    - **Deploy**: Deploys to the target environment (dev or prod) only if all prior stages pass.
    
    Example YAML snippet for the Validation stage:
    
```yaml
- stage: Validation
  displayName: "Validation"
  jobs:
    - job: Validation
        steps:
            - bash: |
                conda activate churn_prediction
                python -m pytest tests/validation -vv --junitxml=validation_test_results.xml --val-model-version=$val_model_version --prev-month-preds=$prev_month_preds --curr-month-preds=$curr_month_preds
            displayName: 'Run Validation Tests'
```
    
2. **Branching Strategy for Controlled Promotion and Scalability**: Adopted a GitOps flow (feat/* → dev → prod → main) to isolate environments and support multiple models:

	- **Feat to Dev**: Auto-deploys to dev Snowflake schema after push/merge and successful CI/CD, enabling quick iteration for any model.
	- **Dev to Prod**: Requires PR merge with 1 approver, triggering manual release pipeline for promotion to prod environment, reusable across teams.
	- **Prod to Main**: Final PR merge (1 approver) archives as immutable record for rollback, applicable to all models.
3. **Approval Gates for Safety**: Enforced via Azure DevOps policies and environments:
    
    - PR gates: Minimum 1 reviewer for dev → prod and prod → main merges.
    - Release pipeline gates: Manual approval to deploy to the prod environment - ensuring no unvetted models reach production.

[Contact Us](/contact){: .btn .btn-primary }

### Overcoming Permissions Bottlenecks

What started as a straightforward infrastructure setup turned into a frustrating delay: the team responsible for granting Azure DevOps and Snowflake permissions was overwhelmed with projects, leaving my requests unanswered for over a month. This stalled the CI/CD rollout twice, threatening deadlines.

I escalated to my manager, who intervened to prioritize the request. Within days, permissions were granted, allowing me to complete the pipeline in record time.

Navigating permissions hurdles in your MLOps setup? Contact us for streamlined solutions!

### Building Trust Through Cross-Team Debugging

Midway through testing the CI/CD pipeline, the training data retrieval failed due to a missing column in the Data Engineering team's view. Their repo SQL showed the column present, but runtime execution omitted it - a classic integration mismatch that could have derailed the entire project.

Rather than pointing fingers, I debugged the error myself, traced it to their deployment process, and provided step-by-step instructions. The DE team fixed it in just a couple of hours, restoring flow. This collaboration highlighted CI/CD's value in catching data issues early through automated integration tests, preventing similar pitfalls in future deploys.

Facing integration challenges in your ML pipelines? Reach out to our consultancy for expert guidance!

This setup required minimal code changes (e.g., env var handling for secrets) and leverages Snowflake tasks for monthly refreshes automation, reducing risks while scaling efficiently for additional models. Dealing with ML deployment challenges? Contact us for tailored MLOps solutions!

This setup required minimal code changes (e.g., env var handling for secrets) and leverages Snowflake tasks for monthly refreshes automation, reducing risks while scaling efficiently for additional models.

Dealing with ML deployment challenges? Contact us for tailored MLOps solutions!

## The Results: From Chaos to Confidence

The impact was immediate and measurable:

| **Metric**                | **Before**       | **After**   | **Improvement**        |
| ------------------------- | ---------------- | ----------- | ---------------------- |
| Deployment Rejection Rate | 100%             | 0%          | ↓100%                  |
| Manual Check Costs        | 30 min/month     | 0 min/month | ↓100% (348 €/year)     |
| Model Scalability         | 1 model          | 3+ models   | ↑200% (2,176 € saved)  |

### Business Impact

- Caught business rules issues pre-deployment, restoring stakeholder confidence and avoiding 9-day fixes.
- Safeguarded 1.9M € monthly predictions.
- Reusable infrastructure scales to new teams, turning a solo effort into a company-wide asset.

## Key Learnings

- Prioritize automation for critical checks to prevent trust-eroding failures.
- Design for scalability from day one, even with small datasets.
- Integrate business rules early to align tech with revenue goals.
- Balance speed with gates to ensure safe promotions.

How does your team handle ML validation?