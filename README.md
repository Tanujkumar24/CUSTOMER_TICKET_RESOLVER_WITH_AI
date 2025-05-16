
# Customer Ticket Resolver with AI

![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=flat&logo=streamlit&logoColor=white)
![Python](https://img.shields.io/badge/Python-3670A0?style=flat&logo=python&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=flat&logo=openai&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

Customer support can be overwhelming and time-consuming, especially when the ticket volume is high. This project — **Customer Ticket Resolver with AI** — leverages the power of AI and natural language processing (NLP) to streamline and automate the customer service process. It intelligently classifies support tickets, analyzes their content, and generates possible resolutions or summaries using advanced language models.

The system helps support teams become faster, more accurate, and more consistent in handling user queries by using Large Language Models (LLMs) such as OpenAI GPT APIs. The interface is built using **Streamlit** to ensure simplicity and accessibility for non-technical users.

---

## 🧠 About MCP (Multi-Class Predictor)

The **MCP** module (Multi-Class Predictor) is the core intelligence engine of the project. It takes raw ticket input (customer issue text) and performs:

1. **Intent Classification** – Understands the context or issue type.
2. **Urgency Detection** – Predicts how urgent the ticket is.
3. **Resolution Suggestion** – Predicts the category and offers relevant resolution paths.
4. **LLM Response Generation** – Uses GPT to summarize the issue or draft a response.

> The MCP runs as a backend model server (`mcp_server.py`) that takes ticket inputs and returns structured insights.

---
## 📊 Workflow Overview

```mermaid
flowchart TD
    %% Presentation Layer
    subgraph "Presentation Layer"
        direction TB
        UA["User Agent (Web Browser)"]:::external
        Streamlit["Streamlit UI\n(main.py)"]:::frontend
    end

    %% Application Layer
    subgraph "Application Layer"
        direction TB
        MCP["MCP Model Server\n(mcp_server.py)"]:::backend
        subgraph "MCP Internal"
            direction TB
            Classifier["Intent/Urgency Classifier\n(tools/classify_ticket.py)"]:::backend
            ReplyGen["LLM Response Generator\n(tools/generate_reply.py)"]:::backend
        end
    end

    %% Integration Layer
    subgraph "Integration Layer"
        direction TB
        OpenAIAPI["OpenAI GPT API"]:::external
        GoogleSheetsAPI["Google Sheets API"]:::external
        SMTPServer["SMTP Server"]:::external
        Creds["Google API Credentials\n(google_creds.json)"]:::external
    end

    %% Auxiliary Modules
    subgraph "Auxiliary / Test Modules"
        direction TB
        RegisterT["Ticket Registration Utility\n(register_ticket.py)"]:::external
        TestE2E["E2E/Test Script\n(test_euri.py)"]:::external
    end

    %% Data Flows
    UA -->|"HTTP POST/GET ticket_text"| Streamlit
    Streamlit -->|"gRPC/REST raw_text"| MCP
    MCP -->|"function call"| Classifier
    MCP -->|"invoke"| ReplyGen
    ReplyGen -->|"REST reply_text"| OpenAIAPI
    OpenAIAPI -->|"reply_text"| ReplyGen
    ReplyGen -->|"response"| MCP
    MCP -->|"response"| Streamlit

    %% Optional Flows
    MCP -.->|"async log_data"| SheetConnector["Google Sheets Logger\n(tools/sheet_connector.py)"]:::backend
    SheetConnector -->|"REST log_request"| GoogleSheetsAPI
    Streamlit -.->|"async email_data"| GmailSender["Email Sender\n(tools/gmail_sender.py)"]:::backend
    GmailSender -->|"SMTP send"| SMTPServer

    %% Configuration Link
    Creds --> GoogleSheetsAPI

    %% Styles
    classDef frontend fill:#D0E8FF,stroke:#0066CC,color:#003366;
    classDef backend  fill:#E6F5D0,stroke:#5C8A00,color:#3A5F00;
    classDef external fill:#F0F0F0,stroke:#CCCCCC,color:#333333;
    class Streamlit,UA external
    class MCP,Classifier,ReplyGen,SheetConnector,GmailSender backend
    class OpenAIAPI,GoogleSheetsAPI,SMTPServer,Creds external


## 🚀 Key Features

- **AI/NLP Ticket Analysis:** Automatically extract intent, urgency, and suggest next steps.
- **MCP (Multi-Class Predictor):** Lightweight, extensible, AI-powered module.
- **LLM Integration:** Uses GPT to generate responses, explanations, or ticket summaries.
- **Streamlit UI:** Clean, responsive web-based interface.
- **Google API Integration:** Log tickets into Google Sheets or Docs if credentials are provided.
- **Modular Python Codebase:** Designed for reusability and easy integration.

---

## 📊 Workflow Overview

```
graph TD
    A[Customer Ticket Input] --> B[Streamlit Form]
    B --> C[MCP Server - NLP & Classification]
    C --> D[LLM Response Generator]
    D --> E[Response Output & Display]
    C --> F[Google Sheet Logger (optional)]
```

### Explanation:
1. The user enters a ticket description in the Streamlit UI.
2. This input is sent to the MCP model server for NLP-based classification.
3. Based on classification, the LLM generates a summary or resolution.
4. The results are presented to the user.
5. Optionally, the ticket is logged using the Google API into a Google Sheet.

---

## 🛠️ Installation

```bash
git clone https://github.com/Tanujkumar24/CUSTOMER_TICKET_RESOLVER_WITH_AI.git
cd CUSTOMER_TICKET_RESOLVER_WITH_AI

# (Optional) Create virtual environment
python -m venv env
source env/bin/activate  # Windows: env\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

## 🔐 API Keys

- Create a `google_creds.json` file for Google Sheets access.
- Add your OpenAI API keys in a `.env` file or as environment variables.

---

## ▶️ Run the App

```bash
streamlit run main.py
```

This opens the app in your browser at `http://localhost:8501`.

---

## 📷 Screenshots

> Replace the placeholder paths below with actual screenshot images from your project

![Ticket Form](images/screenshot_form.png)
*Customer Ticket Submission Interface*

![Analysis Result](images/screenshot_analysis.png)
*AI-generated summary and suggested resolution*

---

## 🌐 Deployment

Deploy the app to:
- [Streamlit Cloud](https://streamlit.io/cloud)
- Heroku
- AWS EC2 / GCP Compute Engine
- Dockerized containers on Kubernetes

---

## 🤝 Contributing

Pull requests are welcome! For major changes, please open an issue first to discuss your ideas.

---

## 📄 License

This project is licensed under the MIT License.
