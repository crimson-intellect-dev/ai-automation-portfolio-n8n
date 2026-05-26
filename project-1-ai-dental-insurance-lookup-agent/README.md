# Project 1 — AI Dental Insurance Lookup Agent with QA Regression Testing

## Overview

This project is an AI-powered dental insurance lookup workflow built in **n8n**. It allows a user to ask whether a dental insurance provider is accepted, checks a **Google Sheets insurance knowledge base**, and returns a clean patient-friendly response.

The project also includes QA documentation, regression testing, defect tracking, smoke testing, and final test evidence.

---

## Business Problem

Dental offices often receive repeated insurance questions from patients, such as:

- Do you accept Delta Dental?
- Do you take Medi-Cal?
- Do you accept Cigna?
- Do you accept UHC?

Staff may need to manually check whether each provider is accepted, limited, requires verification, or is not accepted. This workflow reduces repetitive manual lookup work and improves response consistency.

---

## Workflow Objective

Build a safe prototype AI workflow that can:

1. Extract the insurance provider name from a user question.
2. Check the provider against a maintained Google Sheets knowledge base.
3. Route the request through controlled workflow logic.
4. Return a clean user-facing response.
5. Log the result for QA traceability.
6. Support regression and smoke testing.

---

## Workflow Capabilities

The final workflow can:

- Extract insurance provider names from natural language
- Handle exact provider names
- Handle partial names and aliases
- Normalize provider variations such as `Medi-Cal` to `Medicaid`
- Correct common spelling mistakes such as `Dleta Dental` to `Delta Dental`
- Search a Google Sheets insurance knowledge base
- Route provider-found, provider-not-found, and no-provider-mentioned cases
- Return clean user-facing responses
- Avoid exposing raw JSON or internal placeholder values
- Log workflow activity for traceability
- Support regression and smoke testing

---

## Tools and Technologies Used

- n8n
- OpenAI Chat Model node
- Google Sheets
- IF nodes
- Filter node
- Set/Edit Fields nodes
- Manual chat trigger
- Google Sheets QA evidence workbook

---

## Final Architecture

```text
Chat Trigger
→ Extract Provider Name
→ Provider Mentioned? IF node

If provider is mentioned:
→ Lookup Provider in Sheet
→ Filter Matching Provider
→ Provider Found? IF node
→ Format Found Provider Response
→ Log Result
→ Return Response

If provider is not mentioned:
→ Ask for Provider Name
→ Return Ask Provider Name

| Scenario                             | Workflow Path                                                                       | Expected Result                                     |
| ------------------------------------ | ----------------------------------------------------------------------------------- | --------------------------------------------------- |
| Provider is found                    | Provider Mentioned? → Lookup → Filter → Provider Found? → Found Response            | Clean accepted/limited/verify/not accepted response |
| Provider is mentioned but not listed | Provider Mentioned? → Lookup → Filter → Provider Found? false → Not Listed Response | Controlled fallback response                        |
| No provider is mentioned             | Provider Mentioned? false → Ask for Provider Name                                   | Clarification question                              |


