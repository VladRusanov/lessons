# Formik

```
  npm install formik --save

```

# Что это?

Формик - либа для работы с формами

```
   <Formik>
    // Тут будет наша форма
   </Formik>
  
```

# Что принимает в себя <Formik>
  
- initialValues - объект с базовыми значениями для полей в форме

```
<Formik initialValues={{ email: '', password: '' }}>
    {(
      values, // все значения полей
    ) => (
      <form>
        <input name="email" value={values.email} />
        <input name="password" value={values.password} />
      </form>
    )}
</Formik>

```

- validate - функция, которая используется для валидации ваших полей

```
  <Formik
    validate={values => { // values - текущее значение ваших полей
        const errors = {};
        if (!values.email) {
            errors.email = 'Required';
        } else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$/i.test(values.email)) {
            errors.email = 'Invalid email address';
        }
        return errors;
    }}
  >
    {({ errors }) => ( // errors - объект с ошибками в формате { 'имя_поля': 'ошибка' }
      <form>
        <input name="email" value={values.email} />
        {errors && errors.email}
      </form>
    )}
  </Formik>

```

- onSubmit - функция на сабмит формы

```
<Formik
  onSubmit={(values, { setSubmitting }) => { // setSubmitting - если передаем true, то значит, что мы нажали на сабмит и идет загрузка чего-то ( мы с вами так делали в запросах в thunk )
      setTimeout(() => {
          alert(JSON.stringify(values, null, 2));
          setSubmitting(false); // Говорим, что загрузка закончена
      }, 400);
  }}
>
    {(
      values, // все значения полей
      isSubmitting, // Флаг для дизейбла чего-то. setSubmitting(false) -> isSubmitting тоже false, setSubmitting(true) -> isSubmitting тоже true
    ) => (
      <form>
        <input name="email" value={values.email} />
        <input name="password" value={values.password} />
        <button type="submit" disabled={isSubmitting}>
            Submit
        </button>
      </form>
    )}
</Formik>

```

Мы поговори про основные пропсы, которые можно передать в формик, но их еще много (почитать можно тут https://formik.org/)

# Аргументы коллбэка (который в формике, который рендерит саму форму)

```
<Formik
 ...
>
  {() => ( // это оно
  
  )}
</Formik>

```

Что этот коллбэк принимает?

- values, // все значения полей

- errors, // объект с ошибками в формате { 'имя_поля': 'ошибка' }

- touched, // объект, в котором отображаются поля, которые мы поменяли

- handleChange, // onChange-> handleChange, onBlur-> handleBlurи так далее

-handleBlur,

-handleSubmit,

- isSubmitting // флаг для дизейбла чего-то во время того, как обрабатывается запрос. Выше про него писал

# Пример формы для signIn

```
<Formik
    initialValues={{ email: '', password: '' }}
    validate={values => {
        const errors = {};
        if (!values.email) {
            errors.email = 'Required';
        } else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$/i.test(values.email)) {
            errors.email = 'Invalid email address';
        }
        return errors;
    }}
    onSubmit={(values, { setSubmitting }) => {
        setTimeout(() => {
            alert(JSON.stringify(values, null, 2));
            setSubmitting(false);
        }, 400);
    }}
>
    {({
        values, // все значения полей
        errors, // ошибки 
        touched, // объект, в котором отображаются поля, которые мы поменяли
        handleChange, // onChange-> handleChange, onBlur-> handleBlurи так далее
        handleBlur,
        handleSubmit,
        isSubmitting,
        /* and other goodies */
    }) => {
        return (
            <form onSubmit={handleSubmit}>
                <input
                    type="email"
                    name="email"
                    onChange={handleChange}
                    onBlur={handleBlur}
                    value={values.email}
                />
                {errors.email && touched.email && errors.email}
                <input
                    type="password"
                    name="password"
                    onChange={handleChange}
                    onBlur={handleBlur}
                    value={values.password}
                />
                {errors.password && touched.password && errors.password}
                <button type="submit" disabled={isSubmitting}>
                    Submit
                </button>
            </form>
        )
    }}
</Formik>

```

# Еще вариант написания

Formik поставляется с несколькими дополнительными компонентами, чтобы сделать жизнь проще и код меньше: <Form />, <Field />, и <ErrorMessage />

```
import { Formik, Form, Field, ErrorMessage } from 'formik';

return (
  <Formik
     initialValues={{ email: '', password: '' }}
     validate={values => {
       const errors = {};
       if (!values.email) {
         errors.email = 'Required';
       } else if (
         !/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$/i.test(values.email)
       ) {
         errors.email = 'Invalid email address';
       }
       return errors;
     }}
     onSubmit={(values, { setSubmitting }) => {
       setTimeout(() => {
         alert(JSON.stringify(values, null, 2));
         setSubmitting(false);
       }, 400);
     }}
   >
     {({ isSubmitting }) => (
       <Form> // уже не просто <form>, в собственній компонент Формика
         <Field type="email" name="email" />
         <ErrorMessage name="email" component="div" />
         <Field type="password" name="password" />
         <ErrorMessage name="password" component="div" />
         <button type="submit" disabled={isSubmitting}>
           Submit
         </button>
       </Form>
     )}
  </Formik> 
)

```


