====
Opis
====

Biblioteka jest Wrapper czyli klasą która wywołuje klasę główną. Służy ona do prostszej obsługi i tworzenia zapytań do bazy danych w dużej części autopatyzując część rzeczy. Poniżej znajduje się lista dostępnych metod oraz ich opis.

|table|

pdoQuery()
^^^^^^^^^^

Podstawowa metoda do wykonywania większości zapytań

.. code-block:: php

 $result = $db->pdoQuery('SELECT * FROM users WHERE id = ?', array('1'))->result();

Notka: Warto zaznaczyć ze na końcu jest metoda result() zwraca ona pierwszy wiersz tablicy, w celu użyskania wszystich wierszy zamiast result() powinno się zastosować results()

.. code-block:: php

 $results = $db->pdoQuery('SELECT * FROM users')->results();

select()
^^^^^^^^

Metoda służąca tylko do pobierania

.. code-block:: php

 $select = $pdo->select('users')->results();
 
Można na niej wykonywać prostsze zapytania z określanymi danymi i filtrami

.. code-block:: php

 // Pobiera wszystkie wiersze
 $select = $pdo->select('users', '*')->results();
 
.. code-block:: php

 // Pobiera wiersze z podanych kolumn
 $select = $pdo->select('users', array('user_id', 'user_name'))->results(); 

insert()
^^^^^^^^

Metoda służąca tylko do dodawania danych do bazy

.. code-block:: php

 $dataArray = array('user_name' => 'Jack');
 $insert = $db->insert('users', $dataArray)->getLastInsertId();

InsertBatch()
^^^^^^^^^^^^^

Działająca na podobnej zasadzie jak insert() lecz daje nam możliwość dodanie hurtem

.. code-block:: php

 $dataArray[] = array('user_name' => 'Eli');
 $dataArray[] = array('user_name' => 'Jack');
 $dataArray[] = array('user_name' => 'Mati');
 $insert = $db->insertBatch('users', $dataArray)->getAllLastInsertId();

update()
^^^^^^^^

Najwygdniejsza metoda w całym wraperze do aktualizowania danych

.. code-block:: php

 $dataArray = array('user_name'=>'Monana', 'user_age'=> '35');
 $where = array('id' => 23);
 $update = $db->update('user', $dataArray, $aWhere)->affectedRows();

delete()
^^^^^^^^

delete służy do kasowania prostych danych.

.. code-block:: php

 $aWhere = array('age' => 35);
 $delete = $db->delete('test', $aWhere)->affectedRows();
W przypadku kasowania bardziej skomplikowanego związanymi z większe/mniejsze/podobne używamy **pdoQuery** z zaleceniem użycia już **whereChunkString**

truncate()
^^^^^^^^^^

Czyści tablicę

.. code-block:: php

 $truncate = $db->truncate('users');

drop()
^^^^^^

Usuwa tablicę

.. code-block:: php

 $drop = $db->drop('users');

describe()
^^^^^^^^^^

Pokazuje liste kolumn w bazie oraz ich typy

.. code-block:: php

 $describe = $db->describe('users');

count()
^^^^^^^

Liczenie liczby wierszy w mniej skomplikowanych zapytaniach

.. code-block:: php

 $count = $db->count('employees');
 $bindWhere = array('user_name' = 'Jack');
 $count = $db->count('users', $bindWhere);

showQuery()
^^^^^^^^^^^

showQuery jest bardzo przydatną metodą przy dużych zapytaniach dzięki niej zamiast parametru result()/results() wstawiamy showQuery() dzięki czemu wyświetla nam zapytanie z podstawionymi zmiennymi

.. code-block:: php

 results = $db->pdoQuery('SELECT * FROM users')->showQuery();
 echo $results;

getLastInsertId()
^^^^^^^^^^^^^^^^^

Zwraca ostatnio wstawiony id wiersza

.. code-block:: php

 $getLastInsertId = $db->insert('users', $dataArray)->getLastInsertId();
 echo $getLastInsertId;

getAllLastInsertId()
^^^^^^^^^^^^^^^^^^^^

Zwraca tablicę wszystkich ostatnich wstawionych id dla metody insertBatch

results()
^^^^^^^^^

Zwraca dane w domyślnym formacje **array** dostępne jeszcze **xml/json**

.. code-block:: php

 $data = $db->results();
 $data = $db->results('xml');
 $data = $db->results('json');

result()
^^^^^^^^

Działa na tej samej zasadzie do results i jak juz było wspomnanie zwraca nam tylko pierszy wiersz

.. code-block:: php

 $data = $db->result();
 $data = $db->result('xml');
 $data = $db->result('json');

affectedRows()
^^^^^^^^^^^^^^

