#React.cloneElement

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
