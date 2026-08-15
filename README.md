# SMart

A campus marketplace web application for students at the Singapore Institute of Management, supporting buying, selling, trading and donating items within the student community.

Built by Team 58 for CM2020 Agile Software Projects, BSc Computer Science, University of London.

[Live demo](https://smart-k1xu.onrender.com/) · [Demo video](https://drive.google.com/file/d/1T8clmBOWnLLcRw6_3YfdbfHe7PjVF7ig/view)

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

### Demo accounts

Register from the sign-up page, or read the seed accounts out of `backend/db_schema.sql` after building the database locally.

Seed accounts defined in `db_schema.sql`, for local testing only.

### If the database fails to build

`npm run build-db` pipes the schema through the `sqlite3` CLI. If that errors, run the schema directly to see the underlying SQL error:

```bash
sqlite3 backend/database.db < backend/db_schema.sql
```

## Security

Authentication was reworked after the original submission:

- Passwords are hashed with bcrypt instead of stored and compared in plaintext
- Login looks users up by email, then verifies the hash separately
- Submitted credentials are no longer written to application logs
- The password hash is stripped from the user object before it enters the session store
- The session secret is read from the `SESSION_SECRET` environment variable

## Notes on the live demo

Hosted on Render's free tier. The first request after a period of inactivity can take up to a minute while the instance wakes. The filesystem is ephemeral, so data created on the live site does not survive a restart. Run it locally for a full picture.
