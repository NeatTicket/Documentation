---

### **1. Normal User (Guest)**

#### **Description**:
- A regular user with limited permissions who can browse, interact with, and book events without advanced management capabilities.

#### **Permissions**:
- Browse available events and places.
- Book tickets for events.
- Manage their profile (update personal details, view history, etc.).
- Receive notifications about booked events or changes.
- Search for events by location, category, and keyword.
- View their booking history and ticket details.

---

### **2. Event Creator**

#### **Description**:
- A user responsible for organizing and managing events hosted at places they have booked.

#### **Permissions**:
- Create, update, and cancel events.
- View details about attendees (e.g., number of tickets sold, user details as necessary).
- Communicate with attendees via notifications or event updates.
- Set ticket prices, event descriptions, and dates.
- Choose whether the event is online or offline.
- Manage event capacity and waiting lists.
- Access event analytics, including number of attendees, ticket sales, and revenue.

---

### **3. Place Owner**

#### **Description**:
- A user who owns or manages a place and can list it on the platform for hosting events.

#### **Permissions**:
- Add, update, or delete places they own or manage.
- View and manage booking requests for their places.
- Approve or reject booking requests from event creators.
- Communicate with event creators about bookings or venue requirements.
- View booking statistics and analytics for their venue (e.g., revenue, booking frequency).

---

### **4. Admin**

#### **Description**:
- A platform administrator responsible for managing users, monitoring activities, and ensuring the smooth operation of the platform.

#### **Permissions**:
- Approve or reject new event creator or place owner applications.
- View and manage all users and their roles.
- Monitor platform activities, including event and place statistics, bookings, and revenue.
- Manage system-wide notifications (e.g., platform updates, critical issues).
- Resolve disputes between users, event creators, and place owners.
- Manage user accounts, including disabling problematic accounts or roles.
- Generate platform analytics (e.g., user registration, revenue generation, event activity).
- Perform data backups and ensure system security.

---

### **Role Assignments**

- **User Roles**:
  - **Guest**: Default role for any new user; can browse events and places, and book tickets.
  - **Event Creator**: Granted upon request and approval, with permission to create, manage, and update events.
  - **Place Owner**: Granted upon request and approval, with permission to list, manage, and approve bookings for places.
  - **Admin**: Manually assigned by platform super-admin with full permissions to oversee all user activities and platform management.

---

### **Key Relationships**

- **Normal User**: Can book events organized by **Event Creators** at places owned or managed by **Place Owners**.
- **Event Creators**: Can organize and manage events at venues owned or managed by **Place Owners**.
- **Place Owners**: Can list their places for event creation by **Event Creators**.
- **Admins**: Oversee and manage all relationships and activities between **Normal Users**, **Event Creators**, and **Place Owners**, ensuring no conflicts arise and all roles are appropriately managed.
