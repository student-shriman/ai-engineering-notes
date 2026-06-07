# Notes-1 — Python, Backend & AI Engineering Notes

A structured learning repository covering Python fundamentals, data science, machine learning, backend engineering with FastAPI, SQL, and GenAI system design. Built as a personal reference and interview preparation resource.

---

## Repository Structure

```
Notes-1/
├── BackendLab/                     ← FastAPI + Pydantic notebooks
│   ├── FastAPI_Practice.ipynb
│   ├── Pydantic_Practice.ipynb
│   └── Pydantic.md
│
├── DB/                             ← SQL practice notebook
│   ├── SQL_Practice.ipynb
│   └── sql.json
│
└── Python/                         ← Python, Data Science & ML notebooks
    ├── 1–27 numbered notebooks
    ├── 1 - Artificial Neural Networks.ipynb
    ├── files/                      ← 38 datasets (CSV, TSV, XLSX)
    └── images/                     ← reference images used in notebooks
```

---

## BackendLab

> For GenAI / AI Backend Engineers — FastAPI, Pydantic, MCP Servers

| File | Topics |
|---|---|
| `Pydantic.md` | Reference guide — BaseModel, Field, validators, serialization, ConfigDict |
| `Pydantic_Practice.ipynb` | Practice notebook — type casting, Optional, List/Dict/Any, nested models, inheritance |
| `FastAPI_Practice.ipynb` | 22-module curriculum — see below |

### FastAPI_Practice.ipynb — Module Map

| Module | Topic | Priority |
|---|---|---|
| 1 | Fundamentals — ASGI vs WSGI, App Object, OpenAPI | — |
| 2 | HTTP Methods — GET, POST, PUT, PATCH, DELETE | — |
| 3 | Request Handling — Path, Query, Body, Headers, Cookies, File Uploads | — |
| 4 | Pydantic in FastAPI — custom validators, computed fields, response_model | — |
| 5 | Dependency Injection — Depends(), chaining, LLM client as dependency | ⭐ |
| 6 | Response Types — JSONResponse, StreamingResponse, FileResponse, HTMLResponse | — |
| 7 | Async Programming — sync vs async, event loop, pitfalls | ⭐ |
| 8 | Middleware — request logging, timing, CORS, rate limiting | — |
| 9 | Error Handling — HTTPException, global handlers, structured errors | — |
| 10 | Lifespan — startup/shutdown, init DB, VectorDB, LLM on startup | ⭐ |
| 11 | Background Tasks — post-response processing | — |
| 12 | API Architecture — APIRouter, versioning, modular project structure | ⭐ |
| 13 | Database — SQLAlchemy, session management, repository pattern | ⭐ |
| 14 | Auth & Authorization — OAuth2, JWT, access/refresh tokens, RBAC, API keys | ⭐ |
| 15 | OpenAPI & Docs — Swagger UI, ReDoc, custom docs | — |
| 16 | Streaming APIs — StreamingResponse, SSE, LLM token streaming | ⭐ |
| 17 | WebSockets — echo, AI chat, ConnectionManager for broadcast | ⭐ |
| 18 | Caching — Redis, cache-aside pattern, LLM response caching | — |
| 19 | Observability — structured JSON logging, request IDs | — |
| 20 | Testing — TestClient, dependency overrides, mock LLMs | ⭐ |
| 21 | FastAPI for GenAI — document upload, RAG API, **MCP Server**, Agent API, streaming LLMs | ⭐ |
| 22 | Deployment — Docker, health checks, Gunicorn + Uvicorn | — |

---

## DB

> MySQL curriculum practiced on SQLite — swap connector for MySQL in production

| File | Description |
|---|---|
| `SQL_Practice.ipynb` | 22-module SQL curriculum with live queries on a local SQLite DB |
| `sql.json` | Curriculum definition (modules, topics, interview questions) |

### SQL_Practice.ipynb — Database

The notebook builds a 4-table SQLite database from `Python/files/employees.csv`:

```
employees (1000 rows)  ──→  departments (10 rows)
     ↕  many-to-many
employee_projects  ←──  projects (12 rows)
```

Also creates a GenAI schema: `chat_sessions`, `chat_messages`, `agent_runs`, `tool_calls`, `prompt_store`, `eval_results`

### SQL_Practice.ipynb — Module Map

| Module | Topic | Priority |
|---|---|---|
| 0 | Schema Design & Data Ingestion | — |
| 1 | DB Fundamentals — keys, relationships | — |
| 2 | SELECT — aliases, DISTINCT, ORDER BY, LIMIT | — |
| 3 | Filtering — WHERE, AND/OR, IN, BETWEEN, LIKE, IS NULL | — |
| 4 | CRUD — INSERT, UPDATE, DELETE, soft delete | — |
| 5 | Data Types — INT, TEXT, REAL, BOOLEAN, JSON, ENUM | — |
| 6 | Constraints — PK, FK, UNIQUE, NOT NULL, CHECK, DEFAULT | — |
| 7 | SQL Functions — string, numeric, date, CASE WHEN, COALESCE | — |
| 8 | Aggregation — COUNT, SUM, AVG, GROUP BY, HAVING | — |
| 9 | Joins — INNER, LEFT, anti-join, multi-table | ⭐ |
| 10 | Database Design — relationships, junction tables | ⭐ |
| 11 | Subqueries — scalar, IN, EXISTS, correlated | — |
| 12 | CTEs — WITH, multiple CTEs, recursive CTEs | ⭐ |
| 13 | Window Functions — RANK, DENSE_RANK, PARTITION BY, running totals | ⭐ |
| 14 | Transactions — BEGIN, COMMIT, ROLLBACK, ACID | ⭐ |
| 15 | Indexing — B-Tree, composite, covering index | ⭐ |
| 16 | Query Optimization — EXPLAIN, index scans | ⭐ |
| 17 | Normalization — 1NF → BCNF, denormalization | — |
| 18 | Advanced MySQL — Views, Stored Procedures, Triggers | — |
| 19 | SQLAlchemy — ORM models, sessions, relationships | ⭐ |
| 20 | FastAPI + MySQL — full integration with Depends() | ⭐ |
| 21 | DB Design for GenAI — chat, agents, tools, prompts, evals | — |
| 22 | Interview Prep — Nth salary, duplicates, running totals, top-N | ⭐ |

