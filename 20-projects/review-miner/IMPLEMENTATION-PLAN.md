# review-miner Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a product-agnostic Python CLI that collects competitor reviews from App Store, Google Play, Reddit, G2, and Capterra into one normalized CSV + markdown run-summary for manual analysis.

**Architecture:** A Typer CLI runs an adaptive `questionary` wizard (answers validated by pydantic and saved to a per-project YAML), then dispatches to isolated per-source *connectors* behind one ABC. Each connector fetches raw data and maps it onto a single `Review` schema; the orchestrator dedupes, writes outputs, and never lets one failing source kill the run. Network/browser code is isolated from pure logic so the whole pipeline is unit-testable without live calls.

**Tech Stack:** Python 3.11+, uv, Typer, questionary, pydantic v2, PyYAML, pandas, app-store-scraper, google-play-scraper, PRAW, Playwright, python-dotenv, pytest.

## Global Constraints

- Python 3.11+ only.
- Package name: `reviewminer`. CLI entry point: `review-miner = reviewminer.cli:app`.
- Tool root is the project folder: `20-projects/review-miner/`. All paths below are relative to it.
- One normalized schema everywhere — CSV columns, in exact order: `product, source, review_id, date, rating, title, body, author, url, helpful_count, language, scraped_at`.
- Valid source names (lowercase): `appstore, googleplay, reddit, g2, capterra`.
- A connector's `fetch` MUST NOT raise to the orchestrator: it catches its own errors, logs a warning, and returns the reviews it has (possibly `[]`).
- Default `output_dir` = `20-projects/<project>/research/review-mining/`.
- Output filenames: `reviews_<YYYY-MM-DD>.csv` and `run-summary_<YYYY-MM-DD>.md`.
- Reddit credentials come from env vars only (`REDDIT_CLIENT_ID`, `REDDIT_CLIENT_SECRET`, `REDDIT_USER_AGENT`) — never stored in project config.
- No analysis/sentiment/clustering. Collection only (PRD §8).
- TDD: write the failing test first, watch it fail, implement minimally, watch it pass, commit. No live network/browser in CI tests.
- Run tests with: `uv run pytest`.

## File Structure

```
20-projects/review-miner/
├── pyproject.toml                 # package metadata, deps, entry point (Task 1)
├── .env.example                   # Reddit creds template (Task 1)
├── .gitignore                     # venv, __pycache__, .env, output (Task 1)
├── README.md                      # already exists; usage appended in Task 14
├── reviewminer/
│   ├── __init__.py                # version (Task 1)
│   ├── models.py                  # Review schema + CSV_COLUMNS (Task 2)
│   ├── config.py                  # pydantic config models + load/save (Task 3)
│   ├── normalize.py               # raw dict -> Review per source (Task 4)
│   ├── dedupe.py                  # drop duplicate reviews (Task 5)
│   ├── output.py                  # write CSV + run-summary md (Task 6)
│   ├── collect.py                 # orchestrate connectors → statuses (Task 13)
│   ├── wizard.py                  # adaptive prompts + build_run_config (Task 12)
│   ├── cli.py                     # Typer `run` command (Task 14)
│   └── connectors/
│       ├── __init__.py            # registry: name -> Connector (Task 7)
│       ├── base.py                # Connector ABC + CollectStatus (Task 7)
│       ├── appstore.py            # app-store-scraper (Task 8)
│       ├── googleplay.py          # google-play-scraper (Task 9)
│       ├── reddit.py              # PRAW (Task 10)
│       ├── g2.py                  # Playwright + parse_g2_html (Task 11)
│       └── capterra.py            # Playwright + parse_capterra_html (Task 11)
├── config/                        # generated per-project YAML (gitignored content)
└── tests/
    ├── conftest.py                # shared fixtures (Task 2)
    ├── fixtures/                  # saved HTML/JSON for connector tests
    ├── test_models.py             # (Task 2)
    ├── test_config.py             # (Task 3)
    ├── test_normalize.py          # (Task 4)
    ├── test_dedupe.py             # (Task 5)
    ├── test_output.py             # (Task 6)
    ├── test_connectors_base.py    # (Task 7)
    ├── test_connector_appstore.py # (Task 8)
    ├── test_connector_googleplay.py # (Task 9)
    ├── test_connector_reddit.py   # (Task 10)
    ├── test_connector_g2.py       # (Task 11)
    ├── test_connector_capterra.py # (Task 11)
    ├── test_wizard.py             # (Task 12)
    ├── test_collect.py            # (Task 13)
    └── test_cli.py                # (Task 14)
```

---

### Task 1: Project scaffolding

**Files:**
- Create: `pyproject.toml`, `.env.example`, `.gitignore`, `reviewminer/__init__.py`, `reviewminer/connectors/__init__.py`
- Test: `tests/test_smoke.py`

**Interfaces:**
- Consumes: nothing.
- Produces: an installable package exposing the `review-miner` console script backed by `reviewminer.cli:app`; `reviewminer.__version__`.

- [ ] **Step 1: Write the failing test**

```python
# tests/test_smoke.py
import reviewminer

def test_version_present():
    assert isinstance(reviewminer.__version__, str)
    assert reviewminer.__version__
```

- [ ] **Step 2: Run test to verify it fails**

Run: `uv run pytest tests/test_smoke.py -v`
Expected: FAIL — `ModuleNotFoundError: No module named 'reviewminer'`

- [ ] **Step 3: Write minimal implementation**

```toml
# pyproject.toml
[project]
name = "reviewminer"
version = "0.1.0"
description = "Product-agnostic CLI that collects competitor reviews into a clean CSV."
requires-python = ">=3.11"
dependencies = [
    "typer>=0.12",
    "questionary>=2.0",
    "pydantic>=2.6",
    "pyyaml>=6.0",
    "pandas>=2.2",
    "python-dotenv>=1.0",
    "app-store-scraper>=0.3.5",
    "google-play-scraper>=1.2.7",
    "praw>=7.7",
    "playwright>=1.44",
]

[project.optional-dependencies]
dev = ["pytest>=8.0"]

[project.scripts]
review-miner = "reviewminer.cli:app"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build.targets.wheel]
packages = ["reviewminer"]
```

```python
# reviewminer/__init__.py
__version__ = "0.1.0"
```

```python
# reviewminer/connectors/__init__.py
```

```
# .env.example
# Reddit API credentials — create an app at https://www.reddit.com/prefs/apps
REDDIT_CLIENT_ID=your_client_id
REDDIT_CLIENT_SECRET=your_client_secret
REDDIT_USER_AGENT=review-miner/0.1 by your_username
```

```
# .gitignore
__pycache__/
*.pyc
.venv/
.env
config/*.yaml
output/
.pytest_cache/
```

- [ ] **Step 4: Run test to verify it passes**

Run: `uv run pytest tests/test_smoke.py -v`
Expected: PASS (uv resolves and installs deps on first run)

- [ ] **Step 5: Commit**

```bash
git add pyproject.toml .env.example .gitignore reviewminer/__init__.py reviewminer/connectors/__init__.py tests/test_smoke.py
git commit -m "feat: scaffold reviewminer package"
```

---

### Task 2: Review data model

**Files:**
- Create: `reviewminer/models.py`, `tests/conftest.py`
- Test: `tests/test_models.py`

**Interfaces:**
- Consumes: nothing.
- Produces:
  - `CSV_COLUMNS: list[str]` — the 12 column names in exact output order.
  - `class Review(pydantic.BaseModel)` with fields: `product: str`, `source: str`, `review_id: str`, `date: str | None = None`, `rating: float | None = None`, `title: str | None = None`, `body: str`, `author: str | None = None`, `url: str | None = None`, `helpful_count: int | None = None`, `language: str | None = None`, `scraped_at: str`.
  - `conftest.py` fixture `sample_review() -> Review`.

- [ ] **Step 1: Write the failing test**

```python
# tests/test_models.py
from reviewminer.models import Review, CSV_COLUMNS

def test_csv_columns_exact_order():
    assert CSV_COLUMNS == [
        "product", "source", "review_id", "date", "rating", "title",
        "body", "author", "url", "helpful_count", "language", "scraped_at",
    ]

def test_review_minimal_required_fields():
    r = Review(product="acme", source="appstore", review_id="1",
               body="great app", scraped_at="2026-06-30T00:00:00Z")
    assert r.rating is None
    assert r.body == "great app"

def test_review_model_dump_keys_cover_columns():
    r = Review(product="acme", source="appstore", review_id="1",
               body="x", scraped_at="2026-06-30T00:00:00Z")
    assert set(CSV_COLUMNS) == set(r.model_dump().keys())
```

- [ ] **Step 2: Run test to verify it fails**

Run: `uv run pytest tests/test_models.py -v`
Expected: FAIL — `ModuleNotFoundError: No module named 'reviewminer.models'`

- [ ] **Step 3: Write minimal implementation**

