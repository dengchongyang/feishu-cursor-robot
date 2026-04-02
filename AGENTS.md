# AGENTS.md

## Cursor Cloud specific instructions

### Overview

This is a **Feishu (Lark) Bot + Cursor Cloud Agent bridge service** — a pure Python application with no database, no Docker, and no build step. It connects a Feishu chatbot to the Cursor Cloud Agent API via WebSocket long-connection.

### Running the app

```bash
python main.py
```

The app requires a valid `.env` file with real Feishu and Cursor credentials to connect. See `.env.example` for required variables. Without valid `FEISHU_APP_ID`/`FEISHU_APP_SECRET`, the WebSocket connection will fail with `app_id is invalid`.

### Linting

No lint config is committed in the repo. You can use `ruff check .` for quick linting. Pre-existing warnings (1 unused import in `feishu/client.py`, 2 bare `except` in `feishu/message_parser.py`) are part of the original codebase.

### Testing

No automated test suite exists. Verification is done via module imports and running `python main.py`. Full end-to-end testing requires valid Feishu app credentials and a Cursor API key.

### Key gotchas

- The `config/settings.py` module instantiates `Settings()` at import time, which reads `.env`. A `.env` file must exist with at least `FEISHU_APP_ID`, `FEISHU_APP_SECRET`, and `CURSOR_API_KEY` set (even placeholder values) or the import will fail with a validation error.
- Python 3.10+ is required (uses `str | None` union syntax and `zoneinfo`).
- All state is in-memory — no persistence layer.
- The app uses `pip` with `requirements.txt` (no pyproject.toml, poetry, or conda).
