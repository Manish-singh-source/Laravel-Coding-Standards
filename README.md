# Laravel-Coding-Standards

## Prerequisites
Make sure you have the following installed:

- PHP ≥ 8.1
- Composer
- MySQL or SQLite for database
- Xampp


## Step 1: Create a New Laravel Project
```
composer create-project laravel/laravel <projectName>
```

Go into your project directory:
```
cd <projectName>
```


## Step 2: Run Your App
Start your local server:

```
php artisan serve
```

Now visit http://127.0.0.1:8000
You'll see the home page.


## Step 3: Connect Mysql Database By Setting Up Environment Variables
Edit .env file with your DB details:

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=database_name
DB_USERNAME=root
DB_PASSWORD=your_password
```

Then run:
```
php artisan migrate
```

