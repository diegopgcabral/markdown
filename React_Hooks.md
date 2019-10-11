# React Hooks

1. Instalar o plugin para o React Hooks

```
yarn add eslint-plugin-react-hooks -D
```

2. Preciso alterar o arquivo _.eslintrs.js_ para incluir o plugin do react-hooks e incluir 2 novas _RULES_

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
  plugins: ["react", "prettier", "react-hooks"],
  rules: {
    "prettier/prettier": "error",
    "react/jsx-filename-extension": ["warn", { extensions: [".jsx", ".js"] }],
    "import/prefer-default-export": "off",
    "react/state-in-constructor": "off",
    "react/static-property-placement": "off",
    "jsx-a11y/control-has-associated-label": "off",
    "no-param-reassign": "off",
    "no-console": ["error", { allow: ["tron"] }],
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn"
  }
};
```

## Hook useState

1. importarmos da lib _react_:

```js
import { useState } from "react";
```

2. Não precisamos mais declarar um State e sim uma constante.

```
const [tech, setTech] = useState([]);
```

- Realizar um desestruturação, onde o primeiro param é o nome da lista e o segundo parametro é o método para setar os estados.

```js
import React, { useState } from "react";

function App() {
  const [tech, setTech] = useState(["ReactJS", "React Native"]);
  const [newTech, setNewTech] = useState("");

  function handleAdd() {
    setTech([...tech, newTech]);
    setNewTech("");
  }

  return (
    <>
      <ul>
        {tech.map(t => (
          <li key={t}>{t}</li>
        ))}
      </ul>
      <input value={newTech} onClick={e => setNewTech(e.target.value)} />
      <button type="button" onClick={handleAdd}>
        Adicionar
      </button>
    </>
  );
}

export default App;
```

## Hook useEffect

```js
import React, { useState, useEffect } from "react";

function App() {
  const [tech, setTech] = useState([]);
  const [newTech, setNewTech] = useState("");

  function handleAdd() {
    setTech([...tech, newTech]);
    setNewTech("");
  }

  // Simulando o componentDidMount()
  useEffect(() => {
    const storageTech = localStorage.getItem("tech");

    if (storageTech) {
      setTech(JSON.parse(storageTech));
    }
  }, []);

  useEffect(() => {
    localStorage.setItem("tech", JSON.stringify(tech));
  }, [tech]);

  return (
    <>
      <ul>
        {tech.map(t => (
          <li key={t}>{t}</li>
        ))}
      </ul>
      <input value={newTech} onChange={e => setNewTech(e.target.value)} />
      <button type="button" onClick={handleAdd}>
        Adicionar
      </button>
    </>
  );
}

export default App;
```
