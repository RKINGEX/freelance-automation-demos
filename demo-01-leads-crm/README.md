# Demo 01 — Automated Lead Management & CRM

## Overview

An automation system designed to receive leads from a form or external application, validate the submitted information, automatically store the lead in a CRM, and notify the responsible person in real time.

The workflow was built with **n8n**, using a Webhook as the entry point, Google Sheets as the CRM, and Telegram for real-time notifications.

## Problem

When businesses handle potential customers manually, several problems can occur:

* Leads can be lost or forgotten.
* Customer information can be entered incorrectly.
* Follow-up can be delayed.
* Sales teams may not be notified immediately.
* Lead information may not be stored in a centralized system.

## Solution

This workflow automates the process from receiving a lead to storing and notifying the responsible person.

```text
Form / External System
          ↓
       Webhook
          ↓
     Edit Fields
          ↓
      Validation
          ↓
     ┌────┴────┐
   TRUE       FALSE
     ↓           ↓
Google Sheets   Response
     ↓
  Telegram
     ↓
HTTP Response
```

## Features

* Receives lead data through an **HTTP POST Webhook**.
* Processes structured **JSON** data.
* Normalizes incoming information.
* Validates required fields before processing.
* Automatically generates a unique lead ID.
* Records the date and time of registration.
* Assigns an initial lead status.
* Stores valid leads in Google Sheets.
* Sends real-time notifications through Telegram.
* Returns JSON responses for successful and rejected requests.
* Handles incomplete lead submissions.

## Lead Data

Each lead contains:

| Field                 | Description                             |
| --------------------- | --------------------------------------- |
| `id_lead`             | Automatically generated lead identifier |
| `fecha_registro`      | Registration date and time              |
| `nombre`              | Customer first name                     |
| `apellidos`           | Customer last name                      |
| `correo`              | Customer email                          |
| `numero`              | Customer phone number                   |
| `servicio_solicitado` | Requested service                       |
| `notas`               | Additional information                  |
| `estado`              | Initial lead status                     |

## Technologies

* **n8n** — Workflow automation and orchestration.
* **HTTP / Webhooks** — Lead intake and communication.
* **JSON** — Data structure and transport.
* **Google Sheets** — CRM and lead storage.
* **Telegram Bot API** — Real-time notifications.

## Workflow

### 1. Lead Intake

The system receives a `POST` request through a Webhook.

Example payload:

```json
{
  "nombre": "Juan",
  "apellidos": "Pérez",
  "correo": "juan@email.com",
  "numero": "809-555-1234",
  "servicio_solicitado": "Diseño de página web",
  "notas": "Necesito una cotización."
}
```

### 2. Data Processing

The incoming data is extracted and organized so it can be used consistently throughout the workflow.

### 3. Validation

The system verifies that all required fields are present before continuing.

If the data is valid, the workflow continues to the CRM.

If required information is missing, the request is rejected and an appropriate JSON response is returned.

### 4. CRM Registration

Valid leads are automatically stored in Google Sheets together with:

* Lead ID.
* Registration date.
* Initial status.

### 5. Notification

After the lead is successfully registered, n8n sends a Telegram notification containing the relevant customer information.

### 6. Response

The Webhook returns a JSON response indicating whether the request was successfully processed or rejected.

## Example Notification

```text
🔔 NEW LEAD RECEIVED

👤 Juan Pérez
📧 juan@email.com
📱 809-555-1234

🛠️ Service:
Website Design

📝 Notes:
I need a quote.

📊 Status: New
```

## Use Cases

This type of automation can be applied to businesses that regularly receive customer inquiries or quote requests, including:

* Dental clinics and healthcare practices.
* Gyms and fitness businesses.
* Real estate agencies.
* Car dealerships and vehicle sellers.
* Marketing agencies.
* Web development agencies.
* Professional service providers.
* Businesses that rely on quote requests and lead generation.

## Business Value

The workflow transforms a manual lead intake process into an automated system:

**Receive → Validate → Identify → Store → Notify → Respond**

This reduces repetitive administrative work, centralizes lead information, and helps businesses respond to new potential customers more efficiently.

## Evidence

### Workflow

![Workflow](./screenshots/01-workflow.png)

### Validation

![Validation](./screenshots/02-validation.png)

### CRM

![Google Sheets CRM](./screenshots/03-crm.png)

### Telegram Notification

![Telegram](./screenshots/04-telegram.png)

## Workflow File

The n8n workflow used for this demonstration is available in:

`workflow.json`

> Credentials, API tokens, and other secrets used during development are not included in this repository.
