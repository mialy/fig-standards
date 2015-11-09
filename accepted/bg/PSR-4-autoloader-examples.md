Примерна реализация на PSR-4
============================

Следващите примери илюстрират съвместим с PSR-4 код:

Пример с анонимна функция
-------------------------

```php
<?php
/**
 * Примерна специфична за проект реализация.
 *
 * След регистрирането на тази функция с SPL, следващият ред
 * ще накара функцията да опита да зареди класът \Foo\Bar\Baz\Qux
 * от /path/to/project/src/Baz/Qux.php:
 *
 *      new \Foo\Bar\Baz\Qux;
 *
 * @param string $class Пълно име на класа.
 * @return void
 */
spl_autoload_register(function ($class) {

    // Специфична за проекта представка на именното пространство
    $prefix = 'Foo\\Bar\\';

    // Основна директория за именното пространство
    $base_dir = __DIR__ . '/src/';

    // Класът ползва ли представка на именното пространство?
    $len = strlen($prefix);
    if (strncmp($prefix, $class, $len) !== 0) {
        // не, нататък към следващия регистриран авт. зареждач
        return;
    }

    // вземи относителното име на класа
    $relative_class = substr($class, $len);

    // заместване на именното пространство с основната директория, заместване
    // на разделителите в името с директорийни в относителното име на класа,
    // добавяне на .php
    $file = $base_dir . str_replace('\\', '/', $relative_class) . '.php';

    // ако файлът съществува, изискай го (зареди го)
    if (file_exists($file)) {
        require $file;
    }
});
```

Пример с клас
-------------

Следващият пример е на имплементация на клас, обработващ няколко
именни пространства:

```php
<?php
namespace Example;

/**
 * Примерна реализация с общо предназначение, която включва незадължителната
 * функционалност, позволяваща няколко различни основни директории за една и
 * съща представка на именно пространство.
 *
 * Ако имаме пакет от класове foo-bar в файловата система със следните пътища
 * ...
 *
 *     /path/to/packages/foo-bar/
 *         src/
 *             Baz.php             # Foo\Bar\Baz
 *             Qux/
 *                 Quux.php        # Foo\Bar\Qux\Quux
 *         tests/
 *             BazTest.php         # Foo\Bar\BazTest
 *             Qux/
 *                 QuuxTest.php    # Foo\Bar\Qux\QuuxTest
 *
 * ... и път до файловете с класовете за представката \Foo\Bar\ е следната:
 *
 *      <?php
 *      // instantiate the loader
 *      $loader = new \Example\Psr4AutoloaderClass;
 *
 *      // register the autoloader
 *      $loader->register();
 *
 *      // register the base directories for the namespace prefix
 *      $loader->addNamespace('Foo\Bar', '/path/to/packages/foo-bar/src');
 *      $loader->addNamespace('Foo\Bar', '/path/to/packages/foo-bar/tests');
 *
 * Следващият ред ще накара автоматичния зареждач да опита да зареди класът
 * \Foo\Bar\Qux\Quux от /path/to/packages/foo-bar/src/Qux/Quux.php:
 *
 *      <?php
 *      new \Foo\Bar\Qux\Quux;
 *
 * А следващият ред – класът \Foo\Bar\Qux\QuuxTest от
 * /path/to/packages/foo-bar/tests/Qux/QuuxTest.php:
 *
 *      <?php
 *      new \Foo\Bar\Qux\QuuxTest;
 */
class Psr4AutoloaderClass
{
    /**
     * Асоциативен масив където ключът е представка на именното пространство,
     * а стойността – масив за основни директории, за класовете от това пространство.
     *
     * @var array
     */
    protected $prefixes = array();

    /**
     * Регистриране на зареждач през SPL стек.
     *
     * @return void
     */
    public function register()
    {
        spl_autoload_register(array($this, 'loadClass'));
    }

    /**
     * Добавя основна директория за именно пространство.
     *
     * @param string $prefix Представка на именното пространство.
     * @param string $base_dir Основна директория за файловете на класовете.
     * @param bool $prepend Ако е true, поставя основната директория в началото на стека,
     * вместо в края му; Това позволява да бъде претърсван първи вместо последен.
     * @return void
     */
    public function addNamespace($prefix, $base_dir, $prepend = false)
    {
        // нормализиране на представката на именното пространство
        $prefix = trim($prefix, '\\') . '\\';

        // нормализиране на основната директория с завършващ разделител
        $base_dir = rtrim($base_dir, DIRECTORY_SEPARATOR) . '/';

        // инициализация на масива с именни пространства
        if (isset($this->prefixes[$prefix]) === false) {
            $this->prefixes[$prefix] = array();
        }

        // запазване основната директория за дадената представка на именното пространство
        if ($prepend) {
            array_unshift($this->prefixes[$prefix], $base_dir);
        } else {
            array_push($this->prefixes[$prefix], $base_dir);
        }
    }

    /**
     * Зарежда файлът отговарящ за дадения клас
     *
     * @param string $class Пълно име на класа.
     * @return mixed Съответстващото файлово име при успех, иначе при грешка – false.
     */
    public function loadClass($class)
    {
        // представка на текущото именно пространство
        $prefix = $class;

        // отзад напред през именните пространства на пълното име на класа,
        // за да се намери съответстващият файл
        while (false !== $pos = strrpos($prefix, '\\')) {

            // запазване на разделителя на именното пространство в представката
            $prefix = substr($class, 0, $pos + 1);

            // останалато е относително име на класа
            $relative_class = substr($class, $pos + 1);

            // опит за зареждане на съответстващият файл към относителния клас
            $mapped_file = $this->loadMappedFile($prefix, $relative_class);
            if ($mapped_file) {
                return $mapped_file;
            }

            // премахване на завършващия разделител на именните пространства,
            // за следващата итерация на strrpos()
            $prefix = rtrim($prefix, '\\');
        }

        // не е намерен съответстващ файл
        return false;
    }

    /**
     * Зареждане на съответстващ файл според даден относителен клас и представка
     *
     * @param string $prefix Представка на именното пространство.
     * @param string $relative_class Относително име на класа.
     * @return mixed Връща false ако няма съответстващ файл, който да може да се зареди,
     * или името на файла, което е било заредено.
     */
    protected function loadMappedFile($prefix, $relative_class)
    {
        // налична ли е поне една основна директория за тази представка?
        if (isset($this->prefixes[$prefix]) === false) {
            return false;
        }

        // претърсване през основните директории за тази представка
        foreach ($this->prefixes[$prefix] as $base_dir) {

            // заместване на представката с основната директория,
            // заместване на разделителите на именното пространство с директорийни,
            // добавяне на .php
            $file = $base_dir
                  . str_replace('\\', '/', $relative_class)
                  . '.php';

            // Ако съответстващият файл съществува, изискай го (зареди го)
            if ($this->requireFile($file)) {
                // да, готови сме
                return $file;
            }
        }

        // не е намерен
        return false;
    }

    /**
     * Ако файлът съществува, изисквай го (зареди го) от файловата система.
     *
     * @param string $file Търсен файл
     * @return bool True ако файлът съществува, иначе false.
     */
    protected function requireFile($file)
    {
        if (file_exists($file)) {
            require $file;
            return true;
        }
        return false;
    }
}
```

