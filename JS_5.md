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

# Свойство constructor

Свойство constructor - содержит ссылку на конструктор, которым объект был создан.

```
[].constructor; //Array;
({}).constructor; //Object;

function User() {};
new User().constructor; //User

```


```
function User() {};

alert(User.prototype.constructor); //User

User.prototype = {
	sayHi: function(){}
};

alert(User.prototype.constructor); //Object

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

# Возврат значения из конструктора return

Обычно конструкторы ничего не возвращают явно. Их задача – записать все необходимое в this, который в итоге станет результатом.

Но если return всё же есть, то применяется простое правило:

- При вызове return с объектом, будет возвращён объект, а не this.
- При вызове return с примитивным значением, примитивное значение будет отброшено.

```
function Person() {

  this.name = "Вася";

  return { name: "Godzilla" };  // <-- возвращает этот объект
}

alert( new Person().name );  // Godzilla, получили этот объект


function Person1() {

  this.name = "Вася";

  return; // <-- возвращает this
}

alert( new Person1().name );  // Вася

```

# Отсутствие скобок

```
let user = new User; // <-- без скобок
// то же, что и
let user = new User();

```


# Prototype для нативных объектов

Через prototype мы можем создавать свои методы для нативных объектов таких как Array, Object и т.д.

Давайте сделать свой метод для массива

```
Array.prototype.sayHello = function () {
    console.log('Hello! Im array');
};

const arr = [1, 2, 3];
arr.sayHello(); // Hello! Im array

```

Давайте сделаем свой метод для объекта

```
Object.prototype.getName = function (params) {
    console.log('Hello! Im object');
};

const person = {
    test1: 1,
    test2: 2,
};

person.getName();

```

HomeWork:

0. Доделать прошлую домашку)

1. Дан объект с городами и странами.

Написать функцию getCity. Эта функция (getCity) должна вернуть новый массив, элементы которого будут преобразованы в данный формат: <Столица> - это <Страна>.

Доступ к объекту может быть любым (через контекст, напрямую и т.д.)

Можно использовать Object.entries метод )

```
const citiesAndCountries = {
	'Киев': 'Украина',
	'Нью-Йорк': 'США',
	'Амстердам': 'Нидерланды',
	'Берлин': 'Германия',
	'Париж': 'Франция',
	'Лиссабон': 'Португалия',
	'Вена': 'Австрия',
};

const result = getCity(); // ['Киев - это Украина', 'Нью-Йорк - это США', ... и т.д.]

```

2. Cоздать объект с названиями дней недели. Где ключами будут ru и en, a значением свойства ru будет массив с названиями дней недели на русском, а en - на английском. 

После написать функцию которая будет выводить в консоль название дня недели пользуясь выше созданным объектом (доступ к объекту можно получить напрямую). 

Все дни недели начинаются с 1 и заканичаются цифрой 7 (1- понедельник, 7 - воскресенье). 

Функция принимает в аргументы 2 параметра: 

 - lang - название языка дня недели
 - day - число дня недели

Можно вспомнить про метод indexOf(). А может можно и без него :)

```
const namesOfDays = {
    ru: ['Понедельник', 'Вторник', 'Среда', ... , 'Воскресенье'],
    en: ['Monday', 'Tuesday', 'Wednesday', ... , 'Sunday'],
}

------------------------------------------------

// Пример 1

function getNameOfDay(lang, datNumber){
    ... Your code
}

getNameOfDay('en', 7) // 'Sunday'

------------------------------------------------

// Пример 2

function getNameOfDay(lang, datNumber){
    
    ... Your code
}

getNameOfDay('ru', 3) // 'Среда'

```

3. Написать универсальную функцию setProto, которая принимает в себя 2 аргумента (currentObj, protoObj). Функция должна устанавливать прототип (protoObj) для currentObj. То есть после вызова функции мы должны получить результат:

```
const person = {
    name: 'Vlad'
};


const person1 = {
    age: 1
};

function setProto (currentObj, protoObj) {
    // code
}

setProto(person1, person);
// Теперь прототипом для объекта person1 выступает объект person

```

4. Создать базовый объек person. Этот объект должен выступать в роли прототипа для объекта person1. 

В объекте person должны быть такие методы:
 - метод для **установки** имени и возвраста (setName, setAge)
 - метод для **получения** имени и возвраста (getName, getAge)
 - метод для валидации возраста (ageValidation)

```
person1.setName(...); // установили новое имя
person1.getName(); // имя

person1.setAge(...); // установили возраст
person1.getAge(); // получили возраст

```

Метод ageValidation вызывается при вывозе метода setAge (то есть внутри метода setAge). Метод ageValidation должен как-то проверить возраст, который мы вводим в setAge. Если возраст, который мы ввели, меньше 18, то записываем в age слово 'Validation Error', а есть введенный возраст больше 18, то вписываем в age это значение.

ageValidation только проверяет данные, он ничего не записывает (в ageValidation не должно быть this.age = age)

```
person1.setAge(1); // передать возраст можно как угодно
person1.getAge(); // 'Validation Error'

person1.setAge(20); // передать возраст можно как угодно
person1.getAge(); // Новое значение - 20

```




