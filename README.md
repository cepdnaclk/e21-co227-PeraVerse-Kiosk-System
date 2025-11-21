---
layout: home
permalink: index.html

# Please update this with your repository name and title
repository-name: e21-co227-PeraVerse-Kiosk-System
title: Interactive Exhibition Assistance Platform – Full-Stack Development Project
---

[comment]: # "This is the standard layout for the project, but you can clean this and use your own template"

# Kiosk System of PeraVerse

---

<!-- 
This is a sample image, to show how to add images to your page. To learn more options, please refer [this](https://projects.ce.pdn.ac.lk/docs/faq/how-to-add-an-image/)

![Sample Image](./images/sample.png)
 -->

## Team
-  E/21/054, K.M.D.D. Bandara, [e21054@eng.pdn.ac.lk](mailto:e21054@eng.pdn.ac.lk)
-  E/21/067, H.A.P. Chandrasekara, [e21067@eng.pdn.ac.lk](mailto:e21067@eng.pdn.ac.lk)
-  E/21/096, L.R.C.N. Dileka, [e21096@eng.pdn.ac.lk](mailto:e21096@eng.pdn.ac.lk)
-  E/21/106, D.M.M.B. Dissanayaka, [e21106@eng.pdn.ac.lk](mailto:e21106@eng.pdn.ac.lk)
-  E/21/362, K.G.S. Sathsarani, [e21362@eng.pdn.ac.lk](mailto:e21362@eng.pdn.ac.lk)
-  E/21/363, S.H.T. Savindi, [e21363@eng.pdn.ac.lk](mailto:e21363@eng.pdn.ac.lk)
-  E/21/372, L.A. Senevirathne, [e21372@eng.pdn.ac.lk](mailto:e21372@eng.pdn.ac.lk)
-  E/21/416, J.A.S. Uthpala, [e21416@eng.pdn.ac.lk](mailto:e21416@eng.pdn.ac.lk)
-  E/21/453, H.D. Suriyapperuma, [e21453@eng.pdn.ac.lk](mailto:e21453@eng.pdn.ac.lk)

## Table of Contents
1. [Introduction](#introduction)
2. [Solution & Impact](#solution--impact)
3. [Features & Architecture](#features--architecture)
4. [How to Run (Local)](#how-to-run-local)
5. [Deployment & Recommendations](#deployment--recommendations)
6. [Links](#links)

---

## Introduction

This repository (KIOSK) is a digital kiosk system for displaying campus information,exhibits, events, maps, heatmaps, notifications and more. It is a lightweight full-stack project composed of a Vite + React frontend and several Node.js Express backends (per feature), backed by PostgreSQL databases in some services.

The following report summarizes the system, key features, architecture, how to run it locally, and deployment recommendations so you can upload it to the project Git repository.

## Solution & Impact

KIOSK consolidates multiple campus services into a single kiosk UI. The system provides:

- Centralized event schedule and exhibits categories
- Map and routing services (building search & routing)
- Heatmap generation/analysis for crowd monitoring
- Notifications and feedback capture
- A chatbot/knowledge base integration

Impact:
- Consolidates core campus information into a single kiosk UI for visitors and staff.
- Encourages engagement with events and exhibits via a consistent schedule and notification system.
- Provides analytics and heatmap data that can be used for crowd management and planning.
- provides platform to give the feedback of the event and system.

## Features & Architecture

### Key Features
- Events page: list events, categories, create events (backend_events)
- Map features: building map, routing, search (backend_map + frontend map module)
- Heatmap: store and visualize heatmap data and building samples (backend_heatmap)
- Notifications: periodic notifications and live updates (backend_notifications)
- Exhibits and About pages: static and dynamic content
- Chatbot: knowledge base + simple conversation UI (frontend/chatbot)

### Architecture Overview
- Frontend: React + Vite (source in `frontend/`) — components under `frontend/src/components/`.
- Backends: Several small Node.js + Express services, each in its own folder under the repo root (examples: `backend`,`backend_events`, `backend_map`, `backend_heatmap`, `backend_notifications`, `backend_aboutpage`).
- Database: PostgreSQL used by multiple services — DB schema files are present under `backend/*/` (for example `backend_events` references `event_db` and SQL files exist in `backend/` subfolders). Some services include `.sql` or sample DB files.
- Local configuration: Each backend uses its own `.env` file for DB credentials and PORT. The frontend reads `VITE_API_URL` from the project root `.env`.
- Communication: Frontend calls backend REST endpoints (e.g., `/api/events`) — ensure `VITE_API_URL` points to the correct backend host/port.

## How to Run (Local)

These instructions assume a Windows PowerShell environment (this repo also contains convenience scripts). Adjust for your environment as needed.

1. Prerequisites
   - Node.js (v16+ recommended)
   - npm
   - PostgreSQL (for services that require a DB)

2. Clone repository
```powershell
git clone https://github.com/cepdnaclk/e21-co227-PeraVerse-Kiosk-System.git
cd KIOSK
```

3. Configure environment variables
- Project root `.env` controls the frontend base API URL. Example (edit as needed):
```properties
VITE_API_URL=http://localhost:3036
```
- For each backend (e.g., `backend_events/.env`) set DB credentials and PORT:
```properties
PORT=3036
DB_USER=postgres
DB_HOST=localhost
DB_NAME=event_db
DB_PASSWORD=your_password
DB_PORT=5432
```

4. Install dependencies and start backends
Open separate PowerShell tabs for each backend or use the provided `start_backends.bat` if you prefer.

Example (events backend):
```powershell
cd .\backend_events
npm install
npm start    # or: node server.js
```

Repeat for other backends you need (e.g., `backend_map`, `backend_heatmap`, `backend_notifications`, `backend_aboutpage`).

5. Install and run frontend
```powershell
cd ..\frontend
npm install
npm run dev   # starts Vite dev server (default port 5173)
```

6. Verify
- Ensure each backend logs that it started successfully on its configured `PORT` (e.g., `Server running on port 3036`).
- Visit the frontend (http://localhost:5173 by default). The frontend will fetch from the `VITE_API_URL` — ensure it points to the backend serving `/api/events` and other APIs.

7. Quick API test (PowerShell)
```powershell
# Example: test events endpoint
(Invoke-WebRequest -Uri 'http://localhost:3036/api/events' -UseBasicParsing).Content
```

## Deployment & Recommendations

- Frontend: Deploy the `frontend` build to Vercel, Netlify, or any static hosting that supports SPA routing.
- Backends: Deploy Node services as serverless functions (Vercel/Netlify/Cloudflare Workers) or as small containers on Railway, Render or Heroku. Keep each backend service isolated for easier updates.
- Database: Use a managed PostgreSQL provider (Supabase, ElephantSQL, or any cloud database) to avoid local DB management when deploying.
- Environment variables: Store secrets and DB credentials securely in the hosting provider's environment/secret manager.
- CORS & Security: Verify CORS settings for each backend to allow the deployed frontend origin. Use HTTPS in production.

## Links

- [Project Repository](https://github.com/cepdnaclk/e21-co227-PeraVerse-Kiosk-System)
- [Project Page](https://cepdnaclk.github.io/e21-co227-PeraVerse-Kiosk-System)
- [Department of Computer Engineering](http://www.ce.pdn.ac.lk/)
- [University of Peradeniya](https://eng.pdn.ac.lk/)

### Tags
`React`, `Vite`, `Express.js`, `Node.js`, `PostgreSQL`, `Full-Stack`, `Kiosk`, `Local Deployment`

---
*Developed for CO227 Computer Engineering Project, University of Peradeniya*
