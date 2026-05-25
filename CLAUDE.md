# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A simple FastAPI REST API (Spanish-language domain: "platos" / dishes). Uses an in-memory dict as a simulated database — no real DB or ORM.

## Commands

- **Run the server:** `python main.py` (starts uvicorn with reload on host/port from settings)
- **Run with uvicorn directly:** `uvicorn main:app --reload`
- **Run tests:** `pytest`
- **Lint:** `black . && isort .`
- **Type check:** `mypy main.py schemas.py settings.py`

## Architecture

Single-module layout — all code lives at the project root:

- `main.py` — FastAPI app, CORS config, all route handlers (root, health, items, CRUD `/platos/`)
- `schemas.py` — Pydantic model `Plato` (id, name, precio)
- `settings.py` — App config via `pydantic-settings` (reads from env vars / `.env`)

The `/platos/` endpoints implement full CRUD (POST, GET list with pagination, GET by id, PUT, PATCH, DELETE) against `platos_db: Dict[int, Plato]` — an in-memory dict seeded with 3 dishes.

## Key Conventions

- Build system: hatchling (`pyproject.toml`)
- Python >=3.8, Pydantic v2, pydantic-settings v2
- Formatting: black (line-length 88) + isort (black profile)
- Type checking: mypy with strict settings (`disallow_untyped_defs`, `disallow_incomplete_defs`)
- Route parameters use `Annotated` type hints with `Path`, `Query`, `Body` for OpenAPI docs

## Cursor Rules

From `.cursorrules` and `.cursor/rules/functions.mdc`:
- All functions must have a docstring
- Functions must use type hints for all parameters and return values
