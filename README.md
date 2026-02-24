
# Shobee
The primary purpose of this project, titled **Shobee**, is to provide a custom-developed, web-based e-commerce system specifically designed for **multi-branch fashion retailers**. It aims to bridge the gap for small to medium-sized retailers who often struggle with fragmented workflows and manual inventory management.

Key objectives and features of the system include:

* **Centralized Management**: It empowers a single brand to manage multiple physical branches through a unified online platform. This helps store owners centralize inventory management, streamline online orders, and ensure a consistent customer experience across all locations.


* **Operational Efficiency**: The system provides tools for administrators to coordinate operations, monitor branch-specific stock levels in real-time, and manage product listings and sales reports.


* **Enhanced Customer Experience**: Customers can browse various fashion categories (like tops, pants, and dresses) by branch and complete purchases through a secure, user-friendly checkout process.


* **Automation and Scalability**: Beyond a basic online store, Shobee introduces automation for tasks such as order processing, payment logging, and stock updates, while ensuring data security and system scalability.


* **Support for Multiple Roles**: The platform supports distinct user roles, including customers (for shopping and reviews) and administrators (for oversight and inventory control).

---
The purpose of using REST APIs:

### 1. Enabling Module-to-Module Communication

The REST API serves as a bridge between separate modules, allowing them to exchange data and perform functions without being directly coupled.

* **Cart and Order Integration**: The Cart Module uses RESTful protocols (GET, POST, DELETE) to manage items and then serves as a bridge to the finalized order records in the Order Module.


* **Vendor and Product Integration**: The Product Module provides an API endpoint (e.g., `GET /api/products-by-vendor`) so the Vendor Module can retrieve specific product lists for a given vendor ID.



### 2. Standardizing Data Exchange

The documentation specifies a consistent format for every service, ensuring that the **Source Module** and **Target Module** understand the request and response.

* **Protocols**: Every web service listed uses the **RESTful protocol**.


* **Request/Response Formats**: Data is typically sent using standard parameters (like `user_id` or `product_id`) and consumed in a JSON format that includes a `success` boolean and a `data` object or `message` string.



### 3. Implementing Secure CRUD Operations

The REST API provides a structured way to perform Create, Read, Update, and Delete (CRUD) operations while enforcing security rules on the server side.

* **Public Access**: `GET` requests (like `index` and `show`) allow the public to browse published and vendor-approved products.


* **Authorized Mutations**: Operations that change data—such as `POST` (create), `PUT` (update), and `DELETE` (destroy)—are protected by **Sanctum authentication middleware** to ensure only authorized users or administrators can modify records.



### 4. Supporting the "For Website" Logic

The API endpoints are designed to filter data specifically for the end-user experience. For example, the `index` and `show` functions in the `ProductApiController` use a `forWebsite()` query scope to ensure that only products that are both published and approved by a vendor are exposed via the API.

### Summary of Documented API Functions
* **Cart Management**: Listing, adding, updating, and removing items from a user's cart.


* **Product Management**: Retrieving all products, getting a specific product by ID, and allowing vendors or admins to create, edit, or delete listings.

