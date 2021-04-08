# React.cloneElement

```
React.cloneElement(
  element,
  [props],
  [...children]
)

```

Клонирует и возвращает новый React элемент, используя элемент в качестве отправной точки. 

Полученный элемент будет иметь пропсы исходного элемента, а новые пропсы будут поверхностно слиты воедино. 

Новые дочерние элементы заменят существующие. key и ref из исходного элемента будут сохранены


Этот метод можно удобно использовать вместе с props.children

```
....

const spawn = () => {
        return props.children.map((item, i) => (
            <React.Fragment key={i}>{
                React.cloneElement(item, { //Тут пропсы для компонентов })
            }</React.Fragment>
        ))
    }

....

```

# Практика

Давайте сделаем форму с валидацией

(Желательно повторять за мной)



# Компоненты высшего порядка (Higher-Order Component, HOC)

Компонент высшего порядка (Higher-Order Component, HOC) — это один из продвинутых способов для повторного использования логики. 

HOC не являются частью API React, это react паттерн

**Говоря просто, компонент высшего порядка — это функция, которая принимает компонент и возвращает новый компонент.**

```
function composeComponent(Component) {
    return class extends React.Component {
        render() {
            return <Component />
        }
    }
}

```

В данном примере функция composeComponent принимает аргумент Component и возвращает компонент ES6 class. 

Возвращенный класс отображает аргумент Component. 

Аргумент Component является компонентом React, который будет отображаться возвращенным компонентом class.

# Например:

```
class CatComponent {
    render() {
        return <div>Cat Component</div>
    }
}

```

CatComponent отображает следующее:

```
Cat Component

```

CatComponet может быть передан функции composeComponent для получения другого компонента:

```
const ComposedCatComponent = composeComponent(CatComponent)


```

ComposedCatComponent может быть отображен:


```

<ComposedCatComponent />

```

Результат выглядит следующим образом:

```
Cat Component

```

Та же функция высшего порядка, как и в JS.

Пример:

Создадим компонент, который будет сразу кидать данные

```
import React from 'react';

export const withData = (Component) => {
    return class extends React.Component {
        state = {
            data: [1,2,3,4]
        }

        fetchData = () => {
            // тут типо запрос на сервер
            // данные, которые мы получим - сразу кинем в компонент
        }

        render() {
            return <Component data={this.state.data}/>
        }
    }
}

```

```
import React from 'react';

class Films extends React.Component {
  render() {
    return (
      <>...</>
    )
  }
}

export default withData(Films) // используем нас HOC

```

# React.PureComponent

При разработке реакт-приложений обычно используют два типа компонентов:

1) React.Component

3) React.PureComponent

Давайте разберемся, зачем нужен каждый из типов компонентов.

Как мы уже знаем, в react lyfecycle компонентов существуют два процесса: mounting и updating.


Mounting lifecycle выглядит так:

1) constructor

2) static getDerivedStateFromProps (componentWillMount — deprecated)

3) render

4) componentDidMount


Updating lifecycle:

1) static getDerivedStateFromProps (componentWillReceiveProps - deprecated)

2) shouldComponentUpdate

3) (componentWillUpdate — deprecated)

4) render

5) getSnapshotBeforeUpdate

5) componentDidUpdate


Разница между Component и PureComponent заключается в методе updating lifecycle: shouldComponentUpdate.

В Component этот метод выглядит так:

```
shouldComponentUpdate(.....) {
 return true;
}

```

В PureComponent:

```
shouldComponentUpdate(nextProps, nextState) {
 return !shallowEqual(nextProps, this.props) || !shallowEqual(nextState, this.state);
}

```

PureComponent изначально определяет функцию, которая ответствена за принятие решения — нужно ли продолжать updating lifecycle или нет.

# Разберем пример:

```
lass A extends React.Component {
  render() {
    return (<B props={this.props.someProps}/>  
  }
}
class B extends React.PureComponent {
  render() {
    return (/* some data */)
  }
}

```

Вызывающий код

```
1: ReactDOM.render(<A someProps={1} />, root);
2: ReactDOM.render(<A someProps={1} />, root);

```

1) Здесь, на второй строке, компонент A будет обновлен. Так как он не является чистым компонентом, и его shouldComponentUpdate вернет true;

2) PureComponent B получает задачу на ререндер, но его props не изменились;

3) shouldComponentUpdate класса B вернет false, так как ссылки на все поля props остались прежними. (т.е. this.props.someProps === nextProps.someProps);

4) Дальнейшие методы Updating lifecycle не вызываются.


Теперь попробуем обновить someProps:

```
1: ReactDOM.render(<A someProps={1} />, root);
2: ReactDOM.render(<A someProps={2} />, root);

```

1) A ререндерится

2) PureComponent B получает задачу на ререндер, его someProps изменился

3) shouldComponentUpdate класса B вернет True (this.props.someProps !== nextProps.someProps);

4) Методы Updating lifecycle вызываются, компонент обновляется.

Если бы компонент B был объявлен не как PureComponent, а как обычный Component, то он бы обновился в обоих случаях.

Таким образом, PureComponent помогает нам сократить число операций и оптимизировать наш рендер “из коробки”.

Пара заметок:

1) Не нужно переопределять shouldComponentUpdate, если используете PureComponent.

2) Не нужно делать полное сравнение внутри shouldComponentUpdate следующим образом:

```
shouldComponentUpdate(nextProps, nextState) {
 return !JSON.stringify(nextProps) === !JSON.stringify(this.props) || !JSON.stringify(nextState) === !JSON.stringify(this.state);
}

```

# Оптимизация React.lazy

Функция React.lazy позволяет рендерить динамический импорт как обычный компонент.

До:

```
import OtherComponent from './OtherComponent';

```

После:

```
const OtherComponent = React.lazy(() => import('./OtherComponent'));

```

Она автоматически загрузит бандл, содержащий OtherComponent, когда этот компонент будет впервые отрендерен.

React.lazy принимает функцию, которая должна вызвать динамический import(). 

Результатом возвращённого Promise является модуль, который экспортирует по умолчанию React-компонент (export default).


# Задержка

Компонент с ленивой загрузкой должен рендериться внутри компонента Suspense, который позволяет нам показать запасное содержимое (например, индикатор загрузки) пока происходит загрузка ленивого компонента.

```
import React, { Suspense } from 'react';

const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Загрузка...</div>}>
        <OtherComponent />
      </Suspense>
    </div>
  );
}

```

Проп fallback принимает любой React-элемент, который вы хотите показать, пока происходит загрузка компонента. 

Компонент Suspense можно разместить в любом месте над ленивым компонентом. 

Кроме того, можно обернуть несколько ленивых компонентов одним компонентом Suspense.

```
import React, { Suspense } from 'react';

const OtherComponent = React.lazy(() => import('./OtherComponent'));
const AnotherComponent = React.lazy(() => import('./AnotherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Загрузка...</div>}>
        <section>
          <OtherComponent />
          <AnotherComponent />
        </section>
      </Suspense>
    </div>
  );
}

```


