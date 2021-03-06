# Nuevo Proyecto en Zend Framework 3
Pasos para crear un proyecto desde Zend Framework 3

1. Crear estructura inicial a traves de composer: (reemplazar "path/to/install" por la ruta y nombre de la carpeta donde se va instalar)
```bash
$ composer create-project -s dev zendframework/skeleton-application path/to/install
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

# Librerias disponibles:

1. [MIALayout](https://github.com/MobileIA/mia-layout-zf3)
2. [MIALayoutLTE](https://github.com/MobileIA/mia-layout-lte-zf3)
3. [MIALayoutElite](https://github.com/MobileIA/mia-layout-elite-zf3)
4. [MIAInstaller](https://github.com/MobileIA/mia-installer-zf3)
5. [MIAEmail](https://github.com/MobileIA/mia-mail-zf3)
6. [MIAFile](https://github.com/MobileIA/mia-file-zf3)

# Creación de modulo interno

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
5. Crear archivo Module.php dentro de la carpeta del modulo:
```php
namespace NombreModulo;

class Module implements \Zend\ModuleManager\Feature\ConfigProviderInterface
{
    public function getConfig()
    {
        return include __DIR__ . '/config/module.config.php';
    }

    public function getAutoloaderConfig()
    {
        return array(
            'Zend\Loader\StandardAutoloader' => array(
                'namespaces' => array(
                    __NAMESPACE__ => __DIR__ . '/src/' . __NAMESPACE__,
                ),
            ),
        );
    }
}
```

# Creación de modulo publico para implementar con Composer

1. Crear repositorio en github
2. Clonar repositorio vacío
3. Crear proyecto Netbeans
4. Crear archivo composer.json
5. Comitiar cambios
6. Crear release con el número de versión
7. Si es un modulo para instalar en ZF3: agregar siguientes lineas:
```json
"extra": {
    "zf": {
        "component": "MIAEmail"
    }
}
```

Para usar este modulo en un proyecto creado:

1. Incluir repositorio en composer.json del proyecto
```json
"repositories": [
    {
        "type": "git",
        "url": "https://github.com/MobileIA/mia-base-zf3.git"
    }
],
```
2. Agregado require: "mobileia/mia-authentication-zf3": "^0.0.1"
3. composer update
