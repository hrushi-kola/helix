# 🧠 Nexus — AI-Powered Company Knowledge Assistant

> **Ask your company's knowledge. Get intelligent, context-aware answers.**

Nexus is a full-stack, AI-powered enterprise knowledge assistant that lets employees interact with internal company documents in natural language. Rather than searching through policies, guides, and internal documentation manually, employees can ask questions such as: *“What is the annual leave policy?”*

Nexus retrieves relevant information from the organization’s knowledge base and uses an LLM to produce a grounded, context-aware answer with supporting sources.

## Why Nexus?

Modern organizations accumulate extensive internal information, including:

-  HR policies
-  Security guidelines
-  IT documentation
-  Company procedures
-  Travel policies
-  Expense policies
-  Employee guidelines

Traditional document search requires employees to know where information lives and which keywords to use. Nexus changes that interaction model:

```text
Employee → Ask a question → Retrieve relevant knowledge → Generate a contextual answer → Show supporting sources
```

## Core Features

### (a) AI Knowledge Assistant

Employees can interact with company knowledge using natural language.

```text
User:    What is the annual leave policy?
Nexus:   Employees are entitled to ...
Sources: 01_Annual_Leave_Policy.pdf
```

Responses are grounded in the organization’s internal knowledge base instead of relying solely on the model’s general knowledge.

### (b) Retrieval-Augmented Generation (RAG)

Nexus uses a Retrieval-Augmented Generation architecture:

```text
User question → Query processing → Document retrieval → Relevant chunks → Reranking → Context construction → LLM
→ Grounded answer → Source citations
```

This produces answers based on the organization’s own documents.

### (c) Hybrid Knowledge Retrieval

The modular retrieval pipeline supports:

- Document loading and text extraction
- Text chunking and embedding generation
- Vector search and hybrid retrieval
- Reranking and context construction
- Citation generation

### (d) LLM Integration

Nexus supports multiple LLM providers through a centralized LLM management layer. Current integrations include:

- Google Gemini
- Groq

The provider layer is separated from the application, making model integrations easy to switch or extend.

### (e) Authentication and Authorization

Nexus uses JWT authentication, role-based authorization, and Google Sign-In.

| Role | Capabilities |
| --- | --- |
| Employee | Create an account, sign in, use the AI assistant, access company knowledge, and view conversation history. |
| Administrator | Access the admin dashboard, upload company documents, manage the knowledge base, and monitor the document ecosystem. |

## System Architecture

```text
React + Vite UI
       │ HTTP / REST
       ▼
FastAPI Backend API
       │
       ├── Authentication layer
       ├── Chat / query layer ──────┐
       └── Documents layer ─────────┼── PDF / text processing
                                    ▼
                              RAG engine
                                    │
                         Retrieval pipeline
                  (chunking, embeddings, hybrid search,
                   reranking, and context building)
                                    │
                                 ChromaDB
                                    │
                              LLM manager
                            (Gemini / Groq)
                                    │
                            Grounded answer
```

## RAG Pipeline

### Document ingestion

```text
Company document → Document loader → Text extraction → Text chunking
→ Embedding generation → Vector storage → ChromaDB
```

### Question answering

```text
User query → Query processing → Semantic / hybrid retrieval
→ Candidate documents → Reranking → Top relevant context
→ Prompt construction → LLM → Final answer
```

This separation keeps knowledge retrieval distinct from language generation.

##  Project Structure

```text
Nexus/
├── backend/
│   ├── app/
│   │   ├── api/              # auth, chat, documents, health, history
│   │   ├── database/         # ChromaDB and SQLite access
│   │   ├── llm/              # Gemini, Groq, and LLM manager
│   │   ├── models/           # request and response models
│   │   ├── rag/              # retrieval and ingestion pipeline
│   │   ├── utils/            # authentication and citations
│   │   ├── config.py
│   │   └── main.py
│   ├── sample_documents/
│   ├── requirements.txt
│   └── .env
├── frontend/
│   ├── public/
│   │   └── nexus.svg
│   ├── src/
│   │   ├── components/       # chat, sources, sidebar, upload UI
│   │   ├── context/          # AuthContext and ChatContext
│   │   ├── pages/            # admin, authentication, chat, documents
│   │   ├── services/         # API client
│   │   ├── styles/
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── package.json
│   └── vite.config.js
├── .gitignore
└── README.md
```

