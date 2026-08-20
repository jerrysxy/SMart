# SMart

A campus marketplace web application for students at the Singapore Institute of Management, supporting buying, selling, trading and donating items within the student community.

Built by Team 58 for CM2020 Agile Software Projects, BSc Computer Science, University of London.

[Demo video](https://drive.google.com/file/d/1T8clmBOWnLLcRw6_3YfdbfHe7PjVF7ig/view) · [Live demo](https://smart-k1xu.onrender.com/) (Render free tier, first request after inactivity can take up to a minute)

## Features

- Listing creation with image upload, categories and keyword search
- Offers, favourites and peer reviews between users
- Session-based authentication restricted to SIM email addresses
- User profiles with course details and seller ratings

## Tech stack

| Layer | Technology |
| --- | --- |
| Backend | Node.js, Express |
| Views | EJS |
| Database | SQLite |
| Auth | express-session, bcrypt password hashing |
| File uploads | multer |
| Hosting | Render (backend), Vercel (frontend) |

## Database design

Six normalised tables with foreign key constraints, `CHECK` validation and cascade rules, with `PRAGMA foreign_keys` enabled. Every database call uses parameterised queries. Full schema in [`backend/db_schema.sql`](backend/db_schema.sql).

## Security

The original submission stored and compared passwords in plaintext. Four findings were raised in a later code review and closed:

1. **Plaintext password storage and comparison.** Passwords are hashed with bcrypt. Login looks the user up by email, then verifies the hash separately.
2. **Credentials written to application logs.** The log line carrying submitted credentials was removed.
3. **Password hash carried into the session store.** The hash is stripped from the user object before the session is created.
4. **Hardcoded session secret.** The secret is read from the `SESSION_SECRET` environment variable.

## Running locally

Requires **Node 20**. Newer versions have no prebuilt binary for `sqlite3` and will try to compile it from source.

```bash
cd backend
npm install
npm run build-db
npm start
```

Then open <http://localhost:3000>.

The backend serves the EJS views and static assets from `../frontend`, so there is no separate frontend step.

### Accounts

Register from the sign-up page. Seeded accounts exist for local testing only and are not intended for use.

### If the database fails to build

`npm run build-db` pipes the schema through the `sqlite3` CLI. If that errors, run the schema directly to see the underlying SQL error:

```bash
sqlite3 backend/db_schema.sql
```

## Notes on the live demo

Hosted on Render's free tier. The filesystem is ephemeral, so data created on the live site does not survive a restart. Run it locally for a full picture.
