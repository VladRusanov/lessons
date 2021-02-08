# Навигация по DOM-элементам

# Дети: childNodes, firstChild, lastChild

- Дочерние узлы (или дети) – элементы, которые являются непосредственными детьми узла. 

Другими словами, элементы, которые лежат непосредственно внутри данного. 

Например, <head> и <body> являются детьми элемента <html>.
    
- Потомки – все элементы, которые лежат внутри данного, включая детей, их детей и т.д.

В примере ниже детьми тега <body> являются теги <div> и <ul> (и несколько пустых текстовых узлов):
    
```
<html>
<body>
  <div>Начало</div>

  <ul>
    <li>
      <b>Информация</b>
    </li>
  </ul>
</body>
</html>

```
А потомки <body> – это и прямые дети <div>, <ul> и вложенные в них: <li> (потомок <ul>) и <b> (потомок <li>) – в общем, все элементы поддерева.

# childNodes

Коллекция childNodes содержит список всех детей, включая текстовые узлы.

Пример ниже последовательно выведет детей document.body:

```
<html>
<body>
  <div>Начало</div>

  <ul>
    <li>Информация</li>
  </ul>

  <div>Конец</div>

  <script>
    for (let i = 0; i < document.body.childNodes.length; i++) {
      alert( document.body.childNodes[i] ); // Text, DIV, Text, UL, ..., SCRIPT
    }
  </script>
  ...какой-то HTML-код...
</body>
</html>

```

Свойства **firstChild** и **lastChild** обеспечивают быстрый доступ к первому и последнему дочернему элементу.

Они, по сути, являются всего лишь сокращениями. Если у тега есть дочерние узлы, условие ниже всегда верно:

```
elem.childNodes[0] === elem.firstChild
elem.childNodes[elem.childNodes.length - 1] === elem.lastChild

```

# DOM-коллекции – только для чтения

DOM-коллекции, и даже более – все навигационные свойства, перечисленные в этой главе, доступны только для чтения.

Мы не можем заменить один дочерний узел на другой, просто написав childNodes[i] = ....

# DOM-коллекции живые

Почти все DOM-коллекции, за небольшим исключением, живые. Другими словами, они отражают текущее состояние DOM.

Если мы сохраним ссылку на elem.childNodes и добавим/удалим узлы в DOM, то они появятся в сохранённой коллекции автоматически.

# Соседи и родитель

Соседи – это узлы, у которых один и тот же родитель.

Следующий узел того же родителя (следующий сосед) – **в свойстве nextSibling**, **а предыдущий – в previousSibling**.

Родитель доступен через parentNode.

```
// родителем <body> является <html>
alert( document.body.parentNode === document.documentElement ); // выведет true

// после <head> идёт <body>
alert( document.head.nextSibling ); // HTMLBodyElement

// перед <body> находится <head>
alert( document.body.previousSibling ); // HTMLHeadElement

```

# Навигация только по элементам

- children – коллекция детей, которые являются элементами.

- firstElementChild, lastElementChild – первый и последний дочерний элемент.

- previousElementSibling, nextElementSibling – соседи-элементы.

- parentElement – родитель-элемент.


# Promise. Понимание промисов

Промис - это объект, у которого есть 3 состояния:

- Panding - ожидание

- Resolve - выполнено успешно

- Reject - выполнено с ошибкой

```
let promise = new Promise(function(resolve, reject) {
  // функция-исполнитель (executor)
  // "певец"
});

```

Функция, переданная в конструкцию new Promise, **называется исполнитель (executor)**. 

Когда Promise создаётся, она запускается автоматически. 

Она должна содержать «создающий» код, который когда-нибудь создаст результат. 

Её аргументы resolve и reject – это колбэки, которые предоставляет сам JavaScrip

Когда он получает результат, сейчас или позже – не важно, он должен вызвать один из этих колбэков:

- resolve(value) — если работа завершилась успешно, с результатом value.

- reject(error) — если произошла ошибка, error – объект ошибки.

Итак, исполнитель запускается автоматически, он должен выполнить работу, а затем вызвать resolve или reject.

Ниже пример конструктора Promise и простого исполнителя с кодом, дающим результат с задержкой (через setTimeout):

```
let promise = new Promise(function(resolve, reject) {
  // эта функция выполнится автоматически, при вызове new Promise

  // через 1 секунду сигнализировать, что задача выполнена с результатом "done"
  setTimeout(() => resolve("done"), 1000);
});

```

Мы можем наблюдать две вещи, запустив код выше:

- Функция-исполнитель запускается сразу же при вызове new Promise.

