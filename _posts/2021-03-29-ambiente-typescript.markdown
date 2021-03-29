---
layout: post
title: "Criando ambiente de desenvolvimento com NodeJs e Typescript"
date: 2021-03-08
categories:
  - Typescript
description:
image: https://source.unsplash.com/user/jdiegoph/0o_GEzyargo/2000x1200
image-sm: https://source.unsplash.com/user/jdiegoph/0o_GEzyargo/500x300
---

O mongodb tornou o meu desenvolvimento com banco de dados extremameMeu ambiente de desenvolvimento com typescript pode variar bastante dependendo da tecnologia, no caso com o NodeJs costumo usar algumas ferramentas bem diferentes comparado ao ambiente de desenvolvimento web, talvez com a exceção de duas ferramentas. De qualquer forma espero que o ambiente ao qual descrevo aqui seja de utilidade para alquém, ele foca em produtividade e em evitar erros que possam passar despercebidos. Confesso que ainda não está do jeito que eu queria, ainda falta definir qual a melhor ferramenta de build, mas tem se mostrado eficaz até o momento.

As ferramentas utilizadas são o **eslinter** com um styleguide, o **prettier**, o **sucrase**, **nodemon**, e o **tsc**. Bora instalá-las e configurá-las.

Detalhe todas as ferramentas assume que você tem o nodejs instalado, além de eu próprio está a utilizar um ambiente Linux. Claro que não é difícil replicar as ações abaixo no Windows.
Depois que as primeiras pastas e arquivos são criados instalamos as bibliotecas necessárias. Primeiro instalamos o typescript.

~~~ shell

~~~
<br />

O typescript irá instalar a ferramenta tsc que pode ser executada da seguinte forma para converter typescript em javascript.

~~~ shell

~~~
<br />

No entanto, precisamos de mais configurações que o typecript possa nos oferecer. Então executamos o comando abaixo para criar um arquivo de configuração do próprio typescript.

~~~ shell

~~~
<br />

Arquivo gerado:

~~~ javascript

