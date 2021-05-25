# Тестирование 

userEvent вместе fireEvent

Использовать лучше юзер ивенты, они лучше описывают реальное поведение

userEvent - имитирует реальное поведение браузера более точно при каких-то событиях

Чтобы работать с userEvent их надо подключить

```
import userEvent from '@testing-library/user-event'; 

```

Давайте протестим чекбокс с userEvent

```
it('click on checkbox', () => {
        const { container } = render(<input type="checkbox" />)
        const checbox = container.firstChild;
        expect(checbox).not.toBeChecked();
        userEvent.click(checbox);
        expect(checbox).toBeChecked();
    })

```

Вроде не сложно

Давайте протестим инпут, в который мы что-то вводим

Есть вот такой инпут (controlled)

```
      <input onChange={onChange} value={data} data-testid="input-data" />
```

Давайте его протестим

```
it('onChange input', () => {
    const { getByTestId } = render(<App />);
    const input = getByTestId('input-data');
    expect(input.value).toBeFalsy();
    userEvent.type(input, 'I love tests')
    expect(input.value).toBe('I love tests');
})

```

Как сделать через screen?

```
it('onChange input with screen', () => {
    render(<App />);
    expect(screen.queryByDisplayValue('I love tests')).not.toBeInTheDocument();
    userEvent.type(screen.getByTestId('input-data'), 'I love tests')
    expect(screen.getByDisplayValue('I love tests')).toBeTruthy();
})

```

Давайте еще потестим чекбоксы)

Например, клик по чекбоксу с зажатым shift/ctrl

```
it('click on checkbox', () => {
        const { container } = render(<input type="checkbox" />)
        const checbox = container.firstChild;
        expect(checbox).not.toBeChecked();
        // в случае необходимисти можно добавить настройки для ивента
        // к примеру можно сказать, что мы нажимаем на чекбоекс с зажатым ctrl/shift и т.д.
        // или же в случае даблклика
        userEvent.click(checbox, { ctrlKey: true, shiftKey: true });
        expect(checbox).toBeChecked();
    })

```

Теперь тест на даблклик

```
it('dblClick on checkbox', () => {
    // добавим функцию onChange, которую навесим на checkbox
    const onChange = jest.fn();
    const { container } = render(<input type="checkbox" onChange={onChange} />)
    const checkbox = container.firstChild;
    expect(checkbox).not.toBeChecked();
    userEvent.dblClick(checkbox); // протестим балклик
    expect(onChange).toHaveBeenCalledTimes(2);
})

```

Давайте протестим перемещение по инпутам с помощью кнопки tab

```
it('focus', () => {
    const { getAllByTestId } = render(
        <div>
            <input data-testid='element' type="checkbox" />
            <input data-testid='element' type="radio" />
            <input data-testid='element' type="number" />
        </div>
    )
    const [checkbox, radio, number] = getAllByTestId('element');
    userEvent.tab(); // эмулируем нажатие на кнопку tab
    expect(checkbox).toHaveFocus();
    userEvent.tab(); // эмулируем нажатие на кнопку tab
    expect(radio).toHaveFocus();
    userEvent.tab(); // эмулируем нажатие на кнопку tab
    expect(number).toHaveFocus();
})

```

И давайте глянем на событие select

```
it('select option', () => {
    const { selectOptions, getByRole, getByText } = render(
        <select>
            <option value="1">A</option>
            <option value="2">B</option>
            <option value="3">C</option>
        </select>
    )

    userEvent.selectOptions(getByRole('combobox'), '1'); // значение, которое юзер выбирает
    expect(getByText('A').selected).toBeTruthy();
    userEvent.selectOptions(getByRole('combobox'), '2');
    expect(getByText('A').selected).toBeFalsy();
    expect(getByText('B').selected).toBeTruthy();
})

```

# Асинхронное тестирование запросов

Запрос будем делать сюда - 'http://hn.algolia.com/api/v1/search'

Есть такой компонент

```
import React, { useState } from 'react';
import axios from 'axios';

const baseURL = 'http://hn.algolia.com/api/v1/search';

function App() {
  const [data, setData] = useState([]);
  const [error, setError] = useState('');

  const handleData = async () => {
    try {
      const res = await axios.get(`${baseURL}?query=React`);
      setData(res.data.hits);
    } catch (error) {
      console.log('error: ', error);
      setError(error)
    }
  }

  const spawnData = () => {
    return data.map(({ objectID, url, title }) => {
      return (
        <li key={objectID}>
          <a href={url}>{title}</a>
        </li>
      )
    });
  }

  return (
    <div>
      <button onClick={handleData}>Fetch data</button>

      {error && <h2>We have errors</h2>}

      {spawnData()}
    </div>
  )
}

export default App;


```

Давайте его тестить

Сначала мы мокаем функции, через которые мы делаем запрос
Потом мы сами конструируем ответ от сервиса
И проверяем состояние нашего компонента или результат отрисовки

Начинаем с подмены данных

Нам надо сделать мок библиотеки axios


```
jest.mock('axios');

const hits = [ // описывает ответ от сервера
    {
        onjectID: '1', title: 'Angular'
    },
    {
        onjectID: '2', title: 'React'
    },
    {
        onjectID: '3', title: 'Vue'
    }
]

describe('App', () => {
    it('fetch news from an API', async () => {
        axios.get.mockImplementationOnce(() => Promise.resolve({ data: { hits } })); // переписываем реализацию метода get
        const { getByRole, findAllByRole } = render(<App />);
        userEvent.click(getByRole('button'));
        const items = await findAllByRole('listitem');
        expect(items).toHaveLength(3);// проверям, что в списке 3 элемента

        // Что еще можно протестить?
        // Давайте протестим вызов функции get
        expect(axios.get).toHaveBeenCalledTimes(1);
        // С какими данными был вызван запрос
        expect(axios.get).toHaveBeenCalledWith('http://hn.algolia.com/api/v1/search?query=React');
    })

    // Давайте протести негативный ответ

    it('fetch news from an API and reject', async () => {
        axios.get.mockImplementationOnce(() => Promise.reject(new Error())); // переписываем реализацию метода get
        const { getByRole, findByText } = render(<App />);
        userEvent.click(getByRole('button'));
        const message = await findByText(/We have errors/i);
        expect(message).toBeInTheDocument();
    })
})

```

Основные шаги:

1. Использовать jest для переопределения логики внешних модулей
2. Ждать данные

# Повторим паатеры

Что за паттерн?

class Person {
  constructor() {
    if (typeof Person.instance === 'object') {
      return Person.instance;
    }
    Person.instance = this;
    return this;
  }
}

Напишите мне фабирку и абстрактную фабрику)
