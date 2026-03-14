# MiroFish-Ko — AGENTS.md

**Generated:** 2026-03-14  
**Project:** MiroFish-Ko — Multi-agent collective intelligence engine for prediction simulation  
**Stack:** Python 3.11+ (backend) / Vue 3 + Vite (frontend) / Flask / CAMEL-AI / Zep Cloud  
**License:** AGPL-3.0

---

## OVERVIEW

**MiroFish-Ko** is a next-generation AI prediction engine powered by multi-agent technology. It automatically constructs high-fidelity parallel digital worlds from seed information (news, policy drafts, financial signals), where thousands of intelligent agents with independent personalities, long-term memory, and behavioral logic freely interact and evolve socially. Users can dynamically inject variables from a "god's perspective" to precisely infer future developments.

**Core Workflow:**
1. **Graph Construction** — Extract seeds & inject personal/collective memory & build GraphRAG
2. **Environment Setup** — Extract entity relationships & generate personas & inject agent configs
3. **Simulation** — Dual-platform parallel simulation & auto-parse prediction requirements & dynamic time-series memory updates
4. **Report Generation** — Deep interaction with simulation environment via ReportAgent with rich toolset
5. **Advanced Interaction** — Converse with all agents in simulation world & interact with ReportAgent

---

## STRUCTURE

```
MiroFish-Ko/
  ├── AGENTS.md                      # This file
  ├── pyproject.toml                 # Root package config (monorepo metadata)
  ├── package.json                   # Root npm scripts (setup, dev, build)
  ├── docker-compose.yml             # Docker deployment (frontend + backend)
  ├── Dockerfile                     # Multi-stage build
  ├── .env.example                   # Environment variables template
  ├── .github/workflows/
  │   └── docker-image.yml           # CI: Docker image build & push
  │
  ├── backend/                       # Python Flask backend
  │   ├── pyproject.toml             # Backend package config (uv)
  │   ├── requirements.txt           # Pinned dependencies
  │   ├── uv.lock                    # Locked dependency versions
  │   ├── run.py                     # Flask app entry point
  │   ├── app/
  │   │   ├── __init__.py            # Flask app factory
  │   │   ├── api/                   # REST API endpoints
  │   │   ├── models/                # Pydantic data models
  │   │   ├── services/              # Business logic
  │   │   │   ├── simulation_config_generator.py
  │   │   │   ├── ontology_generator.py
  │   │   │   ├── zep_entity_reader.py
  │   │   │   ├── text_processor.py
  │   │   │   ├── zep_graph_memory_updater.py
  │   │   │   ├── simulation_ipc.py
  │   │   │   └── report_agent.py
  │   │   └── utils/                 # Utilities
  │   │       ├── file_parser.py     # PDF/text parsing
  │   │       ├── logger.py          # Logging setup
  │   │       ├── llm_client.py      # OpenAI SDK wrapper
  │   │       ├── zep_paging.py      # Zep Cloud pagination
  │   │       └── retry.py           # Retry logic
  │   └── scripts/                   # Standalone scripts
  │       ├── run_reddit_simulation.py
  │       ├── run_twitter_simulation.py
  │       ├── run_parallel_simulation.py
  │       ├── action_logger.py
  │       └── test_profile_format.py
  │
  ├── frontend/                      # Vue 3 + Vite frontend
  │   ├── package.json               # Frontend dependencies
  │   ├── vite.config.js             # Vite build config
  │   ├── index.html                 # HTML entry point
  │   └── src/
  │       ├── main.js                # Vue app entry
  │       ├── router/
  │       │   └── index.js           # Vue Router config
  │       ├── store/
  │       │   └── pendingUpload.js   # Pinia/Vuex state
  │       ├── api/                   # API client
  │       │   ├── index.js
  │       │   ├── report.js
  │       │   ├── graph.js
  │       │   └── simulation.js
  │       ├── views/                 # Page components
  │       │   ├── Home.vue
  │       │   ├── SimulationRunView.vue
  │       │   ├── ReportView.vue
  │       │   └── InteractionView.vue
  │       └── components/            # Reusable components
  │           ├── Step1GraphBuild.vue
  │           ├── Step2EnvSetup.vue
  │           ├── Step3Simulation.vue
  │           ├── Step4Report.vue
  │           ├── Step5Interaction.vue
  │           ├── GraphPanel.vue
  │           └── HistoryDatabase.vue
  │
  ├── static/                        # Static assets
  │   └── image/                     # Logos, screenshots, demo images
  │
  ├── README.md                      # Korean README
  ├── README-EN.md                   # English README
  └── README-ZH.md                   # Chinese README
```

---

## KEY FILES

