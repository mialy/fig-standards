Интерфейс за дневници
=====================

Документът описва общият интерфейс на библиотеките за работа с дневници.

Основната цел е да позволи на библиотеките да приемат обект от тип
`Psr\Log\LoggerInterface` и да записват логове по прост и универсален начин.
Рамките и системите за съдържание, имащи специфични нужди, МОЖЕ да разширят
интерфейса, но БИ ТРЯБВАЛО да запазят съвместимост с този документ. Това ще
позволи библиотеки, писани от трети страни и използвани от приложението, да
могат да записват логовете на централизирано място.

Ключовите думи „ТРЯБВА“, „НЕ ТРЯБВА“, „ЗАДЪЛЖИТЕЛЕН“, “СЛЕДВА“, „НЕ СЛЕДВА“,
„БИ ТРЯБВАЛО“, “НЕ БИ ТРЯБВАЛО“, „ПРЕПОРЪЧИТЕЛЕН“, „МОЖЕ“ и „НЕЗАДЪЛЖИТЕЛЕН“ в
този документ се интерпретират като описаните в [RFC 2119][]

Думата `реализатор` в този документ трябва да се интерпретира като някой
който е реализирал `LoggerInterface` в свързаните с дневници библиотеки или
рамки. Потребителите на тези реализации, са посочени като `потребител`.

[RFC 2119]: http://tools.ietf.org/html/rfc2119

1. Спецификация
---------------

### 1.1 Основи

- `LoggerInterface` дефинира осем метода за записване на логове в осем нива
  по [RFC 5424][] (debug, info, notice, warning, error, critical, alert,
  emergency).

- Деветият метод, `log`, получава ниво на лога като първи аргумент. Извиквайки
  този метод с една от константите за нива, ТРЯБВА да има същия резултат като и
  извикването на метода за специфично ниво. Извикването на метод за ниво,
  недефинирано от тази спецификация ТРЯБВА да хвърля `Psr\Log\InvalidArgumentException`
  ако имплементацията не знае нищо за нивото. Потребителите НЕ БИ ТРЯБВАЛО да
  да използват свое ниво без да знаят със сигурност дали текущата реалзиця го поддържа.

[RFC 5424]: http://tools.ietf.org/html/rfc5424

### 1.2 Съобщение

- Всеки метод приема низ като съобщение, или обект с метод `__toString()`.
  Реализаторите МОЖЕ да обработват специфично подадените обекти. Ако случаят
  не е такъв, реализаторите ТРЯБВА да преобразуват обекта в низ.

- Съобщението МОЖЕ да съдържа заместващи поднизове (placeholder) на които
  реализаторите МОЖЕ да заместят със стойности от контекстен масив.

  Заместващите имена ТРЯБВА да съответстват на ключовете от контекстния масив.

  Заместващите имена ТРЯБВА да се разграничат с единична отваряща къдрава скоба `{`
  и единична затваряща `}`. НЕ ТРЯБВА да има празно поле между разделителите и
  името на заместващия подниз.

  Заместващите имена БИ ТРЯБВАЛО да са съставени само от символите `A-Z`, `a-z`,
  `0-9`, подчертаващо тире `_` и точка `.`. Използването на други символи е
  резервирано за бъдещи промени от спецификацията за заместващи поднизове.

  Реализаторите МОЖЕ да използват заместващи поднизове, за да постигнат различни
  начини за ексейпване и превеждане на логове. Потребителите НЕ БИ ТРЯБВАЛО да
  правят предварително ескейпване на заместващите поднизове, тъй като не могат
  да знаят в какъв контекст ще се визуализира информацията.

  Следният пример показва реализация на интерполация със заместващи поднизове,
  предоставен само за справочна цел:

  ```php
  /**
   * Интерполира контекстни стойности в заместващище поднизове на съобщението
   */
  function interpolate($message, array $context = array())
  {
      // построява заместващ масив със заобиколени с къдрави скоби контекстни ключове
      $replace = array();
      foreach ($context as $key => $val) {
          $replace['{' . $key . '}'] = $val;
      }

      // интерполира заместващи стойности в съобщението, а след това го връща
      return strtr($message, $replace);
  }

  // съобщение с заместващ подниз, ограден от къдрави скоби
  $message = "User {username} created";

  // контекстен масив от заместващо име => заместваща стойност
  $context = array('username' => 'bolivar');

  // отпечатва "User bolivar created"
  echo interpolate($message, $context);
  ```

### 1.3 Контекст

- Всеки метод приема масив с контекстна информация. Има за цел да държи
  всякаква допълнителна информация, която не пасва добре в низ. Масивът
  може да съдържа всичко. Реализаторите ТРЯБВА да гарантират, че обработват
  контекстната информация колкото се може по-снизходително. Получените стойности
  от конткеста НЕ ТРЯБВА да хвърлят изключение, нито да вдигат някаква
  PHP грешка, предупреждение или известие.

