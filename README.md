# Airbnb Руководство по Стилю JavaScript() {

*Главным образом разумный подход к JavaScript*


## <a name='TOC'>Оглавление</a>

  1. [Типы](#types)
  1. [Объекты](#objects)
  1. [Массивы](#arrays)
  1. [Строки](#strings)
  1. [Функции](#functions)
  1. [Свойства](#properties)
  1. [Переменные](#variables)
  1. [Вытягивание](#hoisting)
  1. [Условные выражения и равенства](#conditionals)
  1. [Блоки](#blocks)
  1. [Комментарии](#comments)
  1. [Пробелы](#whitespace)
  1. [Запятые](#commas)
  1. [Точки с запятой](#semicolons)
  1. [Приведение типов](#type-coercion)
  1. [Соглашения об именованиях](#naming-conventions)
  1. [Аксессоры](#accessors)
  1. [Конструкторы](#constructors)
  1. [События](#events)
  1. [Модули](#modules)
  1. [jQuery](#jquery)
  1. [Совместимость с ES5](#es5)
  1. [Тестирование](#testing)
  1. [Производительность](#performance)
  1. [Ресурсы](#resources)
  1. [В реальных условиях](#in-the-wild)
  1. [Перевод](#translation)
  1. [Руководство к Руководству по Стилю JavaScript](#guide-guide)
  1. [Авторы](#contributors)
  1. [Лицензия](#license)

## <a name='types'>Типы</a>

  - **Примитивы**: При обращении к примитивному типу вы работаете непосредственно с его значением

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    var foo = 1,
        bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - **Сложные типы**: При обращении к сложному типу вы работаете со ссылкой на его значение

    + `object`
    + `array`
    + `function`

    ```javascript
    var foo = [1, 2],
        bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

    **[[⬆]](#TOC)**

## <a name='objects'>Объекты</a>

  - Используйте литеральный синтаксис для создания объектов.

    ```javascript
    // плохо
    var item = new Object();

    // хорошо
    var item = {};
    ```

  - Не используйте [зарезервированные слова](http://es5.github.io/#x7.6.1) в качестве ключей. Это не будет работать в IE8. [Детали](https://github.com/airbnb/javascript/issues/61)

    ```javascript
    // плохо
    var superman = {
      default: { clark: 'kent' },
      private: true
    };

    // хорошо
    var superman = {
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

  - Используйте различимые синонимы вместо зарезервированных слов.

    ```javascript
    // плохо
    var superman = {
      class: 'alien'
    };

    // плохо
    var superman = {
      klass: 'alien'
    };

    // хорошо
    var superman = {
      type: 'alien'
    };
    ```
    **[[⬆]](#TOC)**

## <a name='arrays'>Массивы</a>

  - Используйте литеральный синтаксис для создания массивов

    ```javascript
    // плохо
    var items = new Array();

    // хорошо
    var items = [];
    ```

  - Если неизвестна длинна массива, используйте Array#push.

    ```javascript
    var someStack = [];


    // плохо
    someStack[someStack.length] = 'abracadabra';

    // хорошо
    someStack.push('abracadabra');
    ```

  - Для копирования массива используйте Array#slice. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length,
        itemsCopy = [],
        i;

    // плохо
    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // хорошо
    itemsCopy = items.slice();
    ```

  - Для преобразования псевдо-массива (array-like) объекта в массив, используйте Array#slice.

    ```javascript
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      ...
    }
    ```

    **[[⬆]](#TOC)**


## <a name='strings'>Строки</a>

  - Используйте одинарные кавычки `''` для строк

    ```javascript
    // плохо
    var name = "Bob Parr";

    // хорошо
    var name = 'Bob Parr';

    // плохо
    var fullName = "Bob " + this.lastName;

    // хорошо
    var fullName = 'Bob ' + this.lastName;
    ```

  - Строки длиннее 80 символов должны быть записаны в несколько строчек с помощью конкатенации.
  - Примечание: При злоупотреблении, конкатенация длинных строк может повлиять на производительность. [jsPerf](http://jsperf.com/ya-string-concat) & [Обсуждение](https://github.com/airbnb/javascript/issues/40)

    ```javascript
    // плохо
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // плохо
    var errorMessage = 'This is a super long error that \
    was thrown because of Batman. \
    When you stop to think about \
    how Batman had anything to do \
    with this, you would get nowhere \
    fast.';


    // хорошо
    var errorMessage = 'This is a super long error that ' +
      'was thrown because of Batman.' +
      'When you stop to think about ' +
      'how Batman had anything to do ' +
      'with this, you would get nowhere ' +
      'fast.';
    ```

  - При программном создании строки, используйте Array#join вместо конкатенации. В основном для IE: [jsPerf](http://jsperf.com/string-vs-array-concat/2).

    ```javascript
    var items,
        messages,
        length,
        i;

    messages = [{
        state: 'success',
        message: 'This one worked.'
    },{
        state: 'success',
        message: 'This one worked as well.'
    },{
        state: 'error',
        message: 'This one did not work.'
    }];

    length = messages.length;

    // плохо
    function inbox(messages) {
      items = '<ul>';

      for (i = 0; i < length; i++) {
        items += '<li>' + messages[i].message + '</li>';
      }

      return items + '</ul>';
    }

    // хорошо
    function inbox(messages) {
      items = [];

      for (i = 0; i < length; i++) {
        items[i] = messages[i].message;
      }

      return '<ul><li>' + items.join('</li><li>') + '</li></ul>';
    }
    ```

    **[[⬆]](#TOC)**


## <a name='functions'>Функции</a>

  - Функциональные выражения:

    ```javascript
    // анонимное функциональное выражение
    var anonymous = function() {
      return true;
    };

    // именованное функциональное выражение
    var named = function named() {
      return true;
    };

    // вызываемая моментально функциональное выражение (IIFE)
    (function() {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - Никогда не объявляйте функцию в нефункциональном блоке (if, while и т.п.). Браузеры позволят вам сделать это, но будут по-разному интерпретировать. Вместо этого присвойте функцию переменной. 
  - **Примечание:** ECMA-262 определяет `блок` как список инструкций. Объявление функции не является инструкцией. [Read ECMA-262's note on this issue](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // плохо
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // хорошо
    var test;
    if (currentUser) {
      test = function test() {
        console.log('Yup.');
      };
    }
    ```

  - Никогда не используйте в названии параметра `arguments`, т.к. в этом случае объект `arguments`, существующий в области видимости любой функции, не будет создан.

    ```javascript
    // плохо
    function nope(name, options, arguments) {
      // ...stuff...
    }

    // хорошо
    function yup(name, options, args) {
      // ...stuff...
    }
    ```

    **[[⬆]](#TOC)**



## <a name='properties'>Свойства</a>

  - Используйте точечную нотацию для доступа к свойствам.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    // плохо
    var isJedi = luke['jedi'];

    // хорошо
    var isJedi = luke.jedi;
    ```

  - Используйте запись квадратными скобками `[]` для доступа к свойствам с помощью переменной.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```

    **[[⬆]](#TOC)**


## <a name='variables'>Переменные</a>

  - Всегда используйте `var` для объявления переменной. Невыполнение этого требования приведет к появлению свойства в глобальном объекте. Мы хотим избежать загрязнения глобального пространства имен. Капитан Планета вас предупредил.

    ```javascript
    // плохо
    superPower = new SuperPower();

    // хорошо
    var superPower = new SuperPower();
    ```

  - Используйте одну директиву `var` при объявлении нескольких переменных, объявляя каждую с новой строки.

    ```javascript
    // плохо
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';

    // хорошо
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';
    ```

  - Объявляйте неинициализированные переменные последними. Это может быть полезно если в дальнейшем вам требуется инициализировать переменную зависящею от одной из инициализированных ранее переменных.

    ```javascript
    // плохо
    var i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // плохо
    var i, items = getItems(),
        dragonball,
        goSportsTeam = true,
        len;

    // хорошо
    var items = getItems(),
        goSportsTeam = true,
        dragonball,
        length,
        i;
    ```

  - Инициализируйте переменные наверху их области видимости. Это поможет избежать проблем с объявлением переменных и проблем связанных с вытягиванием.

    ```javascript
    // плохо
    function() {
      test();
      console.log('doing stuff..');

      //..other stuff..

      var name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // хорошо
    function() {
      var name = getName();

      test();
      console.log('doing stuff..');

      //..other stuff..

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // плохо
    function() {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      return true;
    }

    // хорошо
    function() {
      if (!arguments.length) {
        return false;
      }

      var name = getName();

      return true;
    }
    ```

    **[[⬆]](#TOC)**


## <a name='hoisting'>Вытягивание</a>

  - Обявления переменных будут перенесены наверх заключающей их области видимости. Присвоение перенесено не будет.

    ```javascript
    // мы знаем, что это не будет работать (при условии что
    // нет глобальной переменной notDefined)
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // объявление переменной после ссылки на неё 
    // будет работать благодаря вытягиванию переменной.
    // Примечание: присвоение значения `true`
    // не вытягивается.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // Интерпретатор вытягивает объявление переменной
    // наверх области видимости.
    // Это означает что пример может быть переписан так:
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - Из анонимного функционального выражения вытягивается имя переменной, но не присвоение функции.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - Из анонимного функционального выражения вытягивается имя переменной, но не название функции или её тело.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // то же справедливо когда имя функции
    // совпадает с именем переменной.
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - Из декларации функции вытягиваются имя и тело функции.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - Больше информации [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) автора [Ben Cherry](http://www.adequatelygood.com/)

    **[[⬆]](#TOC)**



## <a name='conditionals'>Условные выражения и равенства</a>

  - Используйте `===` и `!==` вместо `==` и `!=`.
  - Вычисление условных выражений происходит путем приведения с помощью метода `ToBoolean` и подчиняется следующим правилам:

    + **Object** преобразуюeтся в **true**
    + **Undefined** преобразуется в **false**
    + **Null** преобразуется в **false**
    + **Boolean** **равен входному аргументу (без преобразования)**
    + **Number** преобразуется в **false** если **+0, -0, or NaN**, иначе **true**
    + **String** преобразуется в **false** если пустая строка `''`, иначе **true**

    ```javascript
    if ([0]) {
      // true
      // Массив — это объект, а объекты преобразуются в `true`
    }
    ```

  - Используйте сокращения.

    ```javascript
    // плохо
    if (name !== '') {
      // ...stuff...
    }

    // хорошо
    if (name) {
      // ...stuff...
    }

    // плохо
    if (collection.length > 0) {
      // ...stuff...
    }

    // хорошо
    if (collection.length) {
      // ...stuff...
    }
    ```

  - Детальнее [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108), автор Angus Croll

    **[[⬆]](#TOC)**


## <a name='blocks'>Блоки</a>

  - Используйте фигурные скобки во всех многострочных блоках.

    ```javascript
    // плохо
    if (test)
      return false;

    // хорошо
    if (test) return false;

    // хорошо
    if (test) {
      return false;
    }

    // плохо
    function() { return false; }

    // хорошо
    function() {
      return false;
    }
    ```

    **[[⬆]](#TOC)**


## <a name='comments'>Комментарии</a>

  - Используйте `/** ... */` для многострочных комментариев. Включите описание, указажите типы и значения для всех параметров и возвращаемых значений.

    ```javascript
    // плохо
    // make() возвращает новый элемент
    // на основании переданного имени тега
    //
    // @param <String> tag
    // @return <Element> element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // хорошо
    /**
     * make() возвращает новый элемент
     * на основании переданного имени тега
     *
     * @param <String> tag
     * @return <Element> element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - Используйте `//` для однострочных комментариев. Размещайте однострочный комментарий с новой строки над предметом комментирования. Оставляйте пустую строку перед комментарием.

    ```javascript
    // плохо
    var active = true;  // текущий таб

    // хорошо
    // текущий таб
    var active = true;

    // плохо
    function getType() {
      console.log('fetching type...');
      // установим тип по-умолчанию 'no type'
      var type = this._type || 'no type';

      return type;
    }

    // хорошо
    function getType() {
      console.log('fetching type...');

      // установим тип по-умолчанию 'no type'
      var type = this._type || 'no type';

      return type;
    }
    ```

  - Использование префиксов `FIXME` или `TODO` помогает разработчикам понять что вы указываете на проблему, которую необходимо пересмотреть, или если вы предлагаете решение проблемы, которая должна быть решена. Эти отличаются от обычных комментариев поскольку они предполагают принятие мер. Действия: `FIXME -- необходимо выяснить это` или `TODO -- необходимо реализовать`.

  - Используйте `// FIXME:` чтобы комментировать проблемы

    ```javascript
    function Calculator() {

      // FIXME: не следует использовать глобальный контекст
      total = 0;

      return this;
    }
    ```

  - Используйте `// TODO:` для записи решения проблем

    ```javascript
    function Calculator() {

      // TODO: total должен конфмгурироваться парамером options
      this.total = 0;

      return this;
    }
  ```

    **[[⬆]](#TOC)**


## <a name='whitespace'>Пробелы</a>

  - Используйте мягкие табы равные 2 пробелам.

    ```javascript
    // плохо
    function() {
    ∙∙∙∙var name;
    }

    // плохо
    function() {
    ∙var name;
    }

    // хорошо
    function() {
    ∙∙var name;
    }
    ```
  - Ставьте 1 пробел перед открывающей фигурной скобкой.

    ```javascript
    // плохо
    function test(){
      console.log('test');
    }

    // хорошо
    function test() {
      console.log('test');
    }

    // плохо
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // хорошо
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```
  - Оставляйте пустую строку в конце файла.

    ```javascript
    // плохо
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // хорошо
    (function(global) {
      // ...stuff...
    })(this);

    ```

  - Используйте отступы для длинных цепочек методов.

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

    // плохо
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
        .attr('width',  (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // хорошо
    var leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .class('led', true)
        .attr('width',  (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

    **[[⬆]](#TOC)**

## <a name='commas'>Запятые</a>

  - Запятые в начале строки: **Неа.**

    ```javascript
    // плохо
    var once
      , upon
      , aTime;

    // хорошо
    var once,
        upon,
        aTime;

    // плохо
    var hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // хорошо
    var hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };
    ```

  - Дополнительная замыкающая запятая: **Неа.** Это вызовет проблемы в IE6/7 и IE9 если он в режиме совместимости. Также, в некоторых реализациях ES3 будет добавлен пустой элемент в массив если он имеет замыкающую запятую. Это было разъяснено в ES5 ([источник](http://es5.github.io/#D)):

  > Издание 5 разъясняет тот факт, что замыкающая запятую в конце ArrayInitialiser не добавляет к длине массива. Это не семантическое изменение от издания 3, но в некоторые реализациии могли неправильно это толковать.

    ```javascript
    // плохо
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn',
    };

    var heroes = [
      'Batman',
      'Superman',
    ];

    // хорошо
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn'
    };

    var heroes = [
      'Batman',
      'Superman'
    ];
    ```

    **[[⬆]](#TOC)**


## <a name='semicolons'>Точки с запятой</a>

  - **Ага.**

    ```javascript
    // плохо
    (function() {
      var name = 'Skywalker'
      return name
    })()

    // хорошо
    (function() {
      var name = 'Skywalker';
      return name;
    })();

    // хорошо
    ;(function() {
      var name = 'Skywalker';
      return name;
    })();
    ```

    **[[⬆]](#TOC)**


## <a name='type-coercion'>Приведение типов</a>

  - Выполяйте приведение типов в начале выражения.
  - Строки:

    ```javascript
    //  => this.reviewScore = 9;

    // плохо
    var totalScore = this.reviewScore + '';

    // хорошо
    var totalScore = '' + this.reviewScore;

    // плохо
    var totalScore = '' + this.reviewScore + ' total score';

    // хорошо
    var totalScore = this.reviewScore + ' total score';
    ```

  - Используйте `parseInt` для Number всегда указывая основание системы счисления -- radix.

    ```javascript
    var inputValue = '4';

    // плохо
    var val = new Number(inputValue);

    // плохо
    var val = +inputValue;

    // плохо
    var val = inputValue >> 0;

    // плохо
    var val = parseInt(inputValue);

    // хорошо
    var val = Number(inputValue);

    // хорошо
    var val = parseInt(inputValue, 10);
    ```

  - Если по какой-либо причине вы делаете что-то дикое и `parseInt` является узким местом и нужно использовать побитовый сдвиг из [соображений производительности](http://jsperf.com/coercion-vs-casting/3), оставьте комментарий объясняющий что вы делаете.
  - **Примечание:** Будьте осторожны при использовании побитовых сдвигов. Числа представлены как [64-битные значения](http://es5.github.io/#x4.3.19), в то время как результат операций побитовых сдвигов является знаковым 32-битовым целым([источник](http://es5.github.io/#x11.7)). Побитовый сдвиг может привести к неожиданному поведению при целых больше чем 32 бит. [Обсуждение](https://github.com/airbnb/javascript/issues/109)

    ```javascript
    // хорошо
    /**
     * parseInt был причиной медленности моего кода.
     * Побитовый сдвиг преобразует строку в число
     * намного быстрее.
     */
    var val = inputValue >> 0;
    ```

  - Booleans:

    ```javascript
    var age = 0;

    // плохо
    var hasAge = new Boolean(age);

    // хорошо
    var hasAge = Boolean(age);

    // хорошо
    var hasAge = !!age;
    ```

    **[[⬆]](#TOC)**


## <a name='naming-conventions'>Соглашения об именованиях</a>

  - Избегайте однобуквенных названий. Будьте описательны в именованиях.

    ```javascript
    // плохо
    function q() {
      // ...stuff...
    }

    // хорошо
    function query() {
      // ..stuff..
    }
    ```

  - Используйте `camelCase` для именования объектов, функций, и экземпляров

    ```javascript
    // плохо
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    function c() {};
    var u = new user({
      name: 'Bob Parr'
    });

    // хорошо
    var thisIsMyObject = {};
    function thisIsMyFunction() {};
    var user = new User({
      name: 'Bob Parr'
    });
    ```

  - Используйте `PascalCase` при именовании конструкторов и классов

    ```javascript
    // плохо
    function user(options) {
      this.name = options.name;
    }

    var bad = new user({
      name: 'nope'
    });

    // хорошо
    function User(options) {
      this.name = options.name;
    }

    var good = new User({
      name: 'yup'
    });
    ```

  - Используйте нижнее подчеркивание в начале `_` для именования приватных свойств

    ```javascript
    // плохо
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // хорошо
    this._firstName = 'Panda';
    ```

  - Для сохранения ссылки на `this` используйте `_this`.

    ```javascript
    // плохо
    function() {
      var self = this;
      return function() {
        console.log(self);
      };
    }

    // плохо
    function() {
      var that = this;
      return function() {
        console.log(that);
      };
    }

    // хорошо
    function() {
      var _this = this;
      return function() {
        console.log(_this);
      };
    }
    ```

  - Именуйте функции. Это полезно поможет при трассировке.

    ```javascript
    // плохо
    var log = function(msg) {
      console.log(msg);
    };

    // хорошо
    var log = function log(msg) {
      console.log(msg);
    };
    ```

    **[[⬆]](#TOC)**


## <a name='accessors'>Аксессоры</a>

  - Аксессорные функции для свойств не требуются
  - Если вы создаете аксессорные функции используйте getVal() и setVal('hello')

    ```javascript
    // плохо
    dragon.age();

    // хорошо
    dragon.getAge();

    // плохо
    dragon.age(25);

    // хорошо
    dragon.setAge(25);
    ```

  - Для булевого свойства используйте isVal() или hasVal()

    ```javascript
    // плохо
    if (!dragon.age()) {
      return false;
    }

    // хорошо
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - Нет ничего страшного в том чтобы создать get() и set() функции, но нужно быть последовательным.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      var lightsaber = options.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
    }

    Jedi.prototype.set = function(key, val) {
      this[key] = val;
    };

    Jedi.prototype.get = function(key) {
      return this[key];
    };
    ```

    **[[⬆]](#TOC)**


## <a name='constructors'>Конструкторы</a>

  - Присваивайте методы к прототипу, вместо перезаписи прототипа новым объектом. Замена прототипа делает невозможным наследование: сбрасывание прототипа приведет к перезваписи базового класса!

    ```javascript
    function Jedi() {
      console.log('new jedi');
    }

    // плохо
    Jedi.prototype = {
      fight: function fight() {
        console.log('fighting');
      },

      block: function block() {
        console.log('blocking');
      }
    };

    // хорошо
    Jedi.prototype.fight = function fight() {
      console.log('fighting');
    };

    Jedi.prototype.block = function block() {
      console.log('blocking');
    };
    ```

  - Методы могут возвращать `this` для построения цепочек.

    ```javascript
    // плохо
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20) // => undefined

    // хорошо
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return this;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
      return this;
    };

    var luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


  - Использование собственного метода toString() является нормальной практикой, однако, следует убедиться что он выполняется успешно и не вызывает побочных эффектов.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      this.name = options.name || 'no name';
    }

    Jedi.prototype.getName = function getName() {
      return this.name;
    };

    Jedi.prototype.toString = function toString() {
      return 'Jedi - ' + this.getName();
    };
    ```

    **[[⬆]](#TOC)**


## <a name='events'>События</a>

  - При установке обработчиков событий (будь то события DOM или что-то более проприетарное как события в Backbone), следует передавать хеш-таблицу вместо необработанного значения. Это позволит следующему разработчику иметь возможномть добавить больше данных передаваемых событием, без неоходимости искать и обновлять каждый обрабочтик события. Например, вместо:

    ```js
    // плохо
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // do something with listingId
    });
    ```

    предпочтительнее:

    ```js
    // хорошо
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
      // do something with data.listingId
    });
    ```

  **[[⬆]](#TOC)**


## <a name='modules'>Модули</a>

  - Модуль должен начинаться с `!`. Это гарантирует, что если неправильный модуль не включает конечную точку с запятой не возникает ошибки после объединения сценариев. [Объяснение](https://github.com/airbnb/javascript/issues/44#issuecomment-13063933)
  - Название файла должно быть в camelCase, он должнен находиться в папке с тем же именем, и соответсвовать названию экпортируемой переменной.
  - Добавте метод с названием noConflict() который запишет в экспортируемую переменную предыдущую версию и вернет её.
  - Всегда объявляйте `'use strict';` в начале модуля.

    ```javascript
    // fancyInput/fancyInput.js

    !function(global) {
      'use strict';

      var previousFancyInput = global.FancyInput;

      function FancyInput(options) {
        this.options = options || {};
      }

      FancyInput.noConflict = function noConflict() {
        global.FancyInput = previousFancyInput;
        return FancyInput;
      };

      global.FancyInput = FancyInput;
    }(this);
    ```

    **[[⬆]](#TOC)**


## <a name='jquery'>jQuery</a>

  - Используйте префикс `$` для jQuery объектов.

    ```javascript
    // плохо
    var sidebar = $('.sidebar');

    // хорошо
    var $sidebar = $('.sidebar');
    ```

  - Кешируйте результаты поиска jQuery.

    ```javascript
    // плохо
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // хорошо
    function setSidebar() {
      var $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - Для DOM запросов используйте вложенные `$('.sidebar ul')` или дочерние `$('.sidebar > ul')` селекторы. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - Используйте `find` для контекстных запросов к jQuery объектам.

    ```javascript
    // плохо
    $('ul', '.sidebar').hide();

    // плохо
    $('.sidebar').find('ul').hide();

    // хорошо
    $('.sidebar ul').hide();

    // хорошо
    $('.sidebar > ul').hide();

    // хорошо
    $sidebar.find('ul');
    ```

    **[[⬆]](#TOC)**


## <a name='es5'>Совместимость с ECMAScript 5</a>

  - Смотрите [Kangax](https://twitter.com/kangax/) ES5 [compatibility table](http://kangax.github.com/es5-compat-table/)

  **[[⬆]](#TOC)**


## <a name='testing'>Тестирование</a>

  - **Ага.**

    ```javascript
    function() {
      return true;
    }
    ```

    **[[⬆]](#TOC)**


## <a name='performance'>Производительность</a>

  - [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - Загрузка...

  **[[⬆]](#TOC)**


## <a name='resources'>Ресурсы</a>


**Прочитайте это**

  - [Annotated ECMAScript 5.1](http://es5.github.com/)

**Другие руководства по стилю**

  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
  - [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/)

**Другие стили**

  - [Naming this in nested functions](https://gist.github.com/4135065) - Christian Johansen
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52)
  - [Popular JavaScript Coding Conventions on Github](http://sideeffect.kr/popularconvention/#javascript)

**Дополнительные материалы**

  - [Understanding JavaScript Closures](http://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
  - [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer

**Книги**

  - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - [Secrets of the JavaScript Ninja](http://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
  - [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
  - [JSBooks](http://jsbooks.revolunet.com/)

**Блоги**

  - [DailyJS](http://dailyjs.com/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](http://weblog.bocoup.com/)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](http://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://net.tutsplus.com/?s=javascript)

  **[[⬆]](#TOC)**

## <a name='in-the-wild'>В реальных условиях</a>

  Это список организаций, которые используют это руководство по стилю. Отправьте запрос и мы добавим вас в список.

  - **Aan Zee**: [AanZee/javascript](https://github.com/AanZee/javascript)
  - **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - **American Insitutes for Research**: [AIRAST/javascript](https://github.com/AIRAST/javascript)
  - **Compass Learning**: [compasslearning/javascript-style-guide](https://github.com/compasslearning/javascript-style-guide)
  - **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
  - **Gawker Media**: [gawkermedia/javascript](https://github.com/gawkermedia/javascript)
  - **GeneralElectric**: [GeneralElectric/javascript](https://github.com/GeneralElectric/javascript)
  - **GoodData**: [gooddata/gdc-js-style](https://github.com/gooddata/gdc-js-style)
  - **Grooveshark**: [grooveshark/javascript](https://github.com/grooveshark/javascript)
  - **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript)
  - **Mighty Spring**: [mightyspring/javascript](https://github.com/mightyspring/javascript)
  - **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
  - **ModCloth**: [modcloth/javascript](https://github.com/modcloth/javascript)
  - **National Geographic**: [natgeo/javascript](https://github.com/natgeo/javascript)
  - **National Park Service**: [nationalparkservice/javascript](https://github.com/nationalparkservice/javascript)
  - **Razorfish**: [razorfish/javascript-style-guide](https://github.com/razorfish/javascript-style-guide)
  - **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)
  - **Userify**: [userify/javascript](https://github.com/userify/javascript)
  - **Zillow**: [zillow/javascript](https://github.com/zillow/javascript)
  - **ZocDoc**: [ZocDoc/javascript](https://github.com/ZocDoc/javascript)

## <a name='translation'>Перевод</a>

  Это руководство доступно на других языках:

  - :de: **German**: [timofurrer/javascript-style-guide](https://github.com/timofurrer/javascript-style-guide)
  - :jp: **Japanese**: [mitsuruog/javacript-style-guide](https://github.com/mitsuruog/javacript-style-guide)
  - :br: **Portuguese**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)
  - :cn: **Chinese**: [adamlu/javascript-style-guide](https://github.com/adamlu/javascript-style-guide)
  - :es: **Spanish**: [paolocarrasco/javascript-style-guide](https://github.com/paolocarrasco/javascript-style-guide)
  - :kr: **Korean**: [tipjs/javascript-style-guide](https://github.com/tipjs/javascript-style-guide)

## <a name='guide-guide'>Руководство к Руководству по Стилю JavaScript</a>

  - [Reference](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## <a name='authors'>Авторы</a>

  - [View Contributors](https://github.com/airbnb/javascript/graphs/contributors)


## <a name='license'>Лицензия</a>

(The MIT License)

Copyright (c) 2012 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[[⬆]](#TOC)**

# };

