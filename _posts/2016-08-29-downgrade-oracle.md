---
title: Como realizar downgrade de versões no Oracle
date: 2016-08-29 10:51:23 Z
categories:
- posts
---

Como realizar downgrade de versões no Oracle
===================


Uma operação não muito comum mas que as vezes é necessária de se fazer, é realizar downgrade de versões do Oracle.
Talvez você possua em seu ambiente de produção a versão 11g e em seu ambiente de teste/homologação uma versão mais antiga e precise rotineiramente atualizar essa base a partir do banco mais novo.

Para isso, usaremos o utilitário expdp com uma cláusula especificando a versão desejada.

----------


O utilitário expdp
-------------

Nas versões mais antigas do Oracle, quando iamos realizar operações relacionadas à export e import, era necessário usar o exp e o imp respectivamente.

Nas versões mais novas, foi introduzido o DataPump, um utilitário muito mais robusto e com diversas melhorias.

> **Nota:**

> - Veja [aqui](https://docs.oracle.com/cd/B19306_01/server.102/b14215/dp_overview.htm) a documentação completa do DataPump.

#### 1-Criando um diretório no banco de dados para ser usado pelo DataPump:

> - CREATE OR REPLACE DIRECTORY DUMP AS '/backup';

#### 2-Dando permissão de leitura/escrita no diretório criado:
>- GRANT READ,WRITE ON DIRECTORY DUMP TO usuario;



####  Cláusula version

Para realizar o downgrade, devemos especificar uma clausula no momento do export, caso não façamos isso, não será possível importar na versão mais antiga.

>- expdp userid=usuario/senha directory=DUMP dumpfile=expdp_NOME_DO_ARQUIVO.dmp logfile=expdp_NOME_DO_LOG.log OPCOES DE EXPORT **VERSION**=10.2.0.4.0 

Especifique na clausula version, a versão do banco onde vai ser importado o dump.
Nesse exemplo, o backup pode ser feito tanto no Oracle 11g quanto no 12c que vai ser possível importa-lo na versão 10g.

Para realizar o import, não tem nenhuma observação.
