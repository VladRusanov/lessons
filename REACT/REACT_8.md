# Redux-Thunk

По умолчанию, экшены в Redux являются синхронными, что, является проблемой для приложения, которому нужно взаимодействовать с серверным API, или выполнять другие асинхронные действия. К счастью Redux предоставляет нам такую штуку как middleware, которая стоит между диспатчом экшена и редюсером. 

Существует две самые популярные middleware библиотеки для асинхронных экшенов в Redux, это — Redux Thunk и Redux Saga.

Сейчас смотрим на thunk

Redux Thunk это middleware библиотека, которая позволяет вам вызвать action creator, возвращая при этом функцию вместо объекта. 

Функция принимает метод dispatch как аргумент, чтобы после того, как асинхронная операция завершится, использовать его для диспатчинга обычного синхронного экшена, внутри тела функции.

# Установка и настройка

Во первых, добавьте redux-thunk пакет в ваш проект:

```
npm install redux-thunk

```

Затем, добавьте middleware, когда будете создавать store вашего приложения, с помощью applyMiddleware, предоставляемый Redux'ом:

```
// store.js
import { createStore, applyMiddleware  } from 'redux'
import { composeWithDevTools } from 'redux-devtools-extension'
import thunk from 'redux-thunk';

import rootReducer from './rootReducer.js'

const store = createStore(rootReducer, composeWithDevTools(applyMiddleware(thunk)))
export default store

```

# Основное использование

Обычно Redux-Thunk используют для асинхронных запросов к внешней API, для получения или сохранения данных. 

Redux-Thunk позволяет легко диспатчить экшены которые следуют «жизненному циклу» запроса к внешней API

Напишем наш первый thunk для запроса на сервер

```
// action.js

export const setLoading = (value) => {
    return {
        type: 'SET_LOADING',
        payload: value
    }
}

export const getDataSuccess = (data) => {
    return {
        type: 'GET_DATA_SUCCESS',
        payload: data
    }
}

export const getDataFailure = (error) => {
    return {
        type: 'GET_DATA_ERROR',
        payload: error
    }
}

export const myThunkRequest = (api) => (dispatch) => {
    dispatch(setLoading(true));

    const promise = fetch(api);
    promise
        .then(data => {
            return data.json()
        })
        .then(data => {
            dispatch(getDataSuccess(data))
        })
        .catch(error => {
            dispatch(getDataFailure(error))
        })
        .finally(() => {
            dispatch(setLoading(false))
        })
}

```

Редьюсер описывается точно так же, тут ничего нового


# Помимо dispatch-а наш мидлвар принимает еще и getState функцию

Функция, возвращаемая асинхронным action creator'ом с помощью Redux-Thunk, также принимает getState метод как второй аргумент, что позволяет получать стейт прямо внутри action creator'а:

```
export const myThunkRequest = (api) => (dispatch, getState) => {
    dispatch(setLoading(true));
    console.log(getState()); // наш стор
    const promise = fetch(api);
    promise
        .then(data => {
            return data.json()
        })
        .then(data => {
            dispatch(getDataSuccess(data))
        })
        .catch(error => {
            dispatch(getDataFailure(error))
        })
        .finally(() => {
            dispatch(setLoading(false))
        })

}

```

# HomeWork

Берем апи с фильмами, делаем запрос через санк и отображаем его все на страницу (как уже делали)
