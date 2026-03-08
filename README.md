<<<<<<< HEAD
# Salesforce DX Project: Next Steps

Now that you’ve created a Salesforce DX project, what’s next? Here are some documentation resources to get you started.

## How Do You Plan to Deploy Your Changes?

Do you want to deploy a set of changes, or create a self-contained application? Choose a [development model](https://developer.salesforce.com/tools/vscode/en/user-guide/development-models).

## Configure Your Salesforce DX Project

The `sfdx-project.json` file contains useful configuration information for your project. See [Salesforce DX Project Configuration](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_ws_config.htm) in the _Salesforce DX Developer Guide_ for details about this file.

## Read All About It

- [Salesforce Extensions Documentation](https://developer.salesforce.com/tools/vscode/)
- [Salesforce CLI Setup Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_intro.htm)
- [Salesforce DX Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_intro.htm)
- [Salesforce CLI Command Reference](https://developer.salesforce.com/docs/atlas.en-us.sfdx_cli_reference.meta/sfdx_cli_reference/cli_reference.htm)
=======
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
>>>>>>> f39e0a9b462f874901a12461916477d466b244b4
