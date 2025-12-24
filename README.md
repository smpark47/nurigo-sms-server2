# Nurigo/Solapi SMS Proxy (Flask)

A small Flask server that either:
- forwards `/api/sms` and `/api/sms/bulk` payloads to another server (`FORWARD_URL`), or
- sends SMS directly via Solapi (`SOLAPI_KEY` + `SOLAPI_SECRET`), or
- returns a mock response if neither is set.

## Local run

```bash
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS/Linux
# source .venv/bin/activate

pip install -r requirements.txt
python nurigo_server_fixed.py
```

Open:
- `http://localhost:10000/`
- `http://localhost:10000/ui`

## Deploy on Render (Web Service)

**Build command**
```bash
pip install -r requirements.txt
```

**Start command (recommended)**
```bash
gunicorn -w 2 -b 0.0.0.0:$PORT app:app
```

## Environment variables

- `PORT` : Provided by Render (don’t set manually on Render)
- `DEFAULT_SENDER` : Default `from` number for SMS (digits only recommended)
- `AUTH_TOKEN` : If set, API requests must include `Authorization: Bearer <token>`
- `FORWARD_URL` : If set, proxy-forwards requests to this URL
- `SOLAPI_KEY`, `SOLAPI_SECRET` : Solapi credentials (used when `FORWARD_URL` is not set)
- **Storage (recommended on Render)**  
  - `STORAGE_DIR` : Base directory for runtime files (`data/`, `logs/`)  
  - `DATA_DIR` / `LOG_DIR` : Optional overrides

### Runtime files

- `data/roster.json` : roster data
- `logs/sms_log.jsonl` : send logs

These directories are ignored by git (`.gitignore`).  
If you need persistence on Render, attach a **Persistent Disk** and set `STORAGE_DIR` to the disk mount path.
