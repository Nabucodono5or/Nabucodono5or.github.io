---
layout: post
title: "Filters no AngularJs"
date: 2019-08-11
categories:
  - Javascript
description:
image: https://source.unsplash.com/user/markusspiske/AaEQmoufHLk/2000x1200
image-sm: https://source.unsplash.com/user/markusspiske/AaEQmoufHLk/500x300
---
Filters era para mim quando comecei a utilizar o framework AngularJs, uma das
partes menos interessantes do meu estudo, porém hoje as considero uma das funcionalidades
mais relevantes do AngularJs, pricipalmente quando você tem de lidar com listas de
dados requisitados de uma API. Apartir disso deixe-me compartilhar suas funcionalidades.

Em sua definição Filters formatam o valor de uma expressão de modo a ser exibida adequadamente
ao usuário. O próprio AngularJs possui filters bult-in, próprios do framework. No entanto é possível criarmos os nossos próprios filters e podemos utiliza-los em templates html, dentro de classes controller ou mesmo services.

Para o uso em templates é bem simples, ainda que na minha opnião não seja seu melhor uso. Em
templates é utilizado para expressões e outros filters. Para seu uso utilizamos duas chaves {{}} contornado a declaração e seu valor:

 *código {{ expression | filter }}*

 Onde expression é qualquer valor ou variável, e filter seria tanto um bult-in quanto um criado por nós mesmos. Observe que ele está dentro de chaves, espressão e filter, porém separados por uma barra. Essa barra é importante para separar expressão e filter e fazer o filter compreender que a expressão é um argumento passado a ele. Um exemplo prático disso está abaixo:

 *código {{ 12 | filterqualquer }}*

  O valor '12' é alterado pelo 'filterqualquer', que é o nosso filter, retornando um novo valor. Mas preste atenção, filters não se tratam de apenas alterar um valor, eles são como o próprio nome indica "filtradores", então podemos usá-los para "filtrar" valores que atendam a uma especificidade. Esses valores serão exibidos ou alterados dependendo do filter usado.

 *Resultado do filter*

 Filters podem serem aplicados em resultados de outros filters. Isso é chamado de 'chaining', no caso estamos aninhando filters.

*{{ expression | filter1 | filter2 | ... }}*

 *Exemplos abaixo de chaining*

O número de filters chaining pode variar de dois ou mais. Filters também podem possuirem argumentos mesmo em template html, e seu numero também pode variar.

*{{ expression | filter:argument1:argument2:... }}*

Neste caso utilizamos dois pontos para separar os argumentos.

*exemplo de filter com argumentos*
*Resultado*

Filters em Controllers e services:
Para usar filters dentro de services e Controllers, precisamos primeiramente declará-los como dependência. Para declarar um filter, seja ele bult-in ou não do framework, basta fazer DI (dependency injection). E então eles serão repassados como argumentos dentro do próprio código.

*exemplo de codigo controller com um filter*

No código acima declaramos o filter como argumento do nosso controller, e o utilizamos para filtrar (array ou objetos) em uma nova lista de valores definidos pelo filter e seu argumento. No caso acima seria ...(explicação do qu estou filtrando).

Veja que poderiamos filtrar essa mesma lista usando o filter na directiva ng-repeat do template, contudo isso seria custoso e se tornária extremamente trabalhoso se quiséssemos mudar. Usando o filter no controller temos total poder sobre a lista de dados usada e nosso código fica mais condigente com a divisão de responsábilidades do framework. Sinceramente eu prefiro um código com suas responsábilidades bem definidas.

Abaixo seu uso no html onde usamos o ng-repeat para trabalhar com somente uma lista, que pode ser alterada quando quisermos e de diversas formas dento do controller através de outros filters.

*código html*

No uso de services realizamos os mesmos passos: declaramos o filter como dependencia e o usamos para filtrar os valores da lista. No código usamos um método para alterar o valor e retorná-lo.

*código abaxo de um service*

É possível usar filters não somente em services e Controllers, ele tamém é usável em directivas.
Filters são uma mão na roda principalmente se você quer práticidade, manipular dados se tornou uma tarefa bastante rotineira ao programar hoje, então por que não deixá-la simples?
