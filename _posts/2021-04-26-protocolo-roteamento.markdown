---
layout: post
title: "Um pouco sobre protocolos de roteamento"
date: 2021-04-26
categories:
  - Redes
description:
image: https://source.unsplash.com/user/jdiegoph/Pihl8kTtX-s/2000x1200
image-sm: https://source.unsplash.com/user/jdiegoph/Pihl8kTtX-s/500x300
---


Hoje inicio uma série de artigos sobre protocolos de rede. A razão para isso é que nesse exato momento estou estudando muitos dos conceitos de rede  e achei que por ventura seria interessante compartilhar as minhas descobertas. Ainda continuarei publicando sobre programação, porém com menor frequência.

Para começar irei abordar o papel do protocolo no roteamento de endereços, mas antes de mais nada vamos entender o processo de roteamento de um packet (ou pacote).

<br />
### Roteamento de Pacotes

O roteamento de pacote é como o seu router (roteador) entrega um dado ou informação enviado/recebido por você através da internet, ou mesmo dentro de uma rede corporativa seguementada em pequenas redes.

Roteadores não se importam com hosts (computadores, servidores, impressoras, etc) apenas com redes e o melhor meio de alcançar essa rede.

Tente entender que redes privadas ou LAN’s se comunicam usando o endereço MAC, o endereço da sua placa NIC, placa de rede, como uma forma de identificar o host fisicamente na rede. Isso é feito através de outro protocolo, o ARP, o que não é o foco deste artigo.
Se sua rede não possuir roteador então você não está “roteando” pacotes de redes, transferindo informação para outras redes. Pacotes seriam os dados enviados através da rede. 

Para ser capaz de transferir um pacote para outras redes um router deve saber no mínimo a rede destino, conhecer o endereço do roteadores vizinhos dos quais o pacote irá atravessar até seu destino e consequentemente o maior número de roteadores possível, o melhor caminho entre esses roteadores e como armazenar as informações desses roteadores vizinhos e atualiza-las.

Então onde todas essas informações são aprendidas e administradas pelo router? E como os protocolos entram nessa jogada?
Primeiro de tudo um roteador aprende sobre a rede através de justamente seus routers vizinhos, mas ele só consegue tal feito justamente através dos protocolos de roteamento. São eles que determinarão como tais informações serão compartilhadas entre roteadores. Usando um protocolo de roteamento o router cria o chamamos de routing table, uma tabela onde indicamos em quais redes e routes se encontra a rede destino.

Contudo, tenha atenção, se uma rede é diretamente conectada a uma das interfaces do router ela não precisará ser “descoberta”, o router já saberá de sua localização, portanto sem necessidade de protocolos de roteamento.

<br />

### Protocolos de Routing

Os protocolos de routing fazem parte da configuração dinâmica, se configurássemos manualmente, outra forma de lidar com roteamento, teriamos que inserir cada endereço de rede dentro de cada router, isso para uma rede corporativa média, é no mínimo exaustivo.

A vantagem do roteamento dinâmico, além de evitar um trabalho exaustivo, é que se uma mudança ocorre na topologia da rede, a inserção ou remoção de algum roteador por exemplo, cada um dos roteadores conectado a rede é atualizado.

Para usarmos o roteamento dinâmico precisaremos dos nossos queridos protocolos, e para entendermos seu uso, precisaremos classificá-los.
Vamos dividi-los em interior e exterior gateway, IGP e EGP respectivamente. A diferença entre eles é que onde eles trabalham. EGP’s são protocolos usados fora de redes internas, como a de empresas. IGP’s por sua vez são usados internamente.

Como protocolo EGP’s temos o BGP, como ele é mais complexo não irei abordá-lo, basta saber que seu papel é de fazer redes se conectarem através da internet. Sem ele a internet não seria o que é hoje, milhões de conexões ao mesmo tempo.

Para estudarmos o IGP, precisaremos subdividi-lo em outra duas categorias, Distance Vector Protocol e Link State Protocol.
Distance Vector protocolos definem o melhor caminho de roteamento através do números de hops, ou seja, o número de roteadores que se terá de percorrer até o seu destino. Sua principal caracterísitica é que ele compartilha as informações de roteamento com outros roteadores enviando sua tabela de roteamento. Roteadores esses sendo seus vizinhos imediatos, ou seja, os conectados diretamente  a ele.

Como protocolos de roteamento nós temos RIP, RIPv2 e IGRP.

