# 🔐 Auth Starter Kit

A reusable authentication boilerplate for Node.js backend applications. Stop rewriting the same authentication logic for every project — clone this repository and focus on building your product.

Built with **Node.js, Express, TypeScript, and MongoDB**.

## ✨ Features

### Core Authentication

- User registration and login
- Secure password hashing using Argon2id or bcrypt
- JWT-based authentication
- Short-lived access tokens
- Refresh token rotation and session revocation
- Logout and logout from all devices
- Protected routes using authentication middleware

### Authorization & Security

- Role-based access control (RBAC)
- User roles: `user`, `admin`
- Request validation
- Centralized error handling
- Rate limiting for authentication endpoints
- Secure cookie configuration
- Environment-based configuration

### Account Management

- Get the authenticated user's profile
- Update profile details
- Change password
- Forgot password and reset password
- Email verification

### Developer Experience

- TypeScript support
- Modular, maintainable folder structure
- Reusable middleware and services
- Environment variable validation
- API documentation
- Docker support (planned)

> Features should be marked complete only after their implementation and tests are verified.

## 🛠️ Tech Stack

| Technology         | Purpose                            |
| ------------------ | ---------------------------------- |
| Node.js            | JavaScript runtime                 |
| Express.js         | HTTP server and routing            |
| TypeScript         | Type safety                        |
| MongoDB            | User and session persistence       |
| Mongoose           | MongoDB object modeling            |
| JWT                | Access token authentication        |
| Argon2id / bcrypt  | Password hashing                   |
| Zod                | Request and environment validation |
| Nodemailer         | Email delivery                     |
| Vitest / Supertest | Testing                            |

## 📁 Project Structure

```text
auth-starter-kit/
├── src/
│   ├── config/
│   │   ├── database.ts
│   │   └── env.ts
│   ├── modules/
│   │   ├── auth/
│   │   │   ├── auth.controller.ts
│   │   │   ├── auth.service.ts
│   │   │   ├── auth.routes.ts
│   │   │   ├── auth.validation.ts
│   │   │   └── auth.types.ts
│   │   ├── users/
│   │   │   ├── user.model.ts
│   │   │   ├── user.service.ts
│   │   │   └── user.routes.ts
│   │   └── sessions/
│   │       ├── session.model.ts
│   │       └── session.service.ts
│   ├── middleware/
│   │   ├── authenticate.ts
│   │   ├── authorize.ts
│   │   ├── rate-limit.ts
│   │   └── error-handler.ts
│   ├── utils/
│   │   ├── password.ts
│   │   ├── tokens.ts
│   │   └── api-error.ts
│   ├── app.ts
│   └── server.ts
├── tests/
│   ├── auth.test.ts
│   └── authorization.test.ts
├── .env.example
├── .gitignore
├── package.json
├── tsconfig.json
└── README.md
```

## 🚀 Getting Started

### Prerequisites

- Node.js (LTS version)
- npm
- MongoDB local instance or MongoDB Atlas account

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/auth-starter-kit.git
cd auth-starter-kit
```

### 2. Install dependencies

```bash
npm install
```

### 3. Configure environment variables

Create your local `.env` file from the example:

```bash
cp .env.example .env
```

On Windows Command Prompt, use:

```cmd
copy .env.example .env
```

Configure the following variables:

```dotenv
NODE_ENV=development
PORT=3000

MONGODB_URI=mongodb://127.0.0.1:27017/auth_starter

ACCESS_TOKEN_SECRET=replace_with_a_secure_random_secret
REFRESH_TOKEN_SECRET=replace_with_a_different_secure_random_secret

ACCESS_TOKEN_TTL=15m
REFRESH_TOKEN_TTL=7d

CLIENT_ORIGIN=http://localhost:5173

COOKIE_SECURE=false
COOKIE_SAME_SITE=lax

SMTP_HOST=
SMTP_PORT=587
SMTP_USER=
SMTP_PASS=
EMAIL_FROM=
```

Generate strong, independent secrets rather than using the example placeholders. Never commit `.env` or real credentials to GitHub.

### 4. Start the development server

```bash
npm run dev
```

The server should start at:

```text
http://localhost:3000
```

### 5. Run tests

```bash
npm test
```

## 📡 API Endpoints

Base URL: `/api/v1`

| Method | Endpoint                 | Access                |
| ------ | ------------------------ | --------------------- |
| POST   | `/auth/register`         | Public                |
| POST   | `/auth/login`            | Public                |
| POST   | `/auth/refresh`          | Refresh session       |
| POST   | `/auth/logout`           | Authenticated session |
| POST   | `/auth/logout-all`       | Authenticated         |
| POST   | `/auth/forgot-password`  | Public                |
| POST   | `/auth/reset-password`   | Valid reset token     |
| POST   | `/auth/verify-email`     | Verification token    |
| GET    | `/users/me`              | Authenticated         |
| PATCH  | `/users/me`              | Authenticated         |
| PATCH  | `/users/change-password` | Authenticated         |
| GET    | `/admin/dashboard`       | Admin only            |

These endpoints describe the intended API contract; implement and test each route before treating it as available.

## 🔑 Authentication Flow

1. A user registers with their email and password.
2. The server validates the request and hashes the password.
3. On successful login, the server issues a short-lived access token and establishes a refresh session.
4. The client sends the access token when accessing protected resources.
5. When the access token expires, the client uses the refresh mechanism to obtain a new token.
6. Logout revokes the associated refresh session.

For browser applications, prefer secure, `HttpOnly` refresh-token cookies with appropriate `Secure` and `SameSite` settings. Protect cookie-based state-changing requests against CSRF where required. Keep access tokens short-lived and avoid storing sensitive long-lived tokens in browser local storage.

## 🛡️ Security Considerations

- Never store plain-text passwords.
- Validate and normalize incoming data.
- Never expose password hashes, token secrets, or internal errors in API responses.
- Use generic responses for password-reset requests to reduce account enumeration.
- Apply rate limits to login, registration, and recovery endpoints.
- Rotate refresh tokens and detect reuse where supported.
- Revoke sessions when users log out or reset their passwords.
- Use HTTPS in production.
- Validate configuration at startup.
- Keep dependencies updated and run automated security tests.


## 🧩 Reusing This Starter Kit

1. Clone the repository into your new project.
2. Configure `.env` for the new environment.
3. Connect the project's MongoDB database.
4. Configure frontend origins and email delivery.
5. Adjust user fields and roles for your application's needs.
6. Run the tests before deploying.


## 🤝 Contributing

Contributions, issues, and feature suggestions are welcome. Please open an issue to discuss significant changes before submitting a pull request.

## 📄 License

This project is intended to use the MIT License. Add the corresponding `LICENSE` file before publishing the repository.

---

**Built to eliminate repetitive backend authentication work and help you ship projects faster.**
