# Фрагменты

Возврат нескольких элементов из компонента является распространённой практикой в React.

Фрагменты позволяют формировать список дочерних элементов, не создавая лишних узлов в DOM.

```
render() {
  return (
    <React.Fragment>
      <ChildA />
      <ChildB />
      <ChildC />
    </React.Fragment>
  );
}

```

Также существует сокращённая запись.

```
render() {
  return (
    <>
      <ChildA />
      <ChildB />
      <ChildC />
    </>
  );
}

```

Мотивация

Возврат списка дочерних элементов из компонента — распространённая практика. Рассмотрим пример на React:

```
class Table extends React.Component {
  render() {
    return (
      <table>
        <tr>
          <Columns />
        </tr>
      </table>
    );
  }
}

```
Columns должен вернуть несколько элементов td, чтобы HTML получился валидным. 

Если использовать div как родительский элемент внутри метода render() компонента Columns, то HTML окажется невалидным.

```
class Columns extends React.Component {
  render() {
    return (
      <div>
        <td>Привет</td>
        <td>Мир</td>
      </div>
    );
  }
}

```

Результатом вывода <Table /> будет:

```
<table>
  <tr>
    <div>
      <td>Привет</td>
      <td>Мир</td>
    </div>
  </tr>
</table>

```

Фрагменты решают эту проблему.

# Фрагменты с ключами

Фрагменты, объявленные с помощью <React.Fragment>, могут иметь ключи. 

Например, их можно использовать при создании списка определений, преобразовав коллекцию в массив фрагментов.

```
function Glossary(props) {
  return (
    <dl>
      {props.items.map(item => (
        // Без указания атрибута `key`, React выдаст предупреждение об его отсутствии
        <React.Fragment key={item.id}>
          <dt>{item.term}</dt>
          <dd>{item.description}</dd>
        </React.Fragment>
      ))}
    </dl>
  );
}

```

key — это единственный атрибут, допустимый у Fragment


# Приватный роутинг ( аутентифицированные маршруты )

Есть такие роуты, которые должны работать только при каки-то условия

К примеру:

У нас есть sign in в приложение. Нам надо залогиниться в приложение, чтобы пользоваться нашим приложение.

При успешном логине в наше приложение у нас будет токен авторизации.

И только если есть этот токен (к примеру) мы можем перейти по какому-то роуту (к примеру на экран с фильмамми)

# Как реализовать такие приватные роуты

```
class App extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      isAuthenticated: true // ЭТО СОСТОЯНИЕ БУДЕТ МЕНЯТЬСЯ
    }
  }

  render() {
    return (
      <div>
        <BrowserRouter>
          <Switch>
            <Route exact path="/" component={Main} />
            <PrivateRoute
              exact
              path="/films"
              component={Table}
              isAuthenticated={this.state.isAuthenticated}
            />
          </Switch>
        </BrowserRouter>
      </div>
    )
  }
}

```

```
import React from 'react';
import { Redirect, Route } from 'react-router-dom'

export const PrivateRoute = ({ component: Component, isAuthenticated, ...rest }) => {
    return (
        <Route
            {...rest}
            render={props => {
                console.log('props: ', props);
                return (
                    isAuthenticated ? ( // Условие по которому продйет роут
                        <Component {...props} {...rest} />
                    ) : (
                        <Redirect to="/" />
                    )
                )
            }}
        />
    )
}

```

Для выполнения переадресации с одного маршрута на другой в модуле react-router-dom определен компонент Redirect

# Как перенаправить юзера по роуту

Для перенаправления мы использовали <Link>

А если нам надо перенаправить юзера, основываясь на каких-то данные от юзера? 

К примеру:

как только получили какие-то данные от юзера, то сразу перенаправляем его на какой-то роут

# history

У всех компонентов, которые вписаны в роут есть спец. пропс, который называется history

Есть компонент

```
class App extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      isAuthenticated: true // ЭТО СОСТОЯНИЕ БУДЕТ МЕНЯТЬСЯ
    }
  }

  render() {
    return (
      <div>
        <BrowserRouter>
          <Switch>
            <Route exact path="/" component={Main} />
            <PrivateRoute
              exact
              path="/films"
              component={Table}
              isAuthenticated={this.state.isAuthenticated}
            />
          </Switch>
        </BrowserRouter>
      </div>
    )
  }
}

```


```
import React from 'react';
import { Redirect, Route } from 'react-router-dom'

export const PrivateRoute = ({ component: Component, isAuthenticated, ...rest }) => {
    return (
        <Route
            {...rest}
            render={props => {
                console.log('props: ', props);
                return (
                    isAuthenticated ? (
                        <Component {...props} {...rest} />
                    ) : (
                        <Redirect to="/" />
                    )
                )
            }}
        />
    )
}

```

