# movie-recommendation-web

React + TypeScript frontend for an AI movie recommendation experience.  
The UI collects user tastes and mood, sends them to the backend AI engine, and renders personalized results with posters and details.

## What this frontend does
This is not just a static client: it orchestrates the full recommendation journey.

Main user flow:
1. User opens the app (home/login/register available).
2. User can authenticate (email/password or Google sign-in).
3. In the questions flow, the app asks preference cards (topic, years, mood, type, genres, etc.).
4. It first fetches random seed titles from backend (`/api/recommendations/random`) to let the user choose a preferred movie.
5. It sends the final answer set to backend (`/api/recommendations`) where AI generates recommendations.
6. It renders mood + recommended titles + posters.
7. Logged users can open user area and review recent recommendation history saved server-side.

## Architecture overview
- `src/pages`
  - `HomePage`, `LoginPage`, `RegisterPage`, `QuestionsPage`
- `src/components`
  - question cards, recommendation response, selected film detail, user page, loaders
- `src/context/AuthContext.tsx`
  - JWT session state, login/logout, recommendation-history fetch
- `src/api/genericApi.tsx`
  - Axios instance + auth token interceptor
- `src/config/Firebase.ts`
  - Firebase client bootstrap + Google provider

## Tech stack
- React 18 + TypeScript
- React Router
- Styled Components + custom CSS
- Axios
- Firebase Auth (Google popup login)
- JWT decode

## Authentication behavior
- Local login/register via backend `/auth` endpoints.
- Google login:
  - frontend gets Firebase ID token
  - sends token to backend `/auth/google-login`
  - receives app JWT and stores it in `localStorage`
- `AuthContext` decodes JWT and exposes session/user info globally.

## Recommendation UX details
- Question flow is dynamic and image-driven.
- For most questions, image URLs are composed from public bucket env var.
- For “favorite movie” question, options are based on backend random recommendation titles/posters.
- Final response supports:
  - mood display
  - film card list
  - click-through detail view
  - restart (“Cambia mood”) to run a new recommendation cycle

## User area
- Shows current user metadata from decoded token.
- Fetches recommendation history from backend (`GET /api/recommendations/:email`).
- Displays most recent results list.

## Environment variables
Create `.env` from `.env.example` and configure:

- Firebase
  - `REACT_APP_FIREBASE_API_KEY`
  - `REACT_APP_FIREBASE_AUTH_DOMAIN`
  - `REACT_APP_FIREBASE_PROJECT_ID`
  - `REACT_APP_FIREBASE_STORAGE_BUCKET`
  - `REACT_APP_FIREBASE_MESSAGE_SENDER_ID`
  - `REACT_APP_FIREBASE_APP_ID`
- API
  - `REACT_APP_API_URL` (e.g. `http://localhost:3030`)
- Assets/fallback
  - `REACT_APP_URL_NOT_FOUND`
  - `REACT_APP_PUBLIC_BUCKET_URL`

## Run locally
```bash
npm install
npm start
```

App URL: `http://localhost:3000`

## Build
```bash
npm run build
```

## Full stack run (recommended)
Frontend is launched by backend compose.

From `movie-recommendation-api` (backend root):
```bash
npm run docker
```

Services:
- frontend: `http://localhost:3000`
- backend: `http://localhost:3030`
- swagger: `http://localhost:3030/api/docs`
- mongo: `localhost:27017`

## Practical note
Some auth calls in components are hardcoded to `http://localhost:3030` while others use `REACT_APP_API_URL` via Axios wrapper.  
For multi-environment deployments, standardize all calls through `src/api/genericApi.tsx`.
