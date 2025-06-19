# WohnIQ-prompt-portfolio
Alle hier aufgeführten Prompts wurden eingesetzt, um die WohnIQ‑MVP‑Web‑App aufzubauen. Jede Sektion nennt das eingesetzte KI‑Tool und den exakten Wortlaut des Prompts.

---

## 1 · Design & Wireframing  (Figma AI)

```text
Generate a web‑app wireframe with:
• hero section titled *WohnIQ*
• search form: city, budget, size, ‘Meldepflicht’ toggle
• result table below the fold
• export buttons (CSV, PDF)
```

---

## 2 · Datenmodell  (ChatGPT Code‑Assistent)

```text
Create Python 3.12 Pydantic models for a flat listing:
id, title, address, city, warm_rent, cold_rent, size_m2, is_registration_required, url, scraped_at (datetime).
Include docstrings.
```

---

## 3 · API-Abfrage an Nestoria  (ChatGPT Code‑Assistent)

```text
Generate an async Python script that
• uses httpx (async) to call the Nestoria REST API at https://api.nestoria.de/api
• builds a GET request with action=search_listings&encoding=json
• accepts place_name (city), price_min / price_max and size_min / size_max as parameters
• paginates through page=1-3 (three requests) with await/async
• aggregates the "listings" array from every response
• validates each item against the given Pydantic model and returns one consolidated JSON list
• includes robust error handling for HTTP/network time-outs and unexpected API status codes

```

---

## 4 · Listing‑Parser / JSON Extractor  (ChatGPT Function Calling)

**System‑Prompt**

```text
You are a JSON extractor.
```

**User‑Prompt**

```text
Extract warm_rent, cold_rent, deposit, address, size_m2, availability_date from this German real‑estate description: ‹TEXT›.
Reply with strict JSON matching this schema: …
```

---

## 5 · Backend‑API  (ChatGPT Code‑Assistent)

```text
Create a minimal FastAPI app with these routes:
• POST /search accepts JSON → stores job id → triggers run_scraper(city, budget, size, meldepflicht) asynchronously (use asyncio.create_task).
• GET /results/{job_id} returns stored listings from PostgreSQL.
Provide Dockerfile and docker‑compose.yml with Postgres.
```

---

## 6 · Frontend Search Form  (GitHub Copilot Chat)

```text
Create a React component in TypeScript using shadcn/ui:
TextInput city, NumberInput budget, NumberInput size, Switch meldepflicht, Submit button.
On submit call /api/search, then poll /api/results/{job} and render a table.
```

### 6b · Polling & Table Render  (GitHub Copilot Chat)

```text
Poll /results/{job_id} every 2 s until length > 0, then render a table with columns title, warm_rent, size_m2, link.
```

---

## 7 · CSV‑Export  (ChatGPT Code‑Assistent)

```text
In FastAPI add GET /export/csv/{job_id} which:
• queries listings,
• uses pandas to convert to CSV in memory,
• returns text/csv response with Content‑Disposition: attachment.
```

---

## 8 · PDF‑Checkliste  (ChatGPT Code‑Assistent)

```text
Write a Python function generate_checklist(listings: List[Listing]) → bytes that:
• uses ReportLab to place title *WohnIQ Bewerbungs‑Checkliste*,
• lists each address + tick boxes (Personalausweis, Gehaltsnachweise, Schufa),
• returns PDF bytes.
```

---

## 9 · End‑to‑End‑Tests  (ChatGPT Code‑Assistent)

```text
Generate Playwright tests in tests/e2e that
• run the frontend,
• fill the search form with dummy data,
• wait for result rows,
• assert at least one export button is enabled.
```

---

## 10 · Deployment‑Konfiguration  (ChatGPT Code‑Assistent)

```text
Draft a fly.toml to deploy:
• one VM for FastAPI + Playwright workers (Docker image),
• one VM for Next.js (Edge‑compatible),
• Postgres addon.
Include GitHub Action that deploys on main.
```

---

## 11 · Tailwind Theme aus Design‑Tokens  (ChatGPT Code‑Assistent)

```text
Create a tailwind.config.js that imports design/tokens.json and maps tokens to
colors, fontSize and spacing under theme.extend.
```

---

## 12 · Docker Compose (Production)  (ChatGPT Code‑Assistent)

```text
Write docker-compose.prod.yml that starts
• db (Postgres 16),
• backend (build ./backend, env DATABASE_URL),
• frontend (build ./frontend) and exposes port 80.
```

---

> **Hinweis:** Bewahre dieses Prompt‑Portfolio im Repository (z. B. `/docs/PromptPortfolio.md`) auf, damit künftige Contributor exakt nachvollziehen können, welche KI‑Prompts die erste Version von WohnIQ generiert haben.
