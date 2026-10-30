# Dream Estate — Full-stack real estate internship project

**Stack:** HTML, CSS, vanilla JavaScript, Leaflet/OpenStreetMap, Three.js, Node.js, Express and SQLite (`better-sqlite3`).

## Why SQLite?
SQLite stores everything in a local database file and needs no separate database server or account. It is easy for a beginner to run and demonstrate locally. For production or multiple app servers, use a managed database, secure session store, backups and stronger deployment security.

## Important sample-data warning
The 10 listings seeded in this project are **sample demonstration data only**. Coordinates represent approximate Chennai neighbourhoods, not surveyed property parcels. Prices, amenities, availability and listing descriptions are not verified offers. Replace them with verified data before real-world use. Nearby schools/hospitals/water results are fetched from OpenStreetMap Overpass when available; map coverage can be incomplete. Distances use the Haversine formula and are straight-line estimates, not road distances.

## Requirements and start-up
Install Node.js 18+ (LTS recommended). In a terminal opened inside this folder:
1. `npm install`
2. Copy `.env.example` to `.env` (`copy .env.example .env` in Windows CMD; `Copy-Item .env.example .env` in PowerShell; `cp .env.example .env` on macOS/Linux).
3. Edit `.env`: set a strong `SESSION_SECRET` and your chosen `ADMIN_PASSWORD`.
4. Run `npm run seed`
5. Run `npm start`
6. Open http://localhost:3000 (site), http://localhost:3000/admin.html (admin), or http://localhost:3000/api/health (health check).

The seed command inserts the 10 demo properties only if the properties table is empty. Stop the server with Ctrl+C.

## Photos
Admin → sign in → Add property/Edit property → choose up to five JPG/PNG/WEBP/AVIF images (max 5 MB each). Uploads are stored in `public/uploads/`. Sample records have no photos by default and display a clean placeholder. You may put owned/licensed photos in `public/images/`; use the admin uploader or save `/images/filename.jpg` as a property's photo path in SQLite. Never upload images without permission.

## Optional email
Enquiries are saved even when email is not configured. To enable notifications, set `OWNER_EMAIL`, `SMTP_HOST`, `SMTP_PORT`, `SMTP_SECURE`, `SMTP_USER`, `SMTP_PASS`, and `SMTP_FROM` in `.env`, using credentials from your SMTP provider. For Gmail, use an App Password where supported, not your regular password. Restart the server after changing `.env`.

## Test it
- Search listings and change purpose, type, status, bedrooms, furnishing, max price and sort.
- Click map markers. Green=Available, orange=Reserved, red=Sold. Click Directions for Google Maps.
- Select nearby schools, hospitals or water sources and click Find nearby; results are real OpenStreetMap objects when available.
- Open a property detail page, send an enquiry with valid name/phone/email/address. Admin → Enquiries displays the saved record.
- Open an Available listing and submit Buy / book with valid details. The backend transaction marks it Sold, saves customer/sale data and records an enquiry. Another buy request gets HTTP 409 (“just sold”). Open pages poll every five seconds for status changes.
- Admin can add/edit/delete listings, change status, search enquiries/customers and export customers as CSV.
- To reset demo data: stop the server, delete `database/dream-estate.sqlite`, run `npm run seed`, then `npm start`. This deletes local enquiries and customers too.

## REST API
Public:
- `GET /api/health`
- `GET /api/properties?q=&purpose=&type=&status=&furnishing=&minPrice=&maxPrice=&bedrooms=&sort=`
- `GET /api/properties/:id`
- `GET /api/stats`
- `GET /api/changes`
- `POST /api/properties/:id/enquiry`
- `POST /api/properties/:id/buy`

Admin (login required):
- `POST /api/admin/login`, `POST /api/admin/logout`, `GET /api/admin/session`
- `GET /api/admin/enquiries?q=`
- `GET /api/admin/customers?q=`
- `GET /api/admin/customers.csv`
- `POST /api/admin/properties` (multipart form, optional photos)
- `PUT /api/admin/properties/:id` (multipart form, optional replacement photos)
- `DELETE /api/admin/properties/:id`
- `PATCH /api/admin/properties/:id/status`

## Folder structure
```text
dream-estate-fullstack/
├── server.js
├── package.json
├── .env.example
├── .gitignore
├── README.md
├── database/db.js
├── database/seed.js
├── utils/email.js
└── public/
    ├── index.html
    ├── property.html
    ├── admin.html
    ├── style.css
    ├── script.js
    ├── property.js
    ├── admin.js
    ├── 3d.js
    ├── images/
    └── uploads/
```

## Internship presentation points
- How the frontend calls Express REST endpoints using `fetch`.
- Why SQLite tables separate properties, enquiries, customers and sales.
- Server-side validation and atomic status updates prevent two successful buy attempts.
- Polling updates SOLD badges without a page refresh.
- Leaflet renders real OpenStreetMap tiles and markers; Haversine calculates straight-line distance to live mapped places.
- Admin session, photo uploads, email notifications and CSV export.
- Three.js creates a lightweight rotating decorative building.

## Security and scope
Educational local demo, not a real transaction service. “Buy / book” is only a demo status change; it does not handle payment, verify property ownership or form a legal agreement. Before deployment, add HTTPS, CSRF protection, stronger admin auth, production session storage, backups, privacy/legal notices and monitoring.
