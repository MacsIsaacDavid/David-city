# David City — Hotel & Apartment Comparison Demo

David City is a responsive hotel-comparison website with a vanilla HTML/CSS/JavaScript frontend and an Express REST API. It includes illustrative accommodation cards, search, sorting, filters, a property details modal, and a demo booking request flow.

**Important:** This is a learning/demo project. Prices and listings are illustrative, ratings are not verified, and no live hotel provider or payment service is connected. A booking reference from this project is not a confirmed real reservation.

## 1. Requirements
- Node.js 18 or newer
- MySQL 8.x (optional for viewing demo listings; required for persistent bookings and contact messages)
- VS Code

## 2. Install dependencies
Open the project folder in VS Code. Open **Terminal → New Terminal**, then run:

```bash
npm install
```

Copy `.env.example` to a new file named `.env`. Replace the database password with your own local MySQL password. Do not upload `.env` to GitHub.

## 3. Set up MySQL
Start MySQL, then run these SQL files in order using MySQL Workbench or the MySQL command line:

```sql
SOURCE database/schema.sql;
SOURCE database/seed.sql;
```

If using the command line, first navigate to the project directory and run:

```bash
mysql -u root -p < database/schema.sql
mysql -u root -p david_city < database/seed.sql
```

The SQL seed adds sample listings and sample room inventory. Inventory is only sample data and must not be used for real reservations.

## 4. Run the site
In the VS Code terminal:

```bash
npm start
```

The API starts at `http://localhost:3000`. Open `frontend/index.html` in a browser. For a smoother local workflow, install the VS Code **Live Server** extension and choose **Open with Live Server** on `frontend/index.html`.

If MySQL is not configured, the API returns demonstration listings in memory. Persistent bookings and contact messages require a working database.

## 5. Test API endpoints
Open these URLs in your browser:
- `http://localhost:3000/api/health`
- `http://localhost:3000/api/properties`
- `http://localhost:3000/api/properties/1`
- `http://localhost:3000/api/search?destination=Lagos&maxPrice=400000&minStars=3`
- `http://localhost:3000/api/properties/1/availability?checkin=2026-10-20&checkout=2026-10-22`

Example booking request using curl:

```bash
curl -X POST http://localhost:3000/api/bookings \
  -H "Content-Type: application/json" \
  -d "{\"propertyId\":1,\"guestName\":\"Demo Guest\",\"email\":\"guest@example.com\",\"phone\":\"+2348000000000\",\"checkIn\":\"2026-11-10\",\"checkOut\":\"2026-11-12\",\"guests\":2}"
```

Example contact request:

```bash
curl -X POST http://localhost:3000/api/contact \
  -H "Content-Type: application/json" \
  -d "{\"name\":\"Demo Guest\",\"email\":\"guest@example.com\",\"message\":\"I have a question about this demo.\"}"
```

## 6. Beginner explanation
- `frontend/index.html` defines the page structure and form controls.
- `frontend/styles.css` handles colors, layout, responsive breakpoints, cards, and modal styling.
- `frontend/app.js` handles search, filters, sorting, dates, property details, and booking requests.
- `backend/server.js` exposes JSON endpoints, validates requests, and uses parameterized SQL queries.
- `database/schema.sql` creates the database tables and relationships.
- `database/seed.sql` adds the sample properties, photos, room options, and inventory.
- `.env` stores local database settings and is excluded from Git.

## 7. Troubleshooting
- **`npm` is not recognized:** install a current Node.js LTS release, close and reopen VS Code, then check `node -v` and `npm -v`.
- **Cannot connect to MySQL:** confirm MySQL is running and `.env` values match your local setup.
- **Port 3000 is already in use:** change `PORT=3001` in `.env` and update `API_BASE` in `frontend/app.js` to `http://localhost:3001/api`.
- **Browser says Failed to fetch:** start the backend with `npm start`. Check `http://localhost:3000/api/health`. If opening the HTML directly, CORS is enabled for this demo.
- **Images do not appear:** the sample images use remote Unsplash URLs and need an internet connection. Replace them with images you own or have permission to use.
- **Database error on booking:** ensure the schema and seed files were both imported, including room inventory for the chosen dates.
- **Frontend only shows demo mode:** this is expected if the API is offline or the API request fails.

## 8. Deployment notes
The frontend can be hosted on a static host such as GitHub Pages, Netlify, or Vercel. Deploy the backend separately to a Node-capable host and attach a managed MySQL database. Set environment variables in the backend host dashboard. Update `API_BASE` in `frontend/app.js` to your deployed API URL and configure CORS to allow only your deployed frontend domain.

Before production, add authentication for administrators, rate limiting, stronger validation, structured logging, HTTPS, email verification, privacy terms, abuse protection, and a proper inventory model. Use provider contracts/API integrations for actual hotel rates and reservations. The current booking route is a demo request flow, not a production-grade booking engine.