```python
# reviewminer/models.py
from __future__ import annotations
from pydantic import BaseModel

CSV_COLUMNS: list[str] = [
    "product", "source", "review_id", "date", "rating", "title",
    "body", "author", "url", "helpful_count", "language", "scraped_at",
]

class Review(BaseModel):
    product: str
    source: str
    review_id: str
    date: str | None = None
    rating: float | None = None
    title: str | None = None
    body: str
    author: str | None = None
    url: str | None = None
    helpful_count: int | None = None
    language: str | None = None
    scraped_at: str
```

```python
# tests/conftest.py
import pytest
from reviewminer.models import Review

@pytest.fixture
def sample_review() -> Review:
    return Review(
        product="acme", source="appstore", review_id="1",
        date="2026-06-01", rating=5.0, title="Love it", body="great app",
        author="sam", url="https://example.com/1", helpful_count=3,
        language="en", scraped_at="2026-06-30T00:00:00Z",
    )
```

- [ ] **Step 4: Run test to verify it passes**

Run: `uv run pytest tests/test_models.py -v`
Expected: PASS (3 passed)

- [ ] **Step 5: Commit**

```bash
git add reviewminer/models.py tests/conftest.py tests/test_models.py
git commit -m "feat: add Review schema and CSV columns"
```

---

### Task 3: Config models, load/save, validation

**Files:**
- Create: `reviewminer/config.py`
- Test: `tests/test_config.py`

**Interfaces:**
- Consumes: nothing.
- Produces:
  - Per-source spec models: `AppStoreSpec(apps: list[str], country: str = "us")`, `GooglePlaySpec(apps: list[str], country: str = "us", lang: str = "en")`, `RedditSpec(subreddits: list[str] = [], queries: list[str] = [])`, `G2Spec(urls: list[str])`, `CapterraSpec(urls: list[str])`.
  - `class Sources(BaseModel)` with optional fields `appstore, googleplay, reddit, g2, capterra` (each `None` by default).
  - `class RunConfig(BaseModel)` fields: `project: str`, `output_dir: str`, `max_per_source: int = 200`, `sources: Sources`. Validator: at least one source set; `RedditSpec` must have at least one of subreddits/queries.
  - `config_path(project: str, config_dir: str = "config") -> pathlib.Path`
  - `save_config(cfg: RunConfig, config_dir: str = "config") -> pathlib.Path`
  - `load_config(project: str, config_dir: str = "config") -> RunConfig`
  - `config_exists(project: str, config_dir: str = "config") -> bool`

- [ ] **Step 1: Write the failing test**

```python
# tests/test_config.py
import pytest
from pydantic import ValidationError
from reviewminer.config import (
    RunConfig, Sources, AppStoreSpec, RedditSpec,
    save_config, load_config, config_exists, config_path,
)

def _cfg(tmp_out):
    return RunConfig(
        project="acme-lms", output_dir=str(tmp_out), max_per_source=50,
        sources=Sources(appstore=AppStoreSpec(apps=["123"], country="us"),
                        reddit=RedditSpec(subreddits=["edtech"], queries=[])),
    )

def test_requires_at_least_one_source():
    with pytest.raises(ValidationError):
        RunConfig(project="x", output_dir="o", sources=Sources())

def test_reddit_spec_requires_subreddit_or_query():
    with pytest.raises(ValidationError):
        RunConfig(project="x", output_dir="o",
                  sources=Sources(reddit=RedditSpec(subreddits=[], queries=[])))

def test_save_then_load_round_trip(tmp_path):
    cfg = _cfg(tmp_path / "out")
    path = save_config(cfg, config_dir=str(tmp_path / "config"))
    assert path == config_path("acme-lms", str(tmp_path / "config"))
    assert config_exists("acme-lms", str(tmp_path / "config"))
    loaded = load_config("acme-lms", str(tmp_path / "config"))
    assert loaded == cfg
```

- [ ] **Step 2: Run test to verify it fails**

Run: `uv run pytest tests/test_config.py -v`
Expected: FAIL — `ModuleNotFoundError: No module named 'reviewminer.config'`

- [ ] **Step 3: Write minimal implementation**

```python
# reviewminer/config.py
from __future__ import annotations
from pathlib import Path
import yaml
from pydantic import BaseModel, model_validator

class AppStoreSpec(BaseModel):
    apps: list[str]
    country: str = "us"

class GooglePlaySpec(BaseModel):
    apps: list[str]
    country: str = "us"
    lang: str = "en"

class RedditSpec(BaseModel):
    subreddits: list[str] = []
    queries: list[str] = []

    @model_validator(mode="after")
    def _need_target(self):
        if not self.subreddits and not self.queries:
            raise ValueError("reddit needs at least one subreddit or query")
        return self

class G2Spec(BaseModel):
    urls: list[str]

class CapterraSpec(BaseModel):
    urls: list[str]

class Sources(BaseModel):
    appstore: AppStoreSpec | None = None
    googleplay: GooglePlaySpec | None = None
    reddit: RedditSpec | None = None
    g2: G2Spec | None = None
    capterra: CapterraSpec | None = None

    def selected(self) -> dict[str, BaseModel]:
        return {k: v for k, v in self.model_dump().items() if v is not None
                } and {k: getattr(self, k) for k in self.model_fields
                       if getattr(self, k) is not None}

class RunConfig(BaseModel):
    project: str
    output_dir: str
    max_per_source: int = 200
    sources: Sources

    @model_validator(mode="after")
    def _need_one_source(self):
        if not self.sources.selected():
            raise ValueError("at least one source must be configured")
        return self

def config_path(project: str, config_dir: str = "config") -> Path:
    return Path(config_dir) / f"{project}.yaml"

def config_exists(project: str, config_dir: str = "config") -> bool:
    return config_path(project, config_dir).exists()

def save_config(cfg: RunConfig, config_dir: str = "config") -> Path:
    path = config_path(cfg.project, config_dir)
    path.parent.mkdir(parents=True, exist_ok=True)
    data = cfg.model_dump(exclude_none=True)
    path.write_text(yaml.safe_dump(data, sort_keys=False), encoding="utf-8")
    return path

def load_config(project: str, config_dir: str = "config") -> RunConfig:
    raw = yaml.safe_load(config_path(project, config_dir).read_text(encoding="utf-8"))
    return RunConfig(**raw)
```

- [ ] **Step 4: Run test to verify it passes**

Run: `uv run pytest tests/test_config.py -v`
Expected: PASS (4 passed)

- [ ] **Step 5: Commit**

```bash
git add reviewminer/config.py tests/test_config.py
git commit -m "feat: add config models with validation and yaml round-trip"
```

---

### Task 4: Normalizers (raw dict → Review)

**Files:**
- Create: `reviewminer/normalize.py`
- Test: `tests/test_normalize.py`

**Interfaces:**
- Consumes: `reviewminer.models.Review`.
- Produces (all return `Review`):
  - `normalize_appstore(raw: dict, product: str, scraped_at: str) -> Review`
  - `normalize_googleplay(raw: dict, product: str, scraped_at: str) -> Review`
  - `normalize_reddit(raw: dict, product: str, scraped_at: str) -> Review`
  - `normalize_g2(raw: dict, product: str, scraped_at: str) -> Review`
  - `normalize_capterra(raw: dict, product: str, scraped_at: str) -> Review`
  - Each maps source-native keys onto the shared schema; missing fields become `None`. `body` falls back to `""` only if truly absent.

- [ ] **Step 1: Write the failing test**

```python
# tests/test_normalize.py
from reviewminer.normalize import (
    normalize_appstore, normalize_googleplay, normalize_reddit,
    normalize_g2, normalize_capterra,
)

SCRAPED = "2026-06-30T00:00:00Z"

def test_normalize_appstore_maps_fields():
    raw = {"review_id": "a1", "rating": 4, "title": "ok",
           "review": "decent", "userName": "sam", "date": "2026-06-01"}
    r = normalize_appstore(raw, product="acme", scraped_at=SCRAPED)
    assert (r.source, r.review_id, r.rating, r.body, r.author) == \
           ("appstore", "a1", 4.0, "decent", "sam")
    assert r.scraped_at == SCRAPED

def test_normalize_reddit_allows_null_rating():
    raw = {"id": "r1", "title": "thoughts?", "body": "I switched because...",
           "author": "u/x", "permalink": "/r/edtech/1", "created_utc": "2026-06-02"}
    r = normalize_reddit(raw, product="acme", scraped_at=SCRAPED)
    assert r.source == "reddit"
    assert r.rating is None
    assert r.body == "I switched because..."

def test_normalize_googleplay_maps_score_and_content():
    raw = {"reviewId": "g1", "score": 3, "content": "meh", "userName": "ann",
           "at": "2026-05-01"}
    r = normalize_googleplay(raw, product="acme", scraped_at=SCRAPED)
    assert (r.review_id, r.rating, r.body) == ("g1", 3.0, "meh")

def test_normalize_g2_and_capterra_set_source():
    g = normalize_g2({"review_id": "x", "stars": 5, "body": "love"},
                     product="acme", scraped_at=SCRAPED)
    c = normalize_capterra({"review_id": "y", "stars": 2, "body": "bad"},
                           product="acme", scraped_at=SCRAPED)
    assert g.source == "g2" and c.source == "capterra"
    assert g.rating == 5.0 and c.rating == 2.0
```

