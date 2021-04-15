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
