# AGENTS.md

Talk Python course repo, not one app. Each `chNN-*` dir is a standalone snapshot of the same project at a later stage (ch05 -> ch06 -> ch07 -> ch08 are cumulative copies). No tests, no CI.

- Edit only the chapter the task targets; don't propagate changes across chapters unless asked. Duplicated code is intentional.
- Each chapter has its own `requirements.piptools` (inputs) and `requirements.txt` (pinned). Root ones are a union. Regenerate pins with pip-compile, don't hand-edit `requirements.txt`.
- Lint/format: root `ruff.toml` (line length 120, **single quotes**, rules E/F, py311). Run `ruff check` / `ruff format` from the repo root.

## Running apps
- ch03, ch05-ch08: run from inside the chapter dir: `python main.py` (uvicorn on 127.0.0.1:8000). Paths such as `settings.json`, `static/`, and `templates/` are relative to cwd.
- ch05+ need `settings.json` (gitignored) copied from `settings_template.json` with an OpenWeatherMap `api_key`; the app raises at startup without it.
- `configure()` in `main.py` runs on both `__main__` and import, so `uvicorn main:api` also works.
- ch04 is loose standalone scripts (asgi, async scrape, pydantic models, types); run each file directly. The scrape dirs have their own requirements.

## Layout (ch05+)
`main.py` wires routers/static/settings; `api/` = JSON routes (APIRouter), `views/` = HTML pages (Jinja `templates/`), `services/` = OpenWeather client, `models/` = pydantic. ch06+ add `infrastructure/weather_cache.py`; ch07+ add report service and `bin/reportapp.py`; ch08 adds `server/` (nginx, systemd units, scripts) for gunicorn+uvicorn on Ubuntu.
