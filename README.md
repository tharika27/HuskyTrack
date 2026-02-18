# 🐾 Husky Track  
### *AI-Powered Course Navigator & Academic Advisor*

[![React](https://img.shields.io/badge/React-20232A?style=flat&logo=react&logoColor=61DAFB)](https://reactjs.org/)
[![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![Claude](https://img.shields.io/badge/Claude_3-D97757?style=flat&logo=anthropic&logoColor=white)](https://www.anthropic.com/)
[![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/)

**Husky Track** is an intelligent academic advisor designed to simplify course planning for **University of Washington students**.  
It transforms complex curriculum data into a **conversational, AI-driven experience**, helping students make informed decisions about classes, professors, and degree paths.

---

## 🎯 Motivation

UW students navigate:
- Hundreds of courses  
- Complex prerequisite chains  
- Fragmented information across catalogs and review sites  

Husky Track unifies this data and lets students ask **natural language questions** like:

> *“What cse course do you recommend based on my recent grades”*  
> *“When should I take this class to get the highest rated professor”*

---

## ⚠️ Security Disclosure

> **Proof of Concept Only**

This repository is intended for **design demonstration and architectural reference**.

- All sensitive credentials (AWS Access Keys, Bedrock / Anthropic keys) have been **removed**
- The application **will not run locally** without valid credentials
- No production secrets are exposed

---

## 🏗️ System Architecture

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

## 🧠 Architecture Highlights

### 📦 Offline Data Ingestion
- Custom Python scraper aggregates:
  - UW Course Catalog
  - 1,500+ Rate My Professor reviews
- Data normalized into a structured **JSON context file**

### 🧩 Context Injection (No Vector DB)
- Entire curated dataset is injected directly into the model context
- Enables full-curriculum reasoning without retrieval pipelines

### ☁️ Serverless Backend
- AWS Lambda orchestrates:
  - Authentication checks
  - Prompt construction
  - Secure communication with AWS Bedrock

---

## 🛠️ Tech Stack

| Component        | Technology              | Purpose |
|------------------|--------------------------|---------|
| Frontend         | React, JavaScript        | Student chat interface |
| Authentication  | AWS Cognito              | Secure user identity |
| AI Model         | AWS Bedrock (Claude)     | Reasoning & recommendations |
| Compute          | AWS Lambda               | Serverless request handling |
| Data Engineering | Python                   | Web scraping & normalization |

---

## 🚀 Key Features

### 🎓 Smart Course Recommendations
- Filters by prerequisites, difficulty, and professor ratings
- Suggests realistic course options

### 💬 Natural Language Search
- Conversational interface
- No manual filtering required

### ⭐ Integrated Professor Ratings
- Merges official descriptions with student sentiment
- Produces holistic recommendations

---

## 📱 Interface Preview

<div>
  <img width="150" height="435" alt="Mobile View" src="https://github.com/user-attachments/assets/31936ec1-961e-4a14-87ad-bfe1ca5f5f21" />
  <img width="350" height="301" alt="Dashboard" src="https://github.com/user-attachments/assets/1bf68a33-8ea0-4926-9d19-3a60acf07741" />
  <img width="350" height="305" alt="Chat Interface" src="https://github.com/user-attachments/assets/a137896a-d1e5-463a-a28f-87decf029bfd" />
</div>
