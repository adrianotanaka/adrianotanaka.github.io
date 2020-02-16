---
title: Startup e shutdown de uma instância Oracle
date: 2015-09-21 09:20:00 Z
categories:
- posts
---

Entender como uma instância pode ser iniciada ou parada no banco de dados oracle é muito importante para um administrador de banco de dados, existem alguns problemas que podem ser resolvidos em determinado stagio da instância.

Para iniciar uma instância podemos usar o comando startup no sqlplus, mas atenção a essa parte pois o usuário deve ter permissões de DBA.
Podemos usar varias combinações de startup:
<ul><li>startup</li><li>startup nomount</li><li>startup mount</li></ul>

O que acontece em cada estágio:
-
**nomount**

Nesse estágio o Oracle lê o arquivo de inicialização, que pode ser o init_sid.ora ou o spfileSID.ora, onde ele encontra alguns parametros que vão ser usados na configuração da instância.
Depois de ter lido o arquivo, as áreas de memória são configuradas de acordo com o que estava no arquivo de parâmetro e os processos de background são startados.

**mount**

Caso o banco já esteja no modo nomount, você pode executar o comando *alter database mount* para mudar a instância para o modo mount, no modo mount ele abre e lê o arquivo de control file, nesse arquivo estão contido por exemplo a localização dos datafiles, mas preste atenção pois ele não abre os arquivos de dados.

**open**

O open é o ultimo passo do startup de uma instância, nesse momento ele abre os arquivos de dados e se certifica que não existem problemas com os datafiles, logo após isso a instância esta liberada para o uso.

Caso você execute um startup sem nenhum parâmetro, o oracle passa por todos os estágios de forma automática.

Finalizando uma instância Oracle:
-

Assim como para iniciar uma instância, também existem diversas combinações para parar uma instância Oracle, agora descrevo cada uma delas com seus respectivos significados.

**shutdown abort**

Um shutdown abort é semelhante a puxar o cabo de energia da tomada, quando esse comando é emitido o oracle não salva nada, todas as operações SQL são encerradas, o oracle não faz rollback das transações e um recovery é executado no proximo startup da instância, esse é um tipo de shutdown inconsistente.

**shutdown immediate**

Nesse tipo de shutdown, o Oracle espera as operações currentes serem finalizadas, as que não sofreram commit são desfeitas e todos os usuários conectados são desconectados da instância, assim temos um shutdown consistente.

**shutdown normal**


Em um shutdown normal o Oracle não aceita novas conexões e ele espera os usuários finalizarem as sessões ativas.
Nesse tipo de parada, o Oracle é finalizado de forma consistente e não precisa de recovery no proximo inicio.

**shutdown transactional**

Ele executa um shutdown consistente, ele espera que os usuários finalizem suas transações, não permite novas conexões, depois de todas as transações forem commitadas ou finalizadas, o usuário que ainda estiver conectado, acaba sendo desconectado do banco de dados.



