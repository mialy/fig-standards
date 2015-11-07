Ръководство за стил на кодиране
===============================

Ръководството разширява и допълва [PSR-1], стандартът при кодиране.

Целта е да се намали когнитивното съпротивление когато се преглежда кода от
различни автори. Това става чрез изброяване на общи правила и очаквания относно
как да се форматира код на PHP.

Стиловите правила тук са извлечени от приликите на множество проекти. Ако
различни автори си сътрудничат в множество проекти, това помага да се използва
общ набор от напътствия във всичките проекти. Така ползата от това ръководство
не са самите правила, а това да се споделят.

Ключовите думи „ТРЯБВА“, „НЕ ТРЯБВА“, „ЗАДЪЛЖИТЕЛЕН“, “СЛЕДВА“, „НЕ СЛЕДВА“,
„БИ ТРЯБВАЛО“, “НЕ БИ ТРЯБВАЛО“, „ПРЕПОРЪЧИТЕЛЕН“, „МОЖЕ“ и „НЕЗАДЪЛЖИТЕЛЕН“ в
този документ се интерпретират като описаните в [RFC 2119]

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[PSR-4]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader.md


1. Общ преглед
--------------

- Кодът ТРЯБВА да следва PSR „Стандарт при кодиране“ [[PSR-1]].

- Кодът ТРЯБВА да използва 4 интервала за отстъп, без табулатори.

- НЕ ТРЯБВА да има твърдо ограничение дължината на реда; препоръчителната дължина
  ТРЯБВА да е 120 символа; редовете БИ ТРЯБВАЛО да са 80 или по-малко символа.

- ТРЯБВА да има един празен ред след `namespace` декларацията, и ТРЯБВА да има
  един празен ред след блока с `use` декларации.

- Отварящите скоби за класове ТРЯБВА да е на следващия ред, а затварящите скоби
  ТРЯБВА да са на следващ ред след тялото на класа.

- Отварящите скоби за методи ТРЯБВА да е на следващ ред, затварящите ТРЯБВА
  да са на следващ ред след тялото на функцията.

- Видимостта ТРЯБВА да се декларира на всички свойства и методи; `abstract` и
  `final` ТРЯБВА да се декларират преди видимостта; `static` ТРЯБВА да се декларира
  след видимостта.

- След ключовата дума на контролните конструкции ТРЯБВА да има интервал; извикванията
  на методи и функции НЕ ТРЯБВА да имат интервал.

- Отварящите скоби за контролните конструкции ТРЯБВА да са на същия ред,
  затварящите – ТРЯБВА да са на следващ ред след тялото.

- Отварящите скоби на контролните конструкции НЕ ТРЯБВА да имат интервал след тях,
  а затварящите – НЕ ТРЯБВА да имат интервал преди тях.

### 1.1. Пример

Примерът показва някои от правилата:

```php
<?php
namespace Vendor\Package;

use FooInterface;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class Foo extends Bar implements FooInterface
{
    public function sampleFunction($a, $b = null)
    {
        if ($a === $b) {
            bar();
        } elseif ($a > $b) {
            $foo->bar($arg1);
        } else {
            BazClass::bar($arg2, $arg3);
        }
    }

    final public static function bar()
    {
        // тяло на метода
    }
}
```

2. Основни
----------

### 2.1 Основен стандарт за кодиране

Кодът ТРЯБВА да следва всички правила от [PSR-1].

### 2.2 Файлове

Всички PHP файлове ТРЯБВА да използват Unix LF (нов ред) за завършване на реда.

Всички PHP файлове ТРЯБВА да завършват с единичен празен ред.

Тагът `?>` ТРЯБВА да се пропуска от файловете, съдържащи само PHP код.

### 2.3. Редове

НЕ ТРЯБВА да има твърдо ограничение за дължина на реда.

Препоръчителното ограничение на реда ТРЯБВА да е 120 символа; автоматизираните
стилови проверки ТРЯБВА да предупреждават, но НЕ ТРЯБВА да изкарват грешка при
достигане на лимита.

