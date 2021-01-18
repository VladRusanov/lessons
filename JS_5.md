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







