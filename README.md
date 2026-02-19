# movie-recommendation-web

React frontend for the Movie Recommendation app, including authentication flow, user area, preference questions, and recommendation results UI.

## Project purpose
This frontend provides the user-facing experience for:
- registration/login
- password reset pages
- preference/question flow for recommendation generation
- displaying selected films and recommendation responses
- basic user profile area

It consumes the backend API from the companion project (`movie-recommendation-api`).

## Tech stack
- React (Create React App) + TypeScript
- React Router
- Styled Components
- Firebase client SDK (auth-related flows)
- JWT decode utilities

## Main frontend areas
- `src/pages`
  - `HomePage`, `LoginPage`, `RegisterPage`, `QuestionsPage`
- `src/components`
  - recommendation, user, auth and utility components
- `src/context`
  - auth state management (`AuthContext`)
- `src/api`
  - API calls to backend

## Environment variables
Create `.env` from `.env.example` and set:
- Firebase config (`REACT_APP_FIREBASE_*`)
- `REACT_APP_API_URL` (e.g. `http://localhost:3030`)
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

## Full stack Docker run
The full stack docker setup is managed by the backend project.  
From `movie-recommendation-api` (backend root), run:

```bash
npm run docker
```

This starts:
- frontend (`movie-recommendation-fe`) on `3000`
- backend on `3030`
- mongo on `27017`
