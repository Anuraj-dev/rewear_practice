<h1 align="center">ReWear</h1>

<p align="center">
  A Flask practice app for a sustainable fashion marketplace.<br />
  Register, sign in, and open a dashboard with your account details.
</p>

<p align="center">
  <a href="#running-it">Run it locally</a>
  ·
  <a href="#routes">Routes</a>
</p>

ReWear is a practice project for a marketplace where people sell the clothes they no longer wear. Right now it covers accounts. You can register, sign in, and open a dashboard. Listing items, browsing and purchases are not built yet.

It is one Flask app in `app.py`. It stores users in SQLite, keeps sign-in sessions with Flask-Login, and renders Jinja templates styled with Bootstrap 5 and Font Awesome from CDNs, so the browser needs internet access. You need Python 3 and pip. No API keys or accounts are required.

## What you get

- **Registration checks the form first.** All four fields are required, the email must match a pattern, and the two passwords must match. A password needs at least 8 characters, an uppercase letter, a lowercase letter and a digit. Nothing is written until every check passes.
- **Passwords are stored as hashes.** Registration saves `generate_password_hash(password)` in the `password` column. Sign-in compares the input with `check_password_hash`, so the plain password never reaches the database.
- **Usernames and emails are unique.** Registration rejects a taken email or username with a message. The `User` model also marks both columns as unique, so the database enforces the same rule.
- **Sign-in uses Flask-Login sessions.** A successful sign-in calls `login_user`. An anonymous visit to `/dashboard` or `/logout` redirects to `/login?next=...`, and sign-in sends you back to the page you asked for.
- **Emails are case-insensitive.** Registration and sign-in trim and lowercase the email, so `Alex@Example.com` and `alex@example.com` open the same account.

## Running it

### Set up

```sh
git clone https://github.com/Anuraj-dev/rewear_practice.git
cd rewear_practice
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

On Windows, run `.venv\Scripts\activate` instead of the `source` line.

### Create the database

The app does not create its tables on startup, and the repo has no `migrations/` folder. Run this once from the project folder:

```sh
python3 -c "from app import app, db; app.app_context().push(); db.create_all()"
```

This creates `instance/site.db`. Skip this step and registering fails with a server error.

### Start the server

```sh
python3 app.py
```

Flask starts in debug mode on http://127.0.0.1:5000. Open it, choose Register, and create an account. Then sign in to reach the dashboard.

## Routes

| Path | What it does |
| --- | --- |
| `/` | Home page. Shows Get started and Sign in, or a link to the dashboard when you are signed in. |
| `/register` | Creates an account, then sends you to the sign-in page. |
| `/login` | Signs you in and honors a `next` parameter after sign-in. |
| `/dashboard` | Shows your username, email and join date. Requires sign-in. |
| `/logout` | Signs you out and returns to the home page. Requires sign-in. |

## What is not built yet

- Listing, browsing and purchases have no routes. The dashboard counters are hardcoded to `0`, and its three buttons have no handlers.
- `app.py` sets `SECRET_KEY` to a placeholder string and runs with `debug=True`. Change both before you run this anywhere other people can reach.
- There is no migration history. `app.py` sets up Flask-Migrate, but the repo has no `migrations/` folder. `db.create_all()` adds missing tables and leaves existing ones alone. After a schema change, delete `instance/site.db`, which removes existing accounts, then run the create step again.
- `requirements.txt` lists Flask-WTF, Flask-Mail and python-dotenv, but no module imports them yet.

## License

MIT. See [LICENSE](LICENSE).
