# AutoPrompt Evaluator

A full-stack AI SaaS application where users submit a task, receive three AI-generated prompt variations, execute all variants against an LLM, and get an automated evaluation showing which prompt performed best.

---

## Overview

AutoPrompt Evaluator helps users compare prompt engineering strategies by automatically:

- Generating multiple prompt variants from a single task
- Executing each prompt against an LLM
- Evaluating response quality using an LLM-as-a-Judge approach
- Ranking prompts based on weighted scoring criteria
- Displaying detailed results and comparisons

This project demonstrates backend architecture, AI workflow orchestration, prompt engineering, API integration, evaluation pipelines, and full-stack development.

---

## High-Level Architecture

```text
User
  ↓
React Frontend (Vite + TypeScript + Tailwind)
  ↓ REST API calls
Express Backend (Node.js + TypeScript)
  ↓              ↓
MongoDB        OpenAI API
(data store)   (prompt execution + evaluation)
```

---

## Folder Structure

### Backend

```text
backend/
├── src/
│   ├── config/
│   │   ├── db.ts
│   │   └── env.ts
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
```

### Frontend

```text
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
```

---

## Request Flow

### 1. User Creates Experiment

```http
POST /api/experiments
```

The user submits a task description.

### 2. Prompt Generation

The backend generates three unique prompt variants using AI.

### 3. Prompt Execution

Each prompt variant is sent to the LLM independently.

### 4. Response Collection

Three separate responses are gathered.

### 5. Automated Evaluation

Each response is evaluated using an LLM-as-a-Judge strategy.

### 6. Score Calculation

Scores are weighted and aggregated into a final score.

### 7. Ranking

Prompt variants are ranked:

1. Best Prompt
2. Second Best Prompt
3. Third Best Prompt

### 8. Result Visualization

The frontend displays:

- Prompt variants
- Generated responses
- Individual scores
- Rankings
- Evaluation summaries

---

## API Endpoints

| Method | Endpoint | Description |
|----------|----------|-------------|
| POST | `/api/experiments` | Create experiment and run complete pipeline |
| GET | `/api/experiments` | Get all experiments |
| GET | `/api/experiments/:id` | Get experiment details |
| DELETE | `/api/experiments/:id` | Delete experiment |

---

## Core Features

### Prompt Variant Generation

Generate multiple prompt strategies from a single task.

### LLM Execution Pipeline

Run all prompt variants against the selected language model.

### AI-Based Evaluation

Use an LLM to judge response quality based on predefined criteria.

### Prompt Ranking

Automatically identify the highest-performing prompt.

### Experiment History

Store experiments and evaluations in MongoDB.

### Analytics Dashboard

View scores, rankings, and historical experiment data.

---

## Key Design Decisions

### Single Pipeline Endpoint

A single API call executes the entire workflow:

```text
Generate → Execute → Evaluate → Rank
```

This keeps frontend complexity minimal.

### Service-Based Architecture

Responsibilities are separated into dedicated services:

- Prompt Generation Service
- LLM Service
- Evaluation Service

This makes future provider changes simple.

### LLM-as-a-Judge

Instead of relying on hardcoded scoring rules, the system uses an LLM to evaluate outputs.

Benefits:

- More flexible
- Easier to extend
- Better reasoning quality
- Strong AI engineering demonstration

### MongoDB Document Model

Experiment data is naturally nested:

```text
Experiment
 ├── Prompt Variants
 ├── Responses
 └── Evaluations
```

MongoDB handles this structure efficiently without requiring complex relational schemas.

---

## Environment Variables

```env
MONGODB_URI=
OPENAI_API_KEY=
PORT=5000
NODE_ENV=development
FRONTEND_URL=http://localhost:5173
```

---

## Tech Stack

### Frontend

- React
- TypeScript
- Vite
- Tailwind CSS

### Backend

- Node.js
- Express.js
- TypeScript

### Database

- MongoDB
- Mongoose

### AI Layer

- OpenAI API
- Prompt Engineering
- LLM-as-a-Judge Evaluation

---

## Future Enhancements

- Support for Gemini, Claude, and OpenRouter
- User authentication
- Prompt versioning
- Experiment sharing
- Evaluation criteria customization
- Cost and token usage tracking
- A/B testing dashboards
- Export results as PDF or CSV

---

## Learning Outcomes

This project demonstrates:

- Full-stack development
- REST API design
- AI workflow orchestration
- Prompt engineering
- LLM evaluation systems
- Backend architecture
- Database modeling
- TypeScript development
- SaaS application design
