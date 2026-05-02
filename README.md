# Field Service Management (FSM) — Backend API

<p align="center">
  <img src="https://img.shields.io/badge/Node.js-20%2B-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" />
  <img src="https://img.shields.io/badge/Express.js-4.x-000000?style=for-the-badge&logo=express&logoColor=white" />
  <img src="https://img.shields.io/badge/PostgreSQL-16-4169E1?style=for-the-badge&logo=postgresql&logoColor=white" />
  <img src="https://img.shields.io/badge/Prisma-ORM-2D3748?style=for-the-badge&logo=prisma&logoColor=white" />
  <img src="https://img.shields.io/badge/Firebase-Admin-FFCA28?style=for-the-badge&logo=firebase&logoColor=black" />
</p>

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Environment Variables](#environment-variables)
  - [Database Setup](#database-setup)
  - [Running the Server](#running-the-server)
- [API Modules](#api-modules)
- [User Roles](#user-roles)
- [Project Structure](#project-structure)
- [Database Schema](#database-schema)
- [Contributing](#contributing)

---

## Overview

**R2A FSM** is a production-ready RESTful backend API for a **Field Service Management** platform. It manages the full lifecycle of field service operations — from customer service requests and technician dispatching, to payments, commissions, payouts, and real-time push notifications.

The system supports multiple user roles (Admin, Customer, Technician, Call Center Agent, Dispatcher), handles commission & wallet management, OTP-based authentication, SMS notifications, Firebase push notifications, and file uploads.

---

## Features

- 🔐 **JWT Authentication** — Secure access & refresh token flow with role-based authorization
- 📱 **OTP Verification** — Phone-number based OTP via BulkGate SMS API
- 🗂️ **Service Requests & Work Orders** — Full FSM lifecycle management
- 🚀 **Technician Dispatching** — Dispatcher and auto-dispatch support
- 💰 **Commission & Wallet System** — Per-technician commission rates, wallet balances, and payout requests
- 📊 **Reports & Admin Dashboard** — Stats, analytics, and audit logs
- 🔔 **Push Notifications** — Firebase Admin SDK (FCM) integration
- 📂 **File Uploads** — Technician profiles, ID cards, payment proofs, work completion photos
- 🌐 **Multi-language Support** — i18n-ready middleware (English / Bengali)
- 📍 **Location Tracking** — Real-time technician GPS location updates
- ⭐ **Reviews & Ratings** — Customer-to-technician review system
- 🧑‍💼 **Call Center Portal** — Dedicated endpoints for call center agents
- 🔧 **Specialization System** — Technician specializations and categories

---

## Tech Stack

| Layer       | Technology                      |
| ----------- | ------------------------------- |
| Runtime     | Node.js 20+ (ESM)               |
| Framework   | Express.js 4.x                  |
| ORM         | Prisma 6.x                      |
| Database    | PostgreSQL 16                   |
| Auth        | JSON Web Token (JWT) + bcryptjs |
| Push Notif. | Firebase Admin SDK (FCM)        |
| SMS / OTP   | BulkGate SMS API                |
| File Upload | Multer                          |
| Security    | Helmet, CORS                    |
| Logging     | Morgan                          |
| Dev Tool    | Nodemon                         |

---

## Architecture

```
Client (Mobile / Web / Admin Panel)
        │
        ▼
   Express Router
        │
        ▼
   Middleware Layer
   (Auth, Language, Validation, Error Handler)
        │
        ▼
   Controllers
        │
        ▼
   Services / Utils
        │
        ▼
   Prisma ORM
        │
        ▼
   PostgreSQL Database
```

---

## Getting Started

### Prerequisites

- **Node.js** v20 or higher
- **PostgreSQL** v14 or higher
- **npm** v9 or higher
- A **Firebase** project (for push notifications)
- A **BulkGate** account (for SMS/OTP)

---

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/your-org/r2a-server.git
cd r2a-server

# 2. Install dependencies
npm install
```

---

### Environment Variables

Copy the example environment file and fill in your values:

```bash
cp .env.example .env
```

| Variable                   | Description                                           |
| -------------------------- | ----------------------------------------------------- |
| `DATABASE_URL`           | PostgreSQL connection string                          |
| `JWT_SECRET`             | Secret key for access tokens                          |
| `JWT_REFRESH_SECRET`     | Secret key for refresh tokens                         |
| `PORT`                   | Server port (default:`5000`)                        |
| `NODE_ENV`               | `development` or `production`                     |
| `GOOGLE_MAPS_API_KEY`    | Google Maps API key for geocoding                     |
| `BULKGATE_SMS_APP_ID`    | BulkGate SMS App ID                                   |
| `BULKGATE_SMS_APP_TOKEN` | BulkGate SMS App Token                                |
| `BULKGATE_OTP_APP_ID`    | BulkGate OTP App ID                                   |
| `BULKGATE_OTP_APP_TOKEN` | BulkGate OTP App Token                                |
| `DEFAULT_COUNTRY_CODE`   | Country dial code (e.g.,`880` for Bangladesh)       |
| `FIREBASE_PROJECT_ID`    | Firebase project ID                                   |
| `FIREBASE_PRIVATE_KEY`   | Firebase service account private key                  |
| `FIREBASE_CLIENT_EMAIL`  | Firebase service account email                        |
| `MAX_FILE_SIZE`          | Max upload size in bytes (default:`5242880` = 5 MB) |
| `UPLOAD_PATH`            | Local upload directory (default:`uploads/`)         |
| `WEEKLY_PAYOUT_DAY`      | Day of week for payouts (0=Sun, 1=Mon, …)            |
| `SMTP_HOST`              | SMTP host for email (e.g.,`smtp.gmail.com`)         |
| `SMTP_PORT`              | SMTP port (e.g.,`587`)                              |
| `SMTP_USER`              | SMTP email address                                    |
| `SMTP_PASS`              | SMTP app password                                     |
| `ALLOWED_ORIGINS`        | Comma-separated CORS origins                          |

---

### Database Setup

```bash
# Run migrations
npm run prisma:migrate

# Generate Prisma client
npm run prisma:generate

# Seed the database with initial data
npm run prisma:seed

# (Optional) Open Prisma Studio — visual DB browser
npm run prisma:studio
```

---

### Running the Server

```bash
# Development (hot-reload)
npm run dev

# Production
npm start
```

The server will start on `http://localhost:5000` by default.

---

## API Modules

| Module                          | Base Path                      | Description                             |
| ------------------------------- | ------------------------------ | --------------------------------------- |
| **Auth**                  | `/api/auth`                  | Register, login, refresh token, logout  |
| **OTP**                   | `/api/otp`                   | Send & verify OTP via SMS               |
| **Service Requests**      | `/api/sr`                    | Create and manage service requests      |
| **Work Orders**           | `/api/wo`                    | Work order lifecycle management         |
| **Dispatch**              | `/api/dispatch`              | Assign and manage technician dispatch   |
| **Dispatcher**            | `/api/dispatcher`            | Dispatcher dashboard & actions          |
| **Technician**            | `/api/technician`            | Technician profile, status, checkins    |
| **Technician Mgmt**       | `/api/technician-management` | Admin technician management             |
| **Employee**              | `/api/employee`              | Internal employee management            |
| **Payment**               | `/api/payment`               | Payment recording and verification      |
| **Commission**            | `/api/commission`            | Commission calculation and history      |
| **Payout**                | `/api/payout`                | Payout requests and processing          |
| **Rate**                  | `/api/rate`                  | System-wide commission rate settings    |
| **Wallet**                | (via commissions)              | Technician wallet & transaction history |
| **Category**              | `/api/category`              | Service categories management           |
| **Specialization**        | `/api/specialization`        | Technician specializations              |
| **Location**              | `/api/location`              | Real-time technician location updates   |
| **Notification**          | `/api/notification`          | In-app notifications                    |
| **Review**                | `/api/review`                | Customer reviews for technicians        |
| **Report**                | `/api/report`                | Admin reports and analytics             |
| **Admin**                 | `/api/admin`                 | Admin panel, users, audit logs          |
| **Call Center**           | `/api/callcenter`            | Call center agent portal                |
| **Call Center Dashboard** | `/api/call-center`           | Call center stats & dashboard           |
| **SMS**                   | `/api/sms`                   | Manual SMS sending                      |

---

## User Roles

| Role            | Description                                                              |
| --------------- | ------------------------------------------------------------------------ |
| `ADMIN`       | Full system access — manage users, rates, reports, payouts              |
| `CUSTOMER`    | Create service requests, view work orders, submit reviews                |
| `TECHNICIAN`  | Receive and complete work orders, manage location and profile            |
| `DISPATCHER`  | Assign technicians to work orders, manage dispatch queue                 |
| `CALL_CENTER` | Create customer accounts, submit service requests on behalf of customers |

---

## Project Structure

```
r2a-server/
├── prisma/
│   ├── schema.prisma        # Database schema
│   ├── seed.js              # Database seeder
│   └── migrations/          # Prisma migration history
│
├── src/
│   ├── server.js            # Entry point — starts HTTP server
│   ├── app.js               # Express app setup, middleware & routes
│   ├── prisma.js            # Prisma client singleton
│   │
│   ├── config/
│   │   └── env.js           # Environment variable loader
│   │
│   ├── controllers/         # Route handler logic (one per module)
│   ├── routes/              # Express route definitions (one per module)
│   ├── middleware/
│   │   ├── auth.js          # JWT authentication middleware
│   │   ├── errorHandler.js  # Global error handler
│   │   └── language.js      # i18n language detection
│   │
│   ├── services/            # Business logic & external integrations
│   ├── translations/        # i18n translation files
│   └── utils/               # Helpers (firebase, SMS, validators, etc.)
│
├── uploads/
│   ├── profiles/            # Technician profile photos
│   ├── documents/           # ID cards, permits, degrees
│   ├── payments/            # Payment proof images
│   └── wo-completion/       # Work order completion photos
│
├── .env.example             # Environment variable template
├── .gitignore
├── package.json
└── README.md
```

---

## Database Schema

Key models in the PostgreSQL database:

| Model                 | Description                                            |
| --------------------- | ------------------------------------------------------ |
| `User`              | All platform users (admin, customer, technician, etc.) |
| `TechnicianProfile` | Extended technician data, rates, documents             |
| `ServiceRequest`    | Customer service request records                       |
| `WorkOrder`         | Assigned work orders derived from service requests     |
| `Payment`           | Payment transactions linked to work orders             |
| `Commission`        | Commission records per technician per work order       |
| `Wallet`            | Technician wallet balance                              |
| `WalletTransaction` | Wallet credit/debit history                            |
| `Payout`            | Payout records processed by admin                      |
| `PayoutRequest`     | Technician-initiated payout requests                   |
| `Notification`      | In-app notification records                            |
| `FCMToken`          | Firebase push notification tokens per user/device      |
| `OTP`               | One-time password records for phone verification       |
| `AuditLog`          | Admin action audit trail                               |
| `Review`            | Customer reviews and ratings for technicians           |
| `Category`          | Service categories                                     |
| `Specialization`    | Technician specialization records                      |
| `TechnicianCheckin` | Daily technician check-in/check-out records            |

---

## Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Commit your changes: `git commit -m "feat: add your feature"`
4. Push to the branch: `git push origin feature/your-feature-name`
5. Open a Pull Request

Please follow the existing code style and ensure all routes are properly protected with the appropriate role middleware before submitting.

---

<p align="center">Built with ❤️ by the R2A Team</p>
# Field-Service-Management-FSM-Backend-API
