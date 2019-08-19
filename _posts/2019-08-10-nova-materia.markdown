---
layout: post
title: "Filters no AngularJs"
date: 2019-08-19
categories:
  - Javascript
description:
image: https://source.unsplash.com/2xU7rYxsTiM/2000x1200
image-sm: https://source.unsplash.com/2xU7rYxsTiM/500x300
---

Filters era para mim quando comecei a utilizar o framework AngularJs, desinteressantes, porém hoje as considero uma das funcionalidades mais relevantes do AngularJs, principalmente quando você tem de lidar com listas de dados requisitados de uma API. A Partir disso deixe-me compartilhar suas funcionalidades.

Em sua definição Filters formatam o valor de uma expressão de modo a ser exibida adequadamente ao usuário. O próprio AngularJs possui filters bult-in, próprios do framework. No entanto é possível criarmos os nossos próprios filters e podemos utilizá-los em templates html, dentro de classes controller ou mesmo services.

Para o uso em templates é bem simples, ainda que na minha opinião não seja seu melhor uso. Em templates é utilizado para expressões e outros filters. Para seu uso utilizamos duas chaves {{}} contornado a declaração e seu valor:

  ~~~ javascript

    //  {{ expression | filter }}

  ~~~
<br>

 Onde expression é qualquer valor ou variável, e filter seria tanto um bult-in quanto um criado por nós mesmos. Observe que ele está dentro de chaves, a expressão e o filter, porém separados por uma barra. Essa barra é importante para separar expressão e filter e fazer o filter compreender que a expressão é um argumento passado a ele. Um exemplo prático disso está abaixo:

 ~~~ javascript

    //  {{ 12 | currency }}

 ~~~
<br>

O valor '12' é alterado pelo 'currency', que é o nosso filter, retornando um novo valor. O valor retornado é uma formatação para valor de moeda '$12.00'. Mas preste atenção, filters não se tratam de apenas alterar um valor, eles são como o próprio nome indica "filtradores", então podemos usá-los para "filtrar" valores que atendam a uma especificidade. Esses valores serão exibidos ou alterados dependendo do filter usado.

Filters podem serem aplicados em resultados de outros filters. Isso é chamado de 'chaining', no caso estamos aninhando filters. A expressão segue abaixo.

 ~~~ javascript

      // {{ expression | filter1 | filter2 | ... }}

 ~~~
<br>

O número de filters chaining pode variar de dois ou mais. Filters também podem possuir argumentos mesmo em template html, e seu número também pode variar.

~~~ javascript

    // {{ expression | filter:argument1:argument2:... }}

~~~
<br>

Abaixo executamos o mesmo código que formata o valor '12' para moeda, contudo agora com um segundo argumento definindo a quantidade de casas decimais. Neste caso utilizamos dois pontos para separar os argumentos. O código retorna '$12.00'.


~~~ javascript

      // {{ 12 | currency:'$':'2' }}

~~~
<br>

#### Filters em Controllers e services:
Para usar filters dentro de services e Controllers, precisamos primeiramente declará-los como dependência. Para declarar um filter, seja ele bult-in ou não do framework, basta fazer DI (dependency injection). E então eles serão repassados como argumentos dentro do próprio código.

Abaixo temos um código bem divertido que lida com um grupo de objetos pokémon, dos quais eu quero filtrar somente pokémon do tipo fogo:

~~~ javascript

      (function () {

        function FilterContorller(filterFilter) {
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

          this.pokemonFiltrados = filterFilter(this.pokemon, 'Fogo');
        }

        angular.module('myApp', [])
          .controller('FilterContorller', ['filterFilter', FilterContorller]);

      })()

~~~
<br>

Declaramos o filter como argumento do nosso controller, e o utilizamos para filtrar os objetos do array, criando assim uma nova lista. Com essa nova lista publicamos os dados.

Veja que poderíamos filtrar essa mesma lista usando o filter na diretiva ng-repeat do template, contudo isso seria custoso e se tornaria extremamente trabalhoso se quiséssemos mudar. Usando o filter no controller temos total poder sobre a lista de dados usada e nosso código fica mais condizente com a divisão de responsabilidades do framework. Sinceramente eu prefiro um código com suas responsabilidades bem definidas.

Abaixo seu uso no html onde usamos o ng-repeat para trabalhar com somente a lista filtrada, que pode ser alterada quando quisermos e de diversas formas dentro do controller através de outros filters.

~~~ html

      <body ng-app="myApp">
        <div ng-controller="FilterContorller as $ctrl">
          <p ng-repeat="pokemon in $ctrl.pokemonFiltrados">
            {{ pokemon.nome }} - Tipo: {{ pokemon.tipo }}
          </p>
        </div>
      </body>

~~~
<br>

No uso de services realizamos os mesmos passos: declaramos o filter como dependência e o usamos para filtrar os valores da lista. No código usamos um método para alterar o valor e retorná-lo.

~~~ javascript

      function FilterService(filterFilter) {
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


        this.filtrePokemonPorTipo = function filtrePokemonPorTipo(tipo) {
          this.pokemonFiltrados = filterFilter(this.pokemon, tipo);
          return this.pokemonFiltrados;
        }
      }

      angular.module('myApp', [])
        .service('FilterService', ['filterFilter', FilterService])
        .controller('FilterContorller', ['FilterService', FilterContorller]);

~~~
<br>

É possível usar filters não somente em services e Controllers, mas também em directivas.
Filters são uma mão na roda, principalmente se você quer praticidade, manipular dados se tornou uma tarefa bastante rotineira ao programar hoje, então por que não deixá-la simples?

Fonte: (https://docs.angularjs.org/guide/filter)
