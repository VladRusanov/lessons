# PropTypes and Default Prop

# Проверка типов с помощью PropTypes
с
С версии React 15.5 React.PropTypes были вынесены в отдельный пакет. Так что используйте библиотеку prop-types.

...Пока глянем без либы

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