- Ако обект `Exception` е подаден към конткестната информация, той ТРЯБВА да
  е в ключ `'exception'`. Логването на изключения е общ шаблон и позволява на
  реализаторите да извлекат стековия трейс от изключението, когато това се поддържа.
  Реализаторите ТРЯБВА все пак да потвърдят дали ключът `'exception'` е всъщност
  е `Exception`, преди да го използват, тъй като той МОЖЕ да съдържа всичко.

### 1.4 Спомагателни класове и интерфейси

- Класът `Psr\Log\AbstractLogger` позволява да се реализира `LoggerInterface`
  много лесно като се наследи и се реализира общият метод `log`.
  Другите осем метода пренасочват съобщението и контекста към него.

- По подобен начин, използвайки `Psr\Log\LoggerTrait` е необходимо само да се
  реализира общият метод `log`. Забележете че тъй като trait не може да реализира
  интерфейси, в случая все пак е необходимо реализирането на `LoggerInterface`.

- `Psr\Log\NullLogger` се предоставя заедно с интерфейса. Той МОЖЕ да се използва
  от потребители на интерфейса, за да предостави аварийна реализация тип „черна дупка“,
  ако никакъв логер не е предоставен. Все пак условното логване може да е по-добър
  подход, ако контекстната информация е доста скъпа.

- `Psr\Log\LoggerAwareInterface` само съдържа метода `setLogger(LoggerInterface $logger)`
  и може да се изпозва от рамките, за да установят инстанции на логера.

- Trait `Psr\Log\LoggerAwareTrait` може да се изпозва за лесно реализиране на
  еквивалентен интерфейс във всеки клас. Той дава достъп чрез `$this->logger`.

- Класът `Psr\Log\LogLevel` съдържа константите за осемте нива на логване.

2. Пакет
--------

Интерфейсите и класовете, както и съответните им изключения и тестове,
потвърждаващи Вашата реализация, се предоставят като част от пакета
[psr/log](https://packagist.org/packages/psr/log).

3. `Psr\Log\LoggerInterface`
----------------------------

```php
<?php

namespace Psr\Log;

/**
 * Describes a logger instance
 *
 * The message MUST be a string or object implementing __toString().
 *
 * The message MAY contain placeholders in the form: {foo} where foo
 * will be replaced by the context data in key "foo".
 *
 * The context array can contain arbitrary data, the only assumption that
 * can be made by implementors is that if an Exception instance is given
 * to produce a stack trace, it MUST be in a key named "exception".
 *
 * See https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-3-logger-interface.md
 * for the full interface specification.
 */
interface LoggerInterface
{
    /**
     * System is unusable.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function emergency($message, array $context = array());

    /**
     * Action must be taken immediately.
     *
     * Example: Entire website down, database unavailable, etc. This should
     * trigger the SMS alerts and wake you up.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function alert($message, array $context = array());

    /**
     * Critical conditions.
     *
     * Example: Application component unavailable, unexpected exception.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function critical($message, array $context = array());

    /**
     * Runtime errors that do not require immediate action but should typically
     * be logged and monitored.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function error($message, array $context = array());

    /**
     * Exceptional occurrences that are not errors.
     *
     * Example: Use of deprecated APIs, poor use of an API, undesirable things
     * that are not necessarily wrong.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function warning($message, array $context = array());

    /**
     * Normal but significant events.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function notice($message, array $context = array());

    /**
     * Interesting events.
     *
     * Example: User logs in, SQL logs.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function info($message, array $context = array());

    /**
     * Detailed debug information.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function debug($message, array $context = array());

    /**
     * Logs with an arbitrary level.
     *
     * @param mixed $level
     * @param string $message
     * @param array $context
     * @return null
     */
    public function log($level, $message, array $context = array());
}
```

4. `Psr\Log\LoggerAwareInterface`
---------------------------------

```php
<?php

namespace Psr\Log;

/**
 * Describes a logger-aware instance
 */
interface LoggerAwareInterface
{
    /**
     * Sets a logger instance on the object
     *
     * @param LoggerInterface $logger
     * @return null
     */
    public function setLogger(LoggerInterface $logger);
}
```

5. `Psr\Log\LogLevel`
---------------------

```php
<?php

namespace Psr\Log;

/**
 * Describes log levels
 */
class LogLevel
{
    const EMERGENCY = 'emergency';
    const ALERT     = 'alert';
    const CRITICAL  = 'critical';
    const ERROR     = 'error';
    const WARNING   = 'warning';
    const NOTICE    = 'notice';
    const INFO      = 'info';
    const DEBUG     = 'debug';
}
```