- Исполнитель получает два аргумента: resolve и reject — это функции, встроенные в JavaScript, поэтому нам не нужно их писать. Нам нужно лишь позаботиться, чтобы исполнитель вызвал одну из них по готовности.

Спустя одну секунду «обработки» исполнитель вызовет resolve("done"), чтобы передать результат:


![help](https://learn.javascript.ru/article/promise-basics/promise-resolve-1.svg)

А теперь пример, в котором исполнитель сообщит, что задача выполнена с ошибкой:

```
let promise = new Promise(function(resolve, reject) {
  // спустя одну секунду будет сообщено, что задача выполнена с ошибкой
  setTimeout(() => reject(new Error("Whoops!")), 1000);
});

```

![help](https://learn.javascript.ru/article/promise-basics/promise-reject-1.svg)




# Может быть что-то одно: либо результат, либо ошибка

```
let promise = new Promise(function(resolve, reject) {
  resolve("done");

  reject(new Error("…")); // игнорируется
  setTimeout(() => resolve("…")); // игнорируется
});

```

# Потребители: then, catch, finally

Объект Promise служит связующим звеном между исполнителем и функциями-потребителями 

- then

Наиболее важный и фундаментальный метод – .then.

Синтаксис:

```
promise.then(
  function(result) { /* обработает успешное выполнение */ },
  function(error) { /* обработает ошибку */ }
);

```

Первый аргумент метода .then – функция, которая выполняется, когда промис переходит в состояние «выполнен успешно», и получает результат.

Второй аргумент .then – функция, которая выполняется, когда промис переходит в состояние «выполнен с ошибкой», и получает ошибку.

Например, вот реакция на успешно выполненный промис:

```
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => resolve("done!"), 1000);
});

// resolve запустит первую функцию, переданную в .then
promise.then(
  result => alert(result), // выведет "done!" через одну секунду
  error => alert(error) // не будет запущена
);

```

Выполнилась первая функция.

А в случае ошибки в промисе – выполнится вторая:

```
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => reject(new Error("Whoops!")), 1000);
});

// reject запустит вторую функцию, переданную в .then
promise.then(
  result => alert(result), // не будет запущена
  error => alert(error) // выведет "Error: Whoops!" спустя одну секунду
);

```

- catch

Если мы хотели бы только обработать ошибку, то можно использовать null в качестве первого аргумента: .then(null, errorHandlingFunction). 

Или можно воспользоваться методом .catch(errorHandlingFunction), который сделает тоже самое:

```
let promise = new Promise((resolve, reject) => {
  setTimeout(() => reject(new Error("Ошибка!")), 1000);
});

// .catch(f) это тоже самое, что promise.then(null, f)
promise.catch(alert); // выведет "Error: Ошибка!" спустя одну секунду

```

Вызов .catch(f) – это сокращённый, «укороченный» вариант .then(null, f).

Вызов .finally(f) похож на .then(f, f), в том смысле, что f выполнится в любом случае, когда промис завершится: успешно или с ошибкой.

finally хорошо подходит для очистки, например остановки индикатора загрузки, его ведь нужно остановить вне зависимости от результата.

Например:

```
new Promise((resolve, reject) => {
  /* сделать что-то, что займёт время, и после вызвать resolve/reject */
})
  // выполнится, когда промис завершится, независимо от того, успешно или нет
  .finally(() => остановить индикатор загрузки)
  .then(result => показать результат, err => показать ошибку)

```

# Разница между finall и than

- Обработчик, вызываемый из finally, не имеет аргументов. В finally мы не знаем, как был завершён промис. И это нормально, потому что обычно наша задача – выполнить «общие» завершающие процедуры.

- Обработчик finally «пропускает» результат или ошибку дальше, к последующим обработчикам.

Например, здесь результат проходит через finally к then:

```
new Promise((resolve, reject) => {
  setTimeout(() => resolve("result"), 2000)
})
  .finally(() => alert("Промис завершён"))
  .then(result => alert(result)); // <-- .then обработает результат

```

А здесь ошибка из промиса проходит через finally к catch:

```
new Promise((resolve, reject) => {
  throw new Error("error");
})
  .finally(() => alert("Промис завершён"))
  .catch(err => alert(err));  // <-- .catch обработает объект ошибки

```

Давайте напишем функция, которая сработает через 2 секунды после нажатия на кнопку. 

Эта функция должна получать данные с нашего воображаемого севрера. 

На нашем сервере должен быть массив с информацией о нашией группе (firstName, surName, mood, eyeColour)

```
const server = [{...}, {...}, {...}, {....}];

```

Внутри функция должен быть промис, который будет это обрабатывать

```

promise
    .then((data) => console.log(data))
    .catch((error) => consoe.log(error))

```

Реализация это задачи ниже:


```
const btn = document.getElementById('btn');

const server = [{
    name: 'Vlad',
    surName: 'Rusanov',
    eyeColour: 'eyeColour'
}, {
    name: 'Vova',
    surName: 'Test',
    eyeColour: 'eyeColour'
}];

const getDataFromServer = () => {
    return new Promise((resolve, reject) => { // (0)
        setTimeout(() => {
            resolve(server); // успех
            // reject(new Error('500 ServerError')); // не успех
        }, 2000);
    })
}

function asyncAction() {
    const responce = getDataFromServer();
    responce
        .then((data) => {  // (1)
            console.log('data: ', data); // [{ name: 'Vlad', ... }, { name: 'Vova', ... }]
        })
        .catch((err) => { // (2)
            console.log('err: ', err);
        })
}

btn.addEventListener('click', asyncAction)

```

0) У нас есть переменная (server), в которой храниться наш "сервер"

1) У нас есть функция asyncAction, которая является обработчиком для нажатия на кнопку.

