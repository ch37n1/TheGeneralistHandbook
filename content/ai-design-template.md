# AI Project Planning Template

## Notes

### Technical Risks

- Insufficient data
- Too strict SLAs
- Low data quality
- Model does not converge

### Business Risks

- Stakeholders disagree with metrics
- Budget overrun
- Time-to-market delays
- Regulatory compliance issues

### Mitigation Strategies

- For each top-3 risk
- Plan B (fallback options)

⸻

## Baseline

### Metrics Discussion Questions

- Which metric best correlates with the business goal?
- Why did you choose this metric? What alternatives were considered?
- What auxiliary metrics are needed for the full picture?
- What loss function are we using and why?
- How will we measure fairness/bias?

### Baseline Discussion Questions

- What will be easier/faster to make a working solution?
- What baseline can be launched in a week?
- What accuracy do we expect from the baseline?
- How will the baseline help validate more complex solutions?

### Types of Baseline (select)

- Constant (mean/median/mode)
- Rule-based system (classic backend / webapp)
- Linear model
- Pre-trained model (fine-tuning)

⸻

## Features

### Data Sources and Integrations

- What internal data is available?
- Are external sources needed?
- What is the data freshness (real-time, batch)?
- Are there quality issues?

### Feature Engineering

- Which 2–3 key features are most important?
- What types of features are we using?
- Numerical
- Categorical encoding
- Temporal features (lags, aggregations)
- Text features (embeddings, TF-IDF)
- Interaction features
- Is a feature store needed?
- How do we handle missing values?

### Data Pipeline

- ETL vs ELT approach
- Batch vs streaming
- Data validation schema
- Data versioning

⸻

## Target Model V1

### Discussion Questions

- What architecture are we choosing for MVP?
- Why this model and not alternatives?
- What trade-offs are we making? (accuracy vs latency vs cost)
- Can we use pre-trained models?
- Do we need an ensemble or is one model enough?

### Comparison of Alternatives

- Can list pros/cons of each
- Can list parameters in a table

⸻

## Monitoring

What We Monitor

- Infrastructure: latency, throughput, errors, resource utilization
- Data Quality: missing values, schema changes, distribution shifts
- Model Performance: accuracy, precision, recall (if labels are available)
- Business KPIs: conversion, revenue, user satisfaction

Drift Detection

- What types of drift do we expect? (data/concept/output)
- How do we detect drift? (statistical tests, monitoring dashboards)

⸻

## Validation

### Validation Discussion Questions

- Temporal split or random split?
- How many folds in cross-validation?
- How often do we update validation?
- Is there leakage in the data?
- Is validation needed on separate segments?

### Types of Validation

- K-fold CV (for tabular data)
- Time-series split (for time series)
- Stratified split (for imbalanced classes)
- Group-based split (for hierarchical data)

⸻

## Evaluation Strategy

### Offline Evaluation

- Metric on validation set
- Error analysis (residuals, confusion matrix)
- Segment analysis (by user groups, regions, etc.)

### Online Evaluation

- A/B test design
- Splitting strategy (user/session/time)
- Sample size calculation
- MDE (Minimum Detectable Effect)
- Duration
- Metrics to track (primary + guardrails)
- Success criteria

### Human Evaluation (if needed)

- Who evaluates? (domain experts, crowdsourcing)
- How many samples are needed?
- Evaluation protocol

⸻

## Integration & Deployment

System Context (C4 Level 1)

[User] → [ML System] → [Database]
           ↑
    [External APIs]

### Key Questions

- Where does the data for inference come from?
- Where does training happen? (cloud, on-prem)
- Where and how does inference happen? (online/offline, batch/real-time)
- How does the system interact with the user?
- What dependencies exist on other systems?

### Deployment Strategy

- Blue-green deployment
- Canary deployment (% traffic)
- Shadow mode (parallel testing)
- Feature flags

### API Design

- REST vs gRPC vs message queue
- Request/response format
- SLA requirements (latency, availability)

Additional sources:

- [crisp-ml](https://ml-ops.org/content/crisp-ml)
