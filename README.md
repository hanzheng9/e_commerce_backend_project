# EasyShopas — E-commerce Backend (FastAPI)

A small FastAPI backend for a simple e-commerce demo. It provides user registration, JWT-based authentication, business and product CRUD, image upload for profiles and products, and email verification via SMTP.

Key components
- FastAPI application entrypoint: `main.py`
- ORM models: `models.py` (Tortoise ORM, SQLite by default)
- Auth helpers: `authentication.py` (password hashing and JWT)
- Email sending: `emails.py` (uses `fastapi-mail` and SMTP credentials from `.env`)
- Templates: `templates/verification.html`
- Static files served from: `static/` (images uploaded to `static/images/`)

Prerequisites
- Python 3.10+ (the project was developed with Python 3.12-compatible libraries)
- Bash shell (instructions below target macOS / bash)

Environment variables
Create a `.env` file in the project root with at least these keys:

```
EMAIL=youremail@example.com
PASS=your-email-password-or-app-password
SECRET=some-very-secret-key
```

- `EMAIL` and `PASS` are used by the SMTP client in `emails.py` (the example uses Gmail SMTP).
- `SECRET` is used to sign JWTs.

Install and run (bash)
The minimal steps below set up a virtual environment, install dependencies, and run the app with Uvicorn.

```bash
# from project root
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt

# create a .env file (see sample keys above)
# ensure `static/images/` exists (the app writes uploaded images there)
mkdir -p static/images

# run the app
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

What the server exposes (selected endpoints)
- POST /registration — create a new user (sends verification email)
- POST /token — obtain JWT token (OAuth2 password grant flow)
- GET /verification/?token=... — verify email token (returns HTML template)
- POST /uploadfile/profile — upload profile/logo image (authenticated)
- POST /uploadfile/product/{id} — upload product image (authenticated)
- POST /products — add a product (authenticated)
- GET /product — list products
- GET /product/{id} — get single product + business info
- PUT /product/{id} — update product (authenticated, owner-only)
- DELETE /products/{id} — delete product (authenticated, owner-only)

Notes and tips
- The project uses an on-disk SQLite database by default (file: `database.sqlite3`). No DB setup is required.
- Email sending requires valid SMTP credentials. For Gmail, use an App Password or allow less secure apps (not recommended).
- Static files are served from `/static`. Uploaded filenames are randomized and stored under `static/images/`.
- If you change dependencies, update `requirements.txt` and reinstall.

Troubleshooting
- If JWT auth fails, confirm `SECRET` in `.env` is set and the server was restarted after changes.
- If email doesn't arrive, check SMTP credentials, port, and whether the provider blocks sign-in from the app.

License
- No license declared. Use accordingly.
