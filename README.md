# 🚌 BusBook — MERN Stack Bus Booking System

A full-featured online bus ticket booking platform built with MongoDB, Express, React (Vite), and Node.js.

---

## 📋 Features

### User Features
- 🔐 **Authentication** — Register, Login, Forgot/Reset Password (JWT + bcrypt)
- 🔍 **Bus Search** — Search buses by city, date with smart autocomplete
- 📋 **Bus Listing** — Filter by type, price, seats. Sort by departure/price/rating
- 💺 **Seat Selection** — Visual seat map with real-time availability
- 👥 **Passenger Details** — Multi-passenger form with validation
- 💳 **Payments** — Razorpay & Stripe integration
- 🎫 **E-Ticket** — Printable digital ticket after confirmation
- 📜 **Booking History** — View, cancel, and manage all bookings
- ⭐ **Reviews & Ratings** — Rate bus services after travel
- 👤 **Profile** — Edit profile, change password

### Admin Features
- 📊 **Dashboard** — Stats: users, buses, routes, bookings, revenue
- 🚌 **Bus Management** — Add/edit/delete buses with amenities
- 🛣️ **Route Management** — Define city routes, schedules, pricing
- 📋 **Booking Management** — View all bookings with filters
- 👥 **User Management** — View, activate/deactivate users

### Notifications
- 📧 Booking confirmation email
- ❌ Cancellation email

---

## 🏗️ Project Structure

```
busbook/
├── server/                    # Node.js + Express backend
│   ├── config/
│   │   └── db.js              # MongoDB connection
│   ├── controllers/           # Business logic
│   │   ├── auth.controller.js
│   │   ├── bus.controller.js
│   │   ├── route.controller.js
│   │   ├── booking.controller.js
│   │   ├── payment.controller.js
│   │   ├── review.controller.js
│   │   └── admin.controller.js
│   ├── middleware/
│   │   └── auth.middleware.js  # JWT protect + adminOnly
│   ├── models/
│   │   ├── User.model.js
│   │   ├── Bus.model.js
│   │   ├── Route.model.js
│   │   ├── Booking.model.js
│   │   └── Review.model.js
│   ├── routes/                # Express route files
│   ├── utils/
│   │   ├── email.util.js       # Nodemailer
│   │   └── ticket.util.js      # HTML ticket generator
│   ├── .env.example
│   ├── index.js               # Entry point
│   └── package.json
│
├── client/                    # React + Vite frontend
│   ├── src/
│   │   ├── components/
│   │   │   ├── common/        # Navbar, Footer, Layout, Spinner
│   │   │   ├── home/          # SearchForm
│   │   │   └── booking/       # BusCard, SeatMap, BookingSteps
│   │   ├── context/
│   │   │   ├── AuthContext.jsx
│   │   │   └── BookingContext.jsx
│   │   ├── pages/
│   │   │   ├── HomePage.jsx
│   │   │   ├── SearchPage.jsx
│   │   │   ├── BusListPage.jsx
│   │   │   ├── SeatSelectPage.jsx
│   │   │   ├── PassengerPage.jsx
│   │   │   ├── PaymentPage.jsx
│   │   │   ├── BookingSuccessPage.jsx
│   │   │   ├── MyBookingsPage.jsx
│   │   │   ├── BookingDetailPage.jsx
│   │   │   ├── ProfilePage.jsx
│   │   │   ├── LoginPage.jsx
│   │   │   ├── RegisterPage.jsx
│   │   │   ├── ForgotPasswordPage.jsx
│   │   │   ├── ResetPasswordPage.jsx
│   │   │   └── admin/         # Admin panel pages
│   │   ├── services/
│   │   │   └── api.js          # Axios instance
│   │   ├── App.jsx             # Routes + Guards
│   │   ├── main.jsx
│   │   └── index.css           # Tailwind + custom styles
│   ├── vite.config.js
│   ├── tailwind.config.js
│   └── package.json
│
├── seed.js                    # Database seeder
├── package.json               # Root (concurrently)
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites
- Node.js v18+
- MongoDB (local or Atlas)
- npm or yarn

### 1. Clone & Install

```bash
git clone <repo-url>
cd busbook
npm run install-all
```

### 2. Configure Environment

```bash
cp server/.env.example server/.env
```

Edit `server/.env`:
```env
PORT=5000
MONGO_URI=mongodb://localhost:27017/busbook
JWT_SECRET=your_super_secret_key_minimum_32_chars
JWT_EXPIRE=7d

