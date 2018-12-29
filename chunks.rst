WhereChunk
^^^^^^^^^^

Tak zwane kawałki, Pomocne do przeszukiwania/filtrowania danych w bazie. Gdy tworzymy zapytanie do bazy dorzucamy różne ograniczenia WHERE za pomocą chunków możemy prościej i mniejszą ilością kodu dodawać bądź usuwać parametry doklejane do WHERE

**app/Controller/UsersController.php**

.. code-block:: php

 namespace Controller;

 use Dframe\Config;
 use Dframe\Database\WhereChunk;
 use Dframe\Database\WhereStringChunk;
 use Dframe\Router\Response;

 class UsersController extends \Controller\Controller
 {    
     /**
      * @return mixed
      */
     public function index()
     {
         $UserModel = $this->loadModel('Users');
         $View = $this->loadView('Index');
         return $View->render('users/index');
     }
         
     /**
      * @return mixed
      */
     public function lists()
     {
         $UserModel = $this->loadModel('Users');
         
         switch ($_SERVER['REQUEST_METHOD']) {
             case 'POST':
                 //Some Method
                 break;

             case 'GET':
                 
                 $limit = $_GET['limit'] ?? '30';
                 $start = $_GET['start'] ?? '0';
                
                 $where = [];
                 $params = [
                     'order' => 'users.user_id', 
                     'sort' => 'ASC'
                 ];

                 if (isset($_GET['search']['username'])) {
                     $where[] = new WhereChunk('`users`.`username`', '%' . $_GET['search']['username'] . '%', 'LIKE');
                 }

                 $users = $UserModel->getUsers($where, $params, $limit, $start);
                     
                 $data = [];
                 foreach ($users['data'] as $key => $user) {
                     $data['id'] = $user['user_id'];
                     $data['first_name'] = $user['user_first_name'];
                     $data['last_name'] = $user['user_last_name'];
                 }
                      
                 return Response::renderJSON(['code' => '200', 'data' => ['users' => ['data' => $data]]], 200);
                 break;
         }

         return Response::renderJSON(['code' => 403, 'message' => 'Method Not Allowed'])
             ->headers(['Allow' => 'GET, POST'])
             ->status(403);
     }
 }

 
**app/Model/UserModel.php**

.. code-block:: php

 namespace Model;
    
 class UsersModel extends \Model\Model
 {
     /**
      * @param array  $whereObject
      * @param string $order
      * @param string $sort
      *
      * @return array
      */
     public function getUsers($whereObject, $params = ['order' => 'users.id', 'sort' => 'DESC'], $limit = 30, $start = 0)
     {
    
         $query = $this->db->prepareQuery('SELECT * FROM users');
         $query->prepareWhere($whereObject);
         $query->prepareOrder($params['order'], $params['sort']);
         $query->prepareLimit($limit);
         $query->prepareStart($start);
    
         $results = $this->db->pdoQuery($query->getQuery(), $query->getParams())->results();
  
         return $this->methodResult(true, ['data' => $results]);
     }

W przypadku wywołania $_POST do podstawowego zapytania zostanie doklejony warunek. Wszystkie parametry automatycznie są bindowane do PDO więc nie musimy już oto matwić.

WhereStringChunk
^^^^^^^^^^^^^^^^

Ciekawszą i częściej w praktyce wykorzystywaną klasą jest WhereStringChunk daje ona nam dużo większne możliwości niż zwykłe WhereChunk

.. code-block:: php
 
 $where = [];
 $where[] = new \Dframe\Database\WhereStringChunk('col_id > ?', ['1']);

albo

.. code-block:: php

 $where[] = new \Dframe\Database\WhereStringChunk('col_name LIKE ?', ['%name%']);
 

HavingStringChunk
^^^^^^^^^^^^^^^^

.. code-block:: php

 $having = [];
 $having[] = new \Dframe\Database\HavingStringChunk('col_name = ?', ['example']);
 
 $query = $this->db->prepareQuery('SELECT * FROM users');
 $query->prepareGroupBy('name');
 $query->prepareHaving($having);
 
