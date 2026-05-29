### Customer Service Agent
**Built on Microsoft Copilot Studio 

---

## 📋 What is CSA Copilot?

CSA Copilot is a customer service agent built for support teams using Microsoft Copilot Studio. When a customer reaches out via chat, email, or Microsoft Teams, CSA Copilot automatically:

- **Detects** the customer query and classifies intent
- **Creates** a support ticket record in Microsoft Dataverse
- **Retrieves** relevant answers from FAQs, policies, and product documentation
- **Routes** the query to the correct resolution path (Knowledge / Tool / Escalation)
- **Executes actions** like order tracking, refund processing, or ticket updates
- **Escalates** complex or unresolved issues to a human agent with full context
- **Notifies** the support team in Microsoft Teams with ticket updates
- **Makes** all tickets and resolutions queryable in natural language

> **From customer query to resolution in under 2 minutes. Minimal manual effort.**

---

## 🎯 The Problem It Solves

Customer support teams receive queries across multiple channels and currently handle them manually — reading messages, classifying issues, searching knowledge bases, updating systems, and notifying stakeholders.

This takes **15–30 minutes per ticket** and frequently leads to:

- ❌ Slow response times
- ❌ Inconsistent answers across agents
- ❌ Missed follow-ups and dropped tickets
- ❌ Customer dissatisfaction and churn

**CSA Copilot eliminates this entirely.**

### Target Users

| User Type | Description |
|---|---|
| **Customers / End Users** | People seeking help with orders, billing, technical issues, or general inquiries |
| **Support Teams** | Customer support agents and helpdesk staff who handle incoming requests |
| **Support Managers** | Team leads who monitor ticket volume, resolution rates, and escalations |

---

## 🏗️ Architecture

### Orchestrator-Based Multi-Agent System

```
Customer Query (Chat / Email / Teams)
              ↓
     Power Automate Trigger
       • Detects new query / email
       • Creates Dataverse ticket record
       • Classifies intent via AI prompt
       • Posts Teams notification
       • Sends prompt to Orchestrator
              ↓
     CSA Copilot Orchestrator (Copilot Studio)
              ↓
  ┌─────────────────────────────────────────────────────┐
  │  Intent        │  Knowledge     │  Action &         │
  │  Classifier    │  Retrieval     │  Resolution       │
  │  Agent         │  Agent + MCP   │  Agent + MCP      │
  │                │                │                   │
  │  Escalation    │  Ticket        │                   │
  │  Agent         │  Q&A Agent     │                   │
  └─────────────────────────────────────────────────────┘
              ↓
     Microsoft Dataverse (4 custom tables)
       • csa_ticket
       • csa_customer
       • csa_knowledgebase
       • csa_resolution
```

### Key Components

| Component | Technology |

| Data layer | Microsoft Dataverse |
| Automation | Power Automate |
| Notifications | Microsoft Teams |
| Intelligent data operations | Microsoft Dataverse MCP Server |
| Intent classification | AI Builder Prompts (GPT-4.1 mini) |
| Knowledge source | SharePoint / Dataverse |

---

## 🤖 The Agents

### 1. CSA Copilot
Central coordinator. Receives all inputs — user chat, email trigger, and Teams messages. Delegates to specialist agents using generative orchestration.

### 2. Intent Classifier
- Analyzes customer query in real time
- Classifies into categories: **Billing, Orders, Technical, General, Complaint**
- Routes to the correct specialist agent or resolution path

### 3. Knowledge
- Retrieves answers from official microsoft documentation
- Uses knowledge grounding (RAG) for accurate, citation-backed responses
- Reads from Dataverse knowledge base using **MCP Server**

### 4. Action & Resolution
- Executes real-world actions:
  - 🎫 Create / update support tickets
  - 📧 Send confirmation emails
- Connects to external systems via Power Automate agent flows
- Saves resolution records using **Microsoft Power Automate**

### 5. Escalation
- Detects unresolved, complex, or sensitive issues
- Posts escalation notification in Microsoft Teams channel

### 6. Ticket
- Answers natural language queries about any ticket, resolution, or customer history stored in Dataverse
- Enables support managers to query data conversationally

---

## 📊 Intent Categories

CSA Copilot classifies customer queries across **4 categories**:

| Category | Examples |
|---|---|
| 💳 **Billing** | Refund request, payment issue, invoice query, pricing info |
| 🔧 **Technical** | App not working, login issue, error messages, bug report |
| 📝 **General** | Product info, store hours, return policy, FAQs |
| ⚠️ **Complaints** | Bad experience, escalation request, feedback, service issue |

---

## 🔄 Agent Academy Patterns Applied

| Pattern | How Applied |
|---|---|
| **Knowledge Grounding (RAG)** | FAQs, policies, and product docs integrated as knowledge sources for accurate responses |
| **Tool / Action Pattern** | Power Automate flows for order tracking, refund processing, and ticket management |
| **Orchestration** | Central orchestrator routes queries to the correct agent based on intent |
| **ReAct (Reason + Act)** | Agent reasons about the query, fetches data, validates, and responds step-by-step |
