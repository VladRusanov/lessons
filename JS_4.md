# Глобальные объекты

В браузере это объект window

```
var foo = 42;
console.log(window.foo); // 42
 
const bar = 'top kek';
console.log(window.bar); // undefined
 
window.baz = 'JavaScript <3';
console.log(baz); // JavaScript <3

```

В Node.js тоже есть глобальный объект global

```
var foo = 42;
console.log(global.foo); // 42

```

# Объекты в JavaScript

- Нативные объекты
- Хост объекты
- Пользовательские объекты

# Нативные объекты

Нативными (native object) объектами в JS называют объекты, свойства и поведение которых описаны в спецификации языка JavaScript. 

Их наличие не зависит от того окружения, где запускается код.

```
 Object, Array, Date, Math
 
```
# Хост объекты

Хост (host object) объектами в JS называют объекты, которые предоставляются окружением (зависят от того, где работает код)

```
Например, для браузеров это будут document, location, history, XMLHttpRequest, setTimeout, setInterval

```

# Литерал объекта

Объявление через {...} называют литералом объекта или литеральной нотацией.

# 
