Клиент-сервер
-------------

Примерная схема запросов-ответов

Ошейник-сервер
^^^^^^^^^^^^^^

1) Ошейник посылает раз в 5 часов запрос с данными о местоположении. Местоположение и время последнего сигнала фиксируется в базе данных.

Обновить данные
~~~~~~~~~~~~~~~

``/dogs/update``


* Запрос
  .. code-block::

     {
       "accessDogToken": "JusOh2nRK1kZpxzK",
       "dog_id": 44,
       "coordinates": "52.250323, 104.264442"
     }

* Ответ
  .. code-block::

     {
       "success": "true",
     }

Пользователь-сервер
^^^^^^^^^^^^^^^^^^^

1) При регистрации нового пользователя посылается запрос на сервер. Проверяются данные и записываются в базу данных.

Регистрация
~~~~~~~~~~~

``/user/register``


* Запрос
  .. code-block::

     {
       "nickname":"Andrey",
       "password":"strongpassword1337",
       "special_code": "Danny"
     }

* Ответ
  .. code-block::

     {
       "success": "true",
       "accessToken": "JusOh2nRK1kZpxzK"
     }

2) При авторизации пользователя посылается запрос на сервер. Проверяются данные и отправляется разрешение на вход.

Авторизация
~~~~~~~~~~~

``/user/login``


* Запрос
  .. code-block::

     {
       "nickname":"Andrey",
       "password":"strongpassword1337"
     }

* Ответ
  .. code-block::

     {
       "success": "true",
       "accessToken": "JusOh2nRK1kZpxzK"
     }
  3) При прогрузке страницы с картой собак серверу посылается запрос для получения координат ошейников.
  #### Получение координат собак
  ``/dogs/coordinates``
* Запрос
  .. code-block::

     {
       "accessToken": "JusOh2nRK1kZpxzK",
       "place": "Irkutsk"
     }

* Ответ
  .. code-block::

     {
       "success": "true",
       "dogs": [
       {
           "dog_id": 3,
           "coordinates": "52.250323, 104.264442"
       },
       {
           "dog_id": 12,
           "coordinates": "52.250884, 104.263155"
       }]
     }
  4) При выборе определенной собаки серверу посылается запрос для получения характеристик собаки.
  #### Получение характеристики собаки
  ``/dogs/characteristic``
* Запрос
  .. code-block::

     {
       "accessToken": "JusOh2nRK1kZpxzK",
       "dog_id": 3
     }

* Ответ
  .. code-block::

     {
       "success": "true",
       "characteristic": "Рыжий корги, рост 25 см, вес 10кг, дружелюбный и обаятельный"
     }
  5) При выборе текущих заданий у собаки.
  #### Получение заданий собаки
  ``/dogs/task/list``
* Запрос
  .. code-block::

     {
       "accessToken": "JusOh2nRK1kZpxzK",
       "dog_id": 3
     }

* Ответ
  .. code-block::

     {
       "success": "true",
       "tasks": [
           {
               "task_id": 34,
               "asked_user": "Glebus",
               "goal": "Принести собаку в шаурмечную"
           },
           {
               "task_id": 3,
               "asked_user": "Danny",
               "goal": "Вытащить собаку из шаурмечной"
           }
       ]
     }
  6) При составлении задания серверу посылается запрос. Задание записывается в базу данных.
  #### Создание задания
  ``/dogs/task/create``
* Запрос
  .. code-block::

     {
       "accessToken": "JusOh2nRK1kZpxzK",
       "dog_id": 3,
       "goal": "Покормить собаку"
     }

* Ответ
  .. code-block::

     {
       "success": "true",
       "task_id": 12
     }
  7) Если пользователь решает взять задание, то отправляется запрос. В базе данных фиксируется исполнитель задания.
  #### Взять задание
  ``/dogs/task/take``
* Запрос
  .. code-block::

     {
       "accessToken": "JusOh2nRK1kZpxzK",
       "task_id": 12
     }

* Ответ
  .. code-block::

     {
       "success": "true"
     }
  8) Если пользователь хочет приложить отклик к взятому заданию, отправляется запрос и в базе данных всё это фиксируется.
  #### Приложить отклик
  ``/dogs/task/response/give``
