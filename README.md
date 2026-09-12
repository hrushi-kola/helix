# 🩺 MedCare

MedCare is a full-scale **AI + Full-Stack Web Application** that combines **AI-powered conversational healthcare assistance**, **medical document understanding**, **doctor discovery**, and **digital session booking management** in one platform.

This project integrates **Large Language Models (LLMs)**, modern web development, secure authentication, online payments, AI-provider fallback, and independently deployed services to reflect real-world healthcare software architecture.

### Live Demo: https://medcare24.vercel.app/

---

## 1. Introduction

#### Healthcare often begins with uncertainty. Patients may have questions about:

- Their symptoms or health concerns
- Medical terminology and reports
- Which specialist to consult
- How to proceed efficiently

#### **MedCare bridges this gap** by offering:

- AI-powered conversational healthcare assistance
- Medical-document understanding and explanation
- Doctor and specialization discovery
- Online session booking and tele-consultation
- Secure digital payments

#### MedCare is **not a simple CRUD application**. It is a distributed, production-style system combining:

- Web development
- Artificial intelligence and Large Language Models
- Conversational AI
- Cloud deployment
- REST API architecture
- Secure authentication
- Payment integration
- Production-level debugging

---

## 2. Project Goals

#### The primary goals of this project are:

- Build an AI-powered conversational healthcare assistant
- Integrate Large Language Models with a full-stack web application
- Maintain context within the active conversation
- Help users understand medical and non-medical documents
- Provide doctor discovery and specialization information
- Implement secure online appointment and tele-consultation booking
- Integrate secure digital payments
- Design stable frontend–backend–AI communication
- Implement AI provider fallback handling
- Simulate real-world healthcare software architecture

---

## 3. High-Level Architecture

MedCare follows a **multi-service architecture**, where the frontend, backend, and admin panel are independently deployed and communicate through REST APIs.

```text
                         User Browser
                              │
                              ▼
                       Frontend / Admin
                              │
                              ▼
                           Backend
              ┌───────────────┼───────────────┐
              ▼               ▼               ▼
       AI Orchestrator      MongoDB       Payment APIs
              │
       ┌──────┼──────┐
       ▼      ▼      ▼
    Gemini  OpenAI  Groq
```

The backend provides authentication, doctor, booking, payment, document-processing, and AI-chat APIs. The configured AI-provider order is:

```text
Gemini → OpenAI → Groq
```

If a provider is unavailable or reaches its configured usage limit, the system can attempt the next available provider.

---

## 4. Core Capabilities

### Patient

- Secure registration and login
- Natural conversation with Healio AI
- Ask healthcare and general-purpose questions
- Discuss symptoms conversationally
- Understand medical terminology and uploaded documents
- Browse doctors and specializations
- Book consultation sessions and tele-consultations online
- Pay consultation fees securely
- View booking history

### Admin

- Add, update, and remove doctors
- Handle bookings and doctor details
- Manage platform operations
- Use a unified admin dashboard

---

## 5. Installation & Setup

Follow the steps below to run **MedCare** locally.

### (a) Clone the repository

```bash
git clone https://github.com/sai-hrushita-kolachina/MedCare.git
cd MedCare
```

### (b) Backend setup

```bash
cd backend
npm install
npm start
```

Create a `.env` file inside `backend`:

```dotenv
MONGO_URL=your_mongodb_url
STRIPE_KEY_SECRET=your_stripe_key_secret
JWT_SECRET=your_secret_key
PORT=4000

VITE_FRONTEND_URL=http://localhost:5173
VITE_ADMIN_URL=http://localhost:5174

AI_PROVIDER_ORDER=gemini,openai,groq

GEMINI_API_KEY=your_gemini_api_key
GEMINI_MODEL=gemini-3.7-flash

OPENAI_API_KEY=your_openai_api_key
OPENAI_MODEL=gpt-5

GROQ_API_KEY=your_groq_api_key
GROQ_MODEL=openai/gpt-oss-120b
```

### (c) Frontend setup

```bash
cd frontend
npm install
npm run dev
```

Create a `.env` file inside `frontend`:

```dotenv
VITE_BACKEND_URL=http://localhost:4000
```

### (d) Admin panel setup

```bash
cd admin
npm install
npm run dev
```

Create a `.env` file inside `admin`:

```dotenv
VITE_BACKEND_URL=http://localhost:4000
```

## Service Connectivity Overview

- Frontend → `http://localhost:5173`
- Admin → `http://localhost:5174`
- Backend → `http://localhost:4000`

> Never commit `.env` files or API keys to GitHub.

---

## 6. Frontend

### Tech Stack

- React.js (Vite)
- JavaScript (ES6+)
- HTML / CSS
- Axios / Fetch API
- React Markdown

### Overview

- Deployed on Vercel
- Renders the user interface
- Supports Healio AI chatbot interaction
- Sends conversational requests to the backend
- Supports medical-document upload and displays AI responses
- Supports doctor browsing, filtering, booking, and route protection

The frontend communicates with the backend through REST APIs. It never communicates directly with AI providers, so provider API keys are not exposed in the browser.

---

## 7. Backend

### Tech Stack

- MongoDB (Mongoose ODM)
- Node.js
- Express.js
- JWT Authentication
- Stripe API & Webhooks
- Gemini, OpenAI, and Groq APIs

