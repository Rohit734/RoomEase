# ✅ Trea AI Rules for RoomEase 
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
