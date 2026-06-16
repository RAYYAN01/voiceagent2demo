# Naaz Resort — AI Voice Agent & CRM

A full-stack hotel management system with an AI voice agent, public booking website, and internal CRM dashboard.

**Live:** https://project-8rvir.vercel.app
**CRM:** https://project-8rvir.vercel.app/crm

---

## Overview

| Layer | Tech |
|---|---|
| Backend | FastAPI (Python) + SQLAlchemy + SQLite |
| CRM Frontend | React 18 + Vite + Tailwind CSS + Recharts |
| Website Frontend | React 18 + Vite + Tailwind CSS + Framer Motion |
| AI Voice | OpenAI GPT-4o-mini + Deepgram + ElevenLabs |
| Deployment | Vercel (serverless) |
| Notifications | SendGrid (email) + WhatsApp wa.me links |

---

## Project Structure

```
voiceagentbase2/
├── backend/                  # FastAPI backend (deployed to Vercel)
│   ├── api/index.py          # Vercel serverless entry point
│   ├── main.py               # FastAPI app, static file serving
│   ├── requirements.txt
│   ├── vercel.json
│   ├── app/
│   │   ├── config.py         # Environment settings
│   │   ├── database.py       # SQLAlchemy + SQLite setup
│   │   ├── models/           # ORM models (Booking, Lead, Spa, etc.)
│   │   ├── routes/           # API route handlers
│   │   │   ├── admin_api.py  # CRM REST endpoints
│   │   │   ├── chat.py       # Chat/AI conversation endpoints
│   │   │   ├── twilio_voice.py
│   │   │   └── seed.py       # Database seeding
│   │   └── services/
│   │       ├── booking_service.py
│   │       ├── notifications.py  # Email + WhatsApp
│   │       ├── ai_service.py
│   │       └── voice_agent.py
│   ├── crm-dist/             # Built CRM (auto-generated, not committed)
│   └── www-dist/             # Built website (auto-generated, not committed)
│
├── frontend/                 # CRM Admin Panel source
│   └── src/
│       ├── pages/            # Dashboard, Bookings, Spa, Housekeeping, etc.
│       ├── components/       # Sidebar, ErrorBoundary, ChatWidget
│       └── services/api.js   # API client
│
└── frontend-website/         # Public Hotel Website source
    └── src/
        ├── pages/            # Home, Rooms, Spa, Dining, Booking, etc.
        ├── components/       # Navbar, Footer, BookingForm, ChatWidget
        └── services/api.js   # API client
```

---

## Branches

| Branch | Description |
|---|---|
| `main` | Full project — backend + both frontends |
| `website` | Public hotel website (`frontend-website/`) |
| `crm` | CRM admin panel (`frontend/`) |

---

## CRM Modules

| Module | Route |
|---|---|
| Dashboard | `/crm/#/` |
| Bookings | `/crm/#/bookings` |
| Leads | `/crm/#/leads` |
| Rooms | `/crm/#/rooms` |
| Spa & Wellness | `/crm/#/spa` |
| Restaurant | `/crm/#/restaurant` |
| Housekeeping | `/crm/#/housekeeping` |
| Activities | `/crm/#/activities` |
| Complaints | `/crm/#/complaints` |
| Loyalty | `/crm/#/loyalty` |
| Events | `/crm/#/events` |
| Call Logs | `/crm/#/calls` |
| Chat Conversations | `/crm/#/chat` |

---

## API Endpoints

```
GET  /api/health
GET  /api/dashboard
GET  /api/bookings
POST /api/bookings
GET  /api/bookings/{id}
POST /api/bookings/{id}/confirm    # Confirms + sends email + WhatsApp link
POST /api/bookings/{id}/pay        # Marks paid + confirms + sends notifications
POST /api/bookings/{id}/cancel
GET  /api/leads
POST /api/leads
GET  /api/rooms/availability
GET  /api/spa
POST /api/spa/book
GET  /api/restaurant
POST /api/restaurant/reserve
GET  /api/housekeeping
POST /api/housekeeping
POST /api/housekeeping/{id}/status
GET  /api/activities
POST /api/activities
GET  /api/complaints
POST /api/complaints
POST /api/complaints/{id}/resolve
GET  /api/loyalty
POST /api/loyalty
GET  /api/events
POST /api/events
POST /api/events/{id}/status
GET  /api/chat/conversations
GET  /api/chat/conversations/{id}
POST /api/chat/message
```

---

## Local Development

### Backend

```bash
cd backend
pip install -r requirements.txt
uvicorn main:app --reload --port 8000
```

### CRM Frontend

```bash
cd frontend
npm install
npm run dev
```

### Website Frontend

```bash
cd frontend-website
npm install
npm run dev
```

---

## Environment Variables

Copy `.env.example` to `backend/.env` and fill in:

```env
OPENAI_API_KEY=           # GPT-4o-mini for AI chat
DEEPGRAM_API_KEY=         # Speech-to-text
ELEVENLABS_API_KEY=       # Text-to-speech
TWILIO_ACCOUNT_SID=       # Voice calls
TWILIO_AUTH_TOKEN=
TWILIO_PHONE_NUMBER=
SENDGRID_API_KEY=         # Email confirmations
RESORT_FROM_EMAIL=reservations@naazresort.com
BASE_URL=https://project-8rvir.vercel.app
```

---

## Deployment

```bash
# 1. Build CRM
cd frontend && npm run build
cp -r dist ../backend/crm-dist

# 2. Build website (auto-outputs to backend/www-dist)
cd frontend-website && npm run build

# 3. Deploy
cd backend && vercel --prod
```

---

## Booking Flow (Website)

1. Guest fills in details on `/booking`
2. Live price preview shown (nights × room rate)
3. Payment step — QR code + amount in USD & INR
4. Guest pays via UPI / Card / Net Banking
5. Click **Confirm Payment** → booking saved as `confirmed` in CRM
6. Email confirmation sent (requires SendGrid key)

## Booking Confirmation (CRM)

- Click ✓ on any pending booking in the list → instantly confirms
- WhatsApp button appears → opens `wa.me` with pre-filled confirmation message
- Email confirmation sent automatically if SendGrid is configured

---

Built for **Naaz Resort** by [Naazai Labs](https://naazailabs.com)
