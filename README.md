<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0a7c8c&height=160&section=header&text=SIMRS&fontSize=52&fontColor=ffffff&fontAlignY=38&desc=Medication%20Prescribing%20System&descAlignY=62&descSize=16&descColor=a4e2d4" width="100%"/>

<br/>

[![Laravel](https://img.shields.io/badge/Laravel-12.x-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)](https://laravel.com)
[![PHP](https://img.shields.io/badge/PHP-8.2%2B-777BB4?style=for-the-badge&logo=php&logoColor=white)](https://php.net)
[![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?style=for-the-badge&logo=mysql&logoColor=white)](https://mysql.com)
[![Alpine.js](https://img.shields.io/badge/Alpine.js-3.x-8BC0D0?style=for-the-badge&logo=alpinedotjs&logoColor=white)](https://alpinejs.dev)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind-3.x-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)](https://tailwindcss.com)

<br/>

> **CS50SQL Final Project** — Harvard University  
> A high-performance electronic health record (EHR) system built on a fully relational schema,
> satisfying the database design and integrity standards of Harvard's CS50 SQL curriculum.

<br/>

[![CS50 Certificate](https://img.shields.io/badge/CS50SQL-Verified%20Certificate-A51C30?style=flat-square&logo=edx&logoColor=white)](https://cs50.harvard.edu/sql)
&nbsp;
[![Status](https://img.shields.io/badge/Status-Production%20Ready-0d9e84?style=flat-square)](.)
&nbsp;
[![License](https://img.shields.io/badge/License-MIT-0a7c8c?style=flat-square)](LICENSE)

</div>

---

## 🏥 Overview

**SIMRS** *(Sistem Informasi Manajemen Rumah Sakit)* is a clinical medication prescribing interface connecting two roles — **Doctor** and **Pharmacist** — through a structured relational database pipeline.

The system manages the full lifecycle of a patient encounter: from examination and vital signs recording, to prescription drafting with live medicine API integration, through to pharmacy dispensing, payment processing, and automatic PDF invoice generation — with every mutation permanently logged in an immutable activity trail.

```
Patient Encounter → Doctor Examination → Prescription Draft
       → Pharmacist Validation → Payment → Hard Lock + PDF Invoice
```
---

## 📸 Screenshot

### 🔐 Welcome & Access
| Welcome Page | Authentication |
| :---: | :---: |
| <a href="https://github.com/user-attachments/assets/3a077659-d255-4939-9dfa-6e223c564c22"><img width="300" alt="Welcome" src="https://github.com/user-attachments/assets/3a077659-d255-4939-9dfa-6e223c564c22" /></a> | <a href="https://github.com/user-attachments/assets/a30d907e-c363-492f-929f-abacf8cc3f4b"><img width="300" alt="Login" src="https://github.com/user-attachments/assets/a30d907e-c363-492f-929f-abacf8cc3f4b" /></a> |

### 👨‍⚕️ Doctor Module
| Dashboard Overview | Patient Examination |
| :---: | :---: |
| <a href="https://github.com/user-attachments/assets/fe8cfebf-059f-4ca7-a214-3eaad07d76ab"><img width="300" alt="Doctor Dashboard" src="https://github.com/user-attachments/assets/fe8cfebf-059f-4ca7-a214-3eaad07d76ab" /></a> | <a href="https://github.com/user-attachments/assets/9572178e-6096-4a73-988c-cb91c8a0bb30"><img width="300" alt="Medical Record" src="https://github.com/user-attachments/assets/9572178e-6096-4a73-988c-cb91c8a0bb30" /></a> |
| **Medication Selection** | **Activity Log** |
| <a ref="https://github.com/user-attachments/assets/0299ca0b-7648-4fb8-b4a2-a42ef5ecc40b"><img width="300" alt="Examination" src="https://github.com/user-attachments/assets/0299ca0b-7648-4fb8-b4a2-a42ef5ecc40b"  /></a> | <a href="https://github.com/user-attachments/assets/f7f24a9d-71be-4276-86e0-2c26fd8f12fb"><img width="300" alt="Activity" src="https://github.com/user-attachments/assets/f7f24a9d-71be-4276-86e0-2c26fd8f12fb" /></a> |

### 💊 Pharmacist Module
| Dashboard Overview | Prescription Processing |
| :---: | :---: |
| <a href="https://github.com/user-attachments/assets/648477d1-d4b0-4634-a2f4-732de6d59878"><img width="300" alt="Inventory" src="https://github.com/user-attachments/assets/648477d1-d4b0-4634-a2f4-732de6d59878" /></a> | <a href="https://github.com/user-attachments/assets/f1c0129f-e5ae-41ce-8ac3-97f2f404001e"><img width="300" alt="Processing" src="https://github.com/user-attachments/assets/f1c0129f-e5ae-41ce-8ac3-97f2f404001e" /></a> 

---

## 🔄 System Workflows

### Architectural Sequence

```mermaid
sequenceDiagram
    participant D  as 🩺 Doctor
    participant API as 🌐 Medicine API
    participant DB  as 🗄️ Database
    participant P  as 💊 Pharmacist

    D->>DB: Save Examination & Vital Signs
    D->>API: Request Live Medicine Catalog
    API-->>D: Return Live Drug Data
    D->>DB: Create Prescription Draft

    P->>API: Sync Real-Time Prices
    API-->>P: Return Fluctuated Pricing
    P->>DB: Validate & Process Payment
    DB-->>P: Hard Lock Record + Export PDF Invoice
```

### Business Logic Flow

```mermaid
graph TD
    A([🏥 Patient Encounter]) --> B[Doctor: Input Examination & Vital Signs]
    B --> C{Prescribe Medication?}

    C -- Yes --> D[Doctor: Sync External API Catalog]
    D --> E[Save Prescription Draft]
    E --> F[Pharmacist: Verify & Sync Real-Time Prices]
    F --> G[Pharmacist: Process Payment]
    G --> H[🔒 System Lock — Set Record to Read-Only]
    H --> I([📄 Generate PDF Invoice])

    C -- No --> J([💾 Save Baseline Medical Record Only])

    style A fill:#0a2e38,color:#a4e2d4,stroke:#0a7c8c
    style H fill:#0a2e38,color:#a4e2d4,stroke:#0d9e84
    style I fill:#0d9e84,color:#fff,stroke:#059669
    style J fill:#4a6870,color:#fff,stroke:#7a9ca5
```
### Prescription Status Lifecycle

```
[pending] ──────► [calculated] ──────► [paid / locked]
    │                   │                      │
  Draft            Pharmacist              Hard Lock
  by Doctor        validates            PDF generated
                  & prices synced       record frozen
```

## 🚀 Installation

### Prerequisites

| Requirement | Version |
|---|---|
| PHP | ≥ 8.2 |
| Composer | ≥ 2.x |
| MySQL | ≥ 8.0 |
| Node.js | ≥ 18.x |

### Step 1 — Clone & Install Dependencies

```bash
git clone https://github.com/your-username/medication-prescribing-web-app.git
cd medication-prescribing-web-app.

composer install
```

### Step 2 — Environment Configuration

```bash
cp .env.example .env
php artisan key:generate
```

Update `.env` with your credentials:

```env
# ── Database ──────────────────────────────────
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=medication_prescribing-web-app
DB_USERNAME=root
DB_PASSWORD=

# ── Medicine API (External) ───────────────────
MEDICINE_API_BASE_URL=https://rxnav.nlm.nih.gov/REST
MEDICINE_API_EMAIL=dummy_email
MEDICINE_API_PASSWORD=dummy_password
MEDICINE_API_TOKEN_CACHE_KEY=medicine_api_bearer_token
```

### Step 3 — Database Migration & Seeding

```bash
php artisan migrate --seed
```

> **Note:** The seeder automatically generates **10 mock patients** via Factories and initializes default access accounts for both the Doctor and Pharmacist roles.

### Step 4 — Run Local Server

```bash
php artisan serve
```

Navigate to `http://localhost:8000` and log in with the seeded demo accounts:

| Role | Email | Password |
|---|---|---|
| 🩺 Doctor | `doctor@harvard.edu` | `password` |
| 💊 Pharmacist | `pharmacist@harvard.edu` | `password` |

---

## 🧩 Core Features

### 🩺 Doctor Module

<details>
<summary><strong>Examination Input</strong></summary>
<br/>

- **Smart Patient Selection** — patients sorted by most recent arrival for instant access
- **Examination Timestamp** — automated logging to track medicine pricing benchmarks at time of recording
- **Vital Signs Tracking** — complete input mapping: Height, Weight, Blood Pressure (Systole/Diastole), Heart Rate, Respiration Rate, Body Temperature
- **Clinical Notes** — free-text SOAP observation fields
- **Document Attachment** — optional upload for external lab or diagnostic files (PDF/Images)

</details>

<details>
<summary><strong>Prescription Management</strong></summary>
<br/>

- **Live API Integration** — real-time drug catalog lookup via external REST API endpoints
- **Medicine Search Widget** — Alpine.js powered live search replaces static dropdowns; filters across full catalog client-side
- **Signa Builder** — route, frequency, timing, and pharmacy notes per medicine item
- **Edit Access Control** — doctors can freely amend prescriptions until pharmacist processes payment
- **Backend Validation** — server-side guard layer preserving medical record integrity

</details>

<details>
<summary><strong>Activity Logging</strong></summary>
<br/>

- Every `CREATE`, `UPDATE`, and `DELETE` mutation is permanently written to `activity_logs`
- Stores before/after snapshots for UPDATED events
- Records IP address, user agent, and timestamp per entry
- Accessible from the Doctor dashboard under *Activity Log*

</details>

---

### 💊 Pharmacist Module

<details>
<summary><strong>Prescription Queue</strong></summary>
<br/>

- View all pending prescription drafts submitted by doctors
- Expandable drawer per row reveals itemized medicine list with quantities and subtotals
- Real-time status indicators: `Waiting` → `Verified` → `Dispensed`

</details>

<details>
<summary><strong>Validation & Payment Processing</strong></summary>
<br/>

- **Price Sync** — fetches active fluctuating medicine prices via medicine ID before calculating totals
- **Validate** — moves prescription from `pending` to `calculated`, locking itemized costs
- **Process Payment** — finalizes transaction and issues hard lock on the clinical record

</details>

<details>
<summary><strong>Finalization & Output</strong></summary>
<br/>

- **Hard Lock** — post-payment, medical record is set to `Read-Only`; doctor edit access is revoked
- **PDF Invoice** — standardized payment receipt exported in PDF format for patient records

</details>

---

## 🏆 Achievement

<div align="center">

<img width="700" alt="CS50 SQL Certificate — Imam Bastomi" src="https://github.com/user-attachments/assets/9cea2d45-f129-4fb2-b0cb-90d17caf7ea3"/>

<br/><br/>

**CS50 SQL — Harvard University**  
*Verified Certificate of Completion*

</div>

---

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0a7c8c&height=100&section=footer&fontColor=ffffff" width="100%"/>

<sub>
© 2026 SIMRS CS50 SQL Final Project &nbsp;·&nbsp; Developed by <strong>Imam Bastomi</strong> &nbsp;·&nbsp; <em>This is CS50.</em>
</sub>

</div>
