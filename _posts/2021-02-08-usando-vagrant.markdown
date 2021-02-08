---
layout: post
title: "Usando Vagrant em projetos web"
date: 2021-02-08
categories:
  - Ruby
description:
image: https://source.unsplash.com/user/jdiegoph/sUG0LWWmNiI/2000x1200
image-sm: https://source.unsplash.com/user/jdiegoph/sUG0LWWmNiI/500x300
---

Recentemente passei a utilizar o vagrant em meus projetos como forma de me acostumar com a tecnologia e não por verdadeira necessidade, o resultado é que gostei muito de seu funcionamento e achei bastante prático para ser inserido no desenvolvimento de aplicaçães, caso esteja interessado em um ambiente de desenvolvimento isolado do sistema.
Vagrant se trata de uma ferramenta usada para a criação e o gerenciamento de ambientes de desenvolvimento ou testes usando virtualização. Imagine que você construiu uma aplicação, mas na hora de rodá-la, seja nos computadores de seus colegas de time de desenvolvimento, ou do cliente, não funciona. A construção de um ambiente de desenvolvimento “limpo” e controlado pode ser muito mais eficiente para o desenvolvimento da aplicação, melhor ainda se ele pode ser compartilhado para ser reproduzido da forma mais simples por outros desenvolvedores. Eis que o Vagrant faz tudo isso.

Para começar vamos nos concentrar em uma construção simples no linux.

Após instalar o Vagrant, você deve ter um software de virtualização, ele pode ser VirtualBox ou Vmware workstation, por exemplo. Com Vagrant instalado hora de construir seu ambiente para isso você precisa está no diretório raiz do seu projeto e executar o seguinte comando, mas preste atenção  vagrant já oferece alguns ambiente pré preparados, use um deles com base, no meu caso eu gosto de “hashicorp/bionic64”, prefiro devido a seu ambiente server ubuntu. 

~~~ shell

 $ vagrant init hashicorp/bionic64

~~~
<br>


Automaticamente será criado um arquivo chamado Vagrantfile. Observe que o arquivo está em linguagem Ruby. Após isso recomendo que olhe o arquivo e faça as configurações necessários para seu uso ou como será o ambiente de desenvolvimento. Não sabe como fazer essas alterações? Então me acompanhe.

Arquivo gerado abaixo:

~~~ ruby

    # -*- mode: ruby -*-
    # vi: set ft=ruby :

    # All Vagrant configuration is done below. The "2" in Vagrant.configure
    # configures the configuration version (we support older styles for
    # backwards compatibility). Please don't change it unless you know what
    # you're doing.
    Vagrant.configure("2") do |config|
        # The most common configuration options are documented and commented below.
        # For a complete reference, please see the online documentation at
        # https://docs.vagrantup.com.

        # Every Vagrant development environment requires a box. You can search for
        # boxes at https://vagrantcloud.com/search.
        config.vm.box = "hashicorp/bionic64"

        # Disable automatic box update checking. If you disable this, then
        # boxes will only be checked for updates when the user runs
        # `vagrant box outdated`. This is not recommended.
        # config.vm.box_check_update = false

        # Create a forwarded port mapping which allows access to a specific port
        # within the machine from a port on the host machine. In the example below,
        # accessing "localhost:8080" will access port 80 on the guest machine.
        # NOTE: This will enable public access to the opened port
        # config.vm.network "forwarded_port", guest: 80, host: 8080

        # Create a forwarded port mapping which allows access to a specific port
        # within the machine from a port on the host machine and only allow access
        # via 127.0.0.1 to disable public access
        # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

        # Create a private network, which allows host-only access to the machine
        # using a specific IP.
        # config.vm.network "private_network", ip: "192.168.33.10"

        # Create a public network, which generally matched to bridged network.
        # Bridged networks make the machine appear as another physical device on
        # your network.
        # config.vm.network "public_network"

        # Share an additional folder to the guest VM. The first argument is
        # the path on the host to the actual folder. The second argument is
        # the path on the guest to mount the folder. And the optional third
        # argument is a set of non-required options.
        # config.vm.synced_folder "../data", "/vagrant_data"

        # Provider-specific configuration so you can fine-tune various
        # backing providers for Vagrant. These expose provider-specific options.
        # Example for VirtualBox:
        #
        # config.vm.provider "virtualbox" do |vb|
        #   # Display the VirtualBox GUI when booting the machine
        #   vb.gui = true
        #
        #   # Customize the amount of memory on the VM:
        #   vb.memory = "1024"
        # end
        #
        # View the documentation for the provider you are using for more
        # information on available options.

        # Enable provisioning with a shell script. Additional provisioners such as
        # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
        # documentation for more information about their specific syntax and use.
        # config.vm.provision "shell", inline: <<-SHELL
        #   apt-get update
        #   apt-get install -y apache2
        # SHELL
    end


