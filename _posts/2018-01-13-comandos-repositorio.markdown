---
layout: post
title: "Adicionando e habilitando repositorios DNF"
date: 2018-01-13
categories:
  - linux
description:
image: https://source.unsplash.com/user/firsara/67l-QujB14w/2000x1200
image-sm: https://source.unsplash.com/user/firsara/67l-QujB14w/500x300
---

Assim como muitos que começam no linux, eu era do tipo que buscava soluções na internet para realizar ações no sistema e acabava por fazer o velho copiar e colar de comandos. Contudo esse comportamento sempre me perturbou tal qual hoje eu prefiro saber o que estou inserindo como comando na meu terminal ao simples copiar de comandos aleatórios, e graças a esse novo hábito amplie meu nível de conhecimento ao sistema e cada vez menos preciso buscar soluções na internet devido as minhas experiências anteriores. Porém vamos deixar de histórias e seguir para o foco do artigo.

Um dos comandos que eu mais colava e copiava era os comandos relacionados a habilitar repositórios **DNF**. Como eu costumava usar o fedora isso era algo até constante no meu dia a dia, sempre que eu queria um software de um repositório não oficial eu sempre tinha que seguir o passo a passo de alguém na web. Acredito aliais que muitos ainda só colam e copiam esses comandos, mesmo que apenas seguindo a linha de como instalar um software específico. Seria bom saber o que você anda adicionado no seu terminal até mesmo para um senso maior de clareza e possibilidades
<br />
A seguir explicarei como adicionar, habilitar e desativar repositórios. Usaremos o comando dnf config-manager para nossa proposta.

Você pode adicionar um repositório no arquivo _/etc/dnf/dnf.config_, contudo eu acho melhor defini-lo como um arquivo **.repo** em _/etc/yum.repos.d/_ , aqui vejo mais flexibilidade e os próprios arquivos tem seus debugs (nem todos).

Os comandos a seguir só podem serem realizados como usuário **root**.
Geralmente quando você está querendo instalar um software (geralmente de uma fonte não oficial) e busca na internet, digamos que o spotify para o seu fedora, o seguinte comando é usado:

~~~ shell

  # dnf config-manager --add-repo=http://negativo17.org/repos/fedora-spotify.repo

~~~

O que esse  comando faz é adicionar o repositório do spotify que você quer instalar ao seu computador como um arquivo .repo (repare no final do link). Ou seja você está usando o comando dnf config-manager para adicionar o link (opção --add-repo) da seguinte url, "http://negativo17.org/repos/fedora-spotify.repo" de onde você baixará o spotify, e a partir deste momento o repositório constará no seu fedora como uma das fontes de update e opção de software.

O comando original é o seguinte :

~~~ shell

  # dnf config-manager --add-repo repository_url

~~~

Caso você precise habilitar um link de repositório use o seguinte comando :

~~~ shell

  # dnf config-manager --set-enabled -dump repository

~~~

Onde _set-enabled_ é a opção para habilitar o repositório,  _–dump_ para display a configuração e _repository_ é substituído por pela id do repositório.

Esse comando  é bem útil quando você desabilitou algum repositório (com o comando seguinte, por exemplo)  e agora quer recuperá-lo.

E por fim para desabilitar um repositório você usa o seguinte:

~~~ shell

  # dnf config-manager --set-disabled repository

~~~

Aqui você também pode adicionar a opção dump para display a configuração. Detalhe o arquivo _.repo_ aqui não é removido do computador apenas configurado como desabilitado. Para remover um repositório do computador apenas remova o arquivo .repo que você quer de _/etc/yum.repos.d/_ ou através do terminal como o comando remove no mesmo caminho.

Espero que agora você saiba um pouco mais do que anda fazendo no seu sistema fedora, e também que saber o que digitas pode ser de grande auxílio. Aliais a documentação do fedora é bem organizada e pode ser uma boa fonte de informação, basta buscá-la de acordo com sua versão.
