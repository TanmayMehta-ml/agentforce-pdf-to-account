# Agentforce PDF to Account — Field Extraction Agent

A Salesforce Agentforce-powered application that reads PDF attachments on Account records, extracts structured business information using Prompt Builder, and updates Account fields upon explicit user confirmation.

Built on Salesforce Developer Edition using Agentforce, Prompt Builder, and Salesforce Flow — no Apex required.

---

## What Problem Does This Solve

In many business workflows, important Account information arrives as PDF documents — contracts, onboarding forms, company profiles. Manually reading these and updating Salesforce fields is time-consuming and error-prone. This agent automates the extraction while keeping a human in control of what actually gets saved.

---

## Architecture

1. User triggers the Agent with an Account ID
2. **Agentforce Agent** (Account Document Assistant) receives the request
3. **Flow 1** — `Extract_Account_Fields_from_Document` fetches the Account record and its PDF attachment, then calls the Prompt Template with account data and PDF content as input nodes
4. **Prompt Template** — `Extract_Information` (Prompt Builder) reads the PDF content alongside existing Account fields and returns a structured JSON with extracted field values
5. Agent presents the JSON output to the user for review — the user can ask the agent to modify any extracted values at this stage
6. User explicitly confirms — *"Update the account fields"*
7. **Flow 2** — `Update_Account_Record_Fields` maps the confirmed JSON output to Account object fields and performs the DML update on the Account record

---

## Key Design Decisions

**Human-in-the-loop confirmation before any DML**
The agent never automatically writes to the Account record. The user must explicitly confirm before Flow 2 runs. This is an intentional enterprise-grade design choice — in production systems handling critical Account data, automatic overwrites without review are a significant data integrity risk.

**Prompt Builder with structured JSON output**
Rather than returning free-text summaries, the Prompt Template is instructed to return a consistent JSON format. This makes field mapping in Flow 2 deterministic and reliable regardless of how different PDFs are structured or worded.

**Flow for orchestration over Apex**
The orchestration layer — fetching records, calling the prompt, mapping outputs — is entirely Flow-based. This keeps the solution declarative, easier to maintain, and accessible to admins without Apex knowledge.

**Modifiable output before confirmation**
Between extraction and confirmation, the user can ask the agent to correct any field value. This handles cases where the AI extraction confidence is low or the PDF data is ambiguous.

---

## Components

| Component | Type | Purpose |
|---|---|---|
| Account_Document_Assistant | Agentforce Agent | Main agent that orchestrates the conversation |
| Extract_Information | Prompt Template (Prompt Builder) | Reads PDF and Account data, returns structured JSON |
| Extract_Account_Fields_from_Document | Flow | Fetches Account and attachment, calls Prompt Template |
| Update_Account_Record_Fields | Flow | Maps JSON output to Account fields and updates record |

---

## Tech Stack

- Salesforce Agentforce
- Prompt Builder (GenAI Prompt Templates)
- Salesforce Flow (Autolaunched)
- Salesforce Account Object
- Salesforce Files / ContentDocument for PDF handling

---

## How to Deploy This to Your Own Org

1. Clone this repository
```
git clone https://github.com/TanmayMehta-ml/agentforce-pdf-to-account.git
```

2. Authorise a Salesforce org with Agentforce enabled
```
sf org login web --alias agentforce-org
```

3. Deploy the metadata
```
sf project deploy start --manifest manifest/package.xml --target-org agentforce-org
```

4. Assign the Agentforce permission set to your user in the target org

5. Attach a PDF to any Account record

6. Open the Account Document Assistant agent and provide the Account ID

---
## Demo
### Before — Account with PDF attached, fields empty
<img width="1919" height="745" alt="image" src="https://github.com/user-attachments/assets/4ccf9b21-c838-4296-9a29-60ea451ece57" />
<img width="1919" height="858" alt="image" src="https://github.com/user-attachments/assets/eb708036-2ce2-4bd4-b0d7-32630423a865" />

<img width="1919" height="731" alt="image" src="https://github.com/user-attachments/assets/3f341930-4e74-4d9d-983f-0f2bab7861b6" />
<img width="1919" height="787" alt="image" src="https://github.com/user-attachments/assets/09c869aa-25e1-4a25-a613-bbd04dae9233" />

### Agent extracts data from PDF
<img width="1919" height="812" alt="image" src="https://github.com/user-attachments/assets/10a921ef-df3e-47f5-8b06-fec9643880a2" />
<img width="1919" height="757" alt="image" src="https://github.com/user-attachments/assets/ee9bb71b-9e68-48b1-9123-de914f34c8b6" />

### User confirms update
<img width="1919" height="804" alt="image" src="https://github.com/user-attachments/assets/40ed5f01-d6e9-43f4-a3cf-9092b7de03f5" />

### After reloading the page — Account fields populated automatically
<img width="1903" height="738" alt="image" src="https://github.com/user-attachments/assets/1317e86f-739e-4fa8-a57f-8ee7ec88a7e6" />


---
## Author

**Tanmay Mehta**

Salesforce Developer | 5x Certified |
[LinkedIn](https://linkedin.com/in/tanmay-here) · [Trailblazer](https://www.salesforce.com/trailblazer/tanmaymehta)
