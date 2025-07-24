# Domain-Centric Module Rules for `trae.ai` Backend

This guide outlines mandatory conventions and structure for organizing domain-specific modules in the `apps/backend/app/modules/` directory of the `trae.ai` backend. It promotes maintainability, scalability, and a clean separation of concerns.

---

## 📁 Required Structure for Each Domain Module

Each sub-directory under `modules/` represents a core business domain and must include the following files:

| File         | Description                                                      |
| ------------ | ---------------------------------------------------------------- |
| `models.py`  | SQLAlchemy ORM models representing DB tables.                    |
| `schemas.py` | Pydantic models for request validation & response serialization. |
| `crud.py`    | CRUD operations interfacing with the database.                   |
| `routers.py` | FastAPI API routes (APIRouter) for domain endpoints.             |

---

## 📐 Naming Conventions

* All domain directories must be lowercase and plural.
  ✅ `users`, `bookings`, `payments`
  ❌ `User`, `BookingsModule`

---

## 📌 File-Level Responsibilities

### `models.py`

* Define ORM models with `__tablename__`.
* Inherit from `Base` in `app.database`.
* No business logic.

### `schemas.py`

* Define separate input/output schemas (`UserCreate`, `UserOut`).
* Set `orm_mode = True` for output models.
* No ORM imports.

### `crud.py`

* Typed CRUD functions using SQLAlchemy `Session`.
* No business logic — only data layer operations.

### `routers.py`

* Use `APIRouter` with route prefix (e.g., `/users`).
* Group HTTP methods logically.
* Use `Depends()` for auth, db, etc.

---

## 🧩 Global Rules

| Rule | Description                                             |
| ---- | ------------------------------------------------------- |
| 🔹 1 | Each domain has its own module folder under `modules/`. |
| 🔹 2 | Cross-domain imports must be minimized.                 |
| 🔹 3 | Avoid business logic in `routers.py` or `crud.py`.      |
| 🔹 4 | Register all routers in `main.py`, not inline in files. |
| 🔹 5 | Return types must be annotated.                         |
| 🔹 6 | Use `__all__ = [...]` to define module exports.         |
| 🔹 7 | ORM models should remain minimal.                       |
| 🔹 8 | Use Alembic for migrations — no manual DDL.             |

---

## 🧪 Optional (Advanced Usage)

You may include these optional files if the domain needs them:

* `services.py` — for use-case specific business logic
* `constants.py` — domain-level constants/enums
* `tests/` — domain-level unit/integration tests

---

## 🚀 Example: `users` Module Structure

```
modules/
└── users/
    ├── crud.py
    ├── models.py
    ├── routers.py
    └── schemas.py
```

```python
# routers.py
from fastapi import APIRouter, Depends
from . import schemas, crud
from sqlalchemy.orm import Session
from app.dependencies import get_db

router = APIRouter(prefix="/users", tags=["Users"])

@router.post("/", response_model=schemas.UserOut)
def create_user(user: schemas.UserCreate, db: Session = Depends(get_db)):
    return crud.create_user(db=db, user=user)
```

---

## 🔚 Summary

This structure guarantees:

* High cohesion within modules
* Low coupling between domains
* Easy testability and readability
* Clear boundaries between layers of the backend

All contributors **must follow this guide** when working with domain logic under `app/modules/`.
