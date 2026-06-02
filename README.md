# CRA Tutorial — Hands-on AI on the National Research Platform

A slimmed-down, single-notebook tour of running AI on [NRP](https://nrp.ai):
calling the managed LLM, running a local LLM on a session GPU, and building a
simple RAG pipeline — all **inside a JupyterHub session**, no pods or YAML to
apply. It ends with a short `opencode` example so you can see an agentic coding
CLI pointed at the same managed LLM.

## Quick Start

Open the workspace in the pre-authenticated JupyterLab environment on NRP:

**[Launch CRA Tutorial Workspace](https://training.nrp-nautilus.io/hub/user-redirect/git-pull?repo=https%3A%2F%2Fgithub.com%2Fnrp-nautilus%2Fcra-tutorial&branch=main&urlpath=lab%2Ftree%2Fcra-tutorial%2F)**

Then open [`inference.ipynb`](inference.ipynb) and run the cells top to bottom.
The managed-LLM and RAG cells work on a **CPU-only** session; spawn with
**1 × NVIDIA-A10** only if you also want to run the local-LLM comparison.

## 📋 Pre-training survey — please take 2 minutes before we start

<a href="images/pre-training-survey-qr.png"><img src="images/pre-training-survey-qr.png" alt="Pre-training survey QR code" width="180" align="right"></a>

Scan the QR on the right (or open [`https://ucsantacruz.co1.qualtrics.com/jfe/form/SV_3wQP0UrsPXy3nMO?Q_CHL=qr`](https://ucsantacruz.co1.qualtrics.com/jfe/form/SV_3wQP0UrsPXy3nMO?Q_CHL=qr)) to take the **pre-training survey** — a quick set of questions about your prior Kubernetes / NRP / AI experience. Comparing pre- and post-training responses is how we decide what to keep, cut, or rework for future cohorts.

<br clear="right">

## What's in here

| File | What it covers |
|---|---|
| [`inference.ipynb`](inference.ipynb) | Managed LLM → local LLM (Ollama) → simple RAG with Milvus |
| This `README` | A short `opencode` agentic example (below) |

Everything uses the **`nrp-training-k8s`** namespace and the NRP managed LLM at
`https://ellm.nrp-nautilus.io/v1`. The session pod already exports
`OPENAI_API_BASE` and `OPENAI_API_KEY`.

## Bonus: agentic coding with `opencode`

The notebook shows that anything speaking an OpenAI-compatible base URL works
against NRP. The same is true for **agentic** coding CLIs — tools that plan,
edit files, and run commands for you. Here we point
[`opencode`](https://opencode.ai) at the NRP managed LLM and have it write a
small program. Run these in a **JupyterHub terminal** (not a notebook cell).

**1. Install `opencode`** (drops a binary in `~/.opencode/bin`, no `sudo`):

```bash
curl -fsSL https://opencode.ai/install | bash
export PATH="$HOME/.opencode/bin:$PATH"
opencode --version
```

**2. Point it at the NRP managed LLM** using the already-exported `OPENAI_API_KEY`:

```bash
mkdir -p ~/.config/opencode
cat > ~/.config/opencode/opencode.json <<'JSON'
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "nrp": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "NRP LLM",
      "options": {
        "baseURL": "https://ellm.nrp-nautilus.io/v1",
        "apiKey": "{env:OPENAI_API_KEY}"
      },
      "models": {
        "gpt-oss":    { "name": "GPT-OSS"    },
        "qwen3":      { "name": "Qwen3"       },
        "gemma":      { "name": "Gemma"       }
      }
    }
  },
  "model": "nrp/gpt-oss"
}
JSON
```

**3. Run a small task.** Start the agent in a clean directory:

```bash
mkdir -p ~/opencode-demo && cd ~/opencode-demo
opencode
```

Inside the `opencode` TUI, press `/` to open the prompt and paste:

```text
Write a single-file Python program fizzbuzz.py that prints the numbers 1 to 100,
but prints "Fizz" for multiples of 3, "Buzz" for multiples of 5, and "FizzBuzz"
for multiples of both. Add a top-of-file docstring, and after writing the file
tell me the exact command to run it.
```

`opencode` plans, writes `fizzbuzz.py`, and prints the run command:

```bash
python fizzbuzz.py
```

Switch models in-session with `Ctrl+P → Switch models` and try the same prompt
against `qwen3` or `gemma` — same agent, same prompt, different inference
backend. The teaching point: you bring the workflow you already use; NRP
supplies the inference.

Reference: [client configs](https://nrp.ai/documentation/userdocs/ai/llm-managed/client-configs/).
