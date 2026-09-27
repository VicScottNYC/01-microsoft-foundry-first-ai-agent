# Microsoft Foundry Computing History AI Agent

A web-based AI application built with **Microsoft Foundry**, **Python**, **Flask**, and the **OpenAI Responses API**. The application provides a conversational interface for interacting with a Microsoft Foundry agent focused on computing history.

This project was developed as part of my hands-on Microsoft Azure AI and agent-development training and demonstrates how a published Microsoft Foundry agent can be integrated into a custom Python web application using Azure identity-based authentication.

## Project Overview

The Computing History AI Agent allows users to interact with an AI agent through a browser-based chat interface.

Rather than communicating with the agent only through the Microsoft Foundry portal, this project implements a custom Flask application that connects to the published agent programmatically.

The application demonstrates several core AI application-development concepts:

- Microsoft Foundry agent integration
- OpenAI Responses API usage
- Azure identity-based authentication
- Python application development
- Flask API development
- Conversation context management
- Secure Markdown rendering
- Input validation
- Environment-based configuration
- Separation of application logic and AI client logic

## Architecture

```text
┌──────────────────────────────┐
│          User Browser        │
│                              │
│       Chat Web Interface     │
└──────────────┬───────────────┘
               │
               │ HTTP
               ▼
┌──────────────────────────────┐
│      Flask Application       │
│          app.py              │
│                              │
│  • Chat endpoint             │
│  • Input validation          │
│  • Markdown rendering        │
│  • HTML sanitization         │
│  • Conversation reset        │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│        Agent Client          │
│      agent_client.py         │
│                              │
│  • Azure authentication      │
│  • Conversation history      │
│  • Responses API client      │
└──────────────┬───────────────┘
               │
               │ OpenAI Responses API
               ▼
┌──────────────────────────────┐
│      Microsoft Foundry       │
│                              │
│    Computing History Agent   │
└──────────────────────────────┘
```

## Technology Stack

| Technology | Purpose |
|---|---|
| Microsoft Foundry | AI agent hosting and management |
| Python | Application and agent-client development |
| Flask | Web application and REST endpoints |
| OpenAI Python SDK | Responses API integration |
| Azure Identity | Passwordless Azure authentication |
| DefaultAzureCredential | Azure credential discovery |
| python-dotenv | Local environment configuration |
| Markdown | Converts agent Markdown responses to HTML |
| Bleach | Sanitizes generated HTML before browser rendering |
| HTML/CSS/JavaScript | Browser-based chat interface |

## Key Features

### Microsoft Foundry Agent Integration

The application communicates with a published Microsoft Foundry agent through the OpenAI Responses API.

The Foundry endpoint is stored outside the source code using an environment variable:

```text
AGENT_ENDPOINT
```

Sensitive or environment-specific configuration is therefore not hard-coded into the application.

### Passwordless Azure Authentication

The agent client uses:

```python
DefaultAzureCredential()
```

together with an Azure bearer-token provider.

This allows the application to authenticate using supported Azure credentials without embedding API keys directly in the source code.

### Conversation Context Management

The application maintains recent conversation history so the agent can respond with conversational context.

The client retains up to three recent user exchanges and removes older exchanges as the conversation grows.

### Flask API

The application provides endpoints for:

```text
GET  /
POST /chat
POST /reset
```

`/chat` validates the incoming message, sends it to the Foundry agent, processes the response, and returns both the original response and safely rendered HTML.

`/reset` clears the current conversation history.

### Input Validation

User messages are validated before being submitted to the agent.

The application:

- Rejects empty messages
- Limits messages to 10,000 characters
- Returns appropriate HTTP errors for invalid requests

### Secure AI Response Rendering

Agent responses may contain Markdown.

The application converts Markdown into HTML and then sanitizes the generated HTML using **Bleach** before returning it to the browser.

Only approved HTML tags, attributes, and URL protocols are allowed.

External links are configured with:

```text
noopener
noreferrer
nofollow
```

to improve browser-side security when opening external resources.

## Project Structure

