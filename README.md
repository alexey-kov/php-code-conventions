<!--# Translations

 - 🇺🇸 **[English](https://github.com/roistat/php-code-conventions/blob/master/README_en.md)** (work is in progress, pull requests are welcome) -->

# Содержание
  1. [Введение](#Введение)
  0. [Ценности](#Ценности)
  0. [Принципы](#Принципы)
  0. [Общие правила](#Общие-правила)
  0. [Правила разделения бизнес-логики](#Правила-разделения-бизнес-логики)
  0. [Работа с файлами](#Работа-с-файлами)
  0. [Работа с переменными](#Работа-с-переменными)
  0. [Логические переменные и методы](#Логические-переменные-и-методы)
  0. [Работа с массивами](#Работа-с-массивами)
  0. [Работа со строками](#Работа-со-строками)
  0. [Работа с датами](#Работа-с-датами)
  0. [Работа с пространствами имён](#Работа-с-пространствами-имён)
  0. [Работа с методами](#Работа-с-методами)
  0. [Возврат результата работы метода](#Возврат-результата-работы-метода)
  0. [Работа с классами](#Работа-с-классами)
  0. [Работа с объектами](#Работа-с-объектами)
  0. [Комментирование кода](#Комментирование-кода)
  0. [Работа с исключениями](#Работа-с-исключениями)
  0. [Работа с внешним хранилищем данных](#Работа-с-внешним-хранилищем-данных)
  0. [Особенности Pull Request (PR)](#Особенности-pull-request-pr)
  0. [Работа с шаблонами](#Работа-с-шаблонами)
  0. [Работа с литералами](#Работа-с-литералами)
  0. [Работа с условиями](#Работа-с-условиями)
  0. [Работа с тернарными операторами](#Работа-с-тернарными-операторами)
  0. [Про тесты](#Про-тесты)
  0. [Использование chain-объектов](#Использование-chain-объектов)
  0. [Работа со скриптами](#Работа-со-скриптами)
  0. [Авторы](#Авторы)
  
## **1. Введение**

Этот документ содержит правила написания кода в агентстве [Kovalenko & son](https://www.upwork.com/o/companies/_~01193052a86a53985e)
(Code Conventions). Документ разработан на основе аналогичного документа от [компании Roistat](http://roistat.com/ru/vacancy?roistat=github_codeconv). Вы можете взять этот документ как есть или использовать его как основу для вашего собственного Code Conv. 

Здесь всегда находится актуальная версия нашего Code Conv, так как мы ссылаемся на него при проведении наших Code Review.

Code Conv — это правила, которые нужно соблюдать при написании любого кода. Мы различаем Code Style и Code Conv. Для нас Code Style — это внешний вид кода. То есть расстановка отступов, запятых, скобок и прочего. А Code Conv — это смысловое содержание кода. Правильные алгоритмы действий, правильные по смыслу названия переменных и методов, правильная композиция кода. Соблюдение Code Style легко проверяется автоматикой. А вот проверить соблюдение Code Conv в большинстве случаев может только человек.

Обратите внимание: Code Style в примерах может отличаться от Code Style вашего проекта. Придерживаться надо тому Code Style, который принят у вас. Code Conv не об этом. 

**Автоматическая проверка**

В комплекте с СС идет описание [правил](https://github.com/alexey-kov/php-code-conventions/blob/master/phpmd.def.xml) для использования с PHP Mess Detector. 

Внутри файла все правила ссылаются на данный документ. Внутри документа правила имеющие описание в phpmd отмечены символом 📖, а не имеющие такового - 🗞. 

> При использовании этого документа при Code Review будьте особенно внимательны с правилами, помеченными символом 🗞, так как их следует проверять вручную.


## **2. Ценности**
Главная цель Code Conv — сохранение низкой стоимости разработки и поддержки кода на длинной дистанции.

Основные ценности, помогающие достичь этой цели:

**Читаемость**

Код должен легко читаться, а не легко записываться. Это значит, что такие вещи как синтаксический сахар (если он направлен на ускорение записи, а не дальнейшего чтения кода) вредны.

Обратите внимание, что быстродействие кода не является ценностью, поэтому не самый оптимальный цикл, но удобный для понимания, будет лучше, чем быстрый, но сложный. Не нужно экономить переменные, буквы для их названий, оперативную память и так далее.

**Вандалоустойчивость**

Код надо писать так, чтобы у разработчика, который с ним будет работать, было как можно меньше возможности внести ошибку. Например, покрывайте тестами не только краевые условия, но и кейсы, которые могут появиться в результате доработок кода и рефакторинга.

**Поддержание наименьшей энтропии**

Энтропия — это количество информации, из которой состоит проект (информационная емкость проекта). Код проекта должен выполнять продуктовые требования с сохранением наименьшей возможной энтропии. 

## **3. Принципы**

Принципы — это способы соблюдения описанных выше ценностей. Они чуть более детальны, содержат основные методологии разработки и подходы, которыми мы руководствуемся. 

Код должен быть:

- Понятным, явным. Явное лучше, чем неявное. Например, не должны использоваться магические методы, если этого можно избежать. Также нельзя использовать `exit` и любые другие операторы, которые могут завершить или изменить работу процесса. 
- Удобным для использования сейчас
- Удобным для использования в будущем
- Должен стремиться к соблюдению принципов [KISS](https://ru.wikipedia.org/wiki/KISS_(%D0%BF%D1%80%D0%B8%D0%BD%D1%86%D0%B8%D0%BF)), [SOLID](https://ru.wikipedia.org/wiki/SOLID_(%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%BD%D0%BE-%D0%BE%D1%80%D0%B8%D0%B5%D0%BD%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B5_%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5)), [DRY](https://ru.wikipedia.org/wiki/Don%E2%80%99t_repeat_yourself), [GRASP](https://ru.wikipedia.org/wiki/GRASP)
- Код должен обладать низкой связанностью и высокой связностью (подробно это описано в GRASP). Любая часть системы должна иметь изолированную логику и при надобности внешний интерфейс, который позволяет с этой логикой работать. Любая внутренняя часть должна иметь возможность быть измененной без какого-либо ущерба внешним системам
- Код должен быть таким, чтобы его можно было автоматически отрефакторить в IDE (например, Find usages и Rename в PHPStorm). То есть должен быть слинкован типизацией и PHPDoc'ами
- В БД не должны храниться части кода (даже названия классов, переменных и констант), так как это делает невозможным автоматический рефакторинг
- Последовательным. Код должен читаться сверху вниз. Читающий не должен держать что-то в уме, возвращаться назад и интерпретировать код иначе. Например, надо избегать обратных циклов `do {} while ();` 
- Должен иметь минимальную [цикломатическую сложность](https://ru.wikipedia.org/wiki/%D0%A6%D0%B8%D0%BA%D0%BB%D0%BE%D0%BC%D0%B0%D1%82%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B0%D1%8F_%D1%81%D0%BB%D0%BE%D0%B6%D0%BD%D0%BE%D1%81%D1%82%D1%8C)

## **4. Общие правила**

### 🗞 4.1 Запрещен неиспользуемый код
Если код можно убрать, и работа системы от этого не изменится, его быть не должно.

Плохо:
```php
if (false) {
    legacyMethodCall();
}
// ...
$legacyCondition = true;
if ($legacyCondition) {
    finalizeData($data);
}
```

Хорошо:
```php
// ...
finalizeData($data);
```

### 🗞 4.2 Не должны напрямую использоваться функции стандартной библиотеки PHP, если этого можно избежать
Это упростит миграцию кода на новую версию языка. Часто в новой версии языка удаляются какие-либо функции или изменяется их работа. Чем меньше идет завязки на язык и его версию, тем лучше.

Специфичные функции всегда лучше использовать через функции-обёртки внутри проекта. Тогда в случае миграции придется исправлять одно место, а не тысячу.

Как понять, можно ли использовать встроенную в PHP функцию или нет?

- Если эта функция уже используется повсеместно в проекте, значит, её можете использовать и вы. Например, это может быть `explode`/`implode`. Если эти функции будут изменены в новой версии PHP, то в любом случае придется переделать много кода и делать это будет автоматика.

- Если эта функция не используется или используется только через обёртку в специализированном сервисе, то и вы использовать её можете только через обёртку (добавляется при необходимости).

Плохо:
```php
if (ctype_digit($number)) {
    // ...
}
$distance = levenshtein($text1, $text2);
$urlParts = parse_url($url);
```

Хорошо:
```php
if ($number >= 0) {
    // ...
}
$distance = $textService->levenshtein($text1, $text2);
$urlParts = $urlService->parseUrl($url);
```

### 🗞 4.3 Вместо отсутствующего скалярного значения используется null
0 и пустую строку нельзя использовать в качестве показателя отсутствия значения.
```php
/**
 * @param string $title
 * @param string $message
 * @param string $date
 */
function sendEmail($title, $message = null, $date = null) {
    // ...
}

// сообщение не было передано
$object->sendEmail('Title', null, '2017-01-01');

// было передано пустое сообщение
$object->sendEmail('Title', '', '2017-01-01');
```

Однако, это правило не относится к массивам.

Плохо:
```php
function deleteUsersByIds(array $ids = [], $someOption = false) {
    // ...
}

deleteUsersByIds(null, true);
```

Хорошо:
```php
deleteUsersByIds([], true);
```

Итого: использование пустой строки почти всегда является ошибкой.

### 4.4 Одна строка кода должна выполнять одну операцию

Плохо:
```php
$foo = (!empty($first = trim($first)) ? $first : $second );
```

Хорошо:
```php
$firstTrimmed = trim($first);
$foo = (!empty($firstTrimmed) ? $firstTrimmed : $second );
```

Плохо:
```php
if($users = $repository->loadUsers()){
    // ...
}
```

Хорошо:
```php
$users = $repository->loadUsers();
if(!empty($users)){
    // ...
}
```

Исключение - если над переменной происходит ряд последовательных преобразований без изменения типа (особенно если используемые функции принимают только один параметр) или цепочка вызовов:
```php
$foo = base64_encode(trim(nl2br($bar)));
$form->createInput($foo)->createSelect($bar)->setSubmit('action','Submit')->build();
```

**[⬆ наверх](#Содержание)**

## **5. Правила разделения бизнес-логики**

### 🗞 5.1 Сервисы
Сервис – это класс без состояния, содержащий бизнес-логику. Данные для обработки сервис получает либо в виде параметров публичных методов, либо других сервисов. 

Сервис не может использовать в качестве источника данных глобальные переменные или окружение, так как при тестировании их, скорее всего, придется мокать:

Плохо:
```php
class User {

    public function loadUsers() {
        $path = getenv('DATA_PATH'); // так нельзя!
        // ...
    }
}
```

Хорошо:
```php
class Env {
    
    /**
     * @return string
     */
    public function getDataPath(): string {
        return getenv('DATA_PATH');
    }
}

class User {

    /**
     * @var Env
     */
    private $_env;
    
    /**
     * @param Env $env
     */
    public function __construct(Env $env) {
        $this->_env = $env;
    }

    public function loadUsers() {
        $path = $this->_env->getDataPath();
        // ...
    }
}
```
Однако, это правило не работает, если получение данных из внешних источников — единственная бизнес-логика сервиса.

Для работы с хранилищем мы используем репозиторий. Это частный случай сервиса, но он получает данные из БД через адаптеры.

### 🗞 5.2 Контроллеры
Контроллер принимает и обрабатывает запросы. Он получает параметры на вход, запрашивает данные из сервисов и возвращает представление.

### 🗞 5.3 Модели
Модель — простой объект со свойствами, не содержащий никакой другой бизнес-логики, кроме геттеров и сеттеров. Геттер — метод, позволяющий получить какую-то информацию из внутреннего состояния объекта. Это не обязательно поле, как оно есть. Он может брать значения нескольких полей и делать простые манипуляции с ними (не запрашивая внешней продуктовой бизнес-логики). Сеттер — аналогично может изменять внутреннее состояние одного или нескольких полей без запросов «наружу».

Условный упрощенный пример:

```php
class Bill {
    public $id;
    public $sum;
    public $isPaid;
    public $paidDate
    // ...
    
    public function markAsPaid(\DateTime $paymentDate) {
        $this->isPaid = true;
        $this->paidDate = $paymentDate;
    }
}
```

Желательно делать модели неизменяемыми, см. [Работа с объектами](#Работа-с-объектами). Хотите больше гибкости — можно использовать [chain-объекты](#Использование-chain-объектов).

### 🗞 5.4 Представления
Представлением в зависимости от требуемого ответа сервера может быть HTML-шаблон, API-объект или что-то иное. Обратите внимание, API-объект и модель данных это разные сущности, даже если у них совпадает название и все поля. Нельзя просто вернуть в JSON-ответе сервера модель из хранилища:

Плохо:
```php
public function actionUsers(): Response {
    $users = $repository->loadUsers();
    return new Response(['data' => $users]);
}
```

Свойства модели хранилища могут поменяться из-за новых технических требований, но объект API это продукт, вы должны изменять его явно:

Хорошо:
```php
// src/Entity/Api/User.php
namespace Entity\Api;

class User {
    public $id;
    public $name;
}

// src/Controller/Api/User.php
public function actionUsers(): Response {
    $users = $repository->loadUsers();
    $apiUsers = array_map(function ($user) {
        return $this->_convertUserToApiObject($user);
    }, $users);
    return new Response(['data' => $apiUsers);
}

private function _convertUserToApiObject(Entity\Mapper\User $user): Entity\Api\User {
    // ...
}
```

**[⬆ наверх](#Содержание)**

## **6 Работа с файлами**

### 🗞 6.1 Названия файлов пишутся строчными буквами через underscore (нижнее подчеркивание, "`_`")
Кроме случаев, когда внутри файла содержится класс. В таком случае файл должен повторять названия класса, то есть должен быть написан в стиле UpperCamelCase. Аналогично обычные директории и пространства имён.

### 🗞 6.2 Файлы классов должны быть расположены в директориях в соответствии со стандартом [PSR-4](https://www.php-fig.org/psr/psr-4/)

**[⬆ наверх](#Содержание)**

## **7 Работа с переменными**

### 🗞 7.1 Название переменных пишутся через camelCase

### 🗞 7.2 Название переменных должно соответствовать содержанию
Нельзя назвать переменную `$day` и хранить в ней массив статистических данных за этот день.

### 🗞 7.3 длинна переменной должна быть больше 2 символов
Нельзя писать короткие названия, например `$c`. 

### 🗞 7.4 Часто упоминаемые объекты именуются одинаково во всем проекте

Плохо:
```php
$customer = new User();
$client = new User();
$object = new User();
```

Хорошо:
```php
$user = new User();
```

### 🗞 7.5 Признак объекта добавляется к названию
Если это отфильтрованный по какому-то признаку проект, то признак добавляется к названию. Например, `$unpaidProject`.

### 🗞 7.6 Переменные, отражающие свойства объекта, должны включать название объекта

Плохо:
```php
$project = new Project();
$name = $project->name;
$project = $project->name;
```

Хорошо:
```php
$project = new Project();
$projectName = $project->name;
```

### 🗞 7.7 Переменные должны называться на корректном английском

Плохо:
```php
$abonents = [];
$currencys = [];
```

Хорошо:
```php
$subscribers = [];
$currencies = [];
```

### 🗞 7.8 Наименование отфильтрованных переменных должно быть префиксным
Вначале идет название объекта, а после условие фильтрации:

Хорошо:
```php
$storedUsers = [];
```

Плохо:
```php
$usersStored = [];
```


### 🗞 7.9 К переменной нельзя обращаться по ссылке (через &)
Амперсанды могут использоваться только как логические или битовые операторы.

Плохо:
```php
/**
 * @param string &$name
 */
function removePrefix(&$name) {
    // ...
}
```

Хорошо:
```php
/**
 * @param string $name
 * @return string
 */
function removePrefix($name) {
    // ...
    return $result;
}
```

### 🗞 7.10 Переменные и свойства объекта должны являться существительными и называться так, чтобы они правильно читались при использовании, а не при инициализации.

Плохо:
```php
$object->expire_at
$object->setExpireAt($date);
$object->getExpireAt();
```

Хорошо:
```php
$object->expiration_date;
$object->setExpirationDate($date);
$object->getExpirationDate();
```

### 🗞 7.11 В названии переменной не должно быть указания типа
Нельзя писать `$projectsArray`, надо писать просто `$projects`. Это же касается и форматов (JSON, XML и т.п.), и любой другой не относящейся к предметной области информации.

Плохо:
```php
$projectsList = $repository->loadProjects();
$projectsListIds = $utils->extractField('id', $projectsList);
```

Хорошо:
```php
$projects = $repository->loadProjects();
$projectsIds = $utils->extractField('id', $projects);
```

### 🗞 7.12 Нельзя изменять переменные, которые передаются в метод на вход 
Исключение — если эта переменная объект.

Плохо:
```php
function parseText($text) {
    $text = trim($text);
    // ...
}
```

Хорошо:
```php
function parseText($text) {
    $trimmedText = trim($text);
    // ...
}
```

### 🗞 7.13 Каждая переменная должна быть объявлена на новой строке

Плохо:
```php
$foo = false; $bar = true;
```

Хорошо:
```php
$foo = false;
$bar = true;
```

### 🗞 7.14 Нельзя нескольким переменным присваивать одно и то же значение

Плохо:
```php
function loadUsers(array $ids) {
    $usersIds = $ids;
    // ...
}
```

Исключение - если требуется сохранить исходное состояние переменной или склонировать массив для дальнейшего прохода циклом:
```php
$usersOriginal = $users;
foreach($usersOriginal as $key => $value){
    $users[$key] = doSomething($value);
}
```

### 🗞 7.15 Оператор `clone` должен использоваться только в тех случаях, когда без него не обойтись
Также можно использовать `clone`, если без него код серьёзно усложнится, а с ним станет понятным и очевидным. Простой пример — клонирование объектов DateTime. Или использование клонирования для сравнения двух версий объекта: старой и новой. 

Плохо:
```php
function loadAnalyticsData(\DateTime $intervalStart) {
    $intervalEnd = new \DateTime($intervalStart->format('Y-m-d H:i:s'));
    $intervalEnd->modify('+1 day');
}

function updateUser(User $user) {
    $oldUser = new User();
    $oldUser->id = $user->id;
    $oldUser->name = $user->name;
    // ...
    logObjectDiff($user, $oldUser);
}
```

Хорошо:
```php
function loadAnalyticsData(\DateTime $intervalStart) {
    $intervalEnd = clone $intervalStart;
    $intervalEnd->modify('+1 day');
}

function updateUser(User $user) {
    $oldUser = clone $user;
    // ...
    logObjectDiff($user, $oldUser);
}
```

### 🗞 7.16 Запрещено использовать результат операции присваивания

Частный случай для правила [4.4](#4.4-Одна-строка-кода-должна-выполнять-одну-операцию "Одна строка кода должна выполнять одну операцию")

Плохо:
```php
$foo = $bar = strlen($someVar);
```

Хорошо:
```php
$bar = strlen($someVar);
$foo = $bar;
```


Плохо:
```php
$this->_callSomeFunc($bar = strlen($foo));
```

Хорошо:
```php
$bar = strlen($foo);
$this->_callSomeFunc($bar);
```


Плохо:
```php
if (strlen($foo = json_encode($bar)) > 100) {
    // ...
}
```

Хорошо:
```php
$foo = json_encode($bar);
if (strlen($foo) > 100) {
    // ...
}
```

### 🗞 7.17 Нельзя использовать константы через метод `constant`

<!-- @tag:??? -->
Плохо:
```php
/**
 * @return string
 */
public function getProjectDir(): string {
    $prefix = 'ACME_';
    $name = $prefix . 'PROJECT_DIR';
    return constant($name);
}

/**
 * @return string
 */
public function getProjectDir(): string {
    return constant('ACME_PROJECT_DIR');
}
```

Хорошо:
```php
/**
 * @return string
 */
public function getProjectDir(): string {
    return ACME_PROJECT_DIR;
}
```

**[⬆ наверх](#Содержание)**

## **8 Логические переменные и методы**

### 🗞 8.1 Названия boolean методов и переменных должны содержать глагол `is`, `has` или `can`

Переменные правильно называть, описывая их содержимое, а методы — задавая вопрос. Если переменная содержит свойство объекта, следуем правилу [признак объекта добавляется к названию](#-Признак-объекта-добавляется-к-названию).

Плохо:
```php
$isUserValid = $user->valid();
$isProjectAnalytics = $accessManager->getProjectAccess($project, 'analytics');
```

Хорошо:
```php
$userIsValid = $user->isValid();
$projectCanAccessAnalytics = $accessManager->canProjectAccess($project, 'analytics');
```

### 8.2 Геттеры именуются аналогично переменным:

```php
class User {
    private $_billingIsPaid;
    private $_isEnabled;

    public function isEnabled() {
        return $this->_isEnabled;
    }

    public function billingIsPaid() {
        return $this->_billingIsPaid;
    }
}
```

Такое именование позволяет легче читать условия:

```php
// if user is valid, then do something
if ($userIsValid) {
    // do something
}
```

### 🗞 8.3 Запрещены отрицательные логические названия

Плохо:
```php
if ($project->isInvalid()) {
    // ...
}
if ($project->isNotValid()) {
    // ...
}
if ($accessManager->isAccessDenied()) {
    // ...
}
```

Хорошо:
```php
if (!$project->isValid()) {
    // ...
}
if (!$accessManager->isAccessAllowed()) {
    // ...
}
if ($accessManager->canAccess()) {
    // ...
}
```

### 🗞 8.4 Не используйте boolean переменные (флаги) как параметры функции
Флаг в качестве параметра это признак того, что функция делает больше одной вещи, нарушая [принцип единственной ответственности (Single Responsibility Principle или SRP)](https://ru.wikipedia.org/wiki/%D0%9F%D1%80%D0%B8%D0%BD%D1%86%D0%B8%D0%BF_%D0%B5%D0%B4%D0%B8%D0%BD%D1%81%D1%82%D0%B2%D0%B5%D0%BD%D0%BD%D0%BE%D0%B9_%D0%BE%D1%82%D0%B2%D0%B5%D1%82%D1%81%D1%82%D0%B2%D0%B5%D0%BD%D0%BD%D0%BE%D1%81%D1%82%D0%B8). Избавляйтесь от них, выделяя код внтури логических блоков в отдельные ветви выполнения.

Плохо:
```php
function someMethod() {
    // ...
    $projectNotificationIsEnabled = $notificationManager->isProjectNotificationEnabled($project);
    storeUser($user, $projectNotificationIsEnabled);
}

function storeUser(User $user, $isNotificationEnabled) {
    // ...
    if ($isNotificationEnabled) {
        notify('new user');
    }
}
```

Хорошо:
```php
function someMethod() {
    // ...
    storeUser($user);
    if ($notificationManager->isProjectNotificationEnabled($project)) {
        notify('new user');
    }
}

function storeUser(User $user) {
    // ...
}
```

**[⬆ наверх](#Содержание)**

## **9 Работа с массивами**

### 🗞 9.1 Для конкатенации массивов запрещено использовать оператор `+`.
Обратите внимание, что `array_merge` все числовые ключи приводит к `int`, даже если они записаны строкой.

Плохо:
```php
return $initialData + $loadedData;
```

Хорошо:
```php
namespace Service;

class ArrayUtils {

    /**
     * @param array $array1
     * @param array $array2
     * @return array
     */
    public function mergeArrays(array $array1, array $array2): array {
        return array_merge($array1, $array2);
    }
}

class SomeClass {
    public function someMethod() {
        return $this->_arrayUtils->mergeArrays($initialData, $loadedData);
    }
}
```

### 🗞 9.2 Для проверки наличия ключа в ассоциативном массиве используем `array_key_exists`, а не `isset`
`isset` проверяет не ключ на его наличие, а значение этого ключа, если он есть. Это разные методы с разным поведением и назначением. Если вы хотите проверить значение ключа, то делайте это явно. Сначала явно проверьте наличие ключа через `array_key_exists` и обработайте ситуацию его отсутствия, затем приступайте к работе со значением.

Плохо:
```php
function getProjectKey(array $requestData) {
    return isset($requestData['project_key']) ? $requestData['project_key'] : null;
}
```

Хорошо:
```php
function getProjectKey(array $requestData) {
    if (!array_key_exists('project_key', $requestData)) {
        return null;
    }
    return $requestData['project_key'];
}
```

### 🗞 9.3 Ассоциативный массив мы используем как hashmap
То есть не применяем разные встроенные в PHP инструменты. Приведем несколько очевидных примеров (однако, правило ими не исчерпывается):

#### 🗞 9.3.1 Нельзя сортировать ассоциативные массивы

Плохо:
```php
$arr = [
    'project_key' => 'foo',
    'key' => 'bar',
    'user_id' => 300,
];

uasort($arr);
```

#### 🗞 9.3.2 Нельзя смешивать в массиве строковые и числовые ключи

Плохо:
```php
$arr = [
    'project_key' => 'foo',
    'key' => 'bar',
    'user_id' => 300,
    1 => 'value1',
    2 => 'value2',
];

$arr[3] = 'value3';
```

#### 🗞 9.3.3 Для проверки наличия значения по индексу в числовых массивах используем `count($array) > N`

Плохо:
```php
if (array_key_exists(1, $users)) {
    // ...
}
if (isset($users[1])) {
    // ...
}
```

Хорошо:
```php
if (count($users) > 1) {
   // ... 
}
```

**[⬆ наверх](#Содержание)**

## **10 Работа со строками**

### 🗞 10.1 Строки обрамляются одинарными кавычками
Двойные кавычки используются только, если внутри строки используются спец. символы `\n`, `\r`, `\t` и т.д.

Плохо:
```php
$string = "Some string";
$string = "Some 'string'";
```

Хорошо:
```php
$string = 'Some string';
$string = "\tSome string\n";
$string = 'Some \'string\'';
$string = "\t".'Some string'."\n";
```

**[⬆ наверх](#Содержание)**

## **11 Работа с датами**

### 🗞 11.1 Дата всегда должна быть представлена DateTime, интервал как DateInterval

Плохо:
```php
$date = $request->get('date');
$interval = 86400*30;
loadSomeData($date, $interval);
```

Хорошо:
```php
$date = $this->_dateService->instance($request->get('date'));
$interval = new \DateInterval('P30D');
loadSomeData($date, $interval);
```

### 🗞 11.2 Запрещено создавать объект даты при помощи `new \DateTime()`
<!-- @tag: ??? -->
В проекте для этого должен быть фабричный метод в сервисе для работы с датами.

Плохо:
```php
$date = new \DateTime();
```

Хорошо:
```php
$date = $this->_dateService->instance();
```

### 🗞 11.3 Если дата должна быть представлена скалярным значением, необходимо использовать строку

* строка с датой и временем должна быть везде в одинаковом формате
* формат не должен включать временную зону, если для этого нет особых требований
* при прочих равных в дате без часового пояса всегда подразумевается время сервера
* если строку по какой-то причине невозможно использовать, используем `int`
* если нет особых требований, то строчное представление даты должно быть в формате `YYYY-MM-DD HH:mm:ss`

Плохо:
```php
class User {
    public $creation_time;
}

$user->creation_time = time();
```

Хорошо:
```php
class User {
    /**
     * @type string
     */
    public $creation_date;
}

$user->creation_date = '2018-01-18 12:54:11';
```

### 🗞 11.4 При работе с интервалами/периодами нельзя указывать месяц или год если это не является явным требованием

В зависимости от текущей даты месяц и год могут принимать разные временные промежутки (високосный и обычный год, разное количество дней в месяце). Вместо этого в качестве указания интервала используем дни, часы, минуты, секунды.

Плохо:
```php
$dateTime = new \DateTime('-2 month');
$dateInterval = new \DateInterval('P2M');
```

Хорошо:
```php
$dateTime = new $this->_dateTime->instance('-60 days');
$dateInterval = new \DateInterval('P60D');
```

Месяц или год необходимо использовать, если это напрямую указано в требованиях задачи как календарный месяц или календарный год.

**[⬆ наверх](#Содержание)**

## **12 Работа с пространствами имён**

### 🗞 12.1 Все пространства имён должны быть подключены через `use` в начале файла. В самом коде не должно быть обратного слеша перед названием пространства имён, кроме случаев использования глобального пространства (например `\Exception`)

Плохо:
```php
$object = new \Some\Object();
```

Хорошо:
```php
use Some;
$object = new Some\Object();
```

### 🗞 12.2 В свою очередь обычные классы без пространства имён не должны быть подключены через `use`

Плохо:
```php
use TimeZone;
$date = new TimeZone('Europe\Moscow');
```

Хорошо:
```php
$date = new \TimeZone('Europe\Moscow');
```

### 🗞 12.3 Нельзя подключать несколько классов из одного пространства имён через `use`
<!-- @tag: delete ??? -->
Плохо:
```php
use Entity\User;
use Entity\Project;
 
$user = new User();
$project = new Project();
```

Хорошо:
```php
use Entity;
  
$user = new Entity\User();
$project = new Entity\Project();
```

### 🗞 12.4 Следует избегать использования пседонима (alias)
<!-- @taag: delete? -->
Они запутывают код и его понимание. Если у вас совпадают названия пространств имён, то, скорее всего, вы делаете что-то не так. Допустимо использовать псевдоним, если другое решение будет слишком сложным.

Плохо:
```php
use Component\User;
use Entity\User as UserEntity;

$user = new UserEntity();
```

Хорошо:
```php
use Component\User;
use Entity;

$user = new Entity\User();
```

**[⬆ наверх](#Содержание)**

## **13 Работа с методами**

### 🗞 13.1 Должна быть использована максимально возможная типизация для вашей версии PHP. Все параметры и их типы должны быть описаны в PHPDoc. Возвращаемое значение тоже.

Плохо:
```php
/**
 * @param $id
 * @param $name
 * @param $tags
 * @return mixed
 */
function storeUser($id, $name, $tags = []) {
    // ...
}
```

Хорошо:
```php
// для PHP 7.1
/**
 * @param int $id
 * @param string $name
 * @param string[] $tags
 * @return User|null
 */
function storeUser(int $id, string $name, array $tags = []): ?User {
    // ...
}

// для PHP 5.6
// без строгой типизации возвращаемых типов любой метод
// может вернуть null, так что можно его не указывать в PHPDoc
/**
 * @param int $id
 * @param string $name
 * @param string[] $tags
 * @return User
 */
function storeUser($id, $name, array $tags = []) {
    // ...
}
```

### 🗞 13.2 Все возможные типы должны быть определены в PHPDoc
Наибольшую пользу это приносит при работе с массивами:

Плохо:
```php
/**
 * @param array $users
 * @param mixed $project
 * @param int $timestamp
 * @return mixed
 */ 
public function someMethod($users, $project, $timestmap) {
    foreach ($users as $user) {
        // IDE не сможет определить тип $user
    }
    // ...
}
```

Хорошо:
```php
/**
 * @param Users[] $users
 * @param Project $project
 * @param int $timestamp
 * @return Foo
 */
public function someMethod(array $users, Project $project, int $timestmap): Foo {
    foreach ($users as $user) {
        // подсказки IDE и рефакторинг работают корректно
    }
    // ...
}
```

### 🗞 13.3 В PHPDoc в возвращаемом значении не надо указывать `void` и `null`, если метод ничего не возвращает.

Плохо:
```php
/**
 * @param string $controllerName
 * @return void
 */
public function runApplication(string $controllerName) {
    // ...
}

/**
 * @return null
 */
public function run() {
    // ...
}
```

Хорошо:
```php
/**
 * @param string $controllerName
 */
public function runApplication(string $controllerName) {
    // ...
}

public function run() {
    // ...
}
```

### 🗞 13.4 Название метода должно начинаться с глагола и соответствовать правилам именования переменных.

Плохо:
```php
public function items() {
    // ...
}
public function convertedDataObject(array $data) {
    // ...
}
```

Хорошо:
```php
public function loadItems() {
    // ...
}
public function convertDataToObject(array $data) {
    // ...
}
```

### 🗞 13.5 Нельзя использовать глагол get в геттерах
Например, вместо `getDate()` следует писать `date()`. Геттер — метод, работающий только с полями своего объекта.

Плохо:
```php
class User {
    private $_date;
    private $_customFields;

    public function getDate() {
        return $this->_date;
    }

    public function getCustomFields() {
        return json_decode($this->_customFields);
    }
}
```

Хорошо:
```php
class User {
    private $_date;
    private $_customFields;

    public function date() {
        return $this->_date;
    }

    public function decodedCustomFields() {
        return json_decode($this->_customFields);
    }
}
```

### 🗞 13.6 Методы названия, которых начинаются c `check` и `validate`, должны выбрасывать исключения и не возвращать значения

Плохо:
```php
public function validateRequestData(array $requestData) {
    if (!array_key_exists('key', $requestData)) {
        return false;
    }
    // ...
    return true;
}
```

Хорошо:
```php
public function validateRequestData(array $requestData) {
    if (!array_key_exists('key', $requestData)) {
        throw new ValidationError('Field "key" not found');
    }
    // ...
}
```

### 🗞 13.7 Все методы класса по умолчанию должны быть private
Если метод используется наследниками класса, то он объявляется `protected`. Если используется сторонними классами, тогда `public`.

### 🗞 13.8 Использование рекурсий допускается только в исключительном случае
Если код без рекурсии будет очень сложен для написания и понимания и при этом рекурсия гарантированно не выйдет за ограничения стека вызовов.

### 🗞 13.9 Запрещается кешировать данные в статических переменных метода
Для кеширование в памяти используем свойство объекта.

Плохо:
```php
public function loadData() {
    static $_cachedData;
    if ($_cachedData === null) {
        $_cachedData = [];
    }
    return $_cachedData;
}
```

Хорошо:
```php
private $_cachedData = [];

public function loadData() {
    if ($this->_cachedData === null) {
        $this->_cachedData = [];
    }
    return $this->_cachedData;
}
```

### 🗞 13.10 Параметры в методах должны следовать в следующем порядке: обязательные → часто используемые → редко используемые
Нужно соблюдать читаемость при написании вызова.

Плохо:
```php
public function method($required, $practicallyUnused = 5, $often = [], $lessOften = null)
public function filter($value, $name, $operator) // ...$service->filter(15, "id", "=")
```

Хорошо:
```php
public function method($required, $often = [], $lessOften = null, $practicallyUnused = 5)
public function filter($name, $operator, $value) // ...$service->filter("id", "=", 15)
```

### 🗞 13.11 Если переменные, объявленные на вход к методу, могут быть `null`, они должны явно обозначаться как таковые

Хорошо:
```php
/**
 * @param string $projectName
 */
public function someMethod($projectName = null) {
    // ...
}
```

**[⬆ наверх](#Содержание)**

## **14 Возврат результата работы метода**

### 🗞 14.1 Метод всегда должен возвращать только одну структуру данных (или `null`) или ничего не возвращать
Метод не может в разных ситуациях возвращать разные типы данных.

Плохо:
```php
function loadUser() {
    if ($someCondition) {
        return ['id' => 1];
    }
    return new User();
}
```

Хорошо:
```php
function loadUser() {
    if ($someCondition) {
        $user = new User();
        $user->id = 1;
        return $user;
    }
    return new User();
}
```

### 🗞 14.2 Если метод возвращает один объект (или скалярный тип), то в случае, если объект не найден, возвращается `null`
Если же метод возвращает список объектов, то в случае, когда список пуст, возвращает пустой массив. Нельзя возвращать вместо пустого списка `null`.

Плохо:
```php
function loadUsers() {
    if ($someCondition) {
        return null;
    }
    return [new User()];
}
```

Хорошо:
```php
function loadUsers() {
    if ($someCondition) {
        return [];
    }
    return [new User()];
}
```

Однако, бывают ситуации, когда надо явно указать, что данные отсутвуют, а не содержат пустой список.

Пример: значения полей объекта задаются пользователем. Возможны две ситуации:
- пользователь не знает, каким категориям принадлежит объект — `null`
- пользователь знает, что объект не принадлежит ни одной категории — пустой массив (`[]`)

Тогда для получения категорий объекта будет правильным такой код:

```php
/**
 * для PHP 5.6
 * @return array|null
 */
function getObjectCategories($object) {
    if ($object->categories === null) {
        return null;
    }
    return parseCategories($object->categories);
}

/**
 * для PHP 7.1
 * @return array|null
 */
function getObjectCategories($object): ?array {
    if ($object->categories === null) {
        return null;
    }
    return parseCategories($object->categories);
}
```

### 🗞 14.3 Возвращаемая переменная обычно `$result`
Если у вас метод `loadUsers`, то не надо внутри метода возвращаемую переменную называть `$users`. В любом месте в методе должно быть понятно, где вы оперируете результатом, а где локальными переменными.

Плохо:
```php
function loadUsers() {
    $users = [];
    // ...
    foreach ($data as $item) {
        $users[] = new User();
    }
    return $users;
}
```

Хорошо:
```php
function loadUsers() {
    $result = [];
    // ...
    foreach ($data as $item) {
        $result[] = new User();
    }
    return $result;
}
```

### 🗞 14.4 Метод должен явно отличать нормальные ситуации от исключительных
Если никакой ошибки не произошло, но отсутствует результат, то это `null` (или пустой массив), однако если все же произошла исключительная ситуация, которая не заложена системой, то должно кидаться исключение.

Плохо:
```php
function loadUsers() {
    if ($connectionError !== null) {
        return []; // потеряли ошибку, никто не узнает о проблемах с подключением
    }
    // ...
    if (count($data) === 0) {
        return [];
    }
    // ...
    return $result;
}
```

Хорошо:
```php
function loadUsers() {
    if ($connectionError !== null) {
        throw new Exception\ConnectionError();
    }
    // ...
    if (count($data) === 0) {
        return [];
    }
    // ...
    return $result;
}
```

### 🗞 14.5 Метод должен придерживаться следующей структуры: Проверка параметров → Получение данных → Работа → Результат
Во время проверки параметров и получения необходимых данных метод должен возвращать соответствующее пустое значение или кидать исключение. После того как метод получил все необходимые данные и приступил к работе выход из метода крайне нежелателен. Возможны редкие исключения, облегчающие понимание и читаемость кода.

Плохо:
```php
/**
 * @return int
 */
public function someMethod() {
    $isValid = $this->_someCheck();
    if ($isValid) {
        $tmp = 0;
        $someValue = $this->_getSomeValue();
        if ($someValue > 0) {
            $tmp = $someValue;
        }
        $anotherValue = $this->_getAnotherValue();
        if ($anotherValue > 0) {
            return $tmp + $anotherValue;
        } else {
            return $someValue;
        }
    } else {
        throw new \Exception('Invalid condition');
    }
}
```

Хорошо:
```php
/**
 * @return int
 * @throws \Exception
 */
public function someMethod() {
    $result = 0;
     
    $isValid = $this->_someCheck();
    if (!$isValid) {
        throw new \Exception('Invalid condition');
    }
  
    $someValue = $this->_getSomeValue();
    if ($someValue > 0) {
        $result += $someValue;
    }
    $anotherValue = $this->_getAnotherValue();
    if ($anotherValue > 0) {
        $result += $anotherValue;
    }
    return $result;
}
```

**[⬆ наверх](#Содержание)**

## **15 Работа с классами**

### 🗞 15.1 Трейты имеют постфикс Trait

Хорошо:
```php
trait AjaxResponseTrait {
    // ...
}
```

### 🗞 15.2 Интерфейсы имеют постфикс Interface

Хорошо:
```php
interface ApplicationInterface {
    // ...
}
```

### 🗞 15.3 Абстрактные классы имеют префикс Abstract

Хорошо:
```php
abstract class AbstractApplication {
    // ...
}
```

### 🗞 15.4 Все свойства класса по умолчанию должны быть private
Если свойство используется наследниками класса, то оно объявляется `protected`. Если используется сторонними классами, тогда `public`.

Плохо:
```php
abstract class Loader {
    public $data = [];

    public function getData() {
        return $this->data;
    }

    public function init() {
        $this->data = $this->load();
    }

    abstract public function load();
}
```

Хорошо:
```php
abstract class Loader {
    
    /**
     * @type array
     */
    private $_cachedData = [];

    /**
     * @return array
     */
    public function getData(): array {
        return $this->_cachedData;
    }

    public function init(): void {
        $this->_cachedData = $this->_load();
    }

    /**
     * @return array
     */
    abstract protected function _load(): array;
}
```

### 🗞 15.5 Методы и свойства в классе должны быть отсортированы по уровням видимости и по порядку использования сверху вниз
Уровни видимости: `public` -> `protected` -> `private`.

Плохо:
```php
class SomeClass {
    private $_privPropA;   
    public $pubPropA;
    protected $_protPropA;
 
    protected function _protA() {
    }
 
 
    public function pubB() {
    }
 
 
    private function _privA() {
        return $this->_protA();
    }
  
    public function pubA() {
        $this->_privA();
        return $this->pubB();
    }
}
```

Хорошо:
```php
class SomeClass {
    public $pubPropA;
    protected $_protPropA;
    private $_privPropA;
 
    public function pubA() {
        $this->_privA();
        return $this->pubB();
    }
 
    public function pubB() {
    }
 
    protected function _protA() {
    }
 
    private function _privA() {
        return $this->_protA();
    }
}
```

**[⬆ наверх](#Содержание)**

## **16 Работа с объектами**

### 🗞 16.1 Все объекты должны быть неизменяемыми (immutable), если от них не требуется обратного

Плохо:
```php
class SomeObject {
    /**
     * @var int
     */
    public $id;
}
```

Хорошо:
```php
class SomeObject {
    /**
     * @var int
     */ 
    private $_id;
  
    /**
     * @param int $id
     */
    public function __construct($id) {
        $this->_id = $id;
    }
  
    /**
     * @var int
     */
    public function id() {
        return $this->_id;
    }
}
```

### 🗞 16.2 Статические вызовы можно делать только у самого класса. У экземпляра можно обращаться только к его свойствам и методам

Плохо:
```php
$type = $user::TYPE;
```

Хорошо:
```php
$type = User::TYPE;
```

**[⬆ наверх](#Содержание)**

## **17 Комментирование кода**
### 🗞 17.1 В общем случае комментарии запрещены

Желание добавить комментарий — признак плохо читаемого кода. Любой участок кода, который вы хотели бы выделить или прокомментировать, надо выносить в отдельный метод.

Фразу, которую вы хотели написать в комментарии, надо привести в простой вид и сделать ее названием метода.

Плохо:
```php
public function deleteApprovedUsers() {
    // load users filter them by approval
    $users = $repository->loadUsers();
    array_filter($users, function($user) {
        return $user->is_approved;
    });

    foreach ($users as $user) {
        // ...
    }
}
```

Хорошо:
```php
public function deleteApprovedUsers() {
    $users = $this->loadApprovedUsers();
    foreach ($users as $user) {
        // ...
    }
}

public function loadApprovedUsers() {
    $users = $repository->loadUsers();
    array_filter($users, function($user) {
        return $user->is_approved;
    });
}
```

### 🗞 17.2 Вынужденные хаки должны быть помечены комментариями
Лучше соблюдать одинаковый формат в рамках проекта

Хорошо:
```php
function loadUsers() {
    $result = $repository->loadUsers();
    // hack: status field was removed from storage 
    foreach ($result as $user) {
        $user->status = 'active';
    }
    // hack end
    return $result;
}
```

### 🗞 17.3 Готовые алгоритмы, взятые из внешнего источника, должны быть помечены ссылкой на источник

Хорошо:
```php
/**
 * https://en.wikipedia.org/wiki/Quicksort
 */
function quickSort(array $arr) {
    // ...
}

/**
 * https://habrahabr.ru/post/320140/
 */
function generateRandomMaze() {
    // ...
}
```

### 🗞 17.4 При разработке прототипа допустимо помечать участки кода @todo

Хорошо:
```php
function loadUsers() {
    $result = $repository->loadUsers();
    // @todo: delete the hack when field will be restored
    // hack: status field was removed from storage
    foreach ($result as $user) {
        $user->status = 'active';
    }
    // hack end
    return $result;
}
```

**[⬆ наверх](#Содержание)**

## **18 Работа с исключениями**
### 🗞 18.1 На каждом уровне бизнес-логики (проект, компонент, библиотека) должно быть абстрактное базовое исключение

### 🗞 18.2 Исключения сторонних библиотек должны быть перехвачены сразу
Далее либо обработаны, либо на их основании должно бросаться свое исключение. Новое исключение должно содержать предыдущее.

Хорошо:
```php
namespace Service\Facebook;

use Exception;
use FacebookAds;

public function function requestData() {
    // ...
    try {
        $objects = $facebookAds->requestData($params);
    } catch (FacebookAds\Exception\Exception $e) {
        throw new Exception\ExternalServiceError("Facebook error", 0, $e);
    }
    //..
}
```

### 🗞 18.3 По умолчанию тексты исключений не должны показываться пользователю
Они предназначены для логирования и отладки. Текст исключения можно показать пользователю, если оно явно для этого предназначено: например, реализует интерфейс `HumanReadableInterface`.

```php
interface HumanReadableInterface {
    
    /**
     * @return string
     */
    public function getUserMessage(): string;
}

public function handleException(\Throwable $exception) {
    if ($exception instanceof HumanReadableInterface) {
        echo $exception->getUserMessage();
        return;
    }
    // ...
}
```

**[⬆ наверх](#Содержание)**

## **19 Работа с внешним хранилищем данных**

### 🗞 19.1 Нельзя делать запросы к внешнему хранилищу внутри цикла с заведомо большим кол-вом итераций

Плохо:
```php
$users = loadUsers();
foreach ($users as $user) {
    $userProjects = loadUserProjects($user);
    // ...
}
```

Хорошо:
```php
$users = loadUsers();
$projects = loadProjects();
$indexedProjects = [];
foreach ($projects as $project) {
    if (!array_key_exists($project->user_id, $indexedProjects)) {
        $indexedProjects[$project->user_id] = [];
    }
    $indexedProjects[$project->user_id][] = $project;
}
foreach ($users as $user) {
    if (!array_key_exists($user->id, $indexedProjects)) {
        continue;
    }
    $userProjects = $indexedProjects[$user->id];
}
```

### 🗞 19.2 Для каждой записи в хранилище должно быть понятна дата ее создания
То есть должна быть колонка `date/creation_date`. Или должен быть зависимый объект (связь 1 к 1), у которого есть такая колонка. Редактируемые записи должны иметь и дату редактирования: `update_date` или `modification_date`.

**[⬆ наверх](#Содержание)**

## **20 Особенности Pull Request (PR)**
### 🗞 20.1 PR должен содержать как можно меньше строк кода
Любая атомарная часть кода должна выделяться в отдельную подзадачу и отдельный PR.

### 🗞 20.2 Нельзя смешивать перенос методов в другие классы и места и последующий рефакторинг между собой
Перенос методов в другие классы и места должны быть выделены в отдельный PR. Последующий рефакторинг после переноса тоже должен быть в отдельном PR.

### 🗞 20.3 В случае большого PR — ответственность за долгий просмотр несет сам разработчик, сделавший такой PR
Нормальный объем кода — 0-300 строк в зависимости от его сложности. PR заглушек и архитектуры может содержать много формального кода, который легко быстро проверить. PR же конкретного метода может содержать много сложностей даже в 10 строчках. 

### 🗞 20.4 Нельзя накапливать изменения в какой-то своей ветке и потом делать большой PR в master
Все что можно смержить в master без последствий (даже если это еще не готовый результат, а только заглушки или часть, но они скрыты от юзеров и никому не мешают), должен мержиться в master и PR должен создаваться в master.

### 🗞 20.5 В Pull Request не должно попадать кода, не относящегося к задаче
Также не должно быть забытых комментариев, бессмысленных переносов строк и прочего "строительного мусора". Каждое изменение, которое вы предлагаете сделать в master-ветке, должно так или иначе относиться к решению поставленной вам задачи.

**[⬆ наверх](#Содержание)**

## **21 Работа с шаблонами**

### 🗞 21.1 В шаблонах не должны вызываться методы объектов (геттеры не в счет)
Все необходимые данные должны быть загружены до рендера и переданы в виде параметров шаблона.

**[⬆ наверх](#Содержание)**

## **22 Работа с литералами**

### 🗞 22.1 Назначение всех числовых литералов должно быть понятным из контекста
Они должны быть или вынесены в переменную или константу, или сравниваться с переменной, или передаваться на вход методу с понятной сигнатурой. В коде должен присутствовать в явном виде ответ: `за что отвечает это число и почему оно именно такое?`

Плохо:
```php
$isOnlyDeleted = 1;
if ($object->is_deleted === $isOnlyDeleted) {
    // ...
}
 
for ($i = 0; $i < 5; $i++) {
    // ...
}
```

Хорошо:
```php

if ($object->is_deleted === 1) {
    // ...
}
 
$apiMaxRetryLimit = 5;
for ($i = 0; $i < $apiMaxRetryLimit; $i++) {
    // ...
} 
```

**[⬆ наверх](#Содержание)**

## **23 Работа с условиями**

### 🗞 23.1 В условном операторе должно проверяться исключительно `boolean` значение

Плохо:
```php
if (count($userProjects)) {
    // ...
}
if ($project) {
    // ...
}
```

Хорошо:
```php
if ($isResponseError) { // $isResponseError = true
    // ...
}
if ($response->isError()) { // isError method returns boolean
    // ...
}
if (count($userProjects) > 0) {
    // ...
}
```

### 🗞 23.2 В сравнении не boolean переменных используется строгое сравнение с приведением типа (===), автоматическое приведение и нестрогое сравнение не используются

Плохо:
```php
if ($project) {
    // ...
}
if ($request->postData('sum') == 100) {
    // ...
}
if (!$request->postData('sum')) {
    // ...
}
if (!$bill->comment) {
    // ...
}
```

Хорошо:
```php
if ($project === null) { // $project is an object
    // ...
}
if ((int)$request->postData('sum') === 100) {
    // ...
}
if ($bill->comment === '') {
    // ...
}
```

### 🗞 23.3 Автоматическое приведение типов разрешено только, когда один из операндов — литерал с зфиксированным типом
При сравнении двух переменных с неизвестными типами для читающего код человека не очевидно, к чему они будут приведены интерпретатором. Если же тип одного из операндов известен, то всё становится очевидно и ручное приведение типов не требуется.

Если вы хотите проверить значение `boolean` пришедшее извне, то делается это так:

Плохо:
```php
if ((int)$request->get('is_something') > 0) {
    // ...
}
if ((int)$request->get('is_something') === 1) {
    // ...
}
if ((int)$user->is_registered === 0) {
    // ...
}
```

Хорошо:
```php
if ($request->get('is_something') > 0) {
    // ...
}
if ($user->is_registered) {
    // ...
}
if (!$user->is_registered) {
    // ...
}
```

#### 🗞 23.4 Не надо сравнивать `boolean` с `true`/`false`
Это нарушает запрет на бесполезный код.

Плохо:
```php
if ($bill->isPaid() == true) {
    // ...
}
if ($bill->isPaid() !== false) {
    // ...
}
if (!$bill->isPaid() === true) {
    // ...
}
if (!(!$bill->isPaid() === true)) {
    // ...
}
if ((bool)$phone->is_external === true) {
    // ...
}
```

Хорошо:
```php
if ($bill->isPaid()) {
    // ...
}
```

### 🗞 23.5 Проверять переменные надо на наличие позитивного вхождения, а не отсутствие негативного
Если вам нужна строка, то проверять надо на то, что переменная является строкой. Не надо проверять на то, что она не является числом или чем-то еще. Перечислять все возможные варианты, чем переменная не должна быть, значит повышать риск ошибки и усложнять поддержку кода.

Плохо:
```php
if (!is_numeric($value) && !is_object($value)) {
    // ...
}
```

Хорошо:
```php
if (is_string($value) && $value !== '') {
    // ...
}
```

### 🗞 23.6 Если вы используете встроенную функцию PHP, которая возвращает `0`, `1` и, возможно, `false`, то при возможности результат ее работы используем в условии как `bool` без дополнительных сравнений
Это не касается случая, когда вам нужно отделить два разных результата между собой, например отдельно отработать `0` и `false`.

Плохо:
```php
if (preg_match($pattern, $subject) === 1) {
    // ...
}
if (!strpos($search, $text)) {
    // ...
}
```

Хорошо:
```php
if (preg_match($pattern, $subject)) { 
    // handle success
}
if (!preg_match($pattern, $subject)) {
    // handle not success
}
if (preg_match($pattern, $subject) === false) {
    // handle error
}
if (strpos($search, $text) === false) {
    // handle not success
}
```

### 🗞 23.7 При использовании в условном выражении одновременно операторов И и ИЛИ обязательно выделять приоритет скобками
Обратите внимание на различие в значении двух вариантов правильного использования

Плохо:
```php
if ($isMobile || $isSizeTooBig && $isAllowedToShrink) {
    // ...
}
```

Хорошо:
```php
if (($isMobile || $isSizeTooBig) && $isAllowedToShrink) {
    // ...
}
if ($isMobile || ($isSizeTooBig && $isAllowedToShrink)) {
    // ...
}
```


**[⬆ наверх](#Содержание)**

## **24 Работа с тернарными операторами**

### 🗞 24.1 При использовании тернарных операторов действуют те же правила, что и при использовании условий

### 🗞 24.2 Тернарный оператор следует использовать, если обе ветви условия предназначены для установки одной переменной одним языковым выражением
При наличии логики в ветках условия следует рассмотреть возможность вынести ее в отдельный метод.

Плохо:
```php
if ($isExternal) {
    $bill = $this->loadExternalBill();
} else {
    $bill = $this->loadInternalBill();
}
```

Хорошо:
```php
$bill = $isExternal ? $this->loadExternalBill() : $this->loadInternalBill();
```

### 🗞 24.5 Использовать цепочки из тернарных операторов `?:` допустимо только при указании значения по умолчанию
<!-- @tag: delete? -->
<!-- @reason: php7.2 allows `??` -->
Плохо:
```php
$contact = $this->loadContactByPhone() ?: $this->loadContactByEmail() ?: $this->loadContactByName();
```

Хорошо:
```php
$lead = $this->loadLeadFromCache() ?: $this->loadLeadFromDB();
$contact = $this->loadContactByPhone() ?: $this->loadContactByEmail() ?: $this->loadContactByName() ?: null;
```

**[⬆ наверх](#Содержание)**

## **25 Про тесты**

### 🗞 25.1 Тесты являются таким же production-кодом, как и любой другой код
Они должны быть написаны с соблюдением соглашений, описанных в этом документе.

### 🗞 25.2 В дата провайдерах для тестов надо писать комментарий к структуре отдаваемого массива значений

Плохо:
```php
public function isEmailAddressData() {
    return [
        ['test@test.ru',            true ],
        // ...
    ]
}
```

Хорошо:
```php
public function isEmailAddressData() {
    return [
        //    email               isValid
        ['test@test.ru',            true ],
        ['invalidEmail',            false],
        // ...
    ]
}
```

**[⬆ наверх](#Содержание)**

## **26 Использование chain-объектов**
### 🗞 26.1 Метод с большим количеством необязательных параметров (А) может быть заменен chain-объектом
Метод с большим количеством необязательных параметров (А) может быть заменен chain-объектом.
В объекте конструктор принимает все обязательные параметры, а все необязательные реализуются сеттерами без глагола set (только существительное), возвращающими текущий объект (chaining методов). Метод-глагол у объекта один без параметров, он завершает использование объекта и выполняет действие, которое должен был выполнить метод А.

**Был метод:**
```php
function send($method, $url, $body = null, $headers = null, $retries = 1, $timeout = 300) {}
```

**Должен замениться на chain-объект:**
```php
public function __construct($method, $url) {
    // ...
}

public function body($body) {
    return $this;
}
// остальные методы с необязательными параметрами

public function send();
```

**Новый объект используется так:**
```php
new $sender($method, $url)->body($body)->retries(10)->timeout(25)->send();
```

**[⬆ наверх](#Содержание)**

## **27 Работа со скриптами**
### 🗞 27.1 Любой скрипт, который изменяет данные, должен иметь подтверждение перед выполнением действий с данными и `debug` по результатам работы

Плохо:
``` php
// cli/delete_items.php
$repository->deleteItems();
```

Исправим, чтобы случайный запуск не удалил элементы:

Хорошо:
```php
// cli/delete_items.php
$totalItems = $repository->countItems();
if (!confirm("Do you want to delete {$totalItems} item(s)?")) {
    echo 'Delete canceled, exit';
    exit(1);
}

$repository->deleteItems();

function confirm(string $question): bool {
    return readline("{$question} [y/n]: ") === 'y'
}
```

**[⬆ наверх](#Содержание)**

## **28 Авторы**
**Оригинала**
- Удодов Евгений ([flrnull](https://github.com/flrnull))
- Рудаченко Сергей ([m1nor](https://github.com/m1nor))
- Зюзькевич Юрий ([Farengier](https://github.com/Farengier))
- 
**Дополненной версии**
- Алексей Коваленко [alexey-kov](https://github.com/alexey-kov)