# Smart Service Marketplace — Use Cases

## 1. Actors

The system has three primary actors:

- Customer
- Service Provider
- Administrator

---

## 2. Customer Use Cases

| ID | Use Case | Description |
|---|---|---|
| UC-C01 | Register | Customer creates an account |
| UC-C02 | Login | Customer logs into the system |
| UC-C03 | Manage Profile | Customer updates profile information |
| UC-C04 | Search Services | Customer searches for services |
| UC-C05 | Filter Services | Customer filters available services |
| UC-C06 | View Service | Customer views service details |
| UC-C07 | View Provider | Customer views provider profile |
| UC-C08 | Check Availability | Customer checks available appointment slots |
| UC-C09 | Book Service | Customer creates a service booking |
| UC-C10 | View Bookings | Customer views current and previous bookings |
| UC-C11 | Cancel Booking | Customer cancels an eligible booking |
| UC-C12 | Review Service | Customer submits rating and review |
| UC-C13 | Receive Notification | Customer receives booking updates |
| UC-C14 | Get Recommendations | Customer receives AI-based service recommendations |

---

## 3. Service Provider Use Cases

| ID | Use Case | Description |
|---|---|---|
| UC-P01 | Register | Provider creates an account |
| UC-P02 | Login | Provider logs into the system |
| UC-P03 | Manage Profile | Provider manages professional profile |
| UC-P04 | Add Service | Provider creates a service listing |
| UC-P05 | Edit Service | Provider updates service information |
| UC-P06 | Delete Service | Provider removes a service |
| UC-P07 | Set Availability | Provider manages available dates and times |
| UC-P08 | View Bookings | Provider views customer bookings |
| UC-P09 | Accept Booking | Provider accepts a booking |
| UC-P10 | Reject Booking | Provider rejects a booking |
| UC-P11 | Update Booking | Provider updates booking status |
| UC-P12 | View Reviews | Provider views customer reviews |
| UC-P13 | View Earnings | Provider views basic earnings information |

---

## 4. Administrator Use Cases

| ID | Use Case | Description |
|---|---|---|
| UC-A01 | Admin Login | Administrator securely logs in |
| UC-A02 | View Dashboard | Administrator views platform statistics |
| UC-A03 | Manage Customers | Administrator manages customer accounts |
| UC-A04 | Manage Providers | Administrator manages provider accounts |
| UC-A05 | Approve Provider | Administrator approves provider registration |
| UC-A06 | Reject Provider | Administrator rejects provider registration |
| UC-A07 | Manage Categories | Administrator manages service categories |
| UC-A08 | Manage Services | Administrator manages service listings |
| UC-A09 | Monitor Bookings | Administrator monitors platform bookings |
| UC-A10 | Manage Reviews | Administrator manages reported reviews |
| UC-A11 | View Reports | Administrator views marketplace reports |

---

# 5. Detailed Use Cases

## UC-C09 — Book Service

### Actor
Customer

### Preconditions
- Customer is registered.
- Customer is logged in.
- Service is available.
- Provider has an available time slot.

### Main Flow

1. Customer searches for a service.
2. Customer selects a service.
3. System displays service details.
4. Customer selects an available date.
5. Customer selects an available time slot.
6. Customer confirms booking.
7. System validates the booking.
8. System creates the booking.
9. System sends a booking notification.
10. Booking status becomes `Pending`.

### Alternative Flow

If the selected slot is no longer available:

1. System rejects the booking.
2. System asks the customer to select another slot.

### Postconditions

A booking record is created successfully.

---

## UC-P09 — Accept Booking

### Actor
Service Provider

### Preconditions
- Provider is logged in.
- A pending booking exists.

### Main Flow

1. Provider opens the booking dashboard.
2. System displays pending bookings.
3. Provider selects a booking.
4. Provider reviews booking details.
5. Provider accepts the booking.
6. System changes booking status to `Confirmed`.
7. Customer receives a notification.

### Postconditions

The booking is confirmed.

---

## UC-A05 — Approve Provider

### Actor
Administrator

### Preconditions

- Administrator is logged in.
- Provider registration is pending.

### Main Flow

1. Administrator opens provider management.
2. System displays pending providers.
3. Administrator selects a provider.
4. Administrator reviews provider information.
5. Administrator approves the provider.
6. System changes provider status to `Approved`.
7. Provider can publish services.

### Postconditions

Provider becomes an approved marketplace provider.

---

# 6. Booking Status Flow

The booking lifecycle is:

Pending
   ↓
Confirmed
   ↓
In Progress
   ↓
Completed

Possible alternative:

Pending → Rejected

Possible cancellation:

Pending → Cancelled
Confirmed → Cancelled