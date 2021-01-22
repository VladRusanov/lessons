# parseInt parseFloat

Функция parseFloat() принимает строку в качестве аргумента и возвращает десятичное число (число с плавающей точкой)

```
parseFloat(3.14);
parseFloat('3.14');
parseFloat('314e-2');
parseFloat('0.0314E+2');
parseFloat('3.14какие-нибудь не цифровые знаки');

// 3.14

```

Функция parseInt() принимает строку в качестве аргумента и возвращает целое число в соответствии с указанным основанием системы счисления.

```
parseInt(" 0xF", 16);
parseInt(" F", 16);
parseInt("17", 8);
parseInt(021, 8);
parseInt("015", 10);  //parseInt(015, 10); вернёт 15
parseInt(15.99, 10);
parseInt("FXX123", 16);
parseInt("1111", 2);
parseInt("15*3", 10);
parseInt("15e2", 10);
parseInt("15px", 10);
parseInt("12", 13);

// 15

```

```
parseInt("Hello", 8); // Не является числом
parseInt("546", 2);   // Неверное число в двоичной системе счисления

// все NaN

```


# Браузерное окружение, спецификации

Окружение предоставляет свои объекты и дополнительные функции, в дополнение базовым языковым. Браузеры, например, дают средства для управления веб-страницами. Node.js делает доступными какие-то серверные возможности и так далее.

На картинке ниже в общих чертах показано, что доступно для JavaScript в браузерном окружении:

![help](https://learn.javascript.ru/article/browser-environment/windowObjects.svg)

Как мы видим, имеется корневой объект window, который выступает в 2 ролях:

- Во-первых, это глобальный объект для JavaScript-кода, об этом более подробно говорится в главе Глобальный объект.
- Во-вторых, он также представляет собой окно браузера и располагает методами для управления им.

# window 

```
function sayHi() {
  alert("Hello");
}

// глобальные функции доступны как методы глобального объекта:
window.sayHi();

```

А здесь мы используем window как объект окна браузера, чтобы узнать его высоту:

```
alert(window.innerHeight); // внутренняя высота окна браузера

```

# BOM (Browser Object Model)

Объектная модель браузера (Browser Object Model, BOM) – это дополнительные объекты, предоставляемые браузером (окружением), чтобы работать со всем, кроме документа.

- Объект navigator даёт информацию о самом браузере и операционной системе. 

Среди множества его свойств самыми известными являются: navigator.userAgent – информация о текущем браузере, и navigator.platform – информация о платформе (может помочь в понимании того, в какой ОС открыт браузер – Windows/Linux/Mac и так далее).

- Объект location позволяет получить текущий URL и перенаправить браузер по новому адресу.


```
alert(location.href); // показывает текущий URL
if (confirm("Перейти на Wikipedia?")) {
  location.href = "https://wikipedia.org"; // перенаправляет браузер на другой URL
}

```

Функции **alert/confirm/prompt** тоже являются частью BOM: они не относятся непосредственно к странице, но представляют собой методы объекта окна браузера для коммуникации с пользователем.

# DOM (Document Object Model)

Document Object Model, сокращённо DOM – объектная модель документа, которая представляет все содержимое страницы в виде объектов, которые можно менять.

Объект document – основная «входная точка». С его помощью мы можем что-то создавать или менять на странице.

```
// заменим цвет фона на красный,
document.body.style.background = "red";

```

# Поиск в DOM: getElement*, querySelector*, getElementsBy*

- document.getElementById или просто id

```
<span id="firstID">Hello World</span>

<script>

// Получить элемент в переменную
const mySpan = document.getElementById('firstID');

// сделать его фон красным
mySpan.style.background = 'red';

</script>

```
Только document.getElementById, а не anyElem.getElementById

Метод getElementById можно вызвать только для объекта document. Он осуществляет поиск по id по всему документу.

- getElementsBy

Возвращает элементы, которые имеют данный CSS-класс. **Возвращаем коллекцию**

```
<span class="className"></span>
<span class="className"></span>
<span class="className"></span>
<span class="className"></span>


// получить все элементы c классом "className" в документе
let spans = document.getElementsByclassName('className');

```
- getElementsByTagName(tag)

Ищет элементы с данным тегом и возвращает их коллекцию.

```
// получить все элементы div в документе
let divs = document.getElementsByTagName('div');

```


- querySelectorAll

Метод для поиска элементов в DOM по селектору


```
<ul>
  <li>Этот</li>
  <li>тест</li>
</ul>
<ul>
  <li>полностью</li>
  <li>пройден</li>
</ul>
<script>
  let elements = document.querySelectorAll('ul > li:last-child');

  for (let elem of elements) {
    alert(elem.innerHTML); // "тест", "пройден"
  }
</script>

```







