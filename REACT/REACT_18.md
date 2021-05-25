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


