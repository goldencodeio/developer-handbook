# Руководство по написанию JavaScript кода

Здесь, в Золотом Коде, для написания JavaScript мы используем лучшие практики от [Google](https://google.github.io/styleguide/jsguide.html) и [Airbnb](https://github.com/airbnb/javascript).

## <a name='TOC'>Оглавление</a>

1. [Оглавление](#TOC)
1. [ES2015](#es2015)
1. [Пробелы](#whitespace)
1. [Типы](#types)
1. [Объекты](#objects)
1. [Массивы](#arrays)
1. [Запятые](#commas)
1. [Точки с запятой](#semicolons)
1. [Приведение типов](#type-coercion)
1. [Строки](#strings)
1. [Функции](#functions)
1. [Свойства](#properties)
1. [Условные выражения и равенства](#conditionals)
1. [Блоки кода](#blocks)
1. [Комментарии](#comments)
1. [jQuery](#jquery)
1. [Быстродействие](#performance)
1. [Ресурсы](#resources)

## <a name='es2015'>ES2015</a>

На проектах используются системы сборки, которая позволяет использовать новый стандарт JavaScript - [ECMAScript 2015](//learn.javascript.ru/es-modern), и компилироваь его в привычный всем бразуерам ES5.

- Объявляйте переменные через `let`, а константы через `const`

```javascript
const CONSTANT_NAME = 'constant';
let variableName = 'value';
```

- Используйте стрелочные функции (arrow functions), когда внутри нее не требуется использовать `this`

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

- Разбивайте код на модули, подключаемые в основной файл. Так же все зависимости, как jquery, UIkit, underscore и т.д., устанавливайте чере `npm` и подключайте, так где они необходимы.

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

## <a name='whitespace'>Пробелы</a>

- Для отступов использовать 2 пробела. В этом примере `∙∙∙∙` - четыре пробела, а `――――` - один отступ с табуляцией.

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

- Используйте отступы, когда делаете цепочки вызовов.

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

**[[⬆]](#Оглавление)**

## <a name='types'>Типы</a>

- **Простые типы**: Когда вы взаимодействуете с простым типом, вы взаимодействуете непосредственно с его значением в памяти.

    - `string`
    - `number`
    - `boolean`
    - `null`
    - `undefined`

    ```javascript
    let foo = 1;
    let bar = foo;
    
    bar = 9;
    
    console.log(foo, bar); // => 1, 9. foo не изменился
    ```

- **Сложные типы**: Когда вы взаимодействуете со сложным типом, вы взаимодействуете с ссылкой на его значение в памяти.

    - `object`
    - `array`
    - `function`

    ```javascript
    let foo = [1, 2];
    let bar = foo;
    
    bar[0] = 9;
    
    console.log(foo[0], bar[0]); // => 9, 9.
    ```

**[[⬆]](#Оглавление)**

## <a name='objects'>Объекты</a>

- Для создания объекта используйте фигурные скобки. Не создавайте объекты через конструктор `new Object`.

```javascript
// плохо
let item = new Object();

// хорошо
let item = {};
```

- Не используйте [зарезервированные слова](//es5.github.io/#x7.6.1) в качестве ключей объектов. Они не будут работать в IE8. [Подробнее](//github.com/airbnb/javascript/issues/61)

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

- Не используйте ключевые слова (в том числе измененные). Вместо них используйте синонимы.

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

**[[⬆]](#Оглавление)**

## <a name='arrays'>Массивы</a>

- Для создания массива используйте квадратные скобки. Не создавайте массивы через конструктор `new Array`.

```javascript
// плохо
let items = new Array();

// хорошо
let items = [];
```

- Если вы не знаете длину массива, используйте `Array::push`.

```javascript
let someStack = [];


// плохо
someStack[someStack.length] = 'zk';

// хорошо
someStack.push('zk');
```

- Если вам необходимо скопировать массив, используйте `Array::slice`. [jsPerf](//jsperf.com/converting-arguments-to-an-array/7)

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

- Чтобы скопировать похожий по свойствам на массив объект (например, `NodeList` или `Arguments`), используйте `Array::slice`.

```javascript
function trigger() {
  let args = Array.prototype.slice.call(arguments);
  ...
}
```

**[[⬆]](#Оглавление)**


## <a name='commas'>Запятые</a>

- Запятые в начале строки: **Нет.**

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

- Дополнительная запятая в конце объектов: **Нет**. Она способна вызвать проблемы с IE6/7 и IE9 в режиме совместимости. В некоторых реализациях ES3 запятая в конце массива увеличивает его длину на 1, что может вызвать проблемы. Этот вопрос был прояснен только в ES5 ([оригинал](//es5.github.io/#D)):

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

**[[⬆]](#Оглавление)**


## <a name='semicolons'>Точки с запятой</a>

- **Да.**

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

**[[⬆]](#Оглавление)**


## <a name='type-coercion'>Приведение типов</a>

- Выполняйте приведение типов в начале операции, но не делайте его избыточным.
- Строки:

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

- Используйте `parseInt` для чисел и всегда указывайте основание для приведения типов.

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

- Если по какой-либо причине вы делаете что-то дикое, и именно на `parseInt` тратится больше всего ресурсов, используйте побитовый сдвиг [из соображений быстродействия](//jsperf.com/coercion-vs-casting/3), но обязательно оставьте комментарий с объяснением причин.

```javascript
// хорошо
/**
 * этот код медленно работал из-за parseInt
 * побитовый сдвиг строки для приведения ее к числу
 * работает значительно быстрее.
 */
let val = inputValue >> 0;
```

- **Примечание:** Будьте осторожны с побитовыми операциями. Числа в JavaScript являются [64-битными значениями](//es5.github.io/#x4.3.19), но побитовые операции всегда возвращают 32-битные значенения. [Источник](//es5.github.io/#x11.7). Побитовые операции над числами, значение которых выходит за 32 бита (верхний предел: 2,147,483,647).

```
2147483647 >> 0 //=> 2147483647
2147483648 >> 0 //=> -2147483648
2147483649 >> 0 //=> -2147483647
```

- логические типы(Boolean):

```javascript
let age = 0;

// плохо
let hasAge = new Boolean(age);

// хорошо
let hasAge = Boolean(age);

// хорошо
let hasAge = !!age;
```

**[[⬆]](#Оглавление)**


## <a name='strings'>Строки</a>

- Используйте одинарные кавычки `''` для строк.

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

- Используйте строки-шаблоны, когда внутри них необходимо использовать переменные:

```javascript
let apples = 2;
let oranges = 3;
console.log(`${apples} + ${oranges} = ${apples + oranges}`); // 2 + 3 = 5
```

**[[⬆]](#Оглавление)**


## <a name='functions'>Функции</a>

- Объявление функций:

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
- Никогда не объявляйте функцию внутри блока кода — например в if, while, else и так далее. Единственное исключение — блок функции. Вместо этого присваивайте функцию уже объявленной через `let` переменной. Условное объявление функций работает, но в различных браузерах работает по-разному.
- **Примечание** ECMA-262 устанавливает понятие `блока` как списка операторов. Объявление функции (не путайте с присвоением функции переменной) не является оператором. [Комментарий по этому вопросу в ECMA-262](//www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

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

- Никогда не используйте аргумент функции `arguments`, он будет более приоритетным над объектом `arguments`, который доступен без объявления для каждой функции.

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

**[[⬆]](#Оглавление)**

## <a name='properties'>Свойства</a>

- Используйте точечную нотацию для доступа к свойствам и методам.

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

- Используйте нотацию с `[]`, когда вы получаете свойство, имя для которого хранится в переменной.

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

**[[⬆]](#Оглавление)**

## <a name='conditionals'>Условные выражения и равенства</a>

- Используйте `===` и `!==` вместо `==` и `!=`.
- Условные выражения вычисляются посредством приведения к логическому типу Boolean через метод `ToBoolean` и всегда следуют следующим правилам:

+ **Object** всегда соответствует **true**
+ **Undefined** всегда соответствует **false**
+ **Null** всегда соответствует **false**
+ **Boolean** остается неизменным
+ **Number** соответствует **false**, если является **+0, -0, или NaN**, в противном случае соответствует **true**
+ **String** означает **false**, если является пустой строкой `''`, в противном случае **true**. Условно говоря, для строки происходит сравнение не ее самой, а ее длины – в соответствии с типом number.

```javascript
if ([0]) {
  // true
  // Массив(Array) является объектом, объекты преобразуются в true
}
```

- Используйте короткий синтаксис.

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

- Более подробно можно прочитать в статье [Truth Equality and JavaScript](//javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) от Angus Croll

**[[⬆]](#Оглавление)**


## <a name='blocks'>Блоки кода</a>

- Используйте фигурные скобки для всех многострочных блоков.

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

**[[⬆]](#Оглавление)**


## <a name='comments'>Комментарии</a>

- Документируйте код в формате [jsdoc](//usejsdoc.org/). Включите описание, опишите типы и значения для всех параметров и возвращаемых значений.

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

- Используйте `//` для комментариев в одну строку. Размещайте комментарии на новой строке над темой комментария. Добавляйте пустую строку над комментарием.

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

- Префикс `TODO` помогает другим разработчикам быстро понять, что вы указываете на проблему, к которой нужно вернуться в дальнейшем, или если вы предлагете решение проблемы, которое должно быть реализовано. Эти комментарии отличаются от обычных комментариев, так как не описывают текущее поведение, а призывают к действию, например `TODO -- нужно реализовать интерфейс`. Такие комментарии также автоматически обнаруживаются многими IDE и редакторами кода, что позволяет быстро перемещаться между ними.

- Используйте `// TODO FIXME:` для аннотирования проблем

```javascript
function Calculator() {

  // TODO FIXME: тут не нужно использовать глобальную переменную
  total = 0;

  return this;
}
```

- Используйте `// TODO:` для указания решений проблем

```javascript
function Calculator() {

  // TODO: должна быть возможность изменять значение через параметр функции
  this.total = 0;

  return this;
}
```

**[[⬆]](#Оглавление)**

## <a name='jquery'>jQuery</a>

- Для jQuery-переменных используйте префикс `$`.

```javascript
// плохо
let sidebar = $('#sidebar');

// хорошо
let $sidebar = $('#sidebar');
```

- Кэшируйте jQuery-запросы. Каждый новый jQuery-запрос делает повторный поиск по DOM-дереву, и приложение начинает работать медленнее.

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

- Для DOM-запросов используйте классический каскадный CSS-синтаксис `$('.sidebar ul')` или родитель > потомок `$('.sidebar > ul')`. [jsPerf](//jsperf.com/jquery-find-vs-context-sel/16)
- Используйте `find` для поиска внутри DOM-объекта.

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

- Для поиска одного элемента используйте только идентификатор `#id`.

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

- Для задания обработчика элементу используйте метод [`.on()`](//api.jquery.com/on/).

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

**[[⬆]](#Оглавление)**

## <a name='performance'>Быстродействие</a>

- [On Layout & Web Performance](//kellegous.com/j/2013/01/26/layout-performance/)
- [String vs Array Concat](//jsperf.com/string-vs-array-concat/2)
- [Try/Catch Cost In a Loop](//jsperf.com/try-catch-in-loop-cost)
- [Bang Function](//jsperf.com/bang-function)
- [jQuery Find vs Context, Selector](//jsperf.com/jquery-find-vs-context-sel/13)
- [innerHTML vs textContent for script text](//jsperf.com/innerhtml-vs-textcontent-for-script-text)
- [Long String Concatenation](//jsperf.com/ya-string-concat)
- В процессе наполнения...

**[[⬆]](#Оглавление)**


## <a name='resources'>Ресурсы</a>

**Прочитайте это**

- [Annotated ECMAScript 5.1](//es5.github.com/)

**Другие руководства по стилю**

- [Google JavaScript Style Guide](//google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
- [jQuery Core Style Guidelines](//docs.jquery.com/JQuery_Core_Style_Guidelines)
- [Principles of Writing Consistent, Idiomatic JavaScript](//github.com/rwldrn/idiomatic.js/)

**Другие стили**

- [Naming this in nested functions](//gist.github.com/4135065) - Christian Johansen
- [Conditional Callbacks](//github.com/airbnb/javascript/issues/52)
- [Popular JavaScript Coding Conventions on Github](//sideeffect.kr/popularconvention/#javascript)

**Дальнейшее прочтение**

- [Understanding JavaScript Closures](//javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
- [Basic JavaScript for the impatient programmer](//www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer

**Книги**

- [JavaScript: The Good Parts](//www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
- [JavaScript Patterns](//www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
- [Pro JavaScript Design Patterns](//www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
- [High Performance Web Sites: Essential Knowledge for Front-End Engineers](//www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
- [Maintainable JavaScript](//www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
- [JavaScript Web Applications](//www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
- [Pro JavaScript Techniques](//www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
- [Smashing Node.js: JavaScript Everywhere](//www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
- [Secrets of the JavaScript Ninja](//www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
- [Human JavaScript](//humanjavascript.com/) - Henrik Joreteg
- [Superhero.js](//superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
- [JSBooks](//jsbooks.revolunet.com/)

**Блоги**

- [DailyJS](//dailyjs.com/)
- [JavaScript Weekly](//javascriptweekly.com/)
- [JavaScript, JavaScript...](//javascriptweblog.wordpress.com/)
- [Bocoup Weblog](//weblog.bocoup.com/)
- [Adequately Good](//www.adequatelygood.com/)
- [NCZOnline](//www.nczonline.net/)
- [Perfection Kills](//perfectionkills.com/)
- [Ben Alman](//benalman.com/)
- [Dmitry Baranovskiy](//dmitry.baranovskiy.com/)
- [Dustin Diaz](//dustindiaz.com/)
- [nettuts](//net.tutsplus.com/?s=javascript)

**[[⬆]](#Оглавление)**
