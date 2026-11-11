# Presentation narration script

**Target duration:** approximately 11–13 minutes  
**Presenter:** Praneeth Kalangi

## Slide 1: When data changes

Hello everyone. My short story is about autonomous drift learning in data streams. I use a 2026 survey by Xiaoyu Yang, En Yu, and Jie Lu as the primary paper, and I connect it to an accepted KDD 2026 benchmark on drift-detection methods.

The central problem is simple. A model can perform well when we deploy it and still become unreliable later, even when its code does not change. The world that produces the data changes. The survey argues that we should think beyond ordinary concept drift and consider changes in time, data, and the model itself.

This topic belongs directly to data mining because the observations arrive as a stream rather than as one fixed dataset. The task is to discover meaningful changes online while separating true distribution shifts from ordinary noise.

## Slide 2: A production model is a snapshot

Imagine a bank that trains a fraud detector in January. Later, customers adopt new payment tools, merchants change their behavior, and attackers adjust their strategy. The model continues to apply patterns learned from the old environment.

Offline accuracy measured on a fixed test set cannot guarantee future performance. If the joint distribution of inputs and labels changes over time, the relationship learned by the model can become outdated. This is the basic idea behind concept drift.

The difficult point is that nothing has to fail technically. The prediction API can remain available and fast while the decisions become steadily less useful. Drift monitoring therefore belongs beside ordinary software monitoring, not after it.

## Slide 3: From concept drift to drift learning

Earlier work organized the problem around detection, understanding, and adaptation. The 2026 survey expands the operational state of a learning system. It includes both the external data distribution and the model's internal parameters or structure.

The authors reviewed 193 representative studies and proposed three dimensions: time-stream drift, data-stream drift, and model-stream drift. This is useful because a deployed system may fail even when the input distribution looks stable. A continually updated model may forget earlier knowledge or diverge from other models.

The survey uses a broader operational state that combines the external distribution with the model's internal hypothesis. Drift means that this state differs across time or across environments. That definition creates a bridge between concept drift, continual learning, federated learning, and reinforcement learning.

## Slide 4: Three-dimensional taxonomy

The first dimension asks how change unfolds over time. It distinguishes arbitrary drift from rhythmic drift.

The second asks what changed in the data. Representation drift changes the feature space. Semantic drift changes the relationship between observations and their meaning.

The third asks what changed inside the learning system. It includes sequence drift, heterogeneity drift, and policy drift.

These dimensions can occur together. A seasonal data shift may trigger continual updates, which may then produce forgetting inside the model.

The taxonomy is therefore diagnostic rather than merely descriptive. If a team identifies the wrong source, it may apply the wrong correction. Retraining a model cannot repair a broken sensor, and feature normalization cannot correct a changed label definition.

## Slide 5: Arbitrary and rhythmic drift

Arbitrary drift is unpredictable. Examples include a new fraud strategy, a sensor failure, or an unexpected economic event. The system needs reactive detection and adaptation.

Rhythmic drift has recurring structure. Electricity demand changes by time of day, and retail behavior follows weekly and seasonal patterns. A system can potentially learn these rhythms and prepare for the next regime.

My interpretation is that monitoring should not treat every distribution change as an incident. Some changes are recurring operating conditions, and the model can exploit that structure.

The response also changes. Arbitrary drift favors rapid detection, recent windows, and robust fallback models. Rhythmic drift supports scheduled specialists, temporal features, or forecasting of the next data regime.

## Slide 6: Representation and semantic drift

Representation drift changes how data appears. A new camera, sensor calibration, or network protocol may alter the input distribution while the task remains the same.

Semantic drift changes what the labels mean or how inputs map to labels. A transaction pattern that was previously harmless may become associated with fraud.

This distinction determines the repair. Feature alignment may help representation drift. Semantic drift usually requires updated labels, decision boundaries, or continual adaptation. A detector that only monitors inputs can miss a label-only change.

This creates a practical design question: what signals are actually available? If labels arrive weeks later, a system may monitor input features and prediction confidence immediately, then verify the suspected drift when labels arrive. The monitoring architecture needs to represent that uncertainty.

## Slide 7: The model can drift too

Sequence drift appears when continual updates cause catastrophic forgetting. Heterogeneity drift appears when models trained by different clients or devices gradually diverge. Policy drift appears when a model's actions change the environment that supplies future data.

A recommender system is a good example. Its recommendations influence what users see, which influences what they click, which then becomes training data. The model participates in the process it is trying to learn.

This is why model monitoring should include parameter movement, model disagreement, update magnitude, and feedback effects, not only input statistics.

In federated learning, this can be especially difficult because there is no single stream or single model state. A global model may look stable while a subset of clients experiences local drift. Aggregating those clients without diagnosis can spread the problem.

