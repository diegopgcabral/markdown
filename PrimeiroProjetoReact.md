# Criando projeto do Zero

**O comando abaixo já cria uma estrutura com tudo encapsulado, como config. de imagens, babel, webpack e etc..**

```
yarn create react-app bootcamp-gostack-module05
```

**Adicionar o ESLint como dependência de Desenvolvimento**

```
yarn add eslint -D
```

**Inicializando o ESlint dentro do projeto**

```
yarn eslint --init
```

## Configurações do ESLint:

1. To check syntax, find problems, and enforce code style;
2. Javascript(import/export);
3. React;
4. Browser;
5. Typescript - NO
6. Use a popular guide
7. Airbnb
8. Javascript
9. Yes
10. Yes

_Após a instalação:_

11. Excluir o arquivo _package-lock.json_
12. Executar _yarn_
13. Instalar a extensão do prettier

```
yarn add prettier eslint-config-prettier eslint-plugin-prettier babel-eslint -D
```

**No package.json, precisamos excluir as configurações do ESLint, pois vamos configurar manualmente**

```js

  "eslintConfig": {
    "extends": "react-app"
  },
```

**Excluir os arquivos da pasta PUBLIC e deixar somente:**

- index.html

```js
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>React App</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>
```

**Excluir os arquivos da pasta SRC e deixar somente:**

- App.js
- index.js

**Gerar o arquivo _.editorconfig_**

```js
root = true

[*]
indent_style = space
indent_size = 2
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
end_of_line = lf
```

**Estrutura do arquivo _.eslintrc.js_**

```js
module.exports = {
  env: {
    browser: true,
    es6: true
  },
  extends: ["airbnb", "prettier", "prettier/react"],
  globals: {
    Atomics: "readonly",
    SharedArrayBuffer: "readonly"
  },
  parser: "babel-eslint",
  parserOptions: {
    ecmaFeatures: {
      jsx: true
    },
    ecmaVersion: 2018,
    sourceType: "module"
  },
  plugins: ["react", "prettier"],
  rules: {
    "prettier/prettier": "error",
    "react/jsx-filename-extension": ["warn", { extensions: [".jsx", ".js"] }],
    "import/prefer-default-export": "off",
    "react/state-in-constructor": "off",
    "react/static-property-placement": "off",
    "jsx-a11y/control-has-associated-label": "off",
    "no-param-reassign": "off",
    "no-console": ["error", { allow: ["tron"] }]
  }
};
```

**Criar o arquivo _.prettierrc_**

```js
{
  "singleQuote": true,
  "trailingComma": "es5"
}

```

## Criando rota no React

**Biblioteca para fazer roteamento no Front-end da APP**

```
yarn add react-router-dom
```

1. Criar um arquivo _routes.js_ dentro da pasta _SRC_

2. **Criar uma pasta _pages_ dentro da pasta _SRC_:** É onde vão ficar as páginas da nossa aplicação;

3. Vamos criar dentro da pasta pages, duas subpastas: _Main_ e _Repository_ e dentro de cada pasta temos que criar um arquivo _index.js_;

4. Para criar uma estrutura de um _React Function component_, precisa instalar o snnipet da Rocketseat de react e digitar: **rfc** no arquivo que a estrutura será criada.

5. Precisamos criar essa estrutura dentro dos 2 arquivos index.js que foram criados;

6. A biblioteca _'react-router-dom'_ exporta vários tipos de Roteadores.

7. Importar a biblioteca no arquivo _routes.js_

```
import {BrowserRouter, Switch, Route} from 'react-router-dom'
```

- _BrowserRouter_ precisa ficar envolta de todos os outros componentes;
- _Switch_ basicamente o que ele faz é garantir que apenas uma rota seja exibida por momento, porque o react-router-dom tem o poder de chamar + de 1 rota ao mesmo tempo.
- _Route_ Cada Route representa uma página da nossa aplicação.

- Dentro do arquivo _App.js_ eu preciso importar o arquivo _routes.js_;

```js
<Route path="/" exact component={Main} />
```

- Quando utilizamos o _exact_ a rota será exclusivamente para o caminho definido;

**Instalando _styled-components_**

```
yarn add styled-components
```

**Criar um arquivo _styles.js_ dentro da pasta MAIN e importar o styled-components nesse arquivo**

```js
import styled from "styled-components";
```

**Instalar a extensão do VScode:** _vscode-styled-components_

**Estrutura de um arquivo _styles.js_**

```js
import styled from "styled-components";

export const Title = styled.h1`
  font-size: 24px;
  color: #7159c1;
  font-family: Arial, Helvetica, sans-serif;
`;
```

1. Para utilizar o arquivo _styles.js_ precisamos importar e utilizá-lo ao invés das tags HTML.

```js
import React from "react";

import { Title } from "./styles";

export default function Main() {
  return <Title>Main</Title>;
}
```

2. Controlando propriedades dentro do styles

```js
import React from "react";

import { Title } from "./styles";

export default function Main() {
  return (
    <Title error>
      Main
      <small>menor</small>
    </Title>
  );
}
```

error: Se _error_ for true, modifica a cor no arquivo _styles.js_

```js
import styled from "styled-components";

export const Title = styled.h1`
  font-size: 24px;
  color: ${props => (props.error ? "red" : "#7159c1")};
  font-family: Arial, Helvetica, sans-serif;

  small {
    font-size: 14px;
    color: #333;
  }
`;
```

3. Quando utilizamos essa definção no css, estamos dizendo que as letras das fontes, serão bem definidas, bem desenhadas:

```js
-webkit-font-smoothing: antialiased !important;
```

**Instalar pacotes de icones mais famosos**

```
yarn add react-icons
```

**Instalar a biblioteca _AXIOS_ para consumir uma API externa**

```
yarn add axios
```

1. Precisamos criar uma pasta dentro _SRC_ chamada de **services** e dentro dessa pasta, vamos criar um arquivo **api.js**

**Passar parametro através de uma URL no React**

```js
<Link to={`/repository/${repository.name}`}>Detalhes</Link>
```

1.  Link -> Importamos do _'react-router-dom'_ e serve para redirecionar as rotas que queremos e precisamos informar o _to_.
2.  Quando utilizamos {} estamos informando que vamos utilizar código javascript e \${XXXXX} indica o argumento que estamos enviando para a rota.

**Vamos adicionar a biblioteca _prop-types_**

```
yarn add prop-types
```
