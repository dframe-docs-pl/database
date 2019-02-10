.. title:: Konfiguracja - PDO wrapper

.. meta::
    :description: Dane dla dfrme powinny znajdować się w pliku app/boostrap.php. Zawiera on biblioteki i zmienne przekazywane do całej aplikacji poprzez zmienną $this->baseClass.
    :keywords: dframe, database, pdo, pdo-mysql, query-builder, query


Instalacja
----------

Z poziomu konsoli bash wykonaj polecenie composera*

.. code-block:: bash

 $ composer require dframe/database

Albo pobierz ręcznie https://github.com/dframe/database/releases

Konfiguracja
----------

Dane dla Dframe powinny znajdować się w pliku app/boostrap.php. Zawiera on biblioteki i zmienne przekazywane do całej aplikacji poprzez zmienną $this->baseClass. W pliku web/config.php podaje się wartości CONST

.. code-block:: php

 use Dframe\Database\Database;
 use \PDO;
 
 try {
    $dbConfig = [
        'host' => DB_HOST,
        'dbname' => DB_DATABASE,
        'username' => DB_USER,
        'password' => DB_PASS
    ];
    
    // Debug Config 
    $config = [
        'logDir' => APP_DIR . 'View/logs/',
        'attributes' => [
            PDO::MYSQL_ATTR_INIT_COMMAND => "SET NAMES utf8", 
            //PDO::ATTR_ERRMODE => PDO::ERRMODE_SILENT,  // Set pdo error mode silent
            PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION, // If you want to Show Class exceptions on Screen, Uncomment below code 
            PDO::ATTR_EMULATE_PREPARES => true, // Use this setting to force PDO to either always emulate prepared statements (if TRUE), or to try to use native prepared statements (if FALSE). 
            PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC // Set default pdo fetch mode as fetch assoc
         ]
    ];
    
    /**
     * Method #1 
     */  
    $dsn = [
        'host' => DB_HOST,
        'dbname' => DB_DATABASE,
        'dbtype' => 'mysql'
    ];
    $db = new Database($dsn, DB_USER, DB_PASS, $config);
    
    /**
     * Method #2
     */  
    //$db = new Database('mysql:host='.DB_HOST.';dbname=' . DB_DATABASE . ';port=3306', DB_USER, DB_PASS, $config);
    
    $db->setErrorLog(false); // Debug
  
 }catch(\Exception $e) {
     echo 'The connect can not create: ' . $e->getMessage(); 
     exit();
 }