- [ ] **Step 2: Run test to verify it fails**

Run: `uv run pytest tests/test_normalize.py -v`
Expected: FAIL — `ModuleNotFoundError: No module named 'reviewminer.normalize'`

- [ ] **Step 3: Write minimal implementation**

```python
# reviewminer/normalize.py
from __future__ import annotations
from .models import Review

def _num(v):
    return float(v) if v is not None and v != "" else None

def _int(v):
    return int(v) if v is not None and v != "" else None

def normalize_appstore(raw: dict, product: str, scraped_at: str) -> Review:
    return Review(
        product=product, source="appstore",
        review_id=str(raw.get("review_id") or raw.get("id") or ""),
        date=str(raw["date"]) if raw.get("date") is not None else None,
        rating=_num(raw.get("rating")),
        title=raw.get("title"),
        body=raw.get("review") or raw.get("body") or "",
        author=raw.get("userName") or raw.get("author"),
        url=raw.get("url"),
        helpful_count=_int(raw.get("helpful_count")),
        language=raw.get("language"), scraped_at=scraped_at,
    )

def normalize_googleplay(raw: dict, product: str, scraped_at: str) -> Review:
    return Review(
        product=product, source="googleplay",
        review_id=str(raw.get("reviewId") or ""),
        date=str(raw["at"]) if raw.get("at") is not None else None,
        rating=_num(raw.get("score")),
        title=raw.get("title"),
        body=raw.get("content") or "",
        author=raw.get("userName") or raw.get("author"),
        url=raw.get("url"),
        helpful_count=_int(raw.get("thumbsUpCount")),
        language=raw.get("language"), scraped_at=scraped_at,
    )

def normalize_reddit(raw: dict, product: str, scraped_at: str) -> Review:
    permalink = raw.get("permalink")
    url = f"https://www.reddit.com{permalink}" if permalink else raw.get("url")
    return Review(
        product=product, source="reddit",
        review_id=str(raw.get("id") or ""),
        date=str(raw["created_utc"]) if raw.get("created_utc") is not None else None,
        rating=None,
        title=raw.get("title"),
        body=raw.get("body") or raw.get("selftext") or "",
        author=raw.get("author"),
        url=url,
        helpful_count=_int(raw.get("score")),
        language=None, scraped_at=scraped_at,
    )

def _normalize_scraped(raw: dict, source: str, product: str, scraped_at: str) -> Review:
    return Review(
        product=product, source=source,
        review_id=str(raw.get("review_id") or ""),
        date=str(raw["date"]) if raw.get("date") is not None else None,
        rating=_num(raw.get("stars") or raw.get("rating")),
        title=raw.get("title"),
        body=raw.get("body") or "",
        author=raw.get("author"),
        url=raw.get("url"),
        helpful_count=_int(raw.get("helpful_count")),
        language=raw.get("language"), scraped_at=scraped_at,
    )

def normalize_g2(raw: dict, product: str, scraped_at: str) -> Review:
    return _normalize_scraped(raw, "g2", product, scraped_at)

def normalize_capterra(raw: dict, product: str, scraped_at: str) -> Review:
    return _normalize_scraped(raw, "capterra", product, scraped_at)
```

- [ ] **Step 4: Run test to verify it passes**

Run: `uv run pytest tests/test_normalize.py -v`
Expected: PASS (4 passed)

- [ ] **Step 5: Commit**

```bash
git add reviewminer/normalize.py tests/test_normalize.py
git commit -m "feat: add per-source normalizers to shared schema"
```

---

### Task 5: Dedupe

**Files:**
- Create: `reviewminer/dedupe.py`
- Test: `tests/test_dedupe.py`

**Interfaces:**
- Consumes: `reviewminer.models.Review`.
- Produces: `dedupe(reviews: list[Review]) -> list[Review]` — keeps first occurrence; drops later reviews that share the same `(source, review_id)` (when `review_id` is non-empty) OR the same normalized body text (whitespace-collapsed, lowercased) across the whole set.

- [ ] **Step 1: Write the failing test**

```python
# tests/test_dedupe.py
from reviewminer.models import Review
from reviewminer.dedupe import dedupe

def _r(source, rid, body):
    return Review(product="acme", source=source, review_id=rid, body=body,
                  scraped_at="2026-06-30T00:00:00Z")

def test_drops_same_source_and_id():
    out = dedupe([_r("appstore", "1", "great"), _r("appstore", "1", "great")])
    assert len(out) == 1

def test_drops_identical_body_across_sources():
    out = dedupe([_r("appstore", "1", "Same  Text"), _r("g2", "9", "same text")])
    assert len(out) == 1

def test_keeps_distinct_reviews():
    out = dedupe([_r("appstore", "1", "a"), _r("appstore", "2", "b")])
    assert len(out) == 2
```

- [ ] **Step 2: Run test to verify it fails**

Run: `uv run pytest tests/test_dedupe.py -v`
Expected: FAIL — `ModuleNotFoundError: No module named 'reviewminer.dedupe'`

- [ ] **Step 3: Write minimal implementation**

```python
# reviewminer/dedupe.py
from __future__ import annotations
import re
from .models import Review

def _body_key(body: str) -> str:
    return re.sub(r"\s+", " ", (body or "").strip().lower())

def dedupe(reviews: list[Review]) -> list[Review]:
    seen_ids: set[tuple[str, str]] = set()
    seen_bodies: set[str] = set()
    out: list[Review] = []
    for r in reviews:
        id_key = (r.source, r.review_id) if r.review_id else None
        body_key = _body_key(r.body)
        if id_key is not None and id_key in seen_ids:
            continue
        if body_key and body_key in seen_bodies:
            continue
        if id_key is not None:
            seen_ids.add(id_key)
        if body_key:
            seen_bodies.add(body_key)
        out.append(r)
    return out
```

- [ ] **Step 4: Run test to verify it passes**

Run: `uv run pytest tests/test_dedupe.py -v`
Expected: PASS (3 passed)

- [ ] **Step 5: Commit**

```bash
git add reviewminer/dedupe.py tests/test_dedupe.py
git commit -m "feat: add review deduplication"
```

---

### Task 6: Output writer (CSV + run-summary)

**Files:**
- Create: `reviewminer/output.py`
- Test: `tests/test_output.py`

**Interfaces:**
- Consumes: `reviewminer.models.Review` + `CSV_COLUMNS`; `reviewminer.config.RunConfig`; `reviewminer.connectors.base.CollectStatus` (defined in Task 7 — for this task, import lazily and treat as an object with `.source: str`, `.count: int`, `.ok: bool`, `.error: str | None`).
- Produces: `write_outputs(reviews: list[Review], statuses: list[CollectStatus], cfg: RunConfig, run_date: str) -> tuple[Path, Path]` — writes `reviews_<run_date>.csv` (columns in `CSV_COLUMNS` order) and `run-summary_<run_date>.md`, returns `(csv_path, summary_path)`. Creates `cfg.output_dir` if missing.

> Note: Task 7 defines `CollectStatus`. To keep Task 6 independently testable, the test below defines a tiny stand-in with the same attributes. The real `CollectStatus` is attribute-compatible.

- [ ] **Step 1: Write the failing test**

```python
# tests/test_output.py
from dataclasses import dataclass
import pandas as pd
from reviewminer.models import Review, CSV_COLUMNS
from reviewminer.config import RunConfig, Sources, AppStoreSpec
from reviewminer.output import write_outputs

@dataclass
class FakeStatus:
    source: str
    count: int
    ok: bool
    error: str | None = None

def _cfg(out):
    return RunConfig(project="acme", output_dir=str(out),
                     sources=Sources(appstore=AppStoreSpec(apps=["1"])))

def test_writes_csv_with_columns_in_order(tmp_path):
    reviews = [Review(product="acme", source="appstore", review_id="1",
                      body="great", scraped_at="2026-06-30T00:00:00Z")]
    statuses = [FakeStatus("appstore", 1, True)]
    csv_path, md_path = write_outputs(reviews, statuses, _cfg(tmp_path), "2026-06-30")
    assert csv_path.name == "reviews_2026-06-30.csv"
    df = pd.read_csv(csv_path)
    assert list(df.columns) == CSV_COLUMNS
    assert df.iloc[0]["body"] == "great"

def test_summary_reports_per_source_status(tmp_path):
    statuses = [FakeStatus("appstore", 187, True), FakeStatus("g2", 0, False, "blocked")]
    _, md_path = write_outputs([], statuses, _cfg(tmp_path), "2026-06-30")
    text = md_path.read_text(encoding="utf-8")
    assert "appstore" in text and "187" in text
    assert "g2" in text and "blocked" in text
```

- [ ] **Step 2: Run test to verify it fails**

