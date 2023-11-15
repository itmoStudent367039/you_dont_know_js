## Глава 1-5:

---

1. ***Hoisting:***

```javascript
{
    console.log(bar);
    let bar = 2;
}

// ReferenceError!
```

```javascript
{
    console.log(bar);
    var bar = 2;
}

// Undefined!
```

```javascript
{
    a = 2;
    var a;
    console.log(a);
}

// 2
```

```javascript
foo();

function foo() {
    console.log(a);
    var a = 2;
}

// undefined
```

```javascript
foo();
var foo = function bar() {

};

// foo(); - не ReferenceError, но TypeError!

var foo;
foo();
foo = function bar() {

};
```

```javascript
var foo;
foo(); // TypeError
bar(); // ReferenceError
foo = function () {
    var bar = this;
}
```

---

+ ***Дупликаты (Сначала поднимаются функции, потом переменные). Последующие объявления переопределяют
  предыдущие.***

```javascript
foo();
var foo;

function foo() {
    console.log(1);
}

foo = function () {
    console.log(2);
};

// 1
```

```javascript
function foo() {
    console.log(1);
}

foo();
foo = function () {
    console.log(2);
};

// 1
```

---

+ ***Объявления в условиях, работают также как в обычных блоках кода***

```javascript
foo();
var a = true;
if (a) {
    function foo() {
        console.log("a");
    }
} else {
    function foo() {
        console.log("b");
    }
}

// "b"
// Нежелательно объявлять функции в блоках кода!
```

---

2. ***Closures:***

+ ***Замыкание — способность функции запоминать свою лексическую область видимости и обращаться к ней даже тогда, когда
  функция выполняется вне своей лексической области видимости.***


+ ***Каждый раз, когда вы передаете функцию обратного вызова, будьте готовы к тому, что к ней
  прилагается замыкание!***

```javascript
function foo() {
    let a = 2;

    function bar() {
        console.log(a);
    }

    return bar;
}

var baz = foo();
baz();

// 2 
// bar() содержит ссылку на область видимости foo()
```

---

2. ***Closures and circles:***

```javascript
for (var i = 1; i <= 5; i++) {
    setTimeout(function timer() {
        console.log(i);
    }, i * 1000);
}

// 6 6 6 6 6 

// По правилам работы области действий все пять функций, 
// хотя они и определяются отдельно при каждой итерации цикла, 
// замыкаются над одной общей глобальной областью видимости, 
// которая на самом деле содержит только один экземпляр i.
```

```javascript
for (var i = 1; i <= 5; i++) {
    (function () {
        var j = i;
        setTimeout(function timer() {
            console.log(j);
        }, j * 1000);
    })();
}
// Каждая функция callback - timer() замыкается над своей (новой) собственной областью видимости уровня итерации IIFE
// На каждой итерации создается собственная копия j = i;
```

```javascript
for (let i = 1; i <= 5; i++) {
    setTimeout(function timer() {
        console.log(i);
    }, i * 1000);
}
```
---