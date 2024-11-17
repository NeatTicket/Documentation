### **Complete Entities for the Event Booking System with Cart**

---

#### **User**
Represents a user of the platform, which can be a `NormalUser`, `EventCreator`, or `Admin`.

| **Attribute**      | **Type**      | **Description**                          |
|--------------------|---------------|------------------------------------------|
| `id`              | UUID          | Unique identifier for the user.          |
| `name`            | String        | Full name of the user.                   |
| `email`           | String        | Unique email address.                    |
| `password`        | String (Hashed) | User's hashed password.                  |
| `role`            | Enum          | User role (`NormalUser`, `EventCreator`, `Admin`). |
| `profilePicture`  | String (URL)  | URL for the user's profile picture (optional). |
| `location`        | String        | User's preferred location (optional).    |
| `createdAt`       | Timestamp     | When the user account was created.       |
| `updatedAt`       | Timestamp     | Last update time for the account.        |

---

#### **Event**
Represents an event created by an event creator.

| **Attribute**      | **Type**      | **Description**                          |
|--------------------|---------------|------------------------------------------|
| `id`              | UUID          | Unique identifier for the event.         |
| `title`           | String        | Name of the event.                       |
| `description`     | Text          | Detailed description of the event.       |
| `location`        | String        | Address or online meeting URL.           |
| `geoCoordinates`  | Object        | Latitude and longitude for geolocation.  |
| `dateTime`        | Timestamp     | Event's scheduled date and time.         |
| `creatorId`       | UUID          | Reference to the `EventCreator`.         |
| `price`           | Decimal       | Price per ticket (0 for free events).    |
| `capacity`        | Integer       | Maximum number of attendees.             |
| `attendeesCount`  | Integer       | Current number of attendees.             |
| `isOnline`        | Boolean       | Indicates if the event is online.        |
| `createdAt`       | Timestamp     | Event creation timestamp.                |
| `updatedAt`       | Timestamp     | Last update time for the event.          |

---

#### **Ticket**
Represents a ticket booked by a normal user for an event.

| **Attribute**      | **Type**      | **Description**                          |
|--------------------|---------------|------------------------------------------|
| `id`              | UUID          | Unique identifier for the ticket.        |
| `userId`          | UUID          | Reference to the `NormalUser`.           |
| `eventId`         | UUID          | Reference to the associated event.       |
| `paymentInvoice`  | String (URL)  | Invoice URL or reference.                |
| `status`          | Enum          | Booking status (`Confirmed`, `Cancelled`, etc.). |
| `createdAt`       | Timestamp     | Ticket creation timestamp.               |

---

#### **Cart**
Represents the cart for holding events before booking.

| **Attribute**      | **Type**      | **Description**                          |
|--------------------|---------------|------------------------------------------|
| `id`              | UUID          | Unique identifier for the cart item.     |
| `userId`          | UUID          | Reference to the `NormalUser`.           |
| `eventId`         | UUID          | Reference to the associated event.       |
| `quantity`        | Integer       | Number of tickets the user intends to book (default: 1). |
| `status`          | Enum          | Status of the item in the cart (`Pending`, `Reserved`, etc.). |
| `createdAt`       | Timestamp     | Timestamp of addition to the cart.       |
| `updatedAt`       | Timestamp     | Last update time for the cart.           |

---

#### **Review**
Represents feedback left by a user for an event.

| **Attribute**      | **Type**      | **Description**                          |
|--------------------|---------------|------------------------------------------|
| `id`              | UUID          | Unique identifier for the review.        |
| `eventId`         | UUID          | Reference to the associated event.       |
| `userId`          | UUID          | Reference to the `NormalUser`.           |
| `rating`          | Integer (1-5) | Numeric rating for the event.            |
| `comment`         | Text          | Optional textual feedback.               |
| `createdAt`       | Timestamp     | Review creation timestamp.               |

---

#### **Notification**
Represents notifications sent to users.

| **Attribute**      | **Type**      | **Description**                          |
|--------------------|---------------|------------------------------------------|
| `id`              | UUID          | Unique identifier for the notification.  |
| `recipientId`     | UUID          | Reference to the user receiving it.      |
| `message`         | Text          | Content of the notification.             |
| `createdAt`       | Timestamp     | Notification creation timestamp.         |

---

#### **Admin**
Represents an admin user managing the platform.

| **Attribute**      | **Type**      | **Description**                          |
|--------------------|---------------|------------------------------------------|
| `id`              | UUID          | Unique identifier for the admin.         |
| `email`           | String        | Admin email address (unique).            |
| `password`        | String (Hashed) | Admin hashed password.                   |
| `createdAt`       | Timestamp     | Account creation timestamp.              |
| `updatedAt`       | Timestamp     | Last account update time.                |

---

#### **Analytics**
Aggregated data for admin insights.

| **Attribute**      | **Type**      | **Description**                          |
|--------------------|---------------|------------------------------------------|
| `id`              | UUID          | Unique identifier for the analytics record. |
| `totalUsers`      | Integer       | Total number of registered users.        |
| `totalEvents`     | Integer       | Total number of events created.          |
| `totalRevenue`    | Decimal       | Total revenue generated from bookings.   |
| `createdAt`       | Timestamp     | Record creation timestamp.               |

---

### **Relationships**
1. **User**:
   - **NormalUser** can book multiple **Tickets** and have items in the **Cart** (One-to-Many).
   - **EventCreator** can create multiple **Events** (One-to-Many).
   - **Admin** manages all other entities.

2. **Event**:
   - Linked to one **EventCreator** (Many-to-One).
   - Can have multiple **Tickets**, **Cart Items**, and **Reviews** (One-to-Many).

3. **Cart**:
   - Links a **NormalUser** to multiple **Events** (One-to-Many).

4. **Review**:
   - Links a **NormalUser** to an **Event** (Many-to-One).

5. **Notification**:
   - Linked to a specific **User** (Many-to-One).

6. **Analytics**:
   - Summarizes the overall platform activity.

---

### **Entity-Relationship Diagram (Updated)**

```
+--------------------+         +---------------------+          +----------------------+
|       Admin        |<------->|        User         |<-------->|      NormalUser      |
+--------------------+         +---------------------+          +----------------------+
          ^                         |   |   |   |                     |
          |                         |   |   |   |                     |
          |                         v   v   v   v                     |
 +----------------+        +---------------------+           +------------------+
 |     Event      |<------->|        Cart         |<----------|    EventCreator  |
 +----------------+        +---------------------+           +------------------+
          |                             |                           |  
          v                             |                           v
 +-----------------+            +-------------+           +------------------+
 |   Categories    |            |   Payment   |           |   Notifications  |
 +-----------------+            +-------------+           +------------------+
          |
          v
 +-------------+
 |   Review    |
 +-------------+
```
