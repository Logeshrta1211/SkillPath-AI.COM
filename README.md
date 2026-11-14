# SkillPath AI

**Discover your gaps. Build your skills. Achieve your career.**

SkillPath AI is an adaptive career learning platform for students. It compares recorded skills with requirements for a chosen career, calculates a transparent readiness score, and turns skill gaps into a practical roadmap. Six Gemini-powered agents add personalized analysis, recommendations, progress feedback, and mentorship.

**Live app:** [skillpath-ai-rta.ai.studio](https://skillpath-ai-rta.ai.studio)

## Features

- Learner profiles with education, projects, certifications, career goals, and learning preferences.
- Weighted skill-gap analysis with prerequisite-aware priorities and deterministic readiness scoring.
- Career matching across Full Stack Developer, Frontend Developer, Backend Developer, and Data Analyst tracks.
- Adaptive learning roadmaps with activities, practical exercises, and curated resources.
- Assessments that adapt recommendations into Recovery, Practice, or Progression modes based on results.
- Daily learning goals, study check-ins, a seven-day activity view, and goal streaks.
- Progress history, resource bookmarks, and a contextual AI mentor.
- Six Gemini agents: Profile Analysis, Skill Gap Analysis, Career Matching, Learning Recommendation, Progress Evaluation, and AI Mentor.
- Google sign-in with Firebase/Firestore, local email-and-password learner accounts stored in the browser, and an Alex Rivera demo sandbox.
- Optional animated Three.js background with a user-facing toggle.

## Tech stack

- React 19, TypeScript 7, Vite 8
- Express 4 and `tsx`
- Google GenAI SDK (`@google/genai`) with Gemini models
- Firebase Authentication and Cloud Firestore
- Tailwind CSS 4, Motion, Three.js, Recharts, and Lucide React

## Run locally

### Requirements

- Node.js compatible with the versions in `package.json`
- A Gemini API key for AI-powered features
- A Firebase project for Google sign-in and cloud persistence (optional for local learner and demo modes)

### Install and configure

```bash
npm install
```

Copy `.env.example` to `.env` and replace the placeholder with your Gemini API key:

```env
GEMINI_API_KEY=your_gemini_api_key
```

The frontend reads Firebase web app settings from `firebase-applet-config.json`, which is included with the project. To use your own Firebase project, replace that file with its web app configuration and the Firestore database ID. In Firebase, enable Google as an Authentication provider, create the database, add local/deployed hostnames to the authorized domains, and publish the supplied `firestore.rules`.

`GEMINI_API_KEY` is a server-side secret and must never be placed in frontend code or committed to GitHub. `APP_URL` is listed in `.env.example` for AI Studio's hosted runtime; the app's local server does not require it. Firebase web API keys identify the client project and do not replace authentication or Firestore rules.

### Start

```bash
npm run dev
```

The Express server listens on port `3000`. Open `http://localhost:3000`. Use the demo sandbox or local learner account for browser-local storage without Firebase sign-in.

## Scripts

| Command | Purpose |
| --- | --- |
| `npm run dev` | Start the Express and Vite development server |
| `npm run build` | Build the frontend into `dist/` |
| `npm run start` | Start the server using its production static-file setup |
| `npm run preview` | Preview the Vite build |
| `npm run lint` | TypeScript check (`tsc --noEmit`) |
| `npm run typecheck` | TypeScript check (`tsc --noEmit`) |
| `npm test` | Run the included unit and API tests |


For a production-style local run, build the app and set `NODE_ENV=production` before starting the server.

## AI and data behavior

- Gemini calls run through the Express API using `@google/genai`; the API key stays on the server.
- The agent implementation tries `gemini-4-hydrogen` first, then `gemini-3.1-flash-lite` as a fallback.
- Readiness scores and career matches come from the deterministic skill engine. Gemini supplies explanations and recommendations; it does not invent the score.
- Signed-in learner data is persisted in Firestore. Local learner accounts and demo state are stored in browser local storage.
- The API verifies Firebase ID tokens for signed-in requests. Local and demo requests use explicit workspace mode headers.

## Deploy with Google AI Studio

The project contains AI Studio metadata and is configured for an AI Studio-style hosted runtime.

1. Open or import the project in Google AI Studio Build mode.
2. Add `GEMINI_API_KEY` using the AI Studio **Secrets** settings. Do not commit the real key.
3. Configure Firebase Authentication, Firestore, authorized domains, and the rules for the Firebase project you intend to use.
4. Deploy through AI Studio and wait for the deployment to complete.
5. Open the deployed app and check sign-in, Firestore persistence, and Gemini-backed features.

The source server listens on port `3000` directly. If you deploy to a different hosting platform that requires its assigned `PORT` variable, update `server.ts` to use that platform's port.

## Repository layout

```text
server.ts                    Express API and static/development server
server/agents.ts             Gemini agent implementations
src/pages/                   Application pages
src/services/                Firestore persistence and agent orchestration
src/shared/catalog.ts        Skills, careers, assessments, and learning resources
src/shared/skillEngine.ts    Readiness, skill-gap, matching, and roadmap calculations
src/shared/dailyGoal.ts      Daily goal and momentum calculations
firestore.rules              Firestore access and validation rules
firebase-applet-config.json  Firebase web app configuration
```

## Security

- Keep `GEMINI_API_KEY` in server-side secrets. Never commit `.env` or `.env.local` files.
- Review and publish `firestore.rules` for the Firebase project you use.
- Firebase client configuration is visible to browsers; access control must come from Firebase Authentication and Firestore rules.

## License

No license file is included. Unless a license is added, this project is not explicitly licensed for reuse.


