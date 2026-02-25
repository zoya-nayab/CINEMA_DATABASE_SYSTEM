# 🎬 Cinema Database System
### Built with Oracle SQL & Oracle APEX
**Developer:** Zoya Nayab  
**Tool:** Oracle APEX 24.2 | Oracle Database  
**Project Type:** Database Management System

---

## 📌 Project Overview

This is a fully functional Cinema Database Management System built using Oracle SQL and Oracle APEX. The system manages movies, theaters, showtimes, customers, ticket bookings, payments, and invoices through a web-based application interface.

---

## 🗄️ Database Tables

The system consists of the following 8 core tables:

### 1. USERSS
Manages application users with role-based access.

| Column | Type | Description |
|--------|------|-------------|
| user_id | NUMBER (IDENTITY) | Primary Key |
| username | VARCHAR2(50) | Unique username |
| password | VARCHAR2(255) | User password |
| role | VARCHAR2(20) | ADMIN / MANAGER / USER |
| created_at | DATE | Account creation date |

### 2. GENRESS
Stores movie genres.

| Column | Type | Description |
|--------|------|-------------|
| genre_id | NUMBER (IDENTITY) | Primary Key |
| genre_name | VARCHAR2(50) | Unique genre name |

### 3. THEATERSS
Manages cinema halls with category-based pricing.

| Column | Type | Description |
|--------|------|-------------|
| theater_id | NUMBER (IDENTITY) | Primary Key |
| theater_name | VARCHAR2(100) | Hall name |
| capacity | NUMBER | Seating capacity |
| category | VARCHAR2(20) | PLATINUM / GOLD / SILVER |

### 4. MOVIESS
Stores all movie information.

| Column | Type | Description |
|--------|------|-------------|
| movie_id | NUMBER (IDENTITY) | Primary Key |
| title | VARCHAR2(150) | Movie title |
| description | CLOB | Movie description |
| duration_minutes | NUMBER | Duration in minutes |
| rating | NUMBER(3,1) | Rating (0 to 10) |
| release_date | DATE | Release date |
| status | VARCHAR2(20) | ACTIVE / INACTIVE |
| genre_id | NUMBER | Foreign Key → GENRESS |

### 5. CUSTOMERSS
Stores customer information.

| Column | Type | Description |
|--------|------|-------------|
| customer_id | NUMBER (IDENTITY) | Primary Key |
| full_name | VARCHAR2(100) | Customer full name |
| email | VARCHAR2(100) | Unique email |
| phone | VARCHAR2(20) | Phone number |
| registration_date | DATE | Registration date |

### 6. SHOWTIMESS
Manages movie showtimes in theaters.

| Column | Type | Description |
|--------|------|-------------|
| showtime_id | NUMBER (IDENTITY) | Primary Key |
| movie_id | NUMBER | Foreign Key → MOVIESS |
| theater_id | NUMBER | Foreign Key → THEATERSS |
| show_date | DATE | Show date |
| show_time | TIMESTAMP | Show time |
| price | NUMBER(10,2) | Ticket price |

### 7. TICKETSS
Manages ticket bookings.

| Column | Type | Description |
|--------|------|-------------|
| ticket_id | NUMBER (IDENTITY) | Primary Key |
| customer_id | NUMBER | Foreign Key → CUSTOMERSS |
| showtime_id | NUMBER | Foreign Key → SHOWTIMESS |
| seat_number | VARCHAR2(10) | Seat number |
| booking_date | DATE | Booking date |
| status | VARCHAR2(20) | BOOKED / CANCELLED |

**Constraint:** Each seat per showtime is unique — no double booking allowed.

### 8. PAYMENTSS
Handles payment records for tickets.

| Column | Type | Description |
|--------|------|-------------|
| payment_id | NUMBER (IDENTITY) | Primary Key |
| ticket_id | NUMBER | Foreign Key → TICKETSS (Unique) |
| payment_date | DATE | Payment date |
| amount | NUMBER(10,2) | Payment amount |
| payment_method | VARCHAR2(20) | CASH / CARD / ONLINE |
| payment_status | VARCHAR2(20) | PAID / PENDING / FAILED |

### 9. INVOICESS
Stores invoice records for completed transactions.

| Column | Type | Description |
|--------|------|-------------|
| invoice_id | NUMBER (IDENTITY) | Primary Key |
| customer_id | NUMBER | Foreign Key → CUSTOMERSS |
| ticket_id | NUMBER | Foreign Key → TICKETSS (Unique) |
| invoice_date | DATE | Invoice date |
| total_amount | NUMBER(12,2) | Total invoice amount |

---

## 🔒 Database Trigger

A trigger is placed on the INVOICESS table that prevents any deletion of invoice records to maintain financial integrity.

```sql
CREATE OR REPLACE TRIGGER prevent_invoice_delete
BEFORE DELETE ON invoicess
FOR EACH ROW
BEGIN
    RAISE_APPLICATION_ERROR(-20001, 'Invoice deletion is not allowed.');
END;
```

---

## ⚙️ How to Set Up

### Prerequisites
- Oracle Database (19c or above)
- Oracle APEX (24.2 or above)
- A configured APEX workspace

### Step 1 — Run the SQL Script
1. Open Oracle APEX
2. Go to **SQL Workshop → SQL Commands**
3. Copy and paste the full script
4. Click **Run**
5. Script will run with 0 errors

### Step 2 — Insert Theater Data
Run each statement one at a time in SQL Commands (no semicolons):

```sql
INSERT INTO theaterss (theater_name, capacity, category) VALUES ('A1', 300, 'PLATINUM')
```
```sql
INSERT INTO theaterss (theater_name, capacity, category) VALUES ('B1', 500, 'GOLD')
```
```sql
INSERT INTO theaterss (theater_name, capacity, category) VALUES ('C1', 700, 'SILVER')
```
```sql
COMMIT
```

### Step 3 — Create the APEX Application
1. Go to **App Builder → Create → New Application**
2. Name it **Cinema_Db**
3. Add pages for each table
4. Click **Create Application**

---

## 📊 Entity Relationship Summary

```
GENRESS ──────────── MOVIESS
                        │
                    SHOWTIMESS ──── THEATERSS
                        │
                    TICKETSS ──── CUSTOMERSS
                        │
                    PAYMENTSS
                        │
                    INVOICESS
```

---

## 👩‍💻 Developer Notes

- All table names end with double SS to avoid conflicts with Oracle reserved words
- IDENTITY columns are used instead of sequences for auto-increment IDs
- All CHECK constraints are defined at database level for data integrity
- The application was built and configured using Oracle APEX 24.2

---

*Cinema Database System — Developed by Zoya Nayab*
