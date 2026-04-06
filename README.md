
# Open-Source LLM with RAG

A demonstration project that shows how to build a hybrid Retrieval-Augmented Generation (RAG) system using an OpenAI-compatible base model client (example uses an NVIDIA-compatible client in the notebook) combined with real-time web search via SerpApi. The repository's main artifact is `RAG.ipynb`, which walks through installing dependencies, wiring up SerpApi, building a hybrid RAG flow, a simple chatbot interface, caching/fallback strategies, and example evaluations.

> NOTE: This repository contains example notebook code for demonstration and experimentation. Do NOT commit your real API keys to the repo. Always use environment variables or secret managers.

## Highlights / Features
- Real-time data search using SerpApi to ground model answers.
- Hybrid RAG pipeline that combines retrieval (SerpApi) and generation (base LLM).
- Basic interactive chatbot interface (Jupyter/Colab).
- Caching for search results to improve speed and reduce quota usage.
- Fallback handling when real-time search fails.
- Notebook-oriented: easy to run in Google Colab or locally.

## Files
- `HYBRID RAG.ipynb` — Main notebook demonstrating setup, real-time search integration, the hybrid RAG system, chatbot, caching, and example tests.
- `app.py` — Standalone script launching the Gradio chatbot web interface directly.

## Requirements
- Python 3.8+
- Recommended pip packages (create `requirements.txt` or install individually):
  - langchain
  - langchain-community (or the current package offering SerpAPI integration)
  - openai (or the SDK you use to access your base model — the notebook uses an OpenAI-compatible client)
  - requests
  - jupyter / ipykernel (if running locally)
  - functools (part of stdlib)

Example requirements (put in `requirements.txt`):
```text
langchain
langchain-community
openai
requests
jupyter
ipykernel
```

(Adjust package names/versions to match your environment. If `langchain-community` is unavailable by that name, the notebook may require a package or import path that matches your langchain extensions — check the latest langchain docs.)

## Environment variables / Secrets
Before running the notebook, set these environment variables (or configure secrets in Colab):
- `SERPAPI_API_KEY` — Your SerpApi API key (https://serpapi.com/)
- `NV_API_KEY` or the appropriate API key variable for your base model client (the notebook uses an NVIDIA endpoint in examples). If you use OpenAI, set `OPENAI_API_KEY`.

Important: remove any hard-coded API keys from the code and replace them with environment variables (e.g., `os.environ["SERPAPI_API_KEY"] = ...` or use Colab secrets).

## Usage

### Google Colab
1. Open the notebook via the Colab link at the top of `HYBRID RAG.ipynb` or open in Colab:
   [Open in Colab](https://colab.research.google.com/github/LIKHITHADITHYA/Nvidia-Llama-RAG/blob/main/HYBRID%20RAG.ipynb)
2. Set your API keys using Colab's UI or at the top of the notebook:
   ```python
   import os
   os.environ["SERPAPI_API_KEY"] = "<your_serpapi_key>"
   os.environ["OPENAI_API_KEY"] = "<your_openai_key>"  # or NV_API_KEY if applicable
   ```
3. Run cells sequentially. Follow the notebook instructions and remove any example/hard-coded keys first.

### Locally (Jupyter)
1. Clone the repository:
   ```bash
   git clone https://github.com/LIKHITHADITHYA/Open-Source-llm-with-RAG-.git
   cd Open-Source-llm-with-RAG-
   ```
2. Install dependencies:
   ```bash
   python -m pip install -r requirements.txt
   ```
3. Start Jupyter:
   ```bash
   jupyter notebook
   ```
4. Rename `.env.example` to `.env` and fill in your API keys.
5. Open `HYBRID RAG.ipynb` or run the standalone UI:
   ```bash
   python app.py
   ```

## Typical workflow in the notebook
1. Install and import dependencies (langchain and any wrappers).
2. Provide API keys via env variables (do not hard-code).
3. Instantiate SerpApi wrapper and a base model client.
4. Implement `get_realtime_search_results(query)` — includes parsing and optional caching.
5. Implement `hybrid_rag_system(query)` — calls search function and sends a combined prompt to the model.
6. Optionally run an interactive `start_chatbot()` that uses the hybrid system.
7. Evaluate results and tune prompts, caching, and fallback behavior.

## Caching & Robustness
The notebook demonstrates using `functools.lru_cache` for simple in-process caching. For production or multi-process deployments consider:
- Redis or Memcached for shared caching.
- Persistent vector DB (FAISS, Milvus) for storing retrieved context and reuse.
- Circuit-breakers and retries for API robustness.

## Security and Privacy
- Never commit API keys to the repository.
- Sanitize logs and outputs before sharing.
- If you store or index external content, make sure you comply with copyright and privacy policies.

## Contributing
Contributions are welcome. Suggested contribution workflow:
1. Fork the repo.
2. Create a branch with your change: `git checkout -b feature/your-change`
3. Commit and push: `git commit -am "Add ..." && git push origin feature/your-change`
4. Open a Pull Request describing your changes.

## Example: Add README to the repo locally
If you want to add this README file to the repository locally and push it:

```bash
# from the repository root
git checkout -b docs/add-readme
# create README.md with content above (save it)
git add README.md
git commit -m "Add README with usage and setup instructions"
git push origin docs/add-readme
# Open a Pull Request on GitHub from the 'docs/add-readme' branch
```

## Troubleshooting
- ModuleNotFoundError: if `langchain_community` import fails, ensure the right package/version is installed and check the import path; langchain extensions change frequently.
- If SerpApi returns unexpected formats, inspect raw responses and adapt the parsing logic accordingly.
- If the base model API returns streaming output or different response shapes, adjust extraction code to match the client SDK.

## License
Specify your license here (e.g., MIT). Example:
```
MIT License
```

## Contact
If you have questions, open an issue in the repo or contact the maintainer (LIKHITHADITHYA) via GitHub.
