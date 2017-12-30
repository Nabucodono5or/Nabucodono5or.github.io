---
layout: post
title: "Shell linux"
date: 2017-12-30
categories:
  - linux
description:
image: https://picsum.photos/2000/1200?image=352
image-sm: https://picsum.photos/500/300?image=352
---

Atualmente é bem tranquilo interagir com o Linux sem a total dependência do terminal ou uso do shell, contudo ele ainda continua sendo uma ferramenta essencial principalmente se você quiser realizar ações mais avançadas no Linux. Agora iniciarei um pequeno artigo sobre o shell e o uso dos terminais, mas de forma bem básica.

Uma coisa que você pode sempre contar em qualquer distribuição Linux é a disponibilidade do shell no sistema.O shell é um interpretador de linguagens. Com ele você pode executar programas e scripts, compilar código, gerenciar o uso do sistema e navegar dentro do próprio sistema.

Eu particularmente considero o Shell extremamente poderoso, bem mais do que o uso das simples interfaces gráficas e espero que você também o ache.
Para realizar diversas ações no shell, você executa comandos, alguns simples, outros um pouco complexo.

Antes de mais nada existem diversos tipos de shell, listarei alguns aqui: o _C Shell (csh), o Korn Shell (Ksh), o dash shell que é padrão Ubuntu_. E existem outros, O detalhe é que eles mudam pouco de um para outro, pelo menos em referência a comandos. As vezes você terá mais de um instalado no seu computador. O que estou usando é _Bash Shell (Bourne Again Shell)_ que é padrão na distro Fedora que uso.

Agora que sabemos que existem outros shell, irei descrever como acessamos o shell, e você verá que existem diversas formas, algumas vantajosas para certas situações. Os três modos mais comuns são shell prompt, Terminal Window e Virtual console.


### SHEL PROMPT
Se seu sistema Linux está sem uma interface gráfica ou ela está indisponível por causa de algum problema do sistema, é provável que você veja apenas o prompt de comando depois de se logar. Neste caso você seria induzido a digitar alguns comandos.

Se você é um usuário regular o prompt seria indicado por **$**. Caso seja o usuário root seria **#**.	Junto a outros dados do usuário, por exemplo `[jake@pine share]$`  .Onde `jake` é nome do usuário, pine o nome do computador e share a localização atual do usuário no sistema de arquivos do tipo `/usr/share/` .

```shell

[jake@pine share]$

```

Existem diversas funcionalidades aqui e você pode sempre digitar alguns comandos.

### TERMINAL WINDOW
Se você usa interface gráfica é bem provável que a maioria de suas interações com o shell sejam através do _Terminal window_. E dependendo da distribuição tem diversas formas de acessá-lo. Mas as mais comuns é  o clic direito no desktop e escolhendo terminal nas opções, outra é indo no menu ou painel do sistema.

Além dessas fique atentos a nomenclaturas. _New/novo Terminal, Open/abrir Terminal, Xterm e Shells_.

Basicamente você trabalha aqui como trabalharia no prompt, sendo que exitem outras funcionalidades dependendo do terminal que você possua. Use o comando abaixo para encontrar todos os terminais disponíveis no seu repositório da distro. E uma das coisas mais legais é que você pode instalar outros terminais caso tenha alguma preferência, só note o tipo de interface gráfica que você possui. Lembrando que para instalar via comando você deve ser usuário _root_.

O Terminal window facilita o acesso ao shell pois você vai precisar apenas acessá-lo em meio a sua área de trabalho como qualquer outra aplicação e é bem provável que você o use mais que qualquer um dos outros modos.


### CONSOLE VIRTUAL
Sistemas Linux que incluem interfaces gráficas, normalmente possuem múltiplos consoles virtuais, consoles esse providos para possibilitar diversas sessões de  shell em conjunto com a interface gráfica. Eles são inciados ao mesmo tempo em que você inicia sua interface gráfica e para acessá-los basta segurar _Ctrl_ e _Alt_, selecionando a sessão pelo _f1, f2_ … até _f6_, porém exitem certos detalhes aqui.

Em algumas distribuições sua sessão atual é f1 que é a primeira sessão  seguida das próximas seis virtuais, e em outros variam (podem serem a quinta sessão ou a sexta), contudo ao pressionar para acessar um console virtual você estará entrando em uma interface formato texto  para a digitação dos comandos (parecido com o prompt) .
Quando você terminar digite exit para fechar a sessão atual e então `Ctrl + Alt + f1` ( ou _f5_ ou _f6_ ), para voltar para a sessão gráfica.

Bom antes de terminar gostaria de demonstrar alguns comando só para não ficar somente na teoria:

Selecione um dos três métodos acima para começar a digitar comandos no Linux:
(comando cd)

Comando que você pode usar para percorrer o sistema de arquivos de forma relativa ( _a partir da localização atual para as subárvores de arquivos_) ou completa( um endereço desde a raiz dos arquivos), independente do que for é um modo rápido de acessar diretórios pelo shell.

(comando ls)

Comando para listar os aquivos no diretório atual do Shell, ele vem com algumas opções, como o `-l` para uma listagem mais completa sobre cada arquivo, como o `-a` para listar arquivos ocultos.

(comando grep)

Para identificar o seu Shell no sistema, uma forma bem prática de saber o shell que você usa, observe que eu uso seguinte Shell (), identificado pelas últimas palavras. Basicamente o `grep` acessa o  arquivo **/etc/passwd** e exibe a linha com o meu nome de usuário.

Todos esse comandos são de acesso a nível regular, ou seja de usuário comum. Futuramente abordarei mai s questões sobre o shell e até mesmo como configurá-lo para efeitos de estética. Por enquanto isso basta, lembre-se de fechar a sessão ou o terminal, ou mesmo deslogar do prompt quando acabar.Até mais.
