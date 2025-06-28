# Airbnb Clone - Backend Requirement Specifications

This document describes the detailed technical and functional requirements for three core backend features.

---

## 1️⃣ User Authentication

### ✨ Overview
Allow guests, hosts, and admins to securely register, log in, and manage sessions.

---

### 🟢 API Endpoints

- **POST /api/v1/auth/register**
  - Registers a new user.
  - **Input (JSON):**
    - `first_name` (string, required)
    - `last_name` (string, required)
    - `email` (string, required, unique, valid email format)
    - `password` (string, required, min 8 characters)
    - `role` (enum: guest | host | admin)
  - **Output:**
    - `201 Created` with user data (excluding password)
    - JWT token for authentication

- **POST /api/v1/auth/login**
  - Logs a user in.
  - **Input (JSON):**
    - `email` (string, required)
    - `password` (string, required)
  - **Output:**
    - `200 OK` with JWT token
    - User profile info

- **POST /api/v1/auth/logout**
  - Invalidates the user session.
  - **Input:**
    - JWT token in headers
  - **Output:**
    - `204 No Content`

---

### 🟢 Validation Rules
- Email must be unique and valid.
- Password minimum length: 8 characters.
- Role must be one of [guest, host, admin].

---

### 🟢 Performance Criteria
- Registration and login should respond in <500ms under normal load.
- Support 100 concurrent login requests.

---

## 2️⃣ Property Management

### ✨ Overview
Enable hosts to create and manage property listings.

---

### 🟢 API Endpoints

- **POST /api/v1/properties/**
  - Creates a new property.
  - **Input (JSON):**
    - `name` (string, required)
    - `description` (string, required)
    - `location` (string, required)
    - `price_per_night` (decimal, required)
    - `amenities` (array of strings)
    - `images` (array of file URLs)
  - **Output:**
    - `201 Created` with property details

- **GET /api/v1/properties/{id}**
  - Retrieve a specific property.
  - **Output:**
    - `200 OK` with property data

- **PUT /api/v1/properties/{id}**
  - Update property details.
  - **Input:**
    - Same fields as create
  - **Output:**
    - `200 OK` with updated data

- **DELETE /api/v1/properties/{id}**
  - Remove property.
  - **Output:**
    - `204 No Content`

---

### 🟢 Validation Rules
- Price must be positive.
- Name and description must not be empty.
- Location required.

---

### 🟢 Performance Criteria
- Listing retrieval in <300ms.
- Support 500 property updates per minute.

---

## 3️⃣ Booking System

### ✨ Overview
Allow guests to book properties, track status, and cancel reservations.

---

### 🟢 API Endpoints

- **POST /api/v1/bookings/**
  - Create a booking.
  - **Input (JSON):**
    - `property_id` (UUID, required)
    - `start_date` (date, required)
    - `end_date` (date, required)
  - **Output:**
    - `201 Created` with booking details

- **GET /api/v1/bookings/{id}**
  - Retrieve booking info.
  - **Output:**
    - `200 OK` with booking data

- **PATCH /api/v1/bookings/{id}/cancel**
  - Cancel booking.
  - **Output:**
    - `200 OK` with updated status

---

### 🟢 Validation Rules
- Start date must be before end date.
- Booking dates must not overlap existing confirmed bookings.
- Only the guest who created the booking or an admin can cancel.

---

### 🟢 Performance Criteria
- Booking creation must complete in <400ms.
- Prevent race conditions in concurrent bookings.

---

