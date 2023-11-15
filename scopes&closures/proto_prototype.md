## Глава 10:

---

+ ***[[Prototype]] - свойство у каждого объекта, которое ссылается на прототип***


+ ***[[GET]], in, for(... in...) - используют цепочку прототипов***


+ ***Цепочка [[Prototype]] завершается на встроенном объекте Object.prototype***


+ ***Присваивание [[PUT]] (myObject.foo = 'bar'):***
  + Если myObject содержит ['foo'], то происходит изменение значения существующего свойства;
  + Если ['foo'] отсутствует, то происходит обход цепочки [[Prototype]]:
    + Если ['foo'] не было найдено нигде в цепочке, то ['foo'] добавляется прямо в myObject;
    + Если ['foo'] находится в цепочке [[Prototype]]:
      + (writable:true) - ['foo'] добавляется к myObject (замещение);
      + (writable:false) - (use strict => throw Error или игнорирование присваивания);
      + Если ['foo'] имеет сеттер, то он будет всегда вызываться, у объекта прототипа, сеттер myObject переопределяться не будет;
      + Чтобы однозначно присвоить ['foo'] myObject - Object.defineProperty(...);
```js
myObject.foo = 'bar';
```

```js
var anotherObject = {
  a: 2
};

var myObject = Object.create(anotherObject);

anotherObject.a; // 2
myObject.a; // 2

anotherObject.hasOwnProperty("a"); // true
myObject.hasOwnProperty("a"); // false

myObject.a++; // неявное замещение!

anotherObject.a; // 2
myObject.a; // 3

myObject.hasOwnProperty("a"); // true
```

+ ***Прототипное наследование***

```js
function Foo(name) {
  this.name = name;
}

Foo.prototype.myName = function () {
  return this.name;
};

function Bar(name, label) {
  Foo.call(this, name);
  this.label = label;
}

// здесь мы создаем новый объект `Bar.prototype`,
// связанный с `Foo.prototype`
Bar.prototype = Object.create(Foo.prototype);
// Внимание! Значение `Bar.prototype.constructor` исчезает.
// Возможно, вам придется вручную "исправить" его, если
// вы привыкли полагаться на такие свойства!
Bar.prototype.myLabel = function () {
  return this.label;
};
var a = new Bar("a", "obj a");
a.myName(); // "a"
a.myLabel(); // "obj a"
```

+ ***Сравним стандартные методы связывания Bar.prototype с Foo.
  prototype до ES6 и в ES6:***

```js
// до ES6
// теряет существующий объект `Bar.prototype` по умолчанию
Bar.prototype = Object.create( Foo.prototype );

// ES6+
// изменяет существующий объект `Bar.prototype`
Object.setPrototypeOf( Bar.prototype, Foo.prototype );
```

+ ***Анализ связей [[Prototype]]:***

```js
Foo.prototype.isPrototypeOf(a); // true
// встречается ли во всей цепочке [[Prototype]] объекта 
// {a} объект Foo.prototype ?
```

```js
// Встречается ли b где-то в цепочке
// [[Prototype]] объекта c?
b.isPrototypeOf(c);
```

+ ***Получить цепочку [[Prototype]]:***

```js
// ES5
Object.getPrototypeOf(a) === Foo.prototype; // true

// Base since ES6
a.__proto__ === Foo.prototype; // true 
// (получает внутреннее свойство [[Prototype]] объекта в виде ссылки)
```

```js
// Simple realization
Object.defineProperty( Object.prototype, "__proto__", {
 get: function() {
 return Object.getPrototypeOf( this );
 },
 set: function(o) {
 // setPrototypeOf(..) as of ES6
 Object.setPrototypeOf( this, o );
 return o;
 }
} );
```

+ ***Object.create(..):***

```js
// ES5
var foo = {
    something: function () {
        console.log("Tell me something good...");
    }
};
var bar = Object.create(foo);
bar.something(); // Tell me something good...
```

```js
// Simple realization
if (!Object.create) {
    Object.create = function (o) {
        function F() {
        }

        F.prototype = o;
        return new F();
    };
}
```