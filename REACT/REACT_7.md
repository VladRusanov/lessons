# Redux

Что надо для работы?

```
npm i redux
npm i react-redux
npm i redux-devtools-extension

```

И надо установить плагин для браузера "redux developer tools"

Создайте папку store, в которой будут:

файл store.js

файл rootReducer.js

папка actions -> файл action.js

папка reducers -> файл dataReducer.js

# Что такое Redux

Это менеджер состояний. Редакс стор хранит в себе данные, которые могутбыть использованы где угодно в приложении.

# Использование Redux

Разберём основные концепции библиотеки Redux, которые нужно понимать начинающим.

# Неизменяемое дерево состояний

В Redux общее состояние приложения представлено одним объектом JavaScript — state (состояние) или state tree (дерево состояний). 

**Неизменяемое дерево состояний доступно только для чтения, изменить ничего напрямую нельзя. **

**Изменения возможны только при отправке action (действия).**

# Store 

Что такое store

Это и есть то место где будут храниться все наши данные

Как его создать?

```
// store.js
import { createStore } from 'redux'
import { composeWithDevTools } from 'redux-devtools-extension'

import rootReducer from './rootReducer.js'

const store = createStore(rootReducer, composeWithDevTools())
export default store

```

# Provider

Чтобы дать доступ к стору всем компонентам нам надо обернуть все в <Providor>

```
// App.jsx
import React from 'react';
import { Test } from './components/Test.jsx';
import { Provider } from 'react-redux'
import store from './store/store.js';

function App() {
  return (
    <Provider store={store}>
      <Test />
    </Provider>
  );
}

export default App;

```


# Действия (action)

Действие (action) — это JavaScript-объект, который лаконично описывает суть изменения:

```
{ 
  type: 'CLICKED_SIDEBAR' 
} 
// подробности об изменении 
{ 
  type: 'SELECTED_USER', 
  payload: {
    userAge: 1
  } 
}

```

Action хранит в себе два поля - type (обязательное) и payload(необязательное)

type - тип action, которые вызвался в приложении. Именно по этому типу redux определяет где и что менять

!!!!!!!!!!!!!!!! ** Типы действий должны быть константами ** !!!!!!!!!!!!!!!!

# Генераторы действий

Генераторы действий (actions creators) — это функции, создающие действия.

```
function addItem(t) { 
  return { 
    type: ADD_ITEM, 
    title: t 
  } 
}

```

# Редукторы (reducer)

При запуске действия обязательно что-то происходит и состояние приложения изменяется. Это работа редьсеров.

# Что такое редуктор (reducer)

Редуктор (reducer) — это чистая функция, которая вычисляет следующее состояние дерева на основании его предыдущего состояния и применяемого действия.

То есть:

action говорит, что в прложении что-то поменялось, а reducer понимает где и что поменялось (по типу экшена) и меняет уже что-то в редакс сторе

Как его создать?

```
// dataReducer.js

const initialState = { // тут уже конфигурация базового состояния для нашего редакс-стора
    data: 1
}

export default function dataReducer(state = initialState, action) {
    switch (action.type) {
        case 'SET_NEW_DATA':
            return {
                data: action.payload
            };
        default:
            return state;
    }
}

```

редьюсер принимает 2 аргумента - state и action(который вызвался в приложении)

Этот редьюсер получает доступ к типу экшена, который вызвался в приложении, и проверят через switch этот тип. 

Если тип экшена под что-то подошел, то он забирает payload из этого экшена и записывает новые данные в нужное нам поле


И таких редьюсеров в нас может быть очень многоооооооооо

# rootReducer

Это точно такой же редьюсер как и все остальные, только он должен объединить все остальные редьюсеры

И именно этот rootReducer вписывается в store

```
import { combineReducers } from 'redux';

import dataReducer from './reducers/dataReducer.js';

export default combineReducers({
    dataReducer,
    // тут будут все остальные редьюсеры
  })

```

# Как вызывать экшены

!!!! Просто напрямую написать вызов action creator нельзя !!!!

Чтобы вызвать action creator нам нужен dispatcher, которые будет явно говорить, что это вызов именно redux action, а не просто функции

Как создать этот диспатчер

```

import React from 'react';
import { useDispatch } from 'react-redux'


export const TestComponent = (props) => {
  const dispatcher = useDispatch();
  dispatcher(/* Тут уже будет вызов какого-то actions creator */);
  
}

```

