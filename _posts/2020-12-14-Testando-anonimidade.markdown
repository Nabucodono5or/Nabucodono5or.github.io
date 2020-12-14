---
layout: post
title: "Testando anonimidade com proxychains"
date: 2020-12-14
categories:
  - linux
description:
image: https://source.unsplash.com/user/jdiegoph/Q1p7bh3SHj8/2000x1200
image-sm: https://source.unsplash.com/user/jdiegoph/Q1p7bh3SHj8/500x300
---

Nesta semana estou postergando a publicação da terceira parte da minha jornada em data visualization nos últimos meses em consequência do que eu estava estudando essa semana, achei muito mais interessante compartilhar essa minha experiência no uso de proxychains do que qualquer outra coisa.

Com o intuito de testar os conceitos e ferramentas de OSINT, essa semana eu tentei melhorar meu conceito de anonimato na rede, ou pelo menos tentar entender um pouco mais da ideia. Para tal testei optei por usar o sistema operacional parrot, um sistema de cybersecurity based debian.

Como primeiro passo verifiquei se proxychains e tor service estavam instalados, e o parrot não decepcionou nesse aspecto. Então, logo em seguida comecei a alterar o arquivo de configuração do proxychains, aliais iremos voltar muitas vezes a esse arquivo, pois ele será nosso principal foco para usar o proxychains.

```sh

[nabucodonsor@parrot-virtual ~]$ sudo nano /etc/proxychains.conf

```

<br />
Arquivo proxychain:

```sh

# proxychains.conf  VER 3.1
#
#        HTTP, SOCKS4, SOCKS5 tunneling proxifier with DNS.
#

# The option below identifies how the ProxyList is treated.
# only one option should be uncommented at time,
# otherwise the last appearing option will be accepted
#
dynamic_chain
#
# Dynamic - Each connection will be done via chained proxies
# all proxies chained in the order as they appear in the list
# at least one proxy must be online to play in chain
# (dead proxies are skipped)
# otherwise EINTR is returned to the app
#
#strict_chain
#
# Strict - Each connection will be done via chained proxies
# all proxies chained in the order as they appear in the list
# all proxies must be online to play in chain
# otherwise EINTR is returned to the app
#
#random_chain
#
# Random - Each connection will be done via random proxy
# (or proxy chain, see  chain_len) from the list.
# this option is good to test your IDS :)

# Make sense only if random_chain
#chain_len = 2

# Quiet mode (no output from library)
#quiet_mode

Proxy DNS requests - no leak for DNS data
proxy_dns

# Some timeouts in milliseconds
tcp_read_time_out 30000
tcp_connect_time_out 16000

# ProxyList format
#       type  host  port [user pass]
#       (values separated by 'tab' or 'blank')
#
#
#        Examples:
#
#            socks5	192.168.67.78	1080	lamer	secret
#		     http	192.168.89.3	8080	justu	hidden
#	 	     socks4	192.168.1.49	1080
#	         http	192.168.39.93	8080
#
#
#       proxy types: http, socks4, socks5, raw
#        * raw: The traffic is simply forwarded to the proxy without modification.
#        ( auth types supported: "basic"-http  "user/pass"-socks )
#
[ProxyList]
# add proxy here ...
# meanwhile
# defaults set to "tor"
#socks4 	127.0.0.1 9050
socks5 	127.0.0.1 9050

```
<br />
Alterei deixei como conexão dinamica e alterei um pouco os tempos de resposta, devido a rede em que estou usando. Também deixei o dns ser mascarado, um erro que muitos que estão começando e querem testar anomidade deixam de considerar. E pra fechar adicionei o acesso ao endereço do tor com uso do protocolo socks5, que permite conexões tcp e udp de forma anonima.

Então após salvar o arquivo eu ativei o service tor.

```sh

[nabucodonsor@parrot-virtual ~]$ sudo service tor start

```
<br />
Então para teste eu usei o firefox. Normalmente você não quer navegar usando um desses browsers e eu explicarei depois, mas por enquanto ele serviu para o meu teste do proxychains, principalmente quando você usa testes de ip e ip geolocalização. O resultado é que meu ip e minha localização são de outros lugares.

