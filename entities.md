---

## 1. Users
### Description
Represents the users of the system.

### Attributes
- `id`: Integer, primary key.
- `first_name`: String, not null.
- `last_name`: String.
- `username`: String, unique, not null.
- `email`: String, unique, not null.
- `password`: String, not null.
- `profile_picture`: Text, optional.
- `location`: String, optional.
- `created_at`: Timestamp, default to `CURRENT_TIMESTAMP`.
- `updated_at`: Timestamp, default to `CURRENT_TIMESTAMP`.

### Relationships
- One-to-many with `user_roles` (via `user_id`).
- One-to-many with `reviews` (via `user_id`).
- One-to-many with `notifications` (via `user_id`).
- One-to-many with `invoices` (via `user_id`).
- One-to-many with `tickets` (via `user_id`).
- One-to-many with `place_creator_applications` (via `user_id`).

---

## 2. Roles
### Description
Defines roles that can be assigned to users.

### Attributes
- `id`: Integer, primary key.
- `name`: String, not null.

### Relationships
- One-to-many with `user_roles` (via `role_id`).

---

## 3. User Roles
### Description
Associates users with roles.

### Attributes
- `user_id`: Integer, foreign key referencing `users.id`.
- `role_id`: Integer, foreign key referencing `roles.id`.
- `created_at`: Timestamp, default to `CURRENT_TIMESTAMP`.

### Relationships
- Many-to-one with `users` (via `user_id`).
- Many-to-one with `roles` (via `role_id`).

---

## 4. Places
### Description
Represents locations where events can be held.

### Attributes
- `id`: Integer, primary key.
- `description`: Text, not null.
- `title`: String, not null.
- `capacity`: Integer, not null.
- `price`: Decimal(10,2), optional.
- `created_at`: Timestamp, default to `CURRENT_TIMESTAMP`.
- `updated_at`: Timestamp, default to `CURRENT_TIMESTAMP`.
- `user_id`: Integer, foreign key referencing `users.id`.

### Relationships
- Many-to-one with `users` (via `user_id`).
- One-to-many with `place_locations` (via `place_id`).
- One-to-many with `reviews` (via `place_id`).
- One-to-many with `events` (via `place_id`).

---

## 5. Place Locations
### Description
Defines the geographical locations of places.

### Attributes
- `id`: Integer, primary key.
- `place_id`: Integer, foreign key referencing `places.id`.
- `location`: String, not null.

### Relationships
- Many-to-one with `places` (via `place_id`).

---

## 6. Reviews
### Description
Captures user reviews for places.

### Attributes
- `id`: Integer, primary key.
- `user_id`: Integer, foreign key referencing `users.id`.
- `place_id`: Integer, foreign key referencing `places.id`.
- `comment`: Text, optional.
- `rating`: Integer, value between 1 and 5.
- `created_at`: Timestamp, default to `CURRENT_TIMESTAMP`.

### Relationships
- Many-to-one with `users` (via `user_id`).
- Many-to-one with `places` (via `place_id`).

---

## 7. Events
### Description
Represents events that take place at specific locations.

### Attributes
- `id`: Integer, primary key.
- `title`: String, not null.
- `description`: Text, optional.
- `start_time`: Timestamp, not null.
- `end_time`: Timestamp, not null.
- `created_at`: Timestamp, default to `CURRENT_TIMESTAMP`.
- `updated_at`: Timestamp, default to `CURRENT_TIMESTAMP`.
- `place_id`: Integer, foreign key referencing `places.id`.

### Relationships
- Many-to-one with `places` (via `place_id`).
- One-to-many with `user_events` (via `event_id`).
- One-to-many with `tickets` (via `event_id`).

---

## 8. User Events
### Description
Tracks which users are attending which events.

### Attributes
- `user_id`: Integer, foreign key referencing `users.id`.
- `event_id`: Integer, foreign key referencing `events.id`.
- `created_at`: Timestamp, default to `CURRENT_TIMESTAMP`.

### Relationships
- Many-to-one with `users` (via `user_id`).
- Many-to-one with `events` (via `event_id`).

---

## 9. Tickets
### Description
Represents tickets purchased by users for events.

### Attributes
- `id`: Integer, primary key.
- `status`: String, default `'pending'`.
- `created_at`: Timestamp, default to `CURRENT_TIMESTAMP`.
- `user_id`: Integer, foreign key referencing `users.id`.
- `event_id`: Integer, foreign key referencing `events.id`.
- `invoice_id`: Integer, foreign key referencing `invoices.id`.

### Relationships
- Many-to-one with `users` (via `user_id`).
- Many-to-one with `events` (via `event_id`).
- Many-to-one with `invoices` (via `invoice_id`).

---

## 10. Notifications
### Description
Tracks messages or alerts sent to users.

### Attributes
- `id`: Integer, primary key.
- `message`: Text, not null.
- `created_at`: Timestamp, default to `CURRENT_TIMESTAMP`.
- `user_id`: Integer, foreign key referencing `users.id`.

### Relationships
- Many-to-one with `users` (via `user_id`).

---

## 11. Invoices
### Description
Captures payment details for purchases.

### Attributes
- `id`: Integer, primary key.
- `status`: String, default `'unpaid'`.
- `amount`: Decimal(10,2), not null.
- `transaction_id`: String, unique.
- `created_at`: Timestamp, default to `CURRENT_TIMESTAMP`.
- `user_id`: Integer, foreign key referencing `users.id`.

### Relationships
- Many-to-one with `users` (via `user_id`).
- One-to-many with `tickets` (via `invoice_id`).
- One-to-many with `payment_methods` (via `invoice_id`).

---

## 12. Payment Methods
### Description
Details payment methods for invoices.

### Attributes
- `id`: Integer, primary key.
- `name`: String, not null.
- `details`: Text, optional.
- `invoice_id`: Integer, foreign key referencing `invoices.id`.

### Relationships
- Many-to-one with `invoices` (via `invoice_id`).

---

## 13. Place Creator Applications
### Description
Tracks applications by users to create new places.

### Attributes
- `id`: Integer, primary key.
- `user_id`: Integer, foreign key referencing `users.id`.
- `place_name`: String, not null.
- `status`: String, default `'pending'`.
- `location`: String, optional.
- `capacity`: Integer, not null.
- `created_at`: Timestamp, default to `CURRENT_TIMESTAMP`.
- `updated_at`: Timestamp, default to `CURRENT_TIMESTAMP`.

### Relationships
- Many-to-one with `users` (via `user_id`).

---
