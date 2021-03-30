# События 

Вешаются так же как и JS. Вешаем напрямую в JSX 

```
import React, { Component } from 'react'
import './App.css'

class App extends Component {
  handleClick = () => {
    console.log('click');
  }

  render() {
    return (
      <div onClick={this.handleClick}>
        Click me
      </div>
    );
  }
}

export default App

```


# Props, State, life cycle 

Props - объект с данными, которые мы перекидываем между компонентами

```
import React, { Component } from 'react'

class App extends Component {
  render() {
    return (
      <div>
        <Test b='Hello' a={5} />
      </div>
    );
  }
}

```


```
class Test extends React.Component {
  render() {
    console.log(this.props); // { a: 5, b: 'Hello' }
    return (
      <input placeholder={this.props.b} value={this.props.a} />
    );
  }
}

```

В нашем примере - a и b это пропсы

# Пропсы можно только читать

```
import React, { Component } from 'react'

class App extends Component {
  render() {
    return (
      <div>
        <Test b='Hello' a={5} />
      </div>
    );
  }
}

```


```
class Test extends React.Component {
  render() {
    console.log(this.props); // { a: 5, b: 'Hello' }
    this.props.b = 10; // !! ТАК НЕЛЬЗЯ !!
    return (
      <input placeholder={this.props.b} value={this.props.a} />
    );
  }
}

```

В пропсы можно кидать все что хотим: числа, строки, функции...


# State (состояние)

!!! ОЧЕНЬ ВАЖНАЯ ТЕМА !!!

!!! ОЧЕНЬ ВАЖНАЯ ТЕМА !!!

!!! ОЧЕНЬ ВАЖНАЯ ТЕМА !!!


Объект state описывает внутреннее состояние компонента, он похож на props за тем исключением, что состояние определяется внутри компонента и доступно только из компонента.

Если props представляет входные данные, которые передаются в компонент извне, то состояние хранит такие объекты, которые создаются в компоненте и полностью зависят от компонента.

Также в отличие от props значения в state можно изменять.

И еще важный момент - значения из state должны использоваться при рендеринге. 

Если какой-то объект не используется в рендерниге компонента, то нет смысла сохранять его в state.



!!!!!! ЕСЛИ МЕНЯЕТСЯ СОСТОЯНИЕ, ТО КОМПОНЕНТ ПЕРЕРЕНДЕРИВАЕТСЯ !!!!!!

# Как создать состояние компонента

Место, где можно установить объект state - это конструктор класса:

```
class App extends Component {
  constructor() {
    super();
    this.state = { // Состояние компонента
      text: 'Hello World'
    }
  }

  render() {
    return (
      <div>
        {this.state.text} // Обращение к полю состояния
      </div>
    );
  }
}

```

# Как менять состояние


!!!!!! ЕСЛИ МЕНЯЕТСЯ СОСТОЯНИЕ, ТО КОМПОНЕНТ ПЕРЕРЕНДЕРИВАЕТСЯ !!!!!!

Для обновления состояния вызывается функция setState():


```
	
this.setState({welcome: "Привет React"});

```

Изменение состояния вызовет повторный рендеринг компонента, в соответствии с чем веб-страница будет обновлена.

В то же время не стоит изменять свойства состояния напрямую, например:

```
	
this.state.welcome = "Привет React";

```

При этом нам не обязательно обновлять все его значения. 

В процессе работы программы мы можем обновить только некоторые свойства. 

Тогда необновленные свойства будут сохранять старые значения.


!!!!!!!!!!!! this.setState нельзя писать в методе render !!!!!!!!!!!!

Это спровоцирует бесконечный рендер компонента и приложение упадет. Почему так?

Смена состояние провоцирует рендер конпонента. 

Метод render срабатывает при рендере компонента

render -> setState -> render -> setState и т.д.


Пример использования:

```
import React, { Component } from 'react'
import './App.css'

class App extends Component {
  constructor() {
    super();
    this.state = {
      test: 'Hello World'
    }
  }

  onClick = () => {
    this.setState({
      test: 'New Text'
    })
  }

  render() {
    return (
      <div onClick={this.onClick}>
        {this.state.test}
      </div>
    );
  }
}

export default App

```

При клике меняем состояние компонента -> при смене состояния компонент перерендеривается -> видим новый результат


# Что будет если мы перепишем arrow function для события на обучную функцию?

```
import React, { Component } from 'react'
import './App.css'

class App extends Component {
  constructor() {
    super();
    this.state = {
      test: 'Hello World'
    }
  }

  onClick() {
    this.setState({
      test: 'New Text'
    })
  }

  render() {
    return (
      <div onClick={this.onClick}>
        {this.state.test}
      </div>
    );
  }
}

export default App

```

В консоли будет ошибка TypeError: Cannot read property 'setState' of undefined

Почему так?

При обращении к this в JSX-колбэках необходимо учитывать, что методы класса в JavaScript по умолчанию не привязаны к контексту.

Если вы забудете привязать метод this.onClick и передать его в onClick, значение this будет undefined в момент вызова функции.


Как пофиксить

