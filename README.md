# AI Lead Generation & Company Intelligence Platform

An AI-powered n8n workflow that researches marketing agencies in Bengaluru, enriches company information, qualifies leads, assigns lead scores, stores results in Google Sheets, and generates a final intelligence report.

## Business Problem

Sales teams and marketing agencies often spend hours manually searching for companies, visiting websites, collecting contact details, and deciding which leads are worth contacting.

This workflow automates that research process and produces structured, ranked lead records.

## What the Workflow Does

1. Accepts a niche, location, and maximum company count through an n8n form.
2. Searches for relevant companies using Tavily.
3. Splits search results into individual company records.
4. Normalizes website URLs and domains.
5. Removes directories, platforms, and duplicate domains.
6. Fetches public company websites.
7. Extracts and cleans website text.
8. Uses ScrapeGraphAI as a fallback for blocked or low-quality websites.
9. Uses OpenRouter AI to qualify each lead.
10. Assigns a lead score from 0 to 100.
11. Classifies leads as High, Medium, or Low.
12. Stores results in Google Sheets.
13. Generates a Markdown intelligence report.
14. Logs workflow failures in a separate Google Sheet.

## Architecture

![Architecture Diagram](documentation/Screenshot%202026-09-22%20161339.png)

## Workflow Diagram

![Workflow Diagram](documentation/Screenshot%202026-09-22%20225019.png)

## Lead Scoring

| Score | Level | Status |
|---:|---|---|
| 80–100 | High | Qualified |
| 60–79 | Medium | Qualified |
| 0–59 | Low | Not Qualified |

The score is calculated using:

- Industry fit: 25 points
- Location fit: 20 points
- Service fit: 25 points
- Business evidence: 20 points
- Contactability: 10 points

The minimum qualification score is 60.

## Technology Stack

- n8n self-hosted with Docker
- n8n Form Trigger
- Tavily Search
- ScrapeGraphAI
- OpenRouter
- JavaScript Code nodes
- Google Sheets
- Markdown report generation

## Repository Structure

```text
workflow/
  ai-lead-generation-platform.json
  ai-lead-platform-error-handler.json

documentation/
  architecture-diagram.png
  workflow-diagram.png
  wireframe.png

screenshots/
  form-input.png
  tavily-results.png
  ai-scoring.png
  google-sheets-crm.png
  final-report.png
  error-handler.png

examples/
  sample-lead-output.json
  sample-intelligence-report.md
Important Limitations
- Results depend on publicly available website information.
- Some websites block automated requests.
- Search results may include incomplete company information.
- Free API limits and model availability can change.
- Google Sheets is suitable for a portfolio and small workflow, but PostgreSQL would be better for large-scale production.
- The workflow does not automatically contact companies.
- Human review is recommended before sales outreach.
Setup
1. Install n8n using Docker.
2. Import the workflow JSON from the workflow folder.
3. Add Tavily credentials.
4. Add OpenRouter credentials.
5. Add ScrapeGraphAI credentials.
6. Connect Google Sheets.
7. Configure the Google Sheets document and worksheet.
8. Publish the workflow.
9. Submit the form using the Production URL.
Example Output
See:
- [Sample Lead Output](examples/sample-lead-output.json)
- [Sample Intelligence Report](examples/sample-intelligence-report.md)
Error Handling
The workflow uses a separate Error Trigger workflow.
When the main workflow fails:
Main workflow error
→ Error Trigger
→ Error formatting
→ Error_Logs Google Sheet
