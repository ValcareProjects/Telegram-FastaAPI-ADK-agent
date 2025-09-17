# Ticket_BOook 🎫

A Telegram-based intelligent assistant bot powered by Google AI that helps users with tour planning, booking assistance, and email content generation.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [API Documentation](#api-documentation)
- [Development](#development)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)

## 🎯 Overview

Ticket_BOook is an intelligent Telegram bot that leverages Google's Gemini AI to provide comprehensive assistance. The bot features a multi-agent architecture that can handle complex tasks through specialized sub-agents for tour planning, purchase assistance, and email content generation.

### Core Capabilities

- **Tour Planning**: Find hotels and flights for your travel needs
- **Purchase Assistance**: Help with booking and purchase decisions
- **Email Content Generation**: Create professional email content
- **Multi-Agent Coordination**: Intelligent task delegation between specialized agents
- **Persistent Sessions**: Maintain conversation context across interactions

## ✨ Features

### 🧳 Tour Planning Agent
- **Hotel Search**: Find available hotels using Amadeus API
- **Flight Search**: Discover flight options for your destinations
- **Parallel Processing**: Simultaneous hotel and flight searches for efficiency

### 🛒 Purchase Helper Agent
- Booking assistance and guidance
- Purchase decision support
- Price comparison and recommendations

### ✉️ Email Content Generator Agent
- Professional email composition
- Context-aware content generation
- Customizable email templates

### 🤖 Manager Agent
- Intelligent task routing to appropriate sub-agents
- Response formatting and optimization
- User-friendly output generation

## 🏗️ Architecture

The system follows a hierarchical multi-agent architecture:

```
Manager Agent (Root)
├── Tour Planner Agent
│   ├── Hotel Finder Agent
│   └── Flight Finder Agent
├── Purchase Helper Agent
└── Email Content Generator Agent
```

### Key Components

- **FastAPI Backend**: RESTful API with webhook support
- **Telegram Integration**: Real-time bot communication
- **Google ADK Framework**: Agent orchestration and management
- **SQLite Database**: Session and state persistence
- **Multi-Agent System**: Specialized task handling

## 📋 Prerequisites

- Python 3.8+
- Telegram Bot Token
- Google GenAI API Key
- Amadeus API credentials (for travel features)

## 🚀 Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd Ticket_BOook
   ```

2. **Create virtual environment**
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

## ⚙️ Configuration

### 1. Environment Variables

Create a `.env` file in the project root:

```env
# Google GenAI API Key
GENAI_API_KEY=your_google_genai_api_key_here

# Get your API key from: https://ai.google.dev/
```

### 2. Telegram Bot Setup

1. Create a bot using [@BotFather](https://t.me/botfather)
2. Get your bot token
3. Update the `TELEGRAM_TOKEN` in `main.py`

### 3. Amadeus API (Optional - for travel features)

Update the API credentials in `main_agent/sub_agents/tour_planner/sub_agents/hotel_finder/tools.py`:

```python
AMADEUS_API_KEY = "your_amadeus_api_key"
AMADEUS_API_SECRET = "your_amadeus_api_secret"
```

## 🎮 Usage

### Starting the Bot

1. **Local Development**
   ```bash
   python main.py
   ```
   The FastAPI server will start on `http://localhost:8000`

2. **Set up Telegram Webhook**
   Configure your Telegram bot webhook to point to:
   ```
   https://your-domain.com/webhook
   ```

### Bot Commands

Simply send natural language messages to your bot:

- "Find hotels in Paris for next week"
- "Help me book a flight to London"
- "Generate an email for meeting confirmation"
- "Plan a trip to Tokyo with hotel and flight options"

### Email Configuration

Users can configure their email for sending capabilities:
```
/set_email your_email@gmail.com your_app_password
```

## 📁 Project Structure

```
Ticket_BOook/
├── main.py                     # FastAPI application entry point
├── per_db.py                   # Database and session management
├── basic_stateful_session.py   # Session state handling
├── .env                        # Environment configuration
├── main_agent/
│   ├── agent.py               # Root manager agent
│   └── sub_agents/
│       ├── tour_planner/
│       │   ├── agent.py       # Tour planning coordinator
│       │   └── sub_agents/
│       │       ├── hotel_finder/
│       │       │   ├── agent.py     # Hotel search agent
│       │       │   ├── tools.py     # Amadeus API integration
│       │       │   └── __init__.py
│       │       └── flight_finder/
│       │           ├── agent.py     # Flight search agent
│       │           └── __init__.py
│       ├── purchase_helper/
│       │   ├── agent.py       # Purchase assistance agent
│       │   └── __init__.py
│       └── email_content_generator/
│           ├── agent.py       # Email generation agent
│           └── __init__.py
└── my_agent_data.db          # SQLite database for sessions
```

## 📚 API Documentation

### Webhook Endpoint

**POST** `/webhook`
- Receives Telegram bot updates
- Processes user messages through agent system
- Returns bot responses

**GET** `/`
- Health check endpoint
- Returns API status

### Agent Communication Flow

1. User sends message via Telegram
2. Webhook receives and processes message
3. Manager agent analyzes and routes to appropriate sub-agent
4. Sub-agent processes task and returns response
5. Manager agent formats response for user
6. Response sent back via Telegram

## 🛠️ Development

### Adding New Agents

1. Create agent directory in `main_agent/sub_agents/`
2. Implement `agent.py` with proper LlmAgent configuration
3. Add `__init__.py` to expose the agent
4. Register agent in root manager agent
5. Update agent instructions and descriptions

### Code Guidelines

- Follow Python PEP 8 standards
- Agent names must follow Python identifier rules (no spaces/special characters)
- Avoid default parameters in function schemas for Google AI compatibility
- Use relative imports within agent modules
- Implement proper error handling and logging

### Testing

```bash
# Test individual components
python -c "from main_agent.sub_agents.tour_planner.sub_agents.hotel_finder import hotel_finder; print('Agent loaded successfully')"

# Test hotel finder functionality
python -c "from main_agent.sub_agents.tour_planner.sub_agents.hotel_finder.tools import find_hotel; print(find_hotel('PAR', '2025-09-20', '2025-09-21', 2))"
```

## 🔧 Troubleshooting

### Common Issues

1. **Import Errors**: Ensure all `__init__.py` files properly expose required symbols
2. **API Key Issues**: Verify `.env` file configuration and API key validity
3. **Database Errors**: Check SQLite database permissions and file access
4. **Agent Configuration**: Validate agent names follow Python identifier rules

### Debug Mode

Enable debug logging in `main.py`:
```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

### Environment Issues

- Ensure virtual environment is activated
- Verify all dependencies are installed
- Check Python version compatibility (3.8+)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Setup

1. Follow installation instructions
2. Install development dependencies
3. Run tests before submitting PRs
4. Follow code style guidelines
5. Update documentation for new features

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🙏 Acknowledgments

- Google AI for Gemini API
- Amadeus for travel API services
- FastAPI framework
- Telegram Bot API
- Google ADK for agent orchestration

---

**Built with ❤️ using Google AI and modern Python frameworks**
