
## Laravel REST API with Passport

Here, we are going to use Auth API with Passport authentication. We will only use and if needed update accordingly.

## How to implement

How to implement Passport in REST API?

**1) Create laravel project.**

        composer create-project --prefer-dist laravel/laravel laraRestApi
        
**2) Install Passport**

Go to your project path in terminal and run below command for passport authentication.
        
        composer require laravel/passport
        
After the successful installation of a package, we required to get default migration for creating new passport tables in our database.  let's run below command:

    php artisan migrate
    
Next, we need to install the Passport using command, and it will create token keys for security. let's run bellow command:

    php artisan passport:install
        
**3) Configure project for passport.**

In user model, use HasApiTokens trait class of passport.

    namespace App;
    
    use Illuminate\Contracts\Auth\MustVerifyEmail;
    use Illuminate\Foundation\Auth\User as Authenticatable;
    use Illuminate\Notifications\Notifiable;
    use Laravel\Passport\HasApiTokens;
    
    class User extends Authenticatable
    {
        use Notifiable,HasApiTokens;
    
        /**
         * The attributes that are mass assignable.
         *
         * @var array
         */
        protected $fillable = [
            'name', 'email', 'password',
        ];
    
        /**
         * The attributes that should be hidden for arrays.
         *
         * @var array
         */
        protected $hidden = [
            'password', 'remember_token',
        ];
    
        /**
         * The attributes that should be cast to native types.
         *
         * @var array
         */
        protected $casts = [
            'email_verified_at' => 'datetime',
        ];
    }
    
Add "Passport::routes()" In AuthServiceProvider.

    namespace App\Providers;    
    
    use Laravel\Passport\Passport;
    use Illuminate\Support\Facades\Gate;
    use Illuminate\Foundation\Support\Providers\AuthServiceProvider as ServiceProvider;
    
    
    class AuthServiceProvider extends ServiceProvider
    {
        /**
         * The policy mappings for the application.
         *
         * @var array
         */
        protected $policies = [
            'App\Model' => 'App\Policies\ModelPolicy',
        ];
    
    
        /**
         * Register any authentication / authorization services.
         *
         * @return void
         */
        public function boot()
        {
            $this->registerPolicies();
    
    
            Passport::routes();
        }
    }

Change config/auth.php file.

    return [
        .....
        'guards' => [
            'web' => [
                'driver' => 'session',
                'provider' => 'users',
            ],
            'api' => [
                'driver' => 'passport',
                'provider' => 'users',
            ],
        ],
        .....
    ]

**4) Download packege**

download packege. and module.sh put on top of the project

also run sh module.sh command (if- Modules folder not present on root directory)

**5) Put Folder on Modules/Auth**

in open Modules Folder and create Auth folder and put all file here

**5) Autoloading**
By default the module classes are not loaded automatically. You can autoload your modules using psr-4. For example :

{
  "autoload": {
    "psr-4": {
      "App\\": "app/",
      "Modules\\": "Modules/"
    }
  }
}

Tip: don't forget to run composer dump-autoload afterwards

don't forget to run php artisan module:enable Auth
