# Twelve - Mobile-first Tarot & Astrology Insights

A beautifully minimal web app combining handwritten tarot readings with AI-powered astrological guidance.

## Features

### Tarot Horoscopes
- **Manual-only** tarot content (never AI-generated)
- Daily, Weekly, and Monthly readings
- All 12 zodiac signs with traditional colored icons
- Admin CMS for content management
- Optional card images and external reader directory

### Astrology AI Chat
- Ask yes/no, timing, career, relationship, or open-ended questions
- AI responds with: Yes, No, Maybe, or Unclear
- Warm, emotionally-aware tone (not robotic)
- Powered by Swiss Ephemeris (local chart calculations)
- Knowledge base built from public-domain astrology sources

## Design
- **Mobile-first** (iPhone baseline width)
- Pure white background, black text, thin black borders
- Rounded corners throughout
- Only zodiac icons use color
- Minimal, clean, readable typography

## Tech Stack

- **Frontend:** React 18 + TypeScript + Vite
- **Backend:** Node.js + Express + TypeScript
- **Database:** PostgreSQL
- **Astrology Engine:** Swiss Ephemeris (Python wrapper)
- **Deployment:** Docker & Docker Compose

## Quick Start

### Prerequisites
- Docker & Docker Compose
- Node.js 18+ (for local development)
- Python 3.9+ (for Swiss Ephemeris)

### Setup

1. Clone the repository:
   ```bash
   git clone https://github.com/r2sgsy8vxz-ui/Twelve.git
   cd Twelve
   ```

2. Copy environment file:
   ```bash
   cp .env.example .env
   ```

3. Edit `.env` with your settings (database, admin token, etc.)

4. Start with Docker:
   ```bash
   docker-compose up -d
   ```

5. Access the site:
   - Frontend: http://localhost:3000
   - Backend API: http://localhost:5000

6. Initialize the database:
   ```bash
   docker-compose exec backend npm run migrate
   ```

## Project Structure

```
Twelve/
├── frontend/                 # React UI
│   ├── src/
│   │   ├── components/      # UI components
│   │   ├── App.tsx
│   │   ├── App.css
│   │   └── main.tsx
│   ├── index.html
│   ├── vite.config.ts
│   ├── tsconfig.json
│   └── package.json
├── backend/                  # Node.js API
│   ├── src/
│   │   ├── routes/          # API endpoints
│   │   ├── services/        # Business logic
│   │   ├── middleware/      # Auth, logging
│   │   └── server.ts
│   ├── sql/                 # Database schemas
│   ├── ephemeris/           # Python wrappers
│   ├── tsconfig.json
│   ├── package.json
│   └── Dockerfile
├── docker-compose.yml
├── .env.example
├── .gitignore
└── README.md
```

## API Endpoints

### Public
- `GET /api/tarot/daily` - Daily tarot readings
- `GET /api/tarot/weekly` - Weekly tarot readings
- `GET /api/tarot/monthly` - Monthly tarot readings
- `POST /api/chat` - Astrology question/answer

### Admin (requires ADMIN_TOKEN)
- `GET /api/admin/tarot` - List all tarot readings
- `POST /api/admin/tarot` - Create/update tarot reading
- `GET /api/admin/kb` - List knowledge base
- `POST /api/admin/kb` - Add KB fragment

## Database

The PostgreSQL schema includes:

- **tarot_readings** - All tarot content (daily/weekly/monthly for each zodiac)
- **knowledge_base** - Astrology KB fragments (Yes/No/Maybe/Unclear, by question type)
- **chat_interactions** - Logged conversations for admin review

Schema is auto-initialized on first `docker-compose up`.

## Admin CMS

Add tarot readings via API:

```bash
curl -X POST http://localhost:5000/api/admin/tarot \
  -H "Authorization: Bearer YOUR_ADMIN_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "daily",
    "zodiacSign": "Aries",
    "readingText": "Today brings new opportunities...",
    "cardImageUrl": "https://example.com/card.jpg"
  }'
```

## Interpretation Tone

All astrology responses follow this structure:

1. **First line:** Yes, No, Maybe, or Unclear (clear verdict)
2. **Tone:** Warm, honest, emotionally aware, grounded (not robotic)
3. **Content:** Acknowledge emotions, provide perspective, suggest next steps
4. **Timing:** Optional simple phrases like "soon", "within a couple weeks", "later this month"
5. **Length:** 4–6 lines total

### Example Response

**Question:** "Will my job offer come through?"

**Response:**
"Yes. You've been holding yourself back more than the situation requires. The offer is moving forward, and you deserve to trust that. Don't second-guess yourself right now. You'll have clarity in the next couple of weeks."

## Environment Variables

Create a `.env` file (see `.env.example`):

```env
# Database
DATABASE_URL=postgresql://tarot_user:secure_password@postgres:5432/twelve_db

# Server
NODE_ENV=development
PORT=5000

# Admin
ADMIN_TOKEN=your_secure_admin_token_here

# Ephemeris
EPHEMERIS_SCRIPT_PATH=./ephemeris/calculate.py
EPHEMERIS_DATA_PATH=/opt/swiss_ephemeris/data
```

## Development

### Frontend Only
```bash
cd frontend
npm install
npm run dev
```

### Backend Only
```bash
cd backend
npm install
npm run dev
```

### With Database
```bash
docker-compose up postgres
# In another terminal:
cd backend && npm run dev
```

## Production Deployment

1. Set `NODE_ENV=production` in `.env`
2. Use strong `ADMIN_TOKEN`
3. Configure `DATABASE_URL` to production PostgreSQL
4. Build and push Docker images:
   ```bash
   docker build -t twelve-frontend ./frontend
   docker build -t twelve-backend ./backend
   ```
5. Deploy using your preferred platform (AWS, DigitalOcean, Heroku, etc.)

## License

MIT License - feel free to use this project however you like.

## Support

For questions or issues, open a GitHub issue or contact the maintainers.

---

**Built with ❤️ for intuitive, grounded guidance.**