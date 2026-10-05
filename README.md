# My Daily Coffee

Vite frontend for the My Daily Coffee experience, configured to talk to the remote AppCube APIs through Vite proxies.

## Run

From project root:

```bash
npm install
npm run dev
```

- Frontend: `http://localhost:5173`
- API base: `VITE_API_URL` from `.env` (default: `/service/ABH019_TestingV10/1.0.0`)

## Backend Organization

```txt
backend/
  .env.example
  data/
    db.json
  server.js                     # compatibility entrypoint
  src/
    server.js                   # server bootstrap + provider wiring
    config/
      env.js                    # environment parsing + validation
    lib/
      errors.js                 # HttpError helpers
      http.js                   # body parser, CORS, JSON responses
    appcube/
      client.js                 # OAuth token + AppCube HTTP client
      token-store.js            # in-memory token cache
    repositories/
      local-db.js               # JSON DB read/write + seed
    services/
      local-coffee-service.js   # local business logic
      appcube-coffee-service.js # AppCube provider implementation
      coffee-service.js         # hybrid fallback orchestration
    routes/
      health-routes.js
      coffee-routes.js
```

## Provider Modes

Set `APP_DATA_PROVIDER`:

- `local`: use JSON DB only
- `appcube`: use AppCube APIs only
- `hybrid`: AppCube first, fallback to local DB for non-4xx failures

## AppCube Configuration

Copy and fill values:

```bash
cp backend/.env.example .env
```

Required for `appcube` or `hybrid` mode:

- `APPCUBE_DOMAIN`
- `APPCUBE_CLIENT_ID`
- `APPCUBE_CLIENT_SECRET`

Endpoint mapping is configurable through `APPCUBE_*_PATH` values in `.env.example`.

## API Endpoints

- `GET /health`
- `GET /api/home/:userId`
- `GET /api/profile/:userId`
- `GET /api/missions/:userId`
- `GET /api/rewards`
- `POST /api/checkins`
- `POST /api/rewards/redeem`
- `POST /api/db/reset`
- `GET /api/admin/:collection`
- `GET /api/admin/:collection/:id`
- `POST /api/admin/:collection`
- `PATCH /api/admin/:collection/:id`
- `DELETE /api/admin/:collection/:id`

Seed user id (local mode): `user_1`

Admin collections: `users`, `cafes`, `checkins`, `missions`, `userMissions`, `rewards`, `blindBoxes`, `userRewards`.

## Scripts

- `npm run dev` - run Vite frontend
- `npm run build` - frontend production build
- `npm run preview` - preview production build
- `npm run lint` - ESLint checks
