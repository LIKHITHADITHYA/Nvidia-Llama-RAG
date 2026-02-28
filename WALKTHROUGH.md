# Walkthrough — Nvidia-Llama-RAG

This project is a Jupyter notebook prototype for a hybrid Retrieval-Augmented Generation (RAG) system that:
- connects to an OpenAI/NVIDIA-compatible LLM client,
- performs real-time searches via SerpAPI,
- builds a `hybrid_rag_system` combining search results + LLM, and
- includes an interactive Gradio UI to compare RAG vs a baseline LLM.

This file explains how to set up, fix the small API key issue, and run the notebook locally.

## Files
- `RAG.ipynb` — main notebook (open this in Jupyter/Colab).

## Prerequisites
- macOS or Linux, Python 3.10+ recommended.
- A Python virtual environment (recommended):

```bash
python3 -m venv .venv
source .venv/bin/activate
```

## Install dependencies

Install the main libraries used in the notebook:

```bash
pip install -U pip
pip install openai langchain gradio langchain-community numpy requests
```

Note: package names and versions may change over time. If you hit import errors, try `pip install langchain-community` or check the import path shown in the notebook.

## Required API keys (do NOT commit these)

- `NV_API_KEY` — API key for the OpenAI/NVIDIA-compatible client used in the notebook.
- `SERPAPI_API_KEY` — SerpAPI key for real-time search.

Export them in your zsh terminal before launching Jupyter or the notebook runtime:

```bash
export NV_API_KEY="your-nv-api-key-here"
export SERPAPI_API_KEY="your-serpapi-key-here"
```

## Quick fix for the notebook error (missing `api_key`)

The notebook currently contains an incomplete line `api_key =` (syntax error). Replace the client init block with a secure env-var-based snippet.

Replace the existing lines with:

```python
from openai import OpenAI
import os

client = OpenAI(
    base_url="https://integrate.api.nvidia.com/v1",
    api_key=os.environ.get("NV_API_KEY")
)
```

This avoids hardcoding secrets and resolves the syntax error seen in the notebook.

## How to open and run the notebook

1. Start Jupyter Lab / Notebook in the project directory:

```bash
jupyter lab
# or
jupyter notebook
```

2. Open `RAG.ipynb` and run cells from top to bottom. The notebook is organized into steps — install/import, search wrapper, RAG function, experiments, and Gradio UI. Run sequentially.

## Running the Gradio demo (locally)

If you reach the Gradio portion, run that cell. On a local machine you may need to set `share=False` in `demo.launch()`; in Colab the notebook auto-enables `share=True`.

Example: open a terminal and run the notebook (or execute the Gradio cell). If you prefer a standalone script, extract the Gradio code into `app.py` and run:

```bash
python app.py
```

## Troubleshooting

- ModuleNotFoundError: `langchain_community` — run `pip install langchain-community`.
- SerpAPI errors — ensure `SERPAPI_API_KEY` is exported correctly and that your network allows outgoing requests.
- If the Gradio app shows a share link that expires (Colab), deploy to Hugging Face Spaces or run locally for persistent hosting.

## Security

- Never commit API keys to git. Use environment variables or a secrets manager.

## Next steps / suggestions

- Optionally create a `requirements.txt` for reproducible env.
- Add a small wrapper script that loads env variables and launches Jupyter with the correct env.

If you want, I can:
- add a `requirements.txt`, or
- patch `RAG.ipynb` to replace the incomplete `api_key` line automatically.

---
Created to make the notebook easier to run and understand. Tell me which follow-up you'd like.