```
import React, { Component } from 'react'
import { Link } from 'react-router-dom'

class Main extends Component {
    constructor(props) {
        super(props)
        this.state = {}
        this.myRef = React.createRef()
    }

    render() {
        console.log('props main:', this.props);
        return (
            <div>
                <span>Main component</span>
            </div>
        )
    }
}

export default Main

```

Что будет в консоли?

![image](https://user-images.githubusercontent.com/16369478/113711216-5ad11700-96ed-11eb-8a2a-b11d930ec442.png)

```
import React, { Component } from 'react'
import { Link } from 'react-router-dom'

class Main extends Component {
    constructor(props) {
        super(props)
        this.state = {}
        this.myRef = React.createRef()
    }

    redirectToFilms = () => {
        const { history } = this.props; // Берем метод push из объекта history.
        history.push('/films'); // Вызываем метод push. Этот метод перенаправит нас на то, что вписал в аргумент метода
    }

    render() {
        return (
            <div>
                <span>Main component</span>
                <button onClick={this.redirectToFilms}>CLICK</button>
            </div>
        )
    }
}

export default Main

```

# Управляемые компоненты

 В управляемом компоненте, данные формы обрабатываются React-компонентом. 
 
 В качестве альтернативы можно использовать неуправляемые компоненты. 
 
 Они хранят данные формы прямо в DOM.
 
 Вместо того, чтобы писать обработчик события для каждого обновления состояния, вы можете использовать неуправляемый компонент и читать значения из DOM через реф.
 
 Вот так, к примеру, обработчик неуправляемого компонента может получить имя от элемента input:
 
```
 import React, { Component } from 'react'

  class Main extends Component {
      constructor(props) {
          super(props)
          this.handleSubmit = this.handleSubmit.bind(this);
          this.input = React.createRef();
      }

      handleSubmit(event) {
          alert('Отправленное имя: ' + this.input.current.value);
          event.preventDefault();
      }

      render() {
          console.log('props main:', this.props);
          return (
              <form onSubmit={this.handleSubmit}>
                  <label>
                      Имя:
                      <input type="text" ref={this.input} />
                  </label>
                  <input type="submit" value="Отправить" />
              </form>
          )
      }
  }

  export default Main
 
 ```
 
 Неуправляемые компоненты опираются на DOM в качестве источника данных и могут быть удобны при интеграции React с кодом, не связанным с React. 
 
 Количество кода может уменьшиться, правда, за счёт потери в его чистоте.
 
 # Значения по умолчанию
 
 На этапе рендеринга атрибут value полей ввода переопределяет значение в DOM. 
 
 С неуправляемым компонентом зачастую нужно, чтобы React определил первоначальное значение, но впоследствии ничего не делал с ним. 
 
 В этом случае необходимо определить атрибут defaultValue вместо value. 
 
 Изменение значения атрибута defaultValue после монтирования компонента не обновит значение в DOM.
 
```

render() {
  return (
    <form onSubmit={this.handleSubmit}>
      <label>
        Имя:
        <input
          defaultValue="Боб"
          type="text"
          ref={this.input} />
      </label>
      <input type="submit" value="Отправить" />
    </form>
  );
}

```

Аналогично, 

```
<input type="checkbox"> и <input type="radio"> используют defaultChecked, а <select> и <textarea> — defaultValue.

```

# props.children

Есть такой пропс как - children

пропс children принимает все дочерние компоненты, которые мы обернем в компонент

Пример:

```
import React, { Component } from 'react'
import WrapperComponent from './WrapperComponent.jsx';
import './styles.css'

class Main extends Component {
    constructor(props) {
        super(props)
        this.handleSubmit = this.handleSubmit.bind(this);
        this.input = React.createRef();
    }

    handleSubmit(event) {
        alert('Отправленное имя: ' + this.input.current.value);
        event.preventDefault();
    }

    render() {
        return (
            <form onSubmit={this.handleSubmit}>
                <WrapperComponent>
                    <label>
                        Имя:
                    <input type="text" />
                    </label>
                    <div>
                        <input type="submit" value="Отправить" />
                    </div>
                </WrapperComponent>
            </form>
        )
    }
}

export default Main

```

```
import React from 'react';

export default class WrapperComponent extends React.Component {
    constructor(props) {
        super(props)
        this.state = {}
    }

    render() {
        return (
            <div className="wrapper">
                <div>{this.props.children}</div>
            </div>
        )
    }
}

```

# Введение в хуки

Хуки — нововведение в React 16.8, которое позволяет использовать состояние и другие возможности React без написания классов

# Что такое хук?

Хук — это специальная функция, которая позволяет «подцепиться» к возможностям React. 

Например, хук useState предоставляет функциональным компонентам доступ к состоянию React. Мы узнаем про другие хуки чуть позже.

# Хук состояния

хук состояние - useState.

```
import React, { useState } from 'react';

export function Hooks() {
    // Объявляем новую переменную состояния "count"
    const [count, setCount] = useState(0);

    // setCount - грубо говоря наш setState, который меняет поле count

    const addCount = () => {
        setCount(count + 1)
    }

    return (
        <div>
            <p>Вы нажали {count} раз</p>
            <button onClick={addCount}>
                Нажми на меня
      </button>
        </div>
    );
}

```

# Эквивалентный пример с классом

```
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>Вы кликнули {this.state.count} раз(а)</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Нажми на меня
        </button>
      </div>
    );
  }
}

```

# Объявление нескольких переменных состояния

```
import React, { useState } from 'react';

export function Hooks() {
    const [count, setCount] = useState(0);
    const [isOpen, setIsOpen] = useState(false);

    const addCount = () => {
        setCount(count + 1)
    }

    const toggleOpen = () => {
        setIsOpen((prevIsOpen) => !prevIsOpen)
    }

    return (
        <div>
            <p>Вы нажали {count} раз</p>
            <p>Is open - {isOpen}</p>
            <button onClick={addCount}>
                +1
            </button>

            <button onClick={toggleOpen}>
                toggle open
            </button>
        </div>
    );
}

```


# Хук эффекта

С помощью хука эффекта useEffect вы можете выполнять побочные эффекты из функционального компонента. 

Он выполняет ту же роль, что и componentDidMount, componentDidUpdate и componentWillUnmount в React-классах, объединив их в единый API.

useEffect прнимает 2 аргумента - коллбэк функция и массив

- коллбэк срабаотывает точно также как и componentDidMount and conponentDidUpdate

- массив нужен для зависимостей. Если в него передаем какую-то переменную, то при изменении этой переменной у нас будет повторно вызван useEffect 

```
import React, { useEffect, useState } from 'react';

export function Hooks() {
    const [count, setCount] = useState(0);
    const [isOpen, setIsOpen] = useState(false);

    const addCount = () => {
        setCount(count + 1)
    }

    const toggleOpen = () => {
        setIsOpen((prevIsOpen) => !prevIsOpen)
    }

    useEffect(() => {
        // componentDidMount and conponentDidUpdate
        toggleOpen(); // как только отрендериться компонент -> выполнится эта функция
        return () => {
            // componentWillUnmount
        }
    }, []) // второй агрумент для зависимостей

    return (
        <div>
            <p>Вы нажали {count} раз</p>
            <p>Is open - {isOpen ? 'yes' : 'no'}</p>
            <button onClick={addCount}>
                +1
            </button>

            <button onClick={toggleOpen}>
                toggle open
            </button>
        </div>
    );
}

```

Пример на зависимости

```
import React, { useEffect, useState } from 'react';

export function Hooks() {
    const [count, setCount] = useState(0);
    const [isOpen, setIsOpen] = useState(false);

    const addCount = () => {
        setCount(count + 1)
    }

    const toggleOpen = () => {
        setIsOpen((prevIsOpen) => !prevIsOpen)
    }

    useEffect(() => {
        // при изменении count будет вызвана функция toggleOpen
        toggleOpen()
    }, [count])

    return (
        <div>
            <p>Вы нажали {count} раз</p>
            <p>Is open - {isOpen ? 'yes' : 'no'}</p>
            <button onClick={addCount}>
                +1
            </button>

            <button>
                toggle open
            </button>
        </div>
    );
}

```

# HomeWork

1. Переделать 1 домашку в REACT_2 (таска с input range) на хуки

2. Нужны 2 формы для регистрации и логина (можно взять из прошлой домашки и как-то модифицировать через props.children).

Попробуйте сделайте через приватные роуты. Пусть у вас в состоянии будет поле signed: true. если это поле true, то роут работает

Делаем все хуками

SignIn - имя, логин, пароль, повторите пароль

SignUp - логин, пароль

При регистрации ( по натажию на кнопку sign up ) в localStorage создаем объект с данными, которые ввел юзер

При вводе этих же данных (логина и пароля) в форму signIn выводим на экран алертик "Получилось!"

Если ввели данные юзера, которого нет у нас, то перекидываем юзера на форму signUp
