# Cinemate

Cinemate is a ticketing system for cinemas that lets guests browse films and showtimes, select seats, purchase or reserve tickets online, and enter the cinema using a digital ticket (QR/ barcode). It's designed to be extensible for multiple venues and to support both guest purchases and reserved bookings.

## Table of contents

- [Features](#features)
- [Quick start](#quick-start)
- [Architecture & Design](#architecture--design)
- [API (examples)](#api-examples)
- [Development](#development)
- [Contributing](#contributing)
- [License](#license)

## Features

- Browse movies, posters and trailers
- View showtimes by date and screen
- Seat map with real-time availability
- Buy or reserve tickets online (guest / account flows)
- Digital ticket delivery (QR or barcode) and entry validation
- Admin panel for creating movies, showtimes and managing screens
- Optional integrations: payment gateways, email/SMS, analytics

## Quick start

These instructions outline a typical development setup. Adjust commands to your stack if different.

1. Clone the repo:

```bash
git clone <repo-url>
cd cinemate
```

2. Create environment config based on `.env.example` (if present):

```bash
cp .env.example .env
# Edit .env to set DB, payment provider keys, etc.
```

3. Install dependencies and run database migrations (example commands — replace if your project uses a different toolchain):

```bash
# install
npm install

# run migrations
npm run migrate

# start services
npm run dev
```

4. Open the frontend at `http://localhost:3000` and backend at `http://localhost:8000` (ports are examples — verify in your config).

## Architecture & Design

- Backend: RESTful API for movies, showtimes, seats, orders, and tickets.
- Database: relational DB (Postgres recommended) to persist shows, seats and orders.
- Frontend: SPA for public booking flows and admin UI.
- Ticketing: generate a unique ticket payload (ID + signature) and render QR/barcode for entry scanning.
- Concurrency: seat reservations use short-lived holds plus atomic confirmation on purchase to avoid double-booking.

## API (examples)

Below are representative endpoints — adapt to the real routes the project implements.

- GET /movies — list movies
- GET /movies/:id/showtimes?date=YYYY-MM-DD — showtimes for a movie or date
- GET /showtimes/:id/seats — seat map and availability
- POST /reservations — place a temporary seat hold (returns hold id)
- POST /orders — complete purchase (accepts payment data + hold id)
- GET /tickets/:ticketId — retrieve ticket payload/QR

Example request (reserve seats):

```http
POST /reservations
Content-Type: application/json

{
	"showtimeId": "st_123",
	"seats": ["A1","A2"],
	"guest": { "email": "guest@example.com" }
}
```

## Development

- Use feature branches and open PRs against `main`.
- Add tests for booking and seat-allocation logic (critical for concurrency correctness).
- Provide an `.env.example` and seed fixtures for local testing (movies, screens, seats).

Local helpful commands (adjust to your tooling):

```bash
# run backend tests
npm test

# lint and format
npm run lint
npm run format
```

## Contributing

Contributions are welcome. Please:

1. Open an issue describing the feature or bug.
2. Create a branch named `feat/your-feature` or `fix/issue-123`.
3. Add tests and ensure existing tests pass.
4. Open a PR and request review.

## License

Specify your project's license here (e.g., MIT). If you don't have one yet, add a `LICENSE` file.

---

If you'd like, I can also:

- add an example `.env.example` and a minimal `docker-compose.yml` for local development,
- scaffold API docs (OpenAPI/Swagger), or
- create a small demo seed script that populates sample movies and showtimes.

If any of those help, tell me which and I'll add them next.
