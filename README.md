# Husky Track
### AI-Powered Course Navigator & Academic Advisor

[![React](https://img.shields.io/badge/React-20232A?style=flat&logo=react&logoColor=61DAFB)](https://reactjs.org/)
[![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![Claude](https://img.shields.io/badge/Claude_3-D97757?style=flat&logo=anthropic&logoColor=white)](https://www.anthropic.com/)
[![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/)

Husky Track is an AI-powered academic advisor for University of Washington students. It consolidates course catalog data and professor reviews into a conversational interface, enabling students to make informed decisions about course selection and degree planning.

---

## Motivation

UW students must navigate hundreds of courses, complex prerequisite chains, and information scattered across multiple platforms. Husky Track centralizes this data and supports natural language queries such as:

> *"What CSE course do you recommend based on my recent grades?"*  
> *"When should I take this class to get the highest-rated professor?"*

---

## Security Disclosure

> **Proof of Concept Only**

This repository is intended for design demonstration and architectural reference.

- All sensitive credentials (AWS Access Keys, Bedrock/Anthropic keys) have been removed
- The application will not run locally without valid credentials
- No production secrets are exposed

---

## System Architecture
```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'background': '#ffffff', 'primaryColor': '#ffffff', 'edgeLabelBackground':'#ffffff', 'tertiaryColor': '#ffffff', 'clusterBkg': '#fafafa', 'clusterBorder': '#e5e7eb', 'lineColor': '#4B0082', 'fontSize': '14px'}}}%%
graph LR
    subgraph Data_Prep ["Offline Data Pipeline"]
        Sources[("UW Catalog & <br/>RateMyProf")]
        Scraper["Python Scraper"]
        Context["JSON Context <br/>Dataset"]
    end

    subgraph Client_Side ["Client Side"]
        User["Student / React App"]
    end

    subgraph AWS_Cloud ["AWS Cloud Infrastructure"]
        Cognito["AWS Cognito<br/>(Auth)"]
        Lambda["AWS Lambda<br/>(Orchestrator)"]
        Bedrock["AWS Bedrock<br/>(Claude Model)"]
    end

    Sources -- "1. Scrape" --> Scraper
    Scraper -- "2. Clean & Format" --> Context
    Context -.-> Lambda

    User -- "3. Auth" --> Cognito
    User -- "4. Query" --> Lambda

    Lambda -- "5. Prompt + Context" --> Bedrock
    Bedrock -- "6. Response" --> Lambda
    Lambda --> User
```

---

## Architecture

**Offline Data Ingestion** — A custom Python scraper aggregates data from the UW Course Catalog and 1,500+ Rate My Professor reviews, normalizing the output into a structured JSON context file.

**Context Injection** — The full curated dataset is injected directly into the model context at request time, enabling complete curriculum reasoning without a vector database or retrieval pipeline.

**Serverless Backend** — AWS Lambda handles authentication checks, prompt construction, and secure communication with AWS Bedrock.

---

## Tech Stack

| Component        | Technology              | Purpose                          |
|------------------|--------------------------|----------------------------------|
| Frontend         | React, JavaScript        | Student chat interface           |
| Authentication   | AWS Cognito              | Secure user identity             |
| AI Model         | AWS Bedrock (Claude)     | Reasoning and recommendations    |
| Compute          | AWS Lambda               | Serverless request handling      |
| Data Engineering | Python                   | Web scraping and normalization   |

---

## Features

**Course Recommendations** — Filters by prerequisites, difficulty, and professor ratings to suggest appropriate course options.

**Natural Language Interface** — Conversational query support with no manual filtering required.

**Integrated Professor Ratings** — Combines official course descriptions with aggregated student sentiment for holistic recommendations.

---

## Interface Preview

<div>
  <img width="150" height="435" alt="Mobile View" src="https://github.com/user-attachments/assets/31936ec1-961e-4a14-87ad-bfe1ca5f5f21" />
  <img width="350" height="301" alt="Dashboard" src="https://github.com/user-attachments/assets/1bf68a33-8ea0-4926-9d19-3a60acf07741" />
  <img width="350" height="305" alt="Chat Interface" src="https://github.com/user-attachments/assets/a137896a-d1e5-463a-a28f-87decf029bfd" />
</div>
