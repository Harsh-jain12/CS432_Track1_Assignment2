# CS 432 — Databases | Assignment 2 — Track 1

## Team Members & Contributions

| Name | Roll Number | Module | Contribution |
|------|-------------|--------|--------------|
| Harsh Jain | 22110093 | Module B | UI & API development; authored the Optimisation Implementation Report (schema design, indexing strategy, benchmarking) |
| Shreyas Dharmatti | 21110202 | Module B | UI & API development; recorded and presented the video explanations for UI walkthrough and API demonstration |
| Sneha Gautam | 22110255 | Module A | B+ Tree implementation, BruteForceDB, performance benchmarking, and Jupyter Notebook report |
| Anushika Mishra | 22110029 | Module A | B+ Tree implementation, BruteForceDB, performance benchmarking, and Jupyter Notebook report |
| Kandarp Jani | 22110104 | Module A | Module A testing, validation, and recorded the Module A video walkthrough |

---

**Course:** CS 432 Databases
**Instructor:** Prof. Yogesh K Meena
**Institution:** CSE, IIT Gandhinagar

---

## Repository Structure

```
CS432_Track1_Assignment2/
├── Module_A/
│   ├── database/
│   │   ├── __init__.py
│   │   ├── bplustree.py          ← B+ Tree implementation (insert, search, delete, range query)
│   │   ├── bruteforce.py         ← BruteForceDB baseline (linear scan implementation)
│   │   ├── db_manager.py         ← DatabaseManager (multi-table, multi-database management)
│   │   ├── table.py              ← Table class (record store + B+ Tree index attachment)
│   │   └── performance_analyzer.py ← Benchmarking engine (insert, search, delete, range, memory)
│   ├── report.ipynb              ← Jupyter Notebook with results, graphs, and visualisations
│   └── requirements.txt
│
└── Module_B/
    ├── app/                      ← Express.js API + Vanilla HTML/CSS/JS frontend
    │   ├── server.js             ← Entry point
    │   ├── config/db.js          ← MySQL connection pool
    │   ├── middleware/           ← JWT auth + RBAC permissions enforcement
    │   ├── routes/               ← API route handlers (members, doctors, appointments, etc.)
    │   ├── utils/audit.js        ← Audit logging utility
    │   └── public/               ← Frontend HTML pages + JS + CSS
    ├── sql/                      ← All database scripts (schema, seed, auth, migration)
    ├── logs/                     ← audit.log (API action log)
    ├── report.pdf                ← Optimisation Implementation Report
    ├── API_DOCS_TO_RUN.md        ← curl command reference for all 9 roles
    └── requirements.txt
```

---

## Module A — B+ Tree Database Engine

### Overview

Module A implements a custom in-memory database engine built on a **B+ Tree index structure**, benchmarked against a **BruteForce linear-scan baseline**. The goal is to demonstrate the performance advantages of tree-based indexing over naive sequential search across insertions, deletions, searches, and range queries.

### File Descriptions

| File | Description |
|------|-------------|
| `bplustree.py` | Full B+ Tree implementation with configurable order. Supports `insert`, `search`, `delete`, and `range_query`. Leaf nodes are linked for efficient range traversal. Uses `graphviz` for tree visualisation. |
| `bruteforce.py` | `BruteForceDB` — a simple list-based database that performs linear scans for all operations. Acts as the performance baseline. |
| `db_manager.py` | `DatabaseManager` — manages multiple named databases, each containing multiple `Table` instances. Handles `create_database`, `delete_database`, `create_table`, `insert`, `search`, `update`, `delete`, and `range_query` at the manager level. |
| `table.py` | `Table` — stores raw records as a dict and attaches a B+ Tree index. Validates records (must be dicts containing the index key), and routes all CRUD operations through both the record store and the index. |
| `performance_analyzer.py` | `PerformanceAnalyzer` — benchmarks both structures across: insert, search, delete, range query, and memory usage. Uses `time.perf_counter` and `tracemalloc`. Generates Matplotlib comparison graphs. |

