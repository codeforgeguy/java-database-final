# Smart Clinic Management System - Database Schema Design

## 1. Relational Database Design (MySQL)

MySQL manages the structured, relational data for clinic administration, patient profiles, doctors, and appointment scheduling.

---

### 1.1 `admin` Table
Manages administrative accounts and role-based access.

| Column Name | Data Type | Key / Index | Constraints | Description |
| :--- | :--- | :--- | :--- | :--- |
| `admin_id` | `BIGINT` | Primary Key | `AUTO_INCREMENT`, `NOT NULL` | Unique identifier for admin |
| `username` | `VARCHAR(50)` | Unique Key | `NOT NULL`, `UNIQUE` | Admin login username |
| `password_hash` | `VARCHAR(255)` | None | `NOT NULL` | Encrypted password |
| `email` | `VARCHAR(100)` | Unique Key | `NOT NULL`, `UNIQUE` | Admin contact email |
| `created_at` | `TIMESTAMP` | None | `DEFAULT CURRENT_TIMESTAMP` | Account creation timestamp |

---

### 1.2 `patients` Table
Maintains patient demographics and contact details.

| Column Name | Data Type | Key / Index | Constraints | Description |
| :--- | :--- | :--- | :--- | :--- |
| `patient_id` | `BIGINT` | Primary Key | `AUTO_INCREMENT`, `NOT NULL` | Unique identifier for patient |
| `first_name` | `VARCHAR(50)` | None | `NOT NULL` | Patient's first name |
| `last_name` | `VARCHAR(50)` | None | `NOT NULL` | Patient's last name |
| `email` | `VARCHAR(100)` | Unique Key | `NOT NULL`, `UNIQUE` | Contact email |
| `phone_number` | `VARCHAR(20)` | None | `NOT NULL` | Contact phone number |
| `date_of_birth` | `DATE` | None | `NOT NULL` | Date of birth |
| `gender` | `VARCHAR(10)` | None | `NOT NULL` | Gender |
| `created_at` | `TIMESTAMP` | None | `DEFAULT CURRENT_TIMESTAMP` | Registration timestamp |

---

### 1.3 `doctors` Table
Maintains licensed medical practitioners, their specialties, and fees.

| Column Name | Data Type | Key / Index | Constraints | Description |
| :--- | :--- | :--- | :--- | :--- |
| `doctor_id` | `BIGINT` | Primary Key | `AUTO_INCREMENT`, `NOT NULL` | Unique identifier for doctor |
| `first_name` | `VARCHAR(50)` | None | `NOT NULL` | Doctor's first name |
| `last_name` | `VARCHAR(50)` | None | `NOT NULL` | Doctor's last name |
| `specialization` | `VARCHAR(100)` | None | `NOT NULL` | Specialty area (e.g., Cardiology) |
| `email` | `VARCHAR(100)` | Unique Key | `NOT NULL`, `UNIQUE` | Doctor clinic email |
| `phone_number` | `VARCHAR(20)` | None | `NOT NULL` | Office contact phone |
| `consultation_fee` | `DECIMAL(10,2)` | None | `NOT NULL`, `CHECK (consultation_fee >= 0)` | Standard visit fee |

---

### 1.4 `appointments` Table
Tracks scheduling details linking patients with doctors.

| Column Name | Data Type | Key / Index | Constraints | Description |
| :--- | :--- | :--- | :--- | :--- |
| `appointment_id` | `BIGINT` | Primary Key | `AUTO_INCREMENT`, `NOT NULL` | Unique appointment ID |
| `patient_id` | `BIGINT` | Foreign Key | `NOT NULL`, References `patients(patient_id)` | Linked patient |
| `doctor_id` | `BIGINT` | Foreign Key | `NOT NULL`, References `doctors(doctor_id)` | Assigned doctor |
| `appointment_date` | `DATETIME` | None | `NOT NULL` | Scheduled date and time |
| `status` | `VARCHAR(20)` | None | `NOT NULL`, `DEFAULT 'SCHEDULED'` | Lifecycle state: SCHEDULED, COMPLETED, CANCELLED |
| `notes` | `TEXT` | None | `NULL` | Reason for visit / symptoms |

---

## 2. Document Database Design (MongoDB)

MongoDB is chosen for clinical documents such as prescriptions because prescription data is semi-structured, variable in length, and frequently updated with different medication lists.

### Collection: `prescriptions`

#### Realistic Document Structure (JSON)
```json
{
  "_id": { "$oid": "670c3a8e9f2b1d001a4e8912" },
  "appointmentId": 105,
  "patientId": 12,
  "doctorId": 3,
  "issueDate": "2026-10-13T14:30:00Z",
  "diagnosis": {
    "primary": "Acute Bronchitis",
    "icdCode": "J20.9",
    "notes": "Mild fever, dry cough, lungs clear on auscultation"
  },
  "medications": [
    {
      "name": "Amoxicillin",
      "dosage": "500mg",
      "frequency": "Every 8 hours",
      "durationDays": 7,
      "instructions": "Take with water after meals"
    },
    {
      "name": "Dextromethorphan Syrup",
      "dosage": "10ml",
      "frequency": "Twice daily as needed",
      "durationDays": 5,
      "instructions": "Avoid alcohol"
    }
  ],
  "followUp": {
    "required": true,
    "recommendedDate": "2026-10-20T10:00:00Z",
    "instructions": "Return if symptoms persist beyond 7 days"
  },
  "digitalSignature": {
    "signedBy": "Dr. Sarah Jenkins",
    "licenseNumber": "DOC-NY-90210",
    "signedAt": "2026-10-13T14:35:00Z"
  }
}
