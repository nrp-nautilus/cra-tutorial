# 3. Agentic coding with `opencode`

The notebook showed that anything speaking an OpenAI-compatible base URL works
against NRP. The same is true for **agentic** coding CLIs — tools that plan,
edit files, and run commands for you. Here we point
[`opencode`](https://opencode.ai) at the NRP managed LLM and have it write a
small program.

Run these in a **JupyterHub terminal** (not a notebook cell).

## Install `opencode`

Drops a binary in `~/.opencode/bin`, no `sudo` needed:

```bash
curl -fsSL https://opencode.ai/install | bash
export PATH="$HOME/.opencode/bin:$PATH"
opencode --version
```

## Point it at the NRP managed LLM

Uses the already-exported `OPENAI_API_KEY`:

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
        "gpt-oss": { "name": "GPT-OSS" },
        "qwen3":   { "name": "Qwen3"   },
        "gemma":   { "name": "Gemma"   }
      }
    }
  },
  "model": "nrp/gpt-oss"
}
JSON
```

## Run a small task

Start the agent in a clean directory:

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
