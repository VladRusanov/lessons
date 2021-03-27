# REACT

REACT- это JavaScript-библиотека для разработки пользовательского интерфейса


# REACT - Декларативный

Создавать интерактивные пользовательские интерфейсы на React — приятно и просто. 

Вам достаточно описать, как части интерфейса приложения выглядят в разных состояниях. 

React будет своевременно их обновлять, когда данные изменяются.

Декларативные представления сделают код более предсказуемым и упростят отладку


# REACT - Основан на компонентах

Создавайте инкапсулированные компоненты с собственным состоянием, а затем объединяйте их в сложные пользовательские интерфейсы.

Поскольку логика компонента написана на JavaScript, а не содержится в шаблонах, можно с лёгкостью передавать самые разные данные по всему приложению и держать состояние вне DOM.


!!! В REACT ВСЕ - КОМПОНЕНТЫ !!!

# Начало работы

Create React App

Create React App — удобная среда для изучения React и лучший способ начать создание нового одностраничного приложения на React.

Инструмент настраивает среду для использования новейших возможностей JavaScript, оптимизирует приложение для продакшена и обеспечивает комфорт во время разработки. 

Вам понадобятся Node.js не ниже версии 8.10 и npm не ниже версии 5.6 на вашем компьютере.

Для создания проекта выполните команды:


my-app - Название нашего проекта

```
npx create-react-app my-app
cd my-app
npm start

```

Create React App не обрабатывает бэкенд логику или базы данных, он только предоставляет команды для сборки фронтенда, 

поэтому вы можете использовать его с любым бэкэндом. «Под капотом» используются Babel и webpack, но вам не нужно ничего знать о них.

# Примечание

npx в первой строке не является опечаткой. Это инструмент запуска пакетов, доступный в версиях npm 5.2 и выше.

# Структура проекта

- папка src 

В этой папке будут все наши компоненты

- папка public

В этой папке хранится наш файлик html. Он у нас будет 1 на все приложение

# Компоненты

Компоненты позволяют разбить интерфейс на независимые части, про которые легко думать в отдельности. Их можно складывать вместе и использовать несколько раз

# Функциональные и классовые компоненты

- Функциональные

Проще всего объявить React-компонент как функцию:

```
import React from 'react';

function Welcome(props) {
  return <h1>Привет, {props.name}</h1>;
}

```

Эта функция — компонент, потому что она получает данные в одном объекте («пропсы») в качестве параметра и возвращает React-элемент. 

Мы будем называть такие компоненты «функциональными», так как они буквально являются функциями.

- компоненты как классы ES6:

```
import React from 'react';

class Welcome extends React.Component {
  render() {
    return <h1>Привет, {this.props.name}</h1>;
  }
}

```

# Как отрендерить компонент

Пока что мы только встречали React-элементы, представляющие собой DOM-теги:

```
const element = <div />;

```

Но элементы могут описывать и наши собственные компоненты:

```
const element = <Welcome name="Алиса" />;

```

Когда React встречает подобный элемент, он собирает все JSX-атрибуты и дочерние элементы в один объект и передаёт их нашему компоненту. 

Этот объект называется «пропсы» (props).

Например, этот компонент выведет «Привет, Алиса» на страницу:

```
function Welcome(props) {
  return <h1>Привет, {props.name}</h1>;
}

const element = <Welcome name="Алиса" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);

```

# Давайте разберём, что именно здесь происходит:

- Мы передаём React-элемент <Welcome name="Алиса" /> в ReactDOM.render().

- React вызывает наш компонент Welcome с пропсами {name: 'Алиса'}.

- Наш компонент Welcome возвращает элемент <h1>Привет, Алиса</h1> в качестве результата.

- React DOM делает минимальные изменения в DOM, чтобы получилось <h1>Привет, Алиса</h1>.


** !!!!!!!!!!!!! Примечание: Всегда называйте компоненты с заглавной буквы. !!!!!!!!!!!!! **

