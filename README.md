# Autonomous Drift Learning in Data Streams

## CMPE 255 Short Story Assignment

**Student:** Praneeth Kalangi  
**Topic:** Autonomous Drift Learning in Data Streams  
**Primary paper:** [Autonomous Drift Learning in Data Streams: A Unified Perspective](https://arxiv.org/abs/2605.01295)  
**Course topic:** Data stream mining, concept drift, continual learning, and adaptive machine learning

This repository contains the article, original diagrams, presentation, and narration materials for the CMPE 255 short-story assignment. The work explains how machine-learning systems can detect, diagnose, and adapt to changes in streaming data and in their own internal model states.

## Published links

Replace the placeholders after publishing.

| Deliverable | Link |
|---|---|
| Approved-topic spreadsheet | [Fall 2026 CMPE 255 topic spreadsheet](https://docs.google.com/spreadsheets/d/1dgFf_VLUsn00PcexC8l3uXd5lKOSDMXS66x70DCNL_g/edit) |
| Medium article | `ADD_MEDIUM_ARTICLE_URL` |
| SlideShare presentation | `ADD_SLIDESHARE_URL` |
| Presentation video | `ADD_VIDEO_URL_OR_REPOSITORY_PATH` |
| Primary survey paper | [arXiv:2605.01295](https://arxiv.org/abs/2605.01295) |
| KDD 2026 benchmark paper | [arXiv:2606.07789](https://arxiv.org/abs/2606.07789) |

## Short story

Machine-learning models usually assume that future data resembles their training data. That assumption fails in long-running systems. Customer behavior changes, sensors age, fraud strategies evolve, and a continuously updated model may forget earlier knowledge.

The primary 2026 survey organizes these problems into three dimensions:

1. **Time-stream drift:** arbitrary versus rhythmic change over time.
2. **Data-stream drift:** changes in representation or meaning.
3. **Model-stream drift:** sequence, heterogeneity, or policy drift inside learning systems.

The article combines this taxonomy with a KDD 2026 benchmark of 14 drift detectors. It discusses architecture, evaluation metrics, hyperparameter ablation, practical applications, limitations, and future research.

## Repository contents

```text
.
├── README.md
├── article/
│   ├── medium-article.md
│   └── references.md
├── presentation/
│   ├── autonomous-drift-learning.pptx
│   └── autonomous-drift-learning.pdf
├── narration/
│   └── presentation-script.md
├── images/
│   ├── cover-drift-learning.png
│   ├── drift-taxonomy.png
│   ├── drift-timeline.png
│   ├── drift-learning-architecture.png
│   ├── evaluation-pipeline.png
│   └── detector-comparison.png
├── video/
│   └── README.md
└── sources/
    └── source-links.md
```

## Main contributions covered

- A plain-language explanation of concept drift in data streams
- The 2026 three-dimensional drift-learning taxonomy
- A reference architecture for controlled model adaptation
- Evaluation metrics that include accuracy, false alarms, and detection delay
- Experimental evidence from the accepted KDD 2026 benchmark
- An interpretation of detector specialization and hyperparameter tuning
- Applications in fraud detection, healthcare, recommendations, and robotics
- Critical analysis of safety, causal diagnosis, and autonomous retraining

## Visualizations

All graphics in this repository were created specifically for this short story. They summarize concepts from the cited papers rather than reproducing published figures.

1. Drift-learning cover illustration
2. Three-dimensional drift taxonomy
3. Arbitrary versus rhythmic drift timeline
4. Autonomous drift-learning system architecture
5. Drift-detector evaluation pipeline
6. Detector rank comparison calculated from KDD 2026 Tables 1 and 2

## Key experimental evidence

The KDD 2026 companion study evaluates 14 detectors on seven real-world datasets. It injects class-prior, label-swap, feature-permutation, and feature-filtering drift under abrupt and gradual transitions. The evaluation uses 50 Monte Carlo trials per dataset and drift mechanism and tunes detector parameters using leave-one-dataset-out optimization.

The reported results show that:

- SEED and STEPD are consistently competitive across many scenarios.
- Abrupt drift is generally easier to detect than gradual drift.
- Feature-only detectors can miss changes that affect only labels.
- No detector performs best across every drift mechanism.
- Hyperparameter optimization materially improves detection performance.

## Presentation and video

The presentation contains 14 slides designed for a 10–15 minute recording. The complete narration appears in [`narration/presentation-script.md`](narration/presentation-script.md).

Suggested recording workflow:

1. Open the PowerPoint or PDF in presentation mode.
2. Record a 1080p presentation using Zoom, PowerPoint Recorder, or OBS.
3. Follow the narration as a guide rather than reading it mechanically.
4. Export the recording as MP4.
5. Add the MP4 to `video/` or upload it to YouTube and place the link above.
6. Verify that the public GitHub repository opens in a private browser window without authentication.

## References

The complete source list and research notes appear in [`article/references.md`](article/references.md).

### Primary sources

- Yang, X., Yu, E., and Lu, J. (2026). *Autonomous Drift Learning in Data Streams: A Unified Perspective.* arXiv:2605.01295.
- Cerqueira, V., Gomes, H. M., Heyden, M., Pfahringer, B., and Bifet, A. (2026). *A Framework for Evaluating and Benchmarking Concept Drift Detection Methods.* KDD 2026.
- Lu, J., Liu, A., Dong, F., Gu, F., Gama, J., and Zhang, G. (2019). *Learning under Concept Drift: A Review.* IEEE TKDE.
- Bifet, A., and Gavaldà, R. (2007). *Learning from Time-Changing Data with Adaptive Windowing.* SDM 2007.
- Gomes, H. M., et al. (2017). *Adaptive Random Forests for Evolving Data Stream Classification.* Machine Learning.

## Submission checklist

- [ ] Topic is marked approved in the professor's spreadsheet
- [ ] Repository visibility is public
- [ ] Medium article is published and linked above
- [ ] Slide deck is uploaded to SlideShare and linked above
- [ ] 10–15-minute video is uploaded and linked above
- [ ] PowerPoint and PDF open correctly
- [ ] Every figure includes attribution
- [ ] README placeholders have been replaced
- [ ] All links work in a private browser window

