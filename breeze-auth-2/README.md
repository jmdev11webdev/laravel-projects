<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400" alt="Laravel Logo"></a></p>

<p align="center">
<a href="https://github.com/laravel/framework/actions"><img src="https://github.com/laravel/framework/workflows/tests/badge.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>

## About Laravel

Laravel is a web application framework with expressive, elegant syntax. We believe development must be an enjoyable and creative experience to be truly fulfilling. Laravel takes the pain out of development by easing common tasks used in many web projects, such as:

- [Simple, fast routing engine](https://laravel.com/docs/routing).
- [Powerful dependency injection container](https://laravel.com/docs/container).
- Multiple back-ends for [session](https://laravel.com/docs/session) and [cache](https://laravel.com/docs/cache) storage.
- Expressive, intuitive [database ORM](https://laravel.com/docs/eloquent).
- Database agnostic [schema migrations](https://laravel.com/docs/migrations).
- [Robust background job processing](https://laravel.com/docs/queues).
- [Real-time event broadcasting](https://laravel.com/docs/broadcasting).

Laravel is accessible, powerful, and provides tools required for large, robust applications.

## Learning Laravel

Laravel has the most extensive and thorough [documentation](https://laravel.com/docs) and video tutorial library of all modern web application frameworks, making it a breeze to get started with the framework. You can also check out [Laravel Learn](https://laravel.com/learn), where you will be guided through building a modern Laravel application.

If you don't feel like reading, [Laracasts](https://laracasts.com) can help. Laracasts contains thousands of video tutorials on a range of topics including Laravel, modern PHP, unit testing, and JavaScript. Boost your skills by digging into our comprehensive video library.

## Laravel Sponsors

We would like to extend our thanks to the following sponsors for funding Laravel development. If you are interested in becoming a sponsor, please visit the [Laravel Partners program](https://partners.laravel.com).

### Premium Partners

- **[Vehikl](https://vehikl.com)**
- **[Tighten Co.](https://tighten.co)**
- **[Kirschbaum Development Group](https://kirschbaumdevelopment.com)**
- **[64 Robots](https://64robots.com)**
- **[Curotec](https://www.curotec.com/services/technologies/laravel)**
- **[DevSquad](https://devsquad.com/hire-laravel-developers)**
- **[Redberry](https://redberry.international/laravel-development)**
- **[Active Logic](https://activelogic.com)**

## Contributing

Thank you for considering contributing to the Laravel framework! The contribution guide can be found in the [Laravel documentation](https://laravel.com/docs/contributions).

## Code of Conduct

In order to ensure that the Laravel community is welcoming to all, please review and abide by the [Code of Conduct](https://laravel.com/docs/contributions#code-of-conduct).

## Security Vulnerabilities

If you discover a security vulnerability within Laravel, please send an e-mail to Taylor Otwell via [taylor@laravel.com](mailto:taylor@laravel.com). All security vulnerabilities will be promptly addressed.

## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).

--- 

````markdown
## 📧 Email Verification Setup (Laravel Breeze + WSL2)

This project uses **Laravel Breeze** for authentication with **email verification enabled**. Users must verify their email addresses before accessing protected routes. The project is developed using **WSL2 Ubuntu**, connecting to **XAMPP MySQL on Windows**.

### 1. User Model

Ensure `app/Models/User.php` implements `MustVerifyEmail`:

```php
use Illuminate\Contracts\Auth\MustVerifyEmail;

class User extends Authenticatable implements MustVerifyEmail
{
    use HasFactory, Notifiable;
}
````

This tells Laravel that users must verify their email.

---

### 2. Built-in Breeze Email Verification Routes

Use Breeze’s **built-in controllers** for email verification (no custom controller needed):

```php
use App\Http\Controllers\Auth\EmailVerificationNotificationController;
use App\Http\Controllers\Auth\EmailVerificationPromptController;
use App\Http\Controllers\Auth\VerifyEmailController;

Route::get('/email/verify', [EmailVerificationPromptController::class, '__invoke'])
    ->middleware('auth')
    ->name('verification.notice');

Route::get('/email/verify/{id}/{hash}', [VerifyEmailController::class, '__invoke'])
    ->middleware(['auth', 'signed'])
    ->name('verification.verify');

Route::post('/email/verification-notification', [EmailVerificationNotificationController::class, 'store'])
    ->middleware(['auth', 'throttle:6,1'])
    ->name('verification.send');
```

* Avoids errors like `Call to undefined method VerifyEmailController::show()`.
* Users who are not verified are automatically redirected to `/email/verify`.

---

### 3. Protect Routes for Verified Users

```php
Route::middleware(['auth', 'verified'])->group(function () {
    Route::get('/dashboard', function () {
        return view('dashboard');
    })->name('dashboard');
});
```

* Only verified users can access these routes.
* Unverified users are redirected to `/email/verify`.

---

### 4. Mail Configuration

In `.env`:

```env
MAIL_MAILER=smtp
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=your_email@gmail.com
MAIL_PASSWORD=your_email_app_password
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=your_email@gmail.com
MAIL_FROM_NAME="${APP_NAME}"
```

* For Gmail, use an **App Password** if 2FA is enabled.
* For local development, `MAIL_MAILER=log` can be used to log emails instead of sending.

---

### 5. Notes on WSL2 + XAMPP

* Laravel runs in **WSL2 Ubuntu**.
* Database connection uses **XAMPP MySQL on Windows** via the Windows host IP (from `/etc/resolv.conf` in WSL2).
* This setup ensures email verification works while connecting Laravel in WSL2 to XAMPP MySQL.

---

### ✅ Summary

* Users must verify their email before accessing protected routes.
* Breeze built-in controllers handle email verification automatically.
* WSL2 Ubuntu connects seamlessly to XAMPP MySQL on Windows for database operations.

---

### 🌍 Making Email Verification Work Globally (Using ngrok – Free)

By default, Laravel generates email verification links using the value of `APP_URL`.  
If `APP_URL` is set to `http://localhost`, the verification link will only work on the local machine.

To allow **email verification links to be accessed globally** (from any device or network) during development, this project uses **ngrok**.

---

### 6. Exposing Laravel Publicly with ngrok (WSL2)

#### Step 1: Run Laravel in WSL2

```bash
php artisan serve --host=0.0.0.0 --port=8000
```

This allows external tools like ngrok to forward traffic to your Laravel app.

---

#### Step 2: Start ngrok

In a new WSL2 terminal:

```bash
ngrok http 8000
```

ngrok will generate a public HTTPS URL similar to:

```
https://abcd-1234.ngrok-free.app
```

---

#### Step 3: Update `APP_URL`

Copy the ngrok HTTPS URL and update `.env`:

```env
APP_URL=https://abcd-1234.ngrok-free.app
```

Then clear cached configuration:

```bash
php artisan config:clear
php artisan cache:clear
```

---

#### Step 4: Resend Verification Email

Log in with an **unverified account** and click **Resend Verification Email**.

The email verification link will now be:

```
https://abcd-1234.ngrok-free.app/email/verify/{id}/{hash}
```

✔ Accessible from any device  
✔ Works outside the local network  
✔ Uses Laravel’s signed & secure verification URLs  

---

### ⚠️ Important Notes About ngrok

* Free ngrok URLs **change every time ngrok restarts**
* Update `APP_URL` whenever the ngrok URL changes
* This setup is **recommended for development only**
* For production, use a real domain and hosting

---

### 🧠 Why This Works

* Laravel uses `APP_URL` to generate verification links
* ngrok exposes the local Laravel server to the internet
* Email verification remains secure via signed URLs
* MySQL (XAMPP on Windows) stays local and protected

---

### ✅ Final Setup Overview

* Laravel Breeze authentication with email verification
* WSL2 Ubuntu for Laravel development
* XAMPP MySQL on Windows connected via WSL2
* ngrok used to make email verification links globally accessible during development

```
