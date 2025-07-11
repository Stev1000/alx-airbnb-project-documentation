# Backend Feature Requirements - Airbnb Clone

This document defines the technical and functional requirements for key backend modules of the Airbnb Clone system.

---

## 1. User Authentication

### 🧩 Functional Requirements

- Users (guest, host) must be able to register and log in.
- The system must hash passwords before storing them.
- Token-based authentication must be used for session management.

### 🌐 API Endpoints

| Method | Endpoint        | Description         |
|--------|------------------|---------------------|
| POST   | /api/auth/signup | Register a new user |
| POST   | /api/auth/login  | Log in and get token|

### 🔽 Inputs

- `email`: string (valid format, unique)
- `password`: string (min 8 characters)
- `role`: enum (`guest`, `host`)

### 🔼 Outputs

- JSON with `user_id`, `token`, and `role`

### ✅ Validation Rules

- Email must be valid and unique
- Password must be minimum 8 characters

### ⚙️ Performance

- Response time ≤ 300ms
- Token expiration: 1 hour

---

## 2. Property Management

### 🧩Functional Requirements

- Hosts can create, edit, and delete properties.
- Guests can view properties with filters.

### 🌐API Endpoints

| Method | Endpoint            | Description             |
|--------|----------------------|-------------------------|
| POST   | /api/properties      | Add new property        |
| GET    | /api/properties      | List all properties     |
| PUT    | /api/properties/:id  | Edit property           |
| DELETE | /api/properties/:id  | Delete property         |

### 🔽 Inputs (for POST/PUT)

- `title`: string (max 150 chars)
- `description`: text
- `city`: string
- `price_per_night`: decimal
- `host_id`: UUID (must be a host)

### 🔼Outputs

- JSON confirmation or list of property objects

### ✅Validation Rules

- Title must be present
- Price must be a positive decimal

### ⚙️Performance

- Listing should support pagination (limit, offset)
- Full-text search for title and city

---

## 3. Booking System

### Functional Requirements

- Guests can book properties.
- System must prevent double bookings.
- Hosts must see booking requests.

### API Endpoints

| Method | Endpoint           | Description             |
|--------|---------------------|-------------------------|
| POST   | /api/bookings       | Create new booking      |
| GET    | /api/bookings/:id   | View booking by ID      |
| GET    | /api/bookings/user  | View user’s bookings    |

### Inputs

- `property_id`: UUID
- `guest_id`: UUID
- `check_in_date`: date
- `check_out_date`: date

### Outputs

- JSON with booking confirmation

### Validation Rules

- Dates must not overlap with existing bookings
- Check-out must be after check-in

### Performance

- Conflict check must run in < 100ms
- Concurrent booking attempts must be handled atomically