Редовете НЕ БИ ТРЯБВАЛО да са по-дълги от 80 символа; по-дългите редове БИ ТРЯБВАЛО
да се разделят на няколко последващи реда, всеки от които не надвишаващ 80 символа.

В края на не-празните редове НЕ ТРЯБВА да има бяло поле.

Празни редове МОЖЕ да бъдат добавяни за да се подобри четливостта и, за да се покаже
връзка между блоковете с код.

НЕ ТРЯБВА да има повече от една конструкция на ред.

### 2.4. Отстъп

Кодът ТРЯБВА да използва отстъп от 4 интервала, и НЕ ТРЯБВА да използва табове.

> N.b.: Използването само на интервали, без да се смесват с табове, помага да
> се избегнат проблеми при сравнения, кръпки, история и анотации. Интервалите
> също правят по-лесно вмъкването на точни отмествания при подравняването на
> няколко реда.

### 2.5. Ключови думи и True/False/Null

PHP [ключовите думи] ТРЯБВА да се пишат в долен регистър.

Константите `true`, `false` и `null` ТРЯБВА да се пишат в долен регистър.

[ключовите думи]: http://php.net/manual/en/reserved.keywords.php



3. Именно пространство и декларации Use
---------------------------------------

ТРЯБВА да има един празен ред след `namespace` декларацията, когато е налична.

Когато са налични, всички `use` декларации ТРЯБВА да се поставят след `namespace`.

ТРЯБВА да има по една ключова дума `use` за декларация.

ТРЯБВА да има един празен ред след блока от `use` декларации.

Например:

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

// ... допълнителен PHP код ...

```


4. Класове, Свойства и методи
-----------------------------

Терминът „клас“ се отнася до всички класове, интерфейси и traits.

### 4.1. Наследявания и реализации

Ключовите думи `extends` и `implements` ТРЯБВА да се декларират на същия ред
на който е и името на класа.

Отварящата скоба за класа ТРЯБВА да е на нов ред; затварящата скоба на класа
ТРЯБВА да е на следващ ред след тялото.

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements \ArrayAccess, \Countable
{
    // константи, свойства, методи
}
```

Изброяването на `implements` МОЖЕ да се раздели на няколко реда, като
всеки ред трябва да се премести един път навътре. Правейки това, първият
елемент от списъка ТРЯБВА да е на следващия ред и ТРЯБВА да има само
един интерфейс на ред.

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements
    \ArrayAccess,
    \Countable,
    \Serializable
{
    // константи, свойства, методи
}
```

### 4.2. Свойства

Видимостта ТРЯБВА да се декларира на всички свойства.

Ключовата дума `var` НЕ ТРЯБВА да се използва при декларация.

НЕ ТРЯБВА да има повече от едно декларирано свойство на ред.

Имената на свойствата НЕ БИ ТРЯБВАЛО да започват с единично подчертаващо
тире, индикиращо видимост protected или private.

Декларацията за свойство  изглежда по следния начин.

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public $foo = null;
}
```

### 4.3. Методи

Видимостта ТРЯБВА да се декларира на всички методи.

Методите НЕ БИ ТРЯБВАЛО да започват с единично подчертаващо тире, индикиращо
видимост protected или private.

Имената на методите НЕ ТРЯБВА да се декларират с интервал след името си.
Отварящата скоба ТРЯБВА да е на нов ред и затварящата ТРЯБВА да е на нов
ред, веднага след тялото. НЕ ТРЯБВА да има интервал след отварящата скоба
и не ТРЯБВА да има интервал преди затварящата скоба.

Декларацията на метод изглежда по следния начин. Забележете позицията на
къдравите скобите, запетаите, интервалите и кръглите скоби.

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function fooBarBaz($arg1, &$arg2, $arg3 = [])
    {
        // тяло на метода
    }
}
```

### 4.4. Аргументи на метод

В списъка с аргументи НЕ ТРЯБВА да има интервал преди всяка запетая; ТРЯБВА
да има интервал след всяка запетая.

Аргументи с подразбиращи се стойности ТРЯБВА да се поставят в края на списъка.

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function foo($arg1, &$arg2, $arg3 = [])
    {
        // тяло на метода
    }
}
```

