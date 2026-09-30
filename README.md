# 🌿 EcoAI — AI for Sustainability

[![Hackathon Ready](https://img.shields.io/badge/Hackathon-Production%20Grade-10b981?style=for-the-badge&logo=target)](https://github.com/)
[![React](https://img.shields.io/badge/React%2018-Vite%20TypeScript-0ea5e9?style=for-the-badge&logo=react)](https://react.dev/)
[![Node.js](https://img.shields.io/badge/Node.js-Express%20TS-059669?style=for-the-badge&logo=node.js)](https://nodejs.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Schema%20%2B%20Seeds-336791?style=for-the-badge&logo=postgresql)](https://www.postgresql.org/)
[![Google Gemini](https://img.shields.io/badge/AI%20Engine-@google/genai-f59e0b?style=for-the-badge&logo=google)](https://aistudio.google.com/)

**EcoAI** is an intelligent sustainability and planetary health platform that uses advanced AI, deterministic greenhouse gas modeling, and computer vision to help individuals, schools, businesses, and communities understand their environmental footprint and take verified, high-leverage actions to eliminate it.

---

## 🏆 Quick Demo Fast-Track (Hackathon Judges)

To evaluate the application in under 3 minutes without setting up external API keys or databases:

1. Start the platform:
   ```bash
   npm run dev
   ```
2. Open **[http://localhost:3000](http://localhost:3000)** in your browser.
3. Click the prominent **"Explore EcoAI (Instant Demo as Prasad)"** or **"Try Demo"** button.
4. You are immediately placed into a fully populated, real-world dashboard with telemetry, trend charts, active challenges, and live AI advisory tools!

---

## 🌍 The Problem

People want to live sustainably, but environmental data is invisible, fragmented, and confusing:
- **Opaque Footprints**: Individuals have no tangible idea how many kilograms of CO₂ their daily electricity, commute, or food habits emit.
- **Decision Paralysis**: Users are bombarded with generic advice ("plant a tree") without knowing which specific change creates the highest leverage for their personal lifestyle.
- **Waste Contamination**: Over 60% of recyclable and e-waste materials end up in open municipal landfills due to unclear local sorting rules.
- **Lack of Motivation**: Climate action often feels isolated without gamified milestones or community momentum.

---

## 💡 The Solution: EcoAI

EcoAI transforms abstract environmental data into personalized, measurable, and rewarding actions through:
1. **Multi-Dimensional Sustainability Scoring (0–100)**: Evaluates Carbon, Energy, Water, Waste, and Transportation dimensions with circular SVG progress gauges.
2. **Transparent Carbon Calculator**: Computes CO₂ emissions using verified CEA India Baseline v19 and IPCC AR6 emission factors, highlighting the user's primary emission driver.
3. **EcoAI Advisor**: A context-aware environmental scientist and agronomist providing structured 6-point clinical recommendations based on user telemetry.
4. **AI Impact Copilot**: Answers *"What should I do this week?"* with the top 3 highest-leverage actions optimized by impact, cost, and difficulty.
5. **AI Computer Vision Waste Scanner**: Classifies waste images or descriptions into **Recyclable, Organic, E-waste, Hazardous, or General** with exact circular disposal procedures.
6. **"WHAT IF?" Real-Time Simulator**: Parametrically forecasts carbon reductions, annualized rupee savings, and equivalent trees saved before committing to new habits.
7. **Gamified Goals & Climate Challenges**: 7-day, 5-day, and 30-day sprints with verified Eco-Points and community leaderboards.

---

## 🛠️ Technology Stack

| Layer | Technologies |
|---|---|
| **Frontend** | React 18, Vite, TypeScript, Tailwind CSS, Recharts, Lucide Icons, Canvas Confetti |
| **Backend** | Node.js, Express.js, TypeScript (`tsx`), Helmet, CORS, Morgan, Express Rate Limit |
| **Validation** | Zod Schema Validation |
| **AI Integration** | Official `@google/genai` SDK (`gemini-2.5-flash`), Agronomic Heuristic Fallback Engine |
| **Database & Security** | PostgreSQL 14+ DDL Schema, JWT Authentication, bcryptjs password hashing |
| **Storage & Upload** | Multer memory storage (5MB strict JPEG/PNG/WebP gate) |

---

## 📐 System Architecture

```mermaid
graph TD
    User([User Browser]) <--> ReactClient[React 18 + Vite Client]
    ReactClient <--> ExpressServer[Express.js REST API]
    
    subgraph Backend Services
        ExpressServer --> AuthGuard[JWT / Demo Auth Middleware]
        AuthGuard --> ZodValidator[Zod Request Body Validation]
        ZodValidator --> CarbonEngine[Carbon Calculator Engine]
        ZodValidator --> ScoreEngine[Sustainability Scoring Engine]
        ZodValidator --> AIService[AI Service Layer]
        
        AIService --> GeminiSDK[@google/genai SDK]
        AIService --> FallbackEngine[Agronomic Heuristic Fallback]
        
        CarbonEngine --> StorageStore[(Persistent Store / PostgreSQL)]
        ScoreEngine --> StorageStore
    end
```

---

## 🗄️ Database Schema & Data Models

The full production PostgreSQL DDL is located in [`database/schema.sql`](file:///d:/eco-ai/database/schema.sql) and [`database/seed.sql`](file:///d:/eco-ai/database/seed.sql).

### Core Tables:
1. `users`: UUID, email, password_hash, full_name, role (`user` \| `admin`), eco_points, eco_level.
2. `profiles`: user_id, bio, city, country, household_size, primary_vehicle, dietary_preference, target_carbon_reduction_pct.
3. `carbon_records`: user_id, calculation_date, electricity_kwh, lpg_kg, vehicle_type, distance_km, public_transport_km, flights_count, food_habit, waste_kg, total_co2_kg, major_source, reduction_target_kg, calculation_details (JSONB).
4. `energy_records`: user_id, record_date, electricity_kwh, renewable_percentage, cost_estimated.
5. `water_records`: user_id, record_date, consumption_liters, rainwater_harvested_liters.
6. `waste_records`: user_id, record_date, total_kg, category (`Recyclable`, `Organic`, `E-waste`, `Hazardous`, `General`).
7. `sustainability_goals`: user_id, title, category, target_value, current_value, unit, deadline, status, points_reward.
8. `challenges`: id, title, description, category, duration_days, points_reward, difficulty, badge_name, icon.
9. `challenge_progress`: challenge_id, user_id, status, progress_percentage, days_completed.
10. `recommendations`: user_id, title, description, category, impact_level, difficulty, estimated_cost, co2_reduction_kg, money_savings_est, status.
11. `community_statistics`: stat_date, total_co2_reduced_kg, total_plastic_avoided_kg, total_energy_saved_kwh, total_water_saved_l, total_trees_equivalent.

---

## 📡 API Documentation

### Authentication & User
- `POST /api/auth/register` — Create new account (returns JWT token).
- `POST /api/auth/login` — Sign in with email and password.
- `POST /api/auth/demo` — 1-click Demo login as Prasad Gurav.
- `POST /api/auth/demo-admin` — 1-click Admin login.
- `GET /api/user/profile` — Fetch current user profile, score, and badges.
- `PUT /api/user/profile` — Update user profile.

### Dashboard & Analytics
- `GET /api/dashboard` — Unified dashboard payload (scores, 4 core telemetry metrics, trend charts, AI insight of the day, active goals, recommendations).

### Carbon & Telemetry
- `POST /api/carbon/calculate` — Audit-grade carbon footprint calculation with itemized breakdown and comparison.
- `GET /api/carbon/history` — Historical carbon records.
- `POST /api/energy` & `GET /api/energy/history` — Energy consumption logging.
- `POST /api/water` & `GET /api/water/history` — Water usage logging.
- `POST /api/waste` & `GET /api/waste/history` — Waste generation logging.

### AI Engine Endpoints
- `POST /api/ai/advice` — Context-aware consultation with EcoAI Advisor (returns 6-part structured prescription).
- `GET /api/ai/recommendations` — Fetch personalized AI recommendations.
- `POST /api/ai/recommendations/:id/complete` — Mark recommendation completed and earn Eco-Points.
- `GET /api/ai/copilot` — "AI Impact Copilot": Returns top 3 prioritized actions for the week.
- `POST /api/ai/simulate` — Parametric "What If?" impact forecaster.
- `POST /api/waste/analyze` — Computer vision waste scanner (accepts multipart image or item description).

### Goals, Challenges & Community
- `GET /api/goals` & `POST /api/goals` — List and create sustainability goals.
- `PUT /api/goals/:id` — Update goal progress or mark complete.
- `GET /api/challenges` — List challenges with user progress.
- `POST /api/challenges/:id/join` — Join a sustainability challenge.
- `PUT /api/challenges/:id/progress` — Check in daily progress.
- `GET /api/community` — Aggregated anonymized stats and leaderboard.
- `GET /api/admin` — Administrator platform analytics and common bottleneck reports.

---

## 💻 Installation & Running Locally

### Prerequisites
- Node.js >= 18.0.0 (tested on v20.18.0)
- npm >= 9.0.0

### 1. Clone & Install Dependencies
```bash
# In project root
npm run install:all
```

### 2. Configure Environment Variables
Create `.env` in `server/`:
```bash
cp server/.env.example server/.env
```
*(Optionally add your Google Gemini API key if available; if left blank, the built-in clinical heuristic engine operates with zero errors!)*

### 3. Start Both Server & Client Concurrently
```bash
npm run dev
```

- **Frontend Client**: [http://localhost:3000](http://localhost:3000)
- **Backend API Server**: [http://localhost:5000](http://localhost:5000)
- **API Health Check**: [http://localhost:5000/api/health](http://localhost:5000/api/health)

---

## 🔑 Demo Credentials

| Role | Email | Password | Quick Access |
|---|---|---|---|
| **User (Prasad Gurav)** | `prasad@ecoai.dev` | `password123` | Click **"Try Demo (Prasad)"** on Login page |
| **Administrator** | `admin@ecoai.dev` | `password123` | Click **"Admin Demo"** on Login page |

---

## 🚀 3-Minute Hackathon Presentation Script

1. **0:00 - 0:30**: Open **Landing Page** → Highlight the mission statement: *"Turn Your Data Into a Greener Future."* Show the 540k+ kg collective counter and the 5-step narrative.
2. **0:30 - 1:00**: Click **"Explore EcoAI"** → Instantly loads **Prasad's Dashboard**. Show the **78/100 Sustainability Score Gauge**, the 4 metric trend charts, and the **AI Insight of the Day**.
3. **1:00 - 1:45**: Launch **"AI Impact Copilot"** (*"What should I do this week?"*) → Show the 3 high-impact prioritized actions, mark one as completed with instant points + confetti!
4. **1:45 - 2:15**: Navigate to **"WHAT IF?" Simulator** → Move the public transport and AC reduction sliders, demonstrating real-time CO₂ drop, tree equivalents, and rupee savings.
5. **2:15 - 2:40**: Open **Waste Scanner** → Click a test sample (e.g. Lithium Battery or PET Bottle) to show automated polymer identification and exact disposal protocol.
6. **2:40 - 3:00**: Show **Community Impact & Leaderboard** → Highlight data anonymization and collective power!

---

## 🔮 Future Improvements

- **IoT Smart Meter Integration**: Direct Webhook sync with Shelly and Tuya home electricity meters.
- **Barcode & EAN Database Scanning**: Scan grocery barcodes for immediate cradle-to-gate LCA carbon ratings.
- **Municipal Open Data Integration**: Dynamic routing to the closest operational battery drop-off kiosk via OpenStreetMap.
- **Corporate ESG Reporting Export**: Automated BRSR / CSRD compliant greenhouse gas emission filings for SMBs.

---

**Built with 💚 for Hackathon 2026 by the EcoAI Engineering Team.**
