# Javascript

Здесь, в Золотом Коде, для написания JavaScript мы используем лучшие практики от [Google](https://google.github.io/styleguide/jsguide.html) и [Airbnb](https://github.com/airbnb/javascript).

## [Оглавление](javascript.md)

1. [Оглавление](javascript.md#TOC)
2. [ES2015](javascript.md#es2015)
3. [Пробелы](javascript.md#whitespace)
4. [Типы](javascript.md#types)
5. [Объекты](javascript.md#objects)
6. [Массивы](javascript.md#arrays)
7. [Запятые](javascript.md#commas)
8. [Точки с запятой](javascript.md#semicolons)
9. [Приведение типов](javascript.md#type-coercion)
10. [Строки](javascript.md#strings)
11. [Функции](javascript.md#functions)
12. [Свойства](javascript.md#properties)
13. [Условные выражения и равенства](javascript.md#conditionals)
14. [Блоки кода](javascript.md#blocks)
15. [Комментарии](javascript.md#comments)
16. [jQuery](javascript.md#jquery)
17. [Быстродействие](javascript.md#performance)
18. [Ресурсы](javascript.md#resources)

## [ES2015](javascript.md)

На проектах используются системы сборки, которая позволяет использовать новый стандарт JavaScript - [ECMAScript 2015](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/learn.javascript.ru/es-modern/README.md), и компилироваь его в привычный всем бразуерам ES5.

* Объявляйте переменные через `let`, а константы через `const`

```javascript
const CONSTANT_NAME = 'constant';
let variableName = 'value';
```

* Используйте стрелочные функции \(arrow functions\), когда внутри нее не требуется использовать `this`

```javascript
$(window).load(() => {
  // Do something

  // But
  $('form-selector').submit(function(e) {
    e.preventDefault();
    let form = $(this);
  });
});
```

* Разбивайте код на модули, подключаемые в основной файл. Так же все зависимости, как jquery, UIkit, underscore и т.д., устанавливайте чере `npm` и подключайте, так где они необходимы.

```javascript
// module.js
import $ from 'jquery'; // from node_modules
export default (() => {
  let body = $('body');
  return () => {
    body.addClass('className');
  };
})();

// main.js
import module form './module'; // local module
module();
```

## [Пробелы](javascript.md)

* Для отступов использовать 2 пробела. В этом примере `∙∙∙∙` - четыре пробела, а `――――` - один отступ с табуляцией.

```javascript
// плохо
function() {
∙let name;
}

// плохо
function() {
∙∙∙∙let name;
}

// плохо
function() {
――――let name;
}

// хорошо
function() {
∙∙let name;
}
```

* Используйте отступы, когда делаете цепочки вызовов.

```javascript
// плохо
$('#items').find('.selected').highlight().end().find('.open').updateCount();

// хорошо
$('#items')
  .find('.selected')
    .highlight()
    .end()
  .find('.open')
    .updateCount();
```

[**\[⬆\]**](javascript.md#Оглавление)

## [Типы](javascript.md)

* **Простые типы**: Когда вы взаимодействуете с простым типом, вы взаимодействуете непосредственно с его значением в памяти.
  * `string`
  * `number`
  * `boolean`
  * `null`
  * `undefined`

    ```javascript
    let foo = 1;
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9. foo не изменился
    ```
* **Сложные типы**: Когда вы взаимодействуете со сложным типом, вы взаимодействуете с ссылкой на его значение в памяти.
  * `object`
  * `array`
  * `function`

    ```javascript
    let foo = [1, 2];
    let bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9.
    ```

[**\[⬆\]**](javascript.md#Оглавление)

## [Объекты](javascript.md)

* Для создания объекта используйте фигурные скобки. Не создавайте объекты через конструктор `new Object`.

```javascript
// плохо
let item = new Object();

// хорошо
let item = {};
```

* Не используйте [зарезервированные слова](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/es5.github.io#x7.6.1) в качестве ключей объектов. Они не будут работать в IE8. [Подробнее](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/github.com/airbnb/javascript/issues/61/README.md)

```javascript
// плохо
let superman = {
  default: { clark: 'kent' },
  private: true
};

// хорошо
let superman = {
  defaults: {clark: 'kent'},
  hidden: true
};
```

* Не используйте ключевые слова \(в том числе измененные\). Вместо них используйте синонимы.

```javascript
// плохо
let superman = {
  class: 'alien'
};

// плохо
let superman = {
  klass: 'alien'
};

// хорошо
let superman = {
  type: 'alien'
};
```

[**\[⬆\]**](javascript.md#Оглавление)

## [Массивы](javascript.md)

* Для создания массива используйте квадратные скобки. Не создавайте массивы через конструктор `new Array`.

```javascript
// плохо
let items = new Array();

// хорошо
let items = [];
```

* Если вы не знаете длину массива, используйте `Array::push`.

```javascript
let someStack = [];


// плохо
someStack[someStack.length] = 'zk';

// хорошо
someStack.push('zk');
```

* Если вам необходимо скопировать массив, используйте `Array::slice`. [jsPerf](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/jsperf.com/converting-arguments-to-an-array/7/README.md)

```javascript
let len = items.length,
    itemsCopy = [],
    i;

// плохо
for (i = 0; i < len; i++) {
  itemsCopy[i] = items[i];
}

// хорошо
itemsCopy = items.slice();
```

* Чтобы скопировать похожий по свойствам на массив объект \(например, `NodeList` или `Arguments`\), используйте `Array::slice`.

```javascript
function trigger() {
  let args = Array.prototype.slice.call(arguments);
  ...
}
```

[**\[⬆\]**](javascript.md#Оглавление)

## [Запятые](javascript.md)

* Запятые в начале строки: **Нет.**

```javascript
// плохо
let hero = {
    firstName: 'Bob'
  , lastName: 'Parr'
  , heroName: 'Mr. Incredible'
  , superPower: 'strength'
};

// хорошо
let hero = {
  firstName: 'Bob',
  lastName: 'Parr',
  heroName: 'Mr. Incredible',
  superPower: 'strength'
};
```

* Дополнительная запятая в конце объектов: **Нет**. Она способна вызвать проблемы с IE6/7 и IE9 в режиме совместимости. В некоторых реализациях ES3 запятая в конце массива увеличивает его длину на 1, что может вызвать проблемы. Этот вопрос был прояснен только в ES5 \([оригинал](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/es5.github.io#D)\):

```javascript
// плохо
let hero = {
  firstName: 'Kevin',
  lastName: 'Flynn',
};

let heroes = [
  'Batman',
  'Superman',
];

// хорошо
let hero = {
  firstName: 'Kevin',
  lastName: 'Flynn'
};

let heroes = [
  'Batman',
  'Superman'
];
```

[**\[⬆\]**](javascript.md#Оглавление)

## [Точки с запятой](javascript.md)

* **Да.**

```javascript
// плохо
(function() {
  let name = 'Skywalker'
  return name
})()

// хорошо
(function() {
  let name = 'Skywalker';
  return name;
})();

// хорошо
;(function() {
  let name = 'Skywalker';
  return name;
})();
```

[**\[⬆\]**](javascript.md#Оглавление)

## [Приведение типов](javascript.md)

* Выполняйте приведение типов в начале операции, но не делайте его избыточным.
* Строки:

```javascript
//  => this.reviewScore = 9;

// плохо
let totalScore = this.reviewScore + '';

// хорошо
let totalScore = '' + this.reviewScore;

// плохо
let totalScore = '' + this.reviewScore + ' итого';

// хорошо
let totalScore = this.reviewScore + ' итого';
```

* Используйте `parseInt` для чисел и всегда указывайте основание для приведения типов.

```javascript
let inputValue = '4';

// плохо
let val = new Number(inputValue);

// плохо
let val = +inputValue;

// плохо
let val = inputValue >> 0;

// плохо
let val = parseInt(inputValue);

// хорошо
let val = Number(inputValue);

// хорошо
let val = parseInt(inputValue, 10);
```

* Если по какой-либо причине вы делаете что-то дикое, и именно на `parseInt` тратится больше всего ресурсов, используйте побитовый сдвиг [из соображений быстродействия](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/jsperf.com/coercion-vs-casting/3/README.md), но обязательно оставьте комментарий с объяснением причин.

```javascript
// хорошо
/**
 * этот код медленно работал из-за parseInt
 * побитовый сдвиг строки для приведения ее к числу
 * работает значительно быстрее.
 */
let val = inputValue >> 0;
```

* **Примечание:** Будьте осторожны с побитовыми операциями. Числа в JavaScript являются [64-битными значениями](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/es5.github.io#x4.3.19), но побитовые операции всегда возвращают 32-битные значенения. [Источник](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/es5.github.io#x11.7). Побитовые операции над числами, значение которых выходит за 32 бита \(верхний предел: 2,147,483,647\).

```text
2147483647 >> 0 //=> 2147483647
2147483648 >> 0 //=> -2147483648
2147483649 >> 0 //=> -2147483647
```

* логические типы\(Boolean\):

```javascript
let age = 0;

// плохо
let hasAge = new Boolean(age);

// хорошо
let hasAge = Boolean(age);

// хорошо
let hasAge = !!age;
```

[**\[⬆\]**](javascript.md#Оглавление)

## [Строки](javascript.md)

* Используйте одинарные кавычки `''` для строк.

```javascript
// плохо
let name = "Боб Дилан";

// хорошо
let name = 'Боб Дилан';

// плохо
let fullName = "Боб " + this.lastName;

// хорошо
let fullName = 'Дилан ' + this.lastName;
```

* Используйте строки-шаблоны, когда внутри них необходимо использовать переменные:

```javascript
let apples = 2;
let oranges = 3;
console.log(`${apples} + ${oranges} = ${apples + oranges}`); // 2 + 3 = 5
```

[**\[⬆\]**](javascript.md#Оглавление)

## [Функции](javascript.md)

* Объявление функций:

```javascript
// объявление анонимной функции
let anonymous = function () {
  return true;
};

// объявление именованной функции
let named = function named() {
  return true;
};

// объявление функции, которая сразу же выполняется (замыкание)
(function () {
  console.log('Если вы читаете это, вы открыли консоль.');
})();
```

* Никогда не объявляйте функцию внутри блока кода — например в if, while, else и так далее. Единственное исключение — блок функции. Вместо этого присваивайте функцию уже объявленной через `let` переменной. Условное объявление функций работает, но в различных браузерах работает по-разному.
* **Примечание** ECMA-262 устанавливает понятие `блока` как списка операторов. Объявление функции \(не путайте с присвоением функции переменной\) не является оператором. [Комментарий по этому вопросу в ECMA-262](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

```javascript
// плохо
if (currentUser) {
  function test() {
    console.log('Плохой мальчик.');
  }
}

// хорошо
let test;
if (currentUser) {
  test = function test() {
    console.log('Молодец.');
  };
}
```

* Никогда не используйте аргумент функции `arguments`, он будет более приоритетным над объектом `arguments`, который доступен без объявления для каждой функции.

```javascript
// плохо
function nope(name, options, arguments) {
  // ...код...
}

// хорошо
function yup(name, options, args) {
  // ...код...
}
```

[**\[⬆\]**](javascript.md#Оглавление)

## [Свойства](javascript.md)

* Используйте точечную нотацию для доступа к свойствам и методам.

```javascript
let luke = {
  jedi: true,
  age: 28
};

// плохо
let isJedi = luke['jedi'];

// хорошо
let isJedi = luke.jedi;
```

* Используйте нотацию с `[]`, когда вы получаете свойство, имя для которого хранится в переменной.

```javascript
let luke = {
  jedi: true,
  age: 28
};

function getProp(prop) {
  return luke[prop];
}

let isJedi = getProp('jedi');
```

[**\[⬆\]**](javascript.md#Оглавление)

## [Условные выражения и равенства](javascript.md)

* Используйте `===` и `!==` вместо `==` и `!=`.
* Условные выражения вычисляются посредством приведения к логическому типу Boolean через метод `ToBoolean` и всегда следуют следующим правилам:
* **Object** всегда соответствует **true**
* **Undefined** всегда соответствует **false**
* **Null** всегда соответствует **false**
* **Boolean** остается неизменным
* **Number** соответствует **false**, если является **+0, -0, или NaN**, в противном случае соответствует **true**
* **String** означает **false**, если является пустой строкой `''`, в противном случае **true**. Условно говоря, для строки происходит сравнение не ее самой, а ее длины – в соответствии с типом number.

```javascript
if ([0]) {
  // true
  // Массив(Array) является объектом, объекты преобразуются в true
}
```

* Используйте короткий синтаксис.

```javascript
// плохо
if (name !== '') {
  // ...код...
}

// хорошо
if (name) {
  // ...код...
}

// плохо
if (collection.length > 0) {
  // ...код...
}

// хорошо
if (collection.length) {
  // ...код...
}
```

* Более подробно можно прочитать в статье [Truth Equality and JavaScript](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/README.md#more-2108) от Angus Croll

[**\[⬆\]**](javascript.md#Оглавление)

## [Блоки кода](javascript.md)

* Используйте фигурные скобки для всех многострочных блоков.

```javascript
// плохо
if (test) {
  return false;
}

// хорошо
if (test)
  return false;

// еще лучше
if (test) return false;

// плохо
function() { return false; }

// хорошо
function() {
  return false;
}
```

[**\[⬆\]**](javascript.md#Оглавление)

## [Комментарии](javascript.md)

* Документируйте код в формате [jsdoc](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/usejsdoc.org). Включите описание, опишите типы и значения для всех параметров и возвращаемых значений.

```javascript
// плохо
// make() возвращает новый элемент
// основываясь на получаемом имени тэга
//
// @param <String> tag
// @return <Element> element
function make(tag) {

  // ...создаем element...

  return element;
}

// хорошо
/**
 * make() возвращает новый элемент
 * основываясь на получаемом имени тэга
 *
 * @param <String> tag
 * @return <Element> element
 */
function make(tag) {

  // ...создаем element...

  return element;
}
```

* Используйте `//` для комментариев в одну строку. Размещайте комментарии на новой строке над темой комментария. Добавляйте пустую строку над комментарием.

```javascript
// плохо
let active = true;  // устанавливаем активным элементом

// хорошо
// устанавливаем активным элементом
let active = true;

// плохо
function getType() {
  console.log('проверяем тип...');
  // задаем тип по умолчанию 'no type'
  let type = this._type || 'no type';

  return type;
}

// хорошо
function getType() {
  console.log('проверяем тип...');

  // задаем тип по умолчанию 'no type'
  let type = this._type || 'no type';

  return type;
}
```

* Префикс `TODO` помогает другим разработчикам быстро понять, что вы указываете на проблему, к которой нужно вернуться в дальнейшем, или если вы предлагете решение проблемы, которое должно быть реализовано. Эти комментарии отличаются от обычных комментариев, так как не описывают текущее поведение, а призывают к действию, например `TODO -- нужно реализовать интерфейс`. Такие комментарии также автоматически обнаруживаются многими IDE и редакторами кода, что позволяет быстро перемещаться между ними.
* Используйте `// TODO FIXME:` для аннотирования проблем

```javascript
function Calculator() {

  // TODO FIXME: тут не нужно использовать глобальную переменную
  total = 0;

  return this;
}
```

* Используйте `// TODO:` для указания решений проблем

```javascript
function Calculator() {

  // TODO: должна быть возможность изменять значение через параметр функции
  this.total = 0;

  return this;
}
```

[**\[⬆\]**](javascript.md#Оглавление)

## [jQuery](javascript.md)

* Для jQuery-переменных используйте префикс `$`.

```javascript
// плохо
let sidebar = $('#sidebar');

// хорошо
let $sidebar = $('#sidebar');
```

* Кэшируйте jQuery-запросы. Каждый новый jQuery-запрос делает повторный поиск по DOM-дереву, и приложение начинает работать медленнее.

```javascript
// плохо
function setSidebar() {
  $('#sidebar').hide();

  // ...код...

  $('#sidebar').css({
    backgroundColor: 'pink'
  });
}

// хорошо
function setSidebar() {
  let $sidebar = $('#sidebar');
  $sidebar.hide();

  // ...код...

  $sidebar.css({
    backgroundColor: 'pink'
  });
}
```

* Для DOM-запросов используйте классический каскадный CSS-синтаксис `$('.sidebar ul')` или родитель &gt; потомок `$('.sidebar > ul')`. [jsPerf](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/jsperf.com/jquery-find-vs-context-sel/16/README.md)
* Используйте `find` для поиска внутри DOM-объекта.

```javascript
// плохо
$('ul', '#sidebar').hide();

// плохо
$('#sidebar').find('ul').hide();

// хорошо
$('#sidebar ul').hide();

// хорошо
$('#sidebar > ul').hide();

// хорошо
$sidebar.find('ul').hide();
```

* Для поиска одного элемента используйте только идентификатор `#id`.

```javascript
// плохо
$('.menu button');

// плохо
$('.menu button#menuToggler');

// плохо
$('button#menuToggler');

// хорошо
$('#menuToggler');
```

* Для задания обработчика элементу используйте метод [`.on()`](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/api.jquery.com/on/README.md).

```javascript
// Плохо
$input
.click(function () { /* ... */ })
.focus(function () { /* ... */ })
.blur(function () { /* ... */ });

// Хорошо
$input
.on('click', function () { /* ... */ })
.on('focus', function () { /* ... */ })
.on('blur', function () { /* ... */ });

// Лучше
// Несколько событий разделяются пробелами
$field.on('click focus', function () { /* ... */ });

$input.on({
// Несколько событий разделяются пробелами
'click focus': function () { /* ... */ },
blur: function () { /* ... */ }
});
```

[**\[⬆\]**](javascript.md#Оглавление)

## [Быстродействие](javascript.md)

* [On Layout & Web Performance](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/kellegous.com/j/2013/01/26/layout-performance/README.md)
* [String vs Array Concat](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/jsperf.com/string-vs-array-concat/2/README.md)
* [Try/Catch Cost In a Loop](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/jsperf.com/try-catch-in-loop-cost/README.md)
* [Bang Function](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/jsperf.com/bang-function/README.md)
* [jQuery Find vs Context, Selector](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/jsperf.com/jquery-find-vs-context-sel/13/README.md)
* [innerHTML vs textContent for script text](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/jsperf.com/innerhtml-vs-textcontent-for-script-text/README.md)
* [Long String Concatenation](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/jsperf.com/ya-string-concat/README.md)
* В процессе наполнения...

[**\[⬆\]**](javascript.md#Оглавление)

## [Ресурсы](javascript.md)

**Прочитайте это**

* [Annotated ECMAScript 5.1](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/es5.github.com)

**Другие руководства по стилю**

* [Google JavaScript Style Guide](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
* [jQuery Core Style Guidelines](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/docs.jquery.com/JQuery_Core_Style_Guidelines/README.md)
* [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/github.com/rwldrn/idiomatic.js)

**Другие стили**

* [Naming this in nested functions](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/gist.github.com/4135065/README.md) - Christian Johansen
* [Conditional Callbacks](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/github.com/airbnb/javascript/issues/52/README.md)
* [Popular JavaScript Coding Conventions on Github](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/sideeffect.kr/popularconvention/README.md#javascript)

**Дальнейшее прочтение**

* [Understanding JavaScript Closures](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/README.md) - Angus Croll
* [Basic JavaScript for the impatient programmer](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer

**Книги**

* [JavaScript: The Good Parts](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742/README.md) - Douglas Crockford
* [JavaScript Patterns](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752/README.md) - Stoyan Stefanov
* [Pro JavaScript Design Patterns](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X/README.md)  - Ross Harmes and Dustin Diaz
* [High Performance Web Sites: Essential Knowledge for Front-End Engineers](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309/README.md) - Steve Souders
* [Maintainable JavaScript](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680/README.md) - Nicholas C. Zakas
* [JavaScript Web Applications](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X/README.md) - Alex MacCaw
* [Pro JavaScript Techniques](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273/README.md) - John Resig
* [Smashing Node.js: JavaScript Everywhere](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595/README.md) - Guillermo Rauch
* [Secrets of the JavaScript Ninja](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X/README.md) - John Resig and Bear Bibeault
* [Human JavaScript](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/humanjavascript.com) - Henrik Joreteg
* [Superhero.js](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/superherojs.com) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
* [JSBooks](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/jsbooks.revolunet.com)

**Блоги**

* [DailyJS](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/dailyjs.com)
* [JavaScript Weekly](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/javascriptweekly.com)
* [JavaScript, JavaScript...](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/javascriptweblog.wordpress.com)
* [Bocoup Weblog](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/weblog.bocoup.com)
* [Adequately Good](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/www.adequatelygood.com)
* [NCZOnline](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/www.nczonline.net)
* [Perfection Kills](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/perfectionkills.com)
* [Ben Alman](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/benalman.com)
* [Dmitry Baranovskiy](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/dmitry.baranovskiy.com)
* [Dustin Diaz](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/dustindiaz.com)
* [nettuts](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/net.tutsplus.com/?s=javascript/README.md)

[**\[⬆\]**](javascript.md#Оглавление)

