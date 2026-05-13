# memory_decay

Tiny experiment repo for comparing **Mem0 memory decay OFF vs ON** by running the same query set against two Mem0 projects and exporting ranked results to `outputs/`.

## Setup

```bash
python3 -m venv .venv
./.venv/bin/pip install -r requirements.txt
cp .env.example .env
```

Fill in `.env`:

- **Single project**
  - `MEM0_API_KEY`
- **Two-project A/B mode**
  - `MEM0_API_KEY_OFF`
  - `MEM0_API_KEY_ON`
  - `MEM0_API_HOST` (defaults to `https://api.mem0.ai`)
- **Optional LLM judge (Azure OpenAI)**
  - `AZURE_OPENAI_ENDPOINT`
  - `AZURE_OPENAI_API_KEY`
  - `AZURE_OPENAI_API_VERSION`
  - `AZURE_OPENAI_DEPLOYMENT_GPT4O_MINI`
  - `AZURE_OPENAI_DEPLOYMENT_GPT5_MINI`

## Run

This project is intended to be run via the script `real_mem0_decay_ab.py`.

```bash
./.venv/bin/python real_mem0_decay_ab.py --mode two-project
```

If you have a different Python environment activated (e.g. conda), make sure you’re installing dependencies into the same interpreter you use to run the script.

## Outputs

Results are written under `outputs/` as both CSV and JSON. The included sample output has these columns/fields:

- **project**: `decay_off` or `decay_on`
- **case_id**: which query case was run (e.g. `breakfast`, `project`, `support`, `allergy`)
- **rank**: 1..K result rank
- **demo_id**: identifier of the seeded memory
- **score**: match score from the retrieval
- **memory**: returned memory text

Example (`outputs/mem0_decay_ab_*.csv`):

```csv
project,case_id,rank,demo_id,score,memory
decay_off,breakfast,1,breakfast_stale,0.3144,"Alice prefers a sesame bagel for breakfast on weekdays."
```

## Notes

- Treat `.env` as secret; don’t commit API keys.
- If you see `ModuleNotFoundError: No module named 'dotenv'`, install `python-dotenv` into the same environment you’re using to run the script (the repo uses `.venv` by default).
