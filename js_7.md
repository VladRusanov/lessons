# setAttribute

Добавляет новый атрибут или изменяет значение существующего атрибута у выбранного элемента.

```
element.setAttribute(name, value);

```

- name - имя атрибута (строка).
- value  - значение атрибута.

В следующем примере, setAttribute() используется, чтобы установить атрибут disabled  кнопки <button>, делая её отключенной.
  
```
<button>Hello World</button>

<script>

const b = document.querySelector("button");

b.setAttribute("disabled", "disabled");

</script>

```


События:

Каждый элемент DOM содержит множество свойств on..., в которые вы можете занести тот или иной обработчик события:

Вот список самых часто используемых DOM-событий, пока просто для ознакомления:

- События мыши:

**click** – происходит, когда кликнули на элемент левой кнопкой мыши (на устройствах с сенсорными экранами оно происходит при касании).

**contextmenu** – происходит, когда кликнули на элемент правой кнопкой мыши.

**mouseover** / mouseout – когда мышь наводится на / покидает элемент.

**mousedown** / mouseup – когда нажали / отжали кнопку мыши на элементе.

**mousemove** – при движении мыши.

- События на элементах управления:

**submit** – пользователь отправил форму <form>.
  
**focus** – пользователь фокусируется на элементе, например нажимает на <input>.

- Клавиатурные события:

**keydown и keyup** – когда пользователь нажимает / отпускает клавишу.

- События документа:

**DOMContentLoaded** – когда HTML загружен и обработан, DOM документа полностью построен и доступен.

- CSS events:

**transitionend** – когда CSS-анимация завершена.


# Установка обработчиков

Для обработки событий вы можете назначить функцию-обработчик события путем присвоения ключа объекта

Так как у элемента DOM может быть только одно свойство с именем onclick, то назначить более одного обработчика так нельзя.

```
someElement.onmousemove = function(event){
    console.log(this, event);
}

someElement.onmousemove = function(event){ // перетрет прошлую функцию
    console.log('Hello');
}

```

Частые ошибки:

Функция должна быть присвоена как sayThanks, а не sayThanks().

```
// правильно
button.onclick = sayThanks;

// неправильно
button.onclick = sayThanks();

```

А вот в разметке, в отличие от свойства, скобки нужны:

```
<input type="button" id="button" onclick="sayThanks()">

```

# addEventListener

Фундаментальный недостаток описанных выше способов назначения обработчика –- невозможность повесить несколько обработчиков на одно событие.

Так же вы можете добавить несколько обработчиков на одно и тоже событие в одном и том же элементе, используя метод addEventListener:

Синтаксис:

```
element.addEventListener(event, handler[, options]);

```

- event

Имя события, например "click".

- handler

Ссылка на функцию-обработчик.

- options

Дополнительный объект со свойствами:

- once: если true, тогда обработчик будет автоматически удалён после выполнения.

- capture: фаза, на которой должен сработать обработчик, подробнее об этом будет рассказано в главе Всплытие и погружение. 

Так исторически сложилось, что options может быть false/true, это то же самое, что {capture: false/true}.

- passive: если true, то указывает, что обработчик никогда не вызовет preventDefault(), подробнее об этом будет рассказано в главе Действия браузера по умолчанию.

```
someElement.addEventListener("mousemove",function(event){
    console.log(this, event);
});

```

# removeEventListener

Для удаления обработчика следует использовать removeEventListener:

```
element.removeEventListener(event, handler[, options]);

```

Пример: 

```
function handler() {
  alert( 'Спасибо!' );
}

input.addEventListener("click", handler);
// ....
input.removeEventListener("click", handler);

```

# Обработчики некоторых событий можно назначать только через addEventListener

```
document.onDOMContentLoaded = function() {
  alert("DOM построен"); // не будет работать
};

document.addEventListener("DOMContentLoaded", function() {
  alert("DOM построен"); // а вот так сработает
});

```

Использование атрибута HTML

Обработчик может быть назначен прямо в разметке, в атрибуте, который называется on<событие>.

Например, чтобы назначить обработчик события click на элементе input, можно использовать атрибут onclick, вот так:


```
<input value="Нажми меня" onclick="alert('Клик!')" type="button">

```


# Данные, передаваемые в обработчик

В обработчик события передается два аргумента:

- this, который ссылается на элемент, на который "навешен" обработчик (это естественно, если вы занесли функцию в объект). 

Используя this вы можете узнать любую информацию о элементе и навешивать один и тот же обработчик на несколько элементов, если вам нужно схожее поведение на многих элементах


