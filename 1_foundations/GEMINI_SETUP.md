# Gemini Setup and Key Check

This note documents the Gemini-related setup so it stays consistent with `agents/pyproject.toml`.

## What was changed

- Updated the API key check cell in `agents/1_foundations/1_lab1.ipynb` to:
- Read `GEMINI_API_KEY` safely (`strip()` and empty fallback).
- Fail fast with a clear `EnvironmentError` when missing.
- Avoid printing the full key.
- Updated `agents/1_foundations/requirements.txt` to include:
- `google-genai`
- `ipykernel`

## Why this does not conflict with `pyproject.toml`

- `agents/pyproject.toml` already declares:
- `google-genai>=1.64.0`
- `python-dotenv>=1.0.1`
- `ipykernel>=6.29.5` (in `dev` group)
- The notebook now relies on those same packages and key names (`GEMINI_API_KEY`), so behavior is aligned across dependency files.

## Expected environment variable

Set this in your `.env` (project root) or system environment:

```env
GEMINI_API_KEY=your_real_key_here
```

## Notebook key-check cell (current)

```python
# Check the key used for this notebook.
# This expects GEMINI_API_KEY to be present after load_dotenv(...) in an earlier cell.

import os

gemini_api_key = (os.getenv("GEMINI_API_KEY") or "").strip()

if gemini_api_key:
    print(f"GEMINI_API_KEY is set (prefix: {gemini_api_key[:8]}...)")
else:
    raise EnvironmentError(
        "GEMINI_API_KEY is not set. Add it to .env (project root) or your system environment."
    )
```

## Install options

Use one approach per environment:

1. `pyproject.toml` flow (recommended in this repo):

```powershell
cd agents
uv sync
```

2. `requirements.txt` flow (for `1_foundations` only):

```powershell
cd agents/1_foundations
python -m pip install -r requirements.txt
```

## Quick verification

```powershell
python -c "from google import genai; print('google-genai import ok')"
```
