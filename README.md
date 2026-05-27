## 🎓 CS50SQL Project & Database Design

This application implements a secure electronic health record (EHR) environment built on a fully normalized relational schema, satisfying the database standards of Harvard's CS50SQL:

* **Schema Design & Integrity:** 3NF normalized relationships spanning Users, Patients, Examinations, Prescriptions, and Payments. Strict foreign key mapping (`ON DELETE RESTRICT/CASCADE`) prevents orphaned medical data.
* **Concurrency & Locking:** Employs programmatic atomic locks to secure clinical data post-payment, forcing a hard *Read-Only* state to guarantee permanent audit trails.
* **Optimization:** Strategic indexing applied across heavily queried composite keys and status attributes (`patient_id`, `doctor_id`, status flags) to optimize fetch speeds for dense patient charts.

---

<img width="1260" height="1184" alt="SIMRS — CS50 SQL Final Project" src="https://github.com/user-attachments/assets/9b075be0-190e-487c-897f-a00ef4ce4ad5" />
<img width="1277" height="834" alt="SIMRS — Secure Portal Auth" src="https://github.com/user-attachments/assets/adc98d48-7954-4d7d-8ca7-2e08fd537fed" />

---

## 🚀 Core Workflows & Features

Both modules feature secure session authentication powered by **Laravel Breeze** alongside server-side validation (`FormRequest`) to ensure data integrity.

### 🩺 Doctor Module
* **Clinical Records:** Document patient vital signs (BP, HR, RR, Temp, BMI), free-text clinical assessment notes, and optional physical file attachments.
* **Prescription Engine:** Real-time drug lookup powered by external API sync. Doctors retain open editing access **strictly until** the prescription gets fulfilled.
* **System Logging:** Every data mutation generates immutable security logs.

### 💊 Pharmacist Module
* **Fulfillment & Pricing:** Fetch pending doctor prescriptions and calculate live costs through external fluctuating medicine API endpoints.
* **Billing System:** Process payments, generate official **PDF** receipts, and instantly issue a global hard lock on the underlying medical files.

---

## 🔄 System Workflows

### 1. Architectural Sequence
```mermaid
sequenceDiagram
    participant D as 🩺 Doctor
    participant API as 🌐 External Medicine API
    participant DB as 🗄️ Database
    participant A as 💊 Pharmacist

    D->>DB: Save Examination Data & Vital Signs
    D->>API: Request Live Medicine Catalog
    API-->>D: Return Live Medicine Data
    D->>DB: Create Prescription Draft
    A->>API: Sync Fluctuation Prices
    A->>DB: Process Payment & Hard Lock Record
    DB-->>A: Export Receipt PDF
```

### 2.Business Logic Flow

```mermaid
graph TD
    A[Patient Encounter] --> B[Doctor: Input Examination Details]
    B --> C{Prescribe Medication?}
    C -- Yes --> D[Doctor: Sync External API Catalog]
    D --> E[Save Prescription Draft]
    E --> F[Pharmacist: Verify & Sync Real-Time Price API]
    F --> G[Pharmacist: Process Payment]
    G --> H[System Lock: Set Medical File to Read-Only]
    H --> I[Generate PDF Invoice]
    C -- No --> J[Save Baseline Medical Record Only]
```