Link State protocolos, ou Shortest Path First Protocol, usam três tabelas em vez de uma, A Uma  para armazenar informações de seus vizinhos diretos, outra  que determina a topologia da rede e a que corresponde a routing table dos outros protocolos de roteamento. Com essas três tabelas OSPF e IS-IS, compartilham informações com seus vizinhos roteadores. Suas atualizações é somente quando é detectado mudanças na rede.

Existe uma terceira categoria, mas não pretendo abordá-la com uma categoria em si, mas como uma união de características das duas acimas, ou como uma categoria para um protocolo que não pertença a nenhuma das anteriores, usando a definição da CompTIA a chamarei de Hybrid.

Um protocolo Hybrid seria o EIGRP.

Abaixo descrevo os protocolos mais relevantes de se conhecer:

**Routing information Protocol**, com a funcionalidade de enviar a routing table a cada 30 segundos e o uso de hops para determinar o melhor caminho, o RIP é limitado a 15 hops por envio. A razão para isso é para evitar problemas de routing loop. Ou seja lidar com redundância é seu ponto fraco. Mais do que isso, RIP é antigo, e só está sendo mencionado aqui porque ele ainda está pora ai. Além disso,  esse protocolo não consegue lidar com subnets e é extremamente lento.

**Routing information Protocol versão 2**, RIPv2, padrão aberto, ou seja, utilizável por qualquer roteador independente do fabricante. O RIPv2 continua sendo lento, contudo agora ele consegue lidar com subnet e não trabalha com o uso de broadcast para envio de atualizações, em vez, utiliza-se de multicast.

**Open Shortest Path First**, OSPF, Padrão aberto de protocolo de roteamento. Para determinar qual o melhor caminho de roteamento ele usa um algoritmo de árvore, consequentemente ele converge muito mais rápido, contudo para que tal conversão seja mais rápida mesmo é preciso estabelecer um design hierárquico de rede para ele trabalhar. Usa a banda como métrica de distância   e trabalha com subnets.

**Enhanced Internal Gateway Routing Protocol**, EIGRP, versão melhorada do antigo Internal  Gateway Routing Protocol, IGRP, ambos são proprietários Cisco, ou seja, só podem serem executados em equipamentos do fabricante. Como melhoria do IGRP ele traz suporte a subnets.
Possui caracterísitcas tanto do Link-State quanto do Vector Distance. Utiliza-se de três tabelas, atualiza somente quando há mudanças na topologia sendo que tais atualizações funcionam como na Vector Distance, toda a informação com relação a endereço e distancia é compartilhado com o adicional de custo para alcançá-las. È bem rápido comparado a outros protocolos, e se você está em uma rede com equipamentos Cisco, eu sinceramente recomendo que use este protocolo para roteamento e nenhum outro. Além das características mencionadas ele possui excelente algoritmo de descoberta de endereços.

<br />

### Na vida real

Na prática isso corresponde a a decidir qual a melhor opção para se trabalhar em uma rede interna. Através do acesso a roteadores é possível definir quais protocolos estarão habilitados, por exemplo para habilitarmos o protocolo xxx usamos o seguinte comando em um roteador cisco:

```

    R1>enable
    R1#configure terminal
    Enter configuration commands, one per line.  End with CNTL/Z.
    R1(config)# router ospf 10
    R1(config-router)#

```

<br />
Contudo, cabe um “Disclamer” aqui, você não vai querer habilitar todos os protocolos de roteamento em uma rede, a razão para não desejar isso é que no final das contas, não importando quais protocolos tenham sido habilitados, somente o protocolo com menor AD será atendido, ou no caso, o mais confiável. Os outros estarão alí apenas para arrancar processamento de seu equipamento, o que não é nada bom em uma rede que atenda a muitas requisições.

AD, ou Adiministrative Distance, define o quão confiável é a informação compartilhada, seu valor de avaliação varia de 0, sendo o mais confiável, a 255, nada confiável. Protocolos de roteamento são avaliadas dessa forma para situações em que tenham que decidir qual a melhor opção de informação sobre endereços a serem atendida.

Então jamais use mais de um protocolo de roteamento em rede, não há vantagens. Para terminar deixo a abaixo uma lista com todos as AD dos protocolos, envolvendo até mesmo a conexão direta e o roteamento estático. Observe que eles possuem maior procedência como fontes confiáveis.

|Protocolos ou fontes de roteamento  |AD
--- | ---
|interface diretamente conectada | 0
|roteamento manual | 1
|BGP (usado em rede externa) | 20
|EIGRP (rede interna) | 90
|IGRP | 100
|OSPF | 110
|IS-IS | 115
|RIP |120
|EIGRP (rede externa) | 170
|BGP (rede interna) | 200
|Desconhecido | 255
