<p align="center">
  <img src="https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" alt="Node.js" />
  <img src="https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white" alt="Express" />
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white" alt="PostgreSQL" />
  <img src="https://img.shields.io/badge/Passport.js-34E27A?style=for-the-badge&logo=passport&logoColor=black" alt="Passport.js" />
  <img src="https://img.shields.io/badge/Google_OAuth-4285F4?style=for-the-badge&logo=google&logoColor=white" alt="Google OAuth" />
  <img src="https://img.shields.io/badge/EJS-B4CA65?style=for-the-badge&logo=ejs&logoColor=black" alt="EJS" />
  <img src="https://img.shields.io/badge/License-ISC-blue?style=for-the-badge" alt="ISC License" />
</p>

# 🤫 Scribble — Share Your Secrets Anonymously

> A full-stack web application where users can register, log in, and share their secrets anonymously. Built with **Node.js**, **Express**, **PostgreSQL**, and **Passport.js** — supporting both local email/password and **Google OAuth 2.0** authentication.

---

## 📑 Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Architecture](#%EF%B8%8F-architecture)
- [Getting Started](#-getting-started)
- [Environment Variables](#-environment-variables)
- [Database Setup](#-database-setup)
- [Project Structure](#-project-structure)
- [Routes](#-routes)
- [Screenshots](#-screenshots)
- [Contributing](#-contributing)
- [License](#-license)
- [Author](#-author)

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🔐 **Local Authentication** | Register & log in with email and password (hashed with bcrypt) |
| 🌐 **Google OAuth 2.0** | One-click sign in with your Google account |
| 🤫 **Share Secrets** | Submit your secret anonymously after authentication |
| 👀 **View Secrets** | See your secret on a protected page |
| 🚪 **Session Management** | Persistent login sessions via express-session |
| 🔒 **Password Security** | Passwords are salted and hashed (10 rounds of bcrypt) |
| 🛡️ **Protected Routes** | Secrets and submit pages require authentication |

---

## 🛠️ Tech Stack

| Technology | Version | Purpose |
|:-----------|:--------|:--------|
| [Node.js](https://nodejs.org/) | ≥ 14.x | JavaScript runtime environment |
| [Express](https://expressjs.com/) | ^4.18.2 | Web framework |
| [PostgreSQL](https://www.postgresql.org/) | — | Relational database for users & secrets |
| [Passport.js](http://www.passportjs.org/) | ^0.7.0 | Authentication middleware |
| [passport-local](http://www.passportjs.org/packages/passport-local/) | ^1.0.0 | Email/password authentication strategy |
| [passport-google-oauth2](http://www.passportjs.org/packages/passport-google-oauth2/) | ^0.2.0 | Google OAuth 2.0 strategy |
| [bcrypt](https://www.npmjs.com/package/bcrypt) | ^5.1.1 | Password hashing |
| [EJS](https://ejs.co/) | ^3.1.9 | Server-side templating engine |
| [express-session](https://www.npmjs.com/package/express-session) | ^1.17.3 | Session management |
| [dotenv](https://www.npmjs.com/package/dotenv) | ^16.3.1 | Environment variable management |
| [pg](https://node-postgres.com/) | ^8.11.3 | PostgreSQL client for Node.js |

---

## 🏗️ Architecture

```
┌──────────────┐       ┌──────────────────┐       ┌──────────────────┐
│              │       │                  │       │                  │
│   Browser    │◄─────►│   Express App    │◄─────►│   PostgreSQL     │
│              │       │   (port 3000)    │       │   Database       │
│              │       │                  │       │                  │
└──────────────┘       └────────┬─────────┘       └──────────────────┘
                                │
                       ┌────────┴─────────┐
                       │                  │
                  ┌────▼────┐       ┌─────▼─────┐
                  │ Local   │       │  Google    │
                  │ Strategy│       │  OAuth 2.0 │
                  │ (email) │       │  Strategy  │
                  └─────────┘       └───────────┘
```

**Authentication Flow:**

1. **Register** → Password hashed with bcrypt → Stored in PostgreSQL
2. **Login (Local)** → bcrypt compares hashed passwords → Session created
3. **Login (Google)** → OAuth 2.0 flow → User upserted in DB → Session created
4. **Access Secrets** → Session checked → Secret rendered from DB

---

## 🚀 Getting Started

### Prerequisites

- **[Node.js](https://nodejs.org/)** v14 or higher
- **[PostgreSQL](https://www.postgresql.org/)** installed and running
- **[Google Cloud Console](https://console.cloud.google.com/)** project (for OAuth)

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/priyanshusahu898218/Scribble.git
cd Scribble

# 2. Install dependencies
npm install

# 3. Set up environment variables (see section below)
cp .env.example .env
# Edit .env with your actual credentials

# 4. Set up the database (see Database Setup section)

# 5. Start the server
node index.js
```

> 🟢 Server running at `http://localhost:3000`

---

## 🔑 Environment Variables

Create a `.env` file in the root directory with the following variables:

```env
# Session
SESSION_SECRET=your_session_secret_here

# PostgreSQL Database
PG_USER=your_postgres_username
PG_HOST=localhost
PG_DATABASE=your_database_name
PG_PASSWORD=your_postgres_password
PG_PORT=5432

# Google OAuth 2.0
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
```

### Getting Google OAuth Credentials

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project (or select an existing one)
3. Navigate to **APIs & Services** → **Credentials**
4. Click **Create Credentials** → **OAuth 2.0 Client ID**
5. Set **Authorized redirect URI** to: `http://localhost:3000/auth/google/secrets`
6. Copy the **Client ID** and **Client Secret** to your `.env` file

---

## 🗄️ Database Setup

1. **Create the database** in PostgreSQL:

```sql
CREATE DATABASE your_database_name;
```

2. **Create the users table:**

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255),
    secret TEXT
);
```

> 💡 If you already have the `users` table without the `secret` column, run:
> ```sql
> ALTER TABLE users ADD COLUMN secret TEXT;
> ```

---

## 📁 Project Structure

```
Scribble/
├── 📄 index.js                # Main application — routes, auth, & database logic
├── 📄 package.json            # Project metadata & dependencies
├── 📄 package-lock.json       # Dependency lock file
├── 📄 solution-queries.sql    # SQL migration queries
├── 📄 .env                    # Environment variables (not committed)
├── 📄 .gitignore              # Git ignore rules
├── 📂 views/
│   ├── 📄 home.ejs            # Landing page
│   ├── 📄 login.ejs           # Login page (local + Google)
│   ├── 📄 register.ejs        # Registration page
│   ├── 📄 secrets.ejs         # Protected — displays user's secret
│   ├── 📄 submit.ejs          # Protected — submit a new secret
│   └── 📂 partials/
│       ├── 📄 header.ejs      # Shared HTML head & navbar
│       └── 📄 footer.ejs      # Shared footer
├── 📂 public/
│   └── 📂 css/
│       └── 📄 styles.css      # Application stylesheet
└── 📂 css/
    └── 📄 styles.css          # Additional styles
```

---

## 🛣️ Routes

| Method | Route | Auth Required | Description |
|:-------|:------|:-------------:|:------------|
| `GET` | `/` | ❌ | Landing / home page |
| `GET` | `/login` | ❌ | Login page |
| `GET` | `/register` | ❌ | Registration page |
| `POST` | `/login` | ❌ | Process login (Passport local strategy) |
| `POST` | `/register` | ❌ | Process registration (bcrypt hash + DB insert) |
| `GET` | `/auth/google` | ❌ | Initiate Google OAuth 2.0 flow |
| `GET` | `/auth/google/secrets` | ❌ | Google OAuth callback |
| `GET` | `/secrets` | ✅ | View your secret |
| `GET` | `/submit` | ✅ | Secret submission form |
| `POST` | `/submit` | ✅ | Save secret to database |
| `GET` | `/logout` | ✅ | Destroy session and log out |

---

## 📸 Screenshots

> _Coming soon — screenshots of the application UI will be added here._

<!--
Add screenshots like this:
![Home Page](./screenshots/home.png)
![Login Page](./screenshots/login.png)
![Secrets Page](./screenshots/secrets.png)
-->

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

---

## 📌 Important Notes

- ⚠️ **Never commit your `.env` file** — it contains sensitive credentials
- 🔐 Passwords are hashed with **bcrypt** (10 salt rounds) before storage
- 🗄️ Requires a running **PostgreSQL** instance
- 📦 Uses **ES Modules** (`"type": "module"` in package.json)
- 🌐 Google OAuth callback is set to `http://localhost:3000/auth/google/secrets`

---

## 📄 License

This project is licensed under the **ISC License**.

---

## 👤 Author

**Priyanshu Sahu**

- GitHub: [@priyanshusahu898218](https://github.com/priyanshusahu898218)

---

<p align="center">
  Made with ❤️ and JavaScript
</p>