Zwraca nam liczbę zmodyfikowanych wierszy

.. code-block:: php

 $data = $db->affectedRows();

start()
^^^^^^^

Rozpoczęcie transakcji mysql

.. code-block:: php

 $data = $db->start();

end()
^^^^^

Zakoczenie transackji mysql

.. code-block:: php

 $data = $db->end();

back()
^^^^^^

Cofnięcie zmian jeśli w trakcie start/end wyskoczył jakiś bład

.. code-block:: php

 $data = $db->back();

setErrorLog()
^^^^^^^^^^^^^

Domyślnie ustawiany przy konfiguracji na false włacza/wyłacza debugowanie

.. code-block:: php

 $db->setErrorLog(true);     // true/false



.. |table| advTable:: width="100%"
 :tr_1:
 :th_1.1: MySQL query/-title.1.1
 :th_1.11:
 :th_1.2: pdoQuery()/-title.1.1
 :th_1.22:
 :tr_2:
 :tr_3:
 :td_1.1: MySQL select query/-title.1.2
 :td_1.11:
 :td_1.2: select()/-title.1.2
 :td_1.22:
 :tr_4:
 :tr_5:
 :td_2.1: MySQL insert query/-title.1.3
 :td_2.11:
 :td_2.2: insert()/-title.1.3
 :td_2.22:
 :tr_6:
 :tr_8:
 :td_3.1: MySQL insert batch/-title.1.4
 :td_3.11:
 :td_3.2: insertBatch()/-title.1.4
 :td_3.22:
 :tr_9:
 :tr_10:
 :td_4.1: MySQL update query/-title.1.5
 :td_4.11:
 :td_4.2: update()/-title.1.5
 :td_4.22:
 :tr_11:
 :tr_12:
 :td_5.1: MySQL delete query/-title.1.6
 :td_5.11:
 :td_5.2: delete()/-title.1.6
 :td_5.22:
 :tr_13:
 :tr_14:
 :td_6.1: MySQL truncate table/-title.1.7
 :td_6.11:
 :td_6.2: truncate()/-title.1.7
 :td_6.22:
 :tr_15:
 :tr_16:
 :td_7.1: MySQL drop table/-title.1.8
 :td_7.11:
 :td_7.2: drop()/-title.1.8
 :td_7.22:
 :tr_17:
 :tr_28:
 :td_8.1: MySQL describe table/-title.1.9
 :td_8.11:
 :td_8.2: describe()/-title.1.9
 :td_8.22:
 :tr_29:
 :tr_30:
 :td_9.1: MySQL count records/-title.1.10
 :td_9.11:
 :td_9.2: count()/-title.1.10
 :td_9.22:
 :tr_31:
 :tr_32:
 :td_10.1: Show/debug executed query/-title.1.11
 :td_10.11:
 :td_10.2: showQuery()/-title.1.11
 :td_10.22:
 :tr_33:
 :tr_34:
 :td_11.1: Get last insert id/-title.1.12
 :td_11.11:
 :td_11.2: getLastInsertId()/-title.1.12
 :td_11.22:
 :tr_35:
 :tr_36:
 :td_12.1: Get all last insert id/-title.1.13
 :td_12.11:
 :td_12.2: getAllLastInsertId()/-title.1.13
 :td_12.22:
 :tr_37:
 :tr_39:
 :td_13.1: Get MySQL results/-title.1.14
 :td_13.11:
 :td_13.2: results()/-title.1.14
 :td_13.22:
 :tr_40:
 :tr_41:
 :td_14.1: Get MySQL result/-title.1.15
 :td_14.11:
 :td_14.2: result()/-title.1.15
 :td_14.22:
 :tr_42:
 :tr_43:
 :td_15.1: Get status of executed query/-title.1.16
 :td_15.11:
 :td_15.2: affectedRows()/-title.1.16
 :td_15.22:
 :tr_44:
 :tr_45:
 :td_16.1: MySQL begin transactions/-title.1.17
 :td_16.11:
 :td_16.2: start()/-title.1.17
 :td_16.22:
 :tr_46:
 :tr_47:
 :td_17.1: MySQL commit the transaction/-title.1.18
 :td_17.11:
 :td_17.2: end()/-title.1.18
 :td_17.22:
 :tr_48:
 :tr_49:
 :td_18.1: MySQL rollback the transaction/-title.1.19
 :td_18.11:
 :td_18.2: back()/-title.1.19
 :td_18.22:
 :tr_50:
 :tr_51:
 :td_19.1: Debugger PDO Error/-title.1.20
 :td_19.11:
 :td_19.2: setErrorLog()/-title.1.20
 :td_19.22:
 :tr_52:
