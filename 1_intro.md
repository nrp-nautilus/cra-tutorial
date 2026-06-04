# 1. Introduction and Access

This tutorial runs on the
[National Research Platform (NRP)](https://nrp.ai) from a JupyterHub session:
first you'll **use** NRP's managed AI services (LLM inference and RAG), then
you'll **deploy your own** JupyterHub on the cluster with Helm.

## What NRP is

NRP is a shared national cyberinfrastructure built on the Nautilus Kubernetes
cluster. It provides hundreds of GPU nodes, shared storage, and managed
services such as JupyterHub, S3, a vector database (Milvus), and an
OpenAI-compatible **managed LLM endpoint** — all free for U.S. academic
research, teaching, and outreach.

A quick mental model:

1. **CILogon** signs you in through your institution's identity provider.
2. **JupyterHub** gives you a browser-based JupyterLab workspace and terminal.
3. **Kubernetes namespaces** isolate each class or project's workloads.
4. **Managed services** (LLM, Milvus, …) mean you call an API instead of
   running and paying for your own servers.

## Log in

Open the workspace link from the [README](README.md) and sign in through
CILogon. The training JupyterHub is the easiest path because `kubectl` and the
LLM environment variables are already wired up for you.

## Confirm your environment

Open a **terminal** from the JupyterLab launcher and run:

```bash
cd ~/cra-tutorial
echo "$OPENAI_API_BASE"                       # the managed LLM endpoint
kubectl auth can-i list pods -n nrp-training-k8s
```

You should see the endpoint URL and a `yes`. That's everything the notebook
needs.

## A CPU-only session is enough

Everything here — managed LLM inference, RAG, and deploying a JupyterHub with
Helm — runs on a **CPU-only** session. The spawn-form defaults (1 core / 8 GB)
are fine; you do **not** need a GPU.

## For Part 3: your namespace

Part 3 deploys a JupyterHub into a namespace reserved for you
(`nrp-training-000` … `nrp-training-099`) — use the one on the slip you were
handed. Your session's service account can already deploy there.

**Next:** open [`2_inference.ipynb`](2_inference.ipynb) and run the cells top
to bottom.
