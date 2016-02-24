# Reincarnation

[![Build Status](https://travis-ci.org/fusic/Reincarnation.svg?branch=master)](https://travis-ci.org/fusic/Reincarnation)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/fusic/Reincarnation/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/fusic/Reincarnation/?branch=master)

CakePHP3 logical delete Plugin

## Requirements

- PHP >= 5.4.*
- CakePHP >= 3.*

## Installation

- $ composer install

# Usage

 Create users table.

```sql

CREATE TABLE users
(
  id serial NOT NULL,
  username text,
  password text,
  created timestamp without time zone,
  modified timestamp without time zone,
  delete_flg boolean DEFAULT false,
  deleted timestamp with time zone,
  CONSTRAINT users_pkey PRIMARY KEY (id)
)
WITH (
  OIDS=FALSE
);
```

 UsersTable.php

```php
class UsersTable extends Table
{
    public function initialize(array $config)
    {
        // Case 1
        // default
        $this->addBehavior('Reincarnation.SoftDelete');
        ```
        - table field name
          - boolean:deleted
          - timestamp:delete_date
        ```

        // Case 2
        // field name custom
        $this->addBehavior('Reincarnation.SoftDelete', ['boolean' => delete_flg, 'timestamp' => 'deleted']);
        ```
        - table field name
          - boolean:delete_flg
          - timestamp:deleted
        ```

        // Case 3
        // boolean only
        $this->addBehavior('Reincarnation.SoftDelete', ['boolean' => delete_flg, 'timestamp' => false]);
        ```
        - table field name
          - boolean:delete_flg
          - timestamp:none
        ```

        // Case 4
        // timestamp only
        $this->addBehavior('Reincarnation.SoftDelete', ['boolean' => false, 'timestamp' => 'deleted']);
        ```
        - table field name
          - boolean:none
          - timestamp:deleted
        ```
    }
}
```

 UsersController.php

```php
class UsersController extends AppController
{
    public function delete($id = null)
    {
        $this->request->allowMethod(['post', 'delete']);
        $user = $this->Users->get($id);
        if ($this->Users->softDelete($user)) {
            $this->Flash->success(__('The data has been deleted.'));
        } else {
            $this->Flash->error(__('The data could not be deleted. Please, try again.'));
        }
        return $this->redirect('action' => 'index');
    }
}
```