```
class App extends Component {
  constructor() {
    super();
    this.state = {
      test: 'Hello World'
    }

    this.onClick = this.onClick.bind(this); // привязываем контект
  }

  onClick() {
    this.setState({
      test: 'New Text'
    })
  }

  render() {
    return (
      <div onClick={this.onClick}>
        {this.state.test}
      </div>
    );
  }
}

```

Как передать аргументы в событие? (Редко когда надо)

```
class App extends Component {
  constructor() {
    super();
    this.state = {
      test: 'Hello World'
    }
  }

  onClick = (e, newText) => {
    this.setState({
      test: newText
    })
  }

  render() {
    return (
      <div onClick={(e) => this.onClick(e, 'TEST')}>
        {this.state.test}
      </div>
    );
  }
}

```

Проблема этого синтаксиса в том, что при каждом рендере LoggingButton создаётся новый колбэк


# Асинхронное обновление состояния

При наличии нескольких вызовов setState() React может объединять их в один общий пакет обновлений для увеличения производительности.

Есть такой пример

```
class App extends Component {
  constructor() {
    super();
    this.state = {
      count: 1
    }
  }

  addCount = () => {
    this.setState({
      count: this.state.count + 1
    })
    this.setState({
      count: this.state.count + 1
    })
  }

  render() {
    return (
      <div>
        {this.state.count}

        <button onClick={this.addCount}>Add count</button>
      </div>
    );
  }
}

```

Чему будет равен this.state.count при первом клике?

Там будет 2

Мы видим, что в методе addCount мы 2 раза вызвали setState, но обновление состояния произошло только 1 раз

Почему так?

Потому что react объединяет все обновления в одно (последний setState) и только потом идет обновление

# Еще пример

Так как объекты this.props и this.state могут обновляться асинхронно, не стоит полагаться на значения этих объектов для вычисления состояния. Например:

```
this.setState({
  counter: this.state.counter + this.props.increment,
});

```

Для обновления надо использовать другую версию функции setState(), которая в качестве параметра принимает функцию. 

Данная функция имеет два параметра: предыдущее состояние объекта state и объект props на момент применения обновления:

```
this.setState(function(prevState, props) {
  return {
    counter: prevState.counter + props.increment
  };
});

```


Код:

```
// Test.jsx

class Test extends Component {
  constructor() {
    super();
    this.state = {
      counter: 0
    }
  }

  addCount = () => {
    this.setState({ counter: this.state.counter + this.props.increment });
    this.setState({ counter: this.state.counter + this.props.increment });
  }

  render() {
    return (
      <div>
        {this.state.counter}

        <button onClick={this.addCount}>Add count</button>
      </div>
    );
  }
}

```

```
App.jsx

class App extends Component {
  render() {
    return (
      <div>
        <Test increment={1} />
      </div>
    );
  }
}

```

При клике обновление происходить только 1 раз. 

Два this.setState "схлопытаются" и получается 1 обновление (берется только последний setState)

# Как это пофиксить

Для обновления надо использовать другую версию функции setState(), которая в качестве параметра принимает функцию. 

**Данная функция имеет два параметра: предыдущее состояние объекта state и объект props на момент применения обновления:**

```
this.setState(function(prevState, props) {
  return {
    counter: prevState.counter + props.increment
  };
});

```

или же arrow function 

```
this.setState((prevState, props) => {
  return {
    counter: prevState.counter + props.increment
  };
});

```


# Исправим прошлый пример

```
// Test.jsx

class Test extends Component {
    constructor() {
        super();
        this.state = {
            counter: 0
        }
    }

    addCount = () => {
        this.setState((prevState, props) => ({ counter: prevState.counter + props.increment }));
    }

    pressBtn = () => {
        this.addCount();
        this.addCount();
    }

    render() {
        return (
            <div>
                <div>Counter: {this.state.counter} <br />Increment: {this.props.increment}</div>

                <button onClick={this.pressBtn}>Add count</button>
            </div>
        );
    }
}

```

```
// App.jsx

class App extends Component {
  render() {
    return (
      <div>
        <Test increment={1} />
      </div>
    );
  }
}

```

# Еще пример

```
class Test extends Component {
    constructor() {
        super();
        this.state = {
            counter: 0
        }
    }

    addCount = () => {
        this.setState({
            counter: this.state.counter + 1
        });
    }

    pressBtn = () => {
        // Будет только одно обновление состояния
        // При каждом клике будет + 1 в стостоянии, а не + 2
        this.addCount();
        this.addCount();
    }

    render() {
        return (
            <div>
                <span>{this.state.counter}</span>
                <button onClick={this.pressBtn}>Add count</button>
            </div>
        );
    }
}

```

Как пофиксить??

```
class Test extends Component {
    constructor() {
        super();
        this.state = {
            counter: 0
        }
    }

    addCount = () => {
        this.setState((prevState) => ({
            counter: prevState.counter + 1
        }))
    }

    pressBtn = () => {
        this.addCount();
        this.addCount();
    }

    render() {
        return (
            <div>
                <span>{this.state.counter}</span>
                <button onClick={this.pressBtn}>Add count</button>
            </div>
        );
    }
}

```

