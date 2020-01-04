---
title: Criando um Standby manual no Oracle
date: 2020-01-04 00:18:23 Z
categories:
- posts
---



# Criando um Standby manual no Oracle

As vezes precisamos realizar a replicação de um banco de dados muito grande para outro servidor, nesse tipo de cenário uma das melhores ferramentas seria o uso do Data Guard mas como pode ser visto, essa opção não contempla a versão Standard:

![enter image description here](https://i.imgur.com/oM9FyCM.png)

Uma opção seria realizar um backup e de forma manual realizar o recover de tempos em tempos para manter os ambientes sincronizados, esse script que compartilho agora serve para facilitar um pouco esse trabalho.

> Nota: Nesse artigo não vou cobrir a criação e restauração do backup, vamos supor que isso já tenha sido realizado.

## Preparação do ambiente

### Pacotes
Você vai precisar instalar em ambos os servidores o programa rsync caso ele não esteja instalado, nas distribuições baseadas em Red Hat (CentOS, Oracle Linux..) você pode usar o seguinte comando:

    yum install rsync -y

### Estabelecer confiança entre as máquinas

O ideal é que a relação de confiança ssh entre as máquina seja configurada para que você não precise digitar a senha do usuário do sistema operacional toda vez que o script executar.

#### Na máquina de destino executar os seguintes comandos
>Caso você já possua uma chave publica criada, não execute esse comando pois ele vai sobrescrever a que você já possui

    ssh-keygen -t rsa   
    
   ![enter image description here](https://i.imgur.com/6AgOT0r.png)

Copiar a chave para o servidor de origem:

    ssh-copy-id oracle@ip-producao 
A partir desse momento você pode se conectar do servidor de destino para o de origem sem precisar de senha.

### tnsnames.ora
Após isso, devemos configurar duas entradas no tnsnames.ora para que seja possível verificar se os ambientes estão sincronizados:

    DB_PRD =
     (DESCRIPTION = 
        (ADDRESS_LIST =
             (ADDRESS = (PROTOCOL = TCP)(HOST = db-orig)(PORT = 1521))
    	    )
    	     (CONNECT_DATA =
    	        (SERVICE_NAME = dborcl)
    		 )
    		 )
    		 
    DB_STBY =
      (DESCRIPTION = 
         (ADDRESS_LIST =
              (ADDRESS = (PROTOCOL = TCP)(HOST = db-dest)(PORT = 1521))
    	     )
    	      (CONNECT_DATA =
    	         (SERVICE_NAME = dborcl)
    		  )
    		  )


### Variáveis de ambiente

Você precisa criar um arquivo com as seguintes variáveis de ambiente:

    export DIR_PRD=/backup/oracle/archive #Diretório de origem
    export DIR_STD=/backup/oracle/archive #Diretório de destino
    export CLIENTE="XXXX"
    export IP_PRD=192.168.10.180  #IP do servidor de origem
    export IP_STD=192.168.10.183  #IP do servidor de destino
    export DATA=$(date +"%Y%m%d")
    export HORA=$(date +"%T")
    export LOG_DIR=/home/oracle/$CLIENTE/logs
    export LOG_FILE=standby-$ORACLE_SID-$DATA
    export SYS_PASSWD=SENHA_DO_USUARIO_SYS
   
> Recomendo que ele seja criado no diretório /home/oracle/scripts/ com o nome env$ORACLE_SID, caso crie em outro diretório você precisa ajustar a linha 51 do script.

### Execução do script
O script deve ser executado na máquina de destino passando o $ORACLE_SID do banco que você deseja sincronizar.


## O Script

<script charset="UTF-8" src="https://github.com/adrianotanaka/scripts/blob/master/oracle/standby.sh?footer=minimal"></script>