## Slide 8: Reference architecture

This architecture separates observation, diagnosis, adaptation, and validation.

The production model makes predictions from a live stream. The monitoring layer tracks features, predictions, uncertainty, delayed labels, and business outcomes. A detector converts those signals into drift evidence.

The diagnosis stage estimates the drift family and severity. A response controller can wait, adjust calibration, reweight an ensemble, update selected parameters, retrain on a recent window, or restore a specialist for a recurring regime.

The candidate model must pass an evaluation and safety gate before promotion. This prevents one false alarm from causing a harmful automatic update.

The controller should also support proportionate responses. A calibration adjustment is less disruptive than full retraining. A recurring regime may justify restoring an earlier specialist. A high-risk medical or financial decision may require a human reviewer. Autonomy should refer to disciplined control, not uncontrolled self-modification.

## Slide 9: Why evaluation is difficult

A classifier produces predictions that we compare with labels. A drift detector produces alarms on a timeline, and real drift points are usually unknown.

The KDD 2026 benchmark solves part of this problem by injecting controlled changes into seven real-world datasets. It evaluates 14 detectors across four drift mechanisms under abrupt and gradual transitions. Each dataset and drift combination uses 50 Monte Carlo trials.

This preserves realistic data while giving the researchers known drift locations for supervised evaluation.

The four injected mechanisms change class frequencies, swap labels, permute features, or filter feature values. Those mechanisms test whether a detector is sensitive to changes in inputs, labels, or both. Leave-one-dataset-out tuning then tests whether the selected parameters transfer beyond the streams used for optimization.

## Slide 10: Metrics beyond F1

Precision measures how many alarms were correct. Recall measures how many drift events were found. F1 balances those quantities.

Timing also matters. A correct alarm that arrives too late may have little operational value. The benchmark reports normalized detection time, episode recall, and false-alarm rate.

These metrics should be interpreted together. Normalized detection time only describes successfully detected events, so it cannot replace recall. False alarms also matter because each alarm may trigger investigation or retraining.

An ideal detector would find every drift immediately and never raise a false alarm. Real detectors occupy a tradeoff. The acceptable point depends on application cost. Fraud systems may tolerate more alarms than an expensive industrial retraining pipeline, while a safety-critical system may prioritize recall even when investigation cost increases.

## Slide 11: Benchmark findings

The chart shows the mean rank across four drift mechanisms. Lower values are better.

SEED and STEPD perform consistently well across many conditions. Abrupt drift is usually easier than gradual drift. Some methods specialize. ABCD applied to the feature space is strong for feature permutation but weak for label changes because it cannot observe the target variable.

The important result is that no detector wins everywhere. The best monitoring signal depends on the kind of change and whether labels are available.

ABCD applied directly to features ranks strongly for abrupt feature permutation but poorly for label-based changes. ADWIN performs better in some class-prior and feature-filtering cases than in feature permutation. These specializations support a multi-signal strategy rather than blind commitment to one detector.

## Slide 12: Hyperparameter ablation

The benchmark tunes detectors using leave-one-dataset-out optimization. For each test dataset, parameters are selected using the other six datasets. The paper reports that this improves performance over default configurations.

This serves as an ablation study. When cross-dataset tuning is removed, detection quality falls and comparisons can become biased toward methods whose defaults happen to match the evaluation stream.

The broader lesson is that default parameters are not automatically a fair baseline.

The study uses 30 random-search iterations inside the optimization procedure. Parameters are chosen on streams other than the final evaluation stream. This resembles the separation between training and testing in ordinary machine learning, but the unit being held out is an entire dataset rather than individual rows.

## Slide 13: Applications and open risks

Drift learning matters in fraud detection, healthcare, recommendation systems, autonomous vehicles, predictive maintenance, and federated learning.

The same risks appear across domains. Detection does not explain the cause. Adaptation can create forgetting or bias. Fully automatic retraining can turn noise into a production change. Beneficial specialization can also look similar to harmful divergence.

I believe future systems need causal diagnosis, uncertainty estimates, audit trails, rollback, and human approval for high-risk updates.

Another open question is distinguishing beneficial specialization from harmful drift. A local hospital model may adapt productively to its population, while an uncontrolled client may become incompatible with the global model. Movement alone does not tell us whether the direction is useful.

## Slide 14: Final takeaway

The 2026 survey gives us a broader vocabulary for change. Time-stream drift describes temporal behavior. Data-stream drift separates representation from meaning. Model-stream drift captures changes inside learners and distributed systems.

The KDD benchmark shows why evaluation must include detection quality, timing, false alarms, realistic data, and generalizable tuning.

My final takeaway is that the goal is not a model that changes whenever the data moves. The goal is a system that identifies the type of change, chooses a proportional response, and validates that response before deployment.

Thank you.