Run: `uv run pytest tests/test_output.py -v`
Expected: FAIL — `ModuleNotFoundError: No module named 'reviewminer.output'`

- [ ] **Step 3: Write minimal implementation**

```python
# reviewminer/output.py
from __future__ import annotations
from pathlib import Path
import pandas as pd
from .models import Review, CSV_COLUMNS
from .config import RunConfig

def write_outputs(reviews: list[Review], statuses, cfg: RunConfig,
                  run_date: str) -> tuple[Path, Path]:
    out_dir = Path(cfg.output_dir)
    out_dir.mkdir(parents=True, exist_ok=True)

    rows = [r.model_dump() for r in reviews]
    df = pd.DataFrame(rows, columns=CSV_COLUMNS)
    csv_path = out_dir / f"reviews_{run_date}.csv"
    df.to_csv(csv_path, index=False)

    lines = [f"# Review mining run — {run_date}", "",
             f"- **Project:** {cfg.project}",
             f"- **Total reviews:** {len(reviews)}",
             f"- **Max per source:** {cfg.max_per_source}", "",
             "## Per-source results", "",
             "| Source | Count | Status |", "|---|---|---|"]
    for s in statuses:
        mark = "✓" if s.ok else f"✗ {s.error or 'failed'}"
        lines.append(f"| {s.source} | {s.count} | {mark} |")
    md_path = out_dir / f"run-summary_{run_date}.md"
    md_path.write_text("\n".join(lines) + "\n", encoding="utf-8")
    return csv_path, md_path
```

- [ ] **Step 4: Run test to verify it passes**

Run: `uv run pytest tests/test_output.py -v`
Expected: PASS (2 passed)

- [ ] **Step 5: Commit**

```bash
git add reviewminer/output.py tests/test_output.py
git commit -m "feat: write reviews CSV and run-summary markdown"
```

---

### Task 7: Connector base ABC + registry

**Files:**
- Create: `reviewminer/connectors/base.py`
- Modify: `reviewminer/connectors/__init__.py`
- Test: `tests/test_connectors_base.py`

**Interfaces:**
- Consumes: `reviewminer.models.Review`.
- Produces:
  - `@dataclass class CollectStatus` with `source: str`, `count: int`, `ok: bool`, `error: str | None = None`.
  - `class Connector(ABC)` with class attr `name: str` and abstract `fetch(self, spec, limit: int) -> list[Review]`.
  - In `connectors/__init__.py`: `CONNECTORS: dict[str, type[Connector]]` mapping each source name to its class, and `get_connector(name: str) -> Connector` (instantiates). Registry is populated as connector modules are added in Tasks 8–11.

- [ ] **Step 1: Write the failing test**

```python
# tests/test_connectors_base.py
import pytest
from reviewminer.connectors.base import Connector, CollectStatus

def test_collect_status_defaults():
    s = CollectStatus(source="g2", count=0, ok=False)
    assert s.error is None

def test_connector_is_abstract():
    with pytest.raises(TypeError):
        Connector()  # cannot instantiate ABC with abstract fetch

def test_registry_get_unknown_raises():
    from reviewminer.connectors import get_connector
    with pytest.raises(KeyError):
        get_connector("nope")
```

- [ ] **Step 2: Run test to verify it fails**

Run: `uv run pytest tests/test_connectors_base.py -v`
Expected: FAIL — `ModuleNotFoundError: No module named 'reviewminer.connectors.base'`

- [ ] **Step 3: Write minimal implementation**

```python
# reviewminer/connectors/base.py
from __future__ import annotations
from abc import ABC, abstractmethod
from dataclasses import dataclass
from ..models import Review

@dataclass
class CollectStatus:
    source: str
    count: int
    ok: bool
    error: str | None = None

class Connector(ABC):
    name: str = ""

    @abstractmethod
    def fetch(self, spec, limit: int) -> list[Review]:
        ...
```

```python
# reviewminer/connectors/__init__.py
from __future__ import annotations
from .base import Connector, CollectStatus

# Populated as connectors are implemented (Tasks 8–11).
CONNECTORS: dict[str, type[Connector]] = {}

def _register():
    from .appstore import AppStoreConnector
    from .googleplay import GooglePlayConnector
    from .reddit import RedditConnector
    from .g2 import G2Connector
    from .capterra import CapterraConnector
    CONNECTORS.update({
        "appstore": AppStoreConnector,
        "googleplay": GooglePlayConnector,
        "reddit": RedditConnector,
        "g2": G2Connector,
        "capterra": CapterraConnector,
    })

def get_connector(name: str) -> Connector:
    if not CONNECTORS:
        try:
            _register()
        except Exception:
            pass
    if name not in CONNECTORS:
        raise KeyError(f"unknown connector: {name}")
    return CONNECTORS[name]()
```

> Note: `_register()` imports all connector modules. Until Tasks 8–11 exist, those imports fail and `CONNECTORS` stays empty, so `get_connector("nope")` still raises `KeyError` — the test passes. Once connectors exist, `_register()` succeeds.

- [ ] **Step 4: Run test to verify it passes**

Run: `uv run pytest tests/test_connectors_base.py -v`
Expected: PASS (3 passed)

- [ ] **Step 5: Commit**

```bash
git add reviewminer/connectors/base.py reviewminer/connectors/__init__.py tests/test_connectors_base.py
git commit -m "feat: add connector ABC, CollectStatus, and registry"
```

---

### Task 8: App Store connector

**Files:**
- Create: `reviewminer/connectors/appstore.py`
- Test: `tests/test_connector_appstore.py`

**Interfaces:**
- Consumes: `Connector`, `Review`, `normalize_appstore`, `config.AppStoreSpec`.
- Produces: `class AppStoreConnector(Connector)` with `name = "appstore"` and `fetch(self, spec: AppStoreSpec, limit: int) -> list[Review]`. Internally calls `self._fetch_raw(app_id, country, limit) -> list[dict]` (the seam tests monkeypatch) for each app, normalizes, and never raises.

- [ ] **Step 1: Write the failing test**

```python
# tests/test_connector_appstore.py
from reviewminer.connectors.appstore import AppStoreConnector
from reviewminer.config import AppStoreSpec

def test_fetch_normalizes_and_limits(monkeypatch):
    c = AppStoreConnector()
    raw = [{"review_id": str(i), "rating": 5, "review": f"r{i}",
            "userName": "u", "date": "2026-06-01"} for i in range(10)]
    monkeypatch.setattr(c, "_fetch_raw", lambda app_id, country, limit: raw[:limit])
    out = c.fetch(AppStoreSpec(apps=["111"], country="us"), limit=3)
    assert len(out) == 3
    assert out[0].source == "appstore"
    assert out[0].product == "111"

def test_fetch_swallows_errors(monkeypatch):
    c = AppStoreConnector()
    def boom(app_id, country, limit):
        raise RuntimeError("network down")
    monkeypatch.setattr(c, "_fetch_raw", boom)
    out = c.fetch(AppStoreSpec(apps=["111"]), limit=5)
    assert out == []
```

- [ ] **Step 2: Run test to verify it fails**

Run: `uv run pytest tests/test_connector_appstore.py -v`
Expected: FAIL — `ModuleNotFoundError: No module named 'reviewminer.connectors.appstore'`

- [ ] **Step 3: Write minimal implementation**

```python
# reviewminer/connectors/appstore.py
from __future__ import annotations
import logging
from datetime import datetime, timezone
from .base import Connector
from ..models import Review
from ..normalize import normalize_appstore
from ..config import AppStoreSpec

log = logging.getLogger(__name__)

class AppStoreConnector(Connector):
    name = "appstore"

    def _fetch_raw(self, app_id: str, country: str, limit: int) -> list[dict]:
        from app_store_scraper import AppStore
        store = AppStore(country=country, app_name=app_id, app_id=app_id)
        store.review(how_many=limit)
        return store.reviews

    def fetch(self, spec: AppStoreSpec, limit: int) -> list[Review]:
        scraped_at = datetime.now(timezone.utc).isoformat()
        out: list[Review] = []
        for app_id in spec.apps:
            try:
                raw = self._fetch_raw(app_id, spec.country, limit)
            except Exception as e:  # noqa: BLE001 — isolation by design
                log.warning("appstore fetch failed for %s: %s", app_id, e)
                continue
            for item in raw[:limit]:
                out.append(normalize_appstore(item, product=app_id, scraped_at=scraped_at))
        return out
```

- [ ] **Step 4: Run test to verify it passes**

Run: `uv run pytest tests/test_connector_appstore.py -v`
Expected: PASS (2 passed)

- [ ] **Step 5: Commit**

```bash
git add reviewminer/connectors/appstore.py tests/test_connector_appstore.py
git commit -m "feat: add App Store connector"
```

---

### Task 9: Google Play connector

**Files:**
- Create: `reviewminer/connectors/googleplay.py`
- Test: `tests/test_connector_googleplay.py`

