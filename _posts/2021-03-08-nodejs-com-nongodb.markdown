---
layout: post
title: "Usando Nodejs com Mongodb"
date: 2021-03-08
categories:
  - Javascript
description:
image: https://source.unsplash.com/user/jdiegoph/GNyjCePVRs8/2000x1200
image-sm: https://source.unsplash.com/user/jdiegoph/GNyjCePVRs8/500x300
---

O mongodb tornou o meu desenvolvimento com banco de dados extremamente simples na hora de persistir dados. Com ele pude criar aplicações mais sofisticadas utilizando nodejs. Mangodb é um NoSQL que se utiliza de documents e collections para organizar e armazenar informação. As coleções seriam um conjunto de dados ou linhas, referenciando o sql. Definindo bem superficialmente essas informações seriam vistas como json. Minha perspectiva para utilizá-lo é que dados que podem serem organizados hierarquicamente  ou não complexos, possam serem persistidos em tal tipo de banco de dados, mas isso é apenas minha maneira de simplificar minhas decisões ao usá-lo, certamente existem justificativas melhores, como  scablility  e desempenho.

Para esse projeto instalaremos o mongodb localmente. Atualmente o mongodb trabalha com armazenamento em nuvem, o serviço Atlas, contudo apenas para teste e aprendizado o próprio instalado já está bom demais. De qualquer forma apesar de mencionar o uso do banco mongodb em si o objetivo aqui é apresentar como ele pode funcionar bem com um backend, no caso o nodejs.

Para instalar o mongodb caso você não o tenha, recomendo olhar o site [https://www.mongodb.com/](https://www.mongodb.com/) que explica como instalar para cada plataforma. Se você utiliza Linux recomendo descobrir em qual distro foi baseado seu sistema e seguir o passo a passo, pois não adianta tentar pelo gerenciador de pacotes, nem o snap e nem flatpak.

Para demonstrar como eu o uso criarei a seguinte estrutura de arquivos:

~~~ shell
    node_modules
    src/
        server.js
        schemas/
            pokemon.js   

~~~

Onde src/ será onde iremos por todos os nossos arquivos node e a pasta schemas será responsável pelos modelos do banco de dados. Na realidade essa estrutura pode se tornar mais complexa dependendo se você está utilizando outras ferramentas como eslint, typescript, nodemon ou um outro trans pilador, mas pela simplicidade deixaremos como está.

Depois devemos instalar as bibliotecas que utilizaremos. Abaixo instalo o express, o cors e mongoose.

~~~ shell

    $ npm install express mongoose

    ...

    $ npm install cors --save-dev

~~~
Uma dica para quem quiser usar o mongodb com atlas em um projeto git e não quiser por consequência exibir suas credenciais, pode usar a biblioteca config para somente fornecer as credências através de um arquivo não compartilhado.

Abaixo o código para conectar o banco de dados com a aplicação e a criação das rotas.

~~~ javascript

    const express = require('express'); 
    const mongoose = require('mongoose');
    const cors = require('cors');
    const Pokemon = require('./schemas/pokemon');

    const express = express();

    function main() {
        express.use(express.json());
        express.use(cors());
        database();
        routes();
    }

    function database() {
        mongoose.connect('mongodb://127.0.0.1:27017/testmongodb', { useNewUrlParser: true }).then((result) =>{
            console.log('banco de dados testmongodb conectado!');
        });
    }

    function routes() {
        express.get('/pokemon', async (req, res) => {
            const pokemonIndex = await Pokemon.find();

            return res.json(pokemonIndex);
        });

        express.post('/pokemon', async (req, res) => {
            const pokemon = await Pokemon.create(req.body);
        })
    }

    main();

~~~
<br>


Utilizamos o mongoose para fazer a conexão enviando uma url como argumento, tal url é a padrão gerado pelo próprio mangodb, se quiser provar é só acessar o banco via linha de comando. Contudo antes de conectarmos o banco de dados, seria bom criarmos nossos schemas.

Schemas no mongodb seriam uma estrutura definida de como nossos dados ou informações seriam armazenados em coleções. Isso em código seria o uso do mongoose definindo as propriedades e os tipos que elas armazenariam como um objeto. Para contesto prático estou criando o schema de um pokémon. A maioria de suas propriedades seriam do tipo string exceto dados como o número dele na lista.


~~~ javascript

    const mongoose = require('mongoose'); 
    const Schama = mongoose.Schema;

    const express = express();

    const PokemonSchema = new Schema(
        {
            nome: {
                type: String,
                required: true
            },
            numero: {
                type: Number,
                required: true
            },
            tipo: {
                type: String,
                required: true
            },
            descricao: {
                type: String,
                required: true
            }
        },
        {
            timestamps: true,
        },
    );
~~~
<br>


Após criarmos os Shcemas devemos colocá-los em modelos de tal forma que possamos acessá-los como funções e executar operações em cima de tais dados. Não precisaremos buscar por coleção como faríamos manualmente em no mongodb. Dessa forma nosso esquema de acesso ao banco de dados seria simples e completo. 

~~~ javascript

    const mongoose = require('mongoose'); 
    const Schama = mongoose.Schema;
    
    const express = express();

    const PokemonSchema = new Schema(
        //.....
    );

    const Pokemon = mongoose.model('Pokemon', PokemonSchema);
    module.exports = Pokemon;
~~~
<br>


Cada schema e modelo recomendo que sejam criados em um arquivo separado, assim tornamos o código mais limpo e fácil de manutenção.


Por fim, criamos nossas rotas tanto  para acesso, quanto para a criação dos dados. Veja que o mongoose usa  promise tanto para connectar o banco de dados, quanto para realizar requisições e inserções. Para teste podemos mudar a rota post para get e criarmos internamente no node um pokémon, veja que o retorno da promise que cria o dado retorna ele próprio se tudo ocorreu bem. Acessando a rota pelo browser podemos ver o dado em formato json. 

~~~ javascript

    const express = require('express'); 
    const mongoose = require('mongoose');
    const cors = require('cors');
    const Pokemon = require('./schemas/pokemon');

    const express = express();

    function main() {
        //..........
    }

    function database() {
        //..........
    }

    function routes() {
        express.get('/pokemon', async (req, res) => {
            //..........
        });

        express.get('/pokemon', async (req, res) => {
            const pokemon = new Pokemon({
                nome: 'Bulbasaur',
                numero: 1,
                tipo: 'planta/veneno',
                descricao: 'There is a plant seed on its back right from the day this Pokémon is born. The seed slowly grows larger.'
            });

            Pokemon.save().then((result) => {
                console.log(result);
            }).catch((err) => {
                console.log(err);
            })
        });
    }

    main();

~~~
<br>


Apesar de funcionar o teste acima recomendo que se utilize uma ferramenta para testar API. Eu, por  exemplo, utilizo insomnia e, mas existem outros como o postman. Um dia explico o uso dessas ferramentas e suas vantagens.

Voltando a rota como método post, ela emula uma ação que enviará informações através de uma interação com o usuário através do html e javascript. Seja essas informações obtidas de um formulário ou não.

Dessa forma podemos criar quantos schemas queremos e acessá-los e usá-los em uma aplicação web normal. Agora você pode persistir seus dados de forma eficiente e elegante.
