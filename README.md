# NagarPulse AI

**Your City. Your Voice. Your Impact.**

NagarPulse AI is an independent civic-tech prototype for citizen issue reporting, issue triage, map-based visibility, community participation and administrative tracking. It is designed for a buildathon demonstration and is not affiliated with any government body.

## What works in this prototype

- Citizen registration and login (JWT sessions; bcrypt password hashes).
- Seeded citizen and admin demo accounts.
- Submit civic reports with JPG/PNG/WEBP image upload (5 MB limit), category, severity, location coordinates and neighbourhood.
- Browser geolocation is requested only after the user clicks **Use my current location**.
- Text/keyword-assisted category suggestion. This is not a trained AI vision model and does not inspect image contents.
- Basic nearby duplicate suggestion based on category and coordinate proximity.
- Severity-based priority score with small safety/urgency adjustments.
- Public issue explorer, support/upvote action, status badges and Leaflet/OpenStreetMap maps.
- Admin-only issue status updates.
- Community leaderboard, points, activity feed and volunteer sign-up.
- Category and neighbourhood analytics.
- Local JSON persistence, so no database server is needed for the first test.

## Requirements

- Node.js 20 or later (Node 22 LTS recommended)
- npm
- Internet access for Google Fonts, Lucide icons and OpenStreetMap tiles (the app itself runs locally)

## Run on Windows PowerShell

1. Extract the ZIP file.
2. Open PowerShell in the `NagarPulse-AI` folder.
3. Install dependencies:

   ```powershell
   npm install
   ```

4. Optional: copy `.env.example` to `.env` and change the JWT secret and admin password **before first launch**. The initial seed credentials are created only when `data/db.json` does not exist.
5. Start the app:

   ```powershell
   npm start
   ```

6. Open <http://localhost:3000>.

## Demo accounts

| Role | Email | Password |
|---|---|---|
| Citizen | `aarav@example.com` | `Citizen123!` |
| Admin | `admin@nagarpulse.local` | `ChangeMe123!` |

Change demo credentials and JWT secret before any public deployment. Do not use real personal information in this prototype.

## Project structure

```text
NagarPulse-AI/
├── backend/server.js       # Express API, auth, uploads, scoring and JSON store
├── frontend/index.html     # Application shell and views
├── frontend/styles.css     # Responsive visual system
├── frontend/app.js         # Client-side application logic
├── data/db.json            # Seeded demo database (created on first run)
├── uploads/                # Uploaded issue photos
├── documentation/          # Project notes
├── .env.example            # Environment variable template
└── package.json
```

## API overview

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/api/health` | Health check |
| POST | `/api/auth/register` | Create citizen account |
| POST | `/api/auth/login` | Sign in |
| GET | `/api/auth/me` | Current account |
| GET | `/api/issues` | List issues; optional `status`, `category`, `ward` filters |
| POST | `/api/issues/analyze` | Keyword-assisted category suggestion (auth required) |
| POST | `/api/issues` | Create report with optional image (auth required) |
| POST | `/api/issues/:id/upvote` | Support a report (auth required) |
| PATCH | `/api/issues/:id` | Update status/severity/ward (admin only) |
| GET | `/api/dashboard/summary` | Aggregate demo analytics |
| GET | `/api/leaderboard` | Community points ranking |
| GET | `/api/activities` | Recent activity feed |
| POST | `/api/volunteer` | Join a volunteer activity (auth required) |

## Important limitations and responsible-use notes

1. **AI:** Current categorization is a transparent keyword-based helper. It does not use computer vision, a YOLO model, or a cloud vision API. A real image model needs to be trained/evaluated and integrated separately.
2. **Database:** JSON file persistence is suitable for a local prototype only. Replace with MongoDB/PostgreSQL before concurrent or production use.
3. **Duplicate detection:** The current proximity rule is a simple heuristic, not semantic image matching. A duplicate is suggested, not proof that two reports describe the same defect.
4. **Maps:** Demo issue coordinates are illustrative Noida locations. Real reports use coordinates entered or shared by the citizen. OpenStreetMap tiles require internet access and attribution.
5. **Government workflows:** No municipal authority integration, dispatch, SLA, official ward boundary or government budget feed is connected. Statuses are managed inside this prototype only.
6. **Security hardening:** Use HTTPS, a strong secret, stricter CORS, CSRF protections if cookie auth is introduced, rate limiting, file scanning, audit logs, backups and managed storage before deployment. Restrict uploads and validate all user input.
7. **Privacy:** Ask consent before using geolocation; avoid publishing faces, number plates, phone numbers or other personal data. Do not upload sensitive images.
8. **Demo data:** Seeded records, accounts, counts, points and map pins are synthetic demonstration data and must not be presented as official city statistics or real-world impact.

## Reset demo data

Stop the server, delete `data/db.json` and restart `npm start`. The app will recreate the sample users and issue records. Uploaded photos in `uploads/` are not automatically deleted.

## Buildathon delivery extensions

- Connect a validated vision model for image-based issue detection.
- Replace local JSON with MongoDB and add indexes/geospatial queries.
- Add ward boundaries, verified public datasets and source attribution.
- Add email/WhatsApp/SMS notifications with consent.
- Add an audit trail and evidence-based resolution verification.
- Deploy frontend/backend and provide a public landing page and demo videos.
