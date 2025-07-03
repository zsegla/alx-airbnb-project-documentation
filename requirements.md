# 📋 Backend Requirements Specification – Airbnb Clone

## 🎯 Objective
This document outlines the detailed technical and functional specifications for key backend features of the Airbnb Clone backend system.

---

## 1️⃣ User Authentication

### 🔧 Functional Requirements
- Users can register as guests or hosts.
- Users can log in using email and password.
- Sessions are managed securely using JWT.
- Optionally support OAuth logins (e.g., Google).

### 🔗 API Endpoints

| Method | Endpoint          | Description                        |
|--------|-------------------|------------------------------------|
| POST   | /api/auth/signup  | Register a new user                |
| POST   | /api/auth/login   | Log in and receive JWT token       |
| GET    | /api/auth/profile | Retrieve current authenticated user |

### 📝 Input/Output

**Signup Request**
```json
{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "password": "securepass123",
  "role": "guest"
}
```
Signup Response
```
{
  "message": "User created successfully",
  "token": "JWT_TOKEN"
}
```
✅ Validation Rules
  - Email must be valid and unique.

  - Password must be at least 8 characters.

  - Role must be one of: guest, host.

🚀 Performance Criteria
Response time under 500ms.

Token expiration: 24 hours.

Rate limit: 5 signup attempts per minute per IP.
---
## 2️⃣ Property Management
🔧 Functional Requirements
Hosts can add, edit, delete, and view properties.

Guests can view and search for available listings.

🔗 API Endpoints
| Method | Endpoint             | Description                      |
| ------ | -------------------- | -------------------------------- |
| POST   | /api/properties      | Add a new property               |
| PUT    | /api/properties/\:id | Edit an existing property        |
| DELETE | /api/properties/\:id | Delete a property                |
| GET    | /api/properties      | List/search available properties |
| GET    | /api/properties/\:id | Get property details             |

📝 Input/Output
Add Property Request
```
{
  "title": "Ocean View Apartment",
  "location": "Addis Ababa",
  "price": 70,
  "description": "A cozy apartment with a great view.",
  "amenities": ["wifi", "parking"]
}
```
Response
```
{
  "message": "Property listed successfully",
  "property_id": "uuid"
}
```
✅ Validation Rules
  - price must be a positive decimal.

  - title and description are required.

  - Only users with host role can add/edit/delete.

🚀 Performance Criteria
  - Indexed filters on location, price, and amenities.

  - Pagination with default 10 results per page.

  - Image file size capped at 5MB per upload.

3️⃣ Booking System
🔧 Functional Requirements
  - Guests can book available properties.

  - Prevent overlapping bookings via date validation.

  - Bookings can be canceled.

  - Status tracking for bookings.

🔗 API Endpoints
| Method | Endpoint                  | Description                    |
| ------ | ------------------------- | ------------------------------ |
| POST   | /api/bookings             | Create a booking               |
| GET    | /api/bookings             | List bookings for current user |
| PUT    | /api/bookings/\:id/cancel | Cancel a booking               |

📝 Input/Output
Booking Request
```
{
  "property_id": "uuid",
  "start_date": "2025-07-01",
  "end_date": "2025-07-04"
}
```
Booking Response
```
{
  "message": "Booking confirmed",
  "booking_id": "uuid"
}
```
✅ Validation Rules
  - Dates must not overlap with existing bookings.

  - start_date must be before end_date.

  - Only users with guest role can book.

🚀 Performance Criteria
  - Conflict check must complete within 1s.

  - Booking status changes: pending → confirmed.

  - Bookings should be stored with indexing on property_id and dates.

🛠️ General API & System Notes
  - All endpoints must return relevant HTTP status codes:

  - 200 OK, 201 Created, 400 Bad Request, 401 Unauthorized, 404 Not Found, 500 Internal Server Error.

  - Use RESTful conventions for naming and actions.

  - Global error handler to ensure consistent responses.

  - Date and time values should use ISO 8601 format (UTC).

© 2025 ALX – All rights reserved.


