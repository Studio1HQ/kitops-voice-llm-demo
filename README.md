<div align="center">

<img src="https://raw.githubusercontent.com/Studio1HQ/Studio1HQ/main/assets/banners/kitops-voice-llm-demo.webp" alt="Voice AI Pipeline with KitOps and Jozu Hub: A self-hosted voice agent whose STT, LLM and TTS models ship as ModelKits" width="100%" />

<br />
<br />

# Voice AI Pipeline with KitOps and Jozu Hub

### A self-hosted voice agent whose STT, LLM and TTS models ship as ModelKits

A call centre voice agent (Faster-Whisper, Qwen3, Kokoro) where each model is a versioned KitOps ModelKit on Jozu Hub, with selective unpack, champion/challenger promotion and rollback scripts.

<br />

[![Built by Studio1](https://img.shields.io/badge/Built_by_Studio1-f97316?style=for-the-badge&logo=data%3Aimage%2Fsvg%2Bxml%3Bbase64%2CPHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAxNjAgMTYwIiBmaWxsPSJub25lIj48cGF0aCBmaWxsLXJ1bGU9ImV2ZW5vZGQiIGNsaXAtcnVsZT0iZXZlbm9kZCIgZD0iTTAgMzVDMCAxNi42ODA3IDE0LjA3NDQgMS42NDg0NCAzMiAwLjEyNjk1M1YxNDFINTEuMDUyM0M1Mi40MzA4IDEzNy4zNDQgNTQuNzQ0NiAxMzMuNjg5IDU3Ljk5MzggMTMwLjAzM0M2MS4xNDQ2IDEyNi41MDggNjQuNjg5MiAxMjMuNDQgNjguNjI3NyAxMjAuODI5QzcyLjU2NjIgMTE4LjM0OSA3Ni4zNTY5IDExNi43ODIgODAgMTE2LjEyOVY4Ni43NTQ0Qzc1LjQ3MDggODcuOTI5MiA3MS4zODQ2IDg5LjY5MTkgNjcuNzQxNSA5Mi4wNDE1QzY0IDk0LjUyMjUgNjAuNzUwOCA5Ny40NiA1Ny45OTM4IDEwMC44NTRDNTYuNTM0NSAxMDIuNzIgNTUuMjU0NSAxMDQuNjU5IDU0LjE1MzkgMTA2LjY3MVYwSDEyNUMxNDQuMzMgMCAxNjAgMTUuNjY5OSAxNjAgMzVWMTI1QzE2MCAxNDQuMzMgMTQ0LjMzIDE2MCAxMjUgMTYwSDEyNFYxOEgxMDQuOTQ4QzEwMy41NjkgMjEuNjgxNiAxMDEuMjU1IDI1LjM2MjggOTguMDA2MiAyOS4wNDQ0Qzk0Ljg1NTQgMzIuNTk0MiA5MS4zMTA4IDM1LjY4NDEgODcuMzcyMyAzOC4zMTRDODMuNDMzOCA0MC44MTIgNzkuNjQzMSA0Mi4zODk2IDc2IDQzLjA0NzRWNzIuNjMwNEM4MC41MjkyIDcxLjQ0NzMgODQuNjE1NCA2OS42NzI0IDg4LjI1ODUgNjcuMzA1N0M5MiA2NC44MDc2IDk1LjI0OTIgNjEuODQ5MSA5OC4wMDYyIDU4LjQzMDdDOTkuNDY1NSA1Ni41NTEzIDEwMC43NDUgNTQuNTk4NiAxMDEuODQ2IDUyLjU3MjNWMTYwSDM1QzE1LjY3IDE2MCAwIDE0NC4zMyAwIDEyNVYzNVoiIGZpbGw9IiNmZmZmZmYiLz48L3N2Zz4K)](https://studio1hq.com)
[![Built with Jozu](https://img.shields.io/badge/Built_with_Jozu-141414?style=for-the-badge)](https://jozu.com)

[Jozu docs](https://jozu.com/docs) &nbsp;·&nbsp; [Jozu](https://jozu.com) &nbsp;·&nbsp; [KitOps](https://kitops.org) &nbsp;·&nbsp; [More examples](https://github.com/Studio1HQ) &nbsp;·&nbsp; [studio1hq.com](https://studio1hq.com)

</div>

<br />

## Overview

A self-hosted call centre voice agent built from open-source models and managed with [KitOps](https://kitops.org) and [Jozu Hub](https://jozu.com).

```text
Phone -> STT (Faster-Whisper) -> LLM (Qwen3) -> TTS (Kokoro) -> Phone
```

A voice pipeline runs three models that update on different schedules. Without model management you can end up with the wrong version in production, no way to roll back and no audit trail. Here each model is packaged as a versioned ModelKit. Jozu Hub stores them with security scanning, signed attestations and policy gates.

## Features

- **Three ModelKits plus a meta kit**: STT, LLM and TTS are versioned and rolled back independently. The root `Kitfile` bundles everything for a one-step demo (the LLM is the `model:`, STT and TTS weights are `datasets:`).
- **Selective unpack**: `--filter` lets ops pull only weights, devs only code and prompts, and QA only test data.
- **Champion/challenger**: Push a new version, compare it with `kit diff`, promote it, and roll back with a script.
- **WebSocket server**: `pipeline/server.py` runs the STT, LLM, TTS chain.
- **Test client**: Talk through a microphone or type text.
- **Docker**: A Dockerfile and `docker-compose.yaml`.
- **Jozu Hub checks**: On push the README lists scanning with ModelScan, LLM Guard, Garak, Promptfoo and ART, Cosign attestations, an AI SBOM (SPDX 3) and OPA policy gates.

## Tech stack

| Layer | Tool |
| --- | --- |
| STT | [Faster-Whisper](https://github.com/SYSTRAN/faster-whisper) Small (~500MB, CTranslate2) |
| LLM | Qwen3-0.6B Q4_K_M (~400MB) via [llama-cpp-python](https://github.com/abetlen/llama-cpp-python) |
| TTS | [Kokoro](https://github.com/hexgrad/kokoro) (82M parameters, ~300MB) |
| Server | Python, `websockets`, PyYAML |
| Packaging | [KitOps](https://kitops.org) |
| Registry | [Jozu Hub](https://jozu.ml) |
| Deployment | Docker Compose |

## Getting started

### Prerequisites

- Python 3 and `pip`
- The [KitOps CLI](https://kitops.org/docs/cli/installation/) (for the ModelKit options)
- Docker (optional)
- A microphone for audio-mode testing (optional)

### Installation

```bash
git clone https://github.com/Studio1HQ/kitops-voice-llm-demo.git
cd kitops-voice-llm-demo
pip install -r requirements.txt
```

### Environment variables

None are read by the pipeline. Configuration lives in `pipeline/config.yaml` and each model's `config.yaml`.

### Run

Pick one way to get the models.

Option A, one meta ModelKit (demo):

```bash
kit pull jozu.ml/voice-ai/voice-pipeline:v1.0.0
kit unpack jozu.ml/voice-ai/voice-pipeline:v1.0.0 -d . -o
python pipeline/server.py
```

Option B, per-model ModelKits (production):

```bash
kit pull jozu.ml/voice-ai/voice-stt:v1.0.0
kit pull jozu.ml/voice-ai/voice-llm:v1.0.0
kit pull jozu.ml/voice-ai/voice-tts:v1.0.0
kit unpack jozu.ml/voice-ai/voice-stt:v1.0.0 -d ./models/stt --filter=model -o
kit unpack jozu.ml/voice-ai/voice-llm:v1.0.0 -d ./models/llm --filter=model --filter=prompts -o
kit unpack jozu.ml/voice-ai/voice-tts:v1.0.0 -d ./models/tts --filter=model -o
python pipeline/server.py
```

Option C, local development without KitOps:

```bash
./scripts/setup_models.sh
python pipeline/server.py
```

Docker: download the models first (`./scripts/setup_models.sh` or `kit unpack`), then `docker compose up`.

Test it:

```bash
python pipeline/ws_test_client.py          # audio mode, speak into your mic
python pipeline/ws_test_client.py --text   # text mode
```

## How it works

```text
models/stt|llm|tts/   Kitfile, config.yaml, src/*_service.py, weights/, test_data/ (LLM has prompts/, TTS has voice_profiles/)
pipeline/             voice_pipeline.py (STT -> LLM -> TTS), server.py (WebSocket), ws_test_client.py, config.yaml
scripts/              setup_models.sh, pack_and_push.sh, deploy.sh, promote_challenger.sh, rollback.sh
Kitfile               Meta ModelKit for the whole pipeline
```

Package and push:

```bash
kit pack ./models/stt -t jozu.ml/voice-ai/voice-stt:v1.0.0
kit pack ./models/llm -t jozu.ml/voice-ai/voice-llm:v1.0.0
kit pack ./models/tts -t jozu.ml/voice-ai/voice-tts:v1.0.0
kit push jozu.ml/voice-ai/voice-stt:v1.0.0
kit push jozu.ml/voice-ai/voice-llm:v1.0.0
kit push jozu.ml/voice-ai/voice-tts:v1.0.0
# or: VERSION=v1.0.0 ./scripts/pack_and_push.sh
```

Role-based unpack:

```bash
kit unpack jozu.ml/voice-ai/voice-llm:v1.0.0 --filter=model                    # ops
kit unpack jozu.ml/voice-ai/voice-llm:v1.0.0 --filter=code --filter=prompts    # dev
kit unpack jozu.ml/voice-ai/voice-llm:v1.0.0 --filter=datasets                 # QA
```

Champion and challenger:

```bash
kit pack ./models/tts -t jozu.ml/voice-ai/voice-tts:v2.0.0
kit push jozu.ml/voice-ai/voice-tts:v2.0.0
kit diff jozu.ml/voice-ai/voice-tts:v1.0.0 jozu.ml/voice-ai/voice-tts:v2.0.0
./scripts/promote_challenger.sh voice-tts v2.0.0
./scripts/rollback.sh voice-tts v1.0.0
```

Three individual ModelKits are the production pattern: each model versions independently, you can change the TTS voice without re-pushing the LLM, you roll back only the broken model, and scan results are per model. The meta ModelKit is a demo convenience because a Kitfile supports one model per artifact.

## Resources

- [KitOps documentation](https://kitops.org/docs/overview/): ModelKit and Kitfile reference.
- [Jozu documentation](https://jozu.com/docs): Jozu Hub guides.
- [Jozu](https://jozu.com): Product site.

<br />

---

<div align="center">

<a href="https://studio1hq.com">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Studio1HQ/website/main/brand/studio1-logo-on-dark.svg" />
    <img src="https://raw.githubusercontent.com/Studio1HQ/website/main/brand/studio1-logo-on-light.svg" alt="Studio1" width="160" />
  </picture>
</a>

Built by **[Studio1](https://studio1hq.com)**, the technical content and DevRel partner for devtool teams. Made with [Jozu](https://jozu.com).<br />
Browse more open-source examples at [github.com/Studio1HQ](https://github.com/Studio1HQ).

[![Website](https://img.shields.io/badge/studio1hq.com-0b0807?style=for-the-badge&logo=googlechrome&logoColor=white)](https://studio1hq.com)
[![Book a call](https://img.shields.io/badge/Book_a_strategy_call-f97316?style=for-the-badge&logo=googlecalendar&logoColor=white)](https://cal.com/studio1/collab)
[![X](https://img.shields.io/badge/%40Studio1HQ-141414?style=for-the-badge&logo=x&logoColor=white)](https://x.com/Studio1HQ)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0a66c2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/company/studio1hq/)

<sub><a href="https://studio1hq.com">studio1hq.com</a> · Technical content, docs, video and DevRel for devtools</sub>

</div>