| File | Purpose | Notes |
|------|---------|-------|
| `backend/run.py` | Flask app entry point | Initializes app factory, registers blueprints |
| `backend/app/services/report_agent.py` | ReportAgent implementation | Tool calling, response validation, markdown generation |
| `backend/app/services/simulation_config_generator.py` | Config generation from seed | Persona creation, agent setup injection |
| `backend/app/services/ontology_generator.py` | Entity relationship extraction | GraphRAG construction |
| `backend/app/utils/llm_client.py` | OpenAI SDK wrapper | Supports OpenAI-compatible APIs (Qwen, etc.) |
| `backend/app/utils/file_parser.py` | PDF/text parsing | Charset detection, multi-format support |
| `frontend/src/views/Home.vue` | Upload & workflow entry | File upload, prediction requirement input |
| `frontend/src/views/SimulationRunView.vue` | Simulation execution UI | Real-time progress, agent interaction |
| `frontend/src/views/ReportView.vue` | Report display | Markdown rendering, interactive report |
| `frontend/src/views/InteractionView.vue` | Agent conversation | Chat with agents in simulation world |
| `frontend/src/components/GraphPanel.vue` | Graph visualization | D3-based node/edge rendering with pagination |
| `.env.example` | Environment template | LLM_API_KEY, LLM_BASE_URL, LLM_MODEL_NAME, ZEP_API_KEY |
| `docker-compose.yml` | Docker deployment | Frontend (port 3000) + Backend (port 5001) |

---

## CONVENTIONS

### Python (Backend)

**Line length:** 100 chars (ruff)  
**Target Python:** 3.11+  
**Package manager:** uv (with `uv.lock` for reproducibility)  
**Type hints:** Pydantic v2 models for all API contracts  
**Async:** Flask (sync) with optional asyncio in services  
**Logging:** Custom logger in `app/utils/logger.py`  
**Error handling:** Try-catch with retry logic in `app/utils/retry.py`  

**Key patterns:**
- Flask blueprints for API organization (`app/api/`)
- Pydantic models for request/response validation (`app/models/`)
- Service layer for business logic (`app/services/`)
- Utility functions for cross-cutting concerns (`app/utils/`)

### JavaScript/Vue (Frontend)

**Framework:** Vue 3 (Composition API)  
**Build tool:** Vite  
**Package manager:** npm  
**Line length:** 100 chars  
**Module format:** ES modules  
**State management:** Pinia/Vuex (if used)  
**HTTP client:** Axios  
**Visualization:** D3.js for graph rendering  

**Key patterns:**
- Single-file components (`.vue`)
- Router-based page navigation
- API client layer (`src/api/`)
- Reusable component library (`src/components/`)

### Docker

**Base images:**
- Backend: `python:3.11-slim` (lightweight)
- Frontend: `node:18-alpine` (build) → `nginx:alpine` (serve)

**Multi-stage build:** Separate build and runtime stages  
**Port mapping:** Frontend 3000, Backend 5001  
**Environment:** Read from `.env` file at runtime  

---

## COMMANDS

### Setup & Installation

```bash
# Install all dependencies (root + frontend + backend)
npm run setup:all

# Or step-by-step:
npm run setup              # Root + frontend
npm run setup:backend      # Backend (uv sync)
```

### Development

```bash
# Start both frontend and backend concurrently
npm run dev

# Or individually:
npm run backend            # Backend only (Flask on port 5001)
npm run frontend           # Frontend only (Vite on port 3000)
```

### Build

```bash
# Build frontend for production
npm run build

# Output: frontend/dist/
```

### Docker

```bash
# Build and start containers
docker compose up -d

# View logs
docker compose logs -f

# Stop containers
docker compose down
```

### Backend (Python)

```bash
# From backend/ directory:
cd backend

# Run Flask development server
uv run python run.py

# Run tests (if configured)
uv run pytest

# Install new dependency
uv pip install <package>
```

### Frontend (Vue)

```bash
# From frontend/ directory:
cd frontend

# Development server with hot reload
npm run dev

# Production build
npm run build

# Preview production build
npm run preview
```

---

## ENVIRONMENT VARIABLES

**Required (`.env` file):**

```env
# LLM API Configuration (OpenAI SDK compatible)
# Recommended: Alibaba Qwen-plus via BaiLian
# https://bailian.console.aliyun.com/
LLM_API_KEY=your_api_key
LLM_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
LLM_MODEL_NAME=qwen-plus

# Zep Cloud Configuration
# Free tier available: https://app.getzep.com/
ZEP_API_KEY=your_zep_api_key
```

**Optional:**

```env
# Flask configuration
FLASK_ENV=development
FLASK_DEBUG=1

# CORS settings
CORS_ORIGINS=http://localhost:3000

# Logging
LOG_LEVEL=INFO
```

---

## ANTI-PATTERNS

### Backend (Python)

❌ **DO NOT** use `# type: ignore` — fix type errors properly  
❌ **DO NOT** hardcode API keys — use environment variables  
❌ **DO NOT** skip error handling in LLM calls — use retry logic  
❌ **DO NOT** block the event loop with sync I/O — use async where possible  
❌ **DO NOT** modify global state in service functions — use dependency injection  

### Frontend (Vue)

❌ **DO NOT** use `v-html` with untrusted content — sanitize first  
❌ **DO NOT** make API calls in component `setup()` without error handling  
❌ **DO NOT** mutate props directly — use emits for parent communication  
❌ **DO NOT** hardcode API URLs — use environment variables  

### Docker

❌ **DO NOT** run as root in containers — use non-root user  
❌ **DO NOT** include secrets in Dockerfile — use `.env` at runtime  
❌ **DO NOT** use `latest` tag for base images — pin versions  

