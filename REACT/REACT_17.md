Повторим паатеры

Что за паттерн?

```
class Person {
  constructor() {
    if (typeof Person.instance === 'object') {
      return Person.instance;
    }
    Person.instance = this;
    return this;
  }
}

```

Напишите мне фабирку и абстрактную фабрику)

