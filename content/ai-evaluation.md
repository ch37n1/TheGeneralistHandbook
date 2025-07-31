## Oreview
The AI lifecycle describes the stages involved in developing and managing AI systems, from initial ideation to post-deployment maintenance. It typically includes the following phases:
- **Ideation and Planning**: Identifying problems, defining objectives, and determining feasibility, resources, and success criteria.
- **Data Collection and Preparation**: Gathering relevant data, preprocessing, cleaning, and ensuring quality.
- **Model Development**: Selecting algorithms, training models, tuning hyperparameters, and validating results.
- **Deployment**: Integrating the AI system into existing infrastructure, including model serving, scalability, and integration testing.
- **Post-Deployment Monitoring and Maintenance**: Continuously assessing model performance, managing data drift, updating models as necessary, and addressing operational issues.

Each stage involves distinct tasks and challenges that must be carefully managed to ensure the system remains effective, reliable, and aligned with business objectives.

Several perspectives exist for evaluating the quality of AI systems (e.g., assistants, pipelines, tools): machine learning, software engineering, and business. Each perspective emphasizes specific properties of the system and employs its own evaluation methods. Typically, these perspectives are difficult to integrate. Below is a brief overview of each, including key features and evaluation approaches.

Evaluating AI projects involves systematic approaches to assessing system quality, reliability, and alignment with goals. Key foundational concepts include:

**Evaluation, Validation, and Verification**
- **Evaluation** assesses the overall performance and quality of the AI system, typically focusing on its ability to meet specified criteria and stakeholder expectations.
- **Validation** determines whether the AI system meets user requirements and appropriately solves the intended real-world problem.
- **Verification** ensures the AI system was built correctly according to design specifications, typically emphasizing correctness, reliability, and consistency.  

## Classical Evaluation Frameworks
- **CRISP-DM (Cross-Industry Standard Process for Data Mining)**: A widely used process model with phases including business understanding, data understanding, data preparation, modeling, evaluation, and deployment. It emphasizes iterative development and evaluation throughout the project.
- **Machine Learning Lifecycle Models**: Focused on stages specific to ML workflows, including problem definition, data handling, model training, testing, deployment, and monitoring. These models help define where and how evaluation should occur.

### Technical Evaluation Criteria

- **Data Quality and Suitability**
    - Representativeness, bias, labeling quality, leakage
- **Model Performance**
    - Metrics by task type: classification, regression, generation, ranking, etc.
    - Cross-validation and statistical rigor
- **Robustness and Generalization**
    - Adversarial robustness, out-of-distribution generalization
- **Scalability and Efficiency**
    - Time/space complexity, inference latency, hardware compatibility
### Evaluation Tools and Platforms

- Model Evaluation Tools (Weights & Biases, MLflow, etc.)
- Data Evaluation Tools (Great Expectations, Snorkel, etc.)
- Fairness and Explainability Toolkits
- Continuous Evaluation Pipelines (CI/CD for ML)
### **Software Engineering Perspective**
In AI products, machine learning components are tightly integrated with conventional software logic. As a result, standard software testing and quality assurance practices apply and must be rigorously followed. ML components should not be treated as exceptions but tested as part of the system.

All standard layers of the application require classical testing approaches:
- **Business logic** (e.g., product search, shopping cart behavior)
- **Integrations** (e.g., payment gateways, inter-service communication)
- **Infrastructure** (e.g., databases, caching, messaging queues)
    
