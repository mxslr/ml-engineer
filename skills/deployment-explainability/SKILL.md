---
name: deployment-explainability
description: "Use when turning a trained model into something usable (an app, an inference function, a demo) or adding explainability. Covers preprocessing PARITY between training and inference (the #1 deployment bug), Grad-CAM / Grad-CAM++ / saliency for trust, calibrated probabilities, out-of-distribution guardrails, and 'decision-support not autonomous diagnosis' framing for high-stakes domains. Triggers on 'deploy', 'inference', 'Grad-CAM', 'saliency', 'explainability', 'build an app', 'doctor/user uploads an image', 'gradio/streamlit'."
---

# Deployment & Explainability

## Preprocessing PARITY (the bug that silently ruins deployments)
The raw input a user/doctor uploads MUST pass through **exactly** the same pipeline as training (resize, crop, enhancement like CLAHE, normalization). Package it as **one shared function** reused by training and inference. A mismatch confident nonsense predictions.

## Explainability for trust
- Provide **Grad-CAM and Grad-CAM++** overlays (or SHAP for tabular, attention/attribution for text) so the user sees *where/why* the model decided.
- Caption honestly: this is **model attention, not a segmentation of ground truth**.
- Show the **calibrated probability** and the operating threshold, not just a label.
- For an ensemble, serve the single best model for a coherent prediction+explanation; show ensemble probability as a second opinion.

## Guardrails (especially high-stakes)
- **Out-of-distribution check**: flag inputs that don't look like the training domain (wrong modality, low confidence) instead of guessing.
- **Framing**: for medical/safety, state clearly it is a **decision-support / second-reader tool, NOT autonomous diagnosis**; a research prototype needs clinical validation + regulatory clearance before real use.
- Prefer a **high-sensitivity** operating point when a miss is far costlier than a false alarm.
- Log inputs/outputs for audit; never let the model auto-decide irreversible actions.

## Packaging
- Export the chosen model plus the shared preprocessing + an inference function `predict(raw) -> {label, prob, explanation}`.
- A small **Gradio/Streamlit** app is the fastest trustworthy demo (drag-drop an input, then show label + probability + heatmap + disclaimer); runs on Colab/Spaces/local.
