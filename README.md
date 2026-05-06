# 🏥 QA Lifecycle Testing — Patient Appointment Booking System

> End-to-end quality assurance project covering API testing, manual test design, SQL validation, bug reporting, and UAT sign-off for a RESTful booking application.

---

## 📌 Project Overview

This project simulates a **real-world QA engagement** for a patient appointment booking system. It covers the full QA lifecycle — from test planning to UAT sign-off — using industry-standard tools and processes.

The system under test is **Restful Booker** (`https://restful-booker.herokuapp.com`), a live practice API that mimics a booking management backend.

---

## 🎯 Objectives

- Design and execute a comprehensive test plan covering functional and negative scenarios
- Validate all CRUD operations via REST API using Postman
- Write SQL queries to verify data integrity at the database layer
- Identify, document, and track defects using structured bug reports
- Deliver UAT sign-off documentation as a final quality gate

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| **Postman** | API test execution and collection management |
| **MySQL** | Database-layer validation queries |
| **Excel** | Test case documentation and bug reporting |
| **GitHub** | Version control and project artifact storage |
| **MS Word** | Test Plan and UAT Sign-off documents |

---

## 📋 Test Collection Structure

The Postman collection **"Patient Booking System — QA API Test Collection"** is organized into 5 modules:

| Module | Test Cases |
|---|---|
| 1. Authentication | Generate Auth Token |
| 2. Create Booking | TC_008, TC_011, TC_012 |
| 3. Get Booking | TC_015, TC_016, TC_017, TC_018 |
| 4. Update Booking | TC_020, TC_021, TC_022 |
| 5. Delete Booking | TC_025, TC_026, TC_027 |

---

## 🔍 API Endpoints Tested

| Method | Endpoint | Test Case | Description |
|---|---|---|---|
| `POST` | `/auth` | — | Generate authentication token |
| `POST` | `/booking` | TC_008 | Create valid booking |
| `POST` | `/booking` | TC_011 | Create booking with invalid date format (negative) |
| `POST` | `/booking` | TC_012 | Checkout before checkin — BUG (negative) |
| `GET` | `/booking/{id}` | TC_015 | Get booking by valid ID |
| `GET` | `/booking/{id}` | TC_016 | Get booking by invalid ID (negative) |
| `GET` | `/booking` | TC_017 | Get all bookings |
| `GET` | `/booking?firstname=` | TC_018 | Filter bookings by name |
| `PUT` | `/booking/{id}` | TC_020 | Full update with auth token |
| `PATCH` | `/booking/{id}` | TC_021 | Partial update PATCH |
| `PUT` | `/booking/{id}` | TC_022 | Update without auth (negative) |
| `DELETE` | `/booking/{id}` | TC_025 | Delete valid booking |
| `GET` | `/booking/{id}` | TC_027 | Verify booking deleted (GET after DELETE) |
| `DELETE` | `/booking/{id}` | TC_026 | Delete without auth (negative) |

---

## 📸 Test Execution Evidence

### 1. Authentication — Token Generated Successfully
- **POST** `/auth` → `200 OK` · 322ms
- Token received: `ba7d9431a7a2c7b`
- Test Results: **3/3 passed**

### 2. Create Booking (TC_008)
- **POST** `/booking` → `200 OK` · 334ms
- Booking ID assigned: **976**
- Patient: John Smith | Check-in: 2025-03-01 | Check-out: 2025-03-07
- Additional needs: Wheelchair accessible room
- Test Results: **5/5 passed**

### 3. Get Booking by Valid ID (TC_015)
- **GET** `/booking/976` → `200 OK` · 329ms
- Correct booking data returned; all fields match the original POST payload
- Test Results: **4/4 passed**

### 4. Full Update — PUT with Auth (TC_020)
- **PUT** `/booking/976?Cookie=ba7d9431a7a2c7b` → `200 OK` · 2.33s
- Updated patient: James Smith | New price: 2000 | Additional needs: Breakfast included
- Test Results: **3/3 passed**

