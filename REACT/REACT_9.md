# Redux-saga

Redux-saga это библиотека нацеленная делать сайд-эффекты проще и лучше путем работы с сагами.

```
npm i redux-saga

```

# Redux-saga использует ES6 генераторы

Вспомнить чуть чуть генераторы

![image](https://user-images.githubusercontent.com/16369478/115796529-a4eb1580-a3da-11eb-92ad-b5c7ea2d0642.png)


Возвращаясь к redux-saga, если говорить в общем, мы имеем сагу чья работа это следить за отправленными действиями (dispatched actions).

![image](https://user-images.githubusercontent.com/16369478/115796700-f4c9dc80-a3da-11eb-844d-ccd826885b4c.png)

Для координирования логики, которую мы хотим реализовать внутри саги, мы можем использовать вспомогательную функцию takeEvery для создания новой саги для выполнения операции.

![image](https://user-images.githubusercontent.com/16369478/115796922-5db15480-a3db-11eb-9175-a776b9aaf77a.png)

Если есть несколько запросов, **takeEvery** стартует несколько экземпляров саги-рабочего (worker saga).

Теперь мы можем реализовать логику для нашего вотчера 

![image](https://user-images.githubusercontent.com/16369478/115866525-7143d600-a442-11eb-9ae8-7addb773e49c.png)

put возвращает объект с инструкциями для промежуточного слоя (middleware) — отправить действие.

Эти возвращаемые объекты называются Эффекты (Effects). Ниже пример эффекта возвращаемого методом call:

![image](https://user-images.githubusercontent.com/16369478/115868271-e1535b80-a444-11eb-9ebe-2b2175710023.png)


# Пример на асинхронный запрос

![image](https://user-images.githubusercontent.com/16369478/115868199-bf59d900-a444-11eb-88fa-d18b1dec1a14.png)

Вместо вызова асинхронного реквеста напрямую, метод call вернет только объект описывающий эту операцию и redux-saga сможет позаботиться о вызове и возвращении результатов в функцию-генератор.

# Давайте сделаем нашу первую сагу

# Для запуска нашей Саги нужно:

- создать промежуточный слой (Saga middleware) со списком наших Саг, которые мы хотим запустить (пока что у нас есть только helloSaga)

- подключить этот промежуточный слой к хранилищу Redux (Redux store)
Мы внесём следующие изменения в файл main.js:


1. После того как мы скчали либу с сагами

Создаем наш редакс стор

```
// store.js
import { createStore, applyMiddleware  } from 'redux'
import { composeWithDevTools } from 'redux-devtools-extension'
import createSagaMiddleware from 'redux-saga'

import rootReducer from './rootReducer.js'
import rootSaga from './rootSaga.js' // rootSaga по логике делает тоже самое что и rootSaga

// rootSaga объединяет все саги 

const sagaMiddleware = createSagaMiddleware()
const store = createStore(rootReducer, composeWithDevTools(applyMiddleware(sagaMiddleware))) // вот тут поменялось
sagaMiddleware.run(rootSaga)

export default store

```

2. Нужно создать rootSaga. По логике что-то похоже на rootReducer. rootSaga должна собрать все саги в одну и именно эту сагу мы импортируем в наш стор

```
import { watcherSaga } from './sagas/requestSaga.js';
import { all } from 'redux-saga/effects'

// Здесь all эффект используется с массивом, и ваши саги будут выполняться параллельно
// Самая простая реализация
export default function* rootSaga() {
    yield all([
        watcherSaga(),
    ])
    // какой-то код после all-эфекта
}


```

Это будет работать, но есть другая реализация 

Для этого используется эффект fork

```
import { watcherSaga } from './sagas/requestSaga.js';
import { fork } from 'redux-saga/effects'

export default function* rootSaga() {
    yield fork(watcherSaga)
    yield fork(saga2)
    yield fork(saga3)
    // какой-то код после fork
}

```

Разница между одним большим общим эффектом и несколькими эффектами fork заключается в том, что all-эффект блокируется, поэтому код после общего эффекта выполняется, когда все дочерние саги завершены, в то время как fork-эффекты не блокируются, поэтому код после fork выполняется сразу после получения эффектов fork

3. Опишем нашу сагу watcherSaga

У нас есть 2 сущности ( в виде гномика (извините) ). 

- watcher (вотчер)
- Worker (воркер)

# Что делает вотчер?

Вотчер следит за экшенами, которые были вызваны. Если он находит такой экшн, который подходит ему по типу (в первом аргументе), то он вызывает своего воркера

# Что делает воркер?

Воркер уже делает какую-то работу (запросы, вызовы других экшенов и т.д.)

Под все саги (кроме rootSaga) создается отдельная папка, в которой будут все файлы с сагами

```
import { setDataValue, setError } from '../action.js'
import { call, put, takeEvery } from 'redux-saga/effects'

// put - это наш новый диспатч внутри саги. Он делает "тоже самое" что и обычный dispatch
// call - Вместо вызова асинхронного реквеста напрямую, 
// метод call вернет только объект описывающий эту операцию и redux-saga сможет позаботиться о вызове и возвращении результатов в функцию-генератор.

// Это воркер, который будет делать какие-то действия
function* someWorkerSaga(action) {
    console.log(action) // {type: "TRIGGER_SAGA", url: "https://api.themoviedb.org/3/search/movie?api_key=fcd1837ca404652c4523e6b6e90d3aab&query=girls&page=1&include_adult=true}
    try {
        const dataFromServer = yield call(() => 
            fetch(action.url)
                .then(items => items.json())
        )
        yield put(setDataValue(dataFromServer))
    } catch (error) {
        yield put(setError(error))
    }
}

// Это вотчер, которые смотрит на то, какие экшены были вызваны в проложении
// Этот вотчер проверят тип экшена (первый аргумент) и есть он совпал, то вызывает ту сагу, которая в 2-ом аргументе
export function* watcherSaga() {
    yield takeEvery('TRIGGER_SAGA', someWorkerSaga)
    // yield takeLatest('TRIGGER_SAGA', someWorkerSaga)
}



```

- takeEvery используется для запуска нового воркера при каждом отправленном TRIGGER_SAGA экшене
- takeLatest используется для запуска последнего вызванного экшена (допусти по нажати на кнопку у нас вызвалось много одинаковых экшенов, чтобы их все не обрабатывать мы возьмем последний)

# Как теперь вызвать нашу сагу?

```
import React from 'react';
import { useDispatch } from 'react-redux'

export const Test = () => {
    const apiQuery = 'https://api.themoviedb.org/3/search/movie?api_key=fcd1837ca404652c4523e6b6e90d3aab&query=girls&page=1&include_adult=true'
    
    const dispatcher = useDispatch(); 
    
    const trigger = () => {
        dispatcher({ type: 'TRIGGER_SAGA', url: apiQuery }) // надо вызвать экшен с тем типом, который в вотчере.
    }

    return (
        <>
            <button onClick={trigger}>Click</button>
        </>
    )
}

```
