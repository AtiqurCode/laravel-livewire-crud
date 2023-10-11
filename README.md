<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400" alt="Laravel Logo"></a></p>

<p align="center">
<a href="https://github.com/laravel/framework/actions"><img src="https://github.com/laravel/framework/workflows/tests/badge.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>

## Clone and App run process

Basic requirements
-  PHP versions 8.1
-  composer
-  MySQL 8.* (any versions)

```
git clone 
composer install 
or
composer install --ignore-platform-reqs
```
Next update the ***.env*** file

Change these keys in the **.env** file

```
APP_URL=http://127.0.0.1:8000 # change this line app run URL

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel_livewire_crud
DB_USERNAME={user_name}
DB_PASSWORD={password}
```

Now run the migrate and app run command via the terminal
```
php artisan migrate
php artisan serve
```

## Code line 

First, install the livewire package
```
composer require livewire/livewire
```

Then create a component for users
```
php artisan make:livewire users
```

Now they created fies on both path:
```
app/Http/Livewire/Users.php
resources/views/livewire/users.blade.php
```

Update Component File

#### Here, we will write render(), resetInputFields(), store(), edit(), cancel(), update() and delete() method for our crud app.

So, let's update the following file ```App\Livewire\Users```.


```
<?php

namespace App\Livewire;

use Livewire\Component;
use App\Models\User;

class Users extends Component
{
    public $users, $name, $email, $user_id;
    public $updateMode = false;

    public function render()
    {
        $this->users = User::all();
        return view('livewire.users');
    }

    private function resetInputFields(){
        $this->name = '';
        $this->email = '';
    }

    public function store()
    {
        $validatedDate = $this->validate([
            'name' => 'required',
            'email' => 'required|email',
        ]);

        User::create($validatedDate);

        session()->flash('message', 'Users Created Successfully.');

        $this->resetInputFields();

        // $this->emit('userStore'); // Close model to using to jquery

    }

    public function edit($id)
    {
        $this->updateMode = true;
        $user = User::where('id',$id)->first();
        $this->user_id = $id;
        $this->name = $user->name;
        $this->email = $user->email;

    }

    public function cancel()
    {
        $this->updateMode = false;
        $this->resetInputFields();


    }

    public function update()
    {
        $validatedDate = $this->validate([
            'name' => 'required',
            'email' => 'required|email',
        ]);

        if ($this->user_id) {
            $user = User::find($this->user_id);
            $user->update([
                'name' => $this->name,
                'email' => $this->email,
            ]);
            $this->updateMode = false;
            session()->flash('message', 'Users Updated Successfully.');
            $this->resetInputFields();

        }
    }

    public function delete($id)
    {
        if($id){
            User::where('id',$id)->delete();
            session()->flash('message', 'Users Deleted Successfully.');
        }
    }
}
```

#### You can check the blade files code from here
- [blade template](https://github.com/AtiqurCode/laravel-livewire-crud/tree/master/resources/views/livewire)
  
You need to add home, users, create, update .blade.php file for full crud operations

#### update the web.php file

```
Route::view('users', 'livewire.home');
```

Now you can do crud operations from 
**APP_URL/users** or **127.0.0.1:8000/users**