**Interfaces:**
- Consumes: `Connector`, `Review`, `normalize_googleplay`, `config.GooglePlaySpec`.
- Produces: `class GooglePlayConnector(Connector)` with `name = "googleplay"` and `fetch(self, spec: GooglePlaySpec, limit: int) -> list[Review]`. Seam: `self._fetch_raw(app_id, country, lang, limit) -> list[dict]`. Never raises.

- [ ] **Step 1: Write the failing test**

```python
# tests/test_connector_googleplay.py
from reviewminer.connectors.googleplay import GooglePlayConnector
from reviewminer.config import GooglePlaySpec

def test_fetch_normalizes(monkeypatch):
    c = GooglePlayConnector()
    raw = [{"reviewId": "g1", "score": 4, "content": "nice",
            "userName": "u", "at": "2026-05-01"}]
    monkeypatch.setattr(c, "_fetch_raw", lambda app_id, country, lang, limit: raw)
    out = c.fetch(GooglePlaySpec(apps=["com.acme"]), limit=5)
    assert out[0].source == "googleplay"
    assert out[0].rating == 4.0
    assert out[0].product == "com.acme"

def test_fetch_swallows_errors(monkeypatch):
    c = GooglePlayConnector()
    monkeypatch.setattr(c, "_fetch_raw",
                        lambda *a, **k: (_ for _ in ()).throw(RuntimeError("x")))
    assert c.fetch(GooglePlaySpec(apps=["com.acme"]), limit=5) == []
```

- [ ] **Step 2: Run test to verify it fails**

Run: `uv run pytest tests/test_connector_googleplay.py -v`
Expected: FAIL — `ModuleNotFoundError: No module named 'reviewminer.connectors.googleplay'`

- [ ] **Step 3: Write minimal implementation**

```python
# reviewminer/connectors/googleplay.py
from __future__ import annotations
import logging
from datetime import datetime, timezone
from .base import Connector
from ..models import Review
from ..normalize import normalize_googleplay
from ..config import GooglePlaySpec

log = logging.getLogger(__name__)

class GooglePlayConnector(Connector):
    name = "googleplay"

    def _fetch_raw(self, app_id: str, country: str, lang: str, limit: int) -> list[dict]:
        from google_play_scraper import reviews, Sort
        result, _ = reviews(app_id, lang=lang, country=country,
                            sort=Sort.NEWEST, count=limit)
        return result

    def fetch(self, spec: GooglePlaySpec, limit: int) -> list[Review]:
        scraped_at = datetime.now(timezone.utc).isoformat()
        out: list[Review] = []
        for app_id in spec.apps:
            try:
                raw = self._fetch_raw(app_id, spec.country, spec.lang, limit)
            except Exception as e:  # noqa: BLE001
                log.warning("googleplay fetch failed for %s: %s", app_id, e)
                continue
            for item in raw[:limit]:
                out.append(normalize_googleplay(item, product=app_id, scraped_at=scraped_at))
        return out
```

- [ ] **Step 4: Run test to verify it passes**

Run: `uv run pytest tests/test_connector_googleplay.py -v`
Expected: PASS (2 passed)

- [ ] **Step 5: Commit**

```bash
git add reviewminer/connectors/googleplay.py tests/test_connector_googleplay.py
git commit -m "feat: add Google Play connector"
```

---

### Task 10: Reddit connector

**Files:**
- Create: `reviewminer/connectors/reddit.py`
- Test: `tests/test_connector_reddit.py`

**Interfaces:**
- Consumes: `Connector`, `Review`, `normalize_reddit`, `config.RedditSpec`.
- Produces: `class RedditConnector(Connector)` with `name = "reddit"` and `fetch(self, spec: RedditSpec, limit: int) -> list[Review]`. Seam: `self._search(subreddits, queries, limit) -> list[dict]` (PRAW lives here). Product label for Reddit rows is `"reddit"`. Never raises. If env creds missing, logs warning and returns `[]`.

- [ ] **Step 1: Write the failing test**

```python
# tests/test_connector_reddit.py
from reviewminer.connectors.reddit import RedditConnector
from reviewminer.config import RedditSpec

def test_fetch_normalizes(monkeypatch):
    c = RedditConnector()
    raw = [{"id": "p1", "title": "acme?", "selftext": "I switched...",
            "author": "u/x", "permalink": "/r/edtech/p1", "created_utc": 1,
            "score": 12}]
    monkeypatch.setattr(c, "_search", lambda subs, queries, limit: raw)
    out = c.fetch(RedditSpec(subreddits=["edtech"], queries=["acme"]), limit=5)
    assert out[0].source == "reddit"
    assert out[0].rating is None
    assert out[0].body == "I switched..."

def test_fetch_swallows_errors(monkeypatch):
    c = RedditConnector()
    monkeypatch.setattr(c, "_search",
                        lambda *a, **k: (_ for _ in ()).throw(RuntimeError("auth")))
    assert c.fetch(RedditSpec(subreddits=["edtech"]), limit=5) == []
```

- [ ] **Step 2: Run test to verify it fails**

Run: `uv run pytest tests/test_connector_reddit.py -v`
Expected: FAIL — `ModuleNotFoundError: No module named 'reviewminer.connectors.reddit'`

- [ ] **Step 3: Write minimal implementation**

```python
# reviewminer/connectors/reddit.py
from __future__ import annotations
import logging, os
from datetime import datetime, timezone
from .base import Connector
from ..models import Review
from ..normalize import normalize_reddit
from ..config import RedditSpec

log = logging.getLogger(__name__)

class RedditConnector(Connector):
    name = "reddit"

    def _client(self):
        import praw
        return praw.Reddit(
            client_id=os.environ["REDDIT_CLIENT_ID"],
            client_secret=os.environ["REDDIT_CLIENT_SECRET"],
            user_agent=os.environ["REDDIT_USER_AGENT"],
        )

    def _search(self, subreddits: list[str], queries: list[str], limit: int) -> list[dict]:
        reddit = self._client()
        items: list[dict] = []
        targets = subreddits or ["all"]
        for sub in targets:
            sr = reddit.subreddit(sub)
            for q in (queries or [""]):
                listing = sr.search(q, limit=limit) if q else sr.hot(limit=limit)
                for post in listing:
                    items.append({
                        "id": post.id, "title": post.title,
                        "selftext": post.selftext, "author": str(post.author),
                        "permalink": post.permalink,
                        "created_utc": post.created_utc, "score": post.score,
                    })
        return items

    def fetch(self, spec: RedditSpec, limit: int) -> list[Review]:
        scraped_at = datetime.now(timezone.utc).isoformat()
        try:
            raw = self._search(spec.subreddits, spec.queries, limit)
        except KeyError as e:
            log.warning("reddit creds missing (%s); skipping reddit", e)
            return []
        except Exception as e:  # noqa: BLE001
            log.warning("reddit fetch failed: %s", e)
            return []
        return [normalize_reddit(item, product="reddit", scraped_at=scraped_at)
                for item in raw[:limit]]
```

- [ ] **Step 4: Run test to verify it passes**

Run: `uv run pytest tests/test_connector_reddit.py -v`
Expected: PASS (2 passed)

- [ ] **Step 5: Commit**

```bash
git add reviewminer/connectors/reddit.py tests/test_connector_reddit.py
git commit -m "feat: add Reddit connector"
```

---

### Task 11: G2 + Capterra connectors (Playwright)

**Files:**
- Create: `reviewminer/connectors/g2.py`, `reviewminer/connectors/capterra.py`
- Create fixtures: `tests/fixtures/g2_sample.html`, `tests/fixtures/capterra_sample.html`
- Test: `tests/test_connector_g2.py`, `tests/test_connector_capterra.py`

**Interfaces:**
- Consumes: `Connector`, `Review`, `normalize_g2`, `normalize_capterra`, `config.G2Spec`, `config.CapterraSpec`.
- Produces:
  - `parse_g2_html(html: str, url: str) -> list[dict]` and `parse_capterra_html(html: str, url: str) -> list[dict]` — pure HTML→raw-dict parsers (the unit-tested seam), each dict having keys `review_id, stars, title, body, author, date, url`.
  - `class G2Connector(Connector)` / `class CapterraConnector(Connector)` with `fetch(self, spec, limit) -> list[Review]`; internal `self._get_html(url) -> str` drives Playwright (verified manually, not in CI). Never raises. Politeness: `time.sleep` + jitter between page loads.

> The CSS selectors in the parsers below are a starting point and WILL need adjustment against live markup (the accepted DIY-scraping fragility). Tests assert the parser contract using a controlled fixture with these classes, so the contract is locked even as selectors are tuned.

- [ ] **Step 1: Write the failing test + fixtures**

```html
<!-- tests/fixtures/g2_sample.html -->
<div class="paper" data-review-id="rev-1">
  <div class="stars" data-rating="4.5"></div>
  <div class="review-title">Solid but pricey</div>
  <div class="review-body">Great features, but the price keeps climbing.</div>
  <div class="reviewer">Jane D.</div>
  <time datetime="2026-05-20">May 20, 2026</time>
</div>
<div class="paper" data-review-id="rev-2">
  <div class="stars" data-rating="2.0"></div>
  <div class="review-title">Frustrating</div>
  <div class="review-body">Support never responds.</div>
  <div class="reviewer">Bob</div>
  <time datetime="2026-05-22">May 22, 2026</time>
</div>
```

