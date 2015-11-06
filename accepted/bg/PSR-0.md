Стандарт за автоматично зареждане
=================================

> **Deprecated** - От 2014-10-21 PSR-0 е маркиран като непрепоръчителен. Сега [PSR-4] е препоръчителен
като алтернатива.

[PSR-4]: http://www.php-fig.org/psr/psr-4/

Следващите редове описват задължителните изисквания, които трябва да се спазят
за да се постигне интероперабилност при автоматичното зареждане.

Задължителни
------------

* Пълно изписване на именното пространство, със следната
  структура `\<Име на доставчик>\(<Именно пространство>\)*<Име на класа>`
* Всяко именно пространство трябва да има име на най-горно ниво ("Име на доставчик").
* Всяко именно пространство може да има множество под-именни пространства.
* Всеки разделител между именните пространства се преобразува на `DIRECTORY_SEPARATOR`
  когато се зарежда от файловата система.
* Всеки знак `_` в ИМЕТО НА КЛАСА се преобразува в `DIRECTORY_SEPARATOR`.
  Знакът `_` няма специално значение в именното пространство.
* Към пълното изписване на именното пространство и клас се добавя `.php`, когато
  се зарежда от файлова система.
* Буквените знаци в имената на доставчиците, именните пространства и класовете,
  може да бъдат всякаква комбинация от горен и долен регистър.

Примери
--------

* `\Doctrine\Common\IsolatedClassLoader` => `/path/to/project/lib/vendor/Doctrine/Common/IsolatedClassLoader.php`
* `\Symfony\Core\Request` => `/path/to/project/lib/vendor/Symfony/Core/Request.php`
* `\Zend\Acl` => `/path/to/project/lib/vendor/Zend/Acl.php`
* `\Zend\Mail\Message` => `/path/to/project/lib/vendor/Zend/Mail/Message.php`

Подчертаващи тирета в именните пространства и класове
-----------------------------------------------------

* `\namespace\package\Class_Name` => `/path/to/project/lib/vendor/namespace/package/Class/Name.php`
* `\namespace\package_name\Class_Name` => `/path/to/project/lib/vendor/namespace/package_name/Class/Name.php`

Стандартите, които определихме тук, трябва да са най-малкият общо знаменател за
безболезнена интероперабилност при автоматичното зареждане. Можете да тествате дали следвате
тези стандарти като използвате реализацията SplClassLoader, можеща да зарежда
PHP 5.3 класове.

Примерна реализация
-------------------

По-долу следва примерна функция, илюстрираща предложените по-горе стандарти
за автоматично зареждане.

```php
<?php

function autoload($className)
{
    $className = ltrim($className, '\\');
    $fileName  = '';
    $namespace = '';
    if ($lastNsPos = strrpos($className, '\\')) {
        $namespace = substr($className, 0, $lastNsPos);
        $className = substr($className, $lastNsPos + 1);
        $fileName  = str_replace('\\', DIRECTORY_SEPARATOR, $namespace) . DIRECTORY_SEPARATOR;
    }
    $fileName .= str_replace('_', DIRECTORY_SEPARATOR, $className) . '.php';

    require $fileName;
}
spl_autoload_register('autoload');
```

Реализация SplClassLoader
-------------------------

Следният gist адрес показва образеца SplClassLoader, който може да зарежда
класове, ако следвате предложените стандарти за интероперабилност. Това
за момента е препоръчителният начин за зареждане на PHP 5.3 класове, следващ
горните предложени стандарти.

* [http://gist.github.com/221634](http://gist.github.com/221634)

