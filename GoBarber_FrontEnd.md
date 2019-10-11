# GoBarber Front-end

1. Fazer as configurações básicas de uma aplicação reactJS.

2. No Backend precisamos instalar uma nova dependencia _CORS_ que permite que outras aplicações acessem a nossa API.

```
yarn add cors
```

3. Alterar o arquivo app.js do backend para importar e cors e incluir ele no middleware.

4. Alterar o arquivo SessionControler para passar o avatar do usuário. Importar o model FILE para dentro desse controller e fazer um include no findOne do usuário.

5. Preciso alterar o UPDATE do UserController para contemplar o avatar do usuário.

6. Alterar o SchedullerController, para retornar o nome do usuário.

7. Deixar a API rodando.

# Front-End

1. Instalar a lib

```js
yarn add react-router-dom
```

### Definindo estrutura

2. Crias as pastas:

   - src/pages
   - src/routes
   - src/services
     - src/services/history.js

3. Instalar a dependencia history

```
yarn add history
```

4. Criar a estrutura de pages com um arquivo index.js dentro de cada pasta e utilizar o snippet **rfc**.

- Dashboard/index.js
- Profile/index.js
- SignIn/index.js
- SignUp/index.js

5. Alterar o arquivo de routes.js

- importar React sempre...
- importar { Switch, Route} do react-router-dom
- Importar as pages para dentro da aplicação e definir as rotas.

6. Importar as rotas dentro do arquivo _App.js_

```js
import React from "react";
import { Router } from "react-router-dom";

import Routes from "./routes";
import history from "./services/history";

function App() {
  return (
    <Router history={history}>
      <Routes />;
    </Router>
  );
}

export default App;
```

7. Configurar o arquivo service/history.js

```js
import { createBrowserHistory } from "history";

const history = createBrowserHistory();

export default history;
```

### Configurando o reactotron

```
yarn add reactotron-react-js
```

1. Criar uma pasta SRC/config/ReactotronConfig.js

2. Configurar o arquivo

```js
import Reactotron from "reactotron-react-js";

if (process.env.NODE_ENV === "development") {
  const tron = Reactotron.configure().connect();

  tron.clear();

  console.tron = tron;
}
```

3. Importar o Reactotron dentro do App.js

```
import './config/ReactotronConfig';
```

4. Iniciar a aplicação

```
yarn start
```

5. Para facilitar antes do Desenvolvimento, podemos colocar um <h1>Nome da Pagina</h1> para saber e as rotas estão apontando para o lugar correto.

### Definindo rota privada

1. Criar um arquivo routes/Route.js

2) Importar os prop-types

```
yarn add prop-types
```

3. Definimos o Tipo de arquivo de Route e importamos no App.js. Deixamos de importar o Route do react-router-dom e passamos a importar do routes/Route.js

```js
import React from "react";
import PropTypes from "prop-types";
import { Route, Redirect } from "react-router-dom";

export default function RouteWrapper({
  component: Component,
  isPrivate = false,
  ...rest
}) {
  const signed = false;

  if (!signed && isPrivate) {
    return <Redirect to="/" />;
  }

  if (signed && !isPrivate) {
    return <Redirect to="/dashboard" />;
  }

  return <Route {...rest} component={Component} />;
}

RouteWrapper.propTypes = {
  isPrivate: PropTypes.bool,
  component: PropTypes.oneOfType([PropTypes.element, PropTypes.func]).isRequired
};

RouteWrapper.defaultProps = {
  isPrivate: false
};
```

## Configurando Layout

1. Criar uma pasta \__layouts_ dentro de _pages_
2. Instalar a lib de styled-components

```
yarn add styled-components
```

3. Auth (layout) é o layoute de login
4. Após configurar os arquivos index.js das pasta de \_layouts, devemos importar para dentro do arquivo Route.js

```js
import React from "react";
import PropTypes from "prop-types";
import { Route, Redirect } from "react-router-dom";

import AuthLayout from "../pages/_layouts/auth";
import DefaultLayout from "../pages/_layouts/default";

export default function RouteWrapper({
  component: Component,
  isPrivate = false,
  ...rest
}) {
  const signed = false;

  if (!signed && isPrivate) {
    return <Redirect to="/" />;
  }

  if (signed && !isPrivate) {
    return <Redirect to="/dashboard" />;
  }

  const Layout = signed ? DefaultLayout : AuthLayout; // ALTERAMOS AQUI

  return (
    <Route
      {...rest}
      // TIRAMOS O COMPONENT E MUDAMOS PARA RENDER
      render={props => (
        <Layout>
          <Component {...props} />
        </Layout>
      )}
    />
  );
}

RouteWrapper.propTypes = {
  isPrivate: PropTypes.bool,
  component: PropTypes.oneOfType([PropTypes.element, PropTypes.func]).isRequired
};

RouteWrapper.defaultProps = {
  isPrivate: false
};
```