---
![Laravel](https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-777BB4?style=for-the-badge&logo=php&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=node.js&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Stripe](https://img.shields.io/badge/Stripe-008CDD?style=for-the-badge&logo=stripe&logoColor=white)


---

## ℹ️ Information
This project is built using **Windows Subsystem for Linux (WSL)** with the **Ubuntu distribution**.  
Follow the installation and configuration steps below to set up the development environment.

---

## 📑 Table of Contents
- [Installation Guide](#installation-guide)
  1. [WSL Setup](#1-wsl-setup)
  2. [PHP & Composer](#2-php--composer)
  3. [Node.js](#3-nodejs)
  4. [Composer Dependencies](#4-composer-dependencies)
  5. [Environment File](#5-environment-file)
  6. [App Key](#6-app-key)
  7. [Database Setup](#7-database-setup)
  8. [Mailpit](#8-mailpit)
  9. [Stripe](#9-stripe)
  10. [Default User Credentials](#10-default-user-credentials)
- [Running the Project](#running-the-project)

---

## Installation Guide

### 1. WSL Setup
If you are on Windows, install WSL with Ubuntu:  
👉 [WSL Installation Guide](https://learn.microsoft.com/en-us/windows/wsl/install)

---

### 2. PHP & Composer
Install PHP, Composer, and the Laravel installer:
```bash
/bin/bash -c "$(curl -fsSL https://php.new/install/linux/8.4)"
```

---

### 3. Node.js

* If Node.js is already installed, refresh your shell:

```bash
hash -r
```

* If not, install **NVM** and Node.js:

```bash
curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc

nvm install 22
nvm use 22
```

* Install project dependencies:

```bash
npm install
npm run build
```

---

### 4. Composer Dependencies

```bash
composer install
```

---

### 5. Environment File

Copy the example environment file:

```bash
cp .env.example .env
```

---

### 6. App Key

Generate a new application key:

```bash
php artisan key:generate
```

---

### 7. Database Setup

Install and configure MySQL:

```bash
sudo apt install mysql-server
sudo systemctl status mysql
```

If running, log in:

```bash
sudo mysql
```

Set a password:

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
FLUSH PRIVILEGES;
exit;
```

Update `.env`:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=new_password
```

Run migrations with seeders:

```bash
php artisan migrate --seed
```

---

### 8. Mailpit

Install Mailpit for email notifications (password reset, order confirmation, etc.):

```bash
sudo sh < <(curl -sL https://raw.githubusercontent.com/axllent/mailpit/develop/install.sh)
```

---

### 9. Stripe

#### 9.1 Install Stripe CLI

1. Add GPG key:

```bash
curl -s https://packages.stripe.dev/api/security/keypair/stripe-cli-gpg/public | gpg --dearmor | sudo tee /usr/share/keyrings/stripe.gpg
```

2. Add repository:

```bash
echo "deb [signed-by=/usr/share/keyrings/stripe.gpg] https://packages.stripe.dev/stripe-cli-debian-local stable main" | sudo tee -a /etc/apt/sources.list.d/stripe.list
```

3. Update package list:

```bash
sudo apt update
```

4. Install Stripe CLI:

```bash
sudo apt install stripe
```

#### 9.2 Configure Stripe

1. Register an account at [Stripe](https://stripe.com).
2. Create a **sandbox project**.
3. Copy API keys into `.env`:

```env
STRIPE_KEY=<Publishable key>
STRIPE_SECRET=<Secret key>
```

#### 9.3 Stripe CLI Login

```bash
stripe login
```

Press **Enter** to authenticate via browser.

Example output:

```output
Your pairing code is: enjoy-enough-outwit-win
Press Enter to open the browser or visit https://dashboard.stripe.com/stripecli/confirm_auth
```
---
### 10. Default User Credentials


## Default User Credentials

Once you’ve set up the application and seeded the database, you can log in with the following default user credentials:

### Users:

* **User**

  * Email: `user@example.com`
  * Password: `password`

* **Vendor**

  * Email: `vendor@example.com`
  * Password: `password`

* **Admin**

  * Email: `admin@example.com`
  * Password: `password`

These credentials can be used to test different user roles and functionalities within the application. Make sure to change the passwords and details in a production environment.
---

## Running the Project

Run the following commands in separate terminals:

1. Start Laravel:

```bash
composer run dev
```

2. Start Mailpit:

```bash
mailpit
```

3. Start Stripe webhook listener:

```bash
stripe listen --forward-to http://localhost:8000/stripe/webhook
```

Update `.env` with the new webhook secret:

```env
STRIPE_WEBHOOK_SECRET=<WEBHOOK_SECRET>
```

---

✅ You are now ready to run **Shobee** locally!
