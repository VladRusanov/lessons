# Таска на сейчас

Есть апи c фильмами

У вас уже есть форма регистрации. Поле того как вы ввели туда все свои данне и нажали на кнопку "Sign Up", то вас перекидывает на страницу с фильмами.

Фильмы тянем по старому эндпонту

Отобразить на экране картинки фильмов и их title

# Правила хуков

- Хуки следует вызывать только на верхнем уровне. Не вызывайте хуки внутри циклов, условий или вложенных функций.

- Хуки следует вызывать только из функциональных компонентов React. 

Не вызывайте хуки из обычных JavaScript-функций. Есть только одно исключение, откуда можно вызывать хуки — это ваши пользовательские хуки. Мы расскажем о них далее.

# Создание собственных хуков

Давайте напишем собственный хук для игры в "орел-решка"

```
./myHook

import { useEffect, useState } from 'react';


export const useGame = () => {
    const [coin, setCoin] = useState('');
    const [rand, setRand] = useState(0.1);
    const flipCoin = () => {
        const val = Math.random();
        setRand(val);
    }

    useEffect(() => {
        if (rand < .6) {
            setCoin('Орел')
        } else {
            setCoin('Решка')
        }
    }, [rand])
    
    return [coin, flipCoin];
}

```

```
./App.jsx

import React from 'react';
import { useGame } from './myHook.jsx'

function App() {
  const [result, flip] = useGame();

  return (
    <button onClick={() => flip()}>
      {result}
    </button>
  );
}

export default App;


```

# Контекст

Контекст позволяет передавать данные через дерево компонентов без необходимости передавать пропсы на промежуточных уровнях.

В типичном приложении React данные передаются сверху вниз (от родителя к потомку) через свойства props. 

Однако это может оказаться громоздким для определенных типов свойств (тема UI; предпочтения, связанные с локалью), которые требуются для многих компонентов в приложении. 

Контекст предоставляет способ совместного использования таких значений между компонентами без необходимости явно передавать свойство через каждый уровень дерева.


Контекст работает засчет таких вещей:

- React.createContext - создает контекст

- Provider - дает возможность пользоваться контекстом (кидает контекст в компоненті)

- Consumer - полуает этот контекст (ловит)

```
  // Контекст позволяет нам передавать значение глубоко в дерево компонентов
  // без его явной передачи через каждый компонент.
  // Создайте контекст для текущей темы (значение "light" по умолчанию).
  const ThemeContext = React.createContext('light'); // создаем контекст

  class App extends React.Component {
    render() {
      // Используйте Provider, чтобы передать текущую тему вглубь дерева.
      // Любой компонент может считать её, вне зависимости от того как глубоко она находится.
      // В данном примере, мы передаем "dark" как текущее значение.
      return (
        <ThemeContext.Provider value="dark"> // кидаем контекст в компоненты
          <Toolbar />
        </ThemeContext.Provider>
      );
    }
  }
```
```
  // Промежуточному компоненту необязательно
  // явно передавать тему кому-либо далее.
  function Toolbar(props) {
    return (
      <div>
        <ThemedButton />
      </div>
    );
  }
```
```
  function ThemedButton(props) {
    // Используйте Consumer, чтобы считать текущий контекст темы.
    // React будет искать выше ближайший поставщик (Provider) темы и использует его значение.
    // В данном примере текущая тема имеет значение "dark".
    return (
      <ThemeContext.Consumer>
        {theme => <Button {...props} theme={theme} />}
      </ThemeContext.Consumer>
    );
  }

```

# Потребление множества контекстов

Чтобы поддерживать перерисовку контекста быстрой, React должен сделать каждого потребителя контекста отдельным узлом в дереве.

```
// Контекст темы. Светлая тема по умолчанию.
  const ThemeContext = React.createContext('light');
  
  // Контекст Signed-in  пользователя
  const UserContext = React.createContext();
  
  class App extends React.Component {
    render() {
      const {signedInUser, theme} = this.props;
  
      // Компонент приложения, который предоставляет начальные значения контекста
      return (
        <ThemeContext.Provider value={theme}>
          <UserContext.Provider value={signedInUser}>
            <Layout />
          </UserContext.Provider>
        </ThemeContext.Provider>
      );
    }
  }
  ```
  
  ```
  function Layout() {
    return (
      <div>
        <Sidebar />
        <Content />
      </div>
    );
  }
  ```
  
  ```
  // Компонент может потреблять множество контекстов
  function Content() {
    return (
      <ThemeContext.Consumer>
        {theme => (
          <UserContext.Consumer>
            {user => (
              <ProfilePage user={user} theme={theme} />
            )}
          </UserContext.Consumer>
        )}
      </ThemeContext.Consumer>
    );
  }

```

# Контекст вместе с хуками

