---
layout: post
title: "Filters customizáveis"
date: 2019-08-26
categories:
  - Javascript
description:
image: https://source.unsplash.com/2xU7rYxsTiM/2000x1200
image-sm: https://source.unsplash.com/2xU7rYxsTiM/500x300
---

Quando se trata de Filters em angularJs o mesmo já oferece um belo arssenal para lhe ajudar em seus projetos, contudo é limitado. A limitação se dá pela pouca flexibilização de uso em seu código. Mas o próprio framerwork reconhece essa necessidade e por isso oferece para qualquer programador a possibilidade de criar sua propria resposta para aquele problema pertinente. Esses são os filters customizáveis, ou seja, filters que não pertence ao framerwork criado pelo proprio desenvolvedor para sua necessidade. Abaixo irei explicar a sua criação e seu uso em código.

Para começar, filters são oferecidos em pelo proprio framerwork são os bult-in e seu uso se dá em templates, services, controllers e directivas. O mesmo pode ser feito com filters customizáveis, ou seja não existe diferença entre filters customizáveis e bult-in, desde que faça sentido seu uso em código. Na realidade existe sim uma diferença entre filters bult-in e customizáveis, um você tem controle do seu código, o outro não.

Filters customizáveis são simples de criarem, basicamente usaremos de **function factory**. Function factory são funções que criam para nós um objeto ou uma função. Nesse caso especifico criaremos um function factory que nos entrega uma função.

**codigo da factory**

</br>
Na nossa função emtregue pela factory é obrigatório que tenhamos um argumento, outros seriam opcionais. O argumento obrigatório é o valor que iremos filtrar, portanto essencial caso queiramos usar o filter posteriormente. O uso de filter e sua associação com o primeiro argumento ficará claro depois. O segundo argumento não é obrigatório e como o primeiro, será repassado através da chamada ao filter.

Detalhe a função criada deve ser uma **"pure function"**, o mesmo input deve responder ao mesmo output, então nada de genarates, etc.

Com a function factory em mãos iremos acrescentá-los ao método filter do **module** no angularjs, onde os agumentos serão o nome do filter e a entrega de sua function factory. Outra atenção, o nome do filter não deve possuir hyphen ou pontos, para tal utilize o camel case. Nome simples são os melhores.

**código da criação do filter**

</br>
Agora que criamos nosso filer é só usá-lo aonde queremos. Seu uso em controller é direto, declarmos sua dependência, nesse caso estamos utilizando array annotation para a DI(denpendcy injecton). Dentro do controller interpretamos o filter como uma function que pode retornar um novo array ou mesmo uma string.

**Codigo do controller aqui**

</br>
Também é possivel como os demais filters utilizá-lo em templates html. Lembrando que usamos o formato { expressão | filter: option }, Contudo vale uma resalva, filters entendem que a expressão está relacionada ao primeiro argumento de nosso filter. O segundo, ou outros argumentos provem de option intercaldos com dois pontos, para cada argumento após o primeiro.

**Código em tamplate html.**

</br>
Entregamos uma expressão que pode ser um valor primitivo ou objeto, e usamos o nome de nossa função para essa chamada.

Uma observação, é meio que cómico mas extremamente útil. em templates o nome de filter é exatamente o que declaramos no método constructor filter() do module. No entanto o método adiciona a palavra filter no final da função, isso claramente só é contado quando assinamos os controllers com filters, algo que se torna obrigatório se queremos o filter em nosso código. Repetindo a exceção é somente no template. Sobre ser util, é que com a necessidade do sufixo 'filter' dificelmente alguém pode se esquecer que tal função é um filter.

Enfim com a criação de nossos próprios filters, temos toda a flexibilidade que necessitamos para a soluções de problemas e manipulação de dados. Eu mesmo em um artigo aqui enfatizei como eles são importantes.

Fonte: [https://docs.angularjs.org/guide/filter](https://docs.angularjs.org/guide/filter)
