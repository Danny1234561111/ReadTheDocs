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
