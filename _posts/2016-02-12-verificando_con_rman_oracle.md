---
title: "[ORACLE] Verificando a consistência de um arquivo de backup do RMAN"
date: 2016-02-12 00:00:00 Z
categories:
- Post Formats
---

Hoje me deparei com um problema gravíssimo, um backup do Oracle não estava restaurando e o seguinte erro estava sendo exibido:

```shell
channel c1: ORA-19870: error while restoring backup piece /media/bkp/INC0

ORA-19599: block number 793607 is corrupt in backup piece /media/bkp/INC0
```

Para verificar se o backup está ou não integro, podemos usar dois utilitários, um chamado dbv(DBVERIFY) que foi feito pela Oracle e pode ser usado para verificar arquivos de backup e também datafiles e o outro um bem difundido que é o md5sum.

O dbv possui os seguintes parâmetros: 

|  Parametro | Descrição  |
| ------------ | ------------ |
|  **FILE**  |   O nome do arquivo a ser verificado |
|  **START** | O bloco Oracle onde a verificação deve começar  |
|  **END** |  O bloco Oracle onde a verificação deve terminar |
|  **BLOCKSIZE** | Deve ser especificado apenas se o erro DBV-00103 for apresentado  |
|  **LOGFILE** |  Arquivo de log da operação  |
|  **FEEDBACK** | Mostra com um . (ponto) o progresso  |
|  **HELP** |  Exibe a ajuda |
|  **PARFILE** | Um arquivo que contém parâmetros de configuração   |

Em uma verificação simples, vamos usar apenas o seguinte comando:

`dbv file=INC1_xxxx_ `

A saída do comando é bem simples:

```shell
DBVERIFY - Verification complete

Total Pages Examined         : 20946
Total Pages Processed (Data) : 0
Total Pages Failing   (Data) : 0
Total Pages Processed (Index): 0
Total Pages Failing   (Index): 0
Total Pages Processed (Other): 1
Total Pages Processed (Seg)  : 0
Total Pages Failing   (Seg)  : 0
Total Pages Empty            : 0
Total Pages Marked Corrupt   : 20945
Total Pages Influx           : 0
Total Pages Encrypted        : 0
Highest block SCN            : 2078742100 (2121.2078742100)
```

Como pode ser visto, o arquivo verificado possuí 20946 "pages" e dessas 20945 estão corrompidas.
Nesse caso o arquivo pode ter sido corrompido durante a copia e como verificamos isso? 

# Usando o md5sum

O md5sum é um utilitário presente em distribuições linux diversas e tem o mesmo comportamento, seja ela CentOs, Oracle Linux, Ubuntu, etc.

Devemos primeiro verificar o md5 gerado no ambiente de produção:

```shell
[oracle@producao]$ md5sum INC1_xxxxx
84123cf312343d8159adcc21912357cc  INC1_xxxxx
```

E depois o gerado na maquina de teste:

```shell
[oracle@teste]$ md5sum INC1_xxxxx
92124jf312344v8159adcc33912147xx  INC1_xxxxx
```

Como podemos ver, o arquivo esta com md5 diferentes, ou seja, apesar do tamanho e nome ser o mesmo, o arquivo é diferente.

Nesses caso, recomendo que uma nova cópia seja feita e os md5 validados antes de emitir o restore.


Fontes: 
[Oracle DBV ](https://docs.oracle.com/cd/B10501_01/server.920/a96652/ch13.htm "Oracle DBV ")
