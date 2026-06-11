# 🏋️ Minimalist Gyms — Production-Grade Gym Management Platform

A full-stack SaaS-style gym management system with a separate **user portal** and **admin panel** — built solo, deployed for a real client. Handles everything from online admission and Razorpay payments to RBAC-based admin management and automated member notifications.

**Live:** https://www.minimalistgyms.com &nbsp;|&nbsp; **Admin Demo:** https://admin.minimalistgyms.com/

---
## Code Architecture
- backend → https://github.com/Saikat-github/gym-2-backend-mern
- admin panel → https://github.com/Saikat-github/gym-2-admin-mern
- user panel → https://github.com/Saikat-github/gym-2-frontend-mern

---

## Features

### User Portal
- **Online Admission** — multi-step form with strong frontend (React Hook Form) and backend (express-validator) validation
- **Authentication** — Email OTP (cryptographically secure via `crypto.randomBytes`) + Google OAuth (Passport.js)
- **Payments** — Razorpay integration for membership plans and day passes; HMAC webhook signature verification on backend
- **Membership Auto-Renewal** — cron job automatically updates expired memberships
- **Profile Management** — update profile photo (Cloudinary), delete account (Mongoose session for atomic multi-document rollback)

### Admin Panel
- **RBAC** — Super Admin + limited-permission admins. Super Admin created via DB migration script. Every admin route protected by role-checking middleware
- **Member Management** — search/filter members by name, membership status, or date range
- **User & Payment Dashboard** — filter registered users, payments, and day passes with the same flexible filters
- **Gym Settings** — create/update membership plans, update gym timings
- **Holiday Alerts** — one-click notification emails to all active members via Resend
- **Admin Management** — Super Admin can add/verify new admins using email OTP; no self-signup on admin panel

### Backend & Security
- All inputs sanitized on ingress; critical values validated at the controller level before touching the DB
- Rate limiting on sensitive routes
- Winston logger for production observability
- MongoDB aggregation pipeline for dashboard analytics
- Complex Mongoose schemas designed for production

---

## Tech Stack

| Layer | Tech |
|---|---|
| Frontend | React, TailwindCSS, React Hook Form, Context API |
| Backend | Node.js, Express.js, Passport.js (Local + Google OAuth) |
| Database | MongoDB, Mongoose (aggregation, sessions, migration) |
| Auth | Email OTP, Google OAuth, RBAC |
| Payments | Razorpay (webhook HMAC verification) |
| File Storage | Cloudinary, Multer |
| Email | Resend |
| Jobs | node-cron |
| Security | express-rate-limit, express-validator, input sanitization |
| Logging | Winston |


---

## Key Implementation Highlights

- **Atomic account deletion** using `mongoose.startSession()` — user doc, membership, payments deleted in one transaction; rolls back on failure
- **Webhook security** — Razorpay payment events verified server-side using `crypto.createHmac('sha256', secret)`
- **OTP security** — `crypto.randomBytes(3).toString('hex')` instead of `Math.random()`; OTP hashed before DB storage, TTL index for auto-expiry
- **Sparse index** on optional unique fields to allow multiple null values without unique constraint violation

---

## Screenshots

<img width="1348" height="587" alt="minimalistgyms-pic-1" src="https://github.com/user-attachments/assets/9caf00e4-977e-414c-bf42-daa2980155f7" />

<img width="1342" height="584" alt="image" src="https://github.com/user-attachments/assets/12a4ee2a-da4f-4a75-997e-4515b2251902" />

<img width="1343" height="580" alt="image" src="https://github.com/user-attachments/assets/82624594-0855-43cf-b982-d8076ef89c17" />
<img width="1336" height="577" alt="image" src="https://github.com/user-attachments/assets/936293fd-3bce-424c-a668-5b7b8d4aaa09" />



