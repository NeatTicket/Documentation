### **API Documentation**

This REST API is designed for the **Event Booking Platform**, enabling various functionalities for users, events, tickets, and administrative operations. Below is a detailed guide for each endpoint, including usage, parameters, and expected responses.

---

### **1. Overview**

#### **Base URL**
- Development: `http://localhost:8000/api`
- Production: `https://yourdomain.com/api`

#### **General Conventions**
- **Headers**
  - `Authorization: Bearer <token>` (for authenticated routes)
  - `Content-Type: application/json`

- **Response Format**
  All responses return JSON:
  ```json
  {
    "success": true,
    "message": "Operation completed successfully",
    "data": { ... }
  }
  ```
  Errors return:
  ```json
  {
    "success": false,
    "message": "Error message explaining the issue"
  }
  ```

---

### **2. User Management**

#### **Authentication**
- **Register User**  
  **POST** `/auth/register`  
  Registers a new user.
  ```json
  {
    "name": "John Doe",
    "email": "john@example.com",
    "password": "securePassword",
    "role": "NormalUser"
  }
  ```

- **Login User**  
  **POST** `/auth/login`  
  Authenticates and returns a token.
  ```json
  {
    "email": "john@example.com",
    "password": "securePassword"
  }
  ```

- **Logout User**  
  **POST** `/auth/logout`  
  Ends the user's session.

#### **Profile Management**
- **Get User Profile**  
  **GET** `/users/:userId`

- **Update Profile**  
  **PATCH** `/users/:userId`

- **Delete User Account**  
  **DELETE** `/users/:userId`

---

### **3. Event Management**

#### **CRUD Operations**
- **Create Event**  
  **POST** `/events`  
  Requires `EventCreator` role.
  ```json
  {
    "title": "Concert Name",
    "description": "Event description",
    "start_time": "2024-12-25T20:00:00",
    "end_time": "2024-12-25T22:00:00",
    "place_id": "UUID"
  }
  ```

- **List Events**  
  **GET** `/events`  
  Filters supported:
  - `?category=UUID`
  - `?location=City`
  - `?date=YYYY-MM-DD`

- **Get Event Details**  
  **GET** `/events/:eventId`

- **Update Event**  
  **PATCH** `/events/:eventId`

- **Delete Event**  
  **DELETE** `/events/:eventId`

---

### **4. Ticket Management**

#### **Operations**
- **Add Ticket**  
  **POST** `/tickets`
  ```json
  {
    "eventId": "UUID",
    "quantity": 2
  }
  ```

- **List User Tickets**  
  **GET** `/tickets`

- **Ticket Details**  
  **GET** `/tickets/:ticketId`

- **Cancel Ticket**  
  **DELETE** `/tickets/:ticketId`

---

### **5. Invoice Management**

#### **Operations**
- **Create Invoice**  
  **POST** `/invoices`
  ```json
  {
    "userId": "UUID",
    "amount": 100.0
  }
  ```

- **View Invoice**  
  **GET** `/invoices/:invoiceId`

- **List User Invoices**  
  **GET** `/invoices`

---

### **6. Notifications**

#### **Notification Operations**
- **List Notifications**  
  **GET** `/notifications`

- **Mark as Read**  
  **PATCH** `/notifications/:notificationId`

---

### **7. Admin Operations**

#### **Platform Management**
- **View Analytics**  
  **GET** `/admin/analytics`

- **Manage Users**  
  **PATCH** `/admin/users/:userId/status`
  ```json
  {
    "status": "active"
  }
  ```

#### **Event and Ticket Oversight**
- **Approve/Reject Events**  
  **PATCH** `/admin/events/:eventId/status`
  ```json
  {
    "status": "approved"
  }
  ```

- **Manage Payments**  
  **GET** `/admin/payments`

---

### **8. Use Case Examples**

#### **Booking an Event**
1. **Add Tickets**  
   User adds tickets to their cart:
   **POST** `/tickets/add`
   ```json
   {
     "eventId": "UUID",
     "quantity": 2
   }
   ```

2. **Checkout**  
   User proceeds to checkout:
   **POST** `/tickets/checkout`

3. **Invoice Generation**  
   Invoice is created automatically:
   **GET** `/invoices/:invoiceId`

#### **User Registration and Login**
- User registers via **POST** `/auth/register`
- Logs in using **POST** `/auth/login`
- Access profile using **GET** `/users/:userId`

---

### **9. Potential Enhancements**
1. **Detailed Error Codes**:  
   - Provide examples of specific error codes (e.g., `401 Unauthorized`, `404 Not Found`) and explanations for common failures.

2. **Rate Limiting Information**:  
   - Mention if there are limits to API requests (e.g., "Max 100 requests per minute").

3. **Webhooks (if applicable)**:  
   - Define webhook endpoints for real-time updates (e.g., payment status changes, ticket cancellations).

4. **Versioning**:  
   - Clarify if the API is versioned (e.g., `v1`, `v2`).

5. **Authentication Examples**:  
   - Include a sample token and usage example in headers.

6. **Expanded Use Cases**:  
   - Offer more user workflows, like creating venues or admin operations.

7. **Sample Errors**:  
   - Add examples of error responses for invalid requests (e.g., missing parameters, unauthorized actions). 

---
