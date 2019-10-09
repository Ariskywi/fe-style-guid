# 前端开发 JavaScript 风格指南

*JavaScript的最佳实践*

> **注意**: 本指南假设项目正在使用[Babel](https://babeljs.io)  
> 基于`airbnb`的`JavaScript Style Guid`翻译修改

其他风格指南

  - [CSS & Sass](css/)
  - [React](react/)
  - [CSS-in-JavaScript](css-in-javascript/)

## 目录

  1. [类型](#类型)
  1. [引用](#引用)
  1. [对象](#对象)
  1. [数组](#数组)
  1. [解构](#解构)
  1. [字符串](#字符串)
  1. [函数](#函数)
  1. [箭头函数](#箭头函数)
  1. [类与构造函数](#类与构造函数)
  1. [模块](#模块)
  1. [迭代器与生成函数](#迭代器与生成函数)
  1. [属性](#属性)
  1. [变量](#变量)
  1. [作用域提升](#作用域提升)
  1. [比较运算符与等号](#比较运算符与等号)
  1. [代码块](#代码块)
  1. [控制声明](#控制声明)
  1. [注释](#注释)
  1. [留白](#留白)
  1. [逗号](#逗号)
  1. [分号](#分号)
  1. [强制类型转换](#强制类型转换)
  1. [命名规则](#命名规范)
  1. [访问器函数](#访问器函数)
  1. [事件](#事件)
  1. [jQuery](#juery)
  1. [ECMAScript-5-兼容性](#ECMAScript-5-兼容性)
  1. [ECMAScript-6+(ES-2015+)编码规范](#ECMAScript-6+(ES-2015+)编码规范)
  1. [标准库](#标准库)
  1. [测试](#测试)
  1. [License](#License)
  1. [修订](#修订)

## 类型

  <a name="types--primitives"></a><a name="1.1"></a>
  - [1.1](#types--primitives) **基本类型**: 直接存取基本数据类型

    - `string`
    - `number`
    - `boolean`
    - `null`
    - `undefined`
    - `symbol`

    ```javascript
    const foo = 1;
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```

    - Symbols 不能被正确的polyfill。 所以在不能原生支持symbol类型的环境[浏览器]中，不应该使用 symbol 类型。

  <a name="types--complex"></a><a name="1.2"></a>
  - [1.2](#types--complex)  **复合类型**: 通过引用的方式存取复合类型。

    - `object`
    - `array`
    - `function`

    ```javascript
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ 回到顶部](#目录)**

## 引用

  <a name="references--prefer-const"></a><a name="2.1"></a>
  - [2.1](#references--prefer-const) 所有的赋值都用const，避免使用var. eslint: [`prefer-const`](https://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](https://eslint.org/docs/rules/no-const-assign.html)

    > 原因： 这将保证不会改变初始值,重复引用会导致bug，并且代码难以理解。

    ```javascript
    // 反例
    var a = 1;
    var b = 2;

    // 正确示例
    const a = 1;
    const b = 2;
    ```

  <a name="references--disallow-var"></a><a name="2.2"></a>
  - [2.2](#references--disallow-var) 如果一定要对参数重新赋值，那就用let，而不是var。 eslint: [`no-var`](https://eslint.org/docs/rules/no-var.html)

    > 因为let是块级作用域，而var是函数级作用域

    ```javascript
    // 反例
    var count = 1;
    if (true) {
      count += 1;
    }

    // 正确示例, use the let.
    let count = 1;
    if (true) {
      count += 1;
    }
    ```

  <a name="references--block-scope"></a><a name="2.3"></a>
  - [2.3](#references--block-scope) 注意 `let` 和 `const` 都是块级作用域。

    ```javascript
    // const 和 let 仅作用于它们定义的块内
    {
      let a = 1;
      const b = 1;
    }
    console.log(a); // 引用错误
    console.log(b); // 引用错误
    ```

**[⬆ 回到目录](#目录)**

## 对象

  <a name="objects--no-new"></a><a name="3.1"></a>
  - [3.1](#objects--no-new)使用字面量创建对象。 eslint: [`no-new-object`](https://eslint.org/docs/rules/no-new-object.html)

    ```javascript
    // 反例
    const item = new Object();

    // 正确示例
    const item = {};
    ```

  <a name="es6-computed-properties"></a><a name="3.4"></a>
  - [3.2](#es6-computed-properties) 当创建一个带有动态属性名的对象时，使用用可被计算的属性名

    > 这样可以在一个地方定义所有的对象属性。

    ```javascript

    function getKey(k) {
      return `a key named ${k}`;
    }

    // 反例
    const obj = {
      id: 5,
      name: 'San Francisco',
    };
    obj[getKey('enabled')] = true;

    // 正确示例
    const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true,
    };
    ```

  <a name="es6-object-shorthand"></a><a name="3.5"></a>
  - [3.3](#es6-object-shorthand) 使用简写定义对象方法。 eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

    ```javascript
    // 反例
    const atom = {
      value: 1,

      addValue: function (value) {
        return atom.value + value;
      },
    };

    // 正确示例
    const atom = {
      value: 1,

      addValue(value) {
        return atom.value + value;
      },
    };
    ```

  <a name="es6-object-concise"></a><a name="3.6"></a>
  - [3.4](#es6-object-concise) 使用简写定义对象属性。 eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

    > 这样更短，描述性更强。

    ```javascript
    const lukeSkywalker = 'Luke Skywalker';

    // 反例
    const obj = {
      lukeSkywalker: lukeSkywalker,
    };

    // 正确示例
    const obj = {
      lukeSkywalker,
    };
    ```

  <a name="objects--grouped-shorthand"></a><a name="3.7"></a>
  - [3.5](#objects--grouped-shorthand) 在声明对象属性时，将简写的属性分组。

    > 这样可以清晰的看出哪些属性使用了简写。

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // 反例
    const obj = {
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      lukeSkywalker,
      episodeThree: 3,
      mayTheFourth: 4,
      anakinSkywalker,
    };

    // 正确示例
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4,
    };
    ```

  <a name="objects--quoted-props"></a><a name="3.8"></a>
  - [3.6](#objects--quoted-props) 只对无效的属性标识使用引号。 eslint: [`quote-props`](https://eslint.org/docs/rules/quote-props.html)

    > 这种方式主观上更易读，会被代码高亮优化，并且更容易被许多JS引擎压缩。

    ```javascript
    // 反例
    const bad = {
      'foo': 3,
      'bar': 4,
      'data-blah': 5,
    };

    // 正确示例
    const good = {
      foo: 3,
      bar: 4,
      'data-blah': 5,
    };
    ```

  <a name="objects--prototype-builtins"></a>
  - [3.7](#objects--prototype-builtins) 不要直接调用 `Object.prototype` 上的方法，例如 `hasOwnProperty`, `propertyIsEnumerable` 和 `isPrototypeOf`。 eslint: [`no-prototype-builtins`](https://eslint.org/docs/rules/no-prototype-builtins)

    > 这些方法在某些对象上可能会被屏蔽掉。 例如： `{ hasOwnProperty: false }` - 或是一个空对象 (`Object.create(null)`).

    ```javascript
    // 反例
    console.log(object.hasOwnProperty(key));

    // 正确示例
    console.log(Object.prototype.hasOwnProperty.call(object, key));

    // best
    const has = Object.prototype.hasOwnProperty; // cache the lookup once, in module scope.
    console.log(has.call(object, key));
    /* or */
    import has from 'has'; // https://www.npmjs.com/package/has
    console.log(has(object, key));
    ```

  <a name="objects--rest-spread"></a>
  - [3.8](#objects--rest-spread) 对象浅拷贝时，更推荐使用扩展运算符`...` 而不是[`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 获取对象指定的几个属性时，用对象的解构运算符也更好

    ```javascript
    // very bad
    const original = { a: 1, b: 2 };
    const copy = Object.assign(original, { c: 3 }); // 这改变了 `original` ಠ_ಠ
    delete copy.a; // 同样的改变了`original`

    // 反例
    const original = { a: 1, b: 2 };
    const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

    // 正确示例
    const original = { a: 1, b: 2 };
    const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

    const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
    ```

**[⬆ 回到目录](#目录)**

## 数组

  <a name="arrays--literals"></a><a name="4.1"></a>
  - [4.1](#arrays--literals) 使用字面量创建数组。 eslint: [`no-array-constructor`](https://eslint.org/docs/rules/no-array-constructor.html)

    ```javascript
    // 反例
    const items = new Array();

    // 正确示例
    const items = [];
    ```

  <a name="arrays--push"></a><a name="4.2"></a>
  - [4.2](#arrays--push) 向数组添加元素时使用[Array#push](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/push)替代直接赋值。

    ```javascript
    const someStack = [];

    // 反例
    someStack[someStack.length] = 'abracadabra';

    // 正确示例
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a><a name="4.3"></a>
  - [4.3](#es6-array-spreads) 使用拓展运算符 `...` 复制数组。

    ```javascript
    // 反例
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i += 1) {
      itemsCopy[i] = items[i];
    }

    // 正确示例
    const itemsCopy = [...items];
    ```

  <a name="arrays--from"></a>
  <a name="arrays--from-iterable"></a><a name="4.4"></a>
  - [4.4](#arrays--from-iterable) 使用拓展运算符 `...` 代替[`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) 将一个可迭代对象转换为数组。

    ```javascript
    const foo = document.querySelectorAll('.foo');

    // 正确示例
    const nodes = Array.from(foo);

    // best
    const nodes = [...foo];
    ```

  <a name="arrays--from-array-like"></a>
  - [4.5](#arrays--from-array-like) 使用 [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) 将一个类数组对象转换为数组。

    ```javascript
    const arrLike = { 0: 'foo', 1: 'bar', 2: 'baz', length: 3 };

    // 反例
    const arr = Array.prototype.slice.call(arrLike);

    // 正确示例
    const arr = Array.from(arrLike);
    ```

  <a name="arrays--mapping"></a>
  - [4.6](#arrays--mapping) 使用[`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) 而不是拓展运算符 `...` 来做map遍历, 这样可以避免创建一个临时数组。

    ```javascript
    // 反例
    const baz = [...foo].map(bar);

    // 正确示例
    const baz = Array.from(foo, bar);
    ```

  <a name="arrays--callback-return"></a><a name="4.5"></a>
  - [4.7](#arrays--callback-return) 在数组方法的回调函数中使用return语句。 如果函数体返回是一条没有副作用的表达式语句，那么可以忽略return语句。 详见 [8.2](#arrows--implicit-return). eslint: [`array-callback-return`](https://eslint.org/docs/rules/array-callback-return)

    ```javascript
    // 正确示例
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });

    // 正确示例
    [1, 2, 3].map((x) => x + 1);

    // 反例 - 没有返回值，意味着 `acc` 将在首次迭代后变为undefined
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
    });

    // 正确示例
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
      return flatten;
    });

    // 反例
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      } else {
        return false;
      }
    });

    // 正确示例
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      }

      return false;
    });
    ```

  <a name="arrays--bracket-newline"></a>
  - [4.8](#arrays--bracket-newline) 如果数组有很多行，在数组的起始括号`[`后和结束括号`]`前断行。

    ```javascript
    // 反例
    const arr = [
      [0, 1], [2, 3], [4, 5],
    ];

    const objectInArray = [{
      id: 1,
    }, {
      id: 2,
    }];

    const numberInArray = [
      1, 2,
    ];

    // 正确示例
    const arr = [[0, 1], [2, 3], [4, 5]];

    const objectInArray = [
      {
        id: 1,
      },
      {
        id: 2,
      },
    ];

    const numberInArray = [
      1,
      2,
    ];
    ```

**[⬆ 回到目录](#目录)**

## 解构

  <a name="destructuring--object"></a><a name="5.1"></a>
  - [5.1](#destructuring--object) 使用解构来存取和使用对象的多个属性值。 eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

    > Why? Destructuring saves you from creating temporary references for those properties.

    ```javascript
    // 反例
    function getFullName(user) {
      const firstName = user.firstName;
      const lastName = user.lastName;

      return `${firstName} ${lastName}`;
    }

    // 正确示例
    function getFullName(user) {
      const { firstName, lastName } = user;
      return `${firstName} ${lastName}`;
    }

    // best
    function getFullName({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
    }
    ```

  <a name="destructuring--array"></a><a name="5.2"></a>
  - [5.2](#destructuring--array) 使用数组解构。 eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

    ```javascript
    const arr = [1, 2, 3, 4];

    // 反例
    const first = arr[0];
    const second = arr[1];

    // 正确示例
    const [first, second] = arr;
    ```

  <a name="destructuring--object-over-array"></a><a name="5.3"></a>
  - [5.3](#destructuring--object-over-array) 多返回值时使用对象解构，而不是数组解构。

    > 因为可以在后期添加新的属性或改变顺序而不影响调用。

    ```javascript
    // 反例
    function processInput(input) {
      // then a miracle occurs
      return [left, right, top, bottom];
    }

    // 调用者需要考虑返回值的顺序
    const [left, __, top] = processInput(input);

    // 正确示例
    function processInput(input) {
      // 见证奇迹的时刻
      return { left, right, top, bottom };
    }

    // 调用者只需要选择需要使用的值即可
    const { left, top } = processInput(input);
    ```

**[⬆ 回到目录](#目录)**

## 字符串

  <a name="strings--quotes"></a><a name="6.1"></a>
  - [6.1](#strings--quotes) 对string使用单引号 `''` eslint: [`quotes`](https://eslint.org/docs/rules/quotes.html)

    ```javascript
    // 反例
    const name = "Capt. Janeway";

    // 反例 - template literals should contain interpolation or newlines
    const name = `Capt. Janeway`;

    // 正确示例
    const name = 'Capt. Janeway';
    ```

  <a name="strings--line-length"></a><a name="6.2"></a>
  - [6.2](#strings--line-length) 超过100个字符的字符串不应该用string串联成多行。

    > 被断行的字符串工作体验差并且不易被搜索到。

    ```javascript
    // 反例
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // 反例
    const errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';

    // 正确示例
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
    ```

  <a name="es6-template-literals"></a><a name="6.4"></a>
  - [6.3](#es6-template-literals) 使用字符串模板而不是字符串拼接来组织可变的字符串。 eslint: [`prefer-template`](https://eslint.org/docs/rules/prefer-template.html) [`template-curly-spacing`](https://eslint.org/docs/rules/template-curly-spacing)

    > Why? Template strings give you a readable, concise syntax with proper newlines and string interpolation features.

    ```javascript
    // 反例
    function sayHi(name) {
      return 'How are you, ' + name + '?';
    }

    // 反例
    function sayHi(name) {
      return ['How are you, ', name, '?'].join();
    }

    // 反例
    function sayHi(name) {
      return `How are you, ${ name }?`;
    }

    // 正确示例
    function sayHi(name) {
      return `How are you, ${name}?`;
    }
    ```

  <a name="strings--eval"></a><a name="6.5"></a>
  - [6.4](#strings--eval) 不要在字符串中使用 `eval()` , 这就是一个潘多拉魔盒。 eslint: [`no-eval`](https://eslint.org/docs/rules/no-eval)

  <a name="strings--escaping"></a>
  - [6.5](#strings--escaping) 不要使用不必要的转义字符。 eslint: [`no-useless-escape`](https://eslint.org/docs/rules/no-useless-escape)

    > 反斜杠可读性差，应该只在需要时出现。

    ```javascript
    // 反例
    const foo = '\'this\' \i\s \"quoted\"';

    // 正确示例
    const foo = '\'this\' is "quoted"';
    const foo = `my name is '${name}'`;
    ```

**[⬆ 回到目录](#目录)**

## 函数

  <a name="functions--declarations"></a><a name="7.1"></a>
  - [7.1](#functions--declarations) 使用命名函数表达式而不是函数声明。 eslint: [`func-style`](https://eslint.org/docs/rules/func-style)

    > 函数声明提升，意味着在文件中非常容易在定义函数前就使用函数的引用。 这对可读性和可维护性都有损害。如果函数足够大或复杂，以至于影响到了对上下文其他内容的理解, 这可能是时候将这个函数单独抽成一个模块了! 不要忘记显示命名表达式, 无论该名字是否可以从包含的变量推断出(在现代浏览器或使用Babel等编译器时，经常会出现这种情况)。这消除了由错误调用栈产生的所有假设。  ([Discussion](https://github.com/airbnb/javascript/issues/794))

    ```javascript
    // 反例
    function foo() {
      // ...
    }

    // 反例
    const foo = function () {
      // ...
    };

    // 正确示例
    // 函数表达式名和声明的函数名不一样
    const short = function longUniqueMoreDescriptiveLexicalFoo() {
      // ...
    };
    ```

  <a name="functions--iife"></a><a name="7.2"></a>
  - [7.2](#functions--iife) 使用圆括号包裹立即执行函数。 eslint: [`wrap-iife`](https://eslint.org/docs/rules/wrap-iife.html)

    > 因为一个立即调用函数表达式是一个单元 - 使用圆括号将它和它的调用者包裹起来，清晰的表达出了这些。注意：在模块化世界里，几乎用不到IIFE(立即执行函数)。

    ```javascript
    // immediately-invoked function expression (IIFE)
    (function () {
      console.log('Welcome to the Internet. Please follow me.');
    }());
    ```

  <a name="functions--in-blocks"></a><a name="7.3"></a>
  - [7.3](#functions--in-blocks) 不要在非函数模块内声明函数(`if`、 `while` 等)。而是将函数分配给一个变量。一个糟糕的信息是：浏览器允许这样做(在非函数模块内声明函数)，但是不同的浏览器对此解析不同。 eslint: [`no-loop-func`](https://eslint.org/docs/rules/no-loop-func.html)

  <a name="functions--note-on-blocks"></a><a name="7.4"></a>
  - [7.4](#functions--note-on-blocks) **注意:** ECMA-262 对模块 `block` 的定义是一系列的语句。 但函数声明不是一个语句。函数表达式是一个语句。

    ```javascript
    // 反例
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // 正确示例
    let test;
    if (currentUser) {
      test = () => {
        console.log('Yup.');
      };
    }
    ```

  <a name="functions--arguments-shadow"></a><a name="7.5"></a>
  - [7.5](#functions--arguments-shadow) 不要用 `arguments`命名参数，这会导致函数自带的`arguments` 值被覆盖。因为参数arguments的优先级高于每个函数作用域自带的arguments对象。

    ```javascript
    // 反例
    function foo(name, options, arguments) {
      // ...
    }

    // 正确示例
    function foo(name, options, args) {
      // ...
    }
    ```

  <a name="es6-rest"></a><a name="7.6"></a>
  - [7.6](#es6-rest) 不要使用 `arguments`使用解构`...` 语法代替。 eslint: [`prefer-rest-params`](https://eslint.org/docs/rules/prefer-rest-params)

    > 因为`...`是所需参数的明确表示。另外，rest参数是真正的数组而不是类数组的`arguments`.

    ```javascript
    // 反例
    function concatenateAll() {
      const args = Array.prototype.slice.call(arguments);
      return args.join('');
    }

    // 正确示例
    function concatenateAll(...args) {
      return args.join('');
    }
    ```

  <a name="es6-default-parameters"></a><a name="7.7"></a>
  - [7.7](#es6-default-parameters) 使用参数默认值语法，而不是在函数里对参数重新赋值。

    ```javascript
    // 特别糟糕的做法
    function handleThings(opts) {
      // 错误一: 不应该修改arguments
      // 错误二: 如果opts的值为false，它会被赋值为{}
      // 这可能会带来一些bug
      opts = opts || {};
      // ...
    }

    // 仍然错误的做法
    function handleThings(opts) {
      if (opts === void 0) {
        opts = {};
      }
      // ...
    }

    // 正确示例
    function handleThings(opts = {}) {
      // ...
    }
    ```

  <a name="functions--default-side-effects"></a><a name="7.8"></a>
  - [7.8](#functions--default-side-effects) 默认参数避免产生副作用。

    > 因为如果产生副作用，会令人迷惑。如下所示：a到底等于多少？

    ```javascript
    var b = 1;
    // 反例
    function count(a = b++) {
      console.log(a);
    }
    count();  // 1
    count();  // 2
    count(3); // 3
    count();  // 3
    ```

  <a name="functions--defaults-last"></a><a name="7.9"></a>
  - [7.9](#functions--defaults-last) 将默认参数赋值放在最后。

    ```javascript
    // 反例
    function handleThings(opts = {}, name) {
      // ...
    }

    // 正确示例
    function handleThings(name, opts = {}) {
      // ...
    }
    ```

  <a name="functions--constructor"></a><a name="7.10"></a>
  - [7.10](#functions--constructor) 不要使用Function类创建函数。 eslint: [`no-new-func`](https://eslint.org/docs/rules/no-new-func)

    > 因为此种方式创建函数类似于 `eval()`, 会导致漏洞。

    ```javascript
    // 反例
    var add = new Function('a', 'b', 'return a + b');

    // 仍然错误
    var subtract = Function('a', 'b', 'return a - b');
    ```

  <a name="functions--signature-spacing"></a><a name="7.11"></a>
  - [7.11](#functions--signature-spacing) 函数签名要有空格。 eslint: [`space-before-function-paren`](https://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks)

    > 这样统一性好，而且在添加/删除一个名字的时候不需要添加/删除空格。(使用prettier配置即可)

    ```javascript
    // 反例
    const f = function(){};
    const g = function (){};
    const h = function() {};

    // 正确示例
    const x = function () {};
    const y = function a() {};
    ```

  <a name="functions--mutate-params"></a><a name="7.12"></a>
  - [7.12](#functions--mutate-params) 不要修改参数。 eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign.html)

    > 因为修改参数对象会对调用者产生无法预料的副作用(引用类型)。

    ```javascript
    // 反例
    function f1(obj) {
      obj.key = 1;
    }

    // 正确示例
    function f2(obj) {
      const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1;
    }
    ```

  <a name="functions--reassign-params"></a><a name="7.13"></a>
  - [7.13](#functions--reassign-params) 不要重新赋值参数。 eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign.html)

    > 因为重新赋值会导致意外行为，尤其是访问 `arguments` 对象时。这还会产生优化问题，尤其是在v8中。

    ```javascript
    // 反例
    function f1(a) {
      a = 1;
      // ...
    }

    function f2(a) {
      if (!a) { a = 1; }
      // ...
    }

    // 正确示例
    function f3(a) {
      const b = a || 1;
      // ...
    }

    function f4(a = 1) {
      // ...
    }
    ```

  <a name="functions--spread-vs-apply"></a><a name="7.14"></a>
  - [7.14](#functions--spread-vs-apply) 使用拓展运算符`...` 来调用参数可变的函数。 eslint: [`prefer-spread`](https://eslint.org/docs/rules/prefer-spread)

    > 因为这样更清晰，不比提供上下文。另外也难以将 `new` 和 `apply`组合使用。 

    ```javascript
    // 反例
    const x = [1, 2, 3, 4, 5];
    console.log.apply(console, x);

    // 正确示例
    const x = [1, 2, 3, 4, 5];
    console.log(...x);

    // 反例
    new (Function.prototype.bind.apply(Date, [null, 2016, 8, 5]));

    // 正确示例
    new Date(...[2016, 8, 5]);
    ```

  <a name="functions--signature-invocation-indentation"></a>
  - [7.15](#functions--signature-invocation-indentation) 声明或调用一个包含多个参数的函数应该像本指南中其他多行代码写法一样：每行一项，以逗号结尾。 eslint: [`function-paren-newline`](https://eslint.org/docs/rules/function-paren-newline)

    ```javascript
    // 反例
    function foo(bar,
                 baz,
                 quux) {
      // ...
    }

    // 正确示例
    function foo(
      bar,
      baz,
      quux,
    ) {
      // ...
    }

    // 反例
    console.log(foo,
      bar,
      baz);

    // 正确示例
    console.log(
      foo,
      bar,
      baz,
    );
    ```

**[⬆ 回到目录](#目录)**

## 箭头函数

  <a name="arrows--use-them"></a><a name="8.1"></a>
  - [8.1](#arrows--use-them) 当需要使用匿名函数时 (譬如传递一个行内回调), 使用箭头函数语法。 eslint: [`prefer-arrow-callback`](https://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](https://eslint.org/docs/rules/arrow-spacing.html)

    > 因为此方式创建了函数的关于当前`this`上下文的副本,这通常就是想要的。而且箭头函数是更简洁的语法。

    > 什么时候不用? 如果有一个复杂函数，可以将此逻辑移到命名函数表达式里。

    ```javascript
    // 反例
    [1, 2, 3].map(function (x) {
      const y = x + 1;
      return x * y;
    });

    // 正确示例
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  <a name="arrows--implicit-return"></a><a name="8.2"></a>
  - [8.2](#arrows--implicit-return) 如果函数体是由没有副作用的[表达式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions)语句组成, 删除大括号和return. 否则，保持大括号和 `return` 语句。eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html), [`arrow-body-style`](https://eslint.org/docs/rules/arrow-body-style.html)

    > 语法糖，当多个函数组成函数链时易于阅读。

    ```javascript
    // 反例
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      `A string containing the ${nextNumber}.`;
    });

    // 正确示例
    [1, 2, 3].map((number) => `A string containing the ${number + 1}.`);

    // 正确示例
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      return `A string containing the ${nextNumber}.`;
    });

    // 正确示例
    [1, 2, 3].map((number, index) => ({
      [index]: number,
    }));

    // No implicit return with side effects
    function foo(callback) {
      const val = callback();
      if (val === true) {
        // Do something if callback returns true
      }
    }

    let bool = false;

    // 反例
    foo(() => bool = true);

    // 正确示例
    foo(() => {
      bool = true;
    });
    ```

  <a name="arrows--paren-wrap"></a><a name="8.3"></a>
  - [8.3](#arrows--paren-wrap) 如果是多行表达式, 将表达式包裹在圆括号中可读性更好。

    > 因为函数的起始很清晰。

    ```javascript
    // 反例
    ['get', 'post', 'put'].map((httpMethod) => Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
      )
    );

    // 正确示例
    ['get', 'post', 'put'].map((httpMethod) => (
      Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
      )
    ));
    ```

  <a name="arrows--one-arg-parens"></a><a name="8.4"></a>
  - [8.4](#arrows--one-arg-parens) 如果函数只有一个参数并且函数体没有大括号，就删除圆括号。否则，参数总是放在圆括号里。注意：一直使用圆括号也没有问题，只需要配置eslint的always 选项。eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html)

    > 一种风格，可以使添加/删除参数时最小化差异。

    ```javascript
    // 反例
    [1, 2, 3].map(x => x * x);

    // 正确示例
    [1, 2, 3].map((x) => x * x);

    // 反例
    [1, 2, 3].map(number => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ));

    // 正确示例
    [1, 2, 3].map((number) => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ));

    // 反例
    [1, 2, 3].map(x => {
      const y = x + 1;
      return x * y;
    });

    // 正确示例
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  <a name="arrows--confusing"></a><a name="8.5"></a>
  - [8.5](#arrows--confusing) 不要将箭头函数语法 (`=>`) 与比较操作符 (`<=`, `>=`)混淆。 eslint: [`no-confusing-arrow`](https://eslint.org/docs/rules/no-confusing-arrow)

    ```javascript
    // 反例
    const itemHeight = (item) => item.height <= 256 ? item.largeSize : item.smallSize;

    // 反例
    const itemHeight = (item) => item.height >= 256 ? item.largeSize : item.smallSize;

    // 正确示例
    const itemHeight = (item) => (item.height <= 256 ? item.largeSize : item.smallSize);

    // 正确示例
    const itemHeight = (item) => {
      const { height, largeSize, smallSize } = item;
      return height <= 256 ? largeSize : smallSize;
    };
    ```

  <a name="whitespace--implicit-arrow-linebreak"></a>
  - [8.6](#whitespace--implicit-arrow-linebreak) 在隐式return中强制约束函数体的位置。 eslint: [`implicit-arrow-linebreak`](https://eslint.org/docs/rules/implicit-arrow-linebreak)

    ```javascript
    // 反例
    (foo) =>
      bar;

    (foo) =>
      (bar);

    // 正确示例
    (foo) => bar;
    (foo) => (bar);
    (foo) => (
       bar
    )
    ```

**[⬆ 回到目录](#目录)**

## 类与构造函数

  <a name="constructors--use-class"></a><a name="9.1"></a>
  - [9.1](#constructors--use-class) 总是使用 `class`。避免直接操作 `prototype` 。

    > 因为`class`语法更简洁易读。

    ```javascript
    // 反例
    function Queue(contents = []) {
      this.queue = [...contents];
    }
    Queue.prototype.pop = function () {
      const value = this.queue[0];
      this.queue.splice(0, 1);
      return value;
    };

    // 正确示例
    class Queue {
      constructor(contents = []) {
        this.queue = [...contents];
      }
      pop() {
        const value = this.queue[0];
        this.queue.splice(0, 1);
        return value;
      }
    }
    ```

  <a name="constructors--extends"></a><a name="9.2"></a>
  - [9.2](#constructors--extends) 使用 `extends` 来继承。

    > 因为这是一个内置的原型继承方法并且不会破坏 `instanceof`。

    ```javascript
    // 反例
    const inherits = require('inherits');
    function PeekableQueue(contents) {
      Queue.apply(this, contents);
    }
    inherits(PeekableQueue, Queue);
    PeekableQueue.prototype.peek = function () {
      return this.queue[0];
    };

    // 正确示例
    class PeekableQueue extends Queue {
      peek() {
        return this.queue[0];
      }
    }
    ```

  <a name="constructors--chaining"></a><a name="9.3"></a>
  - [9.3](#constructors--chaining) 方法可以返回`this` 来实现链式调用。

    ```javascript
    // 反例
    Jedi.prototype.jump = function () {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function (height) {
      this.height = height;
    };

    const luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // 正确示例
    class Jedi {
      jump() {
        this.jumping = true;
        return this;
      }

      setHeight(height) {
        this.height = height;
        return this;
      }
    }

    const luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```

  <a name="constructors--tostring"></a><a name="9.4"></a>
  - [9.4](#constructors--tostring) 可以自定义`toString()` 方法, 但是要保证它能正确执行并且不产生副作用。 

    ```javascript
    class Jedi {
      constructor(options = {}) {
        this.name = options.name || 'no name';
      }

      getName() {
        return this.name;
      }

      toString() {
        return `Jedi - ${this.getName()}`;
      }
    }
    ```

  <a name="constructors--no-useless"></a><a name="9.5"></a>
  - [9.5](#constructors--no-useless) 如果没有指定，类有默认的构造方法。不需要写空的构造函数或仅代表父类的构造函数。 eslint: [`no-useless-constructor`](https://eslint.org/docs/rules/no-useless-constructor)

    ```javascript
    // 反例
    class Jedi {
      constructor() {}

      getName() {
        return this.name;
      }
    }

    // 反例
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
      }
    }

    // 正确示例
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
        this.name = 'Rey';
      }
    }
    ```

  <a name="classes--no-duplicate-members"></a>
  - [9.6](#classes--no-duplicate-members) 避免重复的类。eslint: [`no-dupe-class-members`](https://eslint.org/docs/rules/no-dupe-class-members)

    > 因为重复的类只会执行最后一个。———— 重复本身就是一个bug。

    ```javascript
    // 反例
    class Foo {
      bar() { return 1; }
      bar() { return 2; }
    }

    // 正确示例
    class Foo {
      bar() { return 1; }
    }

    // 正确示例
    class Foo {
      bar() { return 2; }
    }
    ```

**[⬆ 回到目录](#目录)**

## 模块

  <a name="modules--use-them"></a><a name="10.1"></a>
  - [10.1](#modules--use-them) 使用 (`import`/`export`) 模块而不是非标准模块系统。这样可以随时转到喜欢的模块系统。

    > 模块化是未来，那么让我们现在就开启未来吧。

    ```javascript
    // 反例
    const AirbnbStyleGuide = require('./AirbnbStyleGuide');
    module.exports = AirbnbStyleGuide.es6;

    // ok
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    export default AirbnbStyleGuide.es6;

    // best
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  <a name="modules--no-wildcard"></a><a name="10.2"></a>
  - [10.2](#modules--no-wildcard) 不要使用import通配符(*)。

    > 因为这需要确保每个模块都有一个默认的导出。

    ```javascript
    // 反例
    import * as AirbnbStyleGuide from './AirbnbStyleGuide';

    // 正确示例
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    ```

  <a name="modules--no-export-from-import"></a><a name="10.3"></a>
  - [10.3](#modules--no-export-from-import) 不要直接从import中export

    > 虽然一行很简洁，但是需要有一个明确的入口和一个明确的出口来保持一致性。

    ```javascript
    // 反例
    // 文件名 es6.js
    export { es6 as default } from './AirbnbStyleGuide';

    // 正确示例
    // 文件名 es6.js
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  <a name="modules--no-duplicate-imports"></a>
  - [10.4](#modules--no-duplicate-imports) 一个路径只import一次。
 eslint: [`no-duplicate-imports`](https://eslint.org/docs/rules/no-duplicate-imports)
    > 因为从同一个路径下import多行会使代码难以维护。

    ```javascript
    // 反例
    import foo from 'foo';
    // … some other imports … //
    import { named1, named2 } from 'foo';

    // 正确示例
    import foo, { named1, named2 } from 'foo';

    // 正确示例
    import foo, {
      named1,
      named2,
    } from 'foo';
    ```

  <a name="modules--no-mutable-exports"></a>
  - [10.5](#modules--no-mutable-exports) 不要导出可变的引用。
 eslint: [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)
    > 模块通常应该避免变动，尤其是在导出可变的引用时。尽管在某些特殊情况下可能需要使用此技术，但通常只应导出常量引用。

    ```javascript
    // 反例
    let foo = 3;
    export { foo };

    // 正确示例
    const foo = 3;
    export { foo };
    ```

  <a name="modules--prefer-default-export"></a>
  - [10.6](#modules--prefer-default-export) 在单一导出的模块里，使用export default 更好。
 eslint: [`import/prefer-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)
    > 鼓励使用多个文件，每个文件只做一件事情并导出，这样可读性和可维护性更好。

    ```javascript
    // 反例
    export function foo() {}

    // 正确示例
    export default function foo() {}
    ```

  <a name="modules--imports-first"></a>
  - [10.7](#modules--imports-first) `import`放在所有其他语句之前。
 eslint: [`import/first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)
    > 因为`import`有作用域提升，将其放在最前面防止发生不可控行为。

    ```javascript
    // 反例
    import foo from 'foo';
    foo.init();

    import bar from 'bar';

    // 正确示例
    import foo from 'foo';
    import bar from 'bar';

    foo.init();
    ```

  <a name="modules--multiline-imports-over-newlines"></a>
  - [10.8](#modules--multiline-imports-over-newlines) 多行import应缩进，就像多行数组和对象字面量一样。

    > 大括号应与本指南中其他大括号模块遵循同样的缩进规则, 末尾的逗号同样如此。

    ```javascript
    // 反例
    import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path';

    // 正确示例
    import {
      longNameA,
      longNameB,
      longNameC,
      longNameD,
      longNameE,
    } from 'path';
    ```

  <a name="modules--no-webpack-loader-syntax"></a>
  - [10.9](#modules--no-webpack-loader-syntax) import语句中不允许使用webpack loader语法。
 eslint: [`import/no-webpack-loader-syntax`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-webpack-loader-syntax.md)
    > 因为如果在import中使用webpack语法，就会使模块打包与代码耦合。 在`webpack.config.js`中配置loader语法会更好。

    ```javascript
    // 反例
    import fooSass from 'css!sass!foo.scss';
    import barCss from 'style!css!bar.css';

    // 正确示例
    import fooSass from 'foo.scss';
    import barCss from 'bar.css';
    ```

**[⬆ 回到目录](#目录)**

## 迭代器与生成函数

  <a name="iterators--nope"></a><a name="11.1"></a>
  - [11.1](#iterators--nope) 不要使用迭代器，使用JavaScript的高级函数来代替`for-in` 或 `for-of`。 eslint: [`no-iterator`](https://eslint.org/docs/rules/no-iterator.html) [`no-restricted-syntax`](https://eslint.org/docs/rules/no-restricted-syntax)

    > 这强调了不可变原则。 Dealing with pure functions that return values is easier to reason about than side effects.

    > 使用迭代方法 `map()` / `every()` / `filter()` / `find()` / `findIndex()` / `reduce()` / `some()` /遍历数组， 使用对象的这些方法 `Object.keys()` / `Object.values()` / `Object.entries()`生成数组，这样就可以遍历对象了。

    ```javascript
    const numbers = [1, 2, 3, 4, 5];

    // 反例
    let sum = 0;
    for (let num of numbers) {
      sum += num;
    }
    sum === 15;

    // 正确示例
    let sum = 0;
    numbers.forEach((num) => {
      sum += num;
    });
    sum === 15;

    // best (use the functional force)
    const sum = numbers.reduce((total, num) => total + num, 0);
    sum === 15;

    // 反例
    const increasedByOne = [];
    for (let i = 0; i < numbers.length; i++) {
      increasedByOne.push(numbers[i] + 1);
    }

    // 正确示例
    const increasedByOne = [];
    numbers.forEach((num) => {
      increasedByOne.push(num + 1);
    });

    // best (keeping it functional)
    const increasedByOne = numbers.map((num) => num + 1);
    ```

  <a name="generators--spacing"></a>
  - [11.2](#generators--spacing) 如果使用生成函数, 确保它们的函数声明中空格与普通函数是一致的。 eslint: [`generator-star-spacing`](https://eslint.org/docs/rules/generator-star-spacing)

    > Why? `function` and `*` are part of the same conceptual keyword - `*` is not a modifier for `function`, `function*` is a unique construct, different from `function`.

    ```javascript
    // 反例
    function * foo() {
      // ...
    }

    // 反例
    const bar = function * () {
      // ...
    };

    // 反例
    const baz = function *() {
      // ...
    };

    // 反例
    const quux = function*() {
      // ...
    };

    // 反例
    function*foo() {
      // ...
    }

    // 反例
    function *foo() {
      // ...
    }

    // very bad
    function
    *
    foo() {
      // ...
    }

    // very bad
    const wat = function
    *
    () {
      // ...
    };

    // 正确示例
    function* foo() {
      // ...
    }

    // 正确示例
    const foo = function* () {
      // ...
    };
    ```

**[⬆ 回到目录](#目录)**

## 属性

  <a name="properties--dot"></a><a name="12.1"></a>
  - [12.1](#properties--dot)使用点号访问属性。 eslint: [`dot-notation`](https://eslint.org/docs/rules/dot-notation.html)

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    // 反例
    const isJedi = luke['jedi'];

    // 正确示例
    const isJedi = luke.jedi;
    ```

  <a name="properties--bracket"></a><a name="12.2"></a>
  - [12.2](#properties--bracket) 使用 `[]` 获取变量属性。

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    function getProp(prop) {
      return luke[prop];
    }

    const isJedi = getProp('jedi');
    ```
  <a name="es2016-properties--exponentiation-operator"></a>
  - [12.3](#es2016-properties--exponentiation-operator) 使用幂操作符`**` 做幂运算。 eslint: [`no-restricted-properties`](https://eslint.org/docs/rules/no-restricted-properties).

    ```javascript
    // 反例
    const binary = Math.pow(2, 10);

    // 正确示例
    const binary = 2 ** 10;
    ```

**[⬆ 回到目录](#目录)**

## 变量

  <a name="variables--const"></a><a name="13.1"></a>
  - [13.1](#variables--const) 使用 `const` 和 `let` 声明变量。否则会产生全局变量。应避免污染全局命名空间。 eslint: [`no-undef`](https://eslint.org/docs/rules/no-undef) [`prefer-const`](https://eslint.org/docs/rules/prefer-const)

    ```javascript
    // 反例
    superPower = new SuperPower();

    // 正确示例
    const superPower = new SuperPower();
    ```

  <a name="variables--one-const"></a><a name="13.2"></a>
  - [13.2](#variables--one-const) 每个变量都用一个 `const` 或 `let` 声明。 eslint: [`one-var`](https://eslint.org/docs/rules/one-var.html)

    > 此种方式很容易声明新的变量，儿不需要考虑将`;` 换成 `,` ，或者产生一个只有标点不同的修改。 此方式也可以使得在调试时，单步跟踪每个声明语句，而不是一下跳过所有声明。

    ```javascript
    // 反例
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // 反例
    // (compare to above, and try to spot the mistake)
    const items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // 正确示例
    const items = getItems();
    const goSportsTeam = true;
    const dragonball = 'z';
    ```

  <a name="variables--const-let-group"></a><a name="13.3"></a>
  - [13.3](#variables--const-let-group) 将`const`与`let`分别放在一起。

    > 当需要分配一个新的变量，而此变量又依赖之前分配过的变量的时候，这样很便捷。

    ```javascript
    // 反例
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // 反例
    let i;
    const items = getItems();
    let dragonball;
    const goSportsTeam = true;
    let len;

    // 正确示例
    const goSportsTeam = true;
    const items = getItems();
    let dragonball;
    let i;
    let length;
    ```

  <a name="variables--define-where-used"></a><a name="13.4"></a>
  - [13.4](#variables--define-where-used) 在需要时声明变量，但是要放在合理的位置。

    > `let` 和 `const` 都是块级作用域而不是函数级作用域。

    ```javascript
    // 反例 - unnecessary function call
    function checkName(hasName) {
      const name = getName();

      if (hasName === 'test') {
        return false;
      }

      if (name === 'test') {
        this.setName('');
        return false;
      }

      return name;
    }

    // 正确示例
    function checkName(hasName) {
      if (hasName === 'test') {
        return false;
      }

      const name = getName();

      if (name === 'test') {
        this.setName('');
        return false;
      }

      return name;
    }
    ```
  <a name="variables--no-chain-assignment"></a><a name="13.5"></a>
  - [13.5](#variables--no-chain-assignment) 不要链式分配变量。 eslint: [`no-multi-assign`](https://eslint.org/docs/rules/no-multi-assign)

    > 链式分配变量会创建隐式全局变量。

    ```javascript
    // 反例
    (function example() {
      // JavaScript interprets this as
      // let a = ( b = ( c = 1 ) );
      // The let keyword only applies to variable a; variables b and c become
      // global variables.
      let a = b = c = 1;
    }());

    console.log(a); // throws ReferenceError
    console.log(b); // 1
    console.log(c); // 1

    // 正确示例
    (function example() {
      let a = 1;
      let b = a;
      let c = a;
    }());

    console.log(a); // throws ReferenceError
    console.log(b); // throws ReferenceError
    console.log(c); // throws ReferenceError

    // the same applies for `const`
    ```

  <a name="variables--unary-increment-decrement"></a><a name="13.6"></a>
  - [13.6](#variables--unary-increment-decrement) 不要使用一元自增减运算符 (`++`, `--`)。 eslint [`no-plusplus`](https://eslint.org/docs/rules/no-plusplus)

    > 根据eslint文档，一元增减运算符受自动插入分号的影响，可能会导致应用中值自增减的静默错误。使用`num += 1` 语句代替 `num++` 或 `num ++`也更有表现力。 禁用一元自增减运算符还会防止无意的预增/预减值，这也会导致程序出现意外行为。

    ```javascript
    // 反例

    const array = [1, 2, 3];
    let num = 1;
    num++;
    --num;

    let sum = 0;
    let truthyCount = 0;
    for (let i = 0; i < array.length; i++) {
      let value = array[i];
      sum += value;
      if (value) {
        truthyCount++;
      }
    }

    // 正确示例

    const array = [1, 2, 3];
    let num = 1;
    num += 1;
    num -= 1;

    const sum = array.reduce((a, b) => a + b, 0);
    const truthyCount = array.filter(Boolean).length;
    ```

<a name="variables--linebreak"></a>
  - [13.7](#variables--linebreak) 在赋值时避免在`=` 前后换行。如果赋值语句超出[`max-len`](https://eslint.org/docs/rules/max-len.html), 就用小括号将此值包起来再换行。 eslint [`operator-linebreak`](https://eslint.org/docs/rules/operator-linebreak.html).

    > 在 `=` 附近换行容易混淆这个赋值语句。

    ```javascript
    // 反例
    const foo =
      superLongLongLongLongLongLongLongLongFunctionName();

    // 反例
    const foo
      = 'superLongLongLongLongLongLongLongLongString';

    // 正确示例
    const foo = (
      superLongLongLongLongLongLongLongLongFunctionName()
    );

    // 正确示例
    const foo = 'superLongLongLongLongLongLongLongLongString';
    ```

<a name="variables--no-unused-vars"></a>
  - [13.8](#variables--no-unused-vars) 不允许有未使用的变量。 eslint: [`no-unused-vars`](https://eslint.org/docs/rules/no-unused-vars)

    > 一个声明了但未使用的变量更像是由于重构未完成而产生的错误。这种变量在代码中出现会使代码可读性变差。

    ```javascript
    // 反例

    var some_unused_var = 42;

    // Write-only variables are not considered as used.
    var y = 10;
    y = 5;

    // A read for a modification of itself is not considered as used.
    var z = 0;
    z = z + 1;

    // Unused function arguments.
    function getX(x, y) {
        return x;
    }

    // 正确示例

    function getXPlusY(x, y) {
      return x + y;
    }

    var x = 1;
    var y = a + 2;

    alert(getXPlusY(x, y));

    // 'type' is ignored even if unused because it has a rest property sibling.
    // This is a form of extracting an object that omits the specified keys.
    var { type, ...coords } = data;
    // 'coords' is now the 'data' object without its 'type' property.
    ```

**[⬆ 回到目录](#目录)**

## 作用域提升

  <a name="hoisting--about"></a><a name="14.1"></a>
  - [14.1](#hoisting--about) `var` 声明会被提升至作用域顶部，但是它的赋值不会提升。 `const` 和 `let` 声明被赋予了一种称为 [Temporal Dead Zones (TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_Dead_Zone)的概念。这对于了解为什么[typeof不再安全](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15)很重要。

    ```javascript
    // 假设没有在全局定义变量notDefined,则如下会出错
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // 因为变量作用域提升，在引用位置之后声明变量，会正常运行。
    // 注：分配的值`true`没有提升
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // 解释器将变量声明提升至顶部
    // 这意味着可以重写示例如下：
    function example() {
      let declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }

    // 使用const和let则不同
    function example() {
      console.log(declaredButNotAssigned); // => throws a ReferenceError
      console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
      const declaredButNotAssigned = true;
    }
    ```

  <a name="hoisting--anon-expressions"></a><a name="14.2"></a>
  - [14.2](#hoisting--anon-expressions) 匿名函数表达式的变量名会被提升，但函数内容并不会。与`var`的情况相同。

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function () {
        console.log('anonymous function expression');
      };
    }
    ```

  <a name="hoisting--named-expresions"></a><a name="hoisting--named-expressions"></a><a name="14.3"></a>
  - [14.3](#hoisting--named-expressions) 命名的函数表达式的变量名会被提升，但函数名和函数内容并不会。

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // 函数名与变量名相同时也一样
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      };
    }
    ```

  <a name="hoisting--declarations"></a><a name="14.4"></a>
  - [14.4](#hoisting--declarations) 函数声明的名称和函数体都会被提升。

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - 更多的信息请参考 [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting/) by [Ben Cherry](http://www.adequatelygood.com/).

**[⬆ 回到目录](#目录)**

## 比较运算符与等号

  <a name="comparison--eqeqeq"></a><a name="15.1"></a>
  - [15.1](#comparison--eqeqeq) 使用 `===` 和 `!==` 而不是 `==` 和 `!=`。 eslint: [`eqeqeq`](https://eslint.org/docs/rules/eqeqeq.html)

  <a name="comparison--if"></a><a name="15.2"></a>
  - [15.2](#comparison--if) 条件表达式如 `if` 语句通过抽象方法 `ToBoolean`强制计算它们的表达式并总是遵守如下规则:

    - **Objects** 被计算为 **true**
    - **Undefined** 被计算为 **false**
    - **Null** 被计算为 **false**
    - **Booleans** 被计算为 **the value of the boolean**
    - **Numbers** 被计算为 **false** if **+0, -0, or NaN**, otherwise **true**
    - **Strings** 被计算为 **false** if an empty string `''`, otherwise **true**

    ```javascript
    if ([0] && []) {
      // true
      // an array (even an empty one) is an object, objects will evaluate to true
    }
    ```

  <a name="comparison--shortcuts"></a><a name="15.3"></a>
  - [15.3](#comparison--shortcuts) 布尔值使用简写, 字符串和数字要明确比较对象。

    ```javascript
    // 反例
    if (isValid === true) {
      // ...
    }

    // 正确示例
    if (isValid) {
      // ...
    }

    // 反例
    if (name) {
      // ...
    }

    // 正确示例
    if (name !== '') {
      // ...
    }

    // 反例
    if (collection.length) {
      // ...
    }

    // 正确示例
    if (collection.length > 0) {
      // ...
    }
    ```

  <a name="comparison--switch-blocks"></a><a name="15.4"></a>
  - [15.4](#comparison--switch-blocks) 在 `case` and `default` 分句中使用花括号创建包含语法声明的块级作用域。(例如： `let`, `const`, `function`, and `class`). eslint: [`no-case-declarations`](https://eslint.org/docs/rules/no-case-declarations.html)

    > 词法声明在整个`switch` 块中都是可见的, 但只有赋值时才会初始化，这在`case` 语句被执行时发生。 当多个 `case` 分句中有相同定义时会引发问题。

    ```javascript
    // 反例
    switch (foo) {
      case 1:
        let x = 1;
        break;
      case 2:
        const y = 2;
        break;
      case 3:
        function f() {
          // ...
        }
        break;
      default:
        class C {}
    }

    // 正确示例
    switch (foo) {
      case 1: {
        let x = 1;
        break;
      }
      case 2: {
        const y = 2;
        break;
      }
      case 3: {
        function f() {
          // ...
        }
        break;
      }
      case 4:
        bar();
        break;
      default: {
        class C {}
      }
    }
    ```

  <a name="comparison--nested-ternaries"></a><a name="15.5"></a>
  - [15.5](#comparison--nested-ternaries) 三元表达式不能嵌套，并且应该在同一行中。 eslint: [`no-nested-ternary`](https://eslint.org/docs/rules/no-nested-ternary.html)

    ```javascript
    // 反例
    const foo = maybe1 > maybe2
      ? "bar"
      : value1 > value2 ? "baz" : null;

    // split into 2 separated ternary expressions
    const maybeNull = value1 > value2 ? 'baz' : null;

    // better
    const foo = maybe1 > maybe2
      ? 'bar'
      : maybeNull;

    // best
    const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
    ```

  <a name="comparison--unneeded-ternary"></a><a name="15.6"></a>
  - [15.6](#comparison--unneeded-ternary) 避免不必要的三元表达式。 eslint: [`no-unneeded-ternary`](https://eslint.org/docs/rules/no-unneeded-ternary.html)

    ```javascript
    // 反例
    const foo = a ? a : b;
    const bar = c ? true : false;
    const baz = c ? false : true;

    // 正确示例
    const foo = a || b;
    const bar = !!c;
    const baz = !c;
    ```

  <a name="comparison--no-mixed-operators"></a><a name="15.7"></a>
  - [15.7](#comparison--no-mixed-operators) 当操作符混用时，使用圆括号。 只有标准运算符: `+`, `-`, 和 `**` 的优先级显而易见时，可以不使用圆括号。 建议将 `/` 和 `*` 放在圆括号中，因为当它们混合使用时，优先级可能产生歧义。
  eslint: [`no-mixed-operators`](https://eslint.org/docs/rules/no-mixed-operators.html)

    > 这样提高了可读性，并且明确了开发者意图。

    ```javascript
    // 反例
    const foo = a && b < 0 || c > 0 || d + 1 === 0;

    // 反例
    const bar = a ** b - 5 % d;

    // 反例
    // one may be confused into thinking (a || b) && c
    if (a || b && c) {
      return d;
    }

    // 反例
    const bar = a + b / c * d;

    // 正确示例
    const foo = (a && b < 0) || c > 0 || (d + 1 === 0);

    // 正确示例
    const bar = a ** b - (5 % d);

    // 正确示例
    if (a || (b && c)) {
      return d;
    }

    // 正确示例
    const bar = a + (b / c) * d;
    ```
    
  <a name="comparison--moreinfo"></a><a name="15.8"></a>
  - [15.8](#comparison--moreinfo) 更多信息请见 Angus Croll 的 [Truth Equality and JavaScript](https://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108)


**[⬆ 回到目录](#目录)**

## 代码块

  <a name="blocks--braces"></a><a name="16.1"></a>
  - [16.1](#blocks--braces) 使用大括号包裹多行代码块。 eslint: [`nonblock-statement-body-position`](https://eslint.org/docs/rules/nonblock-statement-body-position)

    ```javascript
    // 反例
    if (test)
      return false;

    // 正确示例
    if (test) return false;

    // 正确示例
    if (test) {
      return false;
    }

    // 反例
    function foo() { return false; }

    // 正确示例
    function bar() {
      return false;
    }
    ```

  <a name="blocks--cuddled-elses"></a><a name="16.2"></a>
  - [16.2](#blocks--cuddled-elses) 如果通过`if` 和`else`使用多行代码块，把`else`放在`if`代码块关闭括号的同一行。 eslint: [`brace-style`](https://eslint.org/docs/rules/brace-style.html)

    ```javascript
    // 反例
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // 正确示例
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```

  <a name="blocks--no-else-return"></a><a name="16.3"></a>
  - [16.3](#blocks--no-else-return) 如果`if` 代码块中总是需要用 `return` 返回,那么 `else`就不需要写了。如果`if`代码块包含`return`，其后的`else if`也包含`return`，此时可将return分到多个`if` 代码块中。 eslint: [`no-else-return`](https://eslint.org/docs/rules/no-else-return)

    ```javascript
    // 反例
    function foo() {
      if (x) {
        return x;
      } else {
        return y;
      }
    }

    // 反例
    function cats() {
      if (x) {
        return x;
      } else if (y) {
        return y;
      }
    }

    // 反例
    function dogs() {
      if (x) {
        return x;
      } else {
        if (y) {
          return y;
        }
      }
    }

    // 正确示例
    function foo() {
      if (x) {
        return x;
      }

      return y;
    }

    // 正确示例
    function cats() {
      if (x) {
        return x;
      }

      if (y) {
        return y;
      }
    }

    // 正确示例
    function dogs(x) {
      if (x) {
        if (z) {
          return y;
        }
      } else {
        return z;
      }
    }
    ```

**[⬆ 回到目录](#目录)**

## 控制声明

  <a name="control-statements"></a>
  - [17.1](#control-statements) 当控制语句 (`if`, `while` 等) 太长或超过最大长度限制时，把每一个(组)判断条件放在单独一行里。逻辑操作符放在行首。

    > 将操作符放在行首目的是与链式函数调用保持一致。这提高了可读性，也使复杂逻辑更清晰。

    ```javascript
    // 反例
    if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
      thing1();
    }

    // 反例
    if (foo === 123 &&
      bar === 'abc') {
      thing1();
    }

    // 反例
    if (foo === 123
      && bar === 'abc') {
      thing1();
    }

    // 反例
    if (
      foo === 123 &&
      bar === 'abc'
    ) {
      thing1();
    }

    // 正确示例
    if (
      foo === 123
      && bar === 'abc'
    ) {
      thing1();
    }

    // 正确示例
    if (
      (foo === 123 || bar === 'abc')
      && doesItLookGoodWhenItBecomesThatLong()
      && isThisReallyHappening()
    ) {
      thing1();
    }

    // 正确示例
    if (foo === 123 && bar === 'abc') {
      thing1();
    }
    ```

  <a name="control-statement--value-selection"></a><a name="control-statements--value-selection"></a>
  - [17.2](#control-statements--value-selection) 不要用逻辑操作符代替控制语句。

    ```javascript
    // 反例
    !isRunning && startRunning();

    // 正确示例
    if (!isRunning) {
      startRunning();
    }
    ```

**[⬆ 回到目录](#目录)**

## 注释

  <a name="comments--multiline"></a><a name="17.1"></a>
  - [18.1](#comments--multiline) 函数说明使用 `/** ... */` 作为多行注释。包含描述、指定所有参数和返回值的类型和值。

    ```javascript
    // 反例
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...

      return element;
    }

    // 正确示例
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

  <a name="comments--singleline"></a><a name="18.2"></a>
  - [18.2](#comments--singleline) 单行注释用 `//`。将单行注释放在被注释区域上面单独一行。如果注释不是第一行，那么注释前面空一行。

    ```javascript
    // 反例
    const active = true;  // is current tab

    // 正确示例
    // is current tab
    const active = true;

    // 反例
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // 正确示例
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // also good
    function getType() {
      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }
    ```

  <a name="comments--spaces"></a><a name="18.3"></a>
  - [18.3](#comments--spaces) 注释开头空一格，便于阅读。 eslint: [`spaced-comment`](https://eslint.org/docs/rules/spaced-comment)

    ```javascript
    // 反例
    //is current tab
    const active = true;

    // 正确示例
    // is current tab
    const active = true;

    // 反例
    /**
     *make() returns a new element
     *based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }

    // 正确示例
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

  <a name="comments--actionitems"></a><a name="18.4"></a>
  - [18.4](#comments--actionitems) 给注释增加 `FIXME` or `TODO` 来帮助其他开发者快速理解意图。`FIXME: -- 需要研究` ，`TODO: -- 需要实现`.

  <a name="comments--fixme"></a><a name="18.5"></a>
  - [18.5](#comments--fixme) 使用 `// FIXME:` 给问题做注释

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // FIXME: shouldn’t use a global here
        total = 0;
      }
    }
    ```

  <a name="comments--todo"></a><a name="18.6"></a>
  - [18.6](#comments--todo) 使用 `// TODO:` 注释问题的解决方案。

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // TODO: total should be configurable by an options param
        this.total = 0;
      }
    }
    ```

**[⬆ 回到目录](#目录)**

## 留白

  <a name="whitespace--spaces"></a><a name="19.1"></a>
  - [19.1](#whitespace--spaces) 使用2个空格还是4个空格缩进，可以根据项目的prettier配置。 eslint: [`indent`](https://eslint.org/docs/rules/indent.html)

    ```javascript
    // 反例
    function foo() {
    ∙∙∙∙let name;
    }

    // 反例
    function bar() {
    ∙let name;
    }

    // 正确示例
    function baz() {
    ∙∙let name;
    }
    ```

  <a name="whitespace--before-blocks"></a><a name="19.2"></a>
  - [19.2](#whitespace--before-blocks) 在花括号前放置一个空格。 eslint: [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks.html)

    ```javascript
    // 反例
    function test(){
      console.log('test');
    }

    // 正确示例
    function test() {
      console.log('test');
    }

    // 反例
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });

    // 正确示例
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });
    ```

  <a name="whitespace--around-keywords"></a><a name="19.3"></a>
  - [19.3](#whitespace--around-keywords) 在控制语句 (`if`, `while` 等)的小括号前放一个空格。在函数调用及声明中，不在函数的参数列表前加空格。 eslint: [`keyword-spacing`](https://eslint.org/docs/rules/keyword-spacing.html)

    ```javascript
    // 反例
    if(isJedi) {
      fight ();
    }

    // 正确示例
    if (isJedi) {
      fight();
    }

    // 反例
    function fight () {
      console.log ('Swooosh!');
    }

    // 正确示例
    function fight() {
      console.log('Swooosh!');
    }
    ```

  <a name="whitespace--infix-ops"></a><a name="19.4"></a>
  - [19.4](#whitespace--infix-ops) 使用空格将运算符隔开。 eslint: [`space-infix-ops`](https://eslint.org/docs/rules/space-infix-ops.html)

    ```javascript
    // 反例
    const x=y+5;

    // 正确示例
    const x = y + 5;
    ```

  <a name="whitespace--newline-at-end"></a><a name="19.5"></a>
  - [19.5](#whitespace--newline-at-end) 在文件末尾插入一个空行。 eslint: [`eol-last`](https://github.com/eslint/eslint/blob/master/docs/rules/eol-last.md)

    ```javascript
    // 反例
    import { es6 } from './AirbnbStyleGuide';
      // ...
    export default es6;
    ```

    ```javascript
    // 反例
    import { es6 } from './AirbnbStyleGuide';
      // ...
    export default es6;↵
    ↵
    ```

    ```javascript
    // 正确示例
    import { es6 } from './AirbnbStyleGuide';
      // ...
    export default es6;↵
    ```

  <a name="whitespace--chains"></a><a name="19.6"></a>
  - [19.6](#whitespace--chains) 使用长的链式调用时，使用缩进。(多于两个方法). 将点号前置，强调该方法是调用而不是新语句。 eslint: [`newline-per-chained-call`](https://eslint.org/docs/rules/newline-per-chained-call) [`no-whitespace-before-property`](https://eslint.org/docs/rules/no-whitespace-before-property)

    ```javascript
    // 反例
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // 反例
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // 正确示例
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // 反例
    const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', `translate(${radius + margin},${radius + margin})`)
        .call(tron.led);

    // 正确示例
    const leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', `translate(${radius + margin},${radius + margin})`)
        .call(tron.led);

    // 正确示例
    const leds = stage.selectAll('.led').data(data);
    ```

  <a name="whitespace--after-blocks"></a><a name="19.7"></a>
  - [19.7](#whitespace--after-blocks) 在块末尾和新语句前插入空白行。

    ```javascript
    // 反例
    if (foo) {
      return bar;
    }
    return baz;

    // 正确示例
    if (foo) {
      return bar;
    }

    return baz;

    // 反例
    const obj = {
      foo() {
      },
      bar() {
      },
    };
    return obj;

    // 正确示例
    const obj = {
      foo() {
      },

      bar() {
      },
    };

    return obj;

    // 反例
    const arr = [
      function foo() {
      },
      function bar() {
      },
    ];
    return arr;

    // 正确示例
    const arr = [
      function foo() {
      },

      function bar() {
      },
    ];

    return arr;
    ```

  <a name="whitespace--padded-blocks"></a><a name="19.8"></a>
  - [19.8](#whitespace--padded-blocks) 不要在块内使用空白行。 eslint: [`padded-blocks`](https://eslint.org/docs/rules/padded-blocks.html)

    ```javascript
    // 反例
    function bar() {

      console.log(foo);

    }

    // 反例
    if (baz) {

      console.log(qux);
    } else {
      console.log(foo);

    }

    // 反例
    class Foo {

      constructor(bar) {
        this.bar = bar;
      }
    }

    // 正确示例
    function bar() {
      console.log(foo);
    }

    // 正确示例
    if (baz) {
      console.log(qux);
    } else {
      console.log(foo);
    }
    ```

  <a name="whitespace--no-multiple-blanks"></a><a name="19.9"></a>
  - [19.9](#whitespace--no-multiple-blanks) 不要在代码之间使用多个空白行。 eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

    <!-- markdownlint-disable MD012 -->
    ```javascript
    // 反例
    class Person {
      constructor(fullName, email, birthday) {
        this.fullName = fullName;


        this.email = email;


        this.setAge(birthday);
      }


      setAge(birthday) {
        const today = new Date();


        const age = this.getAge(today, birthday);


        this.age = age;
      }


      getAge(today, birthday) {
        // ..
      }
    }

    // 正确示例
    class Person {
      constructor(fullName, email, birthday) {
        this.fullName = fullName;
        this.email = email;
        this.setAge(birthday);
      }

      setAge(birthday) {
        const today = new Date();
        const age = getAge(today, birthday);
        this.age = age;
      }

      getAge(today, birthday) {
        // ..
      }
    }
    ```

  <a name="whitespace--in-parens"></a><a name="19.10"></a>
  - [19.10](#whitespace--in-parens) 不要在括号内添加空格。 eslint: [`space-in-parens`](https://eslint.org/docs/rules/space-in-parens.html)

    ```javascript
    // 反例
    function bar( foo ) {
      return foo;
    }

    // 正确示例
    function bar(foo) {
      return foo;
    }

    // 反例
    if ( foo ) {
      console.log(foo);
    }

    // 正确示例
    if (foo) {
      console.log(foo);
    }
    ```

  <a name="whitespace--in-brackets"></a><a name="19.11"></a>
  - [19.11](#whitespace--in-brackets) 不要在数组括号内的前后添加空格。 eslint: [`array-bracket-spacing`](https://eslint.org/docs/rules/array-bracket-spacing.html)

    ```javascript
    // 反例
    const foo = [ 1, 2, 3 ];
    console.log(foo[ 0 ]);

    // 正确示例
    const foo = [1, 2, 3];
    console.log(foo[0]);
    ```

  <a name="whitespace--in-braces"></a><a name="19.12"></a>
  - [19.12](#whitespace--in-braces) 在大括号内添加空格。 eslint: [`object-curly-spacing`](https://eslint.org/docs/rules/object-curly-spacing.html)

    ```javascript
    // 反例
    const foo = {clark: 'kent'};

    // 正确示例
    const foo = { clark: 'kent' };
    ```

  <a name="whitespace--max-len"></a><a name="19.13"></a>
  - [19.13](#whitespace--max-len) 避免单行代码超过100个字符(包括空格)。 注: 遵循上面的约定 [above](#strings--line-length), 长字符串不适用于此规则，不应被分解。 eslint: [`max-len`](https://eslint.org/docs/rules/max-len.html)

    > 因为这将确保可读性和可维护性。

    ```javascript
    // 反例
    const foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy;

    // 反例
    $.ajax({ method: 'POST', url: 'https://airbnb.com/', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this city.'));

    // 正确示例
    const foo = jsonData
      && jsonData.foo
      && jsonData.foo.bar
      && jsonData.foo.bar.baz
      && jsonData.foo.bar.baz.quux
      && jsonData.foo.bar.baz.quux.xyzzy;

    // 正确示例
    $.ajax({
      method: 'POST',
      url: 'https://airbnb.com/',
      data: { name: 'John' },
    })
      .done(() => console.log('Congratulations!'))
      .fail(() => console.log('You have failed this city.'));
    ```

  <a name="whitespace--block-spacing"></a><a name="19.14"></a>
  - [19.14](#whitespace--block-spacing) 同一行的代码块(开放/闭合)需要保持一致的空格。即作为语句的花括号内也要加空格 —— { 后和 } 前都需要空格。 eslint: [`block-spacing`](https://eslint.org/docs/rules/block-spacing)

    ```javascript
    // 反例
    function foo() {return true;}
    if (foo) { bar = 0;}

    // 正确示例
    function foo() { return true; }
    if (foo) { bar = 0; }
    ```

  <a name="whitespace--comma-spacing"></a>
  - [19.15](#whitespace--comma-spacing) 逗号前不要加空格，逗号后加空格。 eslint: [`comma-spacing`](https://eslint.org/docs/rules/comma-spacing)

    ```javascript
    // 反例
    var foo = 1,bar = 2;
    var arr = [1 , 2];

    // 正确示例
    var foo = 1, bar = 2;
    var arr = [1, 2];
    ```

  <a name="whitespace--computed-property-spacing"></a>
  - [19.16](#whitespace--computed-property-spacing) 计算属性的括号内必须使用空格。 eslint: [`computed-property-spacing`](https://eslint.org/docs/rules/computed-property-spacing)

    ```javascript
    // 反例
    obj[foo ]
    obj[ 'foo']
    var x = {[ b ]: a}
    obj[foo[ bar ]]

    // 正确示例
    obj[foo]
    obj['foo']
    var x = { [b]: a }
    obj[foo[bar]]
    ```

  <a name="whitespace--func-call-spacing"></a>
  - [19.17](#whitespace--func-call-spacing) 调用函数时函数名和小括号之间不要加空格。 eslint: [`func-call-spacing`](https://eslint.org/docs/rules/func-call-spacing)

    ```javascript
    // 反例
    func ();

    func
    ();

    // 正确示例
    func();
    ```

  <a name="whitespace--key-spacing"></a>
  - [19.18](#whitespace--key-spacing) 定义对象的字面量属性时，key和value之间应该加空格。 eslint: [`key-spacing`](https://eslint.org/docs/rules/key-spacing)

    ```javascript
    // 反例
    var obj = { "foo" : 42 };
    var obj2 = { "foo":42 };

    // 正确示例
    var obj = { "foo": 42 };
    ```

  <a name="whitespace--no-trailing-spaces"></a>
  - [19.19](#whitespace--no-trailing-spaces) 行末不要加空格。 eslint: [`no-trailing-spaces`](https://eslint.org/docs/rules/no-trailing-spaces)

  <a name="whitespace--no-multiple-empty-lines"></a>
  - [19.20](#whitespace--no-multiple-empty-lines) 避免出现多个空行，文件末尾只空一行。文件不要以空行开始。 eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

    <!-- markdownlint-disable MD012 -->
    ```javascript
    // 反例 - multiple empty lines
    var x = 1;


    var y = 2;

    // 反例 - 2+ newlines at end of file
    var x = 1;
    var y = 2;


    // 反例 - 1+ newline(s) at beginning of file

    var x = 1;
    var y = 2;

    // 正确示例
    var x = 1;
    var y = 2;

    ```
    <!-- markdownlint-enable MD012 -->

**[⬆ 回到目录](#目录)**

## 逗号

  <a name="commas--leading-trailing"></a><a name="20.1"></a>
  - [20.1](#commas--leading-trailing) 逗号不要前置。 eslint: [`comma-style`](https://eslint.org/docs/rules/comma-style.html)

    ```javascript
    // 反例
    const story = [
        once
      , upon
      , aTime
    ];

    // 正确示例
    const story = [
      once,
      upon,
      aTime,
    ];

    // 反例
    const hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    };

    // 正确示例
    const hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    };
    ```

  <a name="commas--dangling"></a><a name="20.2"></a>
  - [20.2](#commas--dangling) 结尾可以添加额外的逗号。 eslint: [`comma-dangle`](https://eslint.org/docs/rules/comma-dangle.html)

    > 可以使git diff 更清晰。 像Babel这样的编译器会删除代码中的额外逗号，这意味着不必担心旧版浏览器中的 [结尾逗号问题](https://github.com/airbnb/javascript/blob/es5-deprecated/es5/README.md#commas) in legacy browsers.

    ```diff
    // 反例 - 无结尾逗号的git diff
    const hero = {
         firstName: 'Florence',
    -    lastName: 'Nightingale'
    +    lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing']
    };

    // 正确示例 - 有结尾逗号的 git diff
    const hero = {
         firstName: 'Florence',
         lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing'],
    };
    ```

    ```javascript
    // 反例
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully'
    };

    const heroes = [
      'Batman',
      'Superman'
    ];

    // 正确示例
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully',
    };

    const heroes = [
      'Batman',
      'Superman',
    ];

    // 反例
    function createHero(
      firstName,
      lastName,
      inventorOf
    ) {
      // does nothing
    }

    // 正确示例
    function createHero(
      firstName,
      lastName,
      inventorOf,
    ) {
      // does nothing
    }

    // 正确示例 (note that a comma must not appear after a "rest" element)
    function createHero(
      firstName,
      lastName,
      inventorOf,
      ...heroArgs
    ) {
      // does nothing
    }

    // 反例
    createHero(
      firstName,
      lastName,
      inventorOf
    );

    // 正确示例
    createHero(
      firstName,
      lastName,
      inventorOf,
    );

    // 正确示例 (note that a comma must not appear after a "rest" element)
    createHero(
      firstName,
      lastName,
      inventorOf,
      ...heroArgs
    );
    ```

**[⬆ 回到目录](#目录)**

## 分号

  <a name="semicolons--required"></a><a name="21.1"></a>
  - [21.1](#semicolons--required) 编译器发展到如今，分号基本已经成为了一个风格问题。各自遵循项目的约定即可。建议参照vue，不添加分号。 eslint: [`semi`](https://eslint.org/docs/rules/semi.html)

    > 在对分号有兼容要求的项目中，可以配置prettier自动添加分号。

    ```javascript
    // 无分号配置 - prettier
    semi: false,
    // 有分号配置 - prettier
    semi: true,
    ```

    [参考](https://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214#7365214).

**[⬆ 回到目录](#目录)**

## 强制类型转换

  <a name="coercion--explicit"></a><a name="22.1"></a>
  - [22.1](#coercion--explicit) 在语句开始时执行强制类型转换。

  <a name="coercion--strings"></a><a name="22.2"></a>
  - [22.2](#coercion--strings) 字符串: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    // => this.reviewScore = 9;

    // 反例
    const totalScore = new String(this.reviewScore); // typeof totalScore 值为 "object" 而不是 "string"

    // 反例
    const totalScore = this.reviewScore + ''; // 执行的是 this.reviewScore.valueOf()

    // 反例
    const totalScore = this.reviewScore.toString(); // 不能保证一定返回string

    // 正确示例
    const totalScore = String(this.reviewScore);
    ```

  <a name="coercion--numbers"></a><a name="21.3"></a>
  - [22.3](#coercion--numbers) 数字: 对 `Number` 使用 `parseInt` 转换，并始终带上类型转换的基数。 eslint: [`radix`](https://eslint.org/docs/rules/radix) [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    const inputValue = '4';

    // 反例
    const val = new Number(inputValue);

    // 反例
    const val = +inputValue;

    // 反例
    const val = inputValue >> 0;

    // 反例
    const val = parseInt(inputValue);

    // 正确示例
    const val = Number(inputValue);

    // 正确示例
    const val = parseInt(inputValue, 10);
    ```

  <a name="coercion--comment-deviations"></a><a name="21.4"></a>
  - [22.4](#coercion--comment-deviations) 如果由于某些原因 `parseInt` 成为瓶颈，而需要使用位操作符解决 [性能问题](https://jsperf.com/coercion-vs-casting/3)时, 需要在注释中说明原因和目的。

    ```javascript
    // 正确示例
    /**
     * parseInt 是执行慢的原因
     * 使用位操作符可以加快执行速度
     */
    const val = inputValue >> 0;
    ```

  <a name="coercion--bitwise"></a><a name="21.5"></a>
  - [22.5](#coercion--bitwise) **注:** 小心使用位操作符。 数字会被作为 [64位的值](https://es5.github.io/#x4.3.19), 但是位操作总是返回32位的整数。 ([source](https://es5.github.io/#x11.7))。 位运算对于大于32位的整数会产生意外行为。 [讨论](https://github.com/airbnb/javascript/issues/109)。最大的32位整数是 2,147,483,647:

    ```javascript
    2147483647 >> 0; // => 2147483647
    2147483648 >> 0; // => -2147483648
    2147483649 >> 0; // => -2147483647
    ```

  <a name="coercion--booleans"></a><a name="21.6"></a>
  - [22.6](#coercion--booleans) 布尔类型: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    const age = 0;

    // 反例
    const hasAge = new Boolean(age);

    // 正确示例
    const hasAge = Boolean(age);

    // best
    const hasAge = !!age;
    ```

**[⬆ 回到目录](#目录)**

## 命名规范

  <a name="naming--descriptive"></a><a name="23.1"></a>
  - [23.1](#naming--descriptive) 避免一个字母的命名。使命名应具有描述性。 eslint: [`id-length`](https://eslint.org/docs/rules/id-length)

    ```javascript
    // 反例
    function q() {
      // ...
    }

    // 正确示例
    function query() {
      // ...
    }
    ```

  <a name="naming--camelCase"></a><a name="23.2"></a>
  - [23.2](#naming--camelCase) 使用驼峰命名对象、函数和实例。 eslint: [`camelcase`](https://eslint.org/docs/rules/camelcase.html)

    ```javascript
    // 反例
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}

    // 正确示例
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  <a name="naming--PascalCase"></a><a name="23.3"></a>
  - [23.3](#naming--PascalCase) 使用大驼峰(PascalCase)命名构造函数或类。 eslint: [`new-cap`](https://eslint.org/docs/rules/new-cap.html)

    ```javascript
    // 反例
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: 'nope',
    });

    // 正确示例
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: 'yup',
    });
    ```

  <a name="naming--leading-underscore"></a><a name="23.4"></a>
  - [23.4](#naming--leading-underscore) 不要使用前置或后置下划线。 eslint: [`no-underscore-dangle`](https://eslint.org/docs/rules/no-underscore-dangle.html)

    > 因为`JavaScript` 没有私有属性或私有方法的概念。尽管前置下划线通常代表 “私有”, 但实际上, 这些属性是完全公有的, 因此这部分也是 API 的内容。这可能会导致开发者误以为更改这个不会导致崩溃或不需要测试。如果想要私有的内容，就不要让它出现在这里。

    ```javascript
    // 反例
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';
    this._firstName = 'Panda';

    // 正确示例
    this.firstName = 'Panda';

    // 正确示例, in environments where WeakMaps are available
    // 请见 https://kangax.github.io/compat-table/es6/#test-WeakMap
    const firstNames = new WeakMap();
    firstNames.set(this, 'Panda');
    ```

  <a name="naming--self-this"></a><a name="23.5"></a>
  - [23.5](#naming--self-this) 不要保存引用 `this`. 使用箭头函数或 [函数绑定](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind).

    ```javascript
    // 反例
    function foo() {
      const self = this;
      return function () {
        console.log(self);
      };
    }

    // 反例
    function foo() {
      const that = this;
      return function () {
        console.log(that);
      };
    }

    // 正确示例
    function foo() {
      return () => {
        console.log(this);
      };
    }
    ```

  <a name="naming--filename-matches-export"></a><a name="22.6"></a>
  - [23.6](#naming--filename-matches-export) 文件名与default export导出的模块名应该完全一致。

    ```javascript
    // file 1 contents
    class CheckBox {
      // ...
    }
    export default CheckBox;

    // file 2 contents
    export default function fortyTwo() { return 42; }

    // file 3 contents
    export default function insideDirectory() {}

    // in some other file
    // 反例
    import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
    import FortyTwo from './FortyTwo'; // PascalCase import/filename, camelCase export
    import InsideDirectory from './InsideDirectory'; // PascalCase import/filename, camelCase export

    // 反例
    import CheckBox from './check_box'; // PascalCase import/export, snake_case filename
    import forty_two from './forty_two'; // snake_case import/filename, camelCase export
    import inside_directory from './inside_directory'; // snake_case import, camelCase export
    import index from './inside_directory/index'; // requiring the index file explicitly
    import insideDirectory from './insideDirectory/index'; // requiring the index file explicitly

    // 正确示例
    import CheckBox from './CheckBox'; // PascalCase export/import/filename
    import fortyTwo from './fortyTwo'; // camelCase export/import/filename
    import insideDirectory from './insideDirectory'; // camelCase export/import/directory name/implicit "index"
    // ^ supports both insideDirectory.js and insideDirectory/index.js
    ```

  <a name="naming--camelCase-default-export"></a><a name="23.7"></a>
  - [23.7](#naming--camelCase-default-export) 当使用export default 导出函数时，应使用驼峰命名。文件名应与函数名一致。

    ```javascript
    function makeStyleGuide() {
      // ...
    }

    export default makeStyleGuide;
    ```

  <a name="naming--PascalCase-singleton"></a><a name="23.8"></a>
  - [23.8](#naming--PascalCase-singleton) 导出一个结构体/类/单例/函数库/对象时，使用大驼峰(PascalCase)命名。

    ```javascript
    const AirbnbStyleGuide = {
      es6: {
      },
    };

    export default AirbnbStyleGuide;
    ```

  <a name="naming--Acronyms-and-Initialisms"></a>
  - [23.9](#naming--Acronyms-and-Initialisms) 简称或缩写应该全部大写或小写。

    > 因为名字就是给人阅读的，不是为了适应电脑算法的。

    ```javascript
    // 反例
    import SmsContainer from './containers/SmsContainer';

    // 反例
    const HttpRequests = [
      // ...
    ];

    // 正确示例
    import SMSContainer from './containers/SMSContainer';

    // 正确示例
    const HTTPRequests = [
      // ...
    ];

    // also good
    const httpRequests = [
      // ...
    ];

    // best
    import TextMessageContainer from './containers/TextMessageContainer';

    // best
    const requests = [
      // ...
    ];
    ```

  <a name="naming--uppercase"></a>
  - [23.10](#naming--uppercase) 应该使用全大写字母设置静态变量，但是需满足：
   (1) 变量用于导出
   (2) 变量由`const`定义，保证不可改变。
   (3) 变量是可信的，且其自身及子属性都是不可变的。

    > 这是一条额外的规则，来帮助开发者辨识一个变量是不是不可变的。
    - 对于所有的 `const` 变量呢? ———— 这是不必要的。大写变量不应该在同一个文件里定义并使用，它只用来作为导出变量。
    - 那导出对象呢? - 大写变量处于export的最高级 (例如： `EXPORTED_OBJECT.key`) 并且它包含的所有子属性都是不可比变的。

    ```javascript
    // 反例
    const PRIVATE_VARIABLE = 'should not be unnecessarily uppercased within a file';

    // 反例
    export const THING_TO_BE_CHANGED = 'should obviously not be uppercased';

    // 反例
    export let REASSIGNABLE_VARIABLE = 'do not use let with uppercase variables';

    // ---

    // allowed but does not supply semantic value
    export const apiKey = 'SOMEKEY';

    // better in most cases
    export const API_KEY = 'SOMEKEY';

    // ---

    // 反例 - unnecessarily uppercases key while adding no semantic value
    export const MAPPING = {
      KEY: 'value'
    };

    // 正确示例
    export const MAPPING = {
      key: 'value'
    };
    ```

**[⬆ 回到目录](#目录)**

## 访问器函数

  <a name="accessors--not-required"></a><a name="24.1"></a>
  - [24.1](#accessors--not-required) 不应使用属性的访问器函数。

  <a name="accessors--no-getters-setters"></a><a name="24.2"></a>
  - [24.2](#accessors--no-getters-setters) 不要使用JavaScript的getters/setters,因为它们会产生副作用,并且难以测试、理解和维护。可以用`getVal()` 和 `setVal('hello')`来创造自己的accessor函数。

    ```javascript
    // 反例
    class Dragon {
      get age() {
        // ...
      }

      set age(value) {
        // ...
      }
    }

    // 正确示例
    class Dragon {
      getAge() {
        // ...
      }

      setAge(value) {
        // ...
      }
    }
    ```

  <a name="accessors--boolean-prefix"></a><a name="24.3"></a>
  - [24.3](#accessors--boolean-prefix) 如果属性/方法是 `boolean`, 用 `isVal()` 或 `hasVal()`。

    ```javascript
    // 反例
    if (!dragon.age()) {
      return false;
    }

    // 正确示例
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  <a name="accessors--consistent"></a><a name="24.4"></a>
  - [24.4](#accessors--consistent) 可以使用`get()` 和 `set()` 函数, 但是要保持一致。

    ```javascript
    class Jedi {
      constructor(options = {}) {
        const lightsaber = options.lightsaber || 'blue';
        this.set('lightsaber', lightsaber);
      }

      set(key, val) {
        this[key] = val;
      }

      get(key) {
        return this[key];
      }
    }
    ```

**[⬆ 回到目录](#目录)**

## 事件

  <a name="events--hash"></a><a name="24.1"></a>
  - [25.1](#events--hash) 当为事件添加数据时 (无论是DOM事件还是Backbone之类的专有事件), 传递对象作为参数而不是值参数。 这允许后来者在添加更多数据到事件时，无需查找和更新事件的每个调用。因为参数是无序的。例如：

    ```javascript
    // 反例
    $(this).trigger('listingUpdated', listing.id);

    // ...

    $(this).on('listingUpdated', (e, listingID) => {
      // do something with listingID
    });
    ```

    prefer:

    ```javascript
    // 正确示例
    $(this).trigger('listingUpdated', { listingID: listing.id });

    // ...

    $(this).on('listingUpdated', (e, data) => {
      // do something with data.listingID
    });
    ```

  **[⬆ 回到目录](#目录)**

## jQuery

  <a name="jquery--dollar-prefix"></a><a name="26.1"></a>
  - [26.1](#jquery--dollar-prefix) jQuery的变量对象应该以`$`开头。

    ```javascript
    // 反例
    const sidebar = $('.sidebar');

    // 正确示例
    const $sidebar = $('.sidebar');

    // 正确示例
    const $sidebarBtn = $('.sidebar-btn');
    ```

  <a name="jquery--cache"></a><a name="26.2"></a>
  - [26.2](#jquery--cache) 缓存jQuery的选择器。

    ```javascript
    // 反例
    function setSidebar() {
      $('.sidebar').hide();

      // ...

      $('.sidebar').css({
        'background-color': 'pink',
      });
    }

    // 正确示例
    function setSidebar() {
      const $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...

      $sidebar.css({
        'background-color': 'pink',
      });
    }
    ```

  <a name="jquery--queries"></a><a name="26.3"></a>
  - [26.3](#jquery--queries) DOM查找用级联选择器 `$('.sidebar ul')` 或 父节点 > 子节点 `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)

  <a name="jquery--find"></a><a name="26.4"></a>
  - [26.4](#jquery--find) 将 `find` 与jQuery 选择器对象作用域一起使用。

    ```javascript
    // 反例
    $('ul', '.sidebar').hide();

    // 反例
    $('.sidebar').find('ul').hide();

    // 正确示例
    $('.sidebar ul').hide();

    // 正确示例
    $('.sidebar > ul').hide();

    // 正确示例
    $sidebar.find('ul').hide();
    ```

**[⬆ 回到目录](#目录)**

## ECMAScript-5-兼容性

  <a name="es5-compat--kangax"></a><a name="27.1"></a>
  - [27.1](#es5-compat--kangax) Refer to [Kangax](https://twitter.com/kangax/)’s ES5 [compatibility table](https://kangax.github.io/es5-compat-table/).

**[⬆ 回到目录](#目录)**

<a name="ecmascript-6-styles"></a>
## ECMAScript-6+(ES-2015+)编码规范

  <a name="es6-styles"></a><a name="28.1"></a>
  - [28.1](#es6-styles) This is a collection of links to the various ES6+ features.

1. [箭头函数](#箭头函数)
1. [类](#classes--constructors)
1. [Object Shorthand](#es6-object-shorthand)
1. [Object Concise](#es6-object-concise)
1. [Object Computed Properties](#es6-computed-properties)
1. [Template Strings](#es6-template-literals)
1. [Destructuring](#destructuring)
1. [Default Parameters](#es6-default-parameters)
1. [Rest](#es6-rest)
1. [Array Spreads](#es6-array-spreads)
1. [Let and Const](#references)
1. [Exponentiation Operator](#es2016-properties--exponentiation-operator)
1. [Iterators and Generators](#iterators-and-generators)
1. [Modules](#modules)

  <a name="tc39-proposals"></a>
  - [28.2](#tc39-proposals) Do not use [TC39 proposals](https://github.com/tc39/proposals) that have not reached stage 3.

    > Why? [They are not finalized](https://tc39.github.io/process-document/), and they are subject to change or to be withdrawn entirely. We want to use JavaScript, and proposals are not JavaScript yet.

**[⬆ 回到目录](#目录)**

## 标准库

  The [Standard Library](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects)
  contains utilities that are functionally broken but remain for legacy reasons.

  <a name="standard-library--isnan"></a>
  - [29.1](#standard-library--isnan) Use `Number.isNaN` instead of global `isNaN`.
    eslint: [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)

    > Why? The global `isNaN` coerces non-numbers to numbers, returning true for anything that coerces to NaN.
    > If this behavior is desired, make it explicit.

    ```javascript
    // 反例
    isNaN('1.2'); // false
    isNaN('1.2.3'); // true

    // 正确示例
    Number.isNaN('1.2.3'); // false
    Number.isNaN(Number('1.2.3')); // true
    ```

  <a name="standard-library--isfinite"></a>
  - [29.2](#standard-library--isfinite) Use `Number.isFinite` instead of global `isFinite`.
    eslint: [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)

    > Why? The global `isFinite` coerces non-numbers to numbers, returning true for anything that coerces to a finite number.
    > If this behavior is desired, make it explicit.

    ```javascript
    // 反例
    isFinite('2e3'); // true

    // 正确示例
    Number.isFinite('2e3'); // false
    Number.isFinite(parseInt('2e3', 10)); // true
    ```

**[⬆ 回到目录](#目录)**

## 测试

  <a name="testing--for-real"></a><a name="28.2"></a>
  - [30.1](#testing--for-real) **No, but seriously**:
    - 无论使用哪个测试框架，都需要写测试。
    - 尽量写小而美的纯函数。
    - Be cautious about stubs and mocks - they can make your tests more brittle.
    - We primarily use [`mocha`](https://www.npmjs.com/package/mocha) and [`jest`](https://www.npmjs.com/package/jest) at Airbnb. [`tape`](https://www.npmjs.com/package/tape) is also used occasionally for small, separate modules.
    - 100% test coverage is a good goal to strive for, even if it’s not always practical to reach it.
    - Whenever you fix a bug, _write a regression test_. A bug fixed without a regression test is almost certainly going to break again in the future.

**[⬆ 回到目录](#目录)**


## License

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

**[⬆ 回到目录](#目录)**

## 修订



# };
