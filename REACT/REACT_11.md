# Практика + GraphQL

```
npm i apollo-boost
npm i apollo-link-context
npm i graphql
npm i graphql-tag

```

https://github.com/VladRusanov/Module_Example


# Фрагменты

Фрагмент GraphQL - это часть логики, которая может использоваться несколькими запросами и изменениями.

```
fragment NameParts on Person {
  firstName
  lastName
}

query GetPerson {
  people(id: "7") {
    ...NameParts
    avatar(size: LARGE)
  }
}


```

Тоже самое, что и просто

```
query GetPerson {
  people(id: "7") {
    firstName
    lastName
    avatar(size: LARGE)
  }
}

```


Как их использовать?

```
import gql from 'graphql-tag';

export const USER_FRAGMENT = gql`
  fragment UserData on User { // User - тип того, что запрос возвращает (можно посмотреть на сервере)
    login,
    nick
  }
`;

export const SIGN_UP_MUTATION = gql`
  mutation SignUp($login: String, $nick: String, $password: String) {
    UserUpsert(user: { login: $login, nick: $nick, password: $password }) {
      ...UserData
    }
  }
  ${USER_FRAGMENT}
`;

```