### How to Run Module A

**1. Install dependencies:**
```bash
cd Module_A
pip install -r requirements.txt
```

**2. Open the Jupyter Notebook:**
```bash
jupyter notebook report.ipynb
```

**3. Run all cells** — the notebook executes benchmarks and renders:
- B+ Tree vs BruteForceDB performance comparison tables
- Matplotlib bar charts for insert / search / delete / range query times
- Graphviz visualisations of the B+ Tree internal structure (nodes, leaves, linkages)
- Memory usage analysis

**Requirements:** `matplotlib`, `graphviz`, `memory-profiler`, `pandas`, `numpy`

---

## Module B — Dispensary Management System

### Overview

Module B is a full-stack **college dispensary management system** built with Node.js, Express, MySQL, and Vanilla HTML/CSS/JS. It features stateless JWT authentication, role-based access control across 10 roles, composite-index-optimised SQL queries, and a dual-layer audit/tamper-detection system.

### How to Run Module B

**1. Install Node dependencies:**
```bash
cd Module_B/app
npm install
```

**2. Set up environment variables:**
```bash
cp .env.example .env
# Edit .env and fill in:
# DB_HOST, DB_PORT, DB_USER, DB_PASSWORD, DB_NAME, JWT_SECRET, PORT
```

**3. Set up the database** — run SQL files in this exact order:
```bash
mysql -u root -p < sql/DispensaryManagement.sql
mysql -u root -p DispensaryManagement < sql/Dispensary_Management_Insert.sql
mysql -u root -p DispensaryManagement < sql/auth_system.sql
```

Optionally load the large seed dataset (260 members, 55 doctors, 530 appointments):
```bash
mysql -u root -p DispensaryManagement < sql/seed_data.sql
mysql -u root -p DispensaryManagement < sql/doctor_schedule_migration.sql
```

**4. Start the server:**
```bash
node server.js
# Expected output:
# MySQL connected successfully
# Dispensary API running on http://localhost:3000
```

**5. Open the UI:**
```
http://localhost:3000/index.html
```

**6. Set a password and log in:**
```bash
# All accounts start with placeholder password "CHANGEME"
# Set a real password first:
curl -X POST http://localhost:3000/api/auth/set-password \
  -H "Content-Type: application/json" \
  -d '{"userLoginID": 1, "newPassword": "YourPassword@123"}'
```

See `API_DOCS_TO_RUN.md` for ready-to-run curl commands for all 9 roles.

---

## Video Demonstrations

| Video | Module | Link |
|-------|--------|------|
| 🎥 B+ Tree Implementation Walkthrough | Module A | https://drive.google.com/file/d/1p_yqEF5ky3ERx-uxMaZBg4IHQqrLfyUM/view?usp=drivesdk |
| 🎥 UI Walkthrough | Module B | https://drive.google.com/file/d/17jF_sckKu7TVWN-sXxK3fnIFQrToO_9-/view |
| 🎥 API Explanation | Module B | https://drive.google.com/file/d/1nB9LWBbdmK4lKFyRkx5sD2vhiIeqhGTr/view |

---

## Tools & External Sources Used

| Tool / Library | Used In | Purpose |
|----------------|---------|---------|
| Python 3.10 | Module A | Core language |
| `graphviz` | Module A | B+ Tree visualisation |
| `matplotlib` | Module A | Performance benchmark graphs |
| `tracemalloc` | Module A | Memory usage profiling |
| `pandas`, `numpy` | Module A | Data analysis in notebook |
| Node.js / Express.js | Module B | REST API server |
| MySQL 8.0 | Module B | Relational database |
| `jsonwebtoken` | Module B | Stateless JWT authentication |
| `bcrypt` | Module B | Password hashing (cost factor 12) |
| Vanilla HTML/CSS/JS | Module B | Frontend UI |
| LaTeX (pgfplots, TikZ) | Module B | Report generation |
