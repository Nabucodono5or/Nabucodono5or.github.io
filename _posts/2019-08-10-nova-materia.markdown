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
partes mais negligenciadas do meu estudo, porém hoje as considero uma das funcionalidades
mais relevantes do AngularJs, pricipalmente quando você tem de lidar com listas de
dados requisitados de uma API Rest.

Em sua definição Filters formatam o valor de uma expressão de modo a ser exibida adequadamente
ao usuário. O próprio AngularJs possui filters bult-in, próprios do framework, no entanto é possível criarmos os nossos próprios filters e podemos utiliza-los em templates html, dentro de classes controller ou mesmo services.

Para o uso em templates é bem simples, ainda que na minha opnião não seja seu melhor uso. Em
templates é utilizado para expressões e outros filters. Para seu uso utilizamos o duas chaves {{}} contornado a declaração e seu valor:

 *código {{ expression | filter }}*

 Onde expression é qualquer valor ou variável e filter seria tanto um bult-in quanto um criado por nós mesmos. Observe que ele é dentro das chaves e a espressão e o filter são separados por uma barra. Um exemplo prático disso está abaixo:

 *código {{ 12 | filterqualquer }}*

  O valor '12' é alterado pelo 'filterqualquer', que é o nosso filter, retornando o mesmo valor contudo alterado. Mas preste atenção, filters não se trata de apenas alterar valores, eles são como o próprio nome indica filtradores, então podemos usá-los para exiber seomente valores que atendem a uma especificidade.

 *Resultado do filter*

 Filters podem serem aplicados em resultados de outros filters. Isso é chamado de 'chaining', no caso estamos aninhando filters.

*{{ expression | filter1 | filter2 | ... }}*

 *Exemplos abaixo de chaining*

O número de filters chaining pode variar de um ou mais. Filters também podem possuirem argumentos mesmo em template html, e seu numero também podem variar.

*{{ expression | filter:argument1:... }}*

Neste caso utilizamos os pontos para separar os argumentos.

*exemplo de filter com argumentos*
*Resultado*

Filters em Controllers e services:
