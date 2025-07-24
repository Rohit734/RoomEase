# ✅ Trea AI Rules for RoomEase Monorepo

## 1. 🧠 Context Awareness
- Always retain and apply knowledge from:
  - `docs/fullstack-architecture.md`
  - `docs/prd.md`
  - `docs/project-brief.md`
  - `docs/project-plan.md`
- All work must follow the **step-by-step tasks outlined in `project-plan.md`**.
- Ensure all logic aligns with **RoomEase’s MVP goals** (e.g., escrow flow, conditional chat, immutable rooms).

## 2. ⚙️ Approved Tech Stack (No Versions Specified)

| Layer       | Tool/Library        | Notes |
|-------------|---------------------|-------|
| Frontend    | Flutter             | Mobile app only |
| Backend     | FastAPI             | Monolith pattern |
| Language    | Python              | Backend |
| ORM         | SQLAlchemy          | SQL layer |
| Migrations  | Alembic             | Maintain schema history |
| Validation  | Pydantic            | Request/response schemas |
| DB          | PostgreSQL          | Relational DB |
| AI          | Gemini API          | Auto-generate room descriptions |
| Cloud       | GCP (Cloud Run, SQL, Firebase) | Deployment |
| CI/CD       | GitHub Actions      | Optional future use |
| Container   | Docker              | Use `docker-compose.yml` for local dev |

## 3. 🗂️ Monorepo Structure Compliance

- Follow domain-driven structure (`auth`, `rooms`, etc.)
- Use relative imports
- Avoid shared logic outside of `core/`

## 4. 🔁 Development Pattern by Domain

Each module should contain:
- `routers/`
- `schemas/`
- `crud/`
- `models/`

## 5. 🧪 Testing Mandate
- Use `pytest` and `flutter_test` per platform
- Place test files under the correct domain path

## 6. 🧾 Commit Message Convention

```bash
<type>(scope): <short description>
# Example:
feat(rooms): add booking confirmation logic with webhook trigger
```

## 7. 🔐 Business Logic Enforcement
- Room numbers are immutable
- Location & contact visible only after payment
- Escrow must follow 24h response window

## 8. 🧭 UI Rules
- Minimal Airbnb-style
- Use message templates
- Respect PRD-specified screens

## 9. 🔄 Prompting Etiquette
If unsure, ask:
- Should this be shared or local?
- Reuse or recreate this widget/module?

## 10. 📋 Follow the Project Plan Step-by-Step
- Always work from `project-plan.md`
- Do not jump phases
- Confirm each phase/module before proceeding

## 11. ❌ Forbidden Behaviors
- ❌ Don’t use Django/React/Node
- ❌ Don’t modify `.env`, `.gitignore`, or docs without permission
- ❌ Don’t place logic inside routers