# Жизненный цикл компонента

!!! Это все надо выучить !!!

В процессе работы компонент проходит через ряд этапов жизненного цикла. 

На каждом из этапов вызывается определенная функция, в которой мы можем определить какие-либо действия:

Методы срабатывают в том же порядке, в котором написано


# ЭТАП МОНТИРОВАНИЯ

- constructor(props): конструктор, в котором происходит начальная инициализация компонента

- static getDerivedStateFromProps(props, state): вызывается непосредственно перед рендерингом компонента. Этот метод не имеет доступа к текущему объекту компонента (то есть обратиться к объкту компоненту через this) и должен возвращать объект для обновления объекта state или значение null, если нечего обновлять.

- render(): рендеринг компонента

- componentDidMount(): вызывается после рендеринга компонента. Здесь можно выполнять запросы к удаленным ресурсам

- componentWillUnmount(): вызывается перед удалением компонента из DOM

```
class App extends Component {
  constructor() {
    super();
    console.log('1')
  }

  static getDerivedStateFromProps(props, state) {
    console.log('2')
  }

  componentDidMount() {
    console.log('4')
  }

  render() {
    console.log('3')
    return (
      <div>
        <Test increment={1} />
      </div>
    );
  }
}

```


# static getDerivedStateFromProps

Этот метод нужен, если у нас есть зависимость состояния от пропсов


```
constructor() {
    super();
    this.state = {
      count: this.props.count // так делать нельзя
    }
  }

```

При изменении пропсов наше состояние не поменяется. 

React не поймет, что что-то поменялось из-за того что ссылка не поменялась (новый обьет состояния не появился, а остался старый)

Как сделать, чтобы все работало?

```
 static getDerivedStateFromProps(nextProps, prevState) {
    if (prevState.count !== nextProps.count) {  //  Если данные поменялись, то мы поменяем состояние
      return { // возвращаем новое состояние
        count: nextProps.concount
      }
    }
    // Если данные не поменялись, то состояние не меняем
    return null;
  }

```

# componentDidMount

Это метод вызывается когда компонент уже отрендерился на страницу

```
componentDidMount() {
  console.log('Компонент уже в DOM')
}

```


# Этап обновления

- static getDerivedStateFromProps(props, state)

- shouldComponentUpdate(nextProps, nextState): вызывается каждый раз при обновлении объекта props или state. В качестве параметра передаются новый объект props и state. Эта функция должна возвращать true (надо делать обновление) или false (игнорировать обновление). По умолчанию возвращается true. Но если функция будет возвращать false, то тем самым мы отключим обновление компонента, а последующие функции не будут срабатывать.

- render(): рендеринг компонента (если shouldComponentUpdate возвращает true)

- getSnapshotBeforeUpdate(prevProps, prevState): вызывается непосредственно перед компонента. Он позволяет компоненту получить информацию из DOM перед возможным обновлением. Возвращает в качестве значения какой-то отдельный аспект, который передается в качестве третьего параметра в метод componentDidUpdate() и может учитываться в componentDidUpdate при обновлении. Если нечего возвращать, то возвращается значение null

- componentDidUpdate(prevProps, prevState, snapshot): вызывается сразу после обновления компонента (если shouldComponentUpdate возвращает true). В качестве параметров передаются старые значения объектов props и state. Третий параметр - значение, которое возвращает метод getSnapshotBeforeUpdate


# HomeWork

1. Сделать инпут range. При перетаскивании ползунка в range мы записываем value из range в новый инпут

минимальное значение range - 1

максимальное значение range - 100

шаг range - 1

Вот так должно быть


![chrome-capture (1)](https://user-images.githubusercontent.com/16369478/112895642-0bd02400-90e6-11eb-8395-68b1aecb3145.gif)


!!! В КОНСОЛИ НЕ ДОЛЖНО БЫТЬ ВОРНИНГОВ И ОШИБОК !!!!


2. Есть 2 компонента App (который уже у вас есть) и Test (создать)

Как это будет выглядеть?


![chrome-capture](https://user-images.githubusercontent.com/16369478/112891481-dd037f00-90e0-11eb-8e0c-8a9c4bbcd807.gif)

Test "вызывается" в App

Есть инпут и кнопка (в компоненте Test)

По нажатию на кнопку значение в инпуте увеличивается на 1

Test принимает в пропсы 3 поля - trigger (какое-то число), value (какое=то число), handleClick (функция для события)

- trigger - максимальное число, которое может отобразить в инпуте (пусть это будет 15)

- value - текущее значение для инпута (оно храниться в состоянии компонента App и передается в Test)

- handleClick - функция, которая будет менять это value (инициализация метода в App, передается через просто в Test)

В инпут (который в Test) пишется value из пропсов (value получем из состояния App через пропс)
 
На кнопку (которая в Test) вешаем событие "клик", которое обрабатывает функция handleClick из пропсов (функция находится в App и передается через прост в Test). 

Это функция меняет поле value в состоянии компонента App

Как только наше значение дошло до trigger, то значение в инпуте не меняется

- Подсказка

Делаем через метод getDerivedStateFromProps, который будет в компоненте Test

