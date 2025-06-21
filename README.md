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





# Laravel CRUD Operations – Step-by-Step Guide

This guide walks you through the steps to create **CRUD (Create, Read, Update, Delete)** operations in a Laravel application.

---

## 🧪 Sample Model Creation

If not already created, generate the `User` model and migration:

```bash
php artisan make:model User -m
```

Update the migration file with desired columns:

```php
Schema::create('users', function (Blueprint $table) {
    $table->id();
    $table->string('firstName');
    $table->string('lastName');
    $table->string('email')->unique();
    $table->timestamps();
});
```

Run the migration:

```bash
php artisan migrate
```

---


## 🧭 Steps For Creating CRUD Operations

---

### ✅ **Step 1: Create A Route For Opening List of Customers Page**

#### 1. Create a View File

```bash
php artisan make:view customers-list
```

> This command creates a Blade view named `customers-list.blade.php`.

#### 2. Add a Route to Display the List Page

```php
Route::get('/customers', function () {
    return view('customers-list');
})->name('customers.list');
```

---

### ✅ **Step 2: 'Create a New Customer' Page And Store Customer Details**

#### 1. Create a New View File for the Form

```bash
php artisan make:view create-customer
```

> Add your form to this view using the syntax below:

```blade
<form class="row g-3" action="{{ route('customer.add') }}" method="POST">
    @csrf
    @method('POST')

    <!-- Form Inputs -->
    <input type="text" name="firstName" />
    <input type="text" name="lastName" />
    <input type="email" name="email" />

    <button type="submit">Submit</button>
</form>
```

#### 2. Create a Route to Handle the Form Submission

```php
Route::post('/customers/add', [CustomerController::class, 'addCustomer'])->name('customer.add');
```

#### 3. Create the Controller File

```bash
php artisan make:controller CustomerController
```

> This will create `app/Http/Controllers/CustomerController.php`.

#### 4. Store Form Data in Controller

```php
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Validator;
use App\Models\User;

public function addCustomer(Request $request)
{
    $validator = Validator::make($request->all(), [
        'firstName' => 'required|min:3',
        'lastName' => 'required|min:3',
        'email' => 'required|email|unique:users,email',
    ]);

    if ($validator->fails()) {
        return back()->withErrors($validator)->withInput();
    }

    $customer = new User();
    $customer->firstName = $request->firstName;
    $customer->lastName = $request->lastName;
    $customer->email = $request->email;
    $customer->save();

    return redirect()->route('customers.list')->with('success', 'Customer added successfully.');
}
```

---

### ✅ **Step 3: Read and Display All Customers**

#### 1. Update Route and Controller

```php
Route::get('/customers', [CustomerController::class, 'listCustomers'])->name('customers.list');
```

#### 2. Controller Function to Fetch and Display Data

```php
public function listCustomers()
{
    $customers = User::all();
    return view('customers-list', compact('customers'));
}
```

#### 3. Display Customers in View

```blade
@foreach ($customers as $customer)
    <p>{{ $customer->firstName }} {{ $customer->lastName }} ({{ $customer->email }})</p>
@endforeach
```

---

### ✅ **Step 4: Update Customer Details**

#### 1. Add Route to Show Edit Form

```php
Route::get('/customers/edit/{id}', [CustomerController::class, 'editCustomer'])->name('customer.edit');
```

#### 2. Controller Function for Edit

```php
public function editCustomer($id)
{
    $customer = User::findOrFail($id);
    return view('edit-customer', compact('customer'));
}
```

#### 3. Create Edit Form View

```bash
php artisan make:view edit-customer
```

```blade
<form action="{{ route('customer.update', $customer->id) }}" method="POST">
    @csrf
    @method('PUT')

    <input type="text" name="firstName" value="{{ $customer->firstName }}">
    <input type="text" name="lastName" value="{{ $customer->lastName }}">
    <input type="email" name="email" value="{{ $customer->email }}">

    <button type="submit">Update</button>
</form>
```

#### 4. Add Update Route and Controller Logic

```php
Route::put('/customers/update/{id}', [CustomerController::class, 'updateCustomer'])->name('customer.update');
```

```php
public function updateCustomer(Request $request, $id)
{
    $validator = Validator::make($request->all(), [
        'firstName' => 'required|min:3',
        'lastName' => 'required|min:3',
        'email' => 'required|email|unique:users,email,' . $id,
    ]);

    if ($validator->fails()) {
        return back()->withErrors($validator)->withInput();
    }

    $customer = User::findOrFail($id);
    $customer->firstName = $request->firstName;
    $customer->lastName = $request->lastName;
    $customer->email = $request->email;
    $customer->save();

    return redirect()->route('customers.list')->with('success', 'Customer updated successfully.');
}
```

---

### ✅ **Step 5: Delete Customer**

#### 1. Add Route for Delete

```php
Route::delete('/customers/delete/{id}', [CustomerController::class, 'deleteCustomer'])->name('customer.delete');
```

#### 2. Controller Logic

```php
public function deleteCustomer($id)
{
    $customer = User::findOrFail($id);
    $customer->delete();

    return redirect()->route('customers.list')->with('success', 'Customer deleted successfully.');
}
```

#### 3. Add Delete Button in List View

```blade
<form action="{{ route('customer.delete', $customer->id) }}" method="POST" onsubmit="return confirm('Are you sure?')">
    @csrf
    @method('DELETE')
    <button type="submit">Delete</button>
</form>
```

---


## ✅ Summary

| Operation | Method | Route | Controller |
|----------|--------|-------|------------|
| List | GET | `/customers` | `listCustomers()` |
| Create | GET + POST | `/customers/add` | `addCustomer()` |
| Update | GET + PUT | `/customers/edit/{id}` + `/customers/update/{id}` | `editCustomer()`, `updateCustomer()` |
| Delete | DELETE | `/customers/delete/{id}` | `deleteCustomer()` |

---

You now have a complete CRUD system in Laravel! ✅