* Запрос
  .. code-block::

     {
       "accessToken": "JusOh2nRK1kZpxzK",
       "task_id": 12,
       "comments": "Всё сделал как надо",
       "photo": "dog.img",
     }

* Ответ
  .. code-block::

     {
       "success": "true"
     }
  9) Если создатель задания захочет посмотреть отклики.
  #### Просмотреть отклики
  ``/dogs/task/response/list``
* Запрос
  .. code-block::

     {
       "accessToken": "JusOh2nRK1kZpxzK",
       "task_id": 12
     }

* Ответ
  .. code-block::

     {
       "success": "true"
       "responses": [
       {
           "response_user": "Danny",
           "comment": "Всё сделал как надо",
           "photo": "dog.img"
       },
       {
           "response_user": "Danny",
           "comment": "Отвез в шаурменко",
           "photo": "dog2.img"
       }]
     }
  10) Подтверждение, что задание выполнено или отменить задание.
  #### Подтверждение, что задание выполнено
  ``/dogs/task/confirm``
* Запрос
  .. code-block::

     {
       "accessToken": "JusOh2nRK1kZpxzK",
       "task_id": 12,
       "done": "true" 
     }

* Ответ
  .. code-block::

     {
       "success": "true"
     }

Админ-сервер
^^^^^^^^^^^^

1) При регистрации новой собаки на сервер посылается запрос с данными о собаке. Соответственно эти данные фиксируется в базе данных.

Регистрация новой собаки
~~~~~~~~~~~~~~~~~~~~~~~~

``/dogs/register``


* Запрос
  .. code-block::

     {
       "accessToken": "JusOh2nRK1kZpxzK",
       "characteristic": "Рыжий корги, рост 25 см, вес 10кг, дружелюбный и обаятельный",
       "place": "Irkutsk",
     }

* Ответ
  .. code-block::

     {
       "success": "true",
       "dog_id": 44,
       "accessDogToken": "JusOh2nRK1kZpxzK"
     }

2) Посылается запрос, чтобы получить дату последнего сигнала и координаты.

Получить данные
~~~~~~~~~~~~~~~

``/dogs/info``


* Запрос
  .. code-block::

     {
       "accessToken": "JusOh2nRK1kZpxzK",
       "dog_id": 12,
     }

* Ответ
  .. code-block::

     {
       "lastsend": "2024.11.04T11:44:12",
       "coordinates": "52.250323, 104.264442"
     }
  3) Админ может заблокировать пользователя, который нарушил правила, либо его разбанить.
  #### Поменять статус пользователя
  ``/user/changestatus``
* Запрос
  .. code-block::

     {
       "accessToken": "JusOh2nRK1kZpxzK",
       "changed_user_login": 12,
       "delete": true
     }

* Ответ
  .. code-block::

     {
       "success": "true"
     }
  4) Админ может заблокировать/разблокировать собаку.
  #### Поменять статус собаки
  ``/dogs/changestatus``
* Запрос
  .. code-block::

     {
       "accessToken": "JusOh2nRK1kZpxzK",
       "dog_id": 12,
       "delete": true
     }

* Ответ
  .. code-block::

     {
       "success": "true"
     }
  ## Базы данных:
  #### Таблица с пользователями
  .. code-block::

     users (
       id INT PRIMARY KEY,
       login VARCHAR(255),
       password VARCHAR(255)(хэшированный),
       accessToken VARCHAR(255),
       is_admin BOOLEAN,
       is_deleted BOOLEAN
     )

Таблица с собаками
~~~~~~~~~~~~~~~~~~

.. code-block::

   dogs (
       id PRIMARY KEY,
       characteristic VARCHAR(255),
       coords VARCHAR(255),
       last_send DATETIME,
       is_deleted BOOLEAN,
       place VARCHAR(255),
       accessToken VARCHAR(255)
   )

Таблица с заданиями
~~~~~~~~~~~~~~~~~~~

.. code-block::

   tasks (
       id INT PRIMARY KEY,
       upload_user_id INT,
       dog_id INT,
       goal VARCHAR(255),
       done BOOLEAN
   )

Таблица с решениями
~~~~~~~~~~~~~~~~~~~

.. code-block::

   responses (
       id INT PRIMARY KEY,
       do_user_id INT,
       task_id INT,
       comment VARCHAR(255),
       photo VARCHAR(255)
   )