```html
<!-- tests/fixtures/capterra_sample.html -->
<div class="review-card" data-review-id="c-1">
  <span class="star-rating" data-rating="5"></span>
  <h3 class="review-heading">Love it</h3>
  <p class="review-text">Saved us hours every week.</p>
  <span class="reviewer-name">Alex P.</span>
  <span class="review-date">2026-04-10</span>
</div>
```

```python
# tests/test_connector_g2.py
from pathlib import Path
from reviewminer.connectors.g2 import parse_g2_html, G2Connector
from reviewminer.config import G2Spec

FIX = Path(__file__).parent / "fixtures" / "g2_sample.html"

def test_parse_g2_html_extracts_reviews():
    raw = parse_g2_html(FIX.read_text(), url="https://g2.com/products/acme/reviews")
    assert len(raw) == 2
    assert raw[0]["review_id"] == "rev-1"
    assert raw[0]["stars"] == "4.5"
    assert raw[0]["body"].startswith("Great features")

def test_g2_fetch_uses_parser_and_limits(monkeypatch):
    c = G2Connector()
    monkeypatch.setattr(c, "_get_html", lambda url: FIX.read_text())
    out = c.fetch(G2Spec(urls=["https://g2.com/products/acme/reviews"]), limit=1)
    assert len(out) == 1
    assert out[0].source == "g2"
    assert out[0].rating == 4.5

def test_g2_fetch_swallows_errors(monkeypatch):
    c = G2Connector()
    monkeypatch.setattr(c, "_get_html",
                        lambda url: (_ for _ in ()).throw(RuntimeError("blocked")))
    assert c.fetch(G2Spec(urls=["https://g2.com/x"]), limit=5) == []
```

```python
# tests/test_connector_capterra.py
from pathlib import Path
from reviewminer.connectors.capterra import parse_capterra_html, CapterraConnector
from reviewminer.config import CapterraSpec

FIX = Path(__file__).parent / "fixtures" / "capterra_sample.html"

def test_parse_capterra_html_extracts_reviews():
    raw = parse_capterra_html(FIX.read_text(), url="https://capterra.com/p/acme")
    assert len(raw) == 1
    assert raw[0]["review_id"] == "c-1"
    assert raw[0]["stars"] == "5"

def test_capterra_fetch_uses_parser(monkeypatch):
    c = CapterraConnector()
    monkeypatch.setattr(c, "_get_html", lambda url: FIX.read_text())
    out = c.fetch(CapterraSpec(urls=["https://capterra.com/p/acme"]), limit=5)
    assert out[0].source == "capterra" and out[0].rating == 5.0
```

- [ ] **Step 2: Run tests to verify they fail**

Run: `uv run pytest tests/test_connector_g2.py tests/test_connector_capterra.py -v`
Expected: FAIL — `ModuleNotFoundError: No module named 'reviewminer.connectors.g2'`

- [ ] **Step 3: Write minimal implementation**

```python
# reviewminer/connectors/g2.py
from __future__ import annotations
import logging, random, time
from datetime import datetime, timezone
from html.parser import HTMLParser
from .base import Connector
from ..models import Review
from ..normalize import normalize_g2
from ..config import G2Spec

log = logging.getLogger(__name__)

class _G2Parser(HTMLParser):
    def __init__(self, url: str):
        super().__init__()
        self.url = url
        self.reviews: list[dict] = []
        self._cur: dict | None = None
        self._capture: str | None = None

    def handle_starttag(self, tag, attrs):
        a = dict(attrs)
        cls = a.get("class", "")
        if "paper" in cls and a.get("data-review-id"):
            self._cur = {"review_id": a["data-review-id"], "url": self.url,
                         "stars": None, "title": None, "body": None,
                         "author": None, "date": None}
        elif self._cur is not None:
            if "stars" in cls and a.get("data-rating"):
                self._cur["stars"] = a["data-rating"]
            elif "review-title" in cls:
                self._capture = "title"
            elif "review-body" in cls:
                self._capture = "body"
            elif "reviewer" in cls:
                self._capture = "author"
            elif tag == "time" and a.get("datetime"):
                self._cur["date"] = a["datetime"]

    def handle_data(self, data):
        if self._cur is not None and self._capture:
            self._cur[self._capture] = (data or "").strip()
            self._capture = None

    def handle_endtag(self, tag):
        if tag == "div" and self._cur is not None and self._cur.get("body"):
            if self._cur not in self.reviews:
                self.reviews.append(self._cur)
                self._cur = None

def parse_g2_html(html: str, url: str) -> list[dict]:
    p = _G2Parser(url)
    p.feed(html)
    return p.reviews

class G2Connector(Connector):
    name = "g2"

    def _get_html(self, url: str) -> str:
        from playwright.sync_api import sync_playwright
        with sync_playwright() as pw:
            browser = pw.chromium.launch(headless=True)
            page = browser.new_page(user_agent=random.choice(_USER_AGENTS))
            page.goto(url, wait_until="networkidle", timeout=30000)
            html = page.content()
            browser.close()
        return html

    def fetch(self, spec: G2Spec, limit: int) -> list[Review]:
        scraped_at = datetime.now(timezone.utc).isoformat()
        out: list[Review] = []
        for url in spec.urls:
            try:
                html = self._get_html(url)
            except Exception as e:  # noqa: BLE001
                log.warning("g2 fetch failed for %s: %s", url, e)
                continue
            for raw in parse_g2_html(html, url)[:limit]:
                out.append(normalize_g2(raw, product=url, scraped_at=scraped_at))
            time.sleep(1.0 + random.random())
        return out

_USER_AGENTS = [
    "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 "
    "(KHTML, like Gecko) Chrome/124.0 Safari/537.36",
    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 "
    "(KHTML, like Gecko) Version/17.0 Safari/605.1.15",
]
```

```python
# reviewminer/connectors/capterra.py
from __future__ import annotations
import logging, random, time
from datetime import datetime, timezone
from html.parser import HTMLParser
from .base import Connector
from ..models import Review
from ..normalize import normalize_capterra
from ..config import CapterraSpec
from .g2 import _USER_AGENTS

log = logging.getLogger(__name__)

class _CapterraParser(HTMLParser):
    def __init__(self, url: str):
        super().__init__()
        self.url = url
        self.reviews: list[dict] = []
        self._cur: dict | None = None
        self._capture: str | None = None

    def handle_starttag(self, tag, attrs):
        a = dict(attrs)
        cls = a.get("class", "")
        if "review-card" in cls and a.get("data-review-id"):
            self._cur = {"review_id": a["data-review-id"], "url": self.url,
                         "stars": None, "title": None, "body": None,
                         "author": None, "date": None}
        elif self._cur is not None:
            if "star-rating" in cls and a.get("data-rating"):
                self._cur["stars"] = a["data-rating"]
            elif "review-heading" in cls:
                self._capture = "title"
            elif "review-text" in cls:
                self._capture = "body"
            elif "reviewer-name" in cls:
                self._capture = "author"
            elif "review-date" in cls:
                self._capture = "date"

    def handle_data(self, data):
        if self._cur is not None and self._capture:
            self._cur[self._capture] = (data or "").strip()
            self._capture = None

    def handle_endtag(self, tag):
        if tag == "div" and self._cur is not None and self._cur.get("body"):
            if self._cur not in self.reviews:
                self.reviews.append(self._cur)
                self._cur = None

def parse_capterra_html(html: str, url: str) -> list[dict]:
    p = _CapterraParser(url)
    p.feed(html)
    return p.reviews

class CapterraConnector(Connector):
    name = "capterra"

    def _get_html(self, url: str) -> str:
        from playwright.sync_api import sync_playwright
        with sync_playwright() as pw:
            browser = pw.chromium.launch(headless=True)
            page = browser.new_page(user_agent=random.choice(_USER_AGENTS))
            page.goto(url, wait_until="networkidle", timeout=30000)
            html = page.content()
            browser.close()
        return html

    def fetch(self, spec: CapterraSpec, limit: int) -> list[Review]:
        scraped_at = datetime.now(timezone.utc).isoformat()
        out: list[Review] = []
        for url in spec.urls:
            try:
                html = self._get_html(url)
            except Exception as e:  # noqa: BLE001
                log.warning("capterra fetch failed for %s: %s", url, e)
                continue
            for raw in parse_capterra_html(html, url)[:limit]:
                out.append(normalize_capterra(raw, product=url, scraped_at=scraped_at))
            time.sleep(1.0 + random.random())
        return out
```

- [ ] **Step 4: Run tests to verify they pass**

Run: `uv run pytest tests/test_connector_g2.py tests/test_connector_capterra.py -v`
Expected: PASS (5 passed)

