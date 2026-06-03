# 1. Introduction and Access

This tutorial runs AI workloads on the
[National Research Platform (NRP)](https://nrp.ai) — entirely inside a
JupyterHub session, with no pods or YAML to apply.

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

## Spawning with a GPU (optional)

The managed-LLM and RAG cells run fine on a **CPU-only** session. Spawn with
**1 × NVIDIA-A10** only if you also want to run the local-LLM comparison in
section 3 of the notebook.

**Next:** open [`2_inference.ipynb`](2_inference.ipynb) and run the cells top
to bottom.
