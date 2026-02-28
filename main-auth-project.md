# 🔐 SecureAuth Pro – Next.js 16 Hybrid Authentication System

> Production-ready Hybrid Authentication System built with Next.js 16 (App Router), JWT, Access & Refresh Tokens, Token Rotation, Role-Based Access Control, and Secure HTTP-only Cookies.

---

# 🚀 Overview

This project implements a modern authentication system using:

* Next.js 16 (App Router)
* MongoDB (Local)
* JWT Authentication
* Access & Refresh Token Architecture
* Refresh Token Rotation
* Role-Based Access Control (RBAC)
* Logout & Logout All Devices
* Account Disable Enforcement
* Hybrid Stateless + Database Validation Model

This is not a basic tutorial-level auth — this is enterprise-ready architecture.

---

# 🏗 Authentication Architecture

```
Login
   ↓
Access Token (15m)
Refresh Token (7d)
   ↓
HTTP-only cookies
   ↓
Middleware (basic guard)
   ↓
Server-side verifyAndRefresh()
   ↓
Auto refresh if expired
```

---

# 🔐 Core Concepts Implemented

## 1️⃣ JWT Authentication

* JWT signed (not encrypted)
* Secret stored in environment variable
* Stateless verification using signature
* Role embedded in payload

Payload example:

```json
{
  "userId": "abc123",
  "role": "admin",
  "exp": 1234567890
}
```

---

## 2️⃣ Access Token

* Expiry: 15 minutes
* Used for route protection
* Verified server-side
* Short-lived for security

---

## 3️⃣ Refresh Token

* Expiry: 7 days
* Stored in HTTP-only cookie
* Stored in database
* Used to generate new access tokens

---

## 4️⃣ Refresh Token Rotation

Every refresh:

* Old refresh token deleted
* New refresh token generated
* Stored in DB
* Cookie updated

Prevents replay attacks.

---

## 5️⃣ Silent Refresh (Server-Side)

When access token expires:

1. Server component detects expiry
2. Calls `/api/auth/refresh`
3. New access token issued
4. User continues seamlessly

No manual login required.

---

## 6️⃣ Middleware (Next.js 16 Compatible)

Middleware only checks token existence:

```ts
if (!accessToken) redirect("/login")
```

Actual verification happens server-side.

---

## 7️⃣ Role-Based Access Control (RBAC)

Admin-only route:

```ts
if (auth.user.role !== "admin") {
  redirect("/dashboard");
}
```

Roles supported:

* user
* admin

---

## 8️⃣ Logout

Logout performs:

* Delete refresh token from DB
* Clear cookies
* Destroy session

---

## 9️⃣ Logout All Devices

Deletes all refresh tokens for user:

```ts
await RefreshToken.deleteMany({ userId })
```

Logs user out everywhere.

---

## 🔟 Account Disable Enforcement

If `isActive: false`:

* Refresh denied
* All sessions revoked
* User redirected to login

---

# 📂 Folder Structure

```
src/
 ├── app/
 │    ├── api/
 │    │    └── auth/
 │    │         ├── register/
 │    │         ├── login/
 │    │         ├── refresh/
 │    │         ├── logout/
 │    │         └── logout-all/
 │    ├── dashboard/
 │    ├── admin/
 │
 ├── lib/
 │    ├── db.ts
 │    ├── jwt.ts
 │    ├── auth.ts
 │
 ├── models/
 │    ├── User.ts
 │    └── RefreshToken.ts
 │
 └── middleware.ts
```

---

# 🛠 Setup Instructions

## 1️⃣ Clone Project

```bash
git clone <repo-url>
cd secure-auth-pro
```

## 2️⃣ Install Dependencies

```bash
npm install
```

## 3️⃣ Create Environment File

`.env.local`

```
MONGODB_URI=mongodb://127.0.0.1:27017/secure-auth-pro
JWT_SECRET=super_secret_key
NEXT_PUBLIC_BASE_URL=http://localhost:3000
```

Restart server after adding.

---

## 4️⃣ Start MongoDB

Make sure local MongoDB is running.

---

## 5️⃣ Run Development Server

```bash
npm run dev
```

---

# 🔐 Security Features

✔ HTTP-only cookies
✔ Secure flag (production-ready)
✔ SameSite strict
✔ Password hashing (bcrypt)
✔ Token rotation
✔ DB validation
✔ Account disable enforcement
✔ Role-based restriction
✔ Hybrid stateless + stateful validation

---

# ⚖ Hybrid Authentication Model

This system combines:

Stateless (JWT)
+
Stateful (DB refresh token tracking)

Advantages:

* Scalable
* Secure
* Revokable
* Role update capable
* Account disable capable

---

# 🧠 Why Not Pure Stateless?

Pure JWT cannot:

* Instantly revoke access
* Reflect role change immediately
* Disable user instantly

Hybrid solves that.

---

# 🔄 Token Lifecycle

```
Login → Access + Refresh
Access expires → Refresh called
Refresh rotates → New access + refresh
Logout → Tokens removed
Disable account → Refresh denied
```

---

# 🧪 Testing Flow

1. Register user
2. Login
3. Access dashboard
4. Wait 15 min
5. Refresh page → auto refresh
6. Test logout
7. Test logout-all
8. Test admin route
9. Disable user in DB

Everything works without manual login unless refresh expires.

---

# 🎯 Production Notes

In production:

* Set `secure: true`
* Use HTTPS
* Use Redis for scalable session tracking
* Add rate limiting
* Add CSRF protection
* Add device fingerprinting

---

# 🎉 Final Status

This project implements:

✔ Enterprise-grade authentication
✔ Next.js 16 App Router safe
✔ Modern architecture
✔ Clean separation of concerns
✔ Secure session handling

---

# 🏁 Project Status

Authentication module complete.
No banking-level extensions added.

---

# 📜 License

MIT License

---

# 🙌 Author

Built by Gautam 🚀
Full-Stack Developer

---

---

# 🎉 Project Complete
