# Repository Guidelines

## Communication & Language
- 默认使用中文交流（Issues、PR 描述、评审意见、代码注释）。
- 如需面向国际读者，建议提供中英双语，但以中文为准。
- 提交信息可以中英文皆可；保证含义清晰、可检索。

## Project Structure & Module Organization
- `src/` — Node.js service code: `routes/`, `services/`, `middleware/`, `utils/`, `models/`; entrypoint `src/app.js`.
- `cli/` — Interactive admin/ops CLI (`npm run cli`).
- `scripts/` — Maintenance and ops scripts (migrations, manage, tests, status).
- `config/` — App config; copy `config/config.example.js` to `config/config.js`.
- `web/admin-spa/` — Admin SPA (built and served by the backend).
- `docs/` — Screenshots and docs. `resources/` — pricing fallbacks.
- Runtime artifacts: `logs/`, `data/` (created at run time; do not commit).

## Build, Test, and Development Commands
```bash
# First‑time setup
make setup              # copies .env/config and runs bootstrap
npm run install:web     # install admin SPA deps
npm run build:web       # build admin SPA

# Local development
npm run dev             # nodemon + lint on change
npm start               # production mode (lint then start)

# Quality
npm run lint            # ESLint (auto‑fix)
npm run format          # Prettier format
npm test                # Jest test runner

# Service management wrappers
npm run service:start[:daemon]  # start (foreground/background)
npm run service:status|logs     # status and logs
```
Docker users: `docker-compose up -d`. Make equivalents exist in `Makefile` (e.g., `make dev`, `make docker-up`). Node 18+ required.

## Coding Style & Naming Conventions
- JavaScript only; 2‑space indent, LF endings, max line 100, single quotes, no semicolons, no trailing commas (enforced by Prettier).
- Run `npm run lint` before committing. ESLint enforces: `eqeqeq`, braces on all blocks, `prefer-const`, `no-var`, arrow callbacks.
- Filenames: lowerCamelCase for JS in `src/` and `services/` (e.g., `apiKeyService.js`, `openaiRoutes.js`). Directories are lowercase.

## Testing Guidelines
- Frameworks: Jest (+ Supertest for HTTP). Place tests as `**/*.test.js` or `**/*.spec.js` or under `tests/`.
- Aim to cover services and routes (e.g., `services/pricingService.js`, `routes/openaiRoutes.js`).
- Run `npm test` locally; no strict coverage gate yet—add tests for new behavior and bug fixes.

## Commit & Pull Request Guidelines
- Use Conventional Commits: `feat:`, `fix:`, `chore:`, `docs:`, `refactor:` (history already follows this style; English or Chinese OK).
- PRs must include: clear description, rationale, steps to test, and linked issues. Include screenshots/GIFs for `web/admin-spa/` changes.
- Before opening a PR: ensure `npm run lint` and `npm test` pass and update relevant docs.

## Security & Configuration Tips
- Copy `.env.example` to `.env` and set `JWT_SECRET` and a 32‑char `ENCRYPTION_KEY`. Do not commit secrets.
- Redis 6+ required. Admin credentials are stored in `data/init.json` (created by `npm run setup`).
