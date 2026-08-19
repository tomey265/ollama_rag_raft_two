# Low-RAM Ollama Configuration Guide

Honest, evidence-based results for RAM-saving Ollama settings — measured
with real before/after numbers on one specific low-RAM machine, not just
restated documentation.

## Test Machine

- OS: Fedora, KDE Plasma
- RAM: 13 GB total
- GPU: AMD Radeon 680M (integrated, no ROCm support — CPU-only inference)
- Model tested: `hf.co/tomey265/qwen-coder-my:latest` (Qwen2.5-Coder-14B-Instruct,
  Q3_K_M quant, 14.8B params, 32768 max context)
- Embedding model: `nomic-embed-text:latest`

## Method

Every setting was tested using the same fixed prompt, with swap cleared
before each run and `ollama ps` / `free -h` captured immediately after
generation. 2–3 runs per setting where possible. Full methodology and
raw results are in each file below.

## Results

| Setting | File | Outcome |
|---|---|---|
| `OLLAMA_FLASH_ATTENTION=1` | *(see repo history)* | No measurable difference — likely already active by default |
| `OLLAMA_KV_CACHE_TYPE=q8_0` | *(see repo history)* | No measurable difference at default context length |
| `OLLAMA_KV_CACHE_TYPE=q4_0` | [q4_0-results.md](./q4_0-results.md) | ~7% RAM reduction (8.3 GB → 7.7 GB) |
| `OLLAMA_CONTEXT_LENGTH` | [context-length-8000-results.md](./context-length-8000-results.md) | 8000 safe, 16000 crashes (confirmed OOM) |
| `OLLAMA_NUM_PARALLEL=2` | [num-parallel-results.md](./num-parallel-results.md) | 2 concurrent requests succeed, slower per-request |
| `OLLAMA_MAX_LOADED_MODELS=2` | [max-loaded-models-results.md](./max-loaded-models-results.md) | Generation + embedding models coexist cleanly |

## License

MIT
