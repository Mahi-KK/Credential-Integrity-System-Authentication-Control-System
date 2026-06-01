# Password Reuse Detection System

A Django + Bootstrap mini project that detects whether logged-in users reuse passwords across multiple accounts without storing plain-text passwords. The app is MySQL-ready and includes a dark red dashboard UI.

## Objective

Detect accounts that share the same password and generate a readable risk report.

- Login and registration system
- Private dashboard per user
- Store account password records securely
- Hash passwords with PBKDF2 and a unique salt per account
- Compare secret-keyed reuse fingerprints
- Detect duplicate/reused passwords across services
- Generate and download audit reports
- MySQL configuration with Docker Compose support

## Modules

- `Account Entry`: collects account details and stores records
- `Hash Passwords`: creates salted password hashes and reuse fingerprints
- `Compare Passwords`: groups matching fingerprints
- `Generate Report`: writes a timestamped report file

## How It Works

The system stores two password-derived values:

1. A salted PBKDF2 password hash, which is the stored password hash.
2. A secret-keyed HMAC fingerprint, which allows exact password reuse detection.

Per-account salts make normal hashes different even when two users reuse the same password. The HMAC fingerprint solves that mini-project requirement while still avoiding plain-text password storage.

## Install

```powershell
python -m pip install -r requirements.txt
```

## Run Django App

```powershell
python manage.py migrate
python manage.py runserver 127.0.0.1:8000
```

Then open:

```text
http://127.0.0.1:8000
```

## Optional MySQL Setup

Start MySQL:

```powershell
docker compose up -d mysql
```

Then set these environment variables before running migrations:

```powershell
$env:DB_ENGINE="mysql"
$env:MYSQL_DATABASE="password_reuse_db"
$env:MYSQL_USER="password_reuse_user"
$env:MYSQL_PASSWORD="password_reuse_pass"
$env:MYSQL_HOST="127.0.0.1"
$env:MYSQL_PORT="3306"
python manage.py migrate
python manage.py runserver 127.0.0.1:8000
```

If `DB_ENGINE` is not set to `mysql`, the app uses local SQLite so it can run immediately during demos and tests.

## Run Tests

```powershell
python -m unittest discover -s tests
python manage.py test dashboard
```

## Generated Files

Runtime files are created automatically:

- `data/accounts.json`
- `data/reuse_secret.key`
- `db.sqlite3`
- `reports/password_reuse_report_*.txt`

These files are ignored by Git because they may contain sensitive project data.

## Legacy CLI

The original terminal demo is still available:

```powershell
python main.py
```
