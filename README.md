<div align="center">
  <img src="https://tu-dominio.com/AiTania-banner.png" alt="AITania Banner" width="100%"/>
</div>

<br/>
# AITania: The Personal Day-to-Day Architect

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.100+-green.svg)](https://fastapi.tiangolo.com/)
[![LangChain](https://img.shields.io/badge/LangChain-0.1.0+-orange.svg)](https://www.langchain.com/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)

**AITania** is an open-source, voice-first LLM agent engineered to serve as a proactive, empathetic, and deeply integrated companion for navigating the complexities of daily life. She transcends the traditional virtual assistant paradigm, operating not as a command executor but as a **collaborative partner**—blending the roles of personal assistant, life coach, and executive function architect into a single, seamless interface.

By combining state-of-the-art large language models with a novel temporal reasoning engine, a secure personal knowledge graph, and a multi-modal interface, AITania reduces cognitive load, orchestrates complex workflows, and empowers users to focus on what matters most.

---

## Core Capabilities

### 1. Dynamic Orchestration
AITania doesn’t just use tools—she orchestrates them. A single natural language instruction can trigger a chain of cross-domain actions:
- **Travel Recovery:** `"Fix my travel mess"` → Parses flight change email, cross-references calendar, rebooks flight, reschedules dependent meetings.
- **Social Logistics:** `"Dinner for six on Friday, but I have a deadline"` → Suggests menu, builds shared grocery list, coordinates delivery, blocks deep work time.

### 2. Contextual Memory & Proactivity
A persistent, privacy-first memory architecture enables genuine anticipation:
- Remembers dietary restrictions, partner names, and recurring anxieties.
- Proactive nudges based on real-world context: *"Your car is due for service. Jordan’s birthday is next week—shall I book the appointment for Wednesday?"*
- Maintains emotional continuity across sessions.

### 3. Explainable Reasoning
Every action is accompanied by a transparent rationale, building trust and user agency:
> *"I moved your dentist appointment from 2 PM to 10 AM because your calendar shows post-lunch fatigue patterns. This opens a 3-hour block for your report. Does that work?"*

### 4. Well-being & Friction Reduction
- **Energy-Aware Scheduling:** Learns user productivity patterns to optimize task timing.
- **Mood Adaptation:** Detects vocal fatigue and proactively offers rescheduling, note-taking, or even comfort (e.g., ordering a familiar meal).

---

## Architecture

AITania is built on a hybrid architecture that balances generative flexibility with deterministic reliability.

```
┌─────────────────────────────────────────────────────────────┐
│                      Multi-Modal Interface                   │
│         (Voice / Text / Visual Dashboard)                    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    Orchestration Layer                       │
│          (LangChain / Custom Agent Executor)                 │
└─────────────────────────────────────────────────────────────┘
                              │
        ┌─────────────────────┼─────────────────────┐
        ▼                     ▼                     ▼
┌───────────────┐   ┌─────────────────┐   ┌─────────────────┐
│   LLM Core    │   │ Temporal Engine │   │ Knowledge Graph │
│ (Fine-tuned   │   │ (Schedule Opt., │   │ (Neo4j / Vector │
│  for Planning │   │  Time Reasoning)│   │  + Relational)  │
│  & Empathy)   │   │                 │   │                 │
└───────────────┘   └─────────────────┘   └─────────────────┘
        │                     │                     │
        └─────────────────────┼─────────────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                      Tool Ecosystem                          │
│ (Calendar, Email, Travel, Commerce, IoT, Health APIs)       │
└─────────────────────────────────────────────────────────────┘
```

### Key Components

| Component | Technology | Description |
|-----------|------------|-------------|
| **LLM Core** | GPT-4o / Llama 3 (fine-tuned) | Custom fine-tuned for planning, tool selection, and empathetic dialogue. |
| **Orchestration** | LangChain + Custom Agent | Manages chain-of-thought reasoning, tool invocation, and error recovery. |
| **Temporal Engine** | Custom Rust module + PostgreSQL | Handles time zones, recurring events, travel time, and energy-based scheduling. |
| **Knowledge Graph** | Neo4j + Qdrant (vector) | Stores relationships, preferences, and episodic memory with semantic retrieval. |
| **Voice Interface** | Whisper + ElevenLabs | Real-time transcription with emotion detection + naturalistic TTS. |
| **Backend** | FastAPI + WebSockets | Real-time bidirectional communication for streaming responses. |

---

## Getting Started

### Prerequisites
- Python 3.10+
- Docker & Docker Compose (for Neo4j, Qdrant, Redis)
- API keys for LLM provider (OpenAI, Anthropic, or local via Ollama)

### Installation

```bash
# Clone the repository
git clone https://github.com/your-org/aitania.git
cd aitania

# Create virtual environment
python -m venv venv
source venv/bin/activate  # or `venv\Scripts\activate` on Windows

# Install dependencies
pip install -r requirements.txt

# Copy environment template
cp .env.example .env
# Edit .env with your API keys and configuration

# Start infrastructure services
docker-compose up -d

# Run database migrations
alembic upgrade head

# Start the backend server
uvicorn app.main:app --reload --port 8000

# (Optional) Start the voice gateway
python -m services.voice_gateway
```

### Quick Test

```python
from aitania import AITania

# Initialize agent
tania = AITania(user_id="user_123")

# Simple interaction
response = tania.process(
    input="I'm overwhelmed with my schedule this week. Can you help me find some breathing room?",
    context={"mood": "stressed"}
)

print(response)
# Output: "I hear that. Let's look at your calendar together..."
```

---

## API Overview

AITania exposes a RESTful API with real-time WebSocket support for voice interactions.

### REST Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/v1/interact` | Send a text command, receive structured response |
| `POST` | `/api/v1/memory` | Query or update knowledge graph |
| `GET` | `/api/v1/calendar/optimize` | Request schedule optimization |
| `WS` | `/ws/v1/voice` | WebSocket for bidirectional voice streaming |

### Example Request

```json
POST /api/v1/interact
{
  "user_id": "user_123",
  "input": "Reschedule my 2pm meeting to tomorrow, and order a coffee for pickup in 15 minutes.",
  "session_context": {
    "location": "home",
    "energy_level": 0.3
  }
}
```

---

## Memory & Privacy

AITania takes privacy seriously. The memory architecture is designed with:
- **Local-first storage** for sensitive data (optional self-hosted mode)
- **Granular consent controls**—users can review, edit, or delete any memory
- **Ephemeral mode**—a "Do Not Remember" flag for sensitive conversations
- **Encrypted at rest** with user-owned keys (BYOK support)

---

## 🛠 Development

### Project Structure
```
aitania/
├── app/
│   ├── agents/          # Agent definitions and prompt templates
│   ├── core/            # LLM abstraction, orchestration logic
│   ├── tools/           # Tool implementations (calendar, email, etc.)
│   ├── memory/          # Knowledge graph and vector store interfaces
│   ├── temporal/        # Temporal reasoning engine
│   ├── api/             # FastAPI routes and WebSocket handlers
│   └── models/          # Pydantic schemas and DB models
├── services/
│   ├── voice_gateway/   # STT/TTS and emotion detection
│   └── scheduler/       # Background task processor
├── tests/               # Unit and integration tests
├── docker-compose.yml   # Infrastructure services
└── pyproject.toml       # Dependencies and tool config
```

### Running Tests
```bash
pytest tests/ -v --cov=app
```

### Contributing
We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines. Areas of particular interest:
- Additional tool integrations (smart home, health trackers, finance)
- Improved temporal reasoning algorithms
- Emotion detection and mood adaptation
- Local LLM inference support (Ollama, llama.cpp)

---

## 📊 Roadmap

| Milestone | Target |
|-----------|--------|
| **v0.1.0** | Core agent with calendar, email, and task tools |
| **v0.2.0** | Voice interface with emotion detection |
| **v0.3.0** | Temporal reasoning engine & proactive nudges |
| **v0.4.0** | Knowledge graph persistence & multi-user support |
| **v1.0.0** | Production-ready with self-hosting option |

---

## Philosophy

AITania is built on the belief that technology should **reduce, not create, cognitive load**. The goal is not to make users more "productive" in a corporate sense, but to give them back time, mental space, and intentionality. She exists to handle the friction of logistics so users can focus on creativity, connection, and presence.

> *"AITania doesn't manage your life. She helps you live it."*

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.

---

## Acknowledgments

- Built with [LangChain](https://www.langchain.com/), [FastAPI](https://fastapi.tiangolo.com/), and [Neo4j](https://neo4j.com/)
- Voice capabilities powered by [Whisper](https://openai.com/research/whisper) and [ElevenLabs](https://elevenlabs.io/)
- Inspired by research on executive function augmentation and affective computing

---

## Contact

- **Project Lead:** [@OslarCode](https://github.com/OslarCode)
- **Issues:** [GitHub Issues](https://github.com/your-org/aitania/issues)

---

*Built with ❤️ for the future of human-AI collaboration.*