Списъкът с аргументи МОЖЕ да се раздели на няколко реда, където всеки под-ред
се отмества навътре. Правейки това, първият елемент в списъка ТРЯБВА да е на
нов ред и ТРЯБВА да има един аргумент на ред.

Когато списъкът е разделен на няколко реда, затварящата кръгла и отварящата
къдрава скоба ТРЯБВА да се поставят заедно на нов свой ред с един интервал
помежду им.

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function aVeryLongMethodName(
        ClassTypeHint $arg1,
        &$arg2,
        array $arg3 = []
    ) {
        // method body
    }
}
```

### 4.5. `abstract`, `final`, и `static`

Когато присъстват, декларациите `abstract` и `final` ТРЯБВА да предшестват
декларацията за видимост.

Когато присъства, декларацията `static` ТРЯБВА да е след тази за видимост.

```php
<?php
namespace Vendor\Package;

abstract class ClassName
{
    protected static $foo;

    abstract protected function zim();

    final public static function bar()
    {
        // method body
    }
}
```

### 4.6. Извикване на методи и функции

Когато се извиква метод или функция, НЕ ТРЯБВА да има интервал между името
на функцията или метода, и отварящата скоба. НЕ ТРЯБВА да има интервал
след отварящата и преди затварящата скоба. В списъкът с аргументи НЕ ТРЯБВА
да има интервал преди всяка запетая, ТРЯБВА да има интервал след нея.

```php
<?php
bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
```
Списъкът с аргументи може да се раздели на няколко реда, като всеки ред
е отместен веднъж навътре. Правейки това, първият елемент от списъка ТРЯБВА
да е на нов ред и ТРЯБВА да има един аргумент на ред.

```php
<?php
$foo->bar(
    $longArgument,
    $longerArgument,
    $muchLongerArgument
);
```

5. Управляващи конструкции
--------------------------

Основните стилови правила за управляващите структури са:

- ТРЯБВА да има интервал след ключова дума на управляващата структура
- НЕ ТРЯБВА да има интервал след отваряща къдрава скоба
- НЕ ТРЯБВА да има интервал преди затваряща къдрава скоба
- ТРЯБВА да има интервал между затваряща къдрава скоба и отваряща кръгла скоба
- Тялото на структурата ТРЯБВА веднъж да бъде отместено навътре
- Затварящата скоба ТРЯБВА да е на следващ ред след тялото

Тялото на всяка структура ТРЯБВА да се обгради със скоби. Това стандартизира
изгледът на структурата и намалява вероятността за поява на грешки, когато
нови редове се добавят към тялото.


### 5.1. `if`, `elseif`, `else`

Оператора `if` изглежда по следния начин. Забележете местоположението на къдравите
скоби, интервали и кръглите скоби, а също и това че `else` и `elseif` са на същия
ред на който и затварящата скоба от предишното тяло.

```php
<?php
if ($expr1) {
    // ако тяло
} elseif ($expr2) {
    // или ако тяло
} else {
    // или тяло;
}
```

Ключовата дума `elseif` БИ ТРЯБВАЛО да се използва вместо `else if`, за да изглеждат
всички управляващи конструкции като единични ключови думи.


### 5.2. `switch`, `case`

Структурата на `switch` изглежда по следния начин. Забележете местоположението
на къдравите скоби, интервалите и кръглите скоби. Конструкцията `case` ТРЯБВА
да бъде отместена веднъж навътре спрямо `switch`, а `break` (или друга терминираща
дума) ТРЯБВА да бъде подравнена на същото ниво на което е тялото на `case`.
ТРЯБВА да има коментар като `// no break` при умишлено нетерминиран `case`.

```php
<?php
switch ($expr) {
    case 0:
        echo 'Първи случай, с прекъсване';
        break;
    case 1:
        echo 'Втори случай, преминаващ надолу';
        // no break
    case 2:
    case 3:
    case 4:
        echo 'Трети случай, return вместо break';
        return;
    default:
        echo 'Случай по подразбиране';
        break;
}
```


### 5.3. `while`, `do while`

Конструкцията `while` изглежда по този начин. Забележете местоположението
на къдравите скоби, интервалите и кръглите скоби.

