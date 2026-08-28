# Smart Service Marketplace — Database Design

## 1. Database Overview

The Smart Service Marketplace uses a relational database to store users, providers, services, categories, availability, bookings, reviews, and notifications.

Database:

MySQL

Backend ORM:

Django ORM

---

## 2. Entity Relationship Overview

The main entities are:

1. User
2. Provider Profile
3. Category
4. Service
5. Availability
6. Booking
7. Review
8. Notification

Relationship overview:

User
 |
 +---- Customer
 |
 +---- Provider Profile
          |
          +---- Services
          |
          +---- Availability

Category
 |
 +---- Services

Customer
 |
 +---- Bookings
          |
          +---- Service
          |
          +---- Provider

Booking
 |
 +---- Review

User
 |
 +---- Notifications

---

# 3. User Table

Table name:

`users`

Purpose:

Stores authentication and common information for all users.

| Field | Type | Key | Description |
|---|---|---|---|
| id | BIGINT | PK | Unique user ID |
| username | VARCHAR(150) | UNIQUE | Username |
| email | VARCHAR(254) | UNIQUE | Email address |
| password | VARCHAR(255) | | Hashed password |
| first_name | VARCHAR(100) | | First name |
| last_name | VARCHAR(100) | | Last name |
| phone | VARCHAR(20) | | Phone number |
| role | VARCHAR(20) | | CUSTOMER / PROVIDER / ADMIN |
| is_active | BOOLEAN | | Account status |
| created_at | DATETIME | | Account creation time |
| updated_at | DATETIME | | Last update time |

Primary Key:

`id`

---

# 4. Provider Profile Table

Table name:

`provider_profiles`

Purpose:

Stores additional information about service providers.

| Field | Type | Key | Description |
|---|---|---|---|
| id | BIGINT | PK | Provider profile ID |
| user_id | BIGINT | FK | Related user |
| business_name | VARCHAR(200) | | Business/provider name |
| bio | TEXT | | Provider description |
| experience_years | INT | | Years of experience |
| location | VARCHAR(255) | | Service location |
| profile_image | VARCHAR(255) | | Profile image |
| verification_status | VARCHAR(20) | | PENDING / APPROVED / REJECTED |
| created_at | DATETIME | | Creation time |
| updated_at | DATETIME | | Last update |

Relationship:

One User → One Provider Profile

---

# 5. Category Table

Table name:

`categories`

Purpose:

Stores service categories.

| Field | Type | Key | Description |
|---|---|---|---|
| id | BIGINT | PK | Category ID |
| name | VARCHAR(100) | UNIQUE | Category name |
| description | TEXT | | Category description |
| image | VARCHAR(255) | | Category image |
| is_active | BOOLEAN | | Category status |
| created_at | DATETIME | | Creation time |

Examples:

- Home Repair
- Computer Services
- Beauty
- Cleaning
- Tutoring
- Photography
- Plumbing
- Electrical

---

# 6. Service Table

Table name:

`services`

Purpose:

Stores services offered by providers.

| Field | Type | Key | Description |
|---|---|---|---|
| id | BIGINT | PK | Service ID |
| provider_id | BIGINT | FK | Service provider |
| category_id | BIGINT | FK | Service category |
| title | VARCHAR(200) | | Service title |
| description | TEXT | | Service description |
| price | DECIMAL(10,2) | | Service price |
| duration_minutes | INT | | Estimated duration |
| location | VARCHAR(255) | | Service location |
| image | VARCHAR(255) | | Service image |
| is_active | BOOLEAN | | Service availability |
| created_at | DATETIME | | Creation time |
| updated_at | DATETIME | | Last update |

Relationships:

Provider Profile → Many Services

Category → Many Services

---

# 7. Availability Table

Table name:

`availability`

Purpose:

Stores available appointment slots for services.

| Field | Type | Key | Description |
|---|---|---|---|
| id | BIGINT | PK | Availability ID |
| service_id | BIGINT | FK | Related service |
| date | DATE | | Available date |
| start_time | TIME | | Start time |
| end_time | TIME | | End time |
| is_available | BOOLEAN | | Slot availability |
| created_at | DATETIME | | Creation time |