~~~
<br>

Como o meu irá ficar:

~~~ ruby

    # -*- mode: ruby -*-
    # vi: set ft=ruby :

    Vagrant.configure("2") do |config|

    config.vm.box = "hashicorp/bionic64"

    config.vm.provider "virtualbox" do |vb| 
        vb.memory = "2048"
        vb.cpus = 4
    end

    # config.vm.network "forwarded_port", guest: 80, host: 8080
    # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
    config.vm.network "private_network", ip: "192.168.33.10"
    # config.vm.network "public_network"

    # config.vm.synced_folder "../data", "/vagrant_data"
    config.vm.synced_folder "./dist", "/var/www/html", :nfs => { :mount_options => [ "dmode=777", "fmode=665" ] }

    config.vm.provision "shell", path: "bootstrap.sh"
    end


~~~
<br>


A primeira coisa é que temos todos os códigos dentro do bloco Vagrant.configure, costumo deixar alí somente o que irei precisar, no caso a configuração da vm, config.vm.box, que caso você tenha optado por um dos ambientes do site ele estará declarado alí. Depois gosto de manter todas as opções de rede, como crio aplicações web, quero ter o seu acesso rápido através de um endereço. Olhando o arquivo você verá muitos comentários, eles explicam o que cada código comentado pode realizar. Eu apago a maioria desses comentários deixando somente os comandos que me interessam.

Juntado todos os comandos de rede, opto pelo acesso através de um ip privado, mais seguro. 
O próximo comando é como você pode definir as configurações da vm, aliais o vagrant file tem um comando para executar a vm, porém você não precisa disso para apenas testar ou executar sua aplicação. Você pode usar o terminal para vereficar a vm. 


Os comandos para configurar os recursos da vm ficam dentro do bloco config.vm.provider. Abaixo temos os dois últimos comandos, o penúltimo irá sincronizar os arquivos do meu projeto com os que estão na virtuabox. 

Essa é uma parte extremamente interessante, os arquivos que você irá criar para o seu projeto podem serem transferidos automaticamente para a vm, se você está desenvolvendo uma aplicação web com um builder, recomendo transferir os arquivos da pasta dest/ . Por fim você pode escrever de forma direta comandos que serão executados na construção do ambiente,ou usar, minha preferência, um script shell. 

No comando config.vm.synced_folder acrescentamos um comando que não está nos comentários, o nfs, esse comando obriga que você digite sua senha root, ele melhora a sincronização dos arquivos.

Caso você tenha optado pelo uso de um shell script como forma de amontar seu ambiente disponibilizo abaixo um exemplo do mesmo.

~~~ shell

    # Update Packages
    apt-get update
    # Upgrade Packages
    apt-get upgrade

    # Basic Linux Stuff
    apt-get install -y git

    # Apache
    apt-get install -y apache2

    # Enable Apache Mods
    a2enmod rewrite


~~~
<br>

Agora, hora de montar nosso ambiente, execute o comando abaixo para executar a vm:

~~~ shell

   $ vagrant up

~~~
<br>

Você verá tudo sendo instalado, e se seguiu as modificações no Vagrantfile então será pedido sua senha root.

Com o ambiente pronto, se você excluiu a exibição da gui nada será exibido da vm, mas você pode acessá-la com o comando:

~~~ shell

   $ vagrant ssh

~~~


E seu terminal irá mudar para o ambiente da vm, lá você pode modificar o que quiser. Eu pedir para que meus arquivos fossem transferidos para a pasta www, devido ande será executado o servidor apache, mas você pode optar por qualquer pasta, basta modificar no Vagrantfile. 

Por fim, seu eu que quiser acessar minha aplicação web, estando um servidor executando na vm, eu uso o endereço ao qual modifiquei no Vagrantfile. O Vagrant realiza tanto um port forward como redireciona acessos de servidores por endereços ip no host, ou seja, seu computador.

Para fechar temos os comandos:

~~~ shell

   $ vagrant destroy

~~~
<br>

caso queiramos destruir a virtual machine.

~~~ shell

   $ vagrant suspend

~~~
<br>

Para suspender a vm.

~~~ shell

   $ vagrant reload

~~~
<br>

Para recarregar o arquivo vagrant file, caso tenhamos alterado alguma coisa ali, consequentemente o ambiente será remontado.

~~~ shell

   $ vagrant resume

~~~
<br>

Para sair do estado de suspenso. Além disso é possível executar diversas vm ao mesmo tempo com Vagrant, pense em banco de dados ou api.

Vagrant é uma ferramenta muito boa e tranquila de se lidar, prover uma ótima produtividade na criação de aplicações. Futuramente eu abordo a execução do Vagrant com múltiplas virtualizações.