```php
<?php
while ($expr) {
    // тяло на структурата
}
```

По същия начин, конструкцията `do while` изглежда по следния начин. Забележете
местоположението на къдравите скоби, интервалите и кръглите скоби.

```php
<?php
do {
    // тяло на структурата;
} while ($expr);
```

### 5.4. `for`

Конструкцията `for` изглежда по този начин. Забележете местоположението
на къдравите скоби, интервалите и кръглите скоби.

```php
<?php
for ($i = 0; $i < 10; $i++) {
    // for body
}
```

### 5.5. `foreach`

Конструкцията `foreach` изглежда по този начин. Забележете местоположението
на къдравите скоби, интервалите и кръглите скоби.

```php
<?php
foreach ($iterable as $key => $value) {
    // foreach body
}
```

### 5.6. `try`, `catch`

Блокът `try catch` изглежда по този начин. Забележете местоположението
на къдравите скоби, интервалите и кръглите скоби.

```php
<?php
try {
    // try тяло
} catch (FirstExceptionType $e) {
    // catch тяло
} catch (OtherExceptionType $e) {
    // catch тяло
}
```

6. Затварящи функции (closure)
------------------------------

Функциите ТРЯБВА да се декларират с интервал след ключовата дума `function` и
с интервал преди ключовата дума `use`.

Отварящата скоба ТРЯБВА да е на същия ред, а затварящата скоба – ТРЯБВА да е
на следващия ред след тялото.

НЕ ТРЯБВА да има интервал след отварящата и преди затварящата кръгла скоба
за списъка с аргументи или променливи.

В списъка с аргументи и променливи, НЕ ТРЯБВА да има интервал преди всяка запетая,
но ТРЯБВА да има след всяка.

Аргументи с подразбиращи се стойности ТРЯБВА да се поставят в края на списъка.

Декларация на затваряща функция изглежда по следният начин. Забележете
местоположението на кръглите скоби, запетаите, интервалите и къдравите скоби.

```php
<?php
$closureWithArgs = function ($arg1, $arg2) {
    // тяло
};

$closureWithArgsAndVars = function ($arg1, $arg2) use ($var1, $var2) {
    // тяло
};
```

Списъкът с аргументи и променливи МОЖЕ да се раздели на няколко реда, като
всеки под-ред е изместен навътре. Правейки това, първият елемент на
списъка ТРЯБВА да е на нов ред, и ТРЯБВА да има един аргумент или променлива
на ред.

Когато завършващият списък (без значение аргументи или променливи) е разбит
на няколко реда, затварящата кръгла скоба и отварящата къдрава ТРЯБВА да се
поставят на нов ред, една до друга, разделени с интервал помежду им.

Следващите примери показват затварящи функции с и без списък от аргументи и
променливи, разбити на няколко реда.

```php
<?php
$longArgs_noVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) {
   // тяло
};

$noArgs_longVars = function () use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // тяло
};

$longArgs_longVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // тяло
};

$longArgs_shortVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use ($var1) {
   // тяло
};

$shortArgs_longVars = function ($arg) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // тяло
};
```

Забележете, че правилата за форматиране важат и когато затварящата функция се
използва директно в извикването на метод или друга функция като аргумент.

```php
<?php
$foo->bar(
    $arg1,
    function ($arg2) use ($var1) {
        // тяло
    },
    $arg3
);
```


7. Заключение
-------------

Има много аспекти на стилизацията, непокрити от това ръководство. Те включват,
но не са ограничени до:

- Декларация на глобални променливи и константи

- Декларация на функции

- Оператори и присвоявания

- Подравняване на редове

- Коментари и документиращи блокове

- Представки и надствки към имената на класовете

- Най-добри практики

Бъдещи препоръки МОЖЕ да променят и допълват това ръководство, за да се адресират
тези други елементи за стилизиране.


Приложение А. Изследване
------------------------

In writing this style guide, the group took a survey of member projects to
determine common practices.  The survey is retained herein for posterity.

