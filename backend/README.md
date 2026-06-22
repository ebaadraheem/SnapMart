# 🖥️ SnapMart — Backend

The REST API server for SnapMart, built with **Node.js**, **Express**, and **MongoDB**. It powers the point-of-sale and admin dashboard with endpoints for inventory, people, sales, purchases, accounts, expenses, attendance, payroll, and reporting — secured with **Firebase Authentication** and backed by **Azure Blob Storage** for image uploads.

## Features

- 🔐 **Authentication** — Firebase Admin SDK verifies ID tokens; user records are synced between Firebase and MongoDB
- 🧑‍🤝‍🧑 **People Management** — Customers, employees, and suppliers
- 📦 **Inventory** — Products, categories, units of measure (UOMs), and stock transactions
- 🧾 **Sales & Purchases** — Transactions, held invoices, sale returns, purchase returns
- 💰 **Accounts** — Customer/employee/supplier ledgers, payments, payables & receivables
- 🧮 **Expenses & Payroll** — Expense categories, expense tracking, attendance, salary cycles
- 📊 **Reporting** — Dashboard summaries, profit & loss, business capital, and section-specific reports
- ☁️ **Cloud File Storage** — Images (e.g. business logo, product photos) uploaded to Azure Blob Storage
- 🛠️ **Role-Based Access Control** — Configurable roles with granular access permissions

## Tech Stack

- **Node.js** (ES Modules) + **Express 5**
- **MongoDB** + **Mongoose**
- **Firebase Admin SDK** for authentication
- **Azure Blob Storage** (`@azure/storage-blob`) for file storage
- **Multer** (memory storage) for handling uploads before pushing to Azure
- **express-async-handler** for cleaner async route handlers
- **date-fns** for date calculations (reports, salary cycles, attendance)
- **Nodemon** for local development

## Project Structure

```
backend/
├── server.js                          # App entry point, DB connection, route mounting
├── routes/
│   ├── productsection/                  # Products, Categories, UOMs
│   ├── peoplesection/                    # Customers, Employees, Suppliers
│   ├── salesection/                      # Transactions, Sale Returns
│   ├── purchasesection/                  # Purchases, Purchase Returns
│   ├── accountsection/                   # Customer/Employee/Supplier Accounts
│   ├── expensesection/                   # Expense Categories, Expenses
│   ├── systemusersection/                # Users, Roles
│   ├── dashboard.js                      # Dashboard summary endpoints
│   ├── attendance.js                     # Attendance tracking
│   ├── held-invoices.js                  # Held/parked sales
│   ├── areas.js / types.js               # Supporting lookup data
│   └── variables.js                      # Business-wide configuration variables
├── controllers/                         # Request handlers (users, roles)
├── schemas/                             # Mongoose models grouped by domain
├── Firebase/
│   ├── FirebaseAdmin.js                   # Firebase Admin SDK initialization
│   └── authMiddleware.js                  # Verifies Firebase ID tokens on protected routes
├── lib/
│   └── multer.js                          # In-memory upload handling before Azure transfer
└── Dockerfile
```

## Prerequisites

- Node.js (LTS recommended)
- npm or yarn
- A MongoDB connection string (local instance or [MongoDB Atlas](https://www.mongodb.com/atlas))
- A Firebase project with a generated **service account key** (for `firebase-admin`)
- An Azure Storage Account with a Blob container

## Installation

1. Clone the repository and move into the backend folder:
   ```bash
   git clone https://github.com/ebaadraheem/SnapMart.git
   cd SnapMart/backend
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

## Configuration

Create a `.env` file in the `backend/` root directory:

```env
PORT=3000
MONGODB_URI="mongodb+srv://<username>:<password>@cluster.mongodb.net/SnapMart"
FIREBASE_SERVICE_ACCOUNT_KEY='{"type": "service_account", "project_id": "...", ...}'
AZURE_STORAGE_CONNECTION_STRING="DefaultEndpointsProtocol=https;AccountName=...;AccountKey=...;EndpointSuffix=core.windows.net"
AZURE_CONTAINER_NAME="snapmart-images"
```

### Environment Variables Explained

| Variable | Description |
|---|---|
| `PORT` | The port on which the Express server will run |
| `MONGODB_URI` | Connection string for your MongoDB database (e.g. MongoDB Atlas) |
| `FIREBASE_SERVICE_ACCOUNT_KEY` | The full JSON content of your Firebase service account key, used for backend token verification and user creation |
| `AZURE_STORAGE_CONNECTION_STRING` | Connection string for your Azure Storage Account — grants access to Blob Storage for image uploads |
| `AZURE_CONTAINER_NAME` | The Azure Blob container used for storing uploaded images |

> ⚠️ Never commit your `.env` file. Add it to `.gitignore` if it isn't already.
> The `FIREBASE_SERVICE_ACCOUNT_KEY` should be the entire service account JSON as a single-line string.

## Running the Server

**Development** (auto-restarts on file changes via Nodemon):
```bash
npm run dev
```

**Production:**
```bash
npm start
```

The server starts on `http://localhost:3000` (or your configured `PORT`).

## API Overview

All endpoints are mounted under `/api`. Routes are organized by business section:

| Base Path | Covers |
|---|---|
| `/api/products`, `/api/categories`, `/api/uoms` | Inventory: products, categories, units of measure |
| `/api/customers`, `/api/employees`, `/api/suppliers` | People management |
| `/api/transactions`, `/api/sale-return` | Sales and sale returns |
| `/api/purchases`, `/api/purchase-returns` | Purchases and purchase returns |
| `/api/held-invoices` | Parked/held sales |
| `/api/customer-accounts`, `/api/employee-accounts`, `/api/supplier-accounts` | Ledgers, payments, payables & receivables |
| `/api/expense-category`, `/api/expenses` | Expense tracking |
| `/api/attendance` | Employee attendance |
| `/api/employee-salaries` | Salary cycles and payroll |
| `/api/users`, `/api/roles` | System users and role-based access control |
| `/api/dashboard` | Daily/monthly sales summaries and overview stats |
| `/api/area-category`, `/api/types-category`, `/api/business` | Supporting lookup data and business-wide settings |

Each section generally exposes standard CRUD endpoints (`GET /`, `GET /:id`, `POST /`, `PUT /:id`, `DELETE /:id`), plus section-specific endpoints for search, reports, and payments — see the corresponding file under `routes/` for exact paths.

### Authentication

Protected routes expect a Firebase ID token:

```
Authorization: Bearer <firebase-id-token>
```

The `authMiddleware` (in `Firebase/authMiddleware.js`) verifies the token using the Firebase Admin SDK and attaches the decoded user to `req.user`.

## File Uploads

Uploads (e.g. business logo, product images) are received via **Multer** using in-memory storage, then streamed to **Azure Blob Storage** under the configured container. Only `jpeg`, `jpg`, `png`, and `gif` files up to 5MB are accepted.

## Running with Docker

```bash
docker build -t snapmart-backend .
docker run -p 3000:3000 --env-file .env snapmart-backend
```
