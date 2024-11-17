# **API Documentation**

## **Overview**
This document outlines the REST API endpoints for the Event Booking Platform. It includes the routes, required parameters, and expected responses for each entity and functionality.

---

## **Base URL**
- **Development**: `http://localhost:8000/api`
- **Production**: `https://yourdomain.com/api`

---

## **General Conventions**
### **Headers**
- `Authorization: Bearer <token>` (required for authenticated routes)
- `Content-Type: application/json`

### **Response Format**
All responses will return in JSON format:
```json
{
  "success": true,
  "message": "Operation completed successfully",
  "data": { ... }
}
```
If an error occurs:
```json
{
  "success": false,
  "message": "Error message explaining what went wrong"
}
```

---

## **Endpoints**

### **1. User Management**

#### **Authentication**
- **Register User**
  - **POST /auth/register**
    Registers a new user (NormalUser, EventCreator, or Admin).
    **Request Body**:
    ```json
    {
      "name": "John Doe",
      "email": "john@example.com",
      "password": "securePassword",
      "role": "NormalUser"
    }
    ```
    **Response**:
    ```json
    {
      "id": "UUID",
      "name": "John Doe",
      "email": "john@example.com",
      "role": "NormalUser",
      "createdAt": "2024-11-13T12:00:00Z"
    }
    ```

- **Login User**
  - **POST /auth/login**
    Logs in an existing user.
    **Request Body**:
    ```json
    {
      "email": "john@example.com",
      "password": "securePassword"
    }
    ```
    **Response**:
    ```json
    {
      "token": "jwtToken",
      "userId": "UUID",
      "role": "NormalUser"
    }
    ```

- **Logout User**
  - **POST /auth/logout**
    Logs out the current user.
    **Response**:
    ```json
    {
      "message": "Logout successful"
    }
    ```

#### **User Profile**
- **Get Profile**
  - **GET /users/:userId**
    Retrieves user profile details.
    **Response**:
    ```json
    {
      "id": "UUID",
      "name": "John Doe",
      "email": "john@example.com",
      "profilePicture": "https://example.com/image.jpg",
      "role": "NormalUser",
      "createdAt": "2024-11-13T12:00:00Z",
      "updatedAt": "2024-11-14T14:00:00Z"
    }
    ```

- **Update Profile**
  - **PATCH /users/:userId**
    Updates user profile information.
    **Request Body**:
    ```json
    {
      "name": "Jane Doe",
      "profilePicture": "https://example.com/new-image.jpg"
    }
    ```
    **Response**:
    ```json
    {
      "id": "UUID",
      "name": "Jane Doe",
      "email": "john@example.com",
      "profilePicture": "https://example.com/new-image.jpg",
      "updatedAt": "2024-11-15T10:00:00Z"
    }
    ```

- **Delete Account**
  - **DELETE /users/:userId**
    Deletes a user account.
    **Response**:
    ```json
    {
      "message": "User deleted successfully"
    }
    ```

---

### **2. Event Management**

#### **Event Operations**
- **Create Event**
  - **POST /events**
    Creates a new event. Requires `EventCreator` role.
    **Request Body**:
    ```json
    {
      "title": "Music Concert",
      "description": "An exciting music event.",
      "location": "123 Main St, City",
      "geoCoordinates": "45.76,-122.12",
      "dateTime": "2024-11-20T14:00:00Z",
      "price": 50,
      "capacity": 200,
      "isOnline": false,
      "categoryId": "UUID"
    }
    ```
    **Response**:
    ```json
    {
      "id": "UUID",
      "title": "Music Concert",
      "createdAt": "2024-11-13T12:00:00Z"
    }
    ```

- **List Events**
  - **GET /events**
    Retrieves all events with optional filters.
    **Query Parameters**:
    - `?category=UUID`
    - `?location=City`
    - `?date=YYYY-MM-DD`

- **Event Details**
  - **GET /events/:eventId**
    Retrieves details of a specific event.

- **Update Event**
  - **PATCH /events/:eventId**
    Updates event details (requires ownership).

- **Delete Event**
  - **DELETE /events/:eventId**
    Deletes an event (requires admin or ownership).

---

### **3. Cart Management**

- **Add to Cart**
  - **POST /cart/add**
    Adds an event to the cart.
    **Request Body**:
    ```json
    {
      "eventId": "UUID",
      "quantity": 2
    }
    ```
    **Response**:
    ```json
    {
      "message": "Event added to cart successfully"
    }
    ```

- **View Cart**
  - **GET /cart**
    Retrieves the user’s cart.

- **Update Cart**
  - **PATCH /cart/update/:cartId**
    Updates ticket quantities.

- **Remove from Cart**
  - **DELETE /cart/remove/:cartId**
    Removes an item from the cart.

---

### **4. Ticket Booking**

- **Checkout**
  - **POST /tickets/checkout**
    Processes checkout for cart items.

- **List Tickets**
  - **GET /tickets**
    Retrieves all tickets for the user.

---

### **5. Reviews and Ratings**

- **Submit Review**
  - **POST /reviews**
    Submits a review for an event.
    **Request Body**:
    ```json
    {
      "eventId": "UUID",
      "rating": 5,
      "comment": "Amazing event!"
    }
    ```

- **Get Event Reviews**
  - **GET /reviews/:eventId**
    Retrieves all reviews for an event.

---

### **6. Admin Operations**

- **View Analytics**
  - **GET /admin/analytics**
    Retrieves platform metrics (users, events, revenue).

- **Manage Users**
  - **PATCH /admin/users/:userId/status**
    Updates user status (e.g., ban/unban).
