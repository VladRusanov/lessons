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

Аналогично, <input type="checkbox"> и <input type="radio"> используют defaultChecked, а <select> и <textarea> — defaultValue.

# props.children

Есть такой пропс как - children

пропс children принимает все дочерние компоненты, которые мы обернем в компонент

Пример:


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
                <label>
                    Имя:
                    <input type="text" />
                </label>
                <WrapperComponent>
                    <div>
                        <input type="submit" value="Отправить" />
                    </div>
                    <div>
                        <input type="submit" value="Отправить" />
                    </div>
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

WrapperComponent оборачивает какие-то элементы и 
