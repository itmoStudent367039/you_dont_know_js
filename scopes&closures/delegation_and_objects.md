## Глава 11:

---

+ ***Наследование против делегирования***

```js
// OLOO-programming style
Task = {
    setID: function (ID) {
        this.id = ID;
    },
    outputID: function () {
        console.log(this.id);
    }
};
// `XYZ` делегирует обращения `Task`
XYZ = Object.create(Task);
XYZ.prepareTask = function (ID, Label) {
    this.setID(ID);
    this.label = Label;
};
XYZ.outputTaskDetails = function () {
    this.outputID();
    console.log(this.label);
};

// ABC = Object.create( Task );
// ABC ... = ...
```

+ ***С паттерном проектирования "делегация"***
    + Всеми возможными способами нужно предотвратить совпадение имен на разных уровнях цепочки [[Prototype]] => случаи
      при конфликте имен рассмотрены выше

+ ***Взаимное делегирование запрещено!***
    + Если вы обратитесь к свойству/методу, которые не существуют в одном из мест, возникнет бесконечная рекурсия в
      цикле [[Prototype]]. Но если все ссылки
      присутствуют на своих местах, то B может делегировать A и наоборот, и такое решение может работать. Таким образом,
      любой
      объект сможет делегировать обращения другому объекту для
      различных целей. В некоторых узкоспециализированных ситуациях это может быть полезно.

+ ***OLOO vs OO:***

```js
function Foo(who) {
    this.me = who;
}

Foo.prototype.identify = function () {
    return "I am " + this.me;
};

function Bar(who) {
    Foo.call(this, who);
}

Bar.prototype = Object.create(Foo.prototype);
Bar.prototype.speak = function () {
    alert("Hello, " + this.identify() + ".");
};
var b1 = new Bar("b1");
var b2 = new Bar("b2");
b1.speak();
b2.speak();
```

```js
Foo = {
    init: function (who) {
        this.me = who;
    },

    identify: function () {
        return "I am " + this.me;
    }
};

Bar = Object.create(Foo);
Bar.speak = function () {
    alert("Hello, " + this.identify() + ".");
};

var b1 = Object.create(Bar);
b1.init("b1");

var b2 = Object.create(Bar);
b2.init("b2");

b1.speak();
b2.speak();
```

+ ***OOP since ES6 vs OLOO:***

```js
class Widget {
    constructor(width, height) {
        this.width = width || 50;
        this.height = height || 50;
        this.$elem = null;
    }

    render($where) {
        if (this.$elem) {
            this.$elem.css({
                width: this.width + "px",
                height: this.height + "px"
            }).appendTo($where);
        }
    }
}

class Button extends Widget {
    constructor(width, height, label) {
        super(width, height);
        this.label = label || "Default";
        this.$elem = $("<button>").text(this.label);
    }

    render($where) {
        super($where);
        this.$elem.click(this.onClick.bind(this));
    }

    onClick(evt) {
        console.log("Button ‘" + this.label + "’ clicked!");
    }
}

$(document).ready(function () {
    var $body = $(document.body);
    var btn1 = new Button(125, 30, "Hello");
    var btn2 = new Button(150, 40, "World");
    btn1.render($body);
    btn2.render($body);
});
```

```js
var Widget = {
    init: function (width, height) {
        this.width = width || 50;
        this.height = height || 50;
        this.$elem = null;
    },
    insert: function ($where) {
        if (this.$elem) {
            this.$elem.css({
                width: this.width + "px",
                height: this.height + "px"
            }).appendTo($where);
        }
    }
};

var Button = Object.create(Widget);
Button.setup = function (width, height, label) {
    // делегированный вызов
    this.init(width, height);
    this.label = label || "Default";
    this.$elem = $("<button>").text(this.label);
};
Button.build = function ($where) {
    // делегированный вызов
    this.insert($where);
    this.$elem.click(this.onClick.bind(this));
};
Button.onClick = function (evt) {
    console.log("Button '" + this.label + "' clicked!");
};
$(document).ready(function () {
    var $body = $(document.body);
    var btn1 = Object.create(Button);
    btn1.setup(125, 30, "Hello");
    var btn2 = Object.create(Button);
    btn2.setup(150, 40, "World");
    btn1.build($body);
    btn2.build($body);
});
```