- [ ] **Step 5: Commit**

```bash
git add reviewminer/connectors/g2.py reviewminer/connectors/capterra.py tests/fixtures/ tests/test_connector_g2.py tests/test_connector_capterra.py
git commit -m "feat: add G2 and Capterra Playwright connectors with HTML parsers"
```

---

### Task 12: Wizard (adaptive prompts + pure config builder)

**Files:**
- Create: `reviewminer/wizard.py`
- Test: `tests/test_wizard.py`

**Interfaces:**
- Consumes: `config.RunConfig, Sources, AppStoreSpec, GooglePlaySpec, RedditSpec, G2Spec, CapterraSpec`.
- Produces:
  - `build_run_config(answers: dict) -> RunConfig` — pure function turning a flat answers dict into a validated `RunConfig`. Only builds specs for sources present in `answers["sources"]` (the adaptive contract). Splits comma-separated strings into lists.
  - `default_output_dir(project: str) -> str` → `f"20-projects/{project}/research/review-mining"`.
  - `run_wizard(project: str | None = None) -> RunConfig` — interactive (questionary); thin wrapper that gathers answers then calls `build_run_config`. (Interactive path verified manually; tests cover `build_run_config` + `default_output_dir`.)

- [ ] **Step 1: Write the failing test**

```python
# tests/test_wizard.py
import pytest
from pydantic import ValidationError
from reviewminer.wizard import build_run_config, default_output_dir

def test_default_output_dir():
    assert default_output_dir("acme-lms") == "20-projects/acme-lms/research/review-mining"

def test_build_only_includes_selected_sources():
    cfg = build_run_config({
        "project": "acme", "output_dir": "out", "max_per_source": 25,
        "sources": {
            "appstore": {"apps": "111, 222", "country": "us"},
            "reddit": {"subreddits": "edtech", "queries": "acme lms, acme"},
        },
    })
    assert cfg.sources.appstore.apps == ["111", "222"]
    assert cfg.sources.reddit.queries == ["acme lms", "acme"]
    assert cfg.sources.googleplay is None
    assert cfg.sources.g2 is None

def test_build_rejects_no_sources():
    with pytest.raises(ValidationError):
        build_run_config({"project": "x", "output_dir": "o", "sources": {}})
```

- [ ] **Step 2: Run test to verify it fails**

Run: `uv run pytest tests/test_wizard.py -v`
Expected: FAIL — `ModuleNotFoundError: No module named 'reviewminer.wizard'`

- [ ] **Step 3: Write minimal implementation**

```python
# reviewminer/wizard.py
from __future__ import annotations
from .config import (RunConfig, Sources, AppStoreSpec, GooglePlaySpec,
                     RedditSpec, G2Spec, CapterraSpec)

def _csv_list(s: str | list | None) -> list[str]:
    if s is None:
        return []
    if isinstance(s, list):
        return [x.strip() for x in s if str(x).strip()]
    return [x.strip() for x in str(s).split(",") if x.strip()]

def default_output_dir(project: str) -> str:
    return f"20-projects/{project}/research/review-mining"

def build_run_config(answers: dict) -> RunConfig:
    s = answers.get("sources", {})
    kwargs = {}
    if "appstore" in s:
        kwargs["appstore"] = AppStoreSpec(
            apps=_csv_list(s["appstore"].get("apps")),
            country=s["appstore"].get("country", "us"))
    if "googleplay" in s:
        kwargs["googleplay"] = GooglePlaySpec(
            apps=_csv_list(s["googleplay"].get("apps")),
            country=s["googleplay"].get("country", "us"),
            lang=s["googleplay"].get("lang", "en"))
    if "reddit" in s:
        kwargs["reddit"] = RedditSpec(
            subreddits=_csv_list(s["reddit"].get("subreddits")),
            queries=_csv_list(s["reddit"].get("queries")))
    if "g2" in s:
        kwargs["g2"] = G2Spec(urls=_csv_list(s["g2"].get("urls")))
    if "capterra" in s:
        kwargs["capterra"] = CapterraSpec(urls=_csv_list(s["capterra"].get("urls")))
    return RunConfig(
        project=answers["project"],
        output_dir=answers.get("output_dir") or default_output_dir(answers["project"]),
        max_per_source=int(answers.get("max_per_source", 200)),
        sources=Sources(**kwargs),
    )

_LABELS = {"appstore": "App Store", "googleplay": "Google Play",
           "reddit": "Reddit", "g2": "G2", "capterra": "Capterra"}

def run_wizard(project: str | None = None) -> RunConfig:
    import questionary
    project = project or questionary.text("Project name:").ask()
    output_dir = questionary.text(
        "Output folder:", default=default_output_dir(project)).ask()
    chosen = questionary.checkbox(
        "Which sources?",
        choices=[questionary.Choice(v, value=k) for k, v in _LABELS.items()]).ask()
    sources: dict = {}
    if "appstore" in chosen:
        sources["appstore"] = {
            "apps": questionary.text("App Store app IDs (comma-sep):").ask(),
            "country": questionary.text("Country:", default="us").ask()}
    if "googleplay" in chosen:
        sources["googleplay"] = {
            "apps": questionary.text("Google Play package IDs (comma-sep):").ask()}
    if "reddit" in chosen:
        sources["reddit"] = {
            "subreddits": questionary.text("Reddit subreddits (comma-sep):").ask(),
            "queries": questionary.text("Reddit search queries (comma-sep):").ask()}
    if "g2" in chosen:
        sources["g2"] = {"urls": questionary.text("G2 review URLs (comma-sep):").ask()}
    if "capterra" in chosen:
        sources["capterra"] = {"urls": questionary.text("Capterra review URLs (comma-sep):").ask()}
    max_per = questionary.text("Max reviews per source:", default="200").ask()
    return build_run_config({"project": project, "output_dir": output_dir,
                             "max_per_source": max_per, "sources": sources})
```

- [ ] **Step 4: Run test to verify it passes**

Run: `uv run pytest tests/test_wizard.py -v`
Expected: PASS (3 passed)

- [ ] **Step 5: Commit**

```bash
git add reviewminer/wizard.py tests/test_wizard.py
git commit -m "feat: add adaptive wizard and pure config builder"
```

---

### Task 13: Collect orchestrator

**Files:**
- Create: `reviewminer/collect.py`
- Test: `tests/test_collect.py`

**Interfaces:**
- Consumes: `config.RunConfig`, `connectors.get_connector`, `connectors.base.CollectStatus`, `models.Review`.
- Produces: `collect(cfg: RunConfig, get_connector=get_connector) -> tuple[list[Review], list[CollectStatus]]` — iterates over each set source in `cfg.sources`, resolves its connector, calls `fetch(spec, cfg.max_per_source)` inside try/except (belt-and-suspenders over connector-internal handling), and records a `CollectStatus` per source. `get_connector` is injectable for testing.

- [ ] **Step 1: Write the failing test**

```python
# tests/test_collect.py
from reviewminer.models import Review
from reviewminer.config import RunConfig, Sources, AppStoreSpec, G2Spec
from reviewminer.collect import collect

def _review(source):
    return Review(product="acme", source=source, review_id="1", body="x",
                  scraped_at="2026-06-30T00:00:00Z")

def _cfg():
    return RunConfig(project="acme", output_dir="out", max_per_source=10,
                     sources=Sources(appstore=AppStoreSpec(apps=["1"]),
                                     g2=G2Spec(urls=["u"])))

def test_collect_gathers_reviews_and_statuses():
    class FakeOK:
        def fetch(self, spec, limit): return [_review("appstore")]
    class FakeBoom:
        def fetch(self, spec, limit): raise RuntimeError("blocked")
    reg = {"appstore": FakeOK(), "g2": FakeBoom()}
    reviews, statuses = collect(_cfg(), get_connector=lambda n: reg[n])
    assert len(reviews) == 1
    by = {s.source: s for s in statuses}
    assert by["appstore"].ok and by["appstore"].count == 1
    assert not by["g2"].ok and by["g2"].error
```

- [ ] **Step 2: Run test to verify it fails**

Run: `uv run pytest tests/test_collect.py -v`
Expected: FAIL — `ModuleNotFoundError: No module named 'reviewminer.collect'`

- [ ] **Step 3: Write minimal implementation**

```python
# reviewminer/collect.py
from __future__ import annotations
import logging
from .config import RunConfig
from .connectors import get_connector as _default_get
from .connectors.base import CollectStatus
from .models import Review

log = logging.getLogger(__name__)

def collect(cfg: RunConfig, get_connector=_default_get) -> tuple[list[Review], list[CollectStatus]]:
    reviews: list[Review] = []
    statuses: list[CollectStatus] = []
    for name in cfg.sources.model_fields:
        spec = getattr(cfg.sources, name)
        if spec is None:
            continue
        try:
            connector = get_connector(name)
            got = connector.fetch(spec, cfg.max_per_source)
            reviews.extend(got)
            statuses.append(CollectStatus(source=name, count=len(got), ok=True))
        except Exception as e:  # noqa: BLE001
            log.warning("%s connector failed: %s", name, e)
            statuses.append(CollectStatus(source=name, count=0, ok=False, error=str(e)))
    return reviews, statuses
```

