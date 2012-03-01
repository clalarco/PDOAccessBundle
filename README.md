PDOAccessBundle
===============

Doctrine extension which allows to read Microsoft Access (.mdb) 
files via Doctrine DBAL in Symfony2.

It uses the PDO_ODBC PHP driver, so I think it could work with 
any connection, not only MS Access databases.
It has been tested only on microsoft Windows XP.

It is not intented for ORM mapping, I've only used to import data 
from a older Microsoft Access program to Symfony2.

This extension is based on [PDODblibBundle](https://github.com/trooney/PDODblibBundle)

Installation
------------

Configure php.ini in your apache server. Add:

```
extension=php_pdo_odbc.dll
```

Then, restart apache2 server

Add the following lines into deps file:

```
[ClalarcoPDOAccessBundle]
    git=https://github.com/clalarco/ClalarcoPDOAccessBundle.git
    target=/bundles/Clalarco/PDOAccessBundle
    version=origin/master
```

Register it in the `autoload.php` file:

``` php
<?php
// app/autoload.php

$loader->registerNamespaces(array(
    'Clalarco'          => __DIR__.'/../vendor/bundles/Clalarco',
));
```

Now, run the vendors script to download the bundle:

``` bash
$ php bin/vendors install
```

Create a connection
-------------------

** Note: ** You need to separate the default connection in doctrine. 
Check [Symfony2 Doctrine configuration](http://symfony.com/doc/2.0/reference/configuration/doctrine.html).

The way I created a connection is via config.yml 
(or config_dev.yml). In windows it works using:

```
# config.yml

doctrine:
    dbal:
        connections:
            msaccess:
                driver_class: Clalarco\PDOAccessBundle\Doctrine\DBAL\Driver\PDOMdbLib\Driver
                host: "{Microsoft Access Driver (*.mdb)};Dbq=C:\path_to\file.mdb"
                user: Admin
                password: your_password
```

Replace C:\path_to\file.mdb with your path, and your_password 
with your password, if any, blank otherwise. 'msaccess' name can also been renamed.

In your controller, add the following lines to get the 
connection if your connection name is 'msaccess':

```
<?php

class MyController extends Controller
{
    public function myAction()
    {
        ...
        $conn = $this->get('doctrine.dbal.msaccess_connection');
        ...
        // Once the connection is created, SQL queries are accepted.
        // Check Doctrine DBAL documentation for more information.
        $rows = $conn->fetchAll('SELECT * FROM my_table');
        ...
    }
}
```

If a the connection is used in a command, the command to get the connection is:

```
        $conn = $this->getContainer()->get('doctrine')->getConnection('msaccess');
```

TODO
----

- Improve code documentation.