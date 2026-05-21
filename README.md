# 🔐 java-auth-service

> Production-ready JWT authentication microservice with Spring Security, refresh tokens, role management, and OAuth2 social login.

[![Java](https://img.shields.io/badge/Java-17-ED8B00?style=flat-square&logo=openjdk)](https://openjdk.org/)
[![Spring Security](https://img.shields.io/badge/Spring_Security-6DB33F?style=flat-square&logo=springsecurity)](https://spring.io/projects/spring-security)
[![JWT](https://img.shields.io/badge/JWT-Auth-000000?style=flat-square&logo=jsonwebtokens)](https://jwt.io/)
[![MySQL](https://img.shields.io/badge/MySQL-8-4479A1?style=flat-square&logo=mysql)](https://mysql.com)
[![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?style=flat-square&logo=docker)](https://docker.com)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)

---

## 🚀 Features

- ✅ JWT access and refresh token pair
- ✅ Token blacklisting on logout
- ✅ Role-based access control (RBAC)
- ✅ Google and GitHub OAuth2 social login
- ✅ Email verification workflow
- ✅ Password reset via email token
- ✅ Brute-force protection with account lockout
- ✅ Audit log for all login events
- ✅ Swagger UI at `/swagger-ui.html`
- ✅ Docker Compose single-command startup

---

## 🗂️ Project Structure

```
java-auth-service/
├── src/
│   └── main/
│       ├── java/com/blackrootbit/auth/
│       │   ├── AuthServiceApplication.java
│       │   ├── config/
│       │   │   ├── SecurityConfig.java
│       │   │   ├── JwtConfig.java
│       │   │   └── OAuth2Config.java
│       │   ├── controller/
│       │   │   ├── AuthController.java
│       │   │   ├── UserController.java
│       │   │   └── AdminController.java
│       │   ├── service/
│       │   │   ├── AuthService.java
│       │   │   ├── JwtService.java
│       │   │   ├── UserService.java
│       │   │   └── EmailService.java
│       │   ├── security/
│       │   │   ├── JwtAuthFilter.java
│       │   │   ├── JwtTokenProvider.java
│       │   │   └── CustomUserDetailsService.java
│       │   ├── model/
│       │   │   ├── User.java
│       │   │   ├── Role.java
│       │   │   ├── RefreshToken.java
│       │   │   └── AuditLog.java
│       │   ├── repository/
│       │   │   ├── UserRepository.java
│       │   │   └── RefreshTokenRepository.java
│       │   ├── dto/
│       │   │   ├── LoginRequest.java
│       │   │   ├── RegisterRequest.java
│       │   │   ├── AuthResponse.java
│       │   │   └── TokenRefreshRequest.java
│       │   └── exception/
│       │       ├── GlobalExceptionHandler.java
│       │       └── AuthException.java
│       └── resources/
│           └── application.yml
├── docker-compose.yml
├── Dockerfile
├── pom.xml
└── README.md
```

---

## 🔌 API Endpoints

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| `POST` | `/api/v1/auth/register` | Public | Register new account |
| `POST` | `/api/v1/auth/login` | Public | Login and get token pair |
| `POST` | `/api/v1/auth/refresh` | Public | Exchange refresh token |
| `POST` | `/api/v1/auth/logout` | Bearer | Invalidate tokens |
| `POST` | `/api/v1/auth/forgot-password` | Public | Send reset email |
| `POST` | `/api/v1/auth/reset-password` | Token | Set new password |
| `GET` | `/api/v1/auth/verify-email` | Token | Verify email address |
| `GET` | `/api/v1/users/me` | Bearer | Get current user profile |
| `PUT` | `/api/v1/users/me` | Bearer | Update profile |
| `GET` | `/api/v1/admin/users` | ADMIN | List all users |

---

## 🔐 JWT Authentication Flow

```
1. Client sends POST /api/v1/auth/login
   Body: { "email": "user@example.com", "password": "secret123" }

2. Server responds with token pair:
   {
     "accessToken":  "eyJhbGciOiJIUzI1NiJ9...",
     "refreshToken": "a8f3d2c1-4b5e-...",
     "tokenType":    "Bearer",
     "expiresIn":    900
   }

3. Client uses access token in every request:
   Authorization: Bearer eyJhbGciOiJIUzI1NiJ9...

4. When access token expires, refresh it:
   POST /api/v1/auth/refresh
   Body: { "refreshToken": "a8f3d2c1-4b5e-..." }
```

---

## 🗄️ Database Schema

```sql
CREATE TABLE users (
    id            BIGINT AUTO_INCREMENT PRIMARY KEY,
    email         VARCHAR(255) UNIQUE NOT NULL,
    password      VARCHAR(255) NOT NULL,
    full_name     VARCHAR(255),
    role          VARCHAR(50) DEFAULT 'USER',
    is_verified   BOOLEAN DEFAULT FALSE,
    is_locked     BOOLEAN DEFAULT FALSE,
    failed_attempts INT DEFAULT 0,
    created_at    TIMESTAMP DEFAULT NOW()
);

CREATE TABLE refresh_tokens (
    id          BIGINT AUTO_INCREMENT PRIMARY KEY,
    user_id     BIGINT REFERENCES users(id),
    token       VARCHAR(500) UNIQUE NOT NULL,
    expiry_date TIMESTAMP NOT NULL,
    is_revoked  BOOLEAN DEFAULT FALSE
);

CREATE TABLE audit_logs (
    id          BIGINT AUTO_INCREMENT PRIMARY KEY,
    user_id     BIGINT REFERENCES users(id),
    action      VARCHAR(100),
    ip_address  VARCHAR(50),
    created_at  TIMESTAMP DEFAULT NOW()
);
```

---

## 🐳 Quick Start

```bash
# Clone the repo
git clone https://github.com/black-root-bit/java-auth-service.git
cd java-auth-service

# Start with Docker
docker compose up -d

# Swagger UI available at:
# http://localhost:8081/swagger-ui.html
```

---

## ⚙️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Language | Java 17 |
| Framework | Spring Boot 3.2 |
| Security | Spring Security + JWT |
| Database | MySQL 8 |
| Cache | Redis |
| Docs | Swagger / OpenAPI 3 |
| Container | Docker |
| Build | Maven |

---

## 📄 License

MIT © [black-root-bit](https://github.com/black-root-bit)
