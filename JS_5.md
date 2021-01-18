# Конструкторы, создание объектов через "new"

Обычный синтаксис {...} позволяет создать только один объект. 

Но зачастую нам нужно создать множество однотипных объектов, таких как пользователи, элементы меню и т.д.

Это можно сделать при помощи функции-конструктора и оператора "new".

# Функция-конструктор

Функции-конструкторы являются обычными функциями. Но есть два соглашения:

- Имя функции - конструктора должно начинаться с большой буквы.
- Функция - конструктор должна вызываться при помощи оператора "new".

```
function Person(name, lastName, age, dream) {
    this.name = name;
    this.lastName = lastName;
    this.age = age;
    this.dream = dream;
};

const firstUser = new Person('Vova', 'Testovich', '22', 'Become a ninja');

firstUser.name; // Vova
firstUser.nlastNameame; // Testovich
firstUser.dream; // Become a ninja


```

Когда функция вызывается как new Person(...), происходит следующее:

- Создаётся новый пустой объект, и он присваивается this.
- Выполняется код функции. Обычно он модифицирует this, добавляет туда новые свойства.
- Возвращается значение this.

Другими словами, вызов new User(...) делает примерно вот что:

```
function Person(name, lastName, age, dream) {
  // this = {};  (неявно)

  // добавляет свойства к this
    this.name = name;
    this.lastName = lastName;
    this.age = age;
    this.dream = dream;

  // return this;  (неявно)
}
```

# new.target

Используя специальное свойство new.target внутри функции, мы можем проверить, вызвана ли функция при помощи оператора new или без него.

```
function Person() {
    alert(new.target);
}

// без "new":
Person(); // undefined

// с "new":
new Person(); // function User { ... }

```

# Оператор instanceof

Оператор instanceof позволяет проверить, с помощью какого конструктора создан объект.

Если объект создан с помощью определенного конструктора, то оператор возвращает true:

```
function User(...) {...}

const tom = new User("Том", 26);
 
const isUser = tom instanceof User;
const isCar = tom instanceof Car;
console.log(isUser);    // true
console.log(isCar);     // false

```

# Добавление метода к конструктору объекта

Функция конструктора также может определять методы:

```
function Person(first, last, age, eyecolor) {
    this.firstName = first;
    this.lastName = last;
    this.age = age;
    this.eyeColor = eyecolor;
    this.name = function() {return this.firstName + " " + this.lastName;};
}

```

Нельзя добавлять новые методы к конструктору объекта тем же способом, как это делается в случае с существующим объектом. 

Добавление методов к объекту должно происходить внутри функции конструктора:

```
function Person(firstName, lastName, age, eyeColor) {
    this.firstName = firstName;  
    this.lastName = lastName;
    this.age = age;
    this.eyeColor = eyeColor;
    this.changeAge = function (newAge) {
        this.age = newAge;
    };
} 

const person1 = new Person('Vova', 'Testovich', 10, grey);
person1.age; // 10
person1.changeName(11);
person1.age; // 11

```

JavaScript знает, о каком объекте идет речь, "подставляя" в ключевое слово this объект 

# F.prototype

```
let animal = {
  eats: true
};

function Rabbit(name) {
  this.name = name;
}

Rabbit.prototype = animal;

let rabbit = new Rabbit("White Rabbit"); //  rabbit.__proto__ == animal

alert( rabbit.eats ); // true

```

Если в F.prototype содержится объект, оператор new устанавливает его в качестве [[Prototype]] для нового объекта.

Обратите внимание, что F.prototype означает обычное свойство с именем "prototype" для F. 

Это ещё не «прототип объекта», а обычное свойство F с таким именем.

Установка Rabbit.prototype = animal буквально говорит интерпретатору следующее: 

"При создании объекта через new Rabbit() запиши ему animal в [[Prototype]]".

F.prototype используется только в момент вызова new F()


# F.prototype по умолчанию, свойство constructor

У каждой функции по умолчанию уже есть свойство "prototype".

По умолчанию "prototype" – объект с единственным свойством constructor, которое ссылается на функцию-конструктор.

```
function Rabbit() {}

/* прототип по умолчанию
Rabbit.prototype = { constructor: Rabbit };
*/

```

Проверим это:

```
function Rabbit() {}
// по умолчанию:
// Rabbit.prototype = { constructor: Rabbit }

alert( Rabbit.prototype.constructor == Rabbit ); // true

```

Соответственно, если мы ничего не меняем, то свойство constructor будет доступно всем кроликам через [[Prototype]]:

```
function Rabbit() {}
// по умолчанию:
// Rabbit.prototype = { constructor: Rabbit }

let rabbit = new Rabbit(); // наследует от {constructor: Rabbit}

alert(rabbit.constructor == Rabbit); // true (свойство получено из прототипа)

```

**JavaScript сам по себе не гарантирует правильное значение свойства "constructor".**

```
function Rabbit() {}
Rabbit.prototype = {
  jumps: true
};

let rabbit = new Rabbit();
alert(rabbit.constructor === Rabbit); // false

```








