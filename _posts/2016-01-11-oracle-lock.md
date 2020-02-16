---
title: "[ORACLE] Locks"
date: 2016-01-11 11:12:23 Z
categories:
- posts
---

## Oracle Lock
Conversando com alguns amigos entramos no assunto Lock e então resolvi escrever esse post para explicar do que se trata esse mecanismo.

Antes de tudo, devemos entender que o Lock não é um problema e sim um mecanismo de proteção.

<!--more-->

Existe um conceito em banco de dados chamado  **ACID** (**A**tomicidade, **C**onsistência, **I**solamento e **D**urabilidade) veja [aqui na Wikipedia](https://pt.wikipedia.org/wiki/ACID "aqui na Wikipedia") uma melhor explicação , nesse post vamos nos concentrar no **I** (isolamento).

**Imagine a seguinte situação:**

O salário do funcionário João é de R$1000,00 e seu gerente resolve dar um aumento de R$200,00, seu gerente abre o programa que controla a folha de pagamento e ajusta o salário para R$1200,00 mas esquece de clicar em salvar (commit) ou cancelar (rollback), se nesse meio tempo (entre ele confirmar ou cancelar) alguém também tentar atualizar o salário do João, um lock vai ocorrer pois o banco de dados está aguardando que o gerente libere esse campo com um commit ou um rollback.

Já imaginou se o banco de dados deixasse todo mundo atualizar o mesmo campo ao mesmo tempo? Como poderíamos ter certeza de qual a informação é a válida? Por isso costumamos dizer que um lock é um mecanismo de proteção e não um problema.

**Tipos de lock**

Agora que sabemos que um lock é um mecanismo de proteção e não um problema, vamos entender os diferentes tipos de lock existentes em um banco de dados Oracle.

Dentro do banco de dados Oracle existe uma view chamada **V$LOCK** que possui uma coluna chamada **TYPE**, nela conseguimos verificar qual o tipo de lock que esta ocorrendo, os mais comuns são os tipos TX e TM.

**TX também conhecido como Row Lock**

Um lock do tipo TX serve para evitar que diversos usuários editem a mesma linha ao mesmo tempo, graças ao lock TX apenas as linhas em uso ficam bloqueadas, deixando assim o resto da tabela livre para edição, geralmente pegamos esse tipo de lock com SQLs mal escritos que não executam um commit logo após a sua execução.

**TM lock de tabelas**

Esse tipo de lock ocorre quando existem diversas operações DML no mesmo objeto, como já falado, esse tipo de lock serve para proteger o objeto de uma possível alteração na estrutura da tabela enquanto um DML é executado.

**Uma infinidade de locks**

Caso queiram se aprofundar no assunto, recomendo dar uma olhada na documentação oficial da Oracle lá existe uma tabela com as operações e que tipo de lock elas podem causar : 
[http://docs.oracle.com/cd/E11882_01/server.112/e41084/ap_locks001.htm#SQLRF55502](http://docs.oracle.com/cd/E11882_01/server.112/e41084/ap_locks001.htm#SQLRF55502)

##Posso matar um lock?
A resposta é sim e não, isso varia para cada caso e o ideal é que os locks sejam resolvidos de forma automática pelo próprio banco.
Onde trabalho, um cliente nos solicitou que os locks não sejam finalizados pois ele possui uma rotina que gera nota fiscal que é normal ela travar e causar locks, também possuímos clientes que já solicitam isso direto, em caso de lock devemos mata-los.
Só mate o lock caso realmente seja necessário.


##Referencias:
[http://docs.oracle.com/cd/E11882_01/server.112/e41084/ap_locks001.htm#SQLRF55502](http://docs.oracle.com/cd/E11882_01/server.112/e41084/ap_locks001.htm#SQLRF55502)
[https://docs.oracle.com/cloud/latest/db112/REFRN/dynviews_2027.htm#REFRN30121](https://docs.oracle.com/cloud/latest/db112/REFRN/dynviews_2027.htm#REFRN30121)
[https://pt.wikipedia.org/wiki/ACID](https://pt.wikipedia.org/wiki/ACID)
