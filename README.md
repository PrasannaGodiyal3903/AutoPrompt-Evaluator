# AutoPrompt-Evaluator
A full-stack AI SaaS tool where users submit a task, get 3 auto-generated prompt variants, LLM responses for each, and a scored + ranked evaluation of which prompt performed best.

High-Level Architecture
User
  ↓
React Frontend (Vite + TypeScript + Tailwind)
  ↓ REST API calls
Express Backend (Node.js + TypeScript)
  ↓              ↓
MongoDB        OpenAI API
(data store)   (prompt execution + evaluation)

Folder Structure
Backend
backend/
├── src/
│   ├── config/
│   │   ├── db.ts              # MongoDB connection
│   │   └── env.ts             # Environment variables
│   ├── models/
│   │   ├── Experiment.ts
│   │   ├── PromptVariant.ts
│   │   ├── Response.ts
│   │   └── Evaluation.ts
│   ├── controllers/
│   │   ├── experiment.controller.ts
│   │   └── evaluation.controller.ts
│   ├── services/
│   │   ├── promptGenerator.service.ts
│   │   ├── llm.service.ts
│   │   └── evaluator.service.ts
│   ├── routes/
│   │   ├── experiment.routes.ts
│   │   └── evaluation.routes.ts
│   ├── middleware/
│   │   ├── errorHandler.ts
│   │   └── validateRequest.ts
│   └── index.ts
├── .env
├── package.json
└── tsconfig.json
Frontend
frontend/
├── src/
│   ├── api/
│   │   └── experimentApi.ts
│   ├── components/
│   │   ├── ExperimentForm.tsx
│   │   ├── PromptCard.tsx
│   │   ├── ScoreChart.tsx
│   │   └── RankingTable.tsx
│   ├── pages/
│   │   ├── Dashboard.tsx
│   │   ├── NewExperiment.tsx
│   │   └── ExperimentDetail.tsx
│   ├── hooks/
│   │   └── useExperiment.ts
│   ├── types/
│   │   └── index.ts
│   └── App.tsx
├── package.json
└── tailwind.config.ts

Request Flow
1. User submits task → POST /api/experiments
2. Backend generates 3 prompt variants
3. Each variant sent to OpenAI → 3 responses collected
4. Evaluator scores each response (LLM-as-judge)
5. Scores weighted → final score calculated
6. Variants ranked 1st / 2nd / 3rd
7. Full result returned to frontend
8. Frontend renders dashboard + detail view

API Endpoints
POST   /api/experiments          # Create experiment, run full pipeline
GET    /api/experiments          # Get all experiments (dashboard)
GET    /api/experiments/:id      # Get single experiment with full results
DELETE /api/experiments/:id      # Delete experiment

Key Design Decisions & Why
Single POST triggers the full pipeline — keeps the frontend simple. One call does everything: generate → execute → evaluate → rank.
Services are separated — promptGenerator, llm, evaluator are independent. If you want to swap OpenAI for Gemini later, you only touch llm.service.ts.
LLM-as-judge — instead of hardcoded rules, we send the response back to the LLM and ask it to score it. More flexible, more accurate, and genuinely impressive to explain.
MongoDB over SQL — experiment results are nested (variants → responses → scores). Document model fits naturally.

Environment Variables Needed
MONGODB_URI=
OPENAI_API_KEY=
PORT=5000
NODE_ENV=development
FRONTEND_URL=http://localhost:5173