### Компонентни тестове

Следващият пример е компонентно тестване на класът за зареждане по-горе:

```php
<?php
namespace Example\Tests;

class MockPsr4AutoloaderClass extends Psr4AutoloaderClass
{
    protected $files = array();

    public function setFiles(array $files)
    {
        $this->files = $files;
    }

    protected function requireFile($file)
    {
        return in_array($file, $this->files);
    }
}

class Psr4AutoloaderClassTest extends \PHPUnit_Framework_TestCase
{
    protected $loader;

    protected function setUp()
    {
        $this->loader = new MockPsr4AutoloaderClass;

        $this->loader->setFiles(array(
            '/vendor/foo.bar/src/ClassName.php',
            '/vendor/foo.bar/src/DoomClassName.php',
            '/vendor/foo.bar/tests/ClassNameTest.php',
            '/vendor/foo.bardoom/src/ClassName.php',
            '/vendor/foo.bar.baz.dib/src/ClassName.php',
            '/vendor/foo.bar.baz.dib.zim.gir/src/ClassName.php',
        ));

        $this->loader->addNamespace(
            'Foo\Bar',
            '/vendor/foo.bar/src'
        );

        $this->loader->addNamespace(
            'Foo\Bar',
            '/vendor/foo.bar/tests'
        );

        $this->loader->addNamespace(
            'Foo\BarDoom',
            '/vendor/foo.bardoom/src'
        );

        $this->loader->addNamespace(
            'Foo\Bar\Baz\Dib',
            '/vendor/foo.bar.baz.dib/src'
        );

        $this->loader->addNamespace(
            'Foo\Bar\Baz\Dib\Zim\Gir',
            '/vendor/foo.bar.baz.dib.zim.gir/src'
        );
    }

    public function testExistingFile()
    {
        $actual = $this->loader->loadClass('Foo\Bar\ClassName');
        $expect = '/vendor/foo.bar/src/ClassName.php';
        $this->assertSame($expect, $actual);

        $actual = $this->loader->loadClass('Foo\Bar\ClassNameTest');
        $expect = '/vendor/foo.bar/tests/ClassNameTest.php';
        $this->assertSame($expect, $actual);
    }

    public function testMissingFile()
    {
        $actual = $this->loader->loadClass('No_Vendor\No_Package\NoClass');
        $this->assertFalse($actual);
    }

    public function testDeepFile()
    {
        $actual = $this->loader->loadClass('Foo\Bar\Baz\Dib\Zim\Gir\ClassName');
        $expect = '/vendor/foo.bar.baz.dib.zim.gir/src/ClassName.php';
        $this->assertSame($expect, $actual);
    }

    public function testConfusion()
    {
        $actual = $this->loader->loadClass('Foo\Bar\DoomClassName');
        $expect = '/vendor/foo.bar/src/DoomClassName.php';
        $this->assertSame($expect, $actual);

        $actual = $this->loader->loadClass('Foo\BarDoom\ClassName');
        $expect = '/vendor/foo.bardoom/src/ClassName.php';
        $this->assertSame($expect, $actual);
    }
}