Внутри это функции мы получаем данные с "сервера":

```
const responce = getDataFromServer();

```

2) Функция getDataFromServer(). (0)

Эта функция возвращает нам новый промис с каким-то результатом (в нашем случае это успех, понимаем это из-за resolve()).

Внутри промиса, который в getDataFromServer, мы возвращаем успех ( resolve(server) ). 

Все, что мы передали в resolve, получим в первом аргументе коллбэк функции в .then (1)

Если бы мы написали не resolve(server), а reject(new Error('500 ServerError')), то мы бы попали в обработчик .catch(). (2) 

И наше сообщение (500 ServerError) мы можем получить в первом аргументе коллбэк функции .catch



# XMLHttpRequest (реальные запросы на сервер)

XMLHttpRequest – это встроенный в браузер объект, который даёт возможность делать HTTP-запросы к серверу без перезагрузки страницы.

**На сегодняшний день не обязательно использовать XMLHttpRequest, так как существует другой, более современный метод fetch**


# Основы

XMLHttpRequest имеет два режима работы: синхронный и асинхронный.

- асинхронный

Чтобы сделать запрос, нам нужно выполнить три шага:

1) Создать XMLHttpRequest

```
let xhr = new XMLHttpRequest(); // у конструктора нет аргументов

```

2) Инициализировать его

```
xhr.open(method, URL, [async, user, password])

```

- method – HTTP-метод. Обычно это "GET" или "POST".

- URL – URL, куда отправляется запрос: строка, может быть и объект URL.

- async – если указать false, тогда запрос будет выполнен синхронно, это мы рассмотрим чуть позже.

- user, password – логин и пароль для базовой HTTP-авторизации (если требуется).


3) Послать запрос.

```
xhr.send([body])

```

Некоторые типы запросов, такие как GET, не имеют тела.

А некоторые, как, например, POST, используют body, чтобы отправлять данные на сервер


4) Слушать события на xhr, чтобы получить ответ.

Три наиболее используемых события:

- load – происходит, когда получен какой-либо ответ, включая ответы с HTTP-ошибкой, например 404.

- error – когда запрос не может быть выполнен, например, нет соединения или невалидный URL.

- progress – происходит периодически во время загрузки ответа, сообщает о прогрессе.


```
xhr.onload = function() {
  alert(`Загружено: ${xhr.status} ${xhr.response}`);
};

xhr.onerror = function() { // происходит, только когда запрос совсем не получилось выполнить
  alert(`Ошибка соединения`);
};

xhr.onprogress = function(event) { // запускается периодически
  // event.loaded - количество загруженных байт
  // event.lengthComputable = равно true, если сервер присылает заголовок Content-Length
  // event.total - количество байт всего (только если lengthComputable равно true)
  alert(`Загружено ${event.loaded} из ${event.total}`);
};

```

Вот полный пример. Код ниже загружает /article/xmlhttprequest/example/load с сервера и сообщает о прогрессе:

```
// 1. Создаём новый XMLHttpRequest-объект
let xhr = new XMLHttpRequest();

// 2. Настраиваем его: GET-запрос по URL /article/.../load
xhr.open('GET', '/article/xmlhttprequest/example/load');

// 3. Отсылаем запрос
xhr.send();

// 4. Этот код сработает после того, как мы получим ответ сервера
xhr.onload = function() {
  if (xhr.status != 200) { // анализируем HTTP-статус ответа, если статус не 200, то произошла ошибка
    alert(`Ошибка ${xhr.status}: ${xhr.statusText}`); // Например, 404: Not Found
  } else { // если всё прошло гладко, выводим результат
    alert(`Готово, получили ${xhr.response.length} байт`); // response -- это ответ сервера
  }
};

xhr.onprogress = function(event) {
  if (event.lengthComputable) {
    alert(`Получено ${event.loaded} из ${event.total} байт`);
  } else {
    alert(`Получено ${event.loaded} байт`); // если в ответе нет заголовка Content-Length
  }

};

xhr.onerror = function() {
  alert("Запрос не удался");
};

```