## Configurando os estilos Globais da aplicação

1. Criar uma pasta src/styles/global.js
2. Importar a fonte ROBOTO para dentro do arquivo
3. Customize e pegar o bold 700

```
  body {
    -webkit-font-smoothing: antialiased;
  }
```

4. -webkit.... faz com que a fonte fique bem mais definida.

GLOBAL.JS

```js
import { createGlobalStyle } from "styled-components";

export default createGlobalStyle`
  @import url('https://fonts.googleapis.com/css?family=Roboto:400,700&display=swap');

  * {
    margin: 0;
    padding: 0;
    outline: 0;
    box-sizing: border-box;
  }

  *:focus {
    outline: 0;
  }

  html, body, #root {
    height: 100%;
  }

  body {
    -webkit-font-smoothing: antialiased;
  }

  body, input button {
    font: 14px 'Roboto' sans-serif;
  }

  a {
    text-decoration: none;
  }

  ul {
    list-style: none;
  }

  button {
    cursor: pointer;
  }
`;
```

5. Importar esse arquivo global style para dentro do arquivo App.js

6. Vamos instalar duas extensões para permitir a gente alterar essas configurações de caminho

```
yarn add customize-cra react-app-rewired -D
```

- Essas extensões servem para não ficarmos usando ../../ podemos definir o caminho root.

7. Criar um arquivo na raiz do projeto com o nome: **config-overrides.js**

8. Instalar o plugin do babel root import

```
yarn add babel-plugin-root-import -D
```

```
yarn add eslint-import-resolver-babel-plugin-root-import -D
```

```js
const { addBabelPlugin, override } = require("customize-cra");

module.exports = override(
  addBabelPlugin([
    "babel-plugin-root-import",
    {
      rootPathSuffix: "src"
    }
  ])
);
```

9. Alterar o arquivo package.json

```
 "scripts": {
    "start": "react-app-rewired start",
    "build": "react-app-rewired build",
    "test": "react-app-rewired test",
    "eject": "react-scripts eject"
  },
```

10. Criar um arquivo na raiz jsconfig.json

```
{
  "compilerOptions": {
    "baseUrl": "src",
    "paths": {
      "~/*": ["*"]
    }
  }
}
```

## Estilização da autenticação

1. Alterando o arquivo SignIn/index.js para criar a tela de login

2. Alterar o arquivo auth/index.js para a tela de login.

3. Configurar o arquivo auth/styles.js e importar a lib abaixo para podermos usar o darken ou lighten

```
yarn add polished
```

4. copiar a estrutura do arquivo signin/index.js para o arquivo signup/index.js e incluir os campos para essa tela.

# Utilizando UnForm

1. A rocketseat criar uma lib para criação de formulário, performatico dentro do reactjs. Vamos instalar essa LIB no projeto.

```
yarn add @rocketseat/unform
```

2. Alterar os arquivos index.js das pastas SignIn e SignUp, para importar o Form e o input da rocketseat/unform

```js
import { Form, Input } from "@rocketseat/unform";
```

3. Instalando a biblioteca de validações Yup

```
yarn add yup
```

4. Realizar as validações nos campos de SignIn e SignUp

5. passamos o schema de validação dentro do componente.

```js
 <Form schema={schema} onSubmit={handleSubmit}>
```

## Configurando Store

1. A parte de validações do usuário vamos fazer dentro do redux.

2. Criar a pasta _src/store_ e instalar as bibliotecas do redux e redux-saga, as integrações com reactotron e immer

```
yarn add redux redux-saga react-redux reactotron-redux reactotron-redux-saga immer
```

3. Criar um arquivo store/index.js
4. Criar a seguinte estrutura de configuração do redux e saga

- store
  - index.js
  - modules
    - auth
      - actions.js
      - reducer.js
      - sagas.js
    - rootReducer.js
    - rootSaga.js

5. Começar pelo reducer.js, ele sempre é uma função. è uma função que recebe um estado e retorna esse estado alterado de acordo com cada actions.

-Estrutura inicial de um reducer sempre será essa:

