# Configurar toda estrutura de uma aplicação REACTJS

- Seguir o arquivo _PrimeiroProjetoReact.md_

# Criando Rotas

1. Criar a pasta _pages_ dentro SRC e fazer o snipet **rfc** da rocketseat dentro do arquivo index.js das pages.

2. Configurar o arquivo _routes.js_

```js
import React from "react";
import { Switch, Route } from "react-router-dom";

import Home from "./pages/Home";
import Cart from "./pages/Cart";

export default function Routes() {
  return (
    <Switch>
      <Route path="/" exact component={Home} />
      <Route path="/cart" component={Cart} />
    </Switch>
  );
}
```

3. Configurar o arquivo App.js

```js
import React from "react";
import { BrowserRouter } from "react-router-dom";

import Routes from "./routes";

function App() {
  return (
    <BrowserRouter>
      <Routes />
    </BrowserRouter>
  );
}

export default App;
```

# Estilos Globais

1. Criar uma pasta _styles_ dentro de SRC
2. Dentro de styles, criar um arquivo _global.js_
3. Instalar a lib de styled-components

```
yarn add styled-components
```

4. Configurar o arquivo global.js

```js
import { createGlobalStyle } from "styled-components";

export default createGlobalStyle`
  * {
    margin: 0;
    padding: 0;
    outline: 0;
    box-sizing: border-box;
  }

  body {
    background: #191919;
    -webkit-font-smoothing: antialiased;
  }

  body, input, button {
    font: 14px sans-serif;
  }

  #root {
    max-width: 1020px;
    margin: 0 auto;
    padding: 0 20px 50px;
  }

  button {
    cursor: pointer;
  }
`;
```

