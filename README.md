# Agentforce PDF to Account — Field Extraction Agent

## What This Does
An Agentforce-powered Salesforce application that reads PDF attachments 
on Account records, extracts structured business information using 
Prompt Builder, and updates Account fields upon explicit user confirmation.

## Architecture
PDF Attachment on Account
        ↓
Agentforce Agent (triggered with Account ID)
        ↓
Flow 1 — fetches Account record and PDF attachment
        ↓
Prompt Template (Prompt Builder)
Extracts fields from PDF, returns structured JSON
        ↓
Agent presents JSON output to user for review
User can modify any extracted fields
        ↓
User explicitly confirms — "Update the account fields"
        ↓
Flow 2 — maps JSON to Account fields and updates record

## Key Design Decisions

**Human-in-the-loop confirmation** — The agent never automatically 
writes to the Account record. The user must explicitly confirm before 
any DML operation runs. This is an intentional design choice for 
data integrity in enterprise contexts.

**Prompt Builder for extraction** — Used Prompt Builder with a 
structured JSON output format to ensure consistent field mapping 
regardless of PDF layout variation.

**Flow for orchestration** — Chose Flow over Apex for the 
orchestration layer to keep the solution declarative and 
maintainable by admins.

## Components
- Prompt Template — extracts and structures PDF data as JSON
- Flow 1 — retrieves Account and attachment, calls Prompt Template
- Flow 2 — maps confirmed JSON output to Account fields

## Tech Stack
- Salesforce Agentforce
- Prompt Builder
- Salesforce Flow
- Salesforce Account Object

## Setup Instructions
1. Clone this repo
2. Authorise a Salesforce org with Agentforce enabled
3. Run `sf project deploy start --manifest manifest/package.xml`
4. Attach a PDF to any Account record and invoke the agent