```js
const INITIAL_STATE = {};

export default function auth(state = INITIAL_STATE, action) {
  switch (action.type) {
    default:
      return state;
  }
}
```

-Estrutura inicial de um sagas.js

```js
import { all } from "redux-saga";

export default all([]);
```

6. Configurar o rootReducer.js

- Estrutura inicial. Nesse arquivo vamos importar todos os **reducer** da aplicação.

```js
import { combineReducers } from "redux";

import auth from "./auth/reducer";

export default combineReducers({
  auth
});
```

7. Configurando o rootSaga.js

```js
import { all } from "redux-saga/effects";

import auth from "./auth/sagas";

export default function* rootSaga() {
  return yield all([auth]);
}
```

8. Criar um arquivo store/createStore.js

```js
```

9. configurar o store/index.js

```js
import createSagaMiddleware from "redux-saga";
import createStore from "./createStore";

import rootReducer from "./modules/rootReducer";
import rootSaga from "./modules/rootSaga";

const sagaMiddleware = createSagaMiddleware();

const middlewares = [sagaMiddleware];

const store = createStore(rootReducer, middlewares);

sagaMiddleware.run(rootSaga);

export default store;
```

10. Precisamos alterar o arquivo reactotronConfig para configurar o redux e o saga.

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

11. Voltar no arquivo store/index.js, precisamos passar o sagaMonitor e validar se está em desenvolvimento e dps passar no sagaMiddleware.

12. Configurar novamente o arquivo createStore.js

```js
import { createStore, compose, applyMiddleware } from "redux";

export default (reducers, middlewares) => {
  const enhancer =
    process.env.NODE_ENV === "development"
      ? compose(
          console.tron.createEnhancer(),
          applyMiddleware(...middlewares)
        )
      : applyMiddleware(...applyMiddleware.middlewares);

  return createStore(reducers, enhancer);
};
```

13. Abrimos o App.js e importamos o Provider e colocamos em volta de toda a aplicação.
    A importação

```
import store from './store';
```

precisa vir depois da importação

```
import './config/ReactotronConfig';
```

Senão não pega as configurações do reactotron.

## Autenticação

1. Começar a definir as actions que vamos utilizar.(actions.js)
2. Por boa forma de programção, declaramos um _payload_ na actions que normalmente recebe o paramentro.
3. Configurar o _sagas.js_
4. Instalar o axios, pois vamos acessar a API.

```js
yarn add axios
```

5. Criar o arquivo services/api.js
6. Configurar o arquivo sagas.js, para acessar a API via axios e fazer todo o tratamento.

7. No arquivo sigIn/index.js precisamos importar e alterar os arquivos.

```
import {useDispatch} from 'react-redux';
import {signInRequest}
```

8. Alterar o arquivo reducer.js para setar as variaveis do INITIAL_STATE.

9. alterar o arquivo Route.js para importar a store e criar uma const que receba esse valor que veio da store.

## Armazenando Perfil

1. Criar uma pasta store/modules/user e os 3 arquivos: actions, reducer e sagas.js

2. Importar o reducer de User dentro do rootReducer e o saga dentro de rootSaga

## Persistindo Autenticação

1. Instalar a lib redux-persist

```
yarn add redux-persist
```

2. Dentro da pasta store, criar o arquivo persistReducers.js

3. Voltar no arquivo store/index.js da aplicação, vamos importar: Linhas incluidas

```
import {persistStore} from 'redex-persist';
import persistReducers from './persistReducers';

const store = createStore(persistReducers(rootReducer), middlewares);
const persistor = persistStore(store);

export { store, persistor };
```

4. Quando salvar dará alguns erros, mas vamos abrir o arquivo App.js, vamos exportar apenas o {store, persistor} e não mais o **store** default e importar:

```
import { PersistGate } from 'redux-persist/integration/react';
```

- O PersistGate precisa ficar envolta do componente Router.

```js
function App() {
  return (
    <Provider store={store}>
      <PersistGate persistor={persistor}>
        <Router history={history}>
          <Routes />
          <GlobalStyle />
        </Router>
      </PersistGate>
    </Provider>
  );
}
```

5. Quando salvar dará alguns erros, mas vamos abrir o arquivo Route.js, vamos exportar apenas o {store} e não mais o **store** default

## Loading na autenticação

1. Alterar o arquivo /auth/reducer.js
2. Tratar as actions dentro do Reducer.
3. Ir no arquivo SignIn/index.js e importar:
   UseSelector do react-redux.