5. Importar a fonte ROBOTO e colocar o @import dentro do global.js [download](https://fonts.google.com/specimen/Roboto?selection.family=Roboto)

6. Criar uma pasta _assets/images_ dentro de SRC, onde ficará as imagens

# Criando Header da Aplicação

1. Criar a pasta _components_ dentro da pasta SRC
2. Criar pasta _Header_ dentro de components
3. Criar os arquivos index e styles dentro de _Header_
4. No arquivo _styles.js_ usar o snipt da **styled-react** para fazer a estrutura padrão

5. Importar o Header dentro do _App.js_
6. Instalar a lib de icones

```
yarn add react-icons
```

7. Biblioteca _POLISHED_ lida com cores.

```
yarn add polished
```

- Quando usamos:

```
background: ${darken(0.03, '#7159c1')};
```

- Falamos o quanto queremos escurecer e no segundo param, a cor que queremos escurecer.

# Configurando API

- API JSON SERVER - Uma API fake enquanto nós desenvolvemos e não temos a API real.

```
yarn global add json-server
```

- Para rodar essa API, precisamos configurar o _axios_ e executar o comando abaixo:

```
json-server server.json -p 3333 -w
```

1. server.json -> o nome do arquivo que contém as informações.

2. -p -> Define a porta que será executada(a mesma que foi configurada na baseURL no axios)

3. -w -> Fica lendo e toda vez que tiver uma alteração no arquivo json, ele atualiza automaticamente.

# Buscando produtos da API

- Função nativa do JS INTL - É um classe para fazer internacionalização

1. Criar uma pasta _util_ dentro de SRC
2. Criar um arquivo _format.js_

```js
export const { format: formatPrice } = new Intl.NumberFormat("pt-BR", {
  style: "currency",
  currency: "BRL"
});
```

- format: formatPrice -> Renomeando o nome do retorno para formatPrice

# Instalação do REDUX

1. Instalar a biblioteca redux e o pacote de integração com o react

```
yarn add redux react-redux
```

2. Criar uma pasta _store_ dentro SRC. Todos os arquivos relacionados a REDUX vão ficar nessa pasta.

3. Criar um arquivo _index.js_ onde será configurado o redux

4. Precisamos importar o redux para dentro do arquivo **App.js** e o Provider precisa ficar em volta de todos os componentes da aplicação e precisamos importar tbm o store e passar dentro do componente **PROVIDER**

```js
import React from "react";
import { BrowserRouter } from "react-router-dom";
import { Provider } from "react-redux";

import GlobalStyle from "./styles/global";
import Header from "./components/Header";
import Routes from "./routes";

import store from "./store";

function App() {
  return (
    <Provider store={store}>
      <BrowserRouter>
        <Header />
        <Routes />
        <GlobalStyle />
      </BrowserRouter>
    </Provider>
  );
}

export default App;
```

5. Dentro da pasta _store_ precisamos criar uma pasta _modules_ e dentro dela a pasta **CART**

6. Se quisermos ter mais de um _reducer_ precisamos criar um arquivo dentro da pasta _STORE/MODULES_ com o nome de **rootReducer**

```js
import { combineReducers } from "redux";

import cart from "./cart/reducer";

export default combineReducers({
  cart
});
```

- Quando eu tiver um novo _reducer_, basta eu adicionar ele aqui nesse arquivo

# Função para adicionar um produto no carrinho de compras

1. No arquivo que eu tenho o botão, eu preciso importar o _connect_ do _redux_

```
import { connect } from 'react-redux';
```

2. Quando utilizamos o _redux_ não podemos declarar uma classe assim:

```js
export default class Home extends Component {}
```

Temos que declarar assim:

```js
class Home extends Component {
  // Códigos
}
export default connect()(Home);
```

3. Sempre que utlizar REDUX, precisamos disparar uma _action_

4. Quando utilizamos o REDUX, dentro das _props_ conseguimos pegar o **dispatch** que é usado para disparar as actions.

5. Toda _ACTION_ precisa ter um _TYPE_ e pode ter o restante do conteudo.

```js
dispatch({
  type: "ADD_TO_CART",
  product
});
```

6. Todo _REDUCER_ por padrão recebe como parametro (state, action)

7. Todo _REDUCER_ nós fazemos um switch dentro dele.

8. Exemplo de um arquivo _reducer.js_

```js
export default function cart(state = [], action) {
  switch (action.type) {
    case "ADD_TO_CART":
      return [...state, action.product];
    default:
      return state;
  }
}
```

9. No arquivo Header, quando para acessarmos a informação do Carrinho de compras, precisamos exportar da seguinte maneira:

```js
function Header(cartSize) {}

export default connect(state => ({
  cartSize: state.cart.length
}))(Header);
```

- **function Header(cartSize)** -> Precisamos enviar como parametro o reducer que queremos acessar. É o mesmo nome no export da function.
- **connect(state =>** -> São os reducer
- **cartSize: state.cart** -> cart É o nome do reducer, que existe dentro do arquivo _rootReducer_.

- Toda vez que um estado do REDUX é alterado a pagina é renderizada.

**REVISANDO**

- Tudo partiu do arquivo _index.js_ da pasta HOME.
- Conectamos ele com o redux
- Quando utilizamos o _connect_ da lib 'react-redux', temos uma props que se chama _dispatch_ que serve para disparar as actions.
- A action é obrigatório uma TYPE.
- Dentro do _reducer.js_ eles escutam todas as actions, por isso utilizamos os SWITCH/CASE para ele pegar apenas as actions q interessa

# Configurando o Reactotron

1. Precisamos instalar as libs de integrações do reactotron com o react e com o redux

```
yarn add reactotron-react-js reactotron-redux
```

2. Criar uma pasta _config_ dentro SRC
3. Criar um arquivo **ReactotronConfig.js**

```js
import Reactotron from "reactotron-react-js";
import { reactotronRedux } from "reactotron-redux";

if (process.env.NODE_ENV === "development") {
  const tron = Reactotron.configure()
    .use(reactotronRedux())
    .connect();

  tron.clear();

  console.tron = tron;
}
```

4. Dentro do arquivo _store/index.js_ vamos declarar uma variavel chamada **enhancer**

```js
const enhancer =
  process.env.NODE_ENV === "development" ? console.tron.createEnhancer() : null;
```

5. Precisamos importar dentro do _App.js_ a configuração do Reactotron

# Listar os produtos dentro do carrinho de compras

1. **mapStateToProps** -> Vai pegar informações do nosso estado e vai mapear em formato de propriedades por componente

# Produtos duplicados.

1. Instalar a biblioteca [IMMER](https://immerjs.github.io/immer/docs/introduction) - Para lidar com objetos imutáveis dentro do REDUX.

```
yarn add immer
```

2. Importar o método produce da lib immer, dentro do _Cart/reducer.js_

```
import produce from 'immmer';
```

```js
import produce from "immer";

export default function cart(state = [], action) {
  switch (action.type) {
    case "ADD_TO_CART":
      return produce(state, draft => {
        // Verifico se o produto já existe
        const productIndex = draft.findIndex(p => p.id === action.product.id);
        // Se o produto já existe, adiciono a quantida
        if (productIndex >= 0) {
          draft[productIndex].amount += 1;
          // Se não existe, crio um novo produto
        } else {
          draft.push({
            ...action.product,
            amount: 1
          });
        }
      });
    default:
      return state;
  }
}
```

# Refatorando as actions

1. Dentro da pasta _modules/cart_ vamos criar um arquivo _actions.js_

2. Quando utilizamos a seguinte formatação:

```
import * as CartActions from '../../store/modules/cart/actions';
```

- É porque dentro do arquivo existe mais de 1 export

3. **mapDispatchToProps** -> Converte actions do redux em propriedades do nosso componente

# Calculando Totais

1. Podemos realizar esse calculo dentro da metodo **mapStateToProps**.

```js
const mapStateToProps = state => ({
  cart: state.cart.map(product => ({
    ...product,
    subtotal: formatPrice(product.price * product.amount)
  })),
  total: formatPrice(
    state.cart.reduce((total, product) => {
      return total + product.price * product.amount;
    }, 0)
  )
});
```

2. Tudo que está dentro do _mapStateToProps_ nós temos acesso como uma propriedade.

# Exibindo Quantidades

1. Vamos criar uma _mapToStateToProps_ dentro o Home/index.js

# Redux-Saga

1. Instalar a dependencia Redux-Saga

```
yarn add redux-saga
```

2. Criar um arquivo _store/modules/cart/sagas.js_

```js
import { call, put, all, takeLatest } from "redux-saga/effects";
import api from "../../../services/api";

import { addToCartSuccess } from "./actions";

function* addToCart({ id }) {
  const response = yield call(api.get, `/product/${id}`);

  yield put(addToCartSuccess(response.data));
}

export default all([takeLatest("@cart/ADD_REQUEST", addToCart)]);
```

- PUT: Usado para disparar uma actions dentro do redux-saga.
- CALL: Usado para executar chamada a funções.
- function\* === async -> Declaração Generation do JS e mais potente do que o async.
- Não utilizamos AWAIT e sim YIELD.
- takeLatest: Sempre pega a action do ultimo clique. 1º Param: a action que quero disparar e 2º Param: o reducer que quero chamar.

3. Criar um arquivo _store/modules/rootSaga.js_

```js
import { all } from "redux-saga/effects";

import cart from "./cart/sagas";

export default function* rootSaga() {
  return yield all([cart]);
}
```

- Quando eu tiver novos SAGAS é só ir adicionando nesse arquivo.

- Configurando o arquivo store/index.js para o redux-saga

```js
import { createStore, applyMiddleware, compose } from "redux";
import createSagaMiddleware from "redux-saga";

import rootReducer from "./modules/rootReducer";
import rootSaga from "./modules/rootSaga";

const sagaMiddleware = createSagaMiddleware();

const enhancer =
  process.env.NODE_ENV === "development"
    ? compose(
        console.tron.createEnhancer(),
        applyMiddleware(sagaMiddleware)
      )
    : applyMiddleware(sagaMiddleware);

const store = createStore(rootReducer, enhancer);

sagaMiddleware.run(rootSaga);

export default store;
```

# Reactotron + Saga

1. Instalar o plugin do reactotron-redux

```
yarn add reactotron-redux-saga
```

2. Alterar o arquivo de configuração do Reactotron

```js
import Reactotron from "reactotron-react-js";
import { reactotronRedux } from "reactotron-redux";
import reactotronSaga from "reactotron-redux-saga";

if (process.env.NODE_ENV === "development") {
  const tron = Reactotron.configure()
    .use(reactotronRedux())
    .use(reactotronSaga())
    .connect();

  tron.clear();

  console.tron = tron;
}
```

# React Toastify

1. Instalar a lib

```
yarn add react-toastify
```

2. Acessar o App.js que é o nosso componente principal e importar o {ToastContainer}.

```js
import React from "react";
import { BrowserRouter } from "react-router-dom";
import { Provider } from "react-redux";
import { ToastContainer } from "react-toastify";

import "./config/ReactotronConfig";

import GlobalStyle from "./styles/global";
import Header from "./components/Header";
import Routes from "./routes";

import store from "./store";

function App() {
  return (
    <Provider store={store}>
      <BrowserRouter>
        <Header />
        <Routes />
        <GlobalStyle />
        <ToastContainer />
      </BrowserRouter>
    </Provider>
  );
}

export default App;
```

3. Dentro do arquivo styles/global.js precisamos importar o css do toastify

```js
import "react-toastify/dist/ReactToastify.css";
```

4. Dentro do arquivo de store/modules/cart/sagas.js, precisamos importar o toast para podermos dar as mensagens para o usuário.

```js
import toast from "react-toastify";
```

5. Quando formos trabalhar com Saga, é sempre boa prática dividir as actions em 2 partes.

@UPDATE_AMOUNT_REQUEST
@UPDATE_AMOUNT_SUCCESS

# Navegando dentro do REDUX

1. Instalar a dependencia _history_ que é uma parte do JS que serve para controlar o history API do nosso navegador que o react-router-dom utiliza.

```
yarn add history
```

2. Criar um arquivo dentro de services/history.js
3. Importar o history nesse arquivo
4. Tiramos o BrowserRouter e substituimos por Router.

```js
import React from "react";
import { Router } from "react-router-dom";
import { Provider } from "react-redux";
import { ToastContainer } from "react-toastify";

import "./config/ReactotronConfig";

import GlobalStyle from "./styles/global";
import Header from "./components/Header";
import Routes from "./routes";

import history from "./services/history";
import store from "./store";

function App() {
  return (
    <Provider store={store}>
      <Router history={history}>
        <Header />
        <Routes />
        <GlobalStyle />
        <ToastContainer autoClose={3000} />
      </Router>
    </Provider>
  );
}

export default App;
```

5. Importar o history dentro do modules/cart/sagas.js e adiciono a seguinte linha no momento que estou adicionando ao carrinho

```js
history.push("/cart");
```
