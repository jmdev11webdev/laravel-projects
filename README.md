# Laravel Projects

A curated collection of Laravel projects showcasing **progressive learning and real-world application development** without using any starter kits such as breeze, jetstream, livewire, and more, this repository named Laravel Projects is created for the purpose of understanding and creating projects with laravel in only using the programming languages related and applicable with laravel. 

This repository also serves as a **portfolio and learning archive**, developed using for a fast, stable, and native development workflow on Windows.

---

## Purpose of This Repository

The main goals of this repository are to:

* Learn and apply Laravel fundamentals and best practices
* Gradually move toward advanced Laravel architectures
* Build scalable, maintainable, and clean Laravel applications
* Track personal growth from beginner to advanced Laravel development

Each project represents a **specific stage of Laravel proficiency**.

---

## Project Levels

### Beginner Projects

* Laravel installation and configuration
* Routing, controllers, and Blade templates
* Form validation and request handling
* Authentication using Laravel Breeze

### Intermediate Projects

* Authentication and authorization
* Role-based access control
* Jetstream features (profile management, teams)
* Database relationships and migrations
* CRUD systems and RESTful controllers

### Advanced Projects

* Modular and scalable architecture
* Inertia.js or Livewire integration
* API-based Laravel applications
* Notifications, queues, and background jobs
* Activity logs and auditing
* Performance and security optimizations

---

## Repository Structure

```text
laravel-projects/
│
├── (name of project)
│
└── README.md
```

Each folder is an **independent Laravel project** with its own configuration and setup.

---

## Development Environment

This repository is developed using:

* PHP 8.5 or later
* Composer
* Laravel Framework
* MySQL or MariaDB
* Node.js and npm
* Visual Studio Code
  
---

## Getting Started

### Clone the Repository

```bash
git clone git@github.com:your-username/laravel-projects.git
cd laravel-projects
```

To use Laravel make sure you have installed PHP 8.0+ versions, composer, and nodejs.

### 1️⃣ Install dependencies

```bash
npm install
```

---

### 3️⃣ Environment setup

```bash
cp .env.example .env
php artisan key:generate
```

Configure your database credentials inside the `.env` file, then run:

```bash
php artisan migrate
```

---

### 4️⃣ Run frontend assets (if you installed jetstream and with vuejs)

```bash
npm run dev
```

---

### 5️⃣ Run the application

```bash
php artisan serve
```

Your application should now be accessible at:

```
http://127.0.0.1:8000
```

---

### Run Any Project

```bash
composer install
npm install
cp .env.example .env
php artisan key:generate
php artisan migrate
npm run dev
php artisan serve
```

---

## Notes

* Each project uses its own `.env` file
* Database credentials are never committed
* Additional setup steps may exist inside individual project folders
* Intended for learning, experimentation, and portfolio demonstration

---

## Future Plans

* Add more real-world Laravel applications
* API-first and backend-focused Laravel projects
* Improved testing and code quality
* Deployment-ready project configurations

---

## License

This repository is open-source and available under the MIT License.

---

## Author

JMDevStack
