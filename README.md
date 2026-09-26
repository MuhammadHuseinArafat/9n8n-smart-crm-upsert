# 9n8n-smart-crm-upsert
# 🧠 Project 10: Smart CRM Upsert System (Zero Duplicate Architecture)

![n8n](https://img.shields.io/badge/n8n-FF6D5W?style=for-the-badge&logo=n8n&logoColor=white)
![Google Sheets](https://img.shields.io/badge/Google%20Sheets-34A853?style=for-the-badge&logo=google-sheets&logoColor=white)

## 📖 Overview
An intelligent, event-driven data pipeline built in **n8n** designed to completely eliminate duplicate records in Google Sheets CRMs. By evaluating incoming webhook data before executing write operations, the system dynamically decides whether to update an existing lead or create a new one (Upsert logic).

## 🏢 The Business Problem
Standard automations act "blindly" by strictly appending data. When a prospect submits a lead generation form multiple times (e.g., to update their details or re-register for a campaign), the database gets cluttered with duplicate rows. Sales teams waste valuable time cleaning data and identifying the most recent prospect status.

## 💡 The Solution
A dynamic validation pipeline that reads the database prior to any write operations, ensuring 100% database hygiene.

### ⚙️ Workflow Architecture
1. **Webhook (Trigger):** Captures incoming JSON payloads (HTTP POST) from external forms.
2. **Google Sheets (Read/Lookup):** Queries the database using the incoming email as a unique identifier. Configured to `Always Output Data` to prevent workflow crashes on null returns.
3. **Conditional Routing (IF Node):**
   - **Branch True (Lead Exists):** Routes to an `Update Row` node to refresh the prospect's data. Uses Cross-Node Referencing to map webhook data directly.
   - **Branch False (New Lead):** Routes to an `Append Row` node to insert a new database entry.
<img width="692" height="346" alt="image" src="https://github.com/user-attachments/assets/a9da32ed-18db-4a38-8bab-88abea33012d" />
 


## 🛠️ Technical Highlights: Cross-Node Referencing
To ensure data integrity on the `Append` branch when the `Lookup` node returns an empty object `{}`, explicit cross-node referencing was implemented:
`{{ $('Webhook').item.json.body.nama_pendaftar }}`
This guarantees the executor nodes always fetch the raw payload regardless of intermediate node outputs.

## 📈 Business Value
- **100% Database Hygiene:** Zero duplicate entries.
- **Automated Data Maintenance:** Eliminates manual database cleanup.
- **Single Source of Truth:** Sales teams always interact with the most up-to-date prospect profile.

<img width="692" height="302" alt="image" src="https://github.com/user-attachments/assets/55c893d6-1fee-4760-bf6e-0e55f03d0f26" />

in collaboration with Gemini Pro
