# ml-engineer

A Claude Code plugin that makes Claude work like a senior AI/ML engineer and researcher. It researches papers first, picks the best method for your problem, trains and fine-tunes rigorously, evaluates honestly with no data leakage and no inflated numbers, and iteratively pushes accuracy toward the realistic ceiling. It works across Computer Vision, Medical Imaging, NLP and LLM, Tabular, and Time-Series.

## What it does automatically

When you describe an ML task, Claude auto-invokes the right skills:

- "detect vehicles, faces, or objects in images" goes to domain-computer-vision
- "classify brain tumor MRI or benign vs malignant mammogram" goes to domain-medical-imaging
- "predict churn, fraud, or price from this CSV" goes to domain-tabular
- "forecast demand or sales" goes to domain-time-series
- "classify text, build a RAG chatbot, or fine-tune an LLM" goes to domain-nlp-llm
- "accuracy is still too low, improve it" goes to accuracy-improvement-loop

The orchestrator skill (ml-research-methodology) runs the full pipeline: framing, literature review, data-leakage audit, method selection, honest baselines, training, rigorous evaluation, and deployment with explainability.

It also detects whether you want a Kaggle notebook, a Google Colab notebook, or a local GPU run, and adapts the deliverable. Generated notebooks use short simple explanations in a professional research tone, with no em-dash and no emoji.

## Requirements

- Claude Code (CLI, desktop, or IDE extension).
- Git, to clone or add the plugin from a repository.

## Install

### From GitHub (recommended, this is how others install it)

```
/plugin marketplace add mxslr/ml-engineer
/plugin install ml-engineer@ml-engineer-marketplace
```

The first command registers the marketplace defined in this repository. The second installs the plugin named ml-engineer from the marketplace named ml-engineer-marketplace. This is the same pattern as other public plugins, for example `/plugin install superpowers@superpowers-marketplace`.

### From a local folder (for developing the plugin on your own machine)

From the directory that contains the cloned repository:

```
/plugin marketplace add ./ml-engineer
/plugin install ml-engineer@ml-engineer-marketplace
```

After installing, restart or reload Claude Code so the skills are picked up. Verify with /plugin and confirm the plugin appears as enabled.

## Use

- Automatic: describe your task in chat and the skills trigger themselves.
- Explicit: run /ml-project followed by your task, dataset, and goal metric.

## Publish to GitHub (for the maintainer)

Run these from inside the plugin folder.

The gh CLI is the easiest path. If you do not have it, install it from https://cli.github.com then run:

```
git init
git add .
git commit -m "ml-engineer plugin"
gh auth login
gh repo create ml-engineer --public --source=. --remote=origin --push
```

Without gh, create an empty repository named ml-engineer on github.com first, then run:

```
git init
git add .
git commit -m "ml-engineer plugin"
git branch -M main
git remote add origin https://github.com/mxslr/ml-engineer.git
git push -u origin main
```

## Components

| Type | Name | Purpose |
|---|---|---|
| command | /ml-project | Entry point for the full workflow |
| agent | paper-researcher | Read-only literature brief: SOTA, realistic ceiling, leakage traps |
| skill | ml-research-methodology | Orchestrator and routing table |
| skill | notebook-delivery | Detect Kaggle vs Colab vs local GPU, enforce notebook style |
| skill | literature-review | Find and critically appraise papers |
| skill | data-rigor-and-leakage | Correct splits and leakage hunt |
| skill | training-optimization | Fine-tuning recipe, anti-overfit, GPU budget |
| skill | rigorous-evaluation | Right metrics, calibration, thresholds chosen on validation |
| skill | deployment-explainability | Inference parity, Grad-CAM, guardrails |
| skill | accuracy-improvement-loop | Diagnose, research the gap, combine techniques |
| skill | domain-computer-vision | CV model selection |
| skill | domain-medical-imaging | Medical rigor and model selection |
| skill | domain-nlp-llm | NLP, LLM, and RAG method selection |
| skill | domain-tabular | Tabular model selection |
| skill | domain-time-series | Forecasting, classification, anomaly detection |

## Design principle

Honesty over inflated numbers. A valid, reproducible 0.82 AUC beats a leaky 0.99. The plugin's core value is knowing which numbers are real, which are inflated by ROI-cropping or data leakage, and when a model has reached its realistic ceiling.