{
  "compilerOptions": {
    /* Visit https://aka.ms/tsconfig.json to read more about this file */

    /* Basic Options */
    // "incremental": true,                         /* Enable incremental compilation */
    "target": "es5",                                /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019', 'ES2020', or 'ESNEXT'. */
    "module": "commonjs",                           /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', or 'ESNext'. */
    // "lib": [],                                   /* Specify library files to be included in the compilation. */
    // "allowJs": true,                             /* Allow javascript files to be compiled. */
    // "checkJs": true,                             /* Report errors in .js files. */
    // "jsx": "preserve",                           /* Specify JSX code generation: 'preserve', 'react-native', 'react', 'react-jsx' or 'react-jsxdev'. */
    // "declaration": true,                         /* Generates corresponding '.d.ts' file. */
    // "declarationMap": true,                      /* Generates a sourcemap for each corresponding '.d.ts' file. */
    // "sourceMap": true,                           /* Generates corresponding '.map' file. */
    // "outFile": "./",                             /* Concatenate and emit output to single file. */
    "outDir": "./dist",                              /* Redirect output structure to the directory. */
    // "rootDir": "./",                             /* Specify the root directory of input files. Use to control the output directory structure with --outDir. */
    // "composite": true,                           /* Enable project compilation */
    // "tsBuildInfoFile": "./",                     /* Specify file to store incremental compilation information */
    // "removeComments": true,                      /* Do not emit comments to output. */
    // "noEmit": true,                              /* Do not emit outputs. */
    // "importHelpers": true,                       /* Import emit helpers from 'tslib'. */
    // "downlevelIteration": true,                  /* Provide full support for iterables in 'for-of', spread, and destructuring when targeting 'ES5' or 'ES3'. */
    // "isolatedModules": true,                     /* Transpile each file as a separate module (similar to 'ts.transpileModule'). */

    /* Strict Type-Checking Options */
    "strict": true,                                 /* Enable all strict type-checking options. */
    // "noImplicitAny": true,                       /* Raise error on expressions and declarations with an implied 'any' type. */
    // "strictNullChecks": true,                    /* Enable strict null checks. */
    // "strictFunctionTypes": true,                 /* Enable strict checking of function types. */
    // "strictBindCallApply": true,                 /* Enable strict 'bind', 'call', and 'apply' methods on functions. */
    // "strictPropertyInitialization": true,        /* Enable strict checking of property initialization in classes. */
    // "noImplicitThis": true,                      /* Raise error on 'this' expressions with an implied 'any' type. */
    // "alwaysStrict": true,                        /* Parse in strict mode and emit "use strict" for each source file. */

    /* Additional Checks */
    // "noUnusedLocals": true,                      /* Report errors on unused locals. */
    // "noUnusedParameters": true,                  /* Report errors on unused parameters. */
    // "noImplicitReturns": true,                   /* Report error when not all code paths in function return a value. */
    // "noFallthroughCasesInSwitch": true,          /* Report errors for fallthrough cases in switch statement. */
    // "noUncheckedIndexedAccess": true,            /* Include 'undefined' in index signature results */
    // "noPropertyAccessFromIndexSignature": true,  /* Require undeclared properties from index signatures to use element accesses. */

    /* Module Resolution Options */
    // "moduleResolution": "node",                  /* Specify module resolution strategy: 'node' (Node.js) or 'classic' (TypeScript pre-1.6). */
    // "baseUrl": "./",                             /* Base directory to resolve non-absolute module names. */
    // "paths": {},                                 /* A series of entries which re-map imports to lookup locations relative to the 'baseUrl'. */
    // "rootDirs": [],                              /* List of root folders whose combined content represents the structure of the project at runtime. */
    // "typeRoots": [],                             /* List of folders to include type definitions from. */
    // "types": [],                                 /* Type declaration files to be included in compilation. */
    // "allowSyntheticDefaultImports": true,        /* Allow default imports from modules with no default export. This does not affect code emit, just typechecking. */
    "esModuleInterop": true,                        /* Enables emit interoperability between CommonJS and ES Modules via creation of namespace objects for all imports. Implies 'allowSyntheticDefaultImports'. */
    // "preserveSymlinks": true,                    /* Do not resolve the real path of symlinks. */
    // "allowUmdGlobalAccess": true,                /* Allow accessing UMD globals from modules. */

    /* Source Map Options */
    // "sourceRoot": "",                            /* Specify the location where debugger should locate TypeScript files instead of source locations. */
    // "mapRoot": "",                               /* Specify the location where debugger should locate map files instead of generated locations. */
    // "inlineSourceMap": true,                     /* Emit a single file with source maps instead of having a separate file. */
    // "inlineSources": true,                       /* Emit the source alongside the sourcemaps within a single file; requires '--inlineSourceMap' or '--sourceMap' to be set. */

    /* Experimental Options */
    // "experimentalDecorators": true,              /* Enables experimental support for ES7 decorators. */
    // "emitDecoratorMetadata": true,               /* Enables experimental support for emitting type metadata for decorators. */

    /* Advanced Options */
    "skipLibCheck": true,                           /* Skip type checking of declaration files. */
    "forceConsistentCasingInFileNames": true        /* Disallow inconsistently-cased references to the same file. */
  }
}

~~~
<br />

Observe que ele já vem com muitas opções, contudo comentadas. As opções que queremos a *"outDir"* , onde especificaremos o destino dos arquivos executados com o tsc. Basicamente é onde os arquivos javascript ficarão depois de os convertermos do typescript. Escolha a pasta que quiser com destino.

Agora executamos o tsc apenas com o comando tsc sem qualquer argumento.

~~~ shell

$ npx tsc

~~~
<br />

Vamos instalar agora o sucrase. O sucrase será nossa ferramento de desenvolvimento para a conversão do typescript para o javascript. O motivo de usar ele e não o tsc, é por duas razões: Primeiro ele é mais rápido; Segundo ele não checa todos os detalhes do código typescript, como erros de digitação ou tipo, o que será compensado com o nosso eslint depois, nos dando agilidade no desenvolvimento. Tsc é aceitável na fase de build, mas não na fase de desenvolvimento.

~~~ shell


~~~
<br />