### A.1. Survey Data

    url,http://www.horde.org/apps/horde/docs/CODING_STANDARDS,http://pear.php.net/manual/en/standards.php,http://solarphp.com/manual/appendix-standards.style,http://framework.zend.com/manual/en/coding-standard.html,http://symfony.com/doc/2.0/contributing/code/standards.html,http://www.ppi.io/docs/coding-standards.html,https://github.com/ezsystems/ezp-next/wiki/codingstandards,http://book.cakephp.org/2.0/en/contributing/cakephp-coding-conventions.html,https://github.com/UnionOfRAD/lithium/wiki/Spec%3A-Coding,http://drupal.org/coding-standards,http://code.google.com/p/sabredav/,http://area51.phpbb.com/docs/31x/coding-guidelines.html,https://docs.google.com/a/zikula.org/document/edit?authkey=CPCU0Us&hgd=1&id=1fcqb93Sn-hR9c0mkN6m_tyWnmEvoswKBtSc0tKkZmJA,http://www.chisimba.com,n/a,https://github.com/Respect/project-info/blob/master/coding-standards-sample.php,n/a,Object Calisthenics for PHP,http://doc.nette.org/en/coding-standard,http://flow3.typo3.org,https://github.com/propelorm/Propel2/wiki/Coding-Standards,http://developer.joomla.org/coding-standards.html
    voting,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,no,no,no,?,yes,no,yes
    indent_type,4,4,4,4,4,tab,4,tab,tab,2,4,tab,4,4,4,4,4,4,tab,tab,4,tab
    line_length_limit_soft,75,75,75,75,no,85,120,120,80,80,80,no,100,80,80,?,?,120,80,120,no,150
    line_length_limit_hard,85,85,85,85,no,no,no,no,100,?,no,no,no,100,100,?,120,120,no,no,no,no
    class_names,studly,studly,studly,studly,studly,studly,studly,studly,studly,studly,studly,lower_under,studly,lower,studly,studly,studly,studly,?,studly,studly,studly
    class_brace_line,next,next,next,next,next,same,next,same,same,same,same,next,next,next,next,next,next,next,next,same,next,next
    constant_names,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper
    true_false_null,lower,lower,lower,lower,lower,lower,lower,lower,lower,upper,lower,lower,lower,upper,lower,lower,lower,lower,lower,upper,lower,lower
    method_names,camel,camel,camel,camel,camel,camel,camel,camel,camel,camel,camel,lower_under,camel,camel,camel,camel,camel,camel,camel,camel,camel,camel
    method_brace_line,next,next,next,next,next,same,next,same,same,same,same,next,next,same,next,next,next,next,next,same,next,next
    control_brace_line,same,same,same,same,same,same,next,same,same,same,same,next,same,same,next,same,same,same,same,same,same,next
    control_space_after,yes,yes,yes,yes,yes,no,yes,yes,yes,yes,no,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes
    always_use_control_braces,yes,yes,yes,yes,yes,yes,no,yes,yes,yes,no,yes,yes,yes,yes,no,yes,yes,yes,yes,yes,yes
    else_elseif_line,same,same,same,same,same,same,next,same,same,next,same,next,same,next,next,same,same,same,same,same,same,next
    case_break_indent_from_switch,0/1,0/1,0/1,1/2,1/2,1/2,1/2,1/1,1/1,1/2,1/2,1/1,1/2,1/2,1/2,1/2,1/2,1/2,0/1,1/1,1/2,1/2
    function_space_after,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no
    closing_php_tag_required,no,no,no,no,no,no,no,no,yes,no,no,no,no,yes,no,no,no,no,no,yes,no,no
    line_endings,LF,LF,LF,LF,LF,LF,LF,LF,?,LF,?,LF,LF,LF,LF,?,,LF,?,LF,LF,LF
    static_or_visibility_first,static,?,static,either,either,either,visibility,visibility,visibility,either,static,either,?,visibility,?,?,either,either,visibility,visibility,static,?
    control_space_parens,no,no,no,no,no,no,yes,no,no,no,no,no,no,yes,?,no,no,no,no,no,no,no
    blank_line_after_php,no,no,no,no,yes,no,no,no,no,yes,yes,no,no,yes,?,yes,yes,no,yes,no,yes,no
    class_method_control_brace,next/next/same,next/next/same,next/next/same,next/next/same,next/next/same,same/same/same,next/next/next,same/same/same,same/same/same,same/same/same,same/same/same,next/next/next,next/next/same,next/same/same,next/next/next,next/next/same,next/next/same,next/next/same,next/next/same,same/same/same,next/next/same,next/next/next

