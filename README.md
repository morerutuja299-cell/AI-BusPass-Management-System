# BusIQ — AI Based Automatic Bus Pass Management System
### Django + PostgreSQL + OpenCV + Tesseract OCR

---

## QUICK START (Step-by-Step)

### STEP 1 — Install PostgreSQL
Download from: https://www.postgresql.org/download/
- During install, set a password for the `postgres` user (remember it!)
- Default port: 5432

### STEP 2 — Create the database
Open pgAdmin OR open Command Prompt and type:
```
psql -U postgres
CREATE DATABASE busiq_db;
\q
```

### STEP 3 — Install Tesseract OCR (for AI document reading)
Windows: https://github.com/UB-Mannheim/tesseract/wiki
- Download and install the .exe
- During install, note the path (usually: C:\Program Files\Tesseract-OCR\tesseract.exe)
- Add Tesseract to your System PATH:
  Control Panel → System → Advanced → Environment Variables → Path → New
  Add: C:\Program Files\Tesseract-OCR

Verify: Open new CMD and type:  tesseract --version

### STEP 4 — Set up virtual environment
```
cd BusIQ
python -m venv venv
venv\Scripts\activate
```

### STEP 5 — Install Python packages
```
pip install -r requirements.txt
```

### STEP 6 — Configure .env file
Copy the `.env.example` file to `.env` and set your PostgreSQL password:
```
copy .env.example .env
```

Then open the `.env` file and update the database password:
```
DB_PASSWORD=your_actual_postgres_password
```

### STEP 7 — Run database migrations
```
python manage.py makemigrations users
python manage.py makemigrations buspass
python manage.py makemigrations ai_module
python manage.py makemigrations admin_panel
python manage.py migrate
```

### STEP 8 — Load routes fixture
```
python manage.py loaddata fixtures/initial_routes.json
```

### STEP 9 — Create admin user
```
python create_admin.py
```

### STEP 10 — Load sample data (optional but recommended for demo)
```
python setup_sample_data.py
```

### STEP 11 — Run the server
```
python manage.py runserver
```

### STEP 12 — Open in browser
http://127.0.0.1:8000

---

## LOGIN CREDENTIALS

| Role  | Username     | Password   |
|-------|-------------|------------|
| Admin | admin        | admin@123  |
| User  | rahul_patil  | Test@1234  |
| User  | priya_sharma | Test@1234  |
| User  | amit_desai   | Test@1234  |

---

## HOW TO USE THE SYSTEM

### As a NEW USER:
1. Go to http://127.0.0.1:8000/users/register/
2. Fill in your details
3. Upload a clear PROFILE PHOTO and an ID PROOF (Aadhar/PAN)
4. Submit — AI verification runs automatically
5. If AI verification passes, go to Apply for Pass
6. Wait for admin approval
7. Download your digital QR pass

### As ADMIN:
1. Login with admin / admin@123
2. View pending applications in Applications menu
3. Click Review to see user details + AI verification result
4. Approve or Reject with remarks
5. Approved passes get a QR code automatically

---

## PROJECT STRUCTURE

```
BusIQ/
├── busiq_backend/     # Main Django project settings
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── users/             # User registration, login, profile
├── buspass/           # Bus pass application, routes
├── ai_module/         # Face + OCR verification
├── admin_panel/       # Admin dashboard, approve/reject
├── templates/         # All HTML templates
├── fixtures/          # Sample data JSON files
├── media/             # Uploaded photos (auto-created)
├── static/            # CSS/JS static files
├── .env               # Environment config (edit this!)
├── manage.py
├── create_admin.py    # Run once to create admin
├── setup_sample_data.py  # Run once to load demo data
└── requirements.txt
```

---

## TROUBLESHOOTING

**psycopg2 error:**
Make sure PostgreSQL is running and DB_PASSWORD in .env is correct.

**pytesseract error:**
Make sure Tesseract is installed and added to PATH.
Or add this line to settings.py:
  pytesseract.pytesseract.tesseract_cmd = r'C:\Program Files\Tesseract-OCR\tesseract.exe'

**No module named X:**
Make sure your venv is activated:  venv\Scripts\activate
Then:  pip install -r requirements.txt

**Static files 404:**
Run:  python manage.py collectstatic

---

## VIVA READY LINE
"BusIQ is a Django-based AI-assisted bus pass management system that automates
 identity verification using OpenCV face detection and Tesseract OCR, with
 role-based access for users and admins, digital QR code pass generation,
 and PostgreSQL as the production database."