Давайте сделаем запрос на реальный сервер с фильмами:

http://api.tvmaze.com/search/shows?q=girls - эндпоинт (ссылка) на сам сервер

```
// 1. Создаём новый XMLHttpRequest-объект
let xhr = new XMLHttpRequest();

// 2. Настраиваем его: GET-запрос по URL /article/.../load
xhr.open('GET', 'http://api.tvmaze.com/search/shows?q=girls');

// 3. Отсылаем запрос
xhr.send();

// 4. Этот код сработает после того, как мы получим ответ сервера
xhr.onload = function () {
    if (xhr.status != 200) {
        console.log(`Ошибка ${xhr.status}: ${xhr.statusText}`);
    } else {
        console.log(`Готово, получили ${xhr.response} байт`);
    }
};

xhr.onprogress = function (event) {
    if (event.lengthComputable) {
        console.log(`Получено ${event.loaded} из ${event.total} байт`);
    } else {
        console.log(`Получено ${event.loaded} байт`);
    }

};

xhr.onerror = function () {
    console.log("Запрос не удался");
};

```

И глянем в консоль


# Тип ответа

Мы можем использовать свойство xhr.responseType, чтобы указать ожидаемый тип ответа:

- "" (по умолчанию) – строка,

- "text" – строка,

- "arraybuffer" – ArrayBuffer (для бинарных данных, смотрите в ArrayBuffer, бинарные массивы),

- "blob" – Blob (для бинарных данных, смотрите в Blob),

- "document" – XML-документ (может использовать XPath и другие XML-методы),

- "json" – JSON (парсится автоматически).

К примеру, давайте получим ответ в формате JSON:

```
let xhr = new XMLHttpRequest();

xhr.open('GET', '/article/xmlhttprequest/example/json');

xhr.responseType = 'json';

xhr.send();

// тело ответа {"сообщение": "Привет, мир!"}
xhr.onload = function() {
  let responseObj = xhr.response;
  alert(responseObj.message); // Привет, мир!
};

```

# Состояния запроса

У XMLHttpRequest есть состояния, которые меняются по мере выполнения запроса. 

Текущее состояние можно посмотреть в свойстве xhr.readyState.

```
UNSENT = 0; // исходное состояние
OPENED = 1; // вызван метод open
HEADERS_RECEIVED = 2; // получены заголовки ответа
LOADING = 3; // ответ в процессе передачи (данные частично получены)
DONE = 4; // запрос завершён

```


```
xhr.onreadystatechange = function() {
  if (xhr.readyState == 3) {
    // загрузка
  }
  if (xhr.readyState == 4) {
    // запрос завершён
  }
};

```

Hemoweork8: 

0.! Учить теорию ! 

1.Реализовать Числа Фибоначчи двумя способами (рекурсия и цикл)

2.Рассчитать сумму натуральных чисел до n (2 решения - рекурсия и цикл)

3.Напишите функцию printNumbers(from, to), которая выводит число каждую секунду, начиная от from и заканчивая to.

Сделайте два варианта решения.

- Используя setInterval.

- Используя рекурсивный setTimeout.

4.Нужно создать интервал, который выводит в консоль количество секунд, прошедших с момента запуска программы.

"Прошло: 1 сек."

"Прошло: 2 сек." ..... и так далее

Допишите программу так, чтобы она останавливалась при достижении 5 секунд и надпись о пройденном времени больше не выводилась в консоль.

5.В html есть такое

```
<ul id='ulList'>
    <li class="li">How are you?</li>
    <li class="li">1</li>
    <li class="li">2</li>
    <li class="li">3</li>
    <li class="li">3</li>
    <li class="li">Hello</li>
</ul>
<button id='sum'>Get Sum</button>
<input type="text" id="inp">

```

Написать логику, которая будет находить сумму всех **ЧИСЕЛ**, которые вписаны в li, и выводить эту сумму (в формате 1 + 2 + 3 = 6) в инпут (#inp) по нажатию на кнопку (#sum)

Вот так должно быть:

https://prnt.sc/y5v875

6.Написать игру "крестики-нолики" )

Игра должна быть на двоих. 

Должно быть вот так:

https://prnt.sc/y6p58p

Эта домашка будет на 2 занятия. За это будет отдельная оценка.



