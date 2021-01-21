# Глобальные объекты

В браузере это объект window

```
var foo = 42;
console.log(window.foo); // 42
 
const bar = 'top kek';
console.log(window.bar); // undefined
 
window.baz = 'JavaScript <3';
console.log(baz); // JavaScript <3

```

В Node.js тоже есть глобальный объект global

```
var foo = 42;
console.log(global.foo); // 42

```

# Объекты в JavaScript

- Нативные объекты
- Хост объекты
- Пользовательские объекты

# Нативные объекты

Нативными (native object) объектами в JS называют объекты, свойства и поведение которых описаны в спецификации языка JavaScript. 

Их наличие не зависит от того окружения, где запускается код.

```
 Object, Array, Date, Math
 
```
# Хост объекты

Хост (host object) объектами в JS называют объекты, которые предоставляются окружением (зависят от того, где работает код)

```
Например, для браузеров это будут document, location, history, XMLHttpRequest, setTimeout, setInterval

```

# Литерал объекта

Объявление через {...} называют литералом объекта или литеральной нотацией.

# Прототипное наследование

Например, у нас есть объект user со своими свойствами и методами, и мы хотим создать объекты admin и guest как его слегка изменённые варианты. 

Мы хотели бы повторно использовать то, что есть у объекта user, не копировать/переопределять его методы, а просто создать новый объект на его основе.

Прототипное наследование — это возможность языка, которая помогает в этом.

# [[Prototype]]

В JavaScript объекты имеют специальное скрытое свойство [[Prototype]] (так оно названо в спецификации), которое либо равно null, либо ссылается на другой объект. Этот объект называется «прототип»:

Свойство [[Prototype]] является внутренним и скрытым, но есть много способов задать его.

Одним из них является использование __proto__, например так:

```

const animal = {
    eats: true
};

const rabbit = {
    jumps: true,
};

const lions = {
    roar: true,
};

rabbit.__proto__ = animal;
lions.__proto__ = animal;

rabbit.jumps; // true
rabbit.eats; // true

```

Так же можно наследовать методы:

```
const animal = {
    eats: true,
    walk() {
        console.log("Walk");
    }
};

const rabbit = {
    jumps: true,
};

const lions = {
    roar: true,
};

rabbit.__proto__ = animal;
lions.__proto__ = animal;

rabbit.walk(); // Walk
rabbit.eats; // true

```

```
let animal = {
    eats: true,
    walk() {
        alert("Animal walk");
    }
};

let rabbit = {
    jumps: true,
    __proto__: animal
};

// walk взят из прототипа
rabbit.walk(); // Animal walk

```

# Операция записи не использует прототип

В приведённом ниже примере мы присваиваем rabbit собственный метод walk:

```
let animal = {
  eats: true,
  walk() {
    /* этот метод не будет использоваться в rabbit */
  }
};

let rabbit = {
  __proto__: animal
};

rabbit.walk = function() {
  alert("Rabbit! Bounce-bounce!");
};

rabbit.walk(); // Rabbit! Bounce-bounce!

```

Теперь вызов rabbit.walk() находит метод непосредственно в объекте и выполняет его, не используя прототип


# Значение «this»

Неважно, где находится метод: в объекте или его прототипе. 

При вызове метода this — всегда объект перед точкой.


```
// методы animal
let animal = {
    walk() {
        if (!this.isSleeping) {
            alert(`I walk`);
        }
    },
    sleep() {
        this.isSleeping = true;
    }
};

let rabbit = {
    name: "White Rabbit",
    __proto__: animal
};

// модифицирует rabbit.isSleeping
rabbit.sleep();

alert(rabbit.isSleeping); // true
alert(animal.isSleeping); // undefined (нет такого свойства в прототипе)

```


# Методы конструктора Object

- Object.assign()

Создаёт новый объект путём копирования значений всех собственных перечислимых свойств из одного или более исходных объектов в целевой объект.

```
const obj = { a: 1 };
const copy = Object.assign({}, obj);
console.log(copy); // { a: 1 }

```

- Object.create()

Создаёт новый объект с указанными объектом прототипа и свойствами.

```
const human = {
    test: true,
};

const Vova = Object.create(human);

// Vova.__proto__ === human

```

- Object.freeze()

Замораживает объект: другой код не сможет удалить или изменить никакое свойство.

```
const person = {
    name: 'Petya',
};

person.age = 5;

const person1 = Object.freeze(person);
person1.test = '123';
console.log(person1); // {  name: 'Petya', age: 5  }. Свойство test не записалось

```

- Object.getOwnPropertyNames()

Возвращает массив, содержащий имена всех переданных объекту **собственных** перечисляемых и неперечисляемых свойств

```
const object1 = {
  a: 1,
  b: 2,
  c: 3
};

console.log(Object.getOwnPropertyNames(object1));
// expected output: Array ["a", "b", "c"]

```

- Object.keys()

Возвращает массив, содержащий имена всех **собственных** перечислимых свойств переданного объекта.

```
const arr = ['a', 'b', 'c'];
console.log(Object.keys(arr)); // консоль: ['0', '1', '2']

const person = {
 name: 'Test',
 age: '100'
};

console.log(Object.keys(person)); // консоль: ['name', 'age']

```

- Object.values()

Возвращает массив, содержащий value всех **собственных** перечислимых свойств переданного объекта.

```
const person = {
    name: 'Petya',
    age: 20
};

console.log(Object.values(person)); // ['Petya', 20]

```

# HomeWork

1. Написать функцию bindFunc, которая принимает в себя 2 + аргументов (Точно должна принять 2 аргумента, а дальше сколько угодно). 

- 1 аргумент - какая-то функция

- 2 аргумент - значение контекста

- 3 + ... аргументы - любое кол-во аргументов

Эта функция, должна устанавливать контекст для функции, которая в первом аргументе, **и возвращать эту функцию с новым контекстом**. 

Сам контекст, который мы хотим установить, находиться во втором аргументе

2. Написать функцию, которая **не принимает никаких аргументов**. В теле функции написать логику для нахождения суммы значений любого количества ключей **(значения ключей должны быть больше нуля)** из переданного контекста.

Обращаться к objectA напрямую нельзя :)

Пример

```
const func = function() {
 this.a + this.b + .....
}

const objectA = {
 a: 1,
 b: 2,
 c: 3,
}

```

3. Написать функцию, которая возвращает новый массив, в котором должны быть только четные **числа**, которые больше двуx и меньше 10. Новый массив будет состоять из значений ключа values из контекста, если такого ключа нет, то выводим сообщение "Не найдено". 

Обращаться к valObject0 напрямую нельзя :)

Если хотите использовать map, то внутри map this всегда равен глобальному объекту. Чтобы это поменять передаем нужное значение this во второй аргумент map - 

arr.map(() => {}, this);

Пример:

```
function getNewArray() {
 ....
};

const valObject0 = {
 values: [1, '2', 4, 8, '8',  3, 10, null, false],
};


const result = getNewArray...; // Ссылаясь на массив ключа values из valObject0 [4, 8]

```

