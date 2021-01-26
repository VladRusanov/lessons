# HomeWork6


0) УЧИТЬ ТЕОРИЮ | УЧИТЬ ТЕОРИЮ | УЧИТЬ ТЕОРИЮ | УЧИТЬ ТЕОРИЮ | УЧИТЬ ТЕОРИЮ  !important



1) Создать функцию которая будет удалять людей из массива по индексу, который мы передадим параметром. 

```
const arr = ['Vasya', 'Petya', 'Alexey']
removeUser(arr, 1)
console.log(arr) /// ['Vasya', 'Alexey']

etc.

```

2) Повторите по данному по образцу (используя JS):

Родительский div можно добавить просто в html файле

![help](https://raw.githubusercontent.com/olgamaslovaolga/Alevel-Markup/master/images/img-hw7.1.png)

3) У вас есть следующий код:

```
// index.html

<div class="holder">

</div>

```

Используя JS, добавить такие блоки в div с классом holder

```
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
  <div class="item">4</div>
  <div class="item">5</div>

```

С помощью стилей привести блоки в такой вид (в стилях только флекс)

![help](https://github.com/olgamaslovaolga/Alevel-Markup/raw/master/images/hw-8.3.png)

4) Напилить код функции modificator, такой, чтобы в результате работы кода:

```
function sampleFunc () {
    console.log ( `${arguments.callee.name}: ${arguments[0]} | ${arguments[1]}` )
}

function modificator ( func ) {
    ...
}

testFunc = modificator( sampleFunc )

testFunc(); // sampleFunc: test | sample

```

5) Создать массив group, элементы которого будут объектами, содержащими данные каждого студента группы

Какие данные - на ваше усмотрение ( например, имя, фамилия, возраст, наличие ноутбука и т.д. )

```
const group = [{
                name: "...",
                lastName: "...",
                age: ...,
                notebook: false,
                ...
              }, 
              {...},
];


```

Создать функцию, которая итерирует массив group, выводя в консоль данные каждого студента одной строкой

( **предварительно преобразовав объект в строку, не забудьте сивол-разделитель** )

```
function getStudentsList ( arrayOfStudents ) {
        ...
}

```
Пример на преобразование:

```
const test = {
  toString() {
    return 'Hello';
  }
};

String(test); // 'Hello'

```

Вывод строки должен произойти после попытки преобразовать объект к строке 


Пример:

Был массив:

```
const group = [{
  name: 'Vlad',
  lastName: 'Test',
  haveLaptop: true
}];

getStudentsList(group); // Name - Vlad,lastName - Test,haveLaptop - true

```

