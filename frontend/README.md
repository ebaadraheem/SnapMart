# 💻 SnapMart — Frontend

The admin dashboard and point-of-sale client for SnapMart, built with **React 19** and **Vite**. It connects to the [SnapMart backend](../backend/README.md) for all business data and uses **Firebase** for authentication.

## Features

- **Point of Sale Screen** — Dedicated `/pos` route for fast checkout and receipt printing
- **Admin Dashboard** — Sidebar-driven layout covering every part of the business:
  - **Dashboard** — Sales overview and key metrics
  - **People** — Customers, Employees, Suppliers
  - **Inventory** — Products, Categories, UOMs, Stock
  - **Sales** — Point of Sale, Sales List, Sale Returns
  - **Purchases** — Purchase/Add Stock, Purchases List, Purchase Returns
  - **Expenses** — Expense Categories, Manage Expenses
  - **Accounts** — Customer, Employee, and Supplier ledgers
  - **Reports** — Purchase, Sales, Profit & Loss, Business Capital, People, Accounts, Balance, Inventory, Expense, and Attendance reports
  - **Attendance** — Employee attendance tracking and reports
  - **System Users** — User Management, Role Management
  - **Configuration** — Access Control, Password Reset, Business Variables, Areas, Types
- **PDF & Print Support** — Generate and print invoices/receipts/reports
- **Charts** — Visual reporting via Recharts

## Tech Stack

- **React 19** + **Vite**
- **React Router** for client-side routing
- **Zustand** for global state
- **Firebase Auth** (client SDK) for authentication, **Firestore** client available
- **Tailwind CSS 4** for styling
- **React Hook Form** + **Yup** for form handling/validation
- **Axios** for HTTP requests
- **Recharts** for charts/graphs
- **@react-pdf/renderer**, **jsPDF**, **react-to-print**, **dom-to-image-more** for documents and printing
- **React Hot Toast** for notifications
- **Framer Motion** for animations

## Project Structure

```
frontend/
├── src/
│   ├── main.jsx                    # App entry point
│   ├── App.jsx                     # Route definitions (Layout + POS)
│   ├── pages/
│   │   ├── Layout.jsx                # Main authenticated app shell (sidebar + content)
│   │   ├── Login.jsx
│   │   └── POSScreen.jsx             # Standalone point-of-sale screen
│   ├── SideBar/                      # Top-level section containers (Dashboard, Sales, Purchases, etc.)
│   ├── components/
│   │   ├── PosComponents/              # POS UI (cart, checkout, etc.)
│   │   ├── ProductComponents/          # Products, categories, UOMs
│   │   ├── PeopleComponents/           # Customers, employees, suppliers
│   │   ├── PurchaseComponents/         # Purchases & purchase returns
│   │   ├── SalesComponents/            # Sales list & sale returns
│   │   ├── AccountComponents/          # Customer/employee/supplier accounts
│   │   ├── ExpenseComponents/          # Expense tracking
│   │   ├── ReportComponents/           # All report views
│   │   ├── AttendanceComponents.jsx
│   │   ├── SystemusersComponents/      # User & role management
│   │   ├── ConfigurationComponents/    # Access control, business variables, areas, types
│   │   └── SideBar.jsx                 # Sidebar navigation component
│   ├── services/                     # Axios API clients, one per resource
│   ├── store/useStore.js              # Zustand store
│   ├── hooks/                         # useAuth, usePagination
│   ├── utils/                         # Nav items, icons, printing helpers
│   ├── validation/                    # Yup schemas
│   └── firebase.js                    # Firebase client config
├── nginx.conf                         # Nginx config used in the production Docker image
├── Dockerfile                         # Multi-stage build (Vite build → Nginx serve)
└── vite.config.js
```

## Prerequisites

- Node.js (LTS recommended)
- npm or yarn
- The [SnapMart backend](../backend/README.md) running locally or deployed
- A Firebase project (Authentication enabled)

## Installation

1. Clone the repository and move into the frontend folder:
   ```bash
   git clone https://github.com/ebaadraheem/SnapMart.git
   cd SnapMart/frontend
   ```

2. Install dependencies:
   ```bash
   npm install
   # or
   yarn install
   ```

## Configuration

Create a file named **`.env.local`** in the `frontend/` root directory. Vite only exposes variables prefixed with `VITE_` to the client.

```env
VITE_FIREBASE_API_KEY="your-firebase-api-key"
VITE_FIREBASE_AUTH_DOMAIN="your-project.firebaseapp.com"
VITE_FIREBASE_PROJECT_ID="your-project-id"
VITE_FIREBASE_STORAGE_BUCKET="your-project.appspot.com"
VITE_FIREBASE_MESSAGING_SENDER_ID="your-sender-id"
VITE_FIREBASE_APP_ID="your-app-id"
VITE_FIREBASE_MEASUREMENT_ID="your-measurement-id"
VITE_SERVER_URL="http://localhost:3000/api"
VITE_ADMIN_ROLE_ID="firebase-uid-of-the-admin-user"
```

### Environment Variables Explained

| Variable | Description |
|---|---|
| `VITE_FIREBASE_API_KEY` | Your Firebase project's API key |
| `VITE_FIREBASE_AUTH_DOMAIN` | Firebase Authentication domain |
| `VITE_FIREBASE_PROJECT_ID` | Firebase project ID |
| `VITE_FIREBASE_STORAGE_BUCKET` | Firebase Storage bucket URL |
| `VITE_FIREBASE_MESSAGING_SENDER_ID` | Sender ID for Firebase Cloud Messaging |
| `VITE_FIREBASE_APP_ID` | Firebase web app ID |
| `VITE_FIREBASE_MEASUREMENT_ID` | Firebase Analytics measurement ID |
| `VITE_SERVER_URL` | Base URL of the running SnapMart backend API |
| `VITE_ADMIN_ROLE_ID` | Firebase UID of the designated administrator account, used for frontend role checks |

> ⚠️ Never commit `.env.local`. Make sure it's listed in `.gitignore`.

## Running the App

Start the Vite dev server:

```bash
npm run dev
```

Other available scripts:

```bash
npm run build     # Production build (outputs to dist/)
npm run preview   # Preview the production build locally
npm run lint        # Run ESLint
```

## Running with Docker

The included `Dockerfile` builds the app with Vite and serves the static output with Nginx.

```bash
# .env.local must exist before building — it's copied in during the build stage
docker build -t snapmart-frontend .
docker run -p 8080:80 snapmart-frontend
```

The app will be available at `http://localhost:8080`. `nginx.conf` handles SPA routing (`try_files ... /index.html`) and basic static-asset caching.

## Notes

- The backend repository/folder is referenced as the **SnapMart Server** — see [`../backend/README.md`](../backend/README.md) for setup.
- `VITE_ADMIN_ROLE_ID` is used for client-side role checks only; the backend should still enforce its own authorization rules for sensitive operations.