```sh

[nabucodonsor@parrot-virtual ~]$ proxychains firefox www.bing.com

```

<br />
![DNS leak](/imagem/testando_anonimato_1.png)

<br />
![ip geolocation](/imagem/testando_anonimato_2.png)

Concluímos que o proxychain está funcional. Agora vamos avançar um pouco mais a fundo e tentar um controle maior em como nos conectamos anonimamente, eu considero esse formato muito mais anonimo. Eu comento a última linha e começo a inserir outros ips, por preferencia eu opto por conexões de protocolo socks5, mais seguro, contudo outros como http e socks4 já serveriam ao propósito de anonimidade.

```sh

[ProxyList]
# add proxy here ...
# meanwhile
# defaults set to "tor"
#socks4 	127.0.0.1 9050
#socks5 	127.0.0.1 9050
socks5 	174.64.199.79 4145
socks5 	98.185.94.76 4145
socks5 	204.101.61.82 4145


```
<br />
Com os servidores acima começo a testar meu acesso com o firefox, mas antes é preciso algumas explicações.

Primeiro, o que estou a fazer é criar uma conexão onde eu pulo de servidor à servidor até chegar ao meu ip ou destino, ou seja, não estou a realizar uma conexão direta com o site destino. Isso gera uma camada de proteção muito boa, mas a pergunta é: É possível ainda você ser localizado? Caro que sim, esses servidores dos quais estou acessando, os servidores proxy, eles tem logs de acesso com os ips, no entanto isso se torna uma tarefa extremamente dispendiosa visto a localização desses servidores estarem espalhados ao redor do mundo e quanto mais você usa servidores, mais difícil fica determinar sua localização e por isso considero uma ótima forma de anonimato. Além do mais é possível usar proxychains com tor service, bastando adicionar junto com o tor uma ip a mais no arquivo proxychains.conf.

```sh

[ProxyList]
# add proxy here ...
# meanwhile
# defaults set to "tor"
#socks4 	127.0.0.1 9050
socks5 	127.0.0.1 9050
socks5 	174.64.199.79 4145


```
<br />
Agora os problemas que enfrentei realizando essa conexão: A lentidão de acesso e o fato de está usando firefox. Primeiro de tudo a lentidão, é provável que tenha relação com a incapacidade de minha rede em acessar servidores tão distantes, fato do qual considerei na configuração do proxychains (alterei os tempos de resposta no arquivo proxychains.conf), mas o principal aspecto aqui é que esses servidores aos quais estou usando são púbicos então muitos o acessam, além de seu uptime (tempo de funcionamento), ser as vezes bastante limitados, mas isso ainda está sob investigação. Segundo, o firefox, como havia dito, ele não é a melhor opção para esses acessos, isso por causa de dois motivos: O firefox ele rastreia os seus dados, ainda que venda a propaganda de privacidade, pode não informar certos dados de seu acesso, mas eles tem uma boa noção de onde e como você está acessando e isso por questões de segurança. Para exemplificar um pouco desse problema o firefox acessa duas urls antes de enviar seus dados pela rede, um é o settings firefox, cujo papel é buscar atualizações em seus servidores além de verificar se seu acesso não é perigoso, por fim temos também o firefox detect portal, o qual seu papel é verificar o endereço que você está acessando, ele facilita o acesso e a identificação de urls. O problema dessas duas urls, é o que me leva a outra questão, são mais duas urls para eu acessar, o que de certa forma me faz questionar a velocidade com que estou os acessando e quanto mais servidores eu estou acessando. Mas voltando a um ponto que tinha comentado, você não quer usar proxychains para “surfar” na internet, use tor em vez disso, ou se você é mais paranoico com privacidade use whonix ou tail os.

Enfim continuarei investigando a minha lentidão e se é possível amenizar isso, pois como tor também tem acessos pelo mundo eu não deveria sofrer tanta lentidão quanto eu se estivesse acessando tor. A opção que usarei para teste é o bom e valho curl.
