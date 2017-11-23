Dane dla dfrme powinny znajdować się w pliku app/boostrap.php. Zawiera on biblioteki i zmienne przekazywane do całej aplikacji poprzez zmienną $this->baseClass. W pliku web/config.php podaje się wartości CONST

.. code-block:: php

 use Dframe\Database\Database;
 
 try {
     $dbConfig = array(
         'host' => DB_HOST,
         'dbname' => DB_DATABASE,
         'username' => DB_USER,
         'password' => DB_PASS
  );
  $db = new Database($dbConfig);
  $db->setErrorLog(false); // Debug
  
 }catch(DBException $e) {
     echo 'The connect can not create: ' . $e->getMessage(); 
     exit();
 }
