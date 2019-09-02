---
layout: post
title: "Filters customizáveis"
date: 2019-09-02
categories:
  - Javascript
description:
image: https://source.unsplash.com/WkfDrhxDMC8/2000x1200
image-sm: https://source.unsplash.com/WkfDrhxDMC8/500x300
---

Quando se trata de Filters em angularJs o mesmo já oferece um belo arsenal para lhe ajudar em seus projetos, contudo é limitado. A limitação se dá pela pouca flexibilização de uso em seu código. Mas o próprio framerwork reconhece essa necessidade e por isso oferece para qualquer programador a possibilidade de criar sua própria resposta para aquele problema pertinente. Esses são os filters customizáveis, ou seja, filters que não pertence ao framerwork criado pelo próprio desenvolvedor para sua necessidade. Abaixo irei explicar a sua criação e seu uso em código.

Para começar, filters são oferecidos em pelo próprio framerwork são os bult-in e seu uso se dá em templates, services, controllers e directivas. O mesmo pode ser feito com filters customizáveis, ou seja não existe diferença entre filters customizáveis e bult-in, desde que faça sentido seu uso em código. Na realidade existe sim uma diferença entre filters bult-in e customizáveis, um você tem controle do seu código, o outro não.

Filters customizáveis são simples de criarem, basicamente usaremos de **function factory**. Function factory são funções que criam para nós um objeto ou uma função. Nesse caso específico criaremos um function factory que nos entrega uma função. Essa função será um filter para selecionar pokémon por tipo, (sim, eu quero filtrar pokemon).

~~~ javascript


    function filterFactoryFogo() {
      return function(lista, tipo) {
        let out = [];
        let tipo = tipo || '';
        for(let pokemon of lista){
          if (pokemon.tipo == tipo) out.push(pokemon);
        }

        return out;
      }
    }


~~~

</br>
Na nossa função entregue pela factory é obrigatório que tenhamos um argumento, outros seriam opcionais. O argumento obrigatório é o valor que iremos filtrar, portanto essencial caso queiramos usar o filter posteriormente. O uso de filter e sua associação com o primeiro argumento ficará claro depois. O segundo argumento não é obrigatório e como o primeiro, será repassado através da chamada ao filter. Mas nesse caso o nosso segundo argumento acaba sendo importante.

Detalhe a função criada deve ser uma **"pure function"**, o mesmo input deve responder ao mesmo output, então nada de generate, etc.

Com a function factory em mãos iremos acrescentá-los ao método filter do **module** no angularjs, onde os argumentos serão o nome do filter e a entrega de sua function factory. Outra atenção, o nome do filter não deve possuir hyphen ou pontos, para tal utilize o camel case. Nome simples são os melhores.


~~~ javascript

      angular.module('myApp', [])
        .filter('pokemonTipo', filterFactoryFogo)
        .service('FilterService', ['pokemonTipoFilter', FilterService])
        .controller('FilterContorller', ['FilterService', FilterContorller]);

~~~

</br>
Agora que criamos nosso filter é só usá-lo aonde queremos. Seu uso em controller é direto, declarem sua dependência, nesse caso estamos utilizando array annotation para a DI(denpendcy injecton). Dentro do controller interpretamos o filter como uma function que pode retornar um novo array ou mesmo uma string. Temos abaixo três listas criadas baseadas no que filtramos. Também utilizamos um service para separação de código.

~~~ javascript

      function FilterContorller(FilterService) {
        this.pokemonFogo = FilterService.pokemonTipoFogo();
        this.pokemonAgua = FilterService.pokemonTipoAgua
        this.pokemonPlanta = FilterService.pokemonTipoPlanta;
      }

      function FilterService(pokemonTipoFilter) {
        this.pokemon = [
          {
            nome: 'Chamander',
            tipo: 'Fogo',
          },
          {
            nome: 'Bulbasaur',
            tipo: 'Planta',
          },
          {
            nome: 'Squirtle',
            tipo: 'Água',
          },
          {
            nome: 'Arcanaine',
            tipo: 'Fogo',
          }
        ];

        this.pokemonTipoFogo = function () {
          let pokemonFiltrados = pokemonTipoFilter(this.pokemon, 'Fogo');
          return pokemonFiltrados;
        };

        this.pokemonTipoAgua = function () {
          let pokemonFiltrados = pokemonTipoFilter(this.pokemon, 'Água');
          return pokemonFiltrados;
        };

        this.pokemonTipoPlanta = function () {
          let pokemonFiltrados = pokemonTipoFilter(this.pokemon, 'Planta');
          return pokemonFiltrados;
        };

      }

~~~

</br>
Também é possível como os demais filters utilizá-lo em templates html. Lembrando que usamos o formato { expressão | filter: option }, Contudo vale uma ressalva, filters entendem que a expressão está relacionada ao primeiro argumento de nosso filter. O segundo, ou outros argumentos provém de option intercalados com dois pontos, para cada argumento após o primeiro.
Aqui só estamos listando os pokémon do tipo fogo para contexto de exemplo, mas poderíamos acessar qualquer lista, criada no controller.


~~~ html

      <body ng-app="myApp">
        <div ng-controller="FilterContorller as $ctrl">
          <p ng-repeat="pokemon in $ctrl.pokemonFogo">
            {{ pokemon.nome }} - Tipo: {{ pokemon.tipo }}
          </p>
        </div>
      </body>

~~~

</br>
Entregamos uma expressão que pode ser um valor primitivo ou objeto, e usamos o nome de nossa função para essa chamada. Resultado abaixo:

```
      Chamander - Tipo: Fogo

      Arcanaine - Tipo: Fogo

```
</br>
Uma observação, é meio que cómico mas extremamente útil. em templates o nome de filter é exatamente o que declaramos no método constructor filter() do module. No entanto o método adiciona a palavra filter no final da função, isso claramente só é contado quando assinamos os controllers com filters, algo que se torna obrigatório se queremos o filter em nosso código. Repetindo a exceção é somente no template. Sobre ser útil, é que com a necessidade do sufixo 'filter' dificilmente alguém pode se esquecer que tal função é um filter.

Enfim com a criação de nossos próprios filters, temos toda a flexibilidade que necessitamos para a soluções de problemas e manipulação de dados. Eu mesmo em um artigo aqui enfatizei como eles são importantes.

Fonte: [https://docs.angularjs.org/guide/filter](https://docs.angularjs.org/guide/filter)
