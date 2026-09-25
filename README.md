# ComicCraft — AI Comic Story Creator

Complete FastAPI + Jinja2 implementation of the supplied ComicCraft documentation. The workflow is: Gemini Flash outline → Gemini Pro narration/dialogue → Hugging Face image generation → layout builder → PDF export → browser preview.

## Folder structure

```text
ComicCraft/
├── app/
│   ├── main.py config.py models.py routes.py
│   └── services/ gemini_flash.py gemini_pro.py image_generator.py layout_builder.py exporters.py
├── templates/ base.html index.html comic_preview.html export_success.html
├── static/ css/style.css js/app.js panels/ exports/
├── tests/test_app.py
├── .env.example .gitignore requirements.txt README.md
```

## VS Code setup (Windows)

```powershell
cd ComicCraft
python -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
Copy-Item .env.example .env
uvicorn app.main:app --reload
```

If PowerShell blocks activation:

```powershell
Set-ExecutionPolicy -Scope Process Bypass
.venv\Scripts\Activate.ps1
```

Open http://127.0.0.1:8000 and http://127.0.0.1:8000/docs.

## Demo mode

Keep `DEMO_MODE=true` in `.env`. This runs the entire website, five-panel workflow and PDF export without external AI keys. Demo images are generated locally as placeholders.

## Real AI mode

Set:

```env
DEMO_MODE=false
GEMINI_API_KEY=your_gemini_key
HF_TOKEN=hf_your_token
GEMINI_FLASH_MODEL=gemini-2.5-flash
GEMINI_PRO_MODEL=gemini-2.5-pro
HF_PROVIDER=hf-inference
HF_IMAGE_MODEL=stabilityai/stable-diffusion-3.5-large-turbo
```

Model IDs are configurable because model availability can change. The original documentation names Gemini 1.5 Flash/Pro and `runwayml/stable-diffusion-v1-5`; those can be supplied in `.env` when available to your accounts/providers.

## API

`GET /health` — configuration/status check.

`POST /generate` — browser form workflow.

`POST /generate-comic/json` — JSON API with `story_prompt`, `character_name`, `setting`, `tone`, `art_style`.

`POST /test-image` — image-generation test with `{ "prompt": "..." }`.

## Tests

```powershell
pytest -q
```

Tests use demo mode, so no external AI service is needed.

## Notes

The supplied project documentation describes FastAPI, Jinja2, Gemini Flash/Pro, Stable Diffusion, FPDF, a five-panel outline, panel preview, PDF export and the `/generate-comic/json` and `/test-image` routes. This implementation includes those components and adds a demo mode so the application can be verified locally before API keys are configured.