```
const themes = {
  light: {
    foreground: "#000000",
    background: "#eeeeee"
  },
  dark: {
    foreground: "#ffffff",
    background: "#222222"
  }
};

const ThemeContext = React.createContext(themes.light);

function App() {
  return (
    <ThemeContext.Provider value={themes.dark}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

function ThemedButton() {
  const theme = useContext(ThemeContext);
  return (
    <button style={{ background: theme.background, color: theme.foreground }}>
      Я стилизован темой из контекста!
    </button>
  );
}

```

# хук useCallback

```
const memoizedCallback = useCallback(() => {
    doSomething(a, b);
  }, [a, b]);

```

Возвращает мемоизированный колбэк.

Эта функция сработает только есть занчение в зависимостях поменялось

Пример:

```
import React, { useState, useCallback, useEffect } from 'react';

function App() {
  const [val, setVal] = useState('');

  const randomInteger = (min, max) => {
    let rand = min + Math.random() * (max + 1 - min);
    return Math.floor(rand);
  }

  const changeValue = useCallback(() => {
    setVal(randomInteger(1,3))
  }, [randomInteger(1,3)]) // Только если поменяется рандомное число от 1 до 3, то мы вызываем функцию и обновляем состояние

  useEffect(() => {
    console.log('update');
  }, [val])

  return (
    <button onClick={changeValue}>
      {val}
    </button>
  );
}

export default App;


```

# Redux

#  Установка Redux и начало работы

```
npm install redux

```

Также надо установить redux developer tools - расширения в бразуере

В корне проекта надо создать папку "store", в которой будет файл store.js и rootReducer.js

Redux представляет собой контейнер для управления состоянием приложения

Redux не привязан непосредственно к React.js и может также использоваться с другими js-библиотеками и фреймворками.

Ключевые моменты Redux:

- Хранилище (store): хранит состояние приложения

- Действия (actions): некоторый набор информации, который исходит от приложения к хранилищу и который указывает, что именно нужно сделать. 

Для передачи этой информации у хранилища вызывается метод dispatch().

- Создатели действий (action creators): функции, которые создают действия

- Reducer : функция (или несколько функций), которая получает действие и в соответствии с этим действием изменяет состояние хранилища

Общую схему взаимодействия элементов архитектуры Redux можно выразить следующим образом

![image](https://user-images.githubusercontent.com/16369478/114435063-6a100300-9bcc-11eb-87d8-b380108de3fa.png)

Из View (то есть из компонентов React) мы посылаем действие, это действие получает функция reducer, которая в соответствии с действием обновляет состояние хранилища. 

Затем компоненты React применяют обновленное состояние из хранилища.


# createStore

Когда вы создали базовую структуру для работы с хранилищем Redux пришло время понять то как вы можете взаимодействовать с ним.

Глобальное хранилище приложения создаётся в отдельном файле, который как правило называется store.js:

```
// Код файла store.js
import { createStore } from 'redux';

const store = createStore(reducer);

export default store;

```

# reducer()

reducer — чистая функция которая будет отвечать за обновление состояния. 

Здесь реализовывается логика в соответствие с которой будет происходить обновление полей store.

Так выглядит базовая функция reducer:


```
function reducer(state, action) {
    switch(action.type) {
        case ACTION_1: return { value: action.value_1 };
        case ACTION_2: return { value: action.value_2 };
        
        default: return state;
    }
}

```

Функция принимает значение текущего состояния и обьект события (action). Обьект события содержит два свойства — это тип события (action.type) и значение события (action.value).

К примеру если нужно обработать событие onChange для поля ввода то объект события может выглядеть так:

```
{
    type: "ACTION_1",
    value: "Здесь значение поля формы"
}

```

Некоторые события могут не нуждаться в передаче каких-либо значении. 

К примеру, обрабатывая событие onClick мы можем сигнализировать о том, что событие произошло, более никаких данных не требуется

# dispatch()

Что бы обновить store необходимо вызвать метод dispatch(). 

Он вызывается у объекта store который вы создаёте в store.js. Этот объект принято называть store поэтому обновление состояния в моём случае выглядит так:

```
store.dispatch({ type: ACTION_1, value_1: "Some text" });

```

Эта функция вызовет функцию reducer который обработает событие и обновит соответствующие поля хранилища.

# actionCreator()

На самом деле передавать объект события напрямую в dispatch() является признаком плохого тона. 

Для этого нужно использовать функцию под названием actionCreator. 

Она делает ровно то что и ожидается. 

Создаёт событие! Вызов этой функции нужно передавать как аргумент в dispatch а в actionCreator передавать необходимое значение (value). 

Базовый actionCreator выглядит следующим образом:

```
function action_1(value) {
    return { 
        type: ACTION_1,
        value_1: value
    };
}

export default action_1;

```

Таким образом вызов dispatch должен выглядеть так:

```
store.dispatch(action_1("Some value"));

```

# Actions

actions это константы, описывающие событие. 

Обычно это просто строка с названием описывающее событие. 

К примеру константа описывающее событие номер один будет выглядеть следующем образом:

```
const ACTION_1 = "ACTION_1";

export default ACTION_1;

```
