# Design System - Introdução

Base de construção de um Design System utilizando Storybook, Radix UI, Figma e React.

Um conceito importante na construção de um Design System é intender quais são os tokens ou componentes reutilizáveis.

Por exemplo: cores, tamanho de fonte e espaçamentos (paddings e margins).

## Configuração do Storybook
```sh
# Install...

npx sb init --builder @storybook/builder-vite --use-npm

# Por padrão o sb vem configurado com webpack. Neste caso, será configurado com o vite
```
> 💡 Dica: Instale o [Language support](https://marketplace.visualstudio.com/items?itemName=unifiedjs.vscode-mdx) for MDX no editor.

Configuração Dark Theme
```js
// .storybook/manager.js

import { addons } from '@storybook/addons'
import { themes } from '@storybook/theming'

addons.setConfig({
  theme: themes.dark
})

/*---------------------------------------------*/

// .storybook/preview.cjs

import { themes } from '@storybook/theming'

export const parameters = {
  actions: { argTypesRegex: "^on[A-Z].*" },
  controls: {
    matchers: {
      color: /(background|color)$/i,
      date: /Date$/,
    },
  },
  docs: {
    theme: themes.dark,
  }
}
```

Em `main.csj` verificar se está correto o caminho dos arquivos que vai entrar na documentação

```js
module.exports = {
  "stories": [
    "../src/**/*.stories.mdx",
    "../src/**/*.stories.@(js|jsx|ts|tsx)"
  ],
	//....
}
```
## Deploy
Para o deploy, utilizaremos a biblioteca [Storybook Deployer](https://github.com/storybookjs/storybook-deployer) .
```sh
#Install...

npm i @storybook/storybook-deployer --save-dev
```

Após a instalação, incluir em package.json o seguinte script:

```json
/* github page */

{
  "scripts": {
    "deploy-storybook": "storybook-to-ghpages"
  }
}

/* AWS S3 */

{
  "scripts": {
    "deploy-storybook": "storybook-to-aws-s3"
  }
}
```

Quando publicado no GitHub pages, caso seja uma **organização**, para não haver problemas no deploy, é importante fazer a seguinte modificação no `main.cjs`.

```js
module.exports = {
  /*...*/
  viteFinal: (config, {configType}) => {
    if (configType === 'PRODUCTION') {
      config.base = '/SEU-REPOSITÓRIO/'
    }

    return config
  }
}
```

## Adicionando Addons de acessibilidade

[addon a11y](https://storybook.js.org/addons/@storybook/addon-a11y):  `npm install @storybook/addon-a11y`

Adicione esta linha ao seu arquivo main.js (crie este arquivo dentro do seu diretório de configuração do Storybook, se necessário).

```js
module.exports = {
  addons: ['@storybook/addon-a11y'],
};
```