> While traditional testing frameworks often emphasize the “testing pyramid” (heavy unit testing with fewer high-level tests), a more practical approach is the [Testing Diamond](https://www.crispy-engineering.com/p/why-test-diamond-model-makes-sense) This model advocates focusing more on integration and system tests, as unit tests in fast-evolving AI systems tend to degrade into legacy code, increasing maintenance cost without proportional value.

### **Machine Learning Perspective**

Classical software testing frameworks fall short when applied directly to machine learning components. Unlike deterministic systems, ML systems produce outputs with inherent uncertainty. Relying on strict pass/fail test cases often leads to instability and misleading results. Instead, ML components require evaluation methods that reflect the probabilistic and data-dependent nature of model behavior.

**Nature of AI Systems Tasks:**
AI systems typically perform tasks such as:
- **Generation** (e.g., text, images, or recommendations)
- **Ranking** (e.g., search results or candidate selection)
- **Classification** (e.g., spam detection or sentiment analysis)

Each task type demands tailored metrics and evaluation protocols. Evaluating these systems involves both offline (dataset-based) metrics and online (real-world or A/B test-based) measurements.

**Metrics and Evaluation Points:**

Evaluation must occur at every stage of the ML pipeline where a model contributes, with granular metrics per component. This enables:
- Clear isolation of underperforming components
- Robustness testing across diverse data slices
- More actionable insights than global performance alone

**RAG (Retrieval-Augmented Generation) Systems:**
- **Generation Component:** Should be evaluated using metrics like factuality, relevance, and coherence, based on injected context.
- **Retrieval Component:** Evaluated independently using ranking metrics such as recall@k, MRR, or NDCG.
- **End-to-End Performance:** Can be measured by downstream utility (e.g., task success or user satisfaction).

**Hybrid Testing Approaches:**
- ML systems do not operate in isolation; they are embedded in broader products. While core model evaluation uses ML-specific metrics, supporting infrastructure (e.g., API behavior, context construction, caching) should still be validated using classical software QA methods.
- This hybrid approach ensures that probabilistic outputs are evaluated fairly, while deterministic dependencies are tested rigorously.

### **Business Perspective**

From the business perspective, the primary concern is whether the AI system delivers measurable value and aligns with strategic objectives. Evaluation focuses on real-world impact, user experience, and risk management rather than technical correctness alone.  

**User Scenarios:**
Evaluation is centered around end-to-end (E2E) user workflows. These are tested holistically, often with human-in-the-loop or LLM-based judgment to assess whether the system achieves the intended business outcome.

**Key Metrics:**
- **Return on Investment (ROI):** Measures the financial gain relative to the cost of developing and operating the AI system.
- **Expected Losses:** Quantifies the expected value of risk (e.g., reputational damage, operational failures) based on the probability and impact of adverse outcomes.
- **User Experience (UX):** Often evaluated through task efficiency metrics, such as the number of dialog turns or actions required to complete a goal, reflecting system effectiveness and user satisfaction.

## **Long-Running Tasks**

Quality evaluation for AI systems must account for performance over time, as many systems operate across extended interactions or workflows. Static or single-step testing is insufficient for capturing issues that emerge in dynamic, multi-turn scenarios.

**Dialog Assistants:**
Evaluation should be conducted both:
- **Horizontally** – Treating each new dialog as an independent session, measuring performance across many short interactions.
- **Vertically** – Following a single dialog through its full multi-turn progression, benchmarking performance within the same conversation.

> Relying solely on one approach provides an incomplete view of system behavior.
  
**Complex, Extended Tasks:**
Tasks such as code generation or multi-step planning require evaluation across:
- **Short Scenarios** – Focused, isolated tasks or subtasks
- **Long Scenarios** – Full end-to-end workflows where dependencies and context accumulation play a critical role

> **Key Principle:**
> Testing must capture system behavior **in dynamics**, ensuring that performance remains stable and reliable across extended operations, not just in isolated snapshots.
## Typical AI systems blocks

### Routing

Routing is a common pattern in AI systems, where inputs are directed to different components or actions based on their content or intent. Functionally, routing maps to classical machine learning tasks—primarily **classification** and **multi-label classification**.
#### Classification
Many routing decisions in AI systems are built on top of standard text classification models, such as those available via [Hugging Face’s text classification task](https://huggingface.co/tasks/text-classification). Typical applications include:
- Intent classification in dialog systems
- Fraud detection
- Scenario routing in workflow engines
These are all variations of classification problems.

**Evaluation Metrics:**
- **Confusion Matrix**: Provides a breakdown of prediction types (true/false positives and negatives).
- **Accuracy**: Proportion of correct predictions: (TP + TN) / Total
- **Precision**: Correct positive predictions out of all positive predictions: TP / (TP + FP)
- **Recall**: Correct positive predictions out of all actual positives: TP / (TP + FN)
- **F1-Score**: Harmonic mean of precision and recall
- **ROC AUC**: Area under the receiver operating characteristic curve, for binary classification
### Multilabel Classification (Parameters extraction)

Treat each key=value pair as a label:

```
{"intent": "search", "from": "yesterday", "with_web": true}
```

becomes:

```
{"intent=search", "from=yesterday", "with_web=true"}
```

- Use same as in classification such as F1 score, accuracy, ROC-AUC  (but now can be aggregated version mean, weighted, averaged).
- Use micro, macro, and per-label versions depending on your need.

|**Metric Type**|**What It Measures**|**Pros**|**Cons**|
|---|---|---|---|
|Multilabel F1|Per key-value pair accuracy|Fine-grained, robust|Ignores inter-key dependencies|
|Exact match|Whole structure correctness|Strict, intuitive|Harsh on near-misses|
|Per-key accuracy|Accuracy per key|Diagnoses field-specific errors|Doesn’t penalize extra predictions|
|Hamming loss|Average number of incorrect key-value predictions|Penalizes over- and under-prediction|Harder to interpret directly|

Possible implementation
* Add one column with full object (can be converted to sorted string) > calculate full metric
* Convert `[{json}]` to dataframe where each field will be column > calculate per field metric
- `sklearn.metrics.classification_report` (treat each key=value as a label)

### **Question Answering (QA)**
[Hugging Face QA Task Reference](https://huggingface.co/tasks/question-answering)
Question Answering (QA) is a core task in generative AI systems, where the model generates answers based on a question and a given context (extractive QA) or without it (abstractive QA). It is widely used in virtual assistants, search augmentation, knowledge bases, and document summarization systems.

 **Approaches:**

**Fixed answer Evaluation Metrics:**

- **Exact Match (EM):** Measures whether the predicted answer matches the reference exactly.
- **F1 Score:** Measures token-level overlap between predicted and true answers.
- **Latency:** Especially important in real-time systems.

**Abstractive Evaluation Metrics:**

- **Faithfulness & Factuality:** Human or LLM-based judgment to assess whether the generated answer is consistent with source facts.

Classic methods (but it is not often possible to determine the “golden” answer).
- **BLEU / ROUGE / METEOR:** Compare n-gram overlap with reference answers.
- **BERTScore:** Uses contextual embeddings to assess semantic similarity.
### Ranking evaluation (Retrieval)

Ranking is a fundamental task in many AI systems, particularly in search engines, recommendation systems, and Retrieval-Augmented Generation (RAG) pipelines. The objective is to return a ranked list of items (documents, products, answers) ordered by relevance to a given query.


Ranking evaluation requires metrics that reflect the quality of ordering, not just binary classification performance:
- **Precision@k**: Proportion of relevant items in the top _k_ results. Useful for tasks where only the top few results matter.
- **Recall@k**: Proportion of all relevant items retrieved in the top _k_ results. Important for comprehensive retrieval systems.
- **Mean Reciprocal Rank (MRR)**: Inverse of the rank at which the first relevant result appears, averaged across queries. Penalizes late retrieval of relevant content.
- **Normalized Discounted Cumulative Gain (nDCG@k)**: Captures both relevance and rank position, accounting for graded relevance (e.g., very relevant vs. somewhat relevant).
- **Hit@k**: Whether at least one relevant item is in the top _k_.

Evaluation Strategies:
- **Dataset Curation**: Use benchmark datasets (e.g., MS MARCO, TREC) with labeled relevance judgments.
- **Hard Negative Mining**: Include similar but non-relevant candidates to stress-test the model’s ability to rank accurately.
- **A/B Testing**: In production, compare user engagement (CTR, conversion, dwell time) for different ranking strategies.

## Technical Evaluation
Technical metrics assess the system’s performance characteristics such as speed, resource usage, and responsiveness. These metrics are essential for evaluating AI systems in production environments where scalability, user experience, and infrastructure cost are critical.

**Key Metrics:**
- **Latency:**
	- **End-to-End Latency**: Total time from receiving an input to producing a complete output. Includes preprocessing, inference, and postprocessing.
	- **Inference Latency**: Time taken by the model to compute an output from input (excluding external overhead).
	- **Time to First Token (TTFT)**: Time between input submission and the appearance of the first generated token. Especially important for perceived responsiveness in generative systems.
- **Throughput** – Number of requests the system can handle per unit of time (e.g., queries per second). Critical for batch processing and real-time applications under load.
- **Cold Start Time** – Time required to load and initialize a model (especially large ones) before it can serve requests. Relevant in serverless or on-demand deployment environments.
## Usecase-based evaluation
Use case-based evaluation focuses on assessing AI system quality in the context of specific, real-world application scenarios. Rather than evaluating models purely on abstract or isolated metrics, this approach tests the system’s behavior as it performs concrete tasks, often involving complex logic, user interaction, or multi-component workflows.

**Key Principles**
- **Realistic Contexts**: Evaluation is aligned with actual user journeys or business processes, not just synthetic benchmarks.
- **End-to-End Perspective**: Includes all system components — from input ingestion and routing to model inference, business logic, and final output.
- **Task Success over Metric Scores**: The primary question is whether the system fulfills its intended function in a given scenario, not just how well it performs on isolated sub-tasks.
 
**Evaluation Methods**
- **Task Completion Rate**: Whether the system completes the task as defined by the use case.
- **Human-in-the-Loop Judgments**: Manual or LLM-assisted evaluation of success, relevance, and quality within a scenario.
- **Automated Integration Tests**: Simulated execution of workflows, checking key checkpoints and outputs.
- **Behavior Under Edge Cases**: Testing robustness to ambiguity, missing inputs, unexpected user behavior, or degraded context.

Examples of use cases for evaluation:
- **Customer Support Assistant**: Resolving a user issue through multi-turn dialog and escalation handling.
- **E-commerce Search**: Retrieving and ranking relevant products based on user queries, preferences, and filters.
- **Code Assistant**: Generating a complete function based on a prompt, validating through compilation and unit tests.
- **RAG Knowledge Bot**: Answering domain-specific questions based on internal documentation, with context retrieval and summarization.