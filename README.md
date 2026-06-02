# CRA Tutorial — Hands-on AI on the National Research Platform

A slimmed-down tour of running AI on [NRP](https://nrp.ai): calling the managed
LLM, running a local LLM on a session GPU, and building a simple RAG pipeline —
all **inside a JupyterHub session**, no pods or YAML to apply. It ends with a
short `opencode` example pointing an agentic coding CLI at the same managed LLM.

## Quick Start

Open the workspace in the pre-authenticated JupyterLab environment on NRP:

**[Launch CRA Tutorial Workspace](https://training.nrp-nautilus.io/hub/user-redirect/git-pull?repo=https%3A%2F%2Fgithub.com%2Fnrp-nautilus%2Fcra-tutorial&branch=main&urlpath=lab%2Ftree%2Fcra-tutorial%2F)**

The managed-LLM and RAG cells work on a **CPU-only** session; spawn with
**1 × NVIDIA-A10** only if you also want to run the local-LLM comparison.

## Path

Work through the materials in order:

1. [`1_intro.md`](1_intro.md) — Introduction and Access
2. [`2_inference.ipynb`](2_inference.ipynb) — Managed LLM → local LLM (Ollama) → simple RAG with Milvus
3. [`3_opencode.md`](3_opencode.md) — Agentic coding with `opencode`

Everything uses the **`nrp-training-k8s`** namespace and the NRP managed LLM at
`https://ellm.nrp-nautilus.io/v1`. The session pod already exports
`OPENAI_API_BASE` and `OPENAI_API_KEY`.

## 📋 Pre-training survey — please take 2 minutes before we start

<a href="images/pre-training-survey-qr.png"><img src="images/pre-training-survey-qr.png" alt="Pre-training survey QR code" width="180" align="right"></a>

Scan the QR on the right (or open [`https://ucsantacruz.co1.qualtrics.com/jfe/form/SV_3wQP0UrsPXy3nMO?Q_CHL=qr`](https://ucsantacruz.co1.qualtrics.com/jfe/form/SV_3wQP0UrsPXy3nMO?Q_CHL=qr)) to take the **pre-training survey** — a quick set of questions about your prior Kubernetes / NRP / AI experience. Comparing pre- and post-training responses is how we decide what to keep, cut, or rework for future cohorts.

<br clear="right">
