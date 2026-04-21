<img width="1920" height="995" alt="{28D37296-C1E6-46C6-937B-107C26963067}" src="https://github.com/user-attachments/assets/6261f37f-2c77-4efe-aa2a-d55d8e8b4b9b" /># VibeHack

VibeHack is a privacy-first anonymous social UI prototype built for hackathon judging.

## Core Experience

- Login / Sign up gate with guest mode for quick demos
- Anonymous identity with randomized avatar + handle
- Temporary room model with context-based discovery
- Sidebar-driven navigation (Home, Discover, Active Rooms, Profile, Settings)
- Day/Night themes, trust-style profile metrics, and lightweight analytics
- Local persistence for rooms, preferences, selected room, and settings

## Activity Point Criteria (Current)

Authenticity points are backend-owned (`User.auth_points`) and currently increase by:

1. Heartbeat activity: `+1` point when the client sends a `heartbeat` socket event while in a joined room.
2. Gifting points: `+1` point when another authenticated user calls `POST /api/users/gift/{ghost_name}`.

Current notes:

- No hard daily cap is enforced yet.
- Points are permanently stored in the database.
- Ghost identity pointer used for room privacy expires after 24 hours.

## Ghost Identity System (Complete Privacy)

When you join a room, your real identity is completely hidden. You receive a temporary ghost identity:

- **On Join**: Server generates a random ghost name like `SilentPanda_234`, `NeonSpectre_567`, etc.
- **While In Room**: Only your ghost name is visible to other users. Your real username is never exposed.
- **On Leave**: Ghost identity is immediately deleted from Redis. Your temporary persona vanishes.
- **Fallback (Offline)**: If backend unavailable, frontend generates equally random ghost names locally.

This ensures complete anonymity within rooms while maintaining user identity tracking for points and trust metrics outside chat.

## Feature Showcase

### Home Dashboard
![Home view with activity insights](<img width="1920" height="995" alt="{28D37296-C1E6-46C6-937B-107C26963067}" src="https://github.com/user-attachments/assets/34ee438a-737a-431d-85b0-cdba0d84accb" />
)

### Room Discovery
![Discover tab showing available rooms and topics](<img width="1920" height="987" alt="{E4D086BC-D80F-42AC-AE6E-55A7703A78D8}" src="https://github.com/user-attachments/assets/dad72884-1f82-4cf2-b935-5c15801d2f60" />
)

### Active Rooms
![List of active rooms to join](<img width="1920" height="986" alt="{54F1E01C-14EE-4AE1-B57E-A609BB5D3BE9}" src="https://github.com/user-attachments/assets/757d058d-b226-44c9-847e-bf55cf971897" />
)

### Temporary Ghost Room
When you join a room, your profile displays your temporary ghost identity. Other users only see this ghost name in the chat and room members list.
![Room view with temporary ghost identity](<img width="1920" height="991" alt="{7A6A3B0B-DE7C-42C2-94A7-4BAAC87D9FB2}" src="https://github.com/user-attachments/assets/78f79445-5ff2-4c5b-a354-60005b6a9b17" />
)


### User Profile
![Profile dashboard with trust metrics and activity history](<img width="1920" height="989" alt="{7194683D-5903-4ED8-BF00-5EDCC8825EC3}" src="https://github.com/user-attachments/assets/21b58cb1-c0b3-4c3a-89b6-f28d360ea4a3" />
)

### Settings & Preferences
![Settings panel with theme toggles and personal controls](<img width="1915" height="995" alt="{7D33A29E-AA9E-4DAC-B43B-47497CC95986}" src="https://github.com/user-attachments/assets/b5f74b5b-9eba-4c28-87df-797a9de0cdbd" />
)

## Project Layout

- `frontend/` - complete user-facing application (HTML/CSS/JS)
- `backend/` - reserved for backend teammate integration
- `docs/` - AI prompt log and internal handoff notes

## Run Locally

Option 1:
- Open `frontend/index.html` directly in your browser.

Option 2 (static server):
- From repo root, run a local static server that points to `frontend/`.

## Frontend Testing

From `frontend/`:

1. `npm install`
2. `npm test`

Current suite covers auth utility validation and session flows.

## Judge Test (60 Seconds)

1. Login, sign up, or continue as guest.
2. Open app and switch between Home, Discover, Active Rooms, Profile, Settings.
3. Create a new room from Discover or Active Rooms.
4. Open room Details drawer and set a room active.
5. Toggle 1 to 2 settings and switch theme.
6. Refresh page and confirm state persisted.
7. Optional: use Settings -> Reset to restore clean demo defaults.

## Deployment

No repo workflow is required. Fastest zero-config option:

1. Import this repo into Vercel.
2. Keep project root at repo root (do not use `frontend` root override).
3. Deploy (repo includes `vercel.json` to force static frontend output and avoid Next.js auto-detection).

### Frontend + Backend Sync on Vercel

This repo now includes `vercel.json` rewrites so the frontend and backend stay connected automatically:

- `/api/*` -> proxied to backend deployment
- `/socket.io/*` -> proxied to backend deployment
- all other routes -> SPA fallback to `frontend/index.html`

Current backend target is configured in `vercel.json`. If your backend domain changes, update only these two rewrite destinations once and redeploy.

You can also use Netlify with the same setup:

1. New site from Git.
2. Base directory: `frontend`.
3. Publish directory: `.`

## Repo Hygiene

The following local reference files are intentionally ignored:

- `Command & Frontend.md`
- `Rules & Regulation.md`
- `docs/judge-handoff.md`
