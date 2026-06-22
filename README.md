# 🛒 SnapMart

SnapMart is a full-featured **Point-of-Sale (POS) and retail management system** for the MERN stack, covering everything from checkout and inventory to suppliers, payroll, accounts, and financial reporting. Authentication is handled by **Firebase**, file/image storage by **Azure Blob Storage**, and both the frontend and backend ship as Docker containers.

This repository contains both halves of the project:

```
SnapMart/
├── frontend/   → React (Vite) admin dashboard & POS screen
└── backend/    → Express + MongoDB REST API
```

## Features

- 🧾 **Point of Sale** — Fast checkout screen for processing sales and printing receipts
- 📦 **Inventory Management** — Products, categories, units of measure (UOMs), and stock tracking
- 👥 **People Management** — Customers, employees, and suppliers in one place
- 🛍️ **Purchases & Returns** — Record stock purchases, supplier payments, and purchase/sale returns
- 💰 **Accounts** — Customer, employee, and supplier ledgers with payables/receivables tracking
- 🧮 **Expenses & Payroll** — Expense categories, expense tracking, attendance, and salary cycles
- 📊 **Reports & Dashboard** — Daily/monthly sales, profit & loss, business capital, inventory, and attendance reports
- 🔐 **Role-Based Access Control** — Firebase-authenticated users with configurable role permissions
- 🖼️ **Image Uploads** — Product/logo images stored in Azure Blob Storage
- 🐳 **Dockerized** — Both frontend and backend ship with production-ready Dockerfiles

## Tech Stack

| Layer        | Technology |
|--------------|------------|
| Frontend     | React 19, Vite, Tailwind CSS, Zustand, React Hook Form, Recharts |
| Backend API  | Node.js, Express |
| Database     | MongoDB (Mongoose) |
| Auth         | Firebase Authentication (client SDK + Admin SDK) |
| File Storage | Azure Blob Storage |
| PDF/Print    | @react-pdf/renderer, jsPDF, react-to-print |
| Deployment   | Docker (Nginx-served frontend, Node backend) |

## Project Structure

```
SnapMart/
├── frontend/
│   ├── src/
│   │   ├── pages/                # Layout, Login, POS screen
│   │   ├── SideBar/                # Top-level admin sections (Dashboard, Sales, Purchases, etc.)
│   │   ├── components/             # Feature components grouped by domain
│   │   ├── services/                # Axios API clients per resource
│   │   ├── store/                   # Zustand global state
│   │   └── firebase.js              # Firebase client config
│   ├── nginx.conf                   # Nginx config for serving the production build
│   └── Dockerfile
│
└── backend/
    ├── server.js                    # Express app entry point
    ├── routes/                      # Routes grouped by section (sales, purchases, accounts, etc.)
    ├── controllers/                  # Request handlers for users & roles
    ├── schemas/                      # Mongoose models
    ├── Firebase/                     # Firebase Admin SDK + auth middleware
    ├── lib/                          # Multer (file upload) config
    └── Dockerfile
```

## Getting Started

The frontend and backend are run and configured **independently**. See the dedicated README in each folder for full setup instructions:

- 💻 [`frontend/README.md`](./frontend/README.md) — running the React admin dashboard
- 🖥️ [`backend/README.md`](./backend/README.md) — running the API server

### Quick Start

1. **Start the backend first** (the app needs a live API):
   ```bash
   cd backend
   npm install
   npm run dev
   ```

2. **Then start the frontend**, pointing it at your backend's URL:
   ```bash
   cd frontend
   npm install
   npm run dev
   ```

### Running with Docker

Both services include a `Dockerfile` and can be built/run independently:

```bash
# Backend
cd backend
docker build -t snapmart-backend .
docker run -p 3000:3000 --env-file .env snapmart-backend

# Frontend (env vars must be present at build time)
cd frontend
docker build -t snapmart-frontend .
docker run -p 8080:80 snapmart-frontend
```

## Prerequisites

- Node.js v18+ and npm
- A MongoDB instance (local or [MongoDB Atlas](https://www.mongodb.com/atlas))
- A Firebase project (Authentication enabled, plus a service account for the backend)
- An Azure Storage Account with a Blob container (for image uploads)
- Docker (optional, for containerized deployment)

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/my-feature`)
3. Commit your changes
4. Push to the branch and open a Pull Request

## License

This project is licensed under the MIT License.
