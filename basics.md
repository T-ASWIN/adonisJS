
---

# AdonisJS Project – README

## 📌 Overview

This project is built using **AdonisJS**, a full-stack Node.js framework designed for building scalable and maintainable server-side applications with batteries included.

---

## 🔑 Important Concepts

1. **Controller**
2. **View**
3. **Service & Validation**
4. **Pagination**
5. **Routing & Middleware**

---

## ⚡ Key Features (Batteries Included)

1. **Opinionated Framework**
   AdonisJS enforces best practices and a structured approach.

2. **Batteries Included**
   Comes with built-in support for:

   * Authentication
   * ORM (Lucid)
   * Validation
   * Routing
   * Security
   * Configuration & Environment Variables
   * Redis

3. **EdgeJS**
   Template engine used by AdonisJS for server-side rendering.

4. **VineJS**
   Official validation system used in AdonisJS.

5. **Environment Variable System**
   Built-in `.env` support for managing configuration securely.

6. **Japa**
   Testing framework used in AdonisJS.

---

## 🧩 Pre-requisites

### 1. Node.js

AdonisJS runs inside the Node.js environment.

* **Current version:** `v20.17.0`

#### Alternative: FNM (Fast Node Manager)

FNM is a Node.js version manager that helps you:

* Install multiple Node versions
* Switch versions easily
* Maintain consistent environments

---

### 2. PostgreSQL

Used as the primary relational database.

---

### 3. Redis (Caching Database)

Install Redis:

```bash
sudo apt install redis-server -y
```

Start Redis:

```bash
sudo service redis-server start
```

Verify Redis:

```bash
redis-cli ping
```

Expected output:

```text
PONG
```

Enable Redis on WSL startup:

```bash
sudo systemctl enable redis-server
```

---

### 4. Unzip Utility

If a file is compressed as something.zip, unzip helps you open and extract its contents.

🔍 Why is Unzip Needed?

Many tools, frameworks, and packages (including some Node.js and AdonisJS dependencies) are downloaded in ZIP format.
Without the unzip utility, your system cannot extract those files.

Required to extract downloaded packages:

```bash
sudo apt install unzip -y
```

---

## 🚀 Installing AdonisJS

### Step 1: Create a New Project

```bash
npm init adonisjs@latest firstproject
```

---

### Step 2: Select Project Type

Choose **Session-based Authentication** if your app needs:

* Admin dashboards
* Web applications
* Server-rendered pages
* Browser-based apps

This uses cookies and sessions for authentication.

---

### Step 3: Configure Database

Select and configure your database (PostgreSQL recommended).

---

### Step 4: Run the Development Server

```bash
npm run dev
```

---

## 📁 Folder Structure & Core Components

### ACE (Adonis Command Engine)

ACE is the CLI tool used to:

* Generate files (controllers, models, migrations)
* Run migrations
* Manage the application

Example:

```bash
node ace make:controller User
```

---

### Batteries Included Modules

AdonisJS provides ready-to-use modules without extra installation:

* Authentication
* ORM (Lucid)
* Validation (VineJS)
* Routing
* Security
* Configuration & Environment
* Redis Support

---