# Знакомство с JSX

(Сейчас мы пишем все в файлик App.js)

Рассмотрим объявление переменной:

```
const element = <h1>Привет, мир!</h1>;

```

# Что такое JSX?

JSX — синтаксический сахар для функции React.createElement(component, props, ...children). Этот JSX-код:

```
<MyButton color="blue" shadowSize={2}>
  Нажми меня
</MyButton>

```

Скомпилируется в 

```
React.createElement(
  MyButton,
  {color: 'blue', shadowSize: 2},
  'Нажми меня'
)

```

Вы также можете использовать самозакрывающийся тег, если отсутствуют дочерние элементы. Поэтому код:

```
<div className="sidebar" />

```

# Структура классового компонента

Все компоненты надо выносить в новые файлы.

Имя файла должно быть с большой буквы

# Расширение

Если файл возвращает jsx, то расширение - jsx


- Файл Test.jsx

```
import React from 'react';

class Test extends React.Component {
  
}

```

# Метод render 

render() — единственный обязательный метод в классовом компоненте.

Функция render() должна быть чистой. Это означает, что она не изменяет состояние компонента, всегда возвращает один и тот же результат, не взаимодействует напрямую с браузером.

В render пишем "верстку"

Давайте сделаем компонент, который отобразит текст hello world 

```
import React from 'react';

class Test extends React.Component {
  render() {
    return (
      <p>Hello</p>
    )
  }
}

```

Задача:

Перепишите компонент App на классы

Сделайте маркерованный список 

1
2
3

```
App.js

import React from 'react';

class App extends React.Component {
  render() {
    return (
      <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
      </ul>
    )
  }
}

export default App;

```

# Как работать с разными файлами

Давайте сделаем маркерованный список


Давайте создадим два новых компонента

- List.jsx

- LiItem.jsx


# LiItem.jsx

```
import React from 'react';

class LiItem extends React.Component {
    render() {
        return (
            <li>
                test
            </li>
        );
    }
}

export default LiItem;

```

# List.jsx

```
import React from 'react';
import LiItem from './LiElement.jsx';

class List extends React.Component {
    render() {
        return (
            <ul>
                <LiItem />
                <LiItem />
                <LiItem />
            </ul>
        )
    }
}

export default List;

```

# App.jsx

```
import React from 'react';
import List from './List.jsx';

class App extends React.Component {
  render() {
    return (
      <List />
    )
  }
}

export default App;

```

# Как использовать стили

Давайте сделаем отступ для нашего списка

Создадим файли style.css

```
./style.css

.blockElem {
    margin-left: 100px;
}

```

```
./App.js

class App extends React.Component {
  render() {
    return (
      <div className='blockElem'>
        <List />
      </div>
    )
  }
}

export default App;

```


# props

props - данные, которые мы можем перекидывать между компонентами

props - это объект


# Как получить доступ к props

К примеру

У нас есть компонент User

```
./User.jsx

import React from 'react';

class User extends React.Component {
    render() {
        console.log(this.props); // { name: 'Vlad' }
        return (
            <div></div>
        )
    }
}

export default User;

```

```
./App.jsx

import React from 'react';

import User from './User.jsx';
import './App.css';

class App extends React.Component {
  render() {
    const data = 'Vlad';
    return (
      <div className='blockElem'>
        <User name={data} />
      </div>
    )
  }
}

export default App;

```

name - props для компонента User

Получить доступ к переданным пропсам - this.props;



# HomeWork

1. Создать шарик, который "летит" к центру экрана (мы уже делали это на js)

Делаем без событий (шарик сразу летит)

Можно сделать старт анимации при наведении на шарик

2. Создать форму для регистрации. Должны быть поля - Email, Password, Name

В форме должен быть Title (Оглавление).

В input должен быть placeholder

Дизайн делайте любой
