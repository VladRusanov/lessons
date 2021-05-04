# Повторяем графКл

Просто graphQL без apollo

```
export const getGQL = async (headers = {}, query = "", variables = {}) => {
  try {
    const result = await fetch("http://chat.fs.a-level.com.ua/graphql", {
      method: "POST",
      headers: {
        Accept: "application/json",
        "Content-Type": "application/json",
        ...headers,
      },
      body: JSON.stringify({ query, variables }),
    }).then((res) => res.json());

    return result;
  } catch (error) {
    return new Error("getGQL Error");
  }
};

```

Как использовать?

Теперь на не надо писать эти useMutations и т.д.

Используем нашу функцию (getGQL) просто как обычный fetch

```
export function testAction(testData) {
  return {
    status: 'ACTION_STATUS',
    payload: getGQL(
      {
        Authorization: "Bearer " + localStorage.authToken,
      },
      `mutation test{
             test(test: testData)
      }`
    }
  );
}

```

Мы создали обычный ation, который можно будет связать с редаксов и т.д.

# Пример с сагой:

```
unction* everySearch({ query }) {
  const backQuery = `/${query}/`;

  const userToken = yield select((state) => state.auth.jwt); // через select (эффект) мы достаем данные из стора
  const users = yield getGQL(
    {
      Authorization: "Bearer " + userToken,
    },
    `query User($query: String){
          UserFind(query: $query){
            _id
            login
            nick
            avatar {
              _id
              url
            }
          }
        }`,
  );
  yield put(actionSearchResult(users.data.UserFind));
  console.log("search query finish", query);
}

```

# PropTypes and Default Prop

# Проверка типов с помощью PropTypes

С версии React 15.5 React.PropTypes были вынесены в отдельный пакет. Так что используйте библиотеку prop-types.

PropTypes - модуль для установки типов реакт пропсов без typescript

```
import PropTypes from 'prop-types';

class Greeting extends React.Component {
  render() {
    return (
      <h1>Привет, {this.props.name}</h1>
    );
  }
}

Greeting.propTypes = {
  name: PropTypes.string
};

```

В данном примере проверка типа показана на классовом компоненте, но она же может быть применена и к функциональным компонентам

Пример использования возможных валидаторов:

```
import PropTypes from 'prop-types';

MyComponent.propTypes = {
  // Можно объявить проп на соответствие определённому JS-типу.
  // По умолчанию это не обязательно.
  optionalArray: PropTypes.array,
  optionalBool: PropTypes.bool,
  optionalFunc: PropTypes.func,
  optionalNumber: PropTypes.number,
  optionalObject: PropTypes.object,
  optionalString: PropTypes.string,
  optionalSymbol: PropTypes.symbol,

  // Все, что может быть отрендерено:
  // числа, строки, элементы или массивы
  // (или фрагменты) содержащие эти типы
  optionalNode: PropTypes.node,

  // React-элемент
  optionalElement: PropTypes.element,

  // Тип React-элемент (например, MyComponent).
  optionalElementType: PropTypes.elementType,
  
  // Можно указать, что проп должен быть экземпляром класса
  // Для этого используется JS-оператор instanceof.
  optionalMessage: PropTypes.instanceOf(Message),

  // Вы можете задать ограничение конкретными значениями
  // при помощи перечисления
  optionalEnum: PropTypes.oneOf(['News', 'Photos']),

  // Объект, одного из нескольких типов
  optionalUnion: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number,
    PropTypes.instanceOf(Message)
  ]),

  // Массив объектов конкретного типа
  optionalArrayOf: PropTypes.arrayOf(PropTypes.number),

  // Объект со свойствами конкретного типа
  optionalObjectOf: PropTypes.objectOf(PropTypes.number),

  // Объект с определённой структурой
  optionalObjectWithShape: PropTypes.shape({
    color: PropTypes.string,
    fontSize: PropTypes.number
  }),
  
  // При наличии необъявленных свойств в объекте будут вызваны предупреждения
  optionalObjectWithStrictShape: PropTypes.exact({
    name: PropTypes.string,
    quantity: PropTypes.number
  }),   

  // Можно добавить`isRequired` к любому приведённому выше типу,
  // чтобы показывать предупреждение,
  // если проп не передан
  requiredFunc: PropTypes.func.isRequired,

  // Обязательное значение любого типа
  requiredAny: PropTypes.any.isRequired,

  // Можно добавить собственный валидатор.
  // Он должен возвращать объект `Error` при ошибке валидации.
  // Не используйте `console.warn` или `throw` 
  // - это не будет работать внутри `oneOfType`
  customProp: function(props, propName, componentName) {
    if (!/matchme/.test(props[propName])) {
      return new Error(
        'Проп `' + propName + '` компонента' +
        ' `' + componentName + '` имеет неправильное значение'
      );
    }
  },

  // Можно задать свой валидатор для `arrayOf` и `objectOf`.
  // Он должен возвращать объект Error при ошибке валидации.
  // Валидатор будет вызван для каждого элемента в массиве
  // или для каждого свойства объекта.
  // Первые два параметра валидатора 
  // - это массив или объект и ключ текущего элемента
  customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
    if (!/matchme/.test(propValue[key])) {
      return new Error(
        'Проп `' + propFullName + '` компонента' +
        ' `' + componentName + '` имеет неправильное значение'
      );
    }
  })
};

```

# Ограничение на один дочерний компонент

С помощью PropTypes.element вы можете указать, что в качестве дочернего может быть передан только один элемент.

```
import PropTypes from 'prop-types';

class MyComponent extends React.Component {
  render() {
    // Это должен быть ровно один элемент.
    // Иначе вы увидите предупреждение
    const children = this.props.children;
    return (
      <div>
        {children}
      </div>
    );
  }
}

MyComponent.propTypes = {
  children: PropTypes.element.isRequired
};

```

# Значения пропсов по умолчанию defaultProps

Вы можете задать значения по умолчанию для ваших props с помощью специального свойства defaultProps:

```
class Greeting extends React.Component {
  render() {
    return (
      <h1>Привет, {this.props.name}</h1>
    );
  }
}

// Задание значений по умолчанию для пропсов:
Greeting.defaultProps = {
  name: 'Незнакомец'
};

// Отрендерит "Привет, Незнакомец":
ReactDOM.render(
  <Greeting />,
  document.getElementById('example')
);

```

можете объявить defaultProps как статическое свойство класса (для компонента-наследника от React.Component). 

**Этот синтаксис ещё не утверждён, так что для его работы в браузере нужна компиляция**

```
class Greeting extends React.Component {
  static defaultProps = {
    name: 'незнакомец'
  }

  render() {
    return (
      <div>Привет, {this.props.name}</div>
    )
  }
}

```

# Функциональные компоненты

К функциональным компонентам можно также применять PropTypes.


```
export default function HelloWorldComponent({ name }) {
  return (
    <div>Hello, {name}</div>
  )
}

```

Для добавления PropTypes нужно объявить компонент в отдельной функции, которую затем экспортировать:

```
function HelloWorldComponent({ name }) {
  return (
    <div>Hello, {name}</div>
  )
}

export default HelloWorldComponent

```