```text
01-microsoft-foundry-first-ai-agent/
│
├── agent_client.py
├── app.py
├── feature-spec.md
├── requirements.txt
├── .env.example
├── .gitignore
│
├── templates/
│   └── ...
│
└── static/
    └── ...
```

### `agent_client.py`

Handles communication with the Microsoft Foundry agent, Azure authentication, Responses API requests, and conversation history.

### `app.py`

Provides the Flask application, HTTP routes, request validation, Markdown processing, response sanitization, and conversation-reset functionality.

### `feature-spec.md`

Defines requirements and acceptance criteria for a planned multimodal enhancement that would allow users to submit computing-related images alongside text prompts.

### `templates/`

Contains the Flask HTML templates used by the web interface.

### `static/`

Contains frontend static resources used by the application.

## Prerequisites

Before running the project, you will need:

- Python 3
- An Azure account
- Access to Microsoft Foundry
- A published Microsoft Foundry agent
- Azure credentials with appropriate access to the Foundry resource

Azure CLI authentication can be used locally with:

```bash
az login
```

`DefaultAzureCredential` can then obtain an Azure credential from the local development environment.

## Installation

Clone the repository:

```bash
git clone <your-repository-url>
```

Move into the project directory:

```bash
cd 01-microsoft-foundry-first-ai-agent
```

Create a Python virtual environment:

```bash
python3 -m venv .venv
```

Activate it on macOS/Linux:

```bash
source .venv/bin/activate
```

Install the required packages:

```bash
python -m pip install -r requirements.txt
```

## Environment Configuration

Copy the provided example configuration:

```bash
cp .env.example .env
```

Edit `.env` and provide your Microsoft Foundry agent endpoint:

```dotenv
AGENT_ENDPOINT=https://your-foundry-agent-endpoint-here
```

The real `.env` file is excluded from Git through `.gitignore` and should **never be committed to source control**.

## Running the Application

Make sure your Azure authentication is available and then start the Flask application:

```bash
python app.py
```

The application runs locally on:

```text
http://127.0.0.1:5000
```

Open the address in a browser to access the Computing History Agent interface.

## Security Considerations

Several security practices are incorporated into the project:

- Environment variables are used for environment-specific configuration.
- `.env` is excluded from source control.
- Azure identity-based authentication avoids storing an API key in the application.
- User input is validated before agent submission.
- Agent-generated Markdown is sanitized before being rendered as HTML.
- External links receive safer browser attributes.
- Error details are logged server-side rather than returned directly to users.

## Planned Enhancement: Multimodal Image Prompts

The repository includes a feature specification for extending the application with image-based prompts.

The planned enhancement includes:

- PNG and JPEG image support
- Maximum 5 MB image size
- Image preview and removal
- Text-plus-image prompts
- File type and size validation
- Accessible image controls
- Integration through the existing agent request path

This feature is documented in `feature-spec.md` and is not represented here as a completed feature.

## Skills Demonstrated

This project demonstrates practical experience with:

- Microsoft Foundry
- AI agent application development
- OpenAI Responses API
- Azure Identity
- `DefaultAzureCredential`
- Python
- Flask
- REST API development
- Conversation state management
- Environment configuration
- Secure AI output rendering
- Input validation
- Git/GitHub project organization

## Learning Context

This project originated from hands-on exercises in Microsoft's **Develop your first agent with Microsoft Foundry** learning material.

The repository is organized as a portfolio implementation demonstrating the concepts practiced during the module, including connecting a custom Python application to a Microsoft Foundry agent.

Microsoft Learn materials and Microsoft product names remain the property of their respective owners.

## Future Improvements

Potential extensions include:

- Implementing the multimodal image-prompt specification
- Streaming agent responses
- Persistent conversation storage
- Improved application telemetry and logging
- Automated testing
- Azure-hosted deployment
- CI/CD with GitHub Actions
- Application Insights integration
- Agent evaluation and monitoring

## Author

**Vic**

Azure / IT professional developing hands-on expertise in Microsoft Foundry, Azure AI applications, and AI agent engineering.