Instalamos também o nodemon para rodarmos um servidor de desenvolvimento que se atualiza a qualquer mudança no código.

~~~ shell


~~~
<br />

Usaremos o nodemon para executar nosso código on live, no entanto o sucrase precisa ser executado antes, pois não temos como executar typescript no nodemon. Então devemos configurar um arquivo que diga ao nodemon como tratar typescript e como o sucrase se envolve. Então criamos o arquivo de configuração **nodemon.json**.

~~~ javascript

{
    "watch": ["src"],
    "ext": "ts",
    "execMap": {
        "ts": "sucrase-node src/server.ts"
    }
}

~~~
<br />

Para executar de forma fácil o nodemon vou acrescentá-lo ao packet json e aproveito também acrescento um comando de build com o tsc.

~~~ javascript

{
    // .....

    "scripts": {
    "dev": "nodemon src/server.ts",
    "build": "tsc"
    },

    // .....
}

~~~
<br />


Agora utilizaremos o eslint. Se você está lembrando do que mencionei acima então sabe que sem o tsc precisamos ter cuidado para que na hora do desenvolvimento algum erro não passe despercebido. Usando o eslint podemos evitar muitos erros, ainda mais se definimos um style guide condizente. 


~~~ shell


~~~
<br />


Para que o eslint funcione no seu editor, seria bom instalar seu plugin, caso seu editor tenha tal opção. Aqui vou assumir que você utilizará o *visual studio code*, caso não esteja, talvez ainda seja possível estabelecer tal configuração, mas já deixo recomendado que troque para o visual studio code.

Para o eslint precisaremos de alguns plugins a mais, em consequência do fato de estarmos usando typecript.

~~~ shell


~~~
<br />


Feito tudo acima, precisamos agora definirmos as configuraçõe do eslint e isso é feito executando o comando abaixo. 

~~~ shell


~~~
<br />


Você ficará surpreso, se nunca instalou o eslint, que ele trará um prompt de perguntas e opções para ser respondida.

**imagem pergunta check eslint**

Escolha a opção *Check syntax*, problems and enforce code style, se você quiser uma checagem mais completa do código, o qual recomendo para evitar o erros durante o desenvolvimento.

**imagem o tipo de import**

Escolha a opção com *import/export*, se estriver utilizando tais formas de carregar arquivos em código, senão escolher outras opções, Contudo como estamos usando sucrase é obvio que estaremos usando a primeira opção.

**imagem do framework**

A próxima pergunta é uma resposta óbvia, como estamos usando NodeJs e nada mais, recomenda-se a opção *None of these*.

**imagem typescript uso**

Digite *Y* para indicar que estamos usando typescript.

**imagem browser ou node**

Use o botão de espaço para indicar onde utilizaremos o código. Como estamos se focando somente do backend  recomendo marcar somente o *Node*.

**imagem opções style guide**

**imagem opção Airbnb**

Agora vemos algo interessante, ele pergunta se queremos utilizar um popular style guide, eu recomendo que escolha sim e opte pelo do *Airbnb* ou se quiser o default. Se quiser você pode ver como esses guias funcionam. Style guide Airbnb [https://github.com/airbnb/javascript](https://github.com/airbnb/javascript). Esses guias possibilitarão que seu código tenha um padrão de escrita a ser seguido tornando-o legível e coerente.

**imagem Javascript**

Concluindo o questionário do prompt, definimos como o arquivo criado, e que poderá posteriormente ser modificado, será formatado. Eu prefiro o formato Javascript, mas você escolhe o que quiser. Então teremos nosso arquivo criado. No entanto ainda não terminamos, precisamos inserir algumas “coisinhas” no aquivo.

~~~ javascript

{
  "env": {
    "es2021": true,
    "node": true
  },
  "extends": [
    "airbnb-base",
  ],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": 12,
    "sourceType": "module"
  },
  "plugins": ["@typescript-eslint"],
}

~~~
<br />



Alguns dos plugins que instalamos para o eslint já foram absorvidos pelo mesmo, então só iremos inserir as recomendações para o plugin *@typescript-eslint*, usaremos a opção recomended, pois ela automaticamente irá nos disponibilizar configurações para o arquivo de forma confiável, sem que tenhamos que inserir mais coisas no eslint.