### 5. Delete Booking (TC_025)
- **DELETE** `/booking/976?Cookie=ba7d9431a7a2c7b` → `201 Created` · 303ms
- Response body: `Created`
- ⚠️ **Bug noted:** DELETE returned `201 Created` instead of expected `200 OK` or `204 No Content` — logged as BUG-002

### 6. Verify Deletion (TC_027)
- **GET** `/booking/976` → **`404 Not Found`** · 308ms
- Confirms booking 976 was successfully removed from the system
- Test Results: **2/2 passed**

---

## 🐛 Defects Found

| Bug ID | Test Case | Summary | Severity | Status |
|---|---|---|---|---|
| BUG-001 | TC_012 | Checkout date accepted before checkin — no validation error returned | High | Logged |
| BUG-002 | TC_025 | DELETE returns `201 Created` instead of `200 OK` or `204 No Content` | Medium | Logged |
| BUG-003 | TC_011 | Invalid date format accepted in request body without error response | Medium | Logged |

> Full defect details in [`Bug_Report.xlsx`](./Bug_Report.xlsx)

---

## ✅ Test Execution Summary

| Module | Total Tests | Passed | Failed | Pass Rate |
|---|---|---|---|---|
| Authentication | 3 | 3 | 0 | 100% |
| Create Booking | 5 | 4 | 1 | 80% |
| Get Booking | 4 | 4 | 0 | 100% |
| Update Booking | 3 | 3 | 0 | 100% |
| Delete Booking | 3 | 2 | 1 | 67% |
| **Total** | **18** | **16** | **2** | **89%** |

---

## 🗄️ SQL Validation

SQL queries were written to validate:
- Booking record 976 persisted correctly after the POST request
- Updated fields (name, price, dates) reflect correctly after PUT
- Booking 976 no longer exists in the database after DELETE
- Date field formats match expected schema constraints

> Queries available in [`SQL_Validation_Queries.sql`](./SQL_Validation_Queries.sql)

---

## 🚀 How to Run the Postman Collection

1. Download [`BookingAPI_QA_Collection.json`](./BookingAPI_QA_Collection.json)
2. Open Postman → Click **Import** → Select the file
3. Run **"Generate Auth Token"** first — copy the returned token
4. Pass the token as a query param on PUT and DELETE: `?Cookie=<token>`
5. Note the `bookingid` from the Create response and use it in subsequent requests

> Base URL: `https://restful-booker.herokuapp.com`

---

## 📁 Project Artifacts

| File | Description |
|---|---|
| [`Test_Plan.docx`](./Test_Plan.docx) | Scope, approach, entry/exit criteria, schedule |
| [`Test_Cases.xlsx`](./Test_Cases.xlsx) | Detailed test cases with steps, expected vs actual results |
| [`Bug_Report.xlsx`](./Bug_Report.xlsx) | Defect log with severity, steps to reproduce, and status |
| [`SQL_Validation_Queries.sql`](./SQL_Validation_Queries.sql) | DB-layer verification queries |
| [`BookingAPI_QA_Collection.json`](./BookingAPI_QA_Collection.json) | Importable Postman collection |
| [`UAT_Signoff_Document.docx`](./UAT_Signoff_Document.docx) | Final UAT approval document |

---

## 📚 Skills Demonstrated

`API Testing` · `Manual Testing` · `Test Planning` · `Test Case Design` · `Negative Testing` · `Bug Reporting` · `SQL Validation` · `Postman` · `SDLC` · `STLC` · `UAT` · `REST API`

---

## 👩‍💻 Author

**Sakshi Awasthi**  
QA Engineer | [GitHub](https://github.com/sakshii1411)

---

*This project was built as a portfolio piece to demonstrate end-to-end QA skills across a realistic patient booking domain.*