### Overview

- User authentication and authorization
- Role-based access control
- Doctor management
- Tele-consultation booking system
- Secure REST APIs
- AI chatbot orchestration and provider fallback handling
- Temporary document processing
- Payment verification
- Admin dashboard support

The main chatbot logic is separated from individual AI providers:

```text
AI Chat Controller
        │
        ▼
AI Orchestrator
        │
        ▼
AI Provider Layer
   ┌────┼────┐
   ▼    ▼    ▼
Gemini OpenAI Groq
```

---

## 8. Admin Panel

### Tech Stack

- React.js (Vite)
- JavaScript (ES6+)
- HTML / CSS
- Axios / Fetch API

### Overview

- Admin authentication and dashboard access control
- Add, edit, and delete doctors
- Manage doctor details
- View and update booking status
- Monitor user bookings
- Secure admin-only routes

---

## 9. LLM-Powered Healthcare Assistance

### AI capabilities

Healio uses Large Language Models to provide natural, context-aware interaction. It can:

- Answer healthcare-related and general-purpose questions
- Discuss symptoms and offer health guidance
- Ask follow-up questions when more context is useful
- Explain medical terminology
- Understand uploaded medical documents
- Summarize documents and extract information
- Explain difficult sections and compare information within a document

### Conversation context

The backend sends the user's message together with relevant current conversation history to the selected provider. This lets Healio understand references during the active chat.

```text
User: What is diabetes?
Healio: Diabetes is a condition that affects how the body regulates blood glucose.

User: What are its symptoms?
Healio: Common symptoms can include increased thirst, frequent urination,
         fatigue, and unexplained weight changes.
```

Healio uses current-conversation context, not permanent AI memory.

### Document context

Users can upload documents during a conversation. Document content is used as temporary context, allowing Healio to summarize it, extract information, explain terminology, and answer questions grounded in the uploaded material.

---

## 10. Chatbot (Healio)

### Healio is an AI-powered chatbot that:

- Accepts natural-language questions and symptom descriptions
- Maintains context within the active conversation
- Provides healthcare-focused guidance and general assistance
- Explains medical terminology and uploaded documents
- Uses provider fallback to improve availability

The chatbot makes healthcare interaction more accessible and conversational while encouraging professional consultation when appropriate.

---

## 11. Payment Gateway Integration (Stripe)

MedCare integrates Stripe for secure online payments.

### Payment Features

- Stripe Checkout on the frontend
- Backend order creation
- Payment verification after success
- Booking confirmation only after payment
- Supports UPI, cards, net banking, and wallets

No card or UPI data is stored on the server.

---

## 12. CORS & Deployment Challenges

### Services are deployed on different platforms

- Frontend → Vercel
- Backend → Render
- Admin → Vercel

### CORS configuration covers

- Allowed origins
- Headers
- Preflight requests
- Frontend–backend communication

Environment variables are configured separately for local development and production deployment.

---

## 13. Production Debugging Experience

### Key real-world issues addressed

- JSON payload structure errors
- Runtime crashes
- API integration issues
- CORS configuration problems
- AI-provider quota issues and fallback handling
- Document-processing issues
- Frontend–backend communication issues
- Environment configuration and deployment issues
- Booking and payment integration issues

---

## 14. Security Measures

- JWT-based authentication
- Protected admin routes
- Role-based access control
- Input sanitization
- Restricted CORS origins
- Secure payment processing through Stripe
- API keys stored using environment variables
- AI credentials kept on the backend
- Sensitive environment files excluded from version control

---

## 15. Deployment

- Frontend → Vercel
- Backend → Render
- Admin → Vercel

For production, configure the frontend with your deployed backend URL:

```dotenv
VITE_BACKEND_URL=https://your-live-backend-url
```

---

## 16. AI Provider Architecture

### Why use a provider abstraction layer?

The AI provider layer keeps the core chatbot logic independent from Gemini, OpenAI, and Groq. This improves reliability and makes providers easier to change or extend without rewriting the main chat flow.

### Benefits

- Fallback if one provider is unavailable or quota-limited
- Clean separation of concerns
- Flexible model and provider updates
- Better resilience for a production-style AI system

---

## 17. Learning Outcomes

### This project strengthened understanding of:

- AI and full-stack web integration
- Large Language Model integration
- Conversational AI architecture
- AI-provider abstraction and fallback systems
- Document processing and AI-assisted understanding
- Secure API design
- Distributed systems
- Payment gateway integration
- Cloud deployment and production debugging workflows

---

## 18. Future Enhancements

- Multi-Factor Authentication (MFA)
- Doctor ratings and reviews
- Email / SMS notifications
- Booking-cancellation refunds
- Enhanced AI document analysis
- Voice-based interaction with Healio
- Multilingual healthcare assistance

---

## 19. Final Notes

- MedCare demonstrates how AI systems and modern web technologies can be responsibly integrated into a healthcare platform.
- Healio provides conversational AI assistance while professional doctor consultation remains an important part of the healthcare workflow.
- The platform emphasizes clean architecture, secure APIs, reliable service integration, and real-world engineering practices.
- AI-generated information is intended for assistance and education and should not replace professional medical advice.
- © 2026 – MedCare

---

## 20. Author

- Sai Hrushita Kolachina

