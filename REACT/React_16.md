# Тестирование

Пирамида тестирования

![image](https://user-images.githubusercontent.com/16369478/119111084-683b2a00-ba2b-11eb-907d-134a58951eb8.png)

# Не нужно писать тесты, если:

- Вы делаете простой сайт-визитку из 5 статических html-страниц и с одной формой отправки письма. На этом заказчик, скорее всего, успокоится, ничего большего ему не нужно. Здесь нет никакой особенной логики, быстрее просто все проверить «руками»

- Вы занимаетесь рекламным сайтом/простыми флеш-играми или баннерами – сложная верстка/анимация или большой объем статики. Никакой логики нет, только представление

- Вы всегда пишете код без ошибок, обладаете идеальной памятью и даром предвидения. Ваш код настолько крут, что изменяет себя сам, вслед за требованиями клиента. Иногда код объясняет клиенту, что его требования — гов не нужно реализовывать

Unit — модульные тесты, применяемые в различных слоях приложения, тестирующие наименьшую делимую логику приложения: например, класс, но чаще всего — метод. Эти тесты обычно стараются по максимуму изолировать от внешней логики, то есть создать иллюзию того, что остальная часть приложения работает в стандартном режиме.

# Ключевые понятия юнит-тестирования

Покрытие тестов (Code Coverage) — одна из главных оценок качества тестирования приложения. Это процент кода, который был покрыт тестами (0-100%).

Для оценки покрытия тестами обычно используют дополнительные инструменты: JaCoCo, Cobertura, Clover, Emma и т.д.

# TDD

TDD (Test-driven development) — разработка через тестирование. В рамках этого подхода в первую очередь пишется тест, который будет проверять определенный код. Получается тестирование чёрного ящика: мы знаем, что есть на входе и знаем, что должно получиться на выходе.

Разработка через тестирование начинается с проектирования и разработки тестов для каждой небольшой функциональности приложения

![image](https://user-images.githubusercontent.com/16369478/119111674-f44d5180-ba2b-11eb-864f-c71280d20cb1.png)

Подход состоит из таких составляющих:

- Пишем наш тест.

- Запускаем тест, прошел он или нет (видим, что всё красное — не психуем: так и должно быть).

- Добавляем код, который должен удовлетворить данный тест (запускаем тест).

- Выполняем рефакторинг кода.

# BDD

BDD (Behavior-driven development) — разработка через поведение. Это подход основан на TDD. Если говорить точнее, он использует написанные понятным языком примеры (как правило на английском), которые иллюстрируют поведение системы для всех, кто участвует в разработке.

Не будем углубляться в данный термин, так как он в основном затрагивает тестировщиков и бизнес-аналитиков.

# Тестовый сценарий (Test Case)

Тестовый сценарий (Test Case) — сценарий, описывающий шаги, конкретные условия и параметры, необходимые для проверки реализации тестируемого кода. 

# Фикстуры (Fixture)

Фикстуры (Fixture) — состояние среды тестирования, которое необходимо для успешного выполнения испытуемого метода. Это заранее заданный набор объектов и их поведения в используемых условиях.

# Этапы тестирования

- Задание тестируемых данных (фикстур).

- Использование тестируемого кода (вызов тестируемого метода).

- Проверка результатов и сверка с ожидаемыми.

![image](https://user-images.githubusercontent.com/16369478/119112101-69208b80-ba2c-11eb-81a3-2db195cb0296.png)

Чтобы обеспечить модульность теста, нужно нужно изолироваться от других слоев приложения. Сделать это можно помощью заглушек, моков

# Мок

Мок (Mock) — объекты, которые настраиваются (например, специфично для каждого теста) и позволяют задать ожидания вызовы методов в виде ответов, которые мы планируем получить. Проверки соответствия ожиданиям проводятся через вызовы к Mock-объектам.

# Stub

Заглушки (Stub) — обеспечивают жестко зашитый ответ на вызовы во время тестирования.

# Давайте напишем свои тесты

Либа для тестов уже сразу после react-create-app

Надо еще установить плагин для vs code - Jest
![image](https://user-images.githubusercontent.com/16369478/119115637-0fba5b80-ba30-11eb-98ce-9c6f380e10a8.png)


1. Создадим путой реакт проект

2. После этого у вас будет проект с файлом App.test.js

Все файлы с тестами называеются FILE_NAME.test.js

В этой файле и будет описываться тест для нашего приложения (вообще файлов с тестами может быть много)

3. Давайте откроет этот файл

```
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders learn react link', () => { // название теста
  render(<App />);
  const linkElement = screen.getByText(/learn react/i);
  expect(linkElement).toBeInTheDocument();
});


```

Давайте теперь запустим наш тест

```
В терминале

npm test

```

![image](https://user-images.githubusercontent.com/16369478/119113753-16e06a00-ba2e-11eb-8969-820bfb02c259.png)

Видим, что наш тест работает

![image](https://user-images.githubusercontent.com/16369478/119115190-93277d00-ba2f-11eb-8da9-2e49ad8d4665.png)

Если тест валидный, то в vs code видим галку

![image](https://user-images.githubusercontent.com/16369478/119115773-35dffb80-ba30-11eb-967b-850c87630132.png)

Если есть какая-то проблема, то крестик

![image](https://user-images.githubusercontent.com/16369478/119115820-43958100-ba30-11eb-991a-52a5936da2d9.png)

Это работает через плагин Jest

# Что такое снепшот?

Это "снимок" нашего дом-дерева

# Чуть чуть перепишем дефолтный тест

```
test('renders learn react link', () => { // название теста
  const { getByText } = render(<App />);
  const linkElement = getByText(/learn react/i);
  expect(linkElement).toBeInTheDocument();
});

```

render() - рендерит нам компонент. Он возвращает нам возвожность как-то получить доступ к элементам на странице

В примере - мы получили метод getByText, с помощью которого мы можем найти что-то в доме по тексту

```
getByText(/learn react/i); // ищем строку learn react

```

После этого мы вызываем функцию "утверждения"

```
expect(linkElement).toBeInTheDocument();

```

Говорим, что linkElement должен быть в документе. Если это так, то тест отработает, если его там нет, то тест не отработает

Давайте еще чуть чуть подэбажим

чуть чуть поменяет тест

```
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders learn react link', () => { // название теста
  render(<App />)
  screen.debug(); // при запуске теста увидим состояние нашего дома
});

```

И запустим тест

![image](https://user-images.githubusercontent.com/16369478/119118445-dd5e2d80-ba32-11eb-9296-f713ed2a7686.png)

# Идем дальше

У нас есть вот такое приложение

![image](https://user-images.githubusercontent.com/16369478/119139037-3fc32800-ba4b-11eb-8f23-9219cda7619d.png)


![image](https://user-images.githubusercontent.com/16369478/119139054-45207280-ba4b-11eb-871d-226c8ba03729.png)

Есть инпут, в который мы можем что-то ввести.

То что ввели отображается под инпутом.

Давайте протестим этот компонент

Сам компонент

```
import React, { useState } from 'react';

const Search = ({ value, onChange, children }) => (
  <div>
    <label htmlFor="search">{children}</label>
    <input id="search" type="text" value={value} onChange={onChange} />
  </div>
)

function App() {
  const [search, setSearch] = useState('');

  const handleChange = ({ target }) => {
    setSearch(target.value)
  }

  return (
    <div>
      <Search value={search} onChange={handleChange}>Search:</Search>
      <p>Searches for {search ? search : '...'}</p>
    </div>
  );
}

export default App;

```

Теперь давайте напишем тест для этого всего

Тут мы проверим отрендерился ли конпонент вообще

```
import { render, screen } from '@testing-library/react';
import App from './App';

describe('App', () => { // Описывает с каким компонентом мы работаем
  it('render App component', () => { // Название теста
    render(<App />); // рендерим наш компонент
    screen.debug();
    // флаг i - Нечувствительность к регистру
    expect(screen.getByText(/Search:/i)).toBeInTheDocument(); // ищем текст на странице
  })
})

```


Искать что-то можно не только по тексту

Можно искать по роли элемента

Роли элементов можно посмотреть в доке react-testing lib

К примеру:

Роль input - textbox и т.д.

Давайте допишем нащ тест

```
import { render, screen } from '@testing-library/react';
import App from './App';

describe('App', () => { // Описывает с каким компонентом мы работаем
  it('render App component', () => { // Название теста
    render(<App />);
    expect(screen.getByText(/Search:/i)).toBeInTheDocument();
    expect(screen.getByRole('textbox')).toBeInTheDocument(); // Поиск по роли
  })
})


```

Есть еще 4 метода для поиска

- getByLabelText - ищет элемент по тексту в лейбле

- getByPlaceholderText - ищет элемент по тексту в поейсхолдере

- getByAltText - botn 'ktvtyns gj fknthyfnbdyjve ntrcne

- getByDisplayValue - поиск элемента по отображаемому значению или по атрибуту value


Пример:

```
import React, { useState } from 'react';

const Search = ({ value, onChange, children }) => (
  <div>
    <label htmlFor="search">{children}</label>
    <input
      id="search"
      placeholder="search text..."
      type="text"
      value={value}
      onChange={onChange}
    />
  </div>
)

function App() {
  const [search, setSearch] = useState('');

  const handleChange = ({ target }) => {
    setSearch(target.value)
  }

  return (
    <div>
      <img alt="search image" src="" />
      <Search value={search} onChange={handleChange}>Search:</Search>
      <p>Searches for {search ? search : '...'}</p>
    </div>
  );
}

export default App;


```

Тесты:

```
import { render, screen } from '@testing-library/react';
import App from './App';

describe('App', () => { // Описывает с каким компонентом мы работаем
  it('render App component', () => { // Название теста
    render(<App />);
    expect(screen.getByText(/Search:/i)).toBeInTheDocument();
    expect(screen.getByRole('textbox')).toBeInTheDocument();
    expect(screen.getByLabelText(/Search:/i)).toBeInTheDocument();
    expect(screen.getByPlaceholderText('search text...')).toBeInTheDocument();
    expect(screen.getByAltText('search image')).toBeInTheDocument();
    // используется для дефолтных элементов в форме
    expect(screen.getByDisplayValue('123')).toBeInTheDocument();
  })
})


```

