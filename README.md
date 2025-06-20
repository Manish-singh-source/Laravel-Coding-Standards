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


# For Existing Project Setup:

## Step 1: Copy .env.example to .env:
```
cp .env.example .env
```

The .env file holds the environment variables like database credentials, API keys, etc.

## Step 2: Install PHP dependencies:
```
composer install
```

This command installs all PHP dependencies listed in the composer.json file.

## Step 3: Generate the application key:
```
php artisan key:generate
```

This generates a new key in the .env file that is used for encryption.

## Step 4: Run database migrations (if necessary):
```
php artisan migrate
```

This command applies the database migrations and sets up the necessary database structure.

## Step 5: Serve the application:
```
php artisan serve
```

Starts the application on a local server (usually at http://localhost:8000).


## Create View File:
```
php artisan make:view <fileName>
```


## Create Route : Open web.php file
```
// view all customer page 
Route::get('/customer', [UserController::class, 'functionName']);
// open add customer page
Route::get('/customer/create', [UserController::class, 'functionName']);
// store add customer data
Route::post('/customer/store', [UserController::class, 'functionName']);
// go to edit page 
Route::get('/customer/edit/{id}', [UserController::class, 'functionName']);
// store edit page data
Route::put('/customer/edit/{id}', [UserController::class, 'functionName']);
// delete customer data
Route::delete('/customer/{id}', [UserController::class, 'functionName']);
// view individual customer data
Route::get('/customer/{id}', [UserController::class, 'functionName']);

```

## Create Controller File:
```
php artisan make:controller <ControllerName>
```

First letter capital : e.x. AuthController 






## Steps For Creating CRUD Operations

### **Step 1: Create A Route For Opening List of Customers Page**

1. Create a View File and then add PHP code in it:

   ```
      php artisan make:view <fileName>
   ```

   Example:

   ```
      php artisan make:view customers-list
   ```

2. **Displaying List Page:** Displaying above page using following route.

   ```
      Route::get('/url', function () {
         return view('pageName');
      })->name('nameForLinking');
   ```

   Example:

   ```
      Route::get('/url', function () {
         return view('customers-list');
      })->name('customers.list');
   ```



### **Step 2: 'Create a New Customer' Page And Store Customer Details**


1. **Create a New Customer Page:** Using same above step. And Add additional form details as below 

   ```
   <form class="row g-3" action="{{ route('nameForRoute') }}" method="POST">
      @csrf
      @method('POST')

      .... Form inputs with proper name attribute 

   </form>
   ```

   Example: Submit data on customers.list named route with get/post/put/delete method
   ```
   <form class="row g-3" action="{{ route('customers.list') }}" method="POST">
      @csrf
      @method('POST')

      .... Form inputs with proper name attribute 

   </form>
   ```

2. **Make a Route With Post Method For Storing Form Data:**
   ```
      Route::post('/url', [NameForController::class, 'functionInController'])->name('nameForRoute');
   ```

   Example:

   ```
      Route::post('/url', [CustomerController::class, 'addCustomer'])->name('customer.add');
   ```

3. **Create a Controller File** Create Controller File with proper name.

   ```
      php artisan make:controller <ControllerName>
   ```

   Example:

   ```
      php artisan make:controller CustomerController
   ```
