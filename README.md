# 🎟️ Event Registration & Ticketing System on AWS
A fully cloud-native and serverless platform that enables event organizers to create and manage events, while allowing attendees to browse events, register, and download tickets. The system automatically generates PDF tickets with QR codes, secures access using Cognito authentication, and stores data in DynamoDB — all deployed scalably on AWS.

--- 

## 🚀 Features
| Role          | Capabilities                                                             |
| ------------- | ------------------------------------------------------------------------ |
| **Organizer** | Create/manage events, view stats, track registrations                    |
| **Attendee**  | Browse events, register, download PDF tickets                            |
| **System**    | Auto-generated QR code tickets, secure auth, scalable serverless backend |

- ✔ Secure signup & login with user roles (Organizer / Attendee)
- ✔ Automatic ticket PDF + QR code generation
- ✔ Serverless architecture using AWS Lambda & DynamoDB
- ✔ High-availability hosting using S3 + CloudFront/Amplify
- ✔ Optional Stripe & SES integration

---

## 🏗️ System Architecture
| Layer              | AWS Service                | Purpose                        |
| ------------------ | -------------------------- | ------------------------------ |
| Frontend           | S3 / CloudFront / Amplify  | Host UI                        |
| Authentication     | Cognito User Pool + Groups | Role-based access              |
| API Gateway (REST) | Event & ticket operations  |                                |
| API Gateway (HTTP) | Login/Signup               |                                |
| Compute            | AWS Lambda                 | Core business logic            |
| Database           | DynamoDB                   | Events, registrations, tickets |
| Storage            | S3                         | QR + PDF tickets               |
| Optional           | SES / Stripe               | Ticket emails / payments       |

<img width="554" height="491" alt="image" src="https://github.com/user-attachments/assets/1476303c-8b18-4e3f-97de-607a85fecad9" />

<img width="738" height="330" alt="image" src="https://github.com/user-attachments/assets/939ac34a-c787-4138-9465-0ebe6ed65b85" />

<img width="561" height="371" alt="image" src="https://github.com/user-attachments/assets/e375e036-afeb-43e8-8c61-00328af7feb6" />

---

## 🗄️ DynamoDB Tables

### Events Table
Stores event details with seating & organizer metadata.
| Attribute       | Type   |
| --------------- | ------ |
| eventId (PK)    | String |
| organizerId     | String |
| name            | String |
| description     | String |
| date            | String |
| location        | String |
| maxSeats        | Number |
| registeredCount | Number |
| createdAt       | String |

### Registrations Table
| Attribute           | Type   |
| ------------------- | ------ |
| registrationId (PK) | String |
| eventId             | String |
| userId              | String |
| ticketId            | String |
| status              | String |
| timestamp           | String |

### Tickets Table
| Attribute     | Type    |
| ------------- | ------- |
| ticketId (PK) | String  |
| eventId       | String  |
| userId        | String  |
| qrCode        | String  |
| pdfUrl        | String  |
| isUsed        | Boolean |
| expiry        | String  |

---

## 🔐 Authentication Flow (Cognito + HTTP API)
User roles: Organizer / Attendee
/signup → Assign user group
/login → Return JWT
→ JWT used for all protected REST API calls

---

## 🔗 REST API Endpoints

### Organizer
| Endpoint           | Method | Description            |
| ------------------ | ------ | ---------------------- |
| `/createEvent`     | POST   | Create event           |
| `/stats/{eventId}` | POST   | Fetch event statistics |

### Attendee
| Endpoint                     | Method | Description                 |
| ---------------------------- | ------ | --------------------------- |
| `/listEvents`                | GET    | View all events             |
| `/register/{eventId}/{name}` | POST   | Register for event          |
| `/validateTicket/{ticketId}` | POST   | Entry verification QR scan  |

---

## 🎫 Ticket Flow (Automatic)

1️⃣ Attendee registers  
2️⃣ Lambda inserts record in DynamoDB  
3️⃣ TicketId created → QR generated & stored in S3  
4️⃣ PDF ticket generated & uploaded  
5️⃣ Pre-signed URL returned to user  

Ticket PDF contains:
- Event details
- Attendee name
- QR code
- Ticket ID

---

## 📡 QR Code Validation

At entry, staff scans QR & requests:
/validateTicket/{ticketId} → Result returned:

🔹 Valid & unused → Mark used & allow entry  
🔹 Already used → Reject  
🔹 Expired / Invalid → Reject  

---

## 📌 Tech Stack
| Category | Tools                     |
| -------- | ------------------------- |
| Frontend | S3 / CloudFront / Amplify |
| Backend  | AWS Lambda + API Gateway  |
| Auth     | Cognito                   |
| Database | DynamoDB                  |
| Storage  | S3                        |
| Optional | Stripe / SES              |