## Technology Stack

| Area | Technology | Purpose |
| --- | --- | --- |
| Frontend | React | User interface |
| Frontend | Vite | Frontend build tooling |
| Frontend | React Router | Application routing |
| Frontend | Axios | API communication |
| Frontend | Lucide React | UI icons |
| Backend | Python | Backend language |
| Backend | FastAPI | REST API framework |
| Backend | Pydantic | Data validation and configuration |
| Backend | JWT | Authentication |
| Backend | Google Identity Services | Google authentication |
| AI / RAG | Gemini | LLM |
| AI / RAG | Groq | LLM inference |
| AI / RAG | Sentence Transformers | Embeddings |
| AI / RAG | ChromaDB | Vector database |
| Database | SQLite | Application data |
| Database | ChromaDB | Vector storage |

## Security

Nexus includes the following security mechanisms:

- JWT-based authentication
- Role-based authorization
- Protected API endpoints
- Admin-only document management
- Environment-based secret configuration
- Google OAuth / Identity Services
- API key protection through environment variables
- `.gitignore` protection for local secrets and runtime databases

### Environment variables

Store sensitive configuration in `backend/.env`:

```dotenv
ADMIN_EMAIL=your-admin-email
ADMIN_PASSWORD=your-admin-password

JWT_SECRET_KEY=your-secret-key
JWT_ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=60

GOOGLE_CLIENT_ID=your-google-client-id

GEMINI_API_KEY=your-gemini-key
GROQ_API_KEY=your-groq-key
```

Create `frontend/.env` as well:

```dotenv
VITE_API_URL=http://localhost:4000
VITE_GOOGLE_CLIENT_ID=your-google-client-id
```

> Never commit `.env` files or API keys to GitHub.

## Local Development

### 1. Clone the repository

```bash
git clone https://github.com/sai-hrushita-kolachina/Nexus.git
cd Nexus
```

### 2. Set up the backend

```bash
cd backend
python -m venv venv
```

Activate the virtual environment on Windows:

```powershell
venv\Scripts\activate
```

Install dependencies and start the API:

```bash
pip install -r requirements.txt
uvicorn app.main:app --reload --port 4000
```

The backend runs at `http://localhost:4000`.

### 3. Set up the frontend

In another terminal:

```bash
cd frontend
npm install
npm run dev
```

The frontend runs at `http://localhost:5173`.

## Admin Workflow

```text
Admin login → Admin dashboard → Upload document → Document processing
→ Text extraction → Chunking → Embeddings → Vector database → Available for RAG
```

Once documents are indexed, employees can query them through the AI assistant.

## Employee Workflow

```text
Employee login → Nexus chat → Ask question → Retrieve relevant knowledge
→ Generate answer → Display sources
```

## Example Queries

Once company documents are uploaded, employees can ask questions such as:

- What is the annual leave policy?
- What is the work-from-home policy?
- How many sick leaves are available?
- What is the employee attendance policy?
- What is the company’s information security policy?
- How do I report an IT issue?
- What is the expense reimbursement procedure?

The assistant retrieves relevant company information and generates an answer grounded in the available knowledge base.


## Future Improvements

-  Production-grade vector database
-  Persistent cloud storage
-  Advanced admin analytics
-  Support for additional document formats
-  Improved hybrid retrieval
-  Query classification
-  Streaming LLM responses
-  Enterprise user management
-  Retrieval and answer evaluation
-  Document versioning
-  Metadata-based filtering
-  More granular permissions
-  RAG observability and monitoring

##  Deployment

Nexus can be deployed as two services:

```text
GitHub
  ├── Vercel  → Frontend (React)
  └── Render  → Backend (FastAPI)
                       │
                  Nexus API
                 ┌─────┴─────┐
              ChromaDB     SQLite
                 │
             RAG engine
                 │
           Gemini / Groq
```

For production, configure all required environment variables in the hosting platform rather than committing secrets to the repository.

## Application

- **Employee Chat:** A conversational interface for interacting with company knowledge.
- **Admin Dashboard:** A workspace for managing the organization’s knowledge base and uploading documents.
- **Document Management:** Uploaded documents are processed and indexed for retrieval.


## Author

**Sai Hrushita Kolachina**

GitHub: [sai-hrushita-kolachina](https://github.com/sai-hrushita-kolachina)
