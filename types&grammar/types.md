+ ***Что выведет код?***

```js
typeof undefined // "undefined"

typeof 0 // "number"

typeof 10n // "bigint"

typeof true // "boolean"

typeof "foo" // "string"

typeof Symbol("id") // "symbol"

typeof Math // "object"  (1)

typeof null // "object"  (2)

typeof alert // "function"  (3)
```

---
+ ***Что выведет код?***

```js
"" + 1 + 0 = "10"

"" - 1 + 0 = -1

true + false = 1

6 / "3" = 2

"2" * "3" = 6

4 + 5 + "px" = "9px"

"$" + 4 + 5 = "$45"

"4" - 2 = 2

"4px" - 2 = NaN

"  -9  " + 5 = "  -9  5"

"  -9  " - 5 = -14

null + 1 = 1

undefined + 1 = NaN

" \t \n" - 2 = -2
```

---

+ ***Что выведет код?***

```js 
5 > 4 => true

"ананас" > "яблоко" => false

"2" > "12" => true

undefined == null => true

undefined === null => false

null == "\n0\n" => false

null === +"\n0\n" => false
```

---

+ ***Операторы (`!`, `!!`)***

```js
alert( !true ); // false
alert( !0 ); // true

alert( !!"непустая строка" ); // true
alert( !!null ); // false

alert( Boolean("непустая строка") ); // true
alert( Boolean(null) ); // false
```
---

+ ***Что выведет код: alert?***
```js
alert( null || 2 || undefined ); // 2

alert( alert(1) || 2 || alert(3) ); // 1 2

alert( 1 && null && 2 ); // null

alert( alert(1) && alert(2) ); // 1 undefined

alert( null || 2 && 3 || 4 ); // (2 && 3 == 3) => null || 3 || 4 == 3
```
---

+ ***Что выведет код: (||=, &&=)?***

```js
let value = NaN;

value &&= 10;
value ||= 20;
value &&= 30;
value ||= 40;

alert(value);

// 30

// (bool &&= val, not working, if (!bool))
```

---
+ ***Какие условия выполнятся?***

```js
// Выполнится.
// Результат -1 || 0 = -1, в логическом контексте true
if (-1 || 0) alert( 'first' );

// Не выполнится
// -1 && 0 = 0,  в логическом контексте false
if (-1 && 0) alert( 'second' );

// Выполнится
// оператор && имеет больший приоритет, чем ||
// так что -1 && 1 выполнится раньше
// вычисления: null || -1 && 1  ->  null || 1  ->  1
if (null || -1 && 1) alert( 'third' );
```

---

+ ***Можно ли изменить объект, объявленный через const?***
```js
const user = {
  name: "John"
};

user.name = "Pete";
```

---

+ ***Функция на js, которая принимает строку и возвращает ее, делая первый символ заглавным***
```js
function upFirst(str) {
  if (!str) return str;

  return str[0].toUpperCase() + str.slice(1);
}

alert( ucFirst("вася") ); // Вася
// Или
str.charAt(0) // всегда возвращает строку
```
---
+ ***Как обойти неточность чисел с плавающей запятой?***
```js
let sum = 0.1 + 0.2;
alert( +sum.toFixed(2) ); // 0.3
```
---
+ ***Как округлить число?***
```js
alert( 1.35.toFixed(1) ); // 1.4

alert( 6.35.toFixed(1) ); // 6.3
alert( (6.35 * 10).toFixed(20) ); // 63.50000000000000000000
alert( Math.round(6.35 * 10) / 10 ); // 6.35 -> 63.5 -> 64(rounded) -> 6.4
```
---

+ ***Этот цикл бесконечный, почему?***
```js
let i = 0;
while (i != 10) {
  i += 0.2;
}

// let i = 0;
// while (i < 11) {
//     i += 0.2;
//     if (i > 9.8 && i < 10.2) alert( i );
// }
```
---

+ ***Что выведет следующий код?***
```js
let fruits = ["Яблоки", "Груша", "Апельсин"];

// добавляем новое значение в "копию"
let shoppingCart = fruits;
shoppingCart.push("Банан");

// что в fruits?
alert( fruits.length ); // ?

// 4
```
---

+ ***Каков результат? Почему?***
```js
let arr = ["a", "b"];

arr.push(function() {
  alert( this );
});

arr[2](); // ?

// a, b, function(){...}
```
---
+ ***Как отличить массив от объекта?***
```js
alert(typeof {}); // object
alert(typeof []); // object
```
---

+ ***Напишите функцию camelize(str), которая преобразует строки вида «my-short-string» в «myShortString»:***

```js
function camelize(str) {
  return str
    .split('-') // разбивает 'my-long-word' на массив ['my', 'long', 'word']
    .map(
      // Переводит в верхний регистр первые буквы всех элементом массива за исключением первого
      // превращает ['my', 'long', 'word'] в ['my', 'Long', 'Word']
      (word, index) => index == 0 ? word : word[0].toUpperCase() + word.slice(1)
    )
    .join(''); // соединяет ['my', 'Long', 'Word'] в 'myLongWord'
}
```
---

+ ***Напишите функцию, которая возвращает скопированный и отсортированный массив:***
```js
function copySorted(arr) {
  return arr.slice().sort();
}

let arr = ["HTML", "JavaScript", "CSS"];

let sorted = copySorted(arr);

alert( sorted );
alert( arr );
```

---

+ ***Функция, которая переводит массив объектов в массив string***
```js
let vasya = { name: "Вася", age: 25 };
let petya = { name: "Петя", age: 30 };
let masha = { name: "Маша", age: 28 };

let users = [ vasya, petya, masha ];

let names = users.map(item => item.name);

alert( names ); // Вася, Петя, Маша
```
---

+ ***Реализуйте функцию shuffle в js (перемешать массив):***
```js
// Тасование Фишера-Йетса
function shuffle(array) {
  for (let i = array.length - 1; i > 0; i--) {
    let j = Math.floor(Math.random() * (i + 1)); // случайный индекс от 0 до i
    // поменять элементы местами
    // мы используем для этого синтаксис "деструктурирующее присваивание"
    // подробнее о нём - в следующих главах
    // то же самое можно записать как:
    // let t = array[i]; array[i] = array[j]; array[j] = t
    [array[i], array[j]] = [array[j], array[i]];
  }
}
```
---

+ ***Фильтрация уникальных элементов массива:***
```js
function unique(arr) {
  return Array.from(new Set(arr));
}
```
---

+ ***Что выведет?***

```js
    a = ["42"]
    b = "42"
    c = 42
    console.log(a == c);
    // true
```

---
+ ***Что выведет?***

```js
    a = ["42"]
    b = "42"
    c = 42
    console.log(a == b);
    // true
```

---

+ ***Что выведет?***

```js
    a = ["42"]
    b = "42"
    c = 42
    console.log(a === b);
    // false
```

---
+ ***Что выведет?***
```js
    a = ["42", "32"]
    b = [42, 32]
    console.log(a == b);
    // false
```
---

+ ***Отфильтруйте анаграммы***

```js
    function aclean(arr) {
    let map = new Map();

    for (let word of arr) {
        // разбиваем слово на буквы, сортируем и объединяем снова в строку
        let sorted = arr[i] // PAN
            .toLowerCase() // pan
            .split("") // ["p","a","n"]
            .sort() // ["a","n","p"]
            .join(""); // anp
        map.set(sorted, word);
    }

    return Array.from(map.values());
}

let arr = ["nap", "teachers", "cheaters", "PAN", "ear", "era", "hectares"];

alert( aclean(arr) );
// "nap,teachers,ear" или "PAN,cheaters,era"
```