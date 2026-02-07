# 🧭 Studio Bot — AI Voice Agents for the Tattoo Industry

A modern web platform that lets tattoo shops and similar businesses create, customize, and manage **AI voice agents** that handle customer calls, appointments, and inquiries 24/7—so you can focus on your art.

---

## 📚 Table of Contents

[About](#-about)  
[Features](#-features)  
[Tech Stack](#-tech-stack)  
[Installation](#-installation)  
[Usage](#-usage)  
[Configuration](#-configuration)  
[Demo](#-demo)  
[API Documentation](#-api-documentation)  
[Contact](#-contact)  
[Acknowledgements](#-acknowledgements)

---

## 🧩 About

Studio Bot was built to solve a common pain point for tattoo studios: **constant phone calls and booking chaos** that pull artists away from their work. This project provides an intuitive interface to deploy AI voice assistants trained for the tattoo industry—handling calls, scheduling, FAQs, and reminders—powered by a React + TypeScript frontend, Supabase backend, and optional Netlify serverless APIs.

**Goals:**

- Let studios deploy professional AI voice agents without technical expertise  
- Centralize voice agent creation, settings, and analytics in one dashboard  
- Support authentication, billing (Stripe), and secure configuration  
- Deliver a polished, accessible UI with glass-morphism styling and smooth animations  

---

## ✨ Features

- **Authentication & user management** — Email/password and Google OAuth via Supabase Auth; persistent sessions and profile management  
- **Voice agent creation & customization** — Multi-step wizard to create agents; voice type, stability, style, and latency controls  
- **Dashboard & analytics** — Real-time metrics, call stats, success rates, and interactive D3.js charts with agent progression  
- **Settings & configuration** — Business info, operating hours, call handling, voicemail, greetings, and notification preferences  
- **Call history & monitoring** — View and manage call history and agent performance from the dashboard  
- **Pricing & checkout** — Stripe-integrated pricing plans and checkout flow  
- **Help center & ROI calculator** — In-app help and a calculator to estimate ROI from using AI voice agents  
- **Responsive, accessible UI** — Dark theme with purple accents, glass-morphism, Framer Motion animations, ARIA labels, and keyboard navigation  

---

## 🧠 Tech Stack

| Category    | Technologies |
|------------|--------------|
| **Languages** | TypeScript, JavaScript |
| **Frontend**  | React 18, Vite, Tailwind CSS, Framer Motion, Lucide React, D3.js, Chart.js, React Router v6 |
| **Backend / Data** | Supabase (PostgreSQL, Auth, real-time) |
| **Payments** | Stripe (React Stripe, Stripe.js) |
| **API / Hosting** | Netlify (serverless functions, redirects), Axios |
| **Tools**   | ESLint, PostCSS, libphonenumber-js, pdfjs-dist, tesseract.js |

---

## ⚙️ Installation

```bash
# Clone the repository
git clone https://github.com/monsterdev914/studio_bot.git

# Navigate to the project directory
cd studio_bot

# Install dependencies
npm install
```

---

## 🚀 Usage

```bash
# Start the development server
npm run dev
```

Then open your browser and go to:

👉 [http://localhost:5173](http://localhost:5173)

**Other scripts:**

- `npm run build` — TypeScript check + production build  
- `npm run preview` — Preview production build locally  
- `npm run lint` — Run ESLint  

---

## 🧾 Configuration

Create a `.env` file in the project root with your Supabase and (optional) API keys:

```env
# Required for Supabase auth and data
VITE_SUPABASE_URL=your_supabase_project_url
VITE_SUPABASE_ANON_KEY=your_supabase_anon_key

# Backend API (used by axios in src/api/axiosConfig.ts). Default: http://localhost:3457/api
VITE_BACKEND_URL=http://localhost:3457/api

# Optional: for payments, Twilio, Synthflow, etc.
# API_KEY=your_api_key_here
# DB_URL=your_database_url_here
```

- Get `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` from your [Supabase](https://supabase.com) project **Settings → API**.  
- Ensure Supabase Auth (email/password and Google OAuth if used) and RLS policies are set up as in your project docs.

---

## 🖼 Demo

Watch the platform in action:

<div align="center">
  <video src="./public/demo.mp4" controls width="100%" style="max-width: 720px; border-radius: 8px;">
    Your browser does not support the video tag.
  </video>
</div>

**Can't see the video?** [Open or download it here](./public/demo.mp4). If it still doesn’t play inline, upload `public/demo.mp4` by dragging it into a [new Issue](https://github.com/monsterdev914/studio_bot/issues/new) comment—GitHub will host it and give you a URL. Replace the `src` above with that URL for reliable playback.

---

## 📜 API Documentation

The app talks to two backends: **Netlify serverless functions** (auth) and a **backend API** (configured via `VITE_BACKEND_URL`, default `http://localhost:3457/api`). All backend API requests use the axios instance in `src/api/axiosConfig.ts` and send the Supabase access token in the `Authorization` header.

---

### Netlify functions (auth & webhooks)

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/.netlify/functions/supabase-auth` | Auth: body `{ action: "signup" \| "login" \| "logout", email?, password? }` |
| `GET`  | `/.netlify/functions/hello-world` | Test endpoint |
| —      | `/.netlify/functions/webhook/:model_id` | Webhook URL used for Synthflow (e.g. in voice agent config) |

---

### Backend API (relative to base URL `/api`)

**Actions (voice agent actions)**  
| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/action` | Create action; body `{ action }` |
| `GET`  | `/action/:id` | Get action by id |
| `GET`  | `/action/list` | List actions |
| `PUT`  | `/action` | Update action; body `{ id, action }` |
| `DELETE` | `/action/:id` | Delete action |
| `POST` | `/action/action_id` | Resolve by `action_id`; body `{ action_id }` |

**Synthflow (AI voice assistants)**  
| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/synthflow/createAssistant` | Create assistant; body `{ assistant }` |
| `PUT`  | `/synthflow/updateAssistant` | Update assistant; body `cond` |
| `GET`  | `/synthflow/getAssistant` | Get current assistant |
| `GET`  | `/synthflow/action/:action_id` | Get Synthflow action by id |
| `POST` | `/synthflow/createSynthflowAction` | Create Synthflow action; body `{ accessToken }` |
| `POST` | `/get-listCalls` | List calls; body `{ model_id, limit?, offset? }` |

**Phone numbers**  
| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/phone_numbers` | Create/link phone number; body per `src/lib/supabase.ts` |

**User profile**  
| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET`  | `/user-profile` | Get current user profile |
| `POST` | `/user-profile` | Create profile; body per implementation |
| `PUT`  | `/user-profile` | Update profile; body per implementation |

**Optional preferences**  
| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET`  | `/get-optionalPreferences` | Get preferences |
| `POST` | `/optionalPreferences` | Create/set preferences; body `{ preference }` |
| `PUT`  | `/update-optionalPreferences` | Update preferences; body `{ cond }` |

**Payments (Stripe)**  
| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/createPaymentIntent` | Create Stripe PaymentIntent; body `{ plan_id, currency }` |
| `POST` | `/planUpdate` | Record successful subscription; body `{ paymentIntentId }` |

**Integrations**  
| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET`  | `/twilio/buy` | Twilio: buy/provision number (response used by client) |
| `POST` | `/firecrawl/extractTattooShopInfo` | Extract shop info from URL; body `{ url }` |

---

- **Supabase** is used directly from the client for auth state, real-time data, and storage (see `src/lib/supabase.ts`).  
- **External:** ElevenLabs `GET https://api.elevenlabs.io/v1/voices` is called from the client for voice lists (API key client-side or via your backend).

---

## 📬 Contact

- **Author:** Your Name  
- **Email:** skytech2510@gmail.com  
- **GitHub:** [@monsterdev914](https://github.com/monsterdev914)  
- **Website/Portfolio:** [monsterdev914.github.io](https://monsterdev914.github.io)  

---

## 🌟 Acknowledgements

- [Supabase](https://supabase.com) for backend and auth  
- [Vite](https://vitejs.dev) and [React](https://react.dev) for the frontend toolchain  
- [Tailwind CSS](https://tailwindcss.com) and [Framer Motion](https://www.framer.com/motion/) for styling and animations  
- [Lucide](https://lucide.dev) for icons  
- [D3.js](https://d3js.org) and [Chart.js](https://www.chartjs.org) for data visualization  
- [Stripe](https://stripe.com) for payments  
- [Netlify](https://www.netlify.com) for hosting and serverless functions  

---

*Built for tattoo studios who want to automate calls and focus on what they do best.*
