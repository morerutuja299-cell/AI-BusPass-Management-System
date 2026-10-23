# 🚌 BusIQ — AI Based Automatic Bus Pass Management System

> **A Django web application that automates bus pass issuance, management, and verification using Artificial Intelligence.**

---

## 📋 Table of Contents

1. [Project Overview](#1-project-overview)
2. [Problem Statement](#2-problem-statement)
3. [Proposed Solution](#3-proposed-solution)
4. [Technology Stack](#4-technology-stack)
5. [System Architecture](#5-system-architecture)
6. [Application Structure](#6-application-structure)
7. [Module Descriptions](#7-module-descriptions)
8. [User Roles & Features](#8-user-roles--features)
9. [AI Module — How It Works](#9-ai-module--how-it-works)
10. [Database Design](#10-database-design)
11. [API Endpoints & URL Routes](#11-api-endpoints--url-routes)
12. [Installation & Setup](#12-installation--setup)
13. [Running the Project](#13-running-the-project)
14. [Login Credentials](#14-login-credentials)
15. [Screenshots Guide](#15-screenshots-guide)
16. [Security Features](#16-security-features)
17. [Fixtures & Sample Data](#17-fixtures--sample-data)
18. [Troubleshooting](#18-troubleshooting)
19. [Future Enhancements](#19-future-enhancements)
20. [Conclusion](#20-conclusion)

---

## 1. Project Overview

**BusIQ** is a full-stack web application built with **Django (Python)** that replaces manual, paper-based bus pass systems with a smart digital solution. It uses **Artificial Intelligence** to automatically verify user identity through face recognition and document OCR (Optical Character Recognition), reducing fraud and administrative workload.

| Field          | Details                                      |
|----------------|----------------------------------------------|
| Project Name   | BusIQ                                        |
| Category       | Web Application + AI                         |
| Backend        | Django 4.x (Python)                          |
| Database       | PostgreSQL (Production) / SQLite (Dev)       |
| AI Libraries   | OpenCV, Pytesseract, NumPy, Pillow           |
| Frontend       | HTML5, Bootstrap 5, Django Templates         |
| Version Control| Git + GitHub                                 |

---

## 2. Problem Statement

Traditional bus pass management systems face several major challenges:

| Problem | Impact |
|---------|--------|
| Manual document verification | Slow processing, human error |
| Long queues at counters | Poor user experience |
| Fake or duplicate passes | Revenue loss, fraud |
| No centralized records | Difficult tracking and audits |
| High admin workload | Inefficient operations |
| Paper-based system | Environmental waste |

---

## 3. Proposed Solution

BusIQ addresses all the above problems with a digital, AI-powered approach:

- ✅ **Online application** — users apply from home, no queues
- ✅ **AI identity verification** — face recognition + OCR document check
- ✅ **Fraud prevention** — automatic fraud flagging system
- ✅ **Digital QR pass** — printable, scannable bus pass
- ✅ **Centralized admin panel** — one place to manage everything
- ✅ **Role-based access** — separate portals for users and admins
- ✅ **Audit logs** — every action is recorded in system logs

---

## 4. Technology Stack

### Backend
```
Python 3.10+
Django 4.2
Django REST Framework 3.16
```

### Database
```
PostgreSQL (Production)
SQLite (Development/Testing)
psycopg2-binary 2.9.11
```

### AI & Image Processing
```
OpenCV (opencv-contrib-python 4.13)   — Face detection & matching
NumPy 2.2.6                           — Array/matrix operations
Pillow 12.1.1                         — Image loading & processing
Pytesseract 0.3.13                    — OCR text extraction
```

### Frontend
```
HTML5 + CSS3
Bootstrap 5.3
Bootstrap Icons 1.10
Django Templates (Jinja-style)
```

### Other Tools
```
qrcode 8.2          — QR code generation
python-decouple 3.8 — Environment variable management
GitHub              — Version control
VS Code             — Development IDE
```

---

## 5. System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    CLIENT BROWSER                        │
│              (Bootstrap 5 + Django Templates)            │
└──────────────────────────┬──────────────────────────────┘
                           │ HTTP Requests
┌──────────────────────────▼──────────────────────────────┐
│                    DJANGO WEB SERVER                     │
│  ┌─────────────┐  ┌──────────────┐  ┌───────────────┐  │
│  │  users app  │  │ buspass app  │  │ admin_panel   │  │
│  │  (auth,     │  │ (pass mgmt,  │  │ (dashboard,   │  │
│  │   profile)  │  │  routes, QR) │  │  approve,     │  │
│  └──────┬──────┘  └──────┬───────┘  │  reject)      │  │
│         │                │          └───────┬───────┘  │
│  ┌──────▼────────────────▼─────────────────▼───────┐   │
│  │                   ai_module                      │   │
│  │   Face Verification | OCR | Fraud Detection      │   │
│  └──────────────────────┬───────────────────────────┘   │
└─────────────────────────┼───────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────┐
│                  POSTGRESQL DATABASE                     │
│   CustomUser | BusPass | Route | AIVerification | Log   │
└─────────────────────────────────────────────────────────┘
```

---

## 6. Application Structure

```
BusIQ/
│
├── busiq_backend/          # Django project configuration
│   ├── __init__.py
│   ├── settings.py         # All project settings
│   ├── urls.py             # Root URL dispatcher
│   └── wsgi.py             # WSGI entry point
│
├── users/                  # User management app
│   ├── models.py           # CustomUser model
│   ├── views.py            # Register, login, dashboard, profile
│   ├── forms.py            # Registration and profile forms
│   ├── urls.py             # User URL patterns
│   └── admin.py            # Django admin config
│
├── buspass/                # Bus pass management app
│   ├── models.py           # BusPass, Route models
│   ├── views.py            # Apply, view, download, renew
│   ├── forms.py            # Bus pass application form
│   ├── urls.py             # Bus pass URL patterns
│   └── admin.py            # Django admin config
│
├── ai_module/              # AI verification engine
│   ├── models.py           # AIVerification model
│   ├── utils.py            # Face verify, OCR, fraud detect
│   ├── views.py            # Verification status, re-verify
│   ├── urls.py             # AI URL patterns
│   └── admin.py            # Django admin config
│
├── admin_panel/            # Admin dashboard app
│   ├── models.py           # SystemLog model
│   ├── views.py            # Dashboard, approve, reject
│   ├── urls.py             # Admin panel URL patterns
│   └── admin.py            # Django admin config
│
├── templates/              # All HTML templates
│   ├── base.html           # Master layout
│   ├── users/
│   │   ├── login.html
│   │   ├── register.html
│   │   ├── dashboard.html
│   │   └── profile.html
│   ├── buspass/
│   │   ├── apply.html
│   │   ├── view_pass.html
│   │   └── download_pass.html
│   ├── ai_module/
│   │   └── status.html
│   └── admin_panel/
│       ├── dashboard.html
│       ├── application_list.html
│       ├── application_detail.html
│       ├── approve_pass.html
│       ├── reject_pass.html
│       ├── user_list.html
│       └── fraud_alerts.html
│
├── fixtures/               # Sample data JSON files
│   ├── initial_routes.json
│   ├── sample_ai_verifications.json
│   └── sample_logs.json
│
├── media/                  # User uploaded files (auto-created)
├── static/                 # Static CSS/JS files
│
├── manage.py               # Django management CLI
├── create_admin.py         # One-time admin user creation
├── setup_sample_data.py    # Load demo data script
├── requirements.txt        # Python dependencies
├── .env                    # Environment variables (secret)
└── README.md               # This file
```

---

## 7. Module Descriptions

### 🔹 `users` App

Handles all user-related functionality.

| Component | Description |
|-----------|-------------|
| `CustomUser` model | Extends Django's AbstractUser with `role`, `phone`, `address`, `profile_photo`, `id_proof`, `date_of_birth` |
| `UserRegisterForm` | Registration form with photo/ID upload fields |
| `UserLoginForm` | Authentication form |
| `register_view` | Creates user, triggers AI verification automatically |
| `login_view` | Authenticates and redirects based on role |
| `dashboard_view` | Shows pass status + AI verification result |
| `profile_view` | Edit personal information and re-upload documents |

---

### 🔹 `buspass` App

Handles bus pass lifecycle.

| Component | Description |
|-----------|-------------|
| `Route` model | Stores bus routes with source, destination, fare |
| `BusPass` model | Stores pass with status, QR code, validity dates |
| `apply_pass_view` | Submit new application (requires AI verified) |
| `view_pass_view` | List all user's passes |
| `download_pass_view` | View printable pass with QR code |
| `renew_pass_view` | Create renewal application |
| `generate_qr_code()` | Generates QR from pass data using `qrcode` library |

---

### 🔹 `ai_module` App

The intelligence engine of the project.

| Component | Description |
|-----------|-------------|
| `AIVerification` model | Stores all verification results per user |
| `verify_face()` | OpenCV Haar cascade face detection + template matching |
| `verify_document()` | Pytesseract OCR + regex name/ID extraction |
| `check_fraud()` | Flags suspicious low-confidence verifications |
| `run_ai_verification()` | Orchestrates the full verification pipeline |
| `re_verify_view` | Lets user re-trigger verification after updating photos |

---

### 🔹 `admin_panel` App

Admin-only dashboard and controls.

| Component | Description |
|-----------|-------------|
| `SystemLog` model | Tracks all system events with type + message |
| `admin_dashboard_view` | Stats: users, pending, approved, fraud alerts |
| `application_list_view` | Filterable list of all applications |
| `application_detail_view` | Full detail with user photos + AI result |
| `approve_pass_view` | Approve + auto-set validity + generate QR |
| `reject_pass_view` | Reject with reason |
| `fraud_alerts_view` | List all fraud-flagged users |

---

## 8. User Roles & Features

### 👤 User Role

```
Register
  └─► Upload profile photo + ID proof
        └─► AI auto-runs on registration
              └─► If verified → Apply for Bus Pass
                    └─► Admin reviews → Approve/Reject
                          └─► Download digital QR pass
```

| Feature | Description |
|---------|-------------|
| Register & Login | Secure authentication |
| Upload Documents | Profile photo + Government ID |
| AI Verification | Automatic on registration, manual re-run |
| Apply for Pass | Select route + pass type |
| View Pass Status | See pending/approved/rejected |
| Download Pass | Printable pass with QR code |
| Renew Pass | Submit renewal application |
| Edit Profile | Update info and re-upload documents |

---

### 🧑‍💼 Admin Role

```
Login
  └─► Admin Dashboard (stats overview)
        ├─► Applications → Review → Approve / Reject
        ├─► Users → View all registered users
        └─► Fraud Alerts → Flag suspicious accounts
```

| Feature | Description |
|---------|-------------|
| Dashboard | Live stats: users, passes, fraud alerts |
| Review Applications | See user details + AI result side by side |
| Approve Pass | Sets validity dates, generates QR code |
| Reject Pass | Adds rejection reason |
| View Users | All registered user list |
| Fraud Alerts | List of users flagged by AI |
| System Logs | Audit trail of all admin actions |

---

## 9. AI Module — How It Works

### Step 1 — Face Verification

```
Profile Photo ──►┐
                 ├──► OpenCV Haar Cascade Face Detection
ID Photo     ──►┘         │
                           ▼
                   Extract face regions (100×100 px)
                           │
                           ▼
                   Template Matching (TM_CCOEFF_NORMED)
                           │
                           ▼
                   Confidence Score (0–100%)
                   Threshold: ≥ 35% → PASS
```

### Step 2 — Document Verification (OCR)

```
ID Photo ──► Grayscale + Otsu Threshold ──► Pytesseract OCR
                                                  │
                                                  ▼
                                        Extracted Text (raw)
                                                  │
                              ┌───────────────────┴──────────────────┐
                              ▼                                       ▼
                     Name Match Check                      ID Number Regex
                  (first name / last name                  (\d{4} \d{4} \d{4}
                   found in OCR text?)                      PAN pattern, etc.)
                              │                                       │
                              └───────────────┬───────────────────────┘
                                              ▼
                                    Confidence Score:
                                    Name match  → +60%
                                    ID found    → +40%
                                    Total ≥ 60% → PASS
```

### Step 3 — Fraud Detection

```
IF face_confidence < 10%
AND doc_confidence  < 10%
THEN fraud_flag = True
     fraud_reason = "Both verifications returned very low confidence"
```

### AI Output Summary

| Field | Type | Description |
|-------|------|-------------|
| `face_verified` | Boolean | Face match result |
| `document_verified` | Boolean | OCR match result |
| `face_confidence` | Float (%) | Face similarity score |
| `doc_confidence` | Float (%) | Document match score |
| `overall_confidence` | Float (%) | Average of both |
| `extracted_name` | String | Name found in ID by OCR |
| `extracted_id_number` | String | ID number found by OCR |
| `fraud_flag` | Boolean | Whether fraud was detected |
| `fraud_reason` | String | Reason for fraud flag |
| `verification_log` | Text | Full debug log |

---

## 10. Database Design

### Table: `users_customuser`

| Column | Type | Notes |
|--------|------|-------|
| id | BigInt PK | Auto |
| username | Varchar | Unique |
| first_name | Varchar | |
| last_name | Varchar | |
| email | Varchar | |
| password | Varchar | Hashed (PBKDF2) |
| role | Varchar | `user` / `admin` |
| phone | Varchar | |
| address | Text | |
| profile_photo | ImageField | upload_to: profile_photos/ |
| id_proof | ImageField | upload_to: id_proofs/ |
| date_of_birth | Date | |
| is_active | Boolean | |
| date_joined | DateTime | |

---

### Table: `buspass_route`

| Column | Type | Notes |
|--------|------|-------|
| id | BigInt PK | Auto |
| name | Varchar | Route name |
| source | Varchar | Start stop |
| destination | Varchar | End stop |
| distance_km | Float | |
| fare | Decimal | In rupees |
| is_active | Boolean | |

---

### Table: `buspass_buspass`

| Column | Type | Notes |
|--------|------|-------|
| id | BigInt PK | Auto |
| user_id | FK → CustomUser | |
| route_id | FK → Route | |
| pass_type | Varchar | monthly/quarterly/annual |
| status | Varchar | pending/approved/rejected/expired |
| pass_number | Varchar | Unique, auto-generated (BIQ + 8 digits) |
| valid_from | Date | Set on approval |
| valid_until | Date | Set on approval |
| qr_code | ImageField | upload_to: qr_codes/ |
| admin_remarks | Text | |
| created_at | DateTime | |
| updated_at | DateTime | |

---

### Table: `ai_module_aiverification`

| Column | Type | Notes |
|--------|------|-------|
| id | BigInt PK | Auto |
| user_id | FK → CustomUser | |
| face_verified | Boolean | |
| document_verified | Boolean | |
| face_confidence | Float | 0.0 – 100.0 |
| doc_confidence | Float | 0.0 – 100.0 |
| extracted_name | Varchar | OCR result |
| extracted_id_number | Varchar | OCR result |
| fraud_flag | Boolean | |
| fraud_reason | Text | |
| verification_log | Text | Full debug log |
| created_at | DateTime | |

---

### Table: `admin_panel_systemlog`

| Column | Type | Notes |
|--------|------|-------|
| id | BigInt PK | Auto |
| log_type | Varchar | info/warning/error/fraud |
| message | Text | |
| user_id | FK → CustomUser | Nullable |
| created_at | DateTime | |

---

## 11. API Endpoints & URL Routes

### User Routes (`/users/`)

| URL | View | Description |
|-----|------|-------------|
| `/users/register/` | `register_view` | New user registration |
| `/users/login/` | `login_view` | User login |
| `/users/logout/` | `logout_view` | Logout |
| `/users/dashboard/` | `dashboard_view` | User dashboard |
| `/users/profile/` | `profile_view` | Edit profile |

---

### Bus Pass Routes (`/buspass/`)

| URL | View | Description |
|-----|------|-------------|
| `/buspass/apply/` | `apply_pass_view` | Apply for new pass |
| `/buspass/my-passes/` | `view_pass_view` | View all passes |
| `/buspass/download/<id>/` | `download_pass_view` | Download pass with QR |
| `/buspass/renew/<id>/` | `renew_pass_view` | Submit renewal |

---

### AI Routes (`/ai/`)

| URL | View | Description |
|-----|------|-------------|
| `/ai/status/` | `verification_status_view` | View AI results |
| `/ai/re-verify/` | `re_verify_view` | Re-run AI verification |

---

### Admin Panel Routes (`/admin-panel/`)

| URL | View | Description |
|-----|------|-------------|
| `/admin-panel/dashboard/` | `admin_dashboard_view` | Stats overview |
| `/admin-panel/applications/` | `application_list_view` | All applications |
| `/admin-panel/applications/<id>/` | `application_detail_view` | Application detail |
| `/admin-panel/applications/<id>/approve/` | `approve_pass_view` | Approve |
| `/admin-panel/applications/<id>/reject/` | `reject_pass_view` | Reject |
| `/admin-panel/users/` | `user_list_view` | All users |
| `/admin-panel/fraud-alerts/` | `fraud_alerts_view` | Fraud list |

---

## 12. Installation & Setup

### Prerequisites

| Software | Version | Download |
|----------|---------|----------|
| Python | 3.10+ | https://python.org |
| PostgreSQL | 14+ | https://postgresql.org |
| Tesseract OCR | 5.x | https://github.com/UB-Mannheim/tesseract/wiki |
| Git | Any | https://git-scm.com |

### Tesseract PATH Setup (Windows)

After installing Tesseract, add it to System PATH:

```
Control Panel → System → Advanced System Settings
→ Environment Variables → Path → New
→ Add: C:\Program Files\Tesseract-OCR
```

Verify: `tesseract --version`

---

## 13. Running the Project

```bash
# ── Step 1: Activate virtual environment ──────────────────
venv\Scripts\activate              # Windows
# source venv/bin/activate         # Mac/Linux

# ── Step 2: Install dependencies ──────────────────────────
pip install -r requirements.txt

# ── Step 3: Create PostgreSQL database ────────────────────
# In pgAdmin or psql:
# CREATE DATABASE busiq_db;

# ── Step 4: Configure .env ────────────────────────────────
# Open .env and set:
# DB_PASSWORD=your_actual_postgres_password

# ── Step 5: Run migrations ────────────────────────────────
python manage.py makemigrations users
python manage.py makemigrations buspass
python manage.py makemigrations ai_module
python manage.py makemigrations admin_panel
python manage.py migrate

# ── Step 6: Load sample routes ────────────────────────────
python manage.py loaddata fixtures/initial_routes.json

# ── Step 7: Create admin user ─────────────────────────────
python create_admin.py

# ── Step 8: Load sample demo data ─────────────────────────
python setup_sample_data.py

# ── Step 9: Start server ──────────────────────────────────
python manage.py runserver
```

Open browser → **http://127.0.0.1:8000**

---

## 14. Login Credentials

| Role | Username | Password | Access |
|------|----------|----------|--------|
| **Admin** | `admin` | `admin@123` | Full admin panel |
| User 1 | `rahul_patil` | `Test@1234` | AI verified, pass approved |
| User 2 | `priya_sharma` | `Test@1234` | Partial verification |
| User 3 | `amit_desai` | `Test@1234` | Fully verified |

---

## 15. Screenshots Guide

| Page | URL | What to Show |
|------|-----|--------------|
| Login | `/users/login/` | Clean login form with BusIQ branding |
| Register | `/users/register/` | Multi-field form with photo upload |
| User Dashboard | `/users/dashboard/` | Pass status + AI verification banner |
| Apply for Pass | `/buspass/apply/` | Route + type selection |
| View Passes | `/buspass/my-passes/` | Pass cards with status badges |
| Download Pass | `/buspass/download/<id>/` | Digital pass card with QR code |
| AI Verification | `/ai/status/` | Progress bars + confidence scores |
| Admin Dashboard | `/admin-panel/dashboard/` | Stats cards + recent applications |
| Application Detail | `/admin-panel/applications/<id>/` | Full review with approve/reject |
| Fraud Alerts | `/admin-panel/fraud-alerts/` | Fraud case list |

---

## 16. Security Features

| Feature | Implementation |
|---------|---------------|
| Password hashing | Django PBKDF2-SHA256 |
| CSRF protection | Django CSRF middleware on all forms |
| Role-based access | Custom `admin_required` decorator |
| Login required | `@login_required` on all protected views |
| Secure file upload | Django `ImageField` with media root isolation |
| AI fraud detection | Automatic flagging of suspicious submissions |
| Environment secrets | `python-decouple` reads from `.env` file |
| SQL injection | Django ORM (parameterized queries) |

---

## 17. Fixtures & Sample Data

### Fixture Files

| File | Contents |
|------|----------|
| `fixtures/initial_routes.json` | 7 bus routes (Nashik area) |
| `fixtures/sample_ai_verifications.json` | 2 AI verification records |
| `fixtures/sample_logs.json` | 3 system log entries |

### Loading Order

```bash
# Must load in this order (due to foreign key dependencies):
python manage.py loaddata fixtures/initial_routes.json
python create_admin.py
python setup_sample_data.py   # creates users, passes, AI records, logs
```

---

## 18. Troubleshooting

### ❌ `psycopg2.OperationalError` — Cannot connect to database
```
Fix: Make sure PostgreSQL is running
     Check DB_PASSWORD in .env is correct
     Verify database 'busiq_db' exists
```

### ❌ `pytesseract.pytesseract.TesseractNotFoundError`
```
Fix: Install Tesseract from https://github.com/UB-Mannheim/tesseract/wiki
     Add C:\Program Files\Tesseract-OCR to System PATH

     OR add to bottom of settings.py:
     import pytesseract
     pytesseract.pytesseract.tesseract_cmd = r'C:\Program Files\Tesseract-OCR\tesseract.exe'
```

### ❌ `ModuleNotFoundError`
```
Fix: Activate virtual environment first:
     venv\Scripts\activate
     pip install -r requirements.txt
```

### ❌ Images not showing / 404 on media files
```
Fix: Make sure MEDIA_URL and MEDIA_ROOT are in settings.py
     URLs should include: + static(MEDIA_URL, document_root=MEDIA_ROOT)
```

### ❌ `No module named 'cv2'`
```
Fix: pip install opencv-contrib-python==4.13.0.92
```

---

## 19. Future Enhancements

| Feature | Description |
|---------|-------------|
| 📱 Mobile App | React Native or Flutter app for students |
| 💳 NFC Passes | Tap-to-verify using NFC-enabled phones |
| 🛰️ GPS Validation | Verify travel using real-time GPS location |
| 💰 Payment Gateway | Razorpay/Paytm integration for online fees |
| 📊 Analytics Dashboard | Pass usage patterns, route popularity |
| 🤖 Advanced AI | Deep learning face recognition (FaceNet/DeepFace) |
| 📧 Email Notifications | Email alerts for approval/rejection |
| 🔔 SMS Alerts | SMS via Twilio on pass status change |
| 🌐 Multi-language | Support for Marathi, Hindi |

---

## 20. Conclusion

BusIQ successfully modernizes public transport pass management by combining:

- **Django's robust web framework** for reliable backend operations
- **OpenCV face detection** for identity verification
- **Tesseract OCR** for document text extraction and validation
- **PostgreSQL** for production-grade data storage
- **Bootstrap 5** for a clean, responsive user interface

The system eliminates manual paperwork, reduces fraud through AI verification, and provides a seamless digital experience for both commuters and transport authorities.

---

> **Viva Ready Line:**
> *"BusIQ is a Django-based AI-assisted bus pass management system that automates identity verification using OpenCV face recognition and Tesseract OCR, with role-based access for users and admins, digital QR code pass generation, fraud detection, and PostgreSQL as the production database."*

---

*Made with ❤️ | BusIQ Project | Django + AI*
