---
layout: post
title: "Três meses de probabilidade e estatística (parte 3)"
date: 2020-12-28
categories:
  - Javascript
description:
image: https://source.unsplash.com/user/jdiegoph/aQfhbxailCs/2000x1200
image-sm: https://source.unsplash.com/user/jdiegoph/aQfhbxailCs/500x300
---

Aqui eu vou explicar como a estética se cruza com estatística. Quando lidamos com gráficos, esquecemos muitas vezes que esses gráficos não são para nós, mas para um público, muitas vezes específico ou não, considerar isso é talvez o ponto mais importante em uma apresentação de dados, e deveria ser considerado desde o início na hora de se optar por uma solução gráfica de dados.

Tendo em mente quem são nossos “interessados”, precisamos determinar o que queremos que eles vejam. Qual o ponto central que aquele gráfico quer nos alertar ou apenas nos mencionar. Tudo isso parece tedioso e longo, mas é na realidade simples, porém essencial se você quer usar gráficos. Apenas se pergunte o que você quer mostrar. Entender isso ajuda a saber que tipo de gráficos você vai usar, além de contar com cores para destacar as informações e legendas que realmente precisam aparecer no gráfico.

Falando em gráficos e cores, eu tive que reaprender o seu uso. Apesar de eu ter tentado aprender uma grande variedade de gráficos pelo d3js, descobrir que na maior parte das vezes somente um alguns poucos dos quais aprendi serão realmente utilizados no dia a dia. E gráficos como o piechart são extremamente enganadores.

<br />
	![Piechart](/imagem/piechart-min.jpeg)
<br />

Com piechart é difícil você identificar as proporções do dados somente olhando. No fim com o piechhart você não consegue entender o que realmente deveria. A solução nestes casos é o uso sempre de barchart (ou gráficos de barras).

<br />
	![Barchart](/imagem/barchart-min.png)
<br />

Barchart são fáceis de entender e mensurar. E por trabalhar com dados em categorias, você rapidamente identifica os tipos de dados exibidos. Fora, barchart nós temos gráficos em linha, scatterplot e gráficos em barra com pilha. Só com esses gráficos você responde cerca de 90% de sua necessidade diária de representação de dados. Podendo adicionar, ou não, o areachart também a esse grupo.

As cores em meus estudos passaram a ter uma relevância inacreditável na hora do uso de sua representação. Com o uso das cores posso realmente chamar atenção do público aonde eu quero que  eles foquem em meu gráfico. Basicamente destaco os pontos centrais das informações, aquilo que quero interpretar para você. Faço isso usando poucas cores direntes, em média duas ou três cores em um gráfico, e para destacar um valor, ou mais, acima dos outros uso de saturação.

Legendas também tem seu uso, para elas uso o conceito de aproximação, de Gestalt, e o mínimo possível de informações, não quero um gráfico bagunçado.

Emfim, gráficos 3d devem serem evitados, mas animações tornam gráficos vivos e flexíveis em sua exibição. Um linechart  pode exibir dois tipos de informações de cada vez, uma do mês anterior e outra do mês atual, bastando usar animações que reagem a uma iteração do usuário, por exemplo.

E assim terminei meus três meses de estudo envolvendo estatística e a biblioteca d3js. Foi bem satisfatório o número de informações aos quais eu adquiri e a necessidade de eu ter de retomar, e reformular, diversos conhecimentos, seja da época da minha primeira faculdade ou recentemente. E toda essa jornada valeu cada segundo, ainda que eu desejasse mais. Estou por satisfeito e agradeço por ter me acompanhado até aqui.