- [ ] **Step 4: Run test to verify it passes**

Run: `uv run pytest tests/test_collect.py -v`
Expected: PASS (1 passed)

- [ ] **Step 5: Commit**

```bash
git add reviewminer/collect.py tests/test_collect.py
git commit -m "feat: add collect orchestrator with per-source isolation"
```

---

### Task 14: CLI `run` command (wire everything)

**Files:**
- Create: `reviewminer/cli.py`
- Modify: `README.md` (append a Usage section)
- Test: `tests/test_cli.py`

**Interfaces:**
- Consumes: `wizard.run_wizard`, `config.{config_exists, load_config, save_config}`, `collect.collect`, `dedupe.dedupe`, `output.write_outputs`.
- Produces: `app = typer.Typer()` with `run(project: Optional[str] = typer.Argument(None))`. Orchestration: load `.env`; resolve project (prompt via wizard if `None`); if a saved config exists, offer reuse → load, else run wizard + save; `collect` → `dedupe` → `write_outputs(run_date=today)`; print the per-source summary and output paths. Also exposes `_pipeline(cfg, run_date, *, collect_fn, write_fn) -> tuple[Path, Path]` as the testable core (no I/O prompts).

- [ ] **Step 1: Write the failing test**

```python
# tests/test_cli.py
from pathlib import Path
from reviewminer.models import Review
from reviewminer.config import RunConfig, Sources, AppStoreSpec
from reviewminer.connectors.base import CollectStatus
from reviewminer import cli

def _cfg(out):
    return RunConfig(project="acme", output_dir=str(out), max_per_source=10,
                     sources=Sources(appstore=AppStoreSpec(apps=["1"])))

def test_pipeline_dedupes_and_writes(tmp_path):
    dup = Review(product="acme", source="appstore", review_id="1", body="same",
                 scraped_at="2026-06-30T00:00:00Z")
    def fake_collect(cfg):
        return [dup, dup], [CollectStatus("appstore", 2, True)]
    csv_path, md_path = cli._pipeline(_cfg(tmp_path), "2026-06-30",
                                      collect_fn=fake_collect)
    import pandas as pd
    assert len(pd.read_csv(csv_path)) == 1  # deduped
    assert md_path.exists()
```

- [ ] **Step 2: Run test to verify it fails**

Run: `uv run pytest tests/test_cli.py -v`
Expected: FAIL — `ImportError: cannot import name 'cli'` / `module has no attribute '_pipeline'`

- [ ] **Step 3: Write minimal implementation**

```python
# reviewminer/cli.py
from __future__ import annotations
from datetime import date
from pathlib import Path
from typing import Optional
import typer
from dotenv import load_dotenv
from .collect import collect as _collect
from .dedupe import dedupe
from .output import write_outputs
from .config import config_exists, load_config, save_config, RunConfig
from .wizard import run_wizard

app = typer.Typer(help="Collect competitor reviews into a clean CSV.")

def _pipeline(cfg: RunConfig, run_date: str, *, collect_fn=_collect,
              write_fn=write_outputs) -> tuple[Path, Path]:
    reviews, statuses = collect_fn(cfg)
    reviews = dedupe(reviews)
    return write_fn(reviews, statuses, cfg, run_date)

@app.command()
def run(project: Optional[str] = typer.Argument(None, help="Project name")):
    """Run the review collection wizard (or reuse a saved config) and write outputs."""
    load_dotenv()
    if project is None:
        project = typer.prompt("Project name")
    if config_exists(project) and typer.confirm(f'Reuse "{project}" config?', default=True):
        cfg = load_config(project)
    else:
        cfg = run_wizard(project)
        save_config(cfg)
        typer.echo(f"Saved config/{project}.yaml")
    run_date = date.today().isoformat()
    csv_path, md_path = _pipeline(cfg, run_date)
    typer.echo(f"Wrote {csv_path}")
    typer.echo(f"Wrote {md_path}")
```

Append to `README.md`:

```markdown
## Usage

```bash
uv sync                       # install deps
uv run playwright install chromium   # one-time, for G2/Capterra
cp .env.example .env          # add your Reddit API creds
uv run review-miner run       # interactive; or: uv run review-miner run acme-lms
```

Outputs land in `20-projects/<project>/research/review-mining/` as
`reviews_<date>.csv` + `run-summary_<date>.md`. Analyze the CSV manually in
Claude/ChatGPT.
```

- [ ] **Step 4: Run test to verify it passes**

Run: `uv run pytest tests/test_cli.py -v`
Expected: PASS (1 passed)

- [ ] **Step 5: Commit**

```bash
git add reviewminer/cli.py README.md tests/test_cli.py
git commit -m "feat: wire CLI run command end to end"
```

---

### Task 15: Full suite green + manual smoke checklist

**Files:**
- Create: `MANUAL-SMOKE.md`

**Interfaces:**
- Consumes: everything.
- Produces: a documented manual verification checklist for the live paths (network/browser) that CI does not cover, plus confirmation the full unit suite passes.

- [ ] **Step 1: Run the full unit suite**

Run: `uv run pytest -v`
Expected: PASS — all tests from Tasks 1–14 green (no live network).

- [ ] **Step 2: Write the manual smoke checklist**

```markdown
# Manual Smoke Checklist (live paths not covered by CI)

Run after unit tests pass. These hit real networks/browsers.

- [ ] `uv run playwright install chromium` completes.
- [ ] `.env` has valid Reddit creds.
- [ ] `uv run review-miner run smoke-test` → wizard appears; select App Store + Reddit.
- [ ] App Store: enter a real app ID (e.g. a known app) → reviews collected (count > 0).
- [ ] Reddit: enter a subreddit + query → posts collected (count > 0).
- [ ] `config/smoke-test.yaml` was written.
- [ ] Re-run `uv run review-miner run smoke-test` → "Reuse" prompt appears; Y skips prompts and runs.
- [ ] Output folder has `reviews_<date>.csv` (opens, columns correct) + `run-summary_<date>.md`.
- [ ] Select G2 with a real product URL → either reviews collected OR run-summary shows `g2 ✗` and the run still completes (isolation works).
- [ ] Selecting a source but leaving its required field empty is rejected at the prompt (validation).
```

- [ ] **Step 3: Commit**

```bash
git add MANUAL-SMOKE.md
git commit -m "docs: add manual smoke checklist"
```

---

## Self-Review

**Spec coverage (DESIGN-SPEC + PRD):**

- Adaptive wizard (PRD Story 1, spec §User flow) → Task 12 (+ Task 14 wiring).
- Ask-then-save / reuse config (PRD Story 2, spec §Config) → Task 3 (save/load/exists) + Task 14 (reuse prompt).
- Normalized collection across sources (PRD Story 3, spec §Data model/§Connectors) → Tasks 2, 4, 7–11, 13.
- Per-source failure isolation (PRD Story 4, spec §Error handling) → connector-internal try/except (Tasks 8–11) + orchestrator try/except (Task 13); surfaced in summary (Task 6).
- Vault outputs CSV + run-summary (PRD Story 5, spec §Output) → Task 6 (+ default path in Task 12 `default_output_dir`, used by Task 14).
- Config validation fails fast (spec §Error handling) → Task 3 validators; tested in Tasks 3, 12.
- Reddit creds in env only (spec §Secrets) → Task 1 `.env.example`, Task 10 env read, Task 14 `load_dotenv`.
- Politeness on scraped sites (spec §Error handling) → sleep+jitter and rotating UA in Task 11.
- Tech stack (spec §Tech stack) → Task 1 deps; libraries used in their respective connector tasks.
- Testing approach: fixtures/seams, no live network in CI (spec §Testing) → every connector exposes a monkeypatchable seam; Playwright/PRAW/store libs never called in tests.
- Out-of-scope items (no analysis/db/schedule/incremental/GUI) → not implemented (correctly absent).

**Placeholder scan:** No "TBD/TODO/handle edge cases" placeholders; every code step shows real code. The G2/Capterra CSS selectors are explicitly flagged as tunable against live markup, with the parser *contract* locked by fixture tests — this is a documented reality of DIY scraping, not a plan gap.

**Type consistency:** `fetch(spec, limit)` signature is consistent across base ABC (Task 7) and all connectors (Tasks 8–11) and the orchestrator call (Task 13). `CollectStatus(source, count, ok, error)` is consistent across Tasks 6, 7, 13, 14. `Review`/`CSV_COLUMNS` consistent across Tasks 2, 4, 6. `build_run_config`/`default_output_dir` names consistent across Tasks 12, 14. `_pipeline(cfg, run_date, *, collect_fn, write_fn)` consistent in Task 14 and its test.

---

*Plan generated 2026-06-30 from DESIGN-SPEC.md and PRD.md.*