Relationship:

Service → Many Availability Slots

---

# 8. Booking Table

Table name:

`bookings`

Purpose:

Stores customer service bookings.

| Field | Type | Key | Description |
|---|---|---|---|
| id | BIGINT | PK | Booking ID |
| customer_id | BIGINT | FK | Customer |
| service_id | BIGINT | FK | Booked service |
| availability_id | BIGINT | FK | Selected slot |
| booking_date | DATE | | Booking date |
| start_time | TIME | | Booking start |
| end_time | TIME | | Booking end |
| total_price | DECIMAL(10,2) | | Booking price |
| status | VARCHAR(20) | | Booking status |
| customer_note | TEXT | | Customer instructions |
| created_at | DATETIME | | Booking creation |
| updated_at | DATETIME | | Last update |

Booking statuses:

- PENDING
- CONFIRMED
- REJECTED
- IN_PROGRESS
- COMPLETED
- CANCELLED

Relationships:

Customer → Many Bookings

Service → Many Bookings

Availability → Booking

---

# 9. Review Table

Table name:

`reviews`

Purpose:

Stores customer ratings and reviews for completed services.

| Field | Type | Key | Description |
|---|---|---|---|
| id | BIGINT | PK | Review ID |
| booking_id | BIGINT | FK | Related booking |
| customer_id | BIGINT | FK | Customer |
| service_id | BIGINT | FK | Reviewed service |
| rating | INT | | Rating from 1 to 5 |
| comment | TEXT | | Review comment |
| created_at | DATETIME | | Review creation |
| updated_at | DATETIME | | Last update |

Relationships:

Booking → One Review

Customer → Many Reviews

Service → Many Reviews

Rules:

- Rating must be between 1 and 5.
- Only completed bookings can be reviewed.
- A booking can have only one review.

---

# 10. Notification Table

Table name:

`notifications`

Purpose:

Stores system notifications for users.

| Field | Type | Key | Description |
|---|---|---|---|
| id | BIGINT | PK | Notification ID |
| user_id | BIGINT | FK | Notification recipient |
| title | VARCHAR(200) | | Notification title |
| message | TEXT | | Notification message |
| notification_type | VARCHAR(50) | | Notification category |
| is_read | BOOLEAN | | Read status |
| created_at | DATETIME | | Creation time |

Examples:

- Booking Created
- Booking Confirmed
- Booking Rejected
- Booking Cancelled
- Service Completed
- New Review

---

# 11. Relationships

## User → Provider Profile

One-to-One

A provider user can have one provider profile.

---

## Provider Profile → Service

One-to-Many

One provider can offer multiple services.

---

## Category → Service

One-to-Many

One category can contain multiple services.

---

## Service → Availability

One-to-Many

A service can have multiple available slots.

---

## Customer → Booking

One-to-Many

A customer can create multiple bookings.

---

## Service → Booking

One-to-Many

A service can have multiple bookings.

---

## Booking → Review

One-to-One

A completed booking can have one review.

---

## User → Notification

One-to-Many

A user can receive multiple notifications.

---

# 12. Database Relationship Diagram

```text
                         +----------------+
                         |      USER      |
                         +-------+--------+
                                 |
                       +---------+---------+
                       |                   |
                  CUSTOMER              PROVIDER
                       |                   |
                       |                   |
                       |            +------v-------+
                       |            |   PROVIDER   |
                       |            |   PROFILE    |
                       |            +------+--------+
                       |                   |
                       |                   |
                       |            +------v-------+
                       |            |   SERVICE    |
                       |            +------+--------+
                       |                   |
                 +-----v-----+       +-----v-------+
                 |  BOOKING  |       | AVAILABILITY|
                 +-----+-----+       +-------------+
                       |
                       |
                 +-----v-----+
                 |  REVIEW   |
                 +-----------+

+-------------+
|  CATEGORY   |
+------+------+
       |
       |
       v
    SERVICE

USER
 |
 v
NOTIFICATION