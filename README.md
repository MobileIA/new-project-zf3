# Nuevo Proyecto en Zend Framework 3
Pasos para crear un proyecto desde Zend Framework 3

1. Crear estructura inicial a traves de composer: (reemplazar "path/to/install" por la ruta y nombre de la carpeta donde se va instalar)
```bash
$ composer create-project -n -sdev zendframework/skeleton-application path/to/install
```
2. Vamos a agregar la libreria Base de MobileIA, editamos el archivo "composer.json", donde agregamos la ubicación de la misma:
```json
"repositories": [
    {
        "type": "git",
        "url": "https://github.com/MobileIA/mia-base-zf3.git"
    }
],
```
3. Agregamos la librería requerida:
```json
"require": {
    // ... others libraries ...
    "mobileia/mia-base-zf3": "^0.0"
},
```
4. Actualizamos composer para que descargue la libreria recientemente agregada:
```bash
$ composer update
```
5. Activar modulo en zf3, abrir archivo: config/modules.config.php
```php
return [
    // Others modules
    'MIABase',
    'Application',
];
```
6. Si el proyecto va a usar una base de datos en MySQL, agregar en el archivo "config/autoload/development.local.php" los siguientes datos, (tener en cuenta que esta configuración solo servira para el entorno local):
```php
return [
    // Others params
    'db' => array(
        'driver' => 'Pdo_Mysql',
        'dsn' => 'mysql:dbname=NAME_DB;host=localhost:PORT_DB',
        'username' => 'USER_DB',
        'password' => 'PASS_DB',
        'port' => 'PORT_DB',
        'driver_options' => array(
            PDO::MYSQL_ATTR_INIT_COMMAND => 'SET NAMES \'UTF8\''
        ),
    ),
];
```
7. Si vamos a usar MobileIA Authentication, seguir los pasos requeridos en: [https://github.com/MobileIA/mia-authentication-zf3](https://github.com/MobileIA/mia-authentication-zf3).

# Creación de modulo

1. Creamos la carpeta dentro de /modules
2. Editar el archivo composer.json
```json
"autoload": {
    "psr-4": {
        ...
        "NombreModulo\\": "module/NombreModulo/src/"
    }
},
```
3. Ejecutar siguiente comando para actualizar:
```bash
$ composer dump-autoload
```
4. Activar modulo, abrir archivo: config/modules.config.php
```php
return [
    // Others modules
    'NombreModulo',
];
```