4. Definir uma variavel loading dentro da function SignIn.
5. Retornar ao auth/sagas.js e adicionar um try/catch por volta da chamada da API.

## Exibindo o Toastify

1. Instalar o react-toastify

```
yarn add react-toastify
```

2. Importar o toastify dentro do App.js
3. Importar dentro do Global.js
4. Importar também dentro do auth/saga.js

## Cadastro na aplicação

1. Alterar a actions de auth, para incluir os SigUp
2. Alterar o signUp/index.js
3. Import signUpRequest de actions.
4. Precisamos ouvir essa requisição dentro do saga. Alterar o arquivos auth/sagas.js

## Requisições Autenticadas

1. Vamos incluir no Header do Axios, o token.
2. Alterar o arquivo auth/sagas.js
3. Usar o api.defaults....
4. No auth/sagas.js, vamos adicionar no export o takeLatest('persist/REHYDRATE', setToken)
5. Criamos a função setToken

## Configurando Header

1. Configurar o arquivo styles.js DEFAULT
2. Criar uma pasta SRC/components/Header/index.js
3. Codar o arquivo index.js
4. Criar e configurar o styles.js
5. Abrir o arquivo default/index.js e importar o componente Header e colocar ele após o Wrapper.

## Estilizando as notificações

1. Criar uma arquivo componenent/Notifications/index.js
2. Importar o Notifications dentro do header/index.js
3. Instalar a biblioteca de icones

```
yarn add react-icons
```

4. hasUnread -> Será TRUE quando tiver alguma notificação que o usuário ainda não leu.
5. Configurar o hasUnread no styles.js
6. Criar a NotificationList e Notification no index.js
7. left: calc(50% - 130px) -> Divide a tela na metade para ficar sempre centralizada a width = 260.
8. Vamos instalar a biblioteca

```
yarn add react-perfect-scrollbar
```

Com essa lib, quando tivermos muitas notificações, podemos limitar o numero que será exibida e o restante será exibida com scrool.

9. Adicionar o Scroll dentro do Notitifications/index.js

10. Configurar Scroll dentro do styles.js
11. importar o css do Scrollbar [link](https://www.npmjs.com/package/react-perfect-scrollbar) dentro do global.js

## Notificações

1. no arquivo notifications/index.js criar um estado dentro da function Notifications
2. Dentro do componente <_NotificationList_> passar o visible.
3. Alterar o NotificationList no arquivo styles.js, propriedade _display_
4. Precisamos carregar a API dentro do arquivo index.js
5. Precisamos criar um useEffect para disparar uma ação sempre que o componente for montado.
6. Instalar a biblioteca:

```
yarn add date-fns@next
```

7. Configurar a exibição das mensagens.

## Página de Perfil

1. Configurar os snippets de configuração nos arquivos do profile. index.js e styles.js
2. Criar os formulários da tela de profile.

## Atualizando Perfil

1. Importar o useSelector no profile/index.js
2. Criar as actions no store de user.
3. Alterar o saga de user para ele ouvir essas actions.
4. Precisamos agora conectar a action no index.js
5. importar o useDispatch e a user/actions
6. Precisamos atualizar as informações no reducer, para trazer as informações atualizadas.
   criar um case UPDATE_PROFILE_SUCCESS.

## Foto Perfil

1. criar um arquivo Profile/Avatar/index.js
2. Voltamos no index.js de profile e importamos o index de avatar
3. Criar um useState
4. const data = new FormData() -> Para enviar o formulário do tipo Multipart Form Data.
5. Preciso incluir no sagas.js o avatar_id

## Dados no Header

1. Alterar o arquivo header/index.js
2. importar useSelector, para buscar informações no Redux.

## Logout da aplicação.

1. Dentro da auth/actions.js criar uma para o logout
2. No reducer de autenticação auth/reducer.js, criar um case para o logout.
3. Temos que ouvir essa action tbm no reducer de usuário.
4. No saga de auth, vamos ouvir a action de SIGN OUT
5. Vamos no index de profile e importar a ação SIGNOUT
6. vamos criar uma function handleSignOut() e incluir no onClick do Sair

## Estilização do Dashboard

1. Configurar o index.js
2. Configurar o styles.js

## Navegando entre Dias

1. Importar useState e useMemo dentro do index do dashboard.
2. Iniciar as variaves com useState

## Listando agendamentos

1. no index.js, vamos definir o range de horario de atendimentos.
2. importar o Hook useEffect
3. instalar uma biblioteca, para tratarmos a time zone.

```
yarn add date-fns-tz
```
