# Liskov substitution principle
```
// class Person {
//
// }
//
// class Member extends Person {
//   access() {
//     console.log('У тебя есть доступ')
//   }
// }
//
// class Guest extends Person {
//   isGuest = true
// }
//
// class Frontend extends Member {
//   canCreateFrontend() {}
// }
//
// class Backend extends Member {
//   canCreateBackend() {}
// }
//
// class PersonFromDifferentCompany extends Guest {
//   access() {
//     throw new Error('У тебя нет доступа! Иди к себе!')
//   }
// }
//
// function openSecretDoor(member) {
//   member.access()
// }
//
// openSecretDoor(new Frontend())
// openSecretDoor(new Backend())
// openSecretDoor(new PersonFromDifferentCompany())  // There should be member!

// ===============

class Component {
  isComponent = true
}

class ComponentWithTemplate extends Component {
  render() {
    return `<div>Component</div>`
  }
}

class HigherOrderComponent extends Component {

}

class HeaderComponent extends ComponentWithTemplate {
  onInit() {}
}

class FooterComponent extends ComponentWithTemplate {
  afterInit() {}
}

class HOC extends HigherOrderComponent {
  render() {
    throw new Error('Render is impossible here')
  }

  wrapComponent(component) {
    component.wrapped = true
    return component
  }
}

function renderComponent(component) {
  console.log(component.render())
}


renderComponent(new HeaderComponent())
renderComponent(new FooterComponent())

```

# Blob

ArrayBuffer и бинарные массивы являются частью ECMA-стандарта и, соответственно, частью JavaScript.

Кроме того, в браузере имеются дополнительные высокоуровневые объекты, описанные в File API.

**Объект Blob состоит из необязательной строки type (обычно MIME-тип) и blobParts – последовательности других объектов Blob, строк и BufferSource.**

![Ing](https://learn.javascript.ru/article/blob/blob.svg)


Благодаря type мы можем загружать и скачивать Blob-объекты, где type естественно становится Content-Type в сетевых запросах.

Конструктор имеет следующий синтаксис:

```
new Blob(blobParts, options);

```

- blobParts – массив значений Blob/BufferSource/String.

- options – необязательный объект с дополнительными настройками:

type – тип объекта, обычно MIME-тип, например. image/png,

```
// создадим Blob из строки
let blob = new Blob(["<html>…</html>"], {type: 'text/html'});
// обратите внимание: первый аргумент должен быть массивом [...]

```

```
// создадим Blob из типизированного массива и строк
let hello = new Uint8Array([72, 101, 108, 108, 111]); // "hello" в бинарной форме

let blob = new Blob([hello, ' ', 'world'], {type: 'text/plain'});

```

Мы можем получить срез Blob, используя:


```
blob.slice([byteStart], [byteEnd], [contentType]);


```

- byteStart – стартовая позиция байта, по умолчанию 0.
- byteEnd – последний байт, по умолчанию до конца.
- contentType – тип type создаваемого Blob-объекта, по умолчанию такой же, как и исходный.

# File и FileReader


