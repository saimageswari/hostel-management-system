# hostel-management-system
# 🏨 Hostel Management Dashboard

> Dark-themed admin dashboard for managing hostel students, rooms, fees, and complaints — built with **Vanilla HTML/CSS/JS** and backed by **MySQL** via a **Python FastAPI** REST API.

![MySQL](https://img.shields.io/badge/Database-MySQL-f29111?style=flat&logo=mysql&logoColor=white)
![FastAPI](https://img.shields.io/badge/Backend-FastAPI-009688?style=flat&logo=fastapi&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=flat&logo=python&logoColor=white)
![HTML5](https://img.shields.io/badge/Frontend-HTML%2FCSS%2FJS-e34f26?style=flat&logo=html5&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=flat)

---

## 📸 Preview

![Dashboard Preview](https://placehold.co/900x420/0f1117/f29111?text=Hostel+Management+%E2%80%94+MySQL+Dashboard)

---
## 🗄️ Database — MySQL (`hostel_db`)

All data is stored and managed in a **MySQL** database. Every action in the dashboard maps to a real SQL operation:

| Dashboard Action | MySQL Table | SQL Operation |
|---|---|---|
| Add Student | `students` + `allocations` | `INSERT` |
| Remove Student | `students` | `DELETE` |
| Add Room | `rooms` | `INSERT` |
| Record Payment | `payments` | `INSERT` |
| File Complaint | `complaints` | `INSERT` |
| Resolve Complaint | `complaints` | `UPDATE SET status='Resolved'` |
| Stats cards | All tables | `COUNT`, `SUM`, `GROUP BY` |
| Export CSV | `students` | `SELECT *` |

### Schema overview

```sql
CREATE DATABASE hostel_db;

-- Core tables
students      -- student_id, full_name, email, course, phone, joined_at
rooms         -- room_id, room_number, capacity, floor, status, price_per_month
allocations   -- allocation_id, student_id → rooms, check_in, check_out
payments      -- payment_id, student_id, amount, method, status, payment_date
complaints    -- complaint_id, student_id, room_id, category, description, status
staff         -- staff_id, name, role, shift, salary
visitors      -- visit_id, student_id, visitor_name, check_in, check_out
```

Full schema with foreign keys, indexes, views, triggers, and stored procedure → [`phase1_schema.sql`](./schema/phase1_schema.sql)

---

## ✨ Features

- **Students** — Add, search, filter, remove · room assignment on add
- **Rooms** — Visual grid with live occupancy · click to see occupants
- **Fees** — Track monthly payments by mode (UPI / Cash / Bank Transfer) and status
- **Complaints** — File by category · one-click resolve · status tracking
- **Stats** — Live summary cards each sourced from a MySQL table
- **CSV Export** — Downloads student data from `hostel_db.students`
- **MySQL indicators** — Every table, modal, and stat card shows its source table

---

## 🏗️ Architecture

```
Browser (HTML + CSS + JS)
        │  fetch() REST calls
        ▼
FastAPI  (Python)          ← main.py, models.py, auth.py
        │  mysql-connector-python
        ▼
MySQL   (hostel_db)        ← 7 tables, 2 views, triggers
```

---

## 🚀 Quick Start

### 1. MySQL setup
```sql
-- In MySQL Workbench or terminal
source schema/phase1_schema.sql
```

### 2. Backend
```bash
cd backend
python -m venv venv && source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt

# Set your MySQL password
cp .env.example .env

uvicorn main:app --reload
# API running at http://localhost:8000
# Swagger docs at http://localhost:8000/docs
```

### 3. Frontend
```bash
# Just open the file — no build needed
open hostel_dashboard_mysql_v2.html
```

> Make sure `uvicorn` is running before opening the dashboard.

---

## 📁 Project Structure

```
hostel-management/
├── hostel_dashboard_mysql_v2.html   ← Frontend dashboard
├── backend/
│   ├── main.py                      ← All API routes
│   ├── database.py                  ← MySQL connection
│   ├── models.py                    ← Pydantic schemas
│   ├── auth.py                      ← JWT auth helpers
│   ├── requirements.txt
│   └── .env.example
└── schema/
    └── phase1_schema.sql            ← Full MySQL schema
```

---

## 🔌 API Endpoints

| Method | Endpoint | MySQL Table |
|---|---|---|
| `GET` | `/api/students` | `students` |
| `POST` | `/api/students` | `students` |
| `DELETE` | `/api/students/{id}` | `students` |
| `GET` | `/api/rooms` | `rooms` |
| `POST` | `/api/rooms` | `rooms` |
| `POST` | `/api/allocations` | `allocations` |
| `GET` | `/api/payments` | `payments` |
| `POST` | `/api/payments` | `payments` |
| `GET` | `/api/complaints` | `complaints` |
| `POST` | `/api/complaints` | `complaints` |
| `PUT` | `/api/complaints/{id}` | `complaints` |
| `GET` | `/api/dashboard/stats` | All tables |

---

## 🔮 Roadmap

- [x] Phase 1 — Single hostel dashboard with MySQL
- [ ] Phase 2 — JWT login (admin / warden roles)
- [ ] Phase 3 — Hostel finder with location-based search
- [ ] Phase 4 — Roommate matching algorithm
- [ ] Phase 5 — Real-time chat via WebSocket

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Database | MySQL 8.x (`hostel_db`) |
| ORM / Connector | mysql-connector-python |
| Backend | Python 3.11 + FastAPI |
| Validation | Pydantic v2 |
| Auth | JWT (python-jose + bcrypt) |
| Frontend | Vanilla HTML · CSS · JavaScript |
| Icons | Tabler Icons |

---

## 🤝 Contributing

Pull requests welcome. Open an issue first for major changes.

---

## 📄 License

[MIT](LICENSE)

---

> 💡 **For the full hostel finder system** (multi-city search, booking, roommate matching) → [hostel-finder-api](https://github.com/your-username/hostel-finder-api)