dispatcher нам создает хук - useDispatch()

dispatcher принимает 1 аргумент - нужные нам вызов actions creator


# пример использования всего этого

Давайте создадим стор для нащих данных. Пусть в нашем сторе будет одно поле - data

Пусть будет компонент, в котором будет кнопка и по нажатия на эту кнопку мы будем менять нашу data на какое-то новое значение


1. Создадим store

```
// store.js
import { createStore } from 'redux'
import { composeWithDevTools } from 'redux-devtools-extension'

import rootReducer from './rootReducer.js'

const store = createStore(rootReducer, composeWithDevTools())
export default store


```

2. Создадим rootReducer

```
// rootReducer.js

import dataReducer from './reducers/dataReducer.js';
import { combineReducers } from 'redux';

export default combineReducers({
    dataReducer,
    // тут будут все остальные редьюсеры
})

```

3. Создадим редьюсер

```
// dataReducer.js

const initialState = {
    data: 1
}

export default function dataReducer(state = initialState, action) {
    switch (action.type) {
        case 'SET_NEW_DATA':
            return {
                data: action.payload
            };
        default:
            return state;
    }
}

```

4. Создадим Action Creator

```
// action.js
export const addCounter = (newData) => {
    return {
        type: 'SET_NEW_DATA',
        payload: newData
    }
}

```

5. Создадим наш компонент

```
import React from 'react';
import { useDispatch } from 'react-redux'
import { addCounter } from '../store/action'

export const Test = () => {
    const dispatcher = useDispatch();

    const test = () => {
        dispatcher(addCounter(5));
    }

    return (
        <button onClick={test}>Hello</button>
    )
}

```

Открывает консоль redux(то расширение, которое вы качали. Оно находиться в дев тулах браузера) -> Тыкаем на кнопочку -> Радуемся 


# Как получить доступ к данным в сторе

Для этого используется еще один хук - useSelect()

Этот хук нужен для подписки на обновления поле в сторе 

Это хук прнимает в себя колл бэк функция, которая приминает в меня (в аргумент) весь наш стор. Эта функция должна вернуть поле из стора, которая нам нужно

```
const { data } = useSelector(({ dataReducer: { data } }) => ({ // достаем data из поля dataReducer в сторе
        data // возвращаем это поле
}));

```

Давайте допишем наш компонент, и сделать так, чтобы выводилось значение из стора на страницу

```
import React from 'react';
import { useDispatch, useSelector } from 'react-redux'
import { addCounter } from '../store/action'

export const Test = () => {
    const dispatcher = useDispatch();
    const { data } = useSelector(({ dataReducer: { data } }) => ({
        data
    }));

    const test = () => {
        dispatcher(addCounter(5));
    }

    return (
        <button onClick={test}>{data}</button>
    )
}

```

# тоже самое, но классами

Тут все чуть чуть сложнее

У нас появляются 3 новые вещи

- mapStateToProps

- mapDispatchToProps

- connect


mapStateToProps - Это функция, которая в аргумент принимает весь стор. Эта функция должна вернуть обьект с теми свойствами, которые нам нужны в компоненте

mapDispatchToProps - Это обьект, который кидает нужные нам экшены в компонент

connect - НОС, которые собирает mapStateToProps и mapDispatchToProps и кидает это все в компонент

```
import React from 'react'
import { connect } from 'react-redux'

import { addCounter } from '../store/action'

class Test2 extends React.Component {
    click = () => {
        console.log(1);
        this.props.addCounter(5)
    }

    render() {
        return (
            <>
                <button onClick={this.click}>Click</button>
                {this.props.someData}
            </>
        )
    }
}

const mapStateToProps = (state) => ({
    // Подписываеся на обновление поля data в сторе
    someData: state.dataReducer.data // говорим, что хотим кинуть в пропсы компонента поле someData, которое хранит в себе значние из стора
})

const mapDispatchToProps = {
    addCounter, // говорим, что хотим кинуть в пропсы компонента action addCounter
}

export default connect(mapStateToProps, mapDispatchToProps)(Test2) // Собираем это все вместе и кидам в компонент


```

# Homework

Взять форму, которая була в домашке. Теперь по нажатию на кнопку "sign up" пишем данніе не в локалстор, а в стор редакса.

После того как они записались, то отображаем єти данніе под формой (дизайн любой)