- event, объект-событие, переданный первым аргументом в функцию-обработчик (вы можете назвать его как угодно). 

С помощью этого объекта можно узнать информацию о событии (нажатые кнопки мыши и клавиатуры, положение курсора мыши, и т. п.), а так же управлять обработкой потока событий


# Объект события

При выполнении какого-то события нам часто надо получать какие-то данные, которые относятся к этому события. 

Например координаты указателя мыши, какая клавиша нажата и так далее.

Для этого у нас есть объект event

```
<input type="button" value="Нажми меня" id="elem">

<script>
  elem.onclick = function(event) {
    // вывести тип события, элемент и координаты клика
    alert(event.type + " на " + event.target);
    alert("Координаты: " + event.clientX + ":" + event.clientY);
  };
</script>

```

# Некоторые свойства объекта event:

- event.type

Тип события, в данном случае "click"

- event.target

Элемент, на котором сработал обработчик. 

Значение – обычно такое же, как и у this, но если обработчик является функцией-стрелкой или при помощи bind привязан другой объект в качестве this, то мы можем получить элемент из event.currentTarget.


# Разница между this и event.target

- this всегда будет указывать на тот элемент, на ктором висит событие, которое мы вызвали

- event.target всегда будет указывать на элемент, который это событие вызвал



# Задание на сейчас

Добавьте JavaScript к кнопке button, чтобы при нажатии элемент <div id="text"> исчезал


# Формы, элементы управления

События: change, input, cut, copy, paste

- Событие: change

Событие change срабатывает по окончании изменения элемента.

Для текстовых <input> это означает, что событие происходит при потере фокуса.

Пока мы печатаем в текстовом поле в примере ниже, событие не происходит. 

Но когда мы перемещаем фокус в другое место, например, нажимая на кнопку, то произойдёт событие change:

```
<input type="text" onchange="alert(this.value)">
<input type="button" value="Button">

```

Для других элементов: select, input type=checkbox/radio событие запускается сразу после изменения значения:

```
<select onchange="alert(this.value)">
  <option value="">Выберите что-нибудь</option>
  <option value="1">Вариант 1</option>
  <option value="2">Вариант 2</option>
  <option value="3">Вариант 3</option>
</select>

```


# Событие: input

Событие input срабатывает каждый раз при изменении значения.

В отличие от событий клавиатуры, оно работает при любых изменениях значений, даже если они не связаны с клавиатурными действиями: вставка с помощью мыши или распознавание речи при диктовке текста.

Например:

```
<input type="text" id="inputId"> oninput: <span id="result"></span>
<script>
    const inputId = document.getElementById('inputId');
    inputId.oninput = function () {
        result.innerHTML = inputId.value;
    };
</script>

```

# События: cut, copy, paste

Эти события происходят при вырезании/копировании/вставке данных.

Они относятся к классу ClipboardEvent и обеспечивают доступ к копируемым/вставляемым данным.

Мы также можем использовать event.preventDefault() для предотвращения действия по умолчанию, и в итоге ничего не скопируется/не вставится.

Например, код, приведённый ниже, предотвращает все подобные события и показывает, что мы пытаемся вырезать/копировать/вставить:

```
<input type="text" id="inputId">
<script>
    const inputId = document.getElementById('inputId');
    inputId.oncut = function (event) {
        console.log('oncut');
    }
    inputId.oncopy = function (event) {
        console.log('oncopy');
    }
    inputId.onpaste = function () {
        console.log('onpaste');
    }
</script>

```

# Отправка формы: событие и метод submit

При отправке формы срабатывает событие submit, оно обычно используется для проверки (валидации) формы перед её отправкой на сервер или для предотвращения отправки и обработки её с помощью JavaScript.

Метод form.submit() позволяет инициировать отправку формы из JavaScript. 

Мы можем использовать его для динамического создания и отправки наших собственных форм на сервер.

Давайте посмотрим на них подробнее.

 - Событие: submit
 
Есть два основных способа отправить форму:

- Первый – нажать кнопку <input type="submit"> или <input type="image">

- Второй – нажать Enter, находясь на каком-нибудь поле



В примере ниже:

1. Перейдите в текстовое поле и нажмите Enter.
2. Нажмите <input type="submit">.

Оба действия показывают alert и форма не отправится благодаря return false:

```
<form onsubmit="alert('submit!');return false">
    Первый пример: нажмите Enter: <input type="text" value="Текст"><br>
    Второй пример: нажмите на кнопку "Отправить": <input type="submit" value="Отправить">
</form>

```



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

про .calle посмотреть тут https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Functions/arguments/callee

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

