## Глава 6-7:

---

### Заблуждения по поводу this:

+ ***this не является ссылкой на саму функцию***

```javascript
function foo(num) {
    console.log("foo: " + num);
    // Подсчет вызовов `foo`
    this.count++;
}

foo.count = 0;
var i;
for (i = 0; i < 10; i++) {
    if (i > 5) {
        foo(i);
    }
}
// foo: 6
// foo: 7
// foo: 8
// foo: 9
console.log(foo.count); // 0
```

+ ***this не является ссылкой на лексическую область видимости***

```javascript
function foo() {
    var a = 2;
    this.bar();
}

function bar() {
    console.log(this.a);
}

foo();
//ReferenceError: значение a не определено
```
---
### А как на самом деле?

+ ***Связывание this осуществляется для каждого вызова функции на основании исключительно места вызова (способа вызова)***

---
### Виды связываний this:


+ ***По умолчанию***

```javascript
function foo() {
    console.log(this.a);
}

var a = 2;
foo();
// 2
```

```javascript
function foo() {
    "use strict";
    console.log(this.a);
}

var a = 2;
foo();
// TypeError: `this` is `undefined`
```

+ ***Неявное***

```javascript
function foo() {
    console.log(this.a);
}

var obj = {
    a: 2,
    foo: foo
};

obj.foo();
// 2
```

```js
function foo() {
    console.log(this.a);
}

var obj2 = {
    a: 42,
    foo: foo
};

var obj1 = {
    a: 2,
    obj2: obj2
};

obj1.obj2.foo();
// 42
```

```js
function foo() {
    console.log(this.a);
}

var obj = {
    a: 2,
    foo: foo
};

var a = "oops, global"; // `a` также является свойством 
// глобального объекта
setTimeout(obj.foo, 100); // "oops, global"
```

+ ***Явное (call, apply)***

```js
function foo() {
    console.log(this.a);
}

var obj = {
    a: 2
};

foo.call(obj);
// 2
// Вызов foo с явным связыванием foo.call(..) 
// позволяет принудительно задать его this значение obj
```

+ ***Жесткое (call, apply)***

```js
function foo(something) {
    console.log(this.a, something);
    return this.a + something;
}

// Простая вспомогательная функция `bind`
function bind(fn, obj) {
    return function () {
        return fn.apply(obj, arguments);
    };
}

var obj = {
    a: 2
};

var bar = bind(foo, obj);
var b = bar(3); // 2 3
console.log(b); // 5
```

+ ***New***
    + Создается (конструируется) новый объект.
    + Производится связывание сконструированного объекта
      с [[Prototype]].
    + Сконструированный объект назначается в качестве связывания this для этого вызова функции.
    + Если функция не возвращает свой альтернативный объект,
      вызов функции автоматически возвращает сконструированный объект.

```js
function foo(a) {
    this.a = a;
}

var bar = new foo(2);
console.log(bar.a); // 2
```

+ ***Стрелочные функции***
```js
function foo() {
    setTimeout(() => {
        // `this` здесь лексически наследуется от `foo()`
        console.log(this.a);
    }, 100);
}

var obj = {
    a: 2
};
foo.call(obj); // 2
```  