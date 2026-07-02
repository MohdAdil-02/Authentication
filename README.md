# MERN Authentication App

A full-stack authentication system built with **MongoDB, Express, React, and Node.js (MERN)**. Includes secure password hashing, JWT-based login sessions, protected API routes, and a polished, responsive UI built with Tailwind CSS.

---

## Features

- User registration with hashed passwords (bcrypt)
- Login with JWT (JSON Web Token) issuance
- Protected backend routes using auth middleware
- Token stored on the client and sent automatically on requests
- Modern, responsive login/register UI with Tailwind CSS
- Password show/hide toggle and loading states on forms

---

## Tech Stack

**Frontend:** React (Vite), React Router, Axios, Tailwind CSS
**Backend:** Node.js, Express, MongoDB, Mongoose
**Auth:** bcryptjs (password hashing), jsonwebtoken (JWT)

---

## Project Structure

```
mern-auth/
├── server/
│   ├── models/
│   │   └── User.js
│   ├── routes/
│   │   └── auth.js
│   ├── middleware/
│   │   └── auth.js
│   ├── .env
│   ├── .gitignore
│   └── server.js
└── client/
    ├── src/
    │   ├── api/
    │   │   └── axios.js
    │   ├── pages/
    │   │   ├── Login.jsx
    │   │   └── Register.jsx
    │   ├── App.jsx
    │   ├── index.css
    │   └── main.jsx
    └── vite.config.js
```

---

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) installed
- A MongoDB database (local or [MongoDB Atlas](https://www.mongodb.com/atlas))

### 1. Clone / set up the project

```bash
mkdir mern-auth && cd mern-auth
```

### 2. Backend setup

```bash
cd server
npm install
```

Create a `.env` file inside `server/`:

```
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=your_long_random_secret_string
PORT=5000
```

Run the backend:

```bash
node server.js
# or, for auto-restart during development:
npx nodemon server.js
```

You should see:
```
MongoDB connected
Server running on port 5000
```

### 3. Frontend setup

```bash
cd ../client
npm install
npm run dev
```

Visit the local URL shown in the terminal (usually `http://localhost:5173`).

---

## API Endpoints

| Method | Endpoint             | Description                          | Auth Required |
|--------|-----------------------|---------------------------------------|----------------|
| POST   | `/api/auth/register`  | Register a new user                   | No             |
| POST   | `/api/auth/login`     | Log in and receive a JWT token        | No             |
| GET    | `/api/auth/profile`   | Get the logged-in user's profile      | Yes (Bearer token) |

### Example: Register

```json
POST /api/auth/register
{
  "username": "testuser",
  "email": "test@test.com",
  "password": "123456"
}
```

### Example: Login

```json
POST /api/auth/login
{
  "email": "test@test.com",
  "password": "123456"
}
```

Response:

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "...",
    "username": "testuser",
    "email": "test@test.com"
  }
}
```

### Example: Accessing a protected route

```
GET /api/auth/profile
Authorization: Bearer <token>
```

---

## How Authentication Works

1. **Register** — the user's password is hashed with bcrypt before being saved to MongoDB. Plain-text passwords are never stored.
2. **Login** — the submitted password is compared against the stored hash using `bcrypt.compare()`. If it matches, a JWT is signed containing the user's ID and returned to the client.
3. **Storing the token** — the frontend saves the JWT in `localStorage` after login.
4. **Authenticated requests** — an Axios interceptor automatically attaches the token as an `Authorization: Bearer <token>` header on every request.
5. **Protecting routes** — backend middleware verifies the token on protected endpoints. If it's missing or invalid, the request is rejected with a `401` status before it reaches the route handler.

---

## Security Notes

This project covers the fundamentals of authentication, but before using it for anything with real user data, consider:

- Never commit your `.env` file — keep it in `.gitignore`
- Use a strong, randomly generated `JWT_SECRET` (32+ characters)
- Serve the app over HTTPS in production
- Add rate limiting on the login route (e.g. `express-rate-limit`) to slow brute-force attempts
- Add `helmet` middleware for basic HTTP header hardening
- Validate and sanitize all user input on the backend (e.g. with `express-validator`)
- Consider storing the JWT in an `httpOnly` cookie instead of `localStorage` for stronger protection against XSS
- Add token refresh logic if you want longer-lived sessions without long-lived access tokens

---

## Possible Next Steps

- Protect frontend routes (e.g. `/dashboard`) so unauthenticated users are redirected to `/login`
- Add a logout button that clears the stored token
- Build out a real dashboard that fetches and displays the logged-in user's profile
- Add form validation feedback (e.g. password strength, email format)
- Add "forgot password" / email verification flows

---

## License

This project is for learning purposes. Feel free to use and modify it.