~~~ javascript

{
  "env": {
    "es2021": true,
    "node": true
  },
  "extends": [
    "airbnb-base",
    "plugin:@typescript-eslint/recommended", // adicionamos essa parte
  ],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": 12,
    "sourceType": "module"
  },
  "plugins": ["@typescript-eslint"],
}

~~~
<br />


Infelizmente existem algumas configurações no eslint que podem mais atrapalhar que ajudar, algumas vão do gosto de cada um, contudo para mim essas foram as regras aos quais quis me livrar.


~~~ javascript
{
  "env": {
    "es2021": true,
    "node": true
  },
  "extends": [
    "airbnb-base",
    "plugin:@typescript-eslint/recommended",
  ],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": 12,
    "sourceType": "module"
  },
  "plugins": ["@typescript-eslint"],
  "rules": {
    "no-console": "off",
    "class-methods-use-this": "off",
  }
}

~~~
<br />


As regras *“no-console”* impede que eu tenha que ter o comando *console.log()* como error no código e a regra *“class-methods-use-this”*, impede que eu seja obrigado a sempre usar *this* em métodos, ou que ele acuse erros ao não usá-lo.

Uma situação que ocorreu comigo foi a não identificação de certos erros ao instalar o plugin prettier  para podermos utilizar o eslint com o prettier, para antever isso é bom adicionar as seguintes opções abaixo no .eslintrc :

~~~ javascript
{
  "env": {
    "es2021": true,
    "node": true
  },
  "extends": [
    "airbnb-base",
    "plugin:@typescript-eslint/recommended",
    "plugin:prettier/recommended"
  ],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": 12,
    "sourceType": "module"
  },
  "plugins": ["@typescript-eslint"],
  "rules": {
    "no-console": "off",
    "class-methods-use-this": "off",
    "import/extensions": [
      "error",
      "ignorePackages",
      {
        "js": "never",
        "jsx": "never",
        "ts": "never",
        "tsx": "never"
      }
    ]
  },
  "settings": {
    "import/resolver": {
      "node": {
        "extensions": [".js", ".jsx", ".ts", ".tsx"],
        "paths": ["."]
      }
    }
  }
}

~~~
<br />


Agora para inserirmos o prettier no ambiente, você deve ter o prettier extensão instalado , no visual code isso se torna uma tarefa simples, e deve baixar o seguinte plugin:

~~~ shell

~~~
<br />


A função do prettier é meramente estética, ele possibilitará um padrão de tabulação e espaços entre as linhas de código, e com o uso do eslint você poderá ajustar isso apenas com o ato de salvar o arquivo. Os plugins baixados permitem exatamente que o prettier trabalhe com o eslint. Na realidade o eslint estará usando o prettier, mas para isso dá certo precisamos configurar algumas coisas no arquivo de configuração do eslint, o .eslintrc.

Aqui só precisaremos inserir *"plugin:prettier/recomended”* na opção “extends” do arquivo, mas que fique claro que assim como a opção recomended do typescript, o do prettier já disponibiliza para nós uma configuração para o arquivo padrão, senão sem o qual teríamos que adicionar mais informações extras no arquivo.

~~~ javascript
{
  "env": {
    "es2021": true,
    "node": true
  },
  "extends": [
    "airbnb-base",
    "plugin:@typescript-eslint/recommended",
    "plugin:prettier/recommended"
  ],

  // .........

}

~~~
<br />


Enfim, é recomendado criar um arquivo **.prettierrc** para podermos definir as configurações que o prettier irá aplicar ao salvarmos nossos arquivos.

~~~ javascript

module.exports = {
    semi: true,
    trailingComma: "all",
    singleQuote: true,
    printWidth: 120,
    tabWidth: 4
  };

~~~
<br />


Terminamos de criar um ambiente que nos ajudará a desenvolver código nodejs de forma eficiente e  única. Isso pode, ou não, torná-lo um programador mais eficiente, no entanto é meio de evitarmos erros de digitação e facilitar a codificação em typescript. De qualquer forma recomendo o teste do ambiente e veja se ele atende todas as suas necessidades.
