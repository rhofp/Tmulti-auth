# TMulti-Auth

```shell
Diciembre, 2019
email : rhodfra@gmail.com
Laravel Framework 6.7.0 | PHP 7.3.11 (cli) (built: Oct 23 2019 16:31:28) ( NTS ) | Laravel Installer 2.1.0
```

<p align="center"><img src="img/cover.jpg" width="400"></p>
<p align="center">
<a href="https://travis-ci.org/laravel/framework"><img src="https://travis-ci.org/laravel/framework.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://poser.pugx.org/laravel/framework/v/stable.svg" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://poser.pugx.org/laravel/framework/license.svg" alt="License"></a>
</p>

## Multi-Authentication Template

TMulti-Auth es una plantilla de como implementar un sistema multi-usuario:

* Usuario administrador
* Usuario común

Existen dos formas de implementar un sistema multi-usuario. Ambas utilizan el sistema de autentificación semi-integrado con laravel.

* Agregar un atributo al modelo de usuario que podría llamarse `rol` o `tipo` y mediante lógica de programación ir preguntando en cada vista o controlador si el usuario tiene rol  de administrador o su rol es de usuario común.
* Agregar `guards` y `providers`, crear un modelo llamado `Admin` y posteriormente modificar `Middlewares`, `Handlers`, crear nuevos controladores y rutas para los administradores.

La segunda opción es más complicada que la primera, pero ofrecer algunas ventajas:

* Al estar completamente separado el Usuario común y el usuario Admin, se pueden crear jerarquías en el usuario común y los subtipos no tendrán nada que ver con el administrador. A nivel de base de datos esto permite un mayor desempeño.
* La seguridad es mayor ya que los datos del administrador se aislan completamente de los del usuario.
* La lógica de programación se simplifica ligeramente.

**TMulti-Auth es una implementación basada en guards y providers**

## Installation

Asegurarse de tener todo lo necesario para trabajar con laravel

* php (para el caso de TMulti-auth, php >= 7.2)
* composer
* npm

Para usar la plantilla se deberán seguir los siguientes pasos:

1. Descargar el repositorio.

   Se puede usar git

   ```shell
   git clone https://github.com/rhofp/Tmulti-auth.git
   ```

   O directamente se puede descargar el archivo comprimido en formato zip.

2. Descargar las dependencias de php del proyecto.

   ```shell
   composer install
   ```

3. Descargar las dependencias de javascript del proyecto.

   ```shell
   npm install && npm run dev
   ```

4. Crear una llave del proyecto.

   ```shell
   php artisan generate:key
   ```

5. Configurar una la conexión con una base de datos.

   Se puede configurar cualquier base de datos soportada por Laravel, como ejemplo sencillo se presenta la conexión con `sqlite`

   Generar un archivo en blanco llamado `database.sqlite` en la carpeta database

   ```shell
   touch database/database.sqlite
   ```

   Configurar el archivo `.env`

   Este es mi archivo .env el cual nunca se sube al repositorio porque es un archivo de configuración crítico.

   ```shell
   APP_NAME=Laravel
   APP_ENV=local
   APP_KEY=base64:5Tc+VhWjcEW7ayLq+AL147ccKLKk1uj6CzbiykYfvrM=
   APP_DEBUG=true
   APP_URL=http://localhost
   
   LOG_CHANNEL=stack
   
   DB_CONNECTION=sqlite
   
   BROADCAST_DRIVER=log
   CACHE_DRIVER=file
   QUEUE_CONNECTION=sync
   SESSION_DRIVER=cookie
   SESSION_LIFETIME=120
   
   REDIS_HOST=127.0.0.1
   REDIS_PASSWORD=null
   REDIS_PORT=6379
   
   MAIL_DRIVER=smtp
   MAIL_HOST=smtp.mailtrap.io
   MAIL_PORT=2525
   MAIL_USERNAME=null
   MAIL_PASSWORD=null
   MAIL_ENCRYPTION=null
   
   AWS_ACCESS_KEY_ID=
   AWS_SECRET_ACCESS_KEY=
   AWS_DEFAULT_REGION=us-east-1
   AWS_BUCKET=
   
   PUSHER_APP_ID=
   PUSHER_APP_KEY=
   PUSHER_APP_SECRET=
   PUSHER_APP_CLUSTER=mt1
   
   MIX_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
   MIX_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"
   ```

6. Generar las migraciones del proyecto.

   ```shell
   php artisan migrate
   ```

## Usage

El uso de este proyecto se puede inferir con tal solo revisar el archivo `routes/web.php`. Sin embargo se deja una guía básica de que hace y que no hace.

* Se usa el sistema de autentificación que laravel tiene semi-integrado. Dicho sistema permanece intacto.

* Se adiciona un sistema de logeo para usuarios administradores `127.0.0.1:8000/admin/login`

  Al colocar correctamente las credenciales del usuario administrador se le redirige a `127.0.0.1:8000/admin`.

* Un usuario administrador y un usuario común pueden estar logeados al mismo tiempo.

* Las rutas del proyecto se muestran a continuación.

  ```php
  Route::prefix('admin')->group(function() {
      Route::get('/login','Auth\AdminLoginController@showLoginForm')-		>name('admin.login');
      Route::post('/login', 'Auth\AdminLoginController@login')->name('admin.login.submit');
      Route::get('logout/', 'Auth\AdminLoginController@logout')->name('admin.logout');
      Route::get('/', 'AdminController@index')->name('admin.dashboard');
     }) ;
  
  Route::get('/', function () {
      return view('welcome');
  });
  
  Auth::routes();
  
  Route::get('/home', 'HomeController@index')->name('home');
  ```

## Contributing

Todas las contribuciones al proyecto son bienvenidas, se recomienda revisar el archivo correspondiente [CONTRIBUTING](CONTRIBUTING.md).

## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).

## Credits

Este template no hubiera sido posible sin lo siguiente tutoriales

* https://www.codermen.com/blog/123/how-to-make-multi-auth-in-laravel-6
* https://github.com/DevMarketer/multiauth_tutorial
* https://www.youtube.com/watch?v=Ir2nAD9UDGg

Mi contribución se basa en implementar y actualizar dichos tutoriales para que puedan servir directamente a la comunidad sin tener que pasar por los *n pasos* de los tutoriales mencionados.