---

## Python

> Python fundamentals → Data Science → Machine Learning → Deep Learning

### Core Python (Notebooks 1–17)

| Notebook | Topic |
|---|---|
| 1 - Basics | Print, input, comments, indentation |
| 2 - input() & print() | Formatting, f-strings |
| 3 - Variables | Data types, type casting |
| 4 - Operators | Arithmetic, comparison, logical, bitwise |
| 5 - Decision Making | if / elif / else |
| 6 - Loops | for, while, break, continue, comprehensions |
| 7 - Functions | def, return, scope |
| 7 - Functions extended | Decorators, closures, lambda |
| 8 - Args & Kwargs | *args, **kwargs, unpacking |
| 9 - Strings | Methods, slicing, formatting, regex basics |
| 10 - Lists | Methods, slicing, sorting, comprehensions |
| 11 - Tuples | Immutability, packing/unpacking |
| 12 - Sets | Set operations, union, intersection |
| 13 - Dictionary | CRUD, nested dicts, dict comprehensions |
| 14 - Arrays | Array module, operations |
| 15 - OOPs | Classes, inheritance, encapsulation, polymorphism |
| 16 - Try & Except | Exception handling, finally, custom exceptions |
| 17 - Modules & Functions | import, packages, `__name__` |

### Data Science (Notebooks 18–23)

| Notebook | Topic |
|---|---|
| 18 - NumPy | Arrays, broadcasting, linear algebra, ufuncs |
| 18 - NumPy in a Nutshell | Quick reference |
| 19 - Pandas | DataFrame, Series, groupby, merge, reshape |
| 19 - Vaex | Large dataset handling with Vaex |
| 20 - Data Visualization | Matplotlib fundamentals |
| 20 - Plotly | Interactive charts |
| 20 - Seaborn | Statistical visualizations |
| 21 - Statistics | Descriptive stats, distributions, hypothesis testing |
| 22 - EDA | Exploratory data analysis workflow |
| 22 - Automated EDA | Profiling tools |
| 23 - Data Preprocessing | Scaling, encoding, imputation, pipelines |

### Machine Learning (Notebooks 25–27)

| Notebook | Topic |
|---|---|
| 25 - ML Introduction | Types of ML, workflow, evaluation |
| 25 - Gradient Descent | Optimization, learning rate, convergence |
| 26 - ML Start the Journey | Scikit-learn, train/test split, first model |
| 27 - Linear Regression | OLS, cost function, regularization (Ridge/Lasso) |

### Deep Learning

| Notebook | Topic |
|---|---|
| 1 - Artificial Neural Networks | Perceptron, activation functions, backpropagation, optimizers, Keras |

### Datasets (`Python/files/`)

| Dataset | Use |
|---|---|
| `employees.csv` | 1000-row HR dataset — used for SQL practice DB |
| `iris.csv` | Classic ML classification |
| `titanic.csv` | Binary classification |
| `housing.csv` / `Housing dataset.csv` | Regression |
| `red-wine.csv` / `white-wine.csv` | Multi-class classification |
| `Advertising.csv` | Linear regression practice |
| `adult.csv` | Income prediction |
| `imdb_1000.csv` | NLP / sentiment |
| `airline.csv` | Time series |
| + 30 more | EDA, visualization, preprocessing practice |

---

## Interview Priority

### Tier 1 — Must Know
`Pydantic` · `HTTP Methods` · `Depends()` · `JWT + OAuth2` · `Async/Await` · `StreamingResponse` · `SQLAlchemy` · `JOINs` · `Window Functions` · `CTEs` · `Transactions`

### Tier 2 — Strongly Recommended
`SSE` · `WebSockets` · `BackgroundTasks` · `Redis Caching` · `Testing + Dependency Overrides` · `Indexing` · `Subqueries` · `GenAI DB Design`

### Tier 3 — Senior Level
`MCP Server APIs` · `Multi-Agent Architecture` · `Async SQLAlchemy` · `Query Optimization` · `Recursive CTEs` · `FastAPI Deployment (Docker + Gunicorn)`

---

## Setup

```bash
git clone <repo-url>
cd Notes-1
pip install -r requirements.txt
jupyter notebook
```

> **SQL notebook** — runs on SQLite out of the box. No server needed.  
> **FastAPI notebook** — all routes tested via `TestClient`. No server needed.  
> **MySQL** — spin up Docker container and swap the connector in `run_query()`.

```bash
# MySQL via Docker (for SQL_Practice.ipynb)
docker run -d \
  --name mysql-practice \
  -e MYSQL_ROOT_PASSWORD=root \
  -e MYSQL_DATABASE=practice \
  -p 3306:3306 \
  mysql:8
```