# Email (optional — use Gmail App Password)
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USER=your@gmail.com
EMAIL_PASS=your_app_password

# Payment (get from dashboard)
STRIPE_SECRET_KEY=sk_test_...
RAZORPAY_KEY_ID=rzp_test_...
RAZORPAY_KEY_SECRET=...

CLIENT_URL=http://localhost:5173
```

Also create `client/.env`:
```env
VITE_API_URL=http://localhost:5000/api
VITE_RAZORPAY_KEY_ID=rzp_test_...
```

### 3. Seed the Database

```bash
node seed.js
```

This creates:
- **Admin:** `admin@busbook.com` / `admin123`
- **User:** `user@busbook.com` / `user1234`
- 5 buses + 10 popular routes

### 4. Run the App

```bash
npm run dev
```

- Frontend: http://localhost:5173
- Backend API: http://localhost:5000

---

## 🔌 API Reference

### Auth
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/register` | Register new user |
| POST | `/api/auth/login` | Login |
| GET  | `/api/auth/me` | Get current user |
| PUT  | `/api/auth/profile` | Update profile |
| PUT  | `/api/auth/change-password` | Change password |
| POST | `/api/auth/forgot-password` | Send reset email |
| PUT  | `/api/auth/reset-password/:token` | Reset password |

### Buses
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/buses/search?from=&to=&date=` | Search buses |
| GET | `/api/buses/:id/seats?routeId=&date=` | Get seat layout |
| POST | `/api/buses` | Add bus (admin) |
| PUT | `/api/buses/:id` | Update bus (admin) |
| DELETE | `/api/buses/:id` | Delete bus (admin) |

### Routes
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/routes/cities` | Get all cities |
| GET | `/api/routes` | Get all routes |
| POST | `/api/routes` | Add route (admin) |

### Bookings
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/bookings` | Create booking |
| GET | `/api/bookings/my` | My bookings |
| GET | `/api/bookings/:id` | Booking detail |
| PUT | `/api/bookings/:id/cancel` | Cancel booking |
| GET | `/api/bookings` | All bookings (admin) |

### Payments
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/payments/razorpay/create-order` | Create Razorpay order |
| POST | `/api/payments/razorpay/verify` | Verify payment |
| POST | `/api/payments/stripe/create-intent` | Create Stripe intent |
| POST | `/api/payments/stripe/confirm` | Confirm Stripe payment |

### Admin
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/admin/stats` | Dashboard stats |
| GET | `/api/admin/users` | All users |
| PUT | `/api/admin/users/:id/toggle` | Toggle user status |

---

## 💳 Payment Setup

### Razorpay (Recommended for India)
1. Create account at https://razorpay.com
2. Get Key ID and Secret from Settings → API Keys
3. Add to `.env`
4. Add Razorpay script to `client/index.html`:
   ```html
   <script src="https://checkout.razorpay.com/v1/checkout.js"></script>
   ```

### Stripe
1. Create account at https://stripe.com
2. Get Secret key from Dashboard
3. Add to `.env`

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 18, Vite, Tailwind CSS |
| Routing | React Router v6 |
| State | Context API |
| HTTP | Axios |
| Backend | Node.js, Express |
| Database | MongoDB, Mongoose |
| Auth | JWT, bcryptjs |
| Email | Nodemailer |
| Payments | Razorpay, Stripe |
| Icons | Lucide React |
| Toasts | React Hot Toast |

---

## 🔒 Security
- Passwords hashed with bcrypt (10 rounds)
- JWT authentication on protected routes
- Admin-only middleware for admin routes
- Input validation on critical endpoints
- CORS configured for client origin only

---

## 📝 License
MIT — free for personal and commercial use.
