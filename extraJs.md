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


**Blob не изменяем**

# File и FileReader

Объект File наследуется от объекта Blob и обладает возможностями по взаимодействию с файловой системой.

Есть два способа его получить.

Во-первых, есть конструктор, похожий на Blob:

```
new File(fileParts, fileName, [options])

```

- fileParts – массив значений Blob/BufferSource/строки.
- fileName – имя файла, строка.
- options – необязательный объект со свойством:
lastModified – дата последнего изменения в формате таймстамп (целое число).

Во-вторых, чаще всего мы получаем файл из <input type="file"> или через перетаскивание с помощью мыши, или из других интерфейсов браузера. В этом случае файл получает эту информацию из ОС.

Так как File наследует от Blob, у объектов File есть те же свойства плюс:

- name – имя файла,
- lastModified – таймстамп для даты последнего изменения.

В этом примере мы получаем объект File из <input type="file">:


```
<input type="file" onchange="showFile(this)">

<script>
function showFile(input) {
  let file = input.files[0];

  alert(`File name: ${file.name}`); // например, my.png
  alert(`Last modified: ${file.lastModified}`); // например, 1552830408824
}
</script>

```

# FileReader

FileReader объект, цель которого читать данные из Blob (и, следовательно, из File тоже).

Данные передаются при помощи событий, так как чтение с диска может занять время.


```
let reader = new FileReader(); // без аргументов


```
Основные методы:

- readAsArrayBuffer(blob) – считать данные как ArrayBuffer
- readAsText(blob, [encoding]) – считать данные как строку (кодировка по умолчанию: utf-8)
- readAsDataURL(blob) – считать данные как base64-кодированный URL.
- abort() – отменить операцию.

Выбор метода для чтения зависит от того, какой формат мы предпочитаем, как мы хотим далее использовать данные.

- readAsArrayBuffer – для бинарных файлов, для низкоуровневой побайтовой работы с бинарными данными. Для высокоуровневых операций у File есть свои методы, унаследованные от Blob, например, slice, мы можем вызвать их напрямую.
- readAsText – для текстовых файлов, когда мы хотим получить строку.
- readAsDataURL – когда мы хотим использовать данные в src для img или другого тега. Есть альтернатива – можно не читать файл, а вызвать URL.createObjectURL(file).

В процессе чтения происходят следующие события:

- loadstart – чтение начато.
- progress – срабатывает во время чтения данных.
- load – нет ошибок, чтение окончено.
- abort – вызван abort().
- error – произошла ошибка.
- loadend – чтение завершено (успешно или нет).

Когда чтение закончено, мы сможем получить доступ к его результату следующим образом:

- reader.result результат чтения (если оно успешно)
- reader.error объект ошибки (при неудаче).

```
<input type="file" onchange="readFile(this)">

<script>
function readFile(input) {
  let file = input.files[0];

  let reader = new FileReader();

  reader.readAsText(file);

  reader.onload = function() {
    console.log(reader.result);
  };

  reader.onerror = function() {
    console.log(reader.error);
  };

}
</script>

```