---

## DEPLOYMENT

### Local Development

```bash
# 1. Copy environment template
cp .env.example .env

# 2. Edit .env with your API keys
vim .env

# 3. Install dependencies
npm run setup:all

# 4. Start services
npm run dev

# Access:
# - Frontend: http://localhost:3000
# - Backend API: http://localhost:5001
```

### Docker Deployment

```bash
# 1. Prepare environment
cp .env.example .env
# Edit .env with your API keys

# 2. Build and start
docker compose up -d

# 3. Access:
# - Frontend: http://localhost:3000
# - Backend API: http://localhost:5001

# 4. View logs
docker compose logs -f backend
docker compose logs -f frontend

# 5. Stop
docker compose down
```

### Production Considerations

- **HTTPS:** Use reverse proxy (nginx/Caddy) with SSL certificates
- **Rate limiting:** Add rate limiter middleware to Flask
- **CORS:** Configure `CORS_ORIGINS` for production domain
- **Logging:** Use centralized logging (ELK, Datadog, etc.)
- **Monitoring:** Track API latency, error rates, LLM token usage
- **Scaling:** Use load balancer for multiple backend instances
- **Database:** Consider persistent storage for simulation results (PostgreSQL/MongoDB)

---

## TESTING

### Backend

```bash
cd backend

# Run all tests
uv run pytest

# Run specific test file
uv run pytest tests/test_services.py

# Run with coverage
uv run pytest --cov=app

# Run with verbose output
uv run pytest -v
```

### Frontend

```bash
cd frontend

# No test framework configured yet
# Consider adding: Vitest + Vue Test Utils
```

---

## TROUBLESHOOTING

### Backend Issues

**Port 5001 already in use:**
```bash
# Kill process on port 5001
lsof -ti:5001 | xargs kill -9

# Or use different port
FLASK_PORT=5002 npm run backend
```

**LLM API errors:**
- Verify `LLM_API_KEY` is correct
- Check `LLM_BASE_URL` format (must include `/v1`)
- Ensure API account has sufficient quota
- Check network connectivity to API endpoint

**Zep Cloud errors:**
- Verify `ZEP_API_KEY` is valid
- Check Zep Cloud dashboard for rate limits
- Ensure project is active (not archived)

### Frontend Issues

**Port 3000 already in use:**
```bash
# Kill process on port 3000
lsof -ti:3000 | xargs kill -9

# Or use different port
npm run frontend -- --port 3001
```

**API connection errors:**
- Verify backend is running on port 5001
- Check browser console for CORS errors
- Ensure `.env` has correct `LLM_API_KEY`

### Docker Issues

**Container won't start:**
```bash
# Check logs
docker compose logs backend
docker compose logs frontend

# Rebuild images
docker compose down
docker compose build --no-cache
docker compose up -d
```

**Port conflicts:**
```bash
# Edit docker-compose.yml to use different ports
# Or stop other services using ports 3000/5001
```

---

## NOTES

### Project Scale

- **Total files:** 42 (34 Python, 8 JS/Vue)
- **Backend:** Flask + CAMEL-AI + Zep Cloud
- **Frontend:** Vue 3 + Vite + D3.js
- **CI/CD:** GitHub Actions (Docker image build)
- **License:** AGPL-3.0

### Recent Changes

- Korean translation (2026-03-12)
- Image name fix in README (2026-03-12)
- Bug fix: Resolve 500 error from `<think>` tags in reasoning models (MiniMax/GLM)
- Graph pagination implementation for nodes/edges
- ReportAgent tool call handling refinement

### Dependencies

**Backend:**
- Flask 3.0+ — Web framework
- CAMEL-AI 0.2.78 — Multi-agent framework
- CAMEL-OASIS 0.2.5 — Social media simulation
- Zep Cloud 3.13.0 — Memory management
- OpenAI 1.0+ — LLM API client
- Pydantic 2.0+ — Data validation
- PyMuPDF 1.24+ — PDF parsing

**Frontend:**
- Vue 3.5+ — UI framework
- Vite 7.2+ — Build tool
- Axios 1.13+ — HTTP client
- D3 7.9+ — Graph visualization
- Vue Router 4.6+ — Routing

### Future Enhancements

- [ ] Add frontend unit tests (Vitest + Vue Test Utils)
- [ ] Implement persistent storage (PostgreSQL/MongoDB)
- [ ] Add WebSocket support for real-time simulation updates
- [ ] Implement simulation result caching
- [ ] Add multi-language support (i18n)
- [ ] Create admin dashboard for monitoring
- [ ] Add API rate limiting and authentication
- [ ] Implement simulation result export (PDF/JSON)

---

## CONTACT & SUPPORT

**Project:** MiroFish-Ko (Korean fork of MiroFish)  
**Original:** https://github.com/666ghj/MiroFish  
**License:** AGPL-3.0  
**Last Updated:** 2026-03-14

For issues, questions, or contributions, refer to the original MiroFish repository or contact the MiroFish team.

---

**⭐ Star the original MiroFish repository if you find it useful!**