### A.2. Survey Legend

`indent_type`:
The type of indenting. `tab` = "Use a tab", `2` or `4` = "number of spaces"

`line_length_limit_soft`:
The "soft" line length limit, in characters. `?` = not discernible or no response, `no` means no limit.

`line_length_limit_hard`:
The "hard" line length limit, in characters. `?` = not discernible or no response, `no` means no limit.

`class_names`:
How classes are named. `lower` = lowercase only, `lower_under` = lowercase with underscore separators, `studly` = StudlyCase.

`class_brace_line`:
Does the opening brace for a class go on the `same` line as the class keyword, or on the `next` line after it?

`constant_names`:
How are class constants named? `upper` = Uppercase with underscore separators.

`true_false_null`:
Are the `true`, `false`, and `null` keywords spelled as all `lower` case, or all `upper` case?

`method_names`:
How are methods named? `camel` = `camelCase`, `lower_under` = lowercase with underscore separators.

`method_brace_line`:
Does the opening brace for a method go on the `same` line as the method name, or on the `next` line?

`control_brace_line`:
Does the opening brace for a control structure go on the `same` line, or on the `next` line?

`control_space_after`:
Is there a space after the control structure keyword?

`always_use_control_braces`:
Do control structures always use braces?

`else_elseif_line`:
When using `else` or `elseif`, does it go on the `same` line as the previous closing brace, or does it go on the `next` line?

`case_break_indent_from_switch`:
How many times are `case` and `break` indented from an opening `switch` statement?

`function_space_after`:
Do function calls have a space after the function name and before the opening parenthesis?

`closing_php_tag_required`:
In files containing only PHP, is the closing `?>` tag required?

`line_endings`:
What type of line ending is used?

`static_or_visibility_first`:
When declaring a method, does `static` come first, or does the visibility come first?

`control_space_parens`:
In a control structure expression, is there a space after the opening parenthesis and a space before the closing parenthesis? `yes` = `if ( $expr )`, `no` = `if ($expr)`.

`blank_line_after_php`:
Is there a blank line after the opening PHP tag?

`class_method_control_brace`:
A summary of what line the opening braces go on for classes, methods, and control structures.

### A.3. Survey Results

    indent_type:
        tab: 7
        2: 1
        4: 14
    line_length_limit_soft:
        ?: 2
        no: 3
        75: 4
        80: 6
        85: 1
        100: 1
        120: 4
        150: 1
    line_length_limit_hard:
        ?: 2
        no: 11
        85: 4
        100: 3
        120: 2
    class_names:
        ?: 1
        lower: 1
        lower_under: 1
        studly: 19
    class_brace_line:
        next: 16
        same: 6
    constant_names:
        upper: 22
    true_false_null:
        lower: 19
        upper: 3
    method_names:
        camel: 21
        lower_under: 1
    method_brace_line:
        next: 15
        same: 7
    control_brace_line:
        next: 4
        same: 18
    control_space_after:
        no: 2
        yes: 20
    always_use_control_braces:
        no: 3
        yes: 19
    else_elseif_line:
        next: 6
        same: 16
    case_break_indent_from_switch:
        0/1: 4
        1/1: 4
        1/2: 14
    function_space_after:
        no: 22
    closing_php_tag_required:
        no: 19
        yes: 3
    line_endings:
        ?: 5
        LF: 17
    static_or_visibility_first:
        ?: 5
        either: 7
        static: 4
        visibility: 6
    control_space_parens:
        ?: 1
        no: 19
        yes: 2
    blank_line_after_php:
        ?: 1
        no: 13
        yes: 8
    class_method_control_brace:
        next/next/next: 4
        next/next/same: 11
        next/same/same: 1
        same/same/same: 6
