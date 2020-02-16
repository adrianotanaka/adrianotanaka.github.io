---
title: Oracle Cloud, seria esse o futuro da computação?
date: 2015-11-11 11:51:23 Z
categories:
- posts
---

Tive a oportunidade de participar de um workshop sobre Oracle Cloud e pude ver algumas coisas interessantes que podem se tornar realidade no quesito banco de dados e também no suporte a aplicações, tudo utilizando a estrutura da própria Oracle.
Atualmente diversas empresas (as grandes principalmente) estão em uma corrida para ver quem vai dominar a nuvem que segundo especialista é um futuro inevitável: Todos em algum momento no futuro vão estar na nuvem.
No WorkShop foram apresentados diversas funcionalidades da nuvem da Oracle, listo as mais importantes(e interessantes) abaixo:

[***Servidor Oracle Cloud localizado no Brasil***](https://www.oracle.com/corporate/pressrelease/data-center-brazil-062415.html)

Uma boa notícia para o Brasil e países da América Latina: a Oracle começou a oferecer em São Paulo um data center o que em teoria melhora alguns pontos no serviço oferecido (vou falar sobre isso daqui a pouco).

<img src="https://cdn.pbrd.co/images/23wFGBe0.png"/>

***Tudo na nuvem***

Uma das coisas que foi apresentado ontem foi que a Oracle vai passar a oferecer (sem data prevista ainda)  a capacidade de rodar diversas linguagens de programação, sem a necessidade de um servidor dedicado a isso, para java você já pode ver [aqui](https://cloud.oracle.com/en_US/java) como funciona, para as outras linguagens de programação vamos ter que esperar para ver como isso vai funcionar.

Meu banco de dados na nuvem, e agora?
-

Um dos pontos que o instrutor focou muito, foi o banco de dados na nuvem, existem diversas opções, você pode manter alguns serviços locais e outros na nuvem, várias versões de banco de dados e features disponíveis:
<img src="http://i.imgur.com/Sr9yYoC.png"/>
*Imagem retirada do site da Oracle*

Aqui que as coisas começam a ficar interessantes(pelo menos para quem mexe com banco), a Oracle realizar cobranças de duas formas:

**Mensal**
**Por consumo**

Ainda segundo o instrutor, em alguns casos(aqui entra o pessoal de vendas da Oracle), usar uma infraestrutura toda baseada em nuvem acaba saindo mais barato do que utilizar uma infra [On Premises](https://en.wikipedia.org/wiki/On-premises_software) além é claro de oferecer mais segurança, diminuir os custos e principalmente o tempo para uma nova implementação ou ajuste.

Colocando meu banco na nuvem
-

Um dos labs realizados foi o procedimento de subir uma nova instância dentro da nuvem da Oracle.
Todo o processo de selecionar as configurações do servidor, tais como processador(a Oracle tem uma medida própria para isso) memória, disco e versão do banco de dados leva em torno de 1hora para single instânce e 2horas para um ambiente em cluster(infelizmente não tivemos tempo para de testar esse segundo cenário).

Mas a instalação de uma single instânce 12c foi um sucesso, não ocerram problemas durante o procedimento e o resultado foi melhor do que o esperado, ele realmente funciona, inclusive fazendo uso de algumas novas tecnologias como o  multitenant.

Backup em nuvem do banco
-

Depois de clonar um banco local e enviar para nuvem, realizar o mesmo procedimento atráves de um database link e ainda usando o datapump, chegou o momento que a maioria estava aguardando ansiosos: Realizar um backup de uma máquina local para um espaço dentro da nuvem da Oracle, que é um serviço separado, você não precisa comprar uma nuvem para banco, compra apenas um espaço para realizar os seus backups o que torna essa solução muito interessante e barata(pelo menos no meu ponto de vista):
<img src="http://i.imgur.com/cQBZIrd.png"/>
*Imagem retirada do site da Oracle*

Por 33 doláres por mês a Oracle te entrega 1Tb de armazenamento na nuvem dela, dependendo do tamanho do banco de dados esse espaço pode durar por muito, muito tempo.
A implementação dessa tecnologia foi feita de forma simples, bastando algumas configurações rápidas mas ainda precisa de mais alguns testes uma vez que no workshop todos os procedimentos foram realizados dentro da estrutura da própria Oracle.

***Nem tudo são flores***

Em nosso workshop, utilizamos a estrutura da própria Oracle(vale ressaltar que é uma estrutura muito boa), para realizar o backup de uma base local(em minha máquina) para os servidores de backup da Oracle, esbarramos no principal ponto  que todos já imaginavam: Internet.

Demoramos mais de 1hora para conseguir fazer o backup da base de teste(que se não me engano tinha aproximadamente 400Mb) mas após esse tempo de espera o backup foi realizado com sucesso dentro do servidor de backup da Oracle.

Ainda no lab de backup um dos testes foi apagar um tabela da minha base local e realizar o restore a partir do backup que estava na nuvem da Oracle, onde mais uma vez esbarramos no problema da internet que acabou se mostrando o maior gargalo nessa operação.

Depois da espera, a base foi restaurada com sucesso o que animou bastante os participantes do workshop, mas como já falado antes, essa tecnologia ainda precisa de mais testes e infelizmente o servidor brasileiro não estava liberado para os testes desse workshop e acabamos usando servidores americanos para tal tarefa.


***Pontos positivos***
Podemos usar os comandos do RMAN para gerenciar os backups na nuvem
Configuração rápida e simples

***Pontos de atenção***
Internet pode ser o maior gargalo
Consultar as informações direto no storage de backup da Oracle.

* * *


Bem pessoal, essas foram minhas considerações sobre o Workshop Oracle Cloud, qualquer duvida por favor entrem em contato comigo.
