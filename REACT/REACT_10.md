# GraphQL

```
npm install apollo-boost @apollo/react-hooks graphql

```

http://doc.a-level.com.ua/frontend-graphql-backs

Дока -  наше все (https://www.apollographql.com/docs/)

1. Даем доступ к клиенту graphQL

```
import React from 'react';
import { Test } from './components/Test.jsx';
import { Provider } from 'react-redux'
import store from './store/store.js';
import { ApolloProvider } from '@apollo/react-hooks';
import ApolloClient from 'apollo-boost'

function App() {
  const clientParam = { uri: 'http://shop-roles.asmer.fs.a-level.com.ua/graphql' };
  const client = new ApolloClient(clientParam);
  return (
    <ApolloProvider client={client}>
      <Provider store={store}>
        <Test />
      </Provider>
    </ApolloProvider>
  );
}

export default App;


```

```
import React from 'react'
import { useDispatch } from 'react-redux'
import { useQuery, useMutation, useLazyQuery } from '@apollo/react-hooks';
import { gql } from 'apollo-boost';

export const Test = () => {
    const dispatcher = useDispatch();
    const query = gql`
        query Login($name: String!, $password: String!) {
            login(login: $name, password: $password)  
        } 
    `;
    // refetch - метод для повторного запроса
    // const { data, refetch } = useQuery(query, { // Вызывается сразу же
    //     variables: { name: 'VLAD', password: 'VLAD' },
    //     notifyOnNetworkStatusChange: true
    // });

    // LoginRequest - метод запроса
    const [LoginRequest, { data, status }] = useLazyQuery(query); // Сразу не вызывается, можно будет вызвать когда угодно

    const newRequest = () => {
        // refetch({ variables: { name: 'asd', password: 'asd' } })
        LoginRequest({ variables: { name: 'VLAD', password: 'VLAD' } });
    }
    console.log('data: ', data);
    return (
        <>
            <button onClick={newRequest}>Get Data</button>
        </>
    )
}

```
