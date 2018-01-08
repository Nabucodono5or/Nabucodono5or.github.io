---
layout: post
title: "Entendendo comandos aninhados em linux"
date: 2018-01-07
categories:
  - linux
description:
image: https://source.unsplash.com/user/jdiegoph/5LOhydOtTKU/2000x1200
image-sm: https://source.unsplash.com/user/jdiegoph/5LOhydOtTKU/500x300
---

Uma das coisa mais bacanas e eficientes quer dá para fazer em Linux é conectar comandos, ou seja você pode digitar comandos em sequência ou mesmo retirar dados de saída de um comando como entrada para outro comando.

Tudo isso é realizado através dos chamados meta caracteres que seriam os seguintes símbolos ( o pipe \| , os parênteses (  ) , o ampersand & , o ponto e virgula  ;  e os sinais de mais e de menos <  >), que são símbolos que representam significados especiais para o Linux.

Vou exemplificar os diversa situações desses caracteres:

{% highlight shell_session %}
  $ cat /etc/passwd | sort | less

{% endhighlight %}

O pipe pode ser usado para pegar o resultado de saída de um comando e concatená-lo com outros comandos, o exemplo acima usa o comando cat para copiar a entrada do arquivo no endereço etc/passwd e ordenar a saída com sort e finalmente dessa ordenação criar uma paginação usando o comando less.

O mais( > ) e menos( < ) são geralmente usados para direcionar o conteúdo de saída de comandos para algum arquivo ou algum arquivo para um comando, Um exemplo simples é o de baixo, onde uso o comando echo para escrever a palavra "vazio" como saída no terminal, contudo direciono o resultado para um arquivo que crio imediatamente:

{% highlight shell %}
  $ echo "vazio" > novo.txt

{% endhighlight %}


É possível alocar comandos de forma sequencial, basta colocar ponto e vírgula depois de cada comando.

{% highlight shell %}
  $ date; echo "vazio" > novo.txt; date

{% endhighlight %}

O comando acima realiza a mesma ação anterior exceto que agora uso do ponto virgula adcionar ações antes e depois do comando echo para pegar o tempo da tarefa. Aqui tá de forma bem simples, contudo caso você precise realizar uma tarefa mais longa, essa é uma boa forma de você capturar o tempo gasto na ação.


Com o & é possível deixar o comando no modo background, é bem útil para aqueles comandos bem longos, só lembre de deixar um comando para avisar o seu término como no exemplo abaixo, e fica de aviso, se você fechar o shell o comando será morto se não tiver terminado ainda.

{% highlight shell %}
  $ find /home | grep novo &

{% endhighlight %}

Eu cheguei a dizer que o pipe ele conecta ( concatena) o resultado de um comando ao outro, contudo se você está interessado em transformar a saída de um comando como o próprio argumento, talvez o melhor para você é o uso dos parênteses, basicamente o que eles fazem é transformar a saída em um argumento para o outro comando com as opções e tudo. Os parênteses são conhecidos como comandos de substituição.

{% highlight shell %}
  $ find (/home | grep novo)

{% endhighlight %}

Estou usando o resultado da ação dentro dos parenteses como argumento para o comando find. Logo o que está dentro dos parenteses é substituído pelo resultado em si da ação.

Com os meta caracteres a vinculação de diversos comandos como blocos com diferentes tipos de símbolos para retornar uma exigência direta, é excelente e extremamente útil, em vez de digitar comando um por um pode-se resumi-los em uma sequencia de comandos e ainda em background.

Bom espero que tenham desfrutado do artigo e até próxima.
