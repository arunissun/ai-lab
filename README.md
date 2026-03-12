# AI Lab: Open WebUI + LiteLLM + Docker

This project runs a local AI workspace using:
- LiteLLM as a unified OpenAI-compatible gateway
- Open WebUI as the chat interface
- Docker Compose to run everything together

It lets you use many free-tier or low-cost models from different inference providers by adding provider API keys in your local `.env` file.

## Stack Overview

- Open WebUI is available at `http://localhost:3000`
- LiteLLM gateway is available at `http://localhost:4000/v1`
- SearXNG is available at `http://localhost:8888`

Flow:
1. You chat in Open WebUI.
2. Open WebUI sends requests to LiteLLM.
3. LiteLLM routes each request to the configured upstream model/provider (Groq, OpenRouter, NVIDIA NIM, and others).

## Why This Setup

- One consistent API surface (OpenAI-compatible)
- Easy provider switching without changing client apps
- Quick local setup for experimenting with multiple models
- Supports free model tiers exposed by provider APIs

## Files

- `docker-compose.yml`: service definitions for LiteLLM, Open WebUI, and SearXNG
- `litellm-config.yaml`: model catalog and routing configuration
- `.env.example`: sample environment variables (safe to commit)
- `.env`: your real secrets (ignored by git)

## Prerequisites

- Docker Desktop (or Docker Engine + Compose)
- Internet access for pulling images and calling provider APIs
- Provider API keys (at least one)

## Setup

1. Copy sample env file:

```bash
cp .env.example .env
```

On Windows PowerShell, you can use:

```powershell
Copy-Item .env.example .env
```

2. Edit `.env` and add your real keys.

Common keys used by this project:
- `GROQ_API_KEY`
- `OPENROUTER_API_KEY`
- `HF_API_KEY`
- `NVIDIA_NIM_API_KEY`
- `TOGETHER_API_KEY`
- `CEREBRAS_API_KEY`

3. Start services:

```bash
docker compose up -d
```

4. Open Open WebUI:

- `http://localhost:3000`

## Using Models in Open WebUI

1. Open WebUI connects to LiteLLM using `OPENAI_API_BASE_URL=http://litellm:4000/v1`.
2. The model list shown in Open WebUI is sourced from LiteLLM config.
3. Choose any configured model name from `litellm-config.yaml` (for example, `groq-llama-4-scout` or `OR-qwen3-coder-480b`).

## Free-Tier Notes

- Many OpenRouter entries in `litellm-config.yaml` use `:free` model variants.
- Availability and limits can change based on provider policy.
- Some providers enforce request-per-minute or token quotas.

## Security

- Do not commit `.env`.
- Keep keys local and rotate any leaked credentials immediately.
- This repository includes `.gitignore` rules to exclude `.env`.

## Logs and Troubleshooting

Check service status:

```bash
docker compose ps
```

Follow logs:

```bash
docker compose logs -f litellm
```

```bash
docker compose logs -f open-webui
```

Stop services:

```bash
docker compose down
```

If a model fails:
- Verify the corresponding API key exists in `.env`
- Confirm model/provider ID is valid in `litellm-config.yaml`
- Inspect LiteLLM logs for upstream error details

## Disclaimer

"Free" model access depends on third-party providers and can change without notice. Always check each provider's latest limits and terms.
