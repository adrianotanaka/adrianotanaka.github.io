---
layout: post
title: Interagindo com o Oracle Cloud Storage usando o ftmcli.
date: 2017-11-16 00:18:23 Z
categories:
- posts
---



**Interagindo com o Oracle Cloud Storage usando o ftmcli.**

Nesse artigo vou abordar algumas formas simples de como interagir com um storage na cloud Oracle.

Primeiro faça o download do ftmcli [aqui](http://www.oracle.com/technetwork/topics/cloud/downloads/index.html) , note que somente alguns sistemas operacionais são suportados e que você precisa de alguma dessas versões do  java instalado na máquina:

 - JRE 7   
 - JRE 8   
 - OpenJDK 7  
 - OpenJDK 8

Após o download ter terminado, extraia o arquivo em um local de fácil acesso, esses são os arquivos que devem aparecer:

![](https://i.imgur.com/1AHDCPo.png)

Configurando o arquivo de parâmetros
------------------------------------

    #saving authkey
    #Thu Sep 28 14:54:58 BRT 2017
    segment-size=200
    user=storagesvc-DOMAIN\:adriano.tanakaa@gmail.com
    service=storagevc
    identity-domain=DOMAIN
    retries=5
    storage-class=Standard
    segments-container=all_segments
    auth-url=https\://storagesvc-DOMAIN.storage.oraclecloud.com
    max-threads=15

Onde:

user=serviço-DOMINIO\:usuario_com_permissão

service=Nome do serviço

auth-url=Endereço rest do seu storage

Salvando a senha no arquivo de parâmetros
----------------------------

Antes de fazer o upload, execute o seguinte comando para que sua senha seja salva dentro do arquivo de parâmetros:

java -jar ftmcli.jar describe  --save-auth-key

Essa deve ser a saida do comando

    PS C:\Users\adriano\Desktop\ftmcli> java -jar ftmcli.jar describe  --save-auth-key
    Enter your password:
                     Name: storagesvc-DOMAIN
          Container Count: 4
             Object Count: 7521
               Bytes Used: 540327084332
              Bytes Quota: 1099511627776
           Archive Policy: N/A
    Georeplication Policy: uscom-central-1
    PS C:\Users\adriano\Desktop\ftmcli>

Após isso, a senha não vai ser mais requisitada.

Enviando seu primeiro arquivo para o Storage
----------------------------

Como a senha já foi salva no arquivo de parâmetros, basta executar o seguinte comando:

    java -jar ftmcli.jar upload -s -N  Arquivo.txt NOME_CONTAINER C:\Arquivo.txt
    
Onde **upload** indica a operação que desejamos fazer,  **-s**(minúsculo) indica que caso já exista um arquivo com o mesmo nome, ele não envie novamente, **-N** é o nome que o arquivo vai receber no storage, **NOME_CONTAINER** indica em qual container do storage o arquivo deve ser armazenado, caso ele não exista, vai ser criado automaticamente e por fim qual arquivo estamos enviando.

Enviando todos os arquivos de um diretório
----------------------------

Enviar todos os arquivos de um diretório segue a mesma lógica já apresentada:

    java -jar ftmcli.jar upload -s NOME_CONTAINER C:\Arquivos

A diferença é que apontamos para um diretório e não usamos o parâmetro -N, recomendo deixar o parâmetro -s para que arquivos já existentes não sejam enviados novamente.

Apagando um arquivo
----------------------------

Para apagar um arquivo, devemos passar o nome do container e qual o arquivo que queremos apagar:

     java -jar ftmcli.jar delete -s NOME_CONTAINER Arquivo.txt


Dicas
----------------------------

Preciso usar um arquivo de configuração diferente do padrão(que fica na pasta do executável) :
Use o parâmetro  --properties-file e aponte para o arquivo de configuração.
Exemplo:

     java -jar ftmcli.jar upload -s NOME_CONTAINER C:\Arquivos --properties-file C:\Users\joao\Desktop\ftmcli\ftmcli.properties

