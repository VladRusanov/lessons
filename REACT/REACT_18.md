# Порталы

Порталы позволяют рендерить дочерние элементы в DOM-узел, который находится вне DOM-иерархии родительского компонента.

```
ReactDOM.createPortal(child, container)

```

Первый аргумент (child) — это любой React-компонент, который может быть отрендерен, такой как элемент, строка или фрагмент. Следующий аргумент (container) — это DOM-элемент.


```
render() {
  // React *не* создаёт новый div. Он рендерит дочерние элементы в `domNode`.
  // `domNode` — это любой валидный DOM-узел, находящийся в любом месте в DOM.
  return ReactDOM.createPortal(
    this.props.children,
    domNode
  );
}

```

```
// Это два соседних контейнера в DOM
const appRoot = document.getElementById('app-root');
const modalRoot = document.getElementById('modal-root');

class Modal extends React.Component {
  constructor(props) {
    super(props);
    this.el = document.createElement('div');
  }

  componentDidMount() {
    // Элемент портала добавляется в DOM-дерево после того, как
    // потомки компонента Modal будут смонтированы, это значит,
    // что потомки будут монтироваться на неприсоединённом DOM-узле.
    // Если дочерний компонент должен быть присоединён к DOM-дереву
    // сразу при подключении, например, для замеров DOM-узла,
    // или вызова в потомке 'autoFocus', добавьте в компонент Modal
    // состояние и рендерите потомков только тогда, когда
    // компонент Modal уже вставлен в DOM-дерево.
    modalRoot.appendChild(this.el);
  }

  componentWillUnmount() {
    modalRoot.removeChild(this.el);
  }

  render() {
    return ReactDOM.createPortal(
      this.props.children,
      this.el
    );
  }
}

class Parent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {clicks: 0};
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    // Эта функция будет вызвана при клике на кнопку в компоненте Child,
    // обновляя состояние компонента Parent, несмотря на то,
    // что кнопка не является прямым потомком в DOM.
    this.setState(state => ({
      clicks: state.clicks + 1
    }));
  }

  render() {
    return (
      <div onClick={this.handleClick}>
        <p>Количество кликов: {this.state.clicks}</p>
        <p>
          Откройте DevTools браузера,
          чтобы убедиться, что кнопка
          не является потомком блока div
          c обработчиком onClick.
        </p>
        <Modal>
          <Child />
        </Modal>
      </div>
    );
  }
}

function Child() {
  // Событие клика на этой кнопке будет всплывать вверх к родителю,
  // так как здесь не определён атрибут 'onClick' 
  return (
    <div className="modal">
      <button>Кликните</button>
    </div>
  );
}

ReactDOM.render(<Parent />, appRoot);

```


# Рендер-пропсы

Термин «рендер-проп» относится к возможности компонентов React разделять код между собой с помощью пропа, значение которого является функцией.

Компонент с рендер-пропом берёт функцию, которая возвращает React-элемент, и вызывает её вместо реализации собственного рендера.

```
<DataProvider render={data => (
  <h1>Привет, {data.target}</h1>
)}/>

```

```
class Cat extends React.Component {
  render() {
    const mouse = this.props.mouse;
    return (
      <img src="/cat.jpg" style={{ position: 'absolute', left: mouse.x, top: mouse.y }} />
    );
  }
}

class Mouse extends React.Component {
  constructor(props) {
    super(props);
    this.handleMouseMove = this.handleMouseMove.bind(this);
    this.state = { x: 0, y: 0 };
  }

  handleMouseMove(event) {
    this.setState({
      x: event.clientX,
      y: event.clientY
    });
  }

  render() {
    return (
      <div style={{ height: '100vh' }} onMouseMove={this.handleMouseMove}>

        {/*
          Вместо статического представления того, что рендерит <Mouse>,
          используем рендер-проп для динамического определения, что надо отрендерить.
        */}
        {this.props.render(this.state)}
      </div>
    );
  }
}

class MouseTracker extends React.Component {
  render() {
    return (
      <div>
        <h1>Перемещайте курсор мыши!</h1>
        <Mouse render={mouse => (
          <Cat mouse={mouse} />
        )}/>
      </div>
    );
  }
}

```

Иными словами, рендер-проп — функция, которая сообщает компоненту что необходимо рендерить.



Будьте осторожны при использовании рендер-проп вместе с React.PureComponent

Использование рендер-пропа может свести на нет преимущество, которое даёт React.PureComponent, если вы создаёте функцию внутри метода render. Это связано с тем, что поверхностное сравнение пропсов всегда будет возвращать false для новых пропсов и каждый render будет генерировать новое значение для рендер-пропа.

