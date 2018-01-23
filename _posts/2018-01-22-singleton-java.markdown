---
layout: post
title: "Uso de Classes Singleton em java e Android"
date: 2018-01-22
categories:
  - Java
description:
image: https://source.unsplash.com/user/markusspiske/AaEQmoufHLk/2000x1200
image-sm: https://source.unsplash.com/user/markusspiske/AaEQmoufHLk/500x300
---

Singleton é um padrão de projeto que limita a instanciação de uma classe para uma única referência de forma global. Dessa forma podemos limitar o acesso a recursos específicos do aplicativo, como banco de dados, conexões a internet e cache. Em vez de poluir e constantemente criar uma instancia da mesma classe, podendo incorrer em problemas de comportamento do aplicativo, podemos delegar isso para  uma única classe que assegura todo um recurso. Abaixo descrevo seu uso e vantagens usando java.

A classe singleton é responsável por instanciar a si mesma, ou pode delegar a criação de um objeto a uma classe factory ( outro padrão de projeto).

~~~ java
package singletonexemplo;

/**
 *
 * @author jeffetrson
 */
public class Singleton {
        private static Singleton INSTANCE;

    public static Singleton getInstance(){
        if(INSTANCE == null){
            INSTANCE = new Singleton();
        }
        return INSTANCE;
    }

}

~~~

O exemplo acima assegura que a classe singleton só possa ser instanciada através do método getInstance, simplesmente tornando o construtor privado. O método getInstance é static, podendo ser acessado diretamente da classe e retorna uma instancia da própria classe. Instancia que é o retorno da variável static INSTANCE. O método getInstance também assegura que haja somente uma instanciação da classe através de um pequeno check se a classe é null, caso contrário getInstance irá retornar a mesma instancia.

Dessa forma é bem direto como podemos trabalhar com a classe singleton, e podemos verificar através do método hashCode() da classe Object que todas as classes herdam, que os objetos são iguais. A hash key que retorna é a mesma se os objetos são iguais.

~~~ java
package singletonexemplo;

/**
 *
 * @author jeffetrson
 */
public class SingletonExemplo {

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        // TODO code application logic here
        Singleton s1 = Singleton.getInstance();

        Singleton s2 = Singleton.getInstance();

        if(s1.hashCode() == s2.hashCode()) System.out.println("--São igauis--");

        System.out.println("hashcode de s1 : " + s1.hashCode() );
        System.out.println("hashcode de s2 : " + s2.hashCode() );

    }

}

~~~



Podemos nos beneficiar desse comportamento para obter um RestClient para o tratamento de uma Retrofit em Android.

~~~ java
import retrofit2.Retrofit;
import retrofit2.converter.gson.GsonConverterFactory;

public class RetrofitClient {

    private static Retrofit retrofit = null;

    public static Retrofit getClient(String baseUrl) {
        if (retrofit==null) {
            retrofit = new Retrofit.Builder()
                    .baseUrl(baseUrl)
                    .addConverterFactory(GsonConverterFactory.create())
                    .build();
        }
        return retrofit;
    }
}
~~~


Onde nos asseguramos que haja somente a instanciação uma única vez para diversas requisições. E podemos lidar com tudo isso somente chamando RestClient.getInstace() para obter uma o objeto para fazer a requisição.


Agora gostaria de abordar como lidar com multithreading, que é um detalhe muito importante quando você quer trabalhar com instancias únicas em diversas tasks. Para isso gosto de usar o teste abaixo com a classe singleton, para demonstrar isso de forma mais direta:

~~~ java

package multithreadingexemplo;

/**
 *
 * @author jeffetrson
 */
public class MultiThreadingExemplo {

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        // TODO code application logic here

        Thread t1 = new Thread(new Runnable() {
            @Override
            public void run() {
                Singleton s1 = new Singleton();
                System.out.println("primeira singleton " + s1.hashCode());
            }
        });

        Thread t2 = new Thread(new Runnable(){
            @Override
            public void run() {
                Singleton s2 = new Singleton();
                System.out.println("segunda singleton "+ s2.hashCode());
            }
        });

        t1.start();
        t2.start();
    }

}

~~~


Ao executar o código muitas vezes uma hora criaremos instancias diferentes para a mesma singleton, o que por sí só já  viola a proposta desse padrão de projeto. O que afinal está acontecendo? Basicamente executamos a condição INSTANCE == null ao mesmo tempo e por causa disso foram retornados duas novas instancias da mesma classe, não houve como verificar se as duas Threads estavam acessando ao mesmo tempo o mesmo método.

Então para resolver isso basta impedir que ambas Threads acessem o mesmo método ao mesmo tempo. Bom, para isso basta fazer Synchronized o método getInstance.

~~~ java

package multithreadingexemplo;

/**
 *
 * @author jeffetrson
 */
public class Singleton {
        private static Singleton INSTANCE;

    public static synchronized Singleton getInstance(){
        if(INSTANCE == null){
            INSTANCE = new Singleton();
        }
        return INSTANCE;
    }

}

~~~


Para aqueles que não saibam o que a palavra-chave synchronized faz, ela trabalha em sincronizar (óbvio) o acesso de Threads a um mesmo recurso, no caso aqui o método getInstance, dessa forma uma Thread não pode acessar um recurso de um mesmo objeto se ele estiver já estiver sendo acessado por outra Thread, no caso ela espera a primeiro terminar o uso. Mantemos assim a integridade do objeto e seus dados.

Contudo Java VM não é obrigada a fazer o valor de instance surgir até a VM alcançar  um estado conhecido como “memory barrier”, tornado ainda quebrado nosso esquema, e vendo nossa INSTANCE  metade inicializada. Para impedir disso acontecer usamos a palavra-chave volatile, garantindo que toda nossa ação ira acontecer antes mesmo de qualquer leitura da INSTANCE.

~~~ java

package multithreadingexemplo;

/**
 *
 * @author jeffetrson
 */
public class Singleton {
        private static volatile Singleton INSTANCE;

    public static synchronized Singleton getInstance(){
        if(INSTANCE == null){
            INSTANCE = new Singleton();
        }
        return INSTANCE;
    }

}


~~~


Uma outra questão é que as ações acima de synchronized podem deixar nossa aplicação “lenta”, já que precisamos sempre regular o acesso ao getInstance para cada Thread, e sabemos como otimização é algo essencial para dispositivos android. Então para resolver basta notarmos que não precisamos sempre organizar o acesso dos Threads para impedir instanciações diferentes, apenas quando a classe INSTANCE é nula, ou seja, apenas no primeiro acesso a ela. Portanto trocamos a palavra-chave synchronized por um bloco synchronized com a entrada da própria classe como parâmetro.

~~~ java
package multithreadingexemplo;

/**
 *
 * @author jeffetrson
 */
public class Singleton {
        private static volatile Singleton INSTANCE;

    public static synchronized Singleton getInstance(){
        if(INSTANCE == null){

          synchronized(Singleton.class){
              INSTANCE = new Singleton();
          }
        }
        return INSTANCE;
    }

}

~~~


O bloco interno do synchronized será executado somente uma vez para o segundo check null.

Enfim, se você procura uma instanciação única ou assegurar que haja somente um recurso de acesso a sua aplicação ou classe, singleton é extremamente funcional. Claro que ainda sim é possível violar esses conceitos no singleton e criar instanciações diversas da mesma classe ainda que ela seja singleton, um desses modos é o uso de Reflections, ainda sim singleton é bastante útil para a maioria das aplicações.
