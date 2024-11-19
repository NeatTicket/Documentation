# Entities Documentation

This document outlines the core entities and their relationships for the **NeatTicket Event Booking Platform**. It includes essential tables for users, roles, events, places, tickets, invoices, notifications, and applications for place creators.

---

## **1. Core Tables**

### **User** (Users)
Stores details of users and their roles.

| **Field**      | **Type**      | **Description**                         |
|----------------|---------------|-----------------------------------------|
| `id`           | Primary Key   | Unique identifier for a user            |
| `firstName`    | String        | User's first name                       |
| `lastName`     | String        | User's last name                        |
| `email`        | String        | User's email address (required)         |
| `password`     | String        | Password (hashed, for security)         |
| `role_id`      | Foreign Key   | Refers to the **Role** table            |
| `created_at`   | Timestamp     | Date and time when the account was created |
| `updated_at`   | Timestamp     | Last account update timestamp           |

---

### **Role** (Roles)
Defines user roles and associated permissions.

| **Field**      | **Type**      | **Description**                         |
|----------------|---------------|-----------------------------------------|
| `id`           | Primary Key   | Unique identifier for the role          |
| `name`         | String        | Role name (`Admin`, `Place Owner`, etc.) |
| `permissions`  | JSON          | Role-specific permissions (flexible)    |

---

### **Place** (Places)
Stores venue details managed by place owners.

| **Field**      | **Type**      | **Description**                         |
|----------------|---------------|-----------------------------------------|
| `id`           | Primary Key   | Unique identifier for a place           |
| `name`         | String        | Place name                              |
| `location`     | String        | Address of the place                    |
| `capacity`     | Integer       | Maximum capacity of the venue           |
| `description`  | Text          | Detailed description of the place       |
| `price`        | Float         | Booking price                           |
| `owner_id`     | Foreign Key   | Refers to the **User** who owns the place |
| `created_at`   | Timestamp     | Date and time when the place was added  |
| `updated_at`   | Timestamp     | Last modification timestamp             |

---

### **Event** (Events)
Captures information about events hosted at venues.

| **Field**      | **Type**      | **Description**                         |
|----------------|---------------|-----------------------------------------|
| `id`           | Primary Key   | Unique identifier for an event          |
| `title`        | String        | Event name or title                     |
| `description`  | Text          | Description of the event                |
| `place_id`     | Foreign Key   | Refers to the **Place** table           |
| `creator_id`   | Foreign Key   | Refers to the **User** who created the event |
| `start_time`   | Timestamp     | Event start time                        |
| `end_time`     | Timestamp     | Event end time                          |
| `created_at`   | Timestamp     | Date when the event was created         |
| `updated_at`   | Timestamp     | Last update timestamp                   |

---

### **Ticket** (Tickets)
Represents tickets booked by users for specific events.

| **Field**      | **Type**      | **Description**                         |
|----------------|---------------|-----------------------------------------|
| `id`           | Primary Key   | Unique ticket identifier                |
| `event_id`     | Foreign Key   | Refers to the **Event** table           |
| `buyer_id`     | Foreign Key   | Refers to the **User** who purchased the ticket |
| `status`       | Enum          | Status (`booked`, `canceled`)           |
| `price`        | Float         | Ticket price                            |
| `created_at`   | Timestamp     | Timestamp when the ticket was purchased |

---

### **Invoice** (Invoices)
Tracks payment and billing details for ticket purchases.

| **Field**      | **Type**      | **Description**                         |
|----------------|---------------|-----------------------------------------|
| `id`           | Primary Key   | Unique identifier for the invoice       |
| `ticket_id`    | Foreign Key   | Refers to the **Ticket** table          |
| `amount`       | Float         | Total amount paid                       |
| `status`       | Enum          | Payment status (`paid`, `pending`)      |
| `transaction_id`| String       | Reference to an external payment gateway |
| `created_at`   | Timestamp     | Date the invoice was generated          |

---

### **Notification** (Notifications)
Tracks notifications sent to users.

| **Field**      | **Type**      | **Description**                         |
|----------------|---------------|-----------------------------------------|
| `id`           | Primary Key   | Unique identifier for the notification  |
| `message`      | Text          | Notification message content            |
| `user_id`      | Foreign Key   | Refers to the **User** table            |
| `status`       | Enum          | Status (`sent`, `read`)                 |
| `created_at`   | Timestamp     | Date and time of notification creation  |

---

### **PlaceCreatorApplication** (PlaceCreatorApplications)
Manages applications to create or manage venues.

| **Field**      | **Type**      | **Description**                         |
|----------------|---------------|-----------------------------------------|
| `id`           | Primary Key   | Unique identifier for the application   |
| `user_id`      | Foreign Key   | Refers to the **User** table            |
| `place_name`   | String        | Name of the proposed place              |
| `status`       | Enum          | Application status (`pending`, `approved`, `rejected`) |
| `created_at`   | Timestamp     | Date of application submission          |
| `updated_at`   | Timestamp     | Last update timestamp                   |
| `reason`       | Text          | Reason for rejection (if applicable)    |
| `description`  | Text          | Details provided by the applicant       |
| `documents`    | String (Path) | Path or URL to supporting documents      |
| `approved_by`  | Foreign Key   | Refers to the admin who reviewed the application |

---

## **2. Relationships**

### Key Relationships:
1. **User ↔ Role**: **N:1**  
   - Each user has one role, but roles can apply to multiple users.

2. **User ↔ Place**: **1:N**  
   - A user (place owner) can manage multiple venues.

3. **Place ↔ Event**: **1:N**  
   - A venue can host multiple events.

4. **Event ↔ Ticket**: **1:N**  
   - Each event can sell multiple tickets.

5. **Ticket ↔ User**: **N:1**  
   - Each ticket is tied to a specific user (buyer).

6. **Ticket ↔ Invoice**: **1:1**  
   - Each ticket generates a single invoice.

7. **Notification ↔ User**: **N:1**  
   - Notifications are sent to users based on actions or updates.

8. **User ↔ PlaceCreatorApplication**: **1:N**  
   - A user can submit multiple applications to create or manage venues.

--- 
