---
title: Enviando e-mail no Oracle usando SSL/TLS.
date: 2020-02-27 21:25:23 Z
categories:
- posts
---


Atualmente criptografar os dados trafegados é uma coisa fundamental, inclusive a Oracle [liberou o uso de algumas funcionalidades de criptografia para versões não Enterprise](https://oracle-base.com/blog/2015/06/27/native-network-encryption-not-part-of-advanced-security-option/) , e a  criptografia de e-mail está implementada nos grandes fornecedores de tecnologia. 

Então preparei o seguinte tutorial de como se configurar uma Wallet, importar os certificados e também um exemplo bem útil de como enviar o e-mail.

Vamos utilizar o Gmail para testes e você precisa ativar o acesso à aplicativos menos seguros senão mesmo com as credenciais corretas o login não vai ser possível:

[https://support.google.com/accounts/answer/6010255?p=lsa_blocked&hl=pt-BR&visit_id=637184509838379444-493512541&rd=1](https://support.google.com/accounts/answer/6010255?p=lsa_blocked&hl=pt-BR&visit_id=637184509838379444-493512541&rd=1)


## Baixando o certificado

O primeiro passo para isso é baixar o certificado que vai ser utilizado para a autenticação segura no serviço de e-mail.
Usando o Google Chrome, basta acessar o site (nesse caso o Gmail) e clicar no cadeado na barra de navegação (1) e após isso clicar no campo certificado(2):

![enter image description here](https://i.imgur.com/xbJuRTh.png)

Na janela que abrir, clique em Caminho de certificação (1), depois clique no certificado root (o primeiro de cima para baixo)  e por fim use a opção Exibir certificado (2):

![enter image description here](https://i.imgur.com/Ma4rLQ5.png)

Na janela que se abrir, clique  na aba detalhes e depois Copiar para arquivo e siga os passos apresentados para salvar o certificado na sua máquina, copie o arquivo gerado para o servidor de banco de dados.

## Criando a Wallet

Para essa autenticação, precisamos passar o certificado do passo anterior nas nossas chamadas e para isso vamos criar uma wallet seguindo os passos:

 Criar a Wallet:

    [oracle@db-orig email_ssl]$ orapki wallet create -wallet $PWD/wallet_email -pwd Oracle123 -auto_login
    Oracle PKI Tool Release 19.0.0.0.0 - Production
    Version 19.3.0.0.0
    Copyright (c) 2004, 2019, Oracle and/or its affiliates. All rights reserved.
    
    Operation is successfully completed.

Adicionar o certificado à Wallet:

    [oracle@db-orig email_ssl]$ orapki wallet add -wallet $PWD/wallet_email/ewallet.p12 -trusted_cert -cert gmail.com.cer -pwd Oracle123
    Oracle PKI Tool Release 19.0.0.0.0 - Production
    Version 19.3.0.0.0
    Copyright (c) 2004, 2019, Oracle and/or its affiliates. All rights reserved.
    
    Operation is successfully completed.

Listar os certificados presentes em uma Wallet:

    [oracle@db-orig email_ssl]$ orapki wallet display -wallet $PWD/wallet_email/ -pwd Oracle123
    Oracle PKI Tool Release 19.0.0.0.0 - Production
    Version 19.3.0.0.0
    Copyright (c) 2004, 2019, Oracle and/or its affiliates. All rights reserved.
    
    Requested Certificates: 
    User Certificates:
    Trusted Certificates: 
    Subject:        CN=GlobalSign,O=GlobalSign,OU=GlobalSign Root CA - R2
    [oracle@db-orig email_ssl]$ 
#### Dica:
É importante lembrar o caminho e a senha utilizada na Wallet pois vamos aponta-los no nosso envio de email.

## Criando uma ACL

Precisamos criar uma ACL que é o recurso que controla as conexões de rede de um determinado usuário.

    begin
      dbms_network_acl_admin.create_acl (
        acl         => 'acl_gmail.xml',
        description => 'Libera conexao ao GMAIL',
        principal   => 'USUARIO_QUE_ENVIA_EMAIL',
        is_grant    => TRUE,
        privilege   => 'connect',
        start_date  => null,
        end_date    => null
      );
     
      dbms_network_acl_admin.add_privilege (
        acl        => 'acl_gmail.xml',
        principal  => 'USUARIO_QUE_ENVIA_EMAIL',
        is_grant   => TRUE,
        privilege  => 'resolve',
        start_date => null,
        end_date   => null
      );
     
      dbms_network_acl_admin.assign_acl (
        acl        => 'acl_gmail.xml',
        host       => '*.gmail.com',
        lower_port => 587,
        upper_port => 587
      );
      commit;
    end;
    /

## Testando a nossa conexão

Para testar nossa conexão, podemos utilizar a package UTL_HTTP da seguinte forma:

Sem apontar a Wallet e tentando se conectar no servidor:

    SQL> SELECT UTL_HTTP.REQUEST('https://gmail.com') FROM DUAL;
    SELECT UTL_HTTP.REQUEST('https://gmail.com') FROM DUAL
           *
    ERROR at line 1:
    ORA-29273: HTTP request failed
    ORA-06512: at "SYS.UTL_HTTP", line 1530
    ORA-29024: Certificate validation failure
    ORA-06512: at "SYS.UTL_HTTP", line 380
    ORA-06512: at "SYS.UTL_HTTP", line 1470
    ORA-06512: at line 1

Apontando a Wallet criada:

    SQL> select utl_http.request('https://gmail.com',NULL,'file:/home/oracle/email_ssl/wallet_email','Oracle123') from dual;
    
    UTL_HTTP.REQUEST('HTTPS://GMAIL.COM',NULL,'FILE:/HOME/ORACLE/EMAIL_SSL/WALLET_EMAIL','ORACLE123')
    ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    
    <!DOCTYPE html>
    <html lang="pt">
      <head>
      <meta charset="utf-8">
      <meta content="width=300, initial-scale=1" name="viewport">
      <meta name="description" content="O Gmail e um e-mail intuitivo, eficiente e util. S?o 15 GB de armazenamento, acesso em dispositivos moveis e menos spam.">
      <meta name="google-site-verification" content="LrdTUW9psUAMbh4Ia074-BPEVmcpBxF6Gwf0MSgQXZs">
      <title>Gmail</title>
      <style>
      @font-face {
      font-family: 'Open Sans';
      font-style: normal;
      font-weight: 300;
      src: local('Open Sans Light'), local('OpenSans-Light'), url(//fonts.gstatic.com/s/opensans/v15/mem5YaGs126MiZpBA-UN_r8OUuhs.ttf) format('truetype');
    }
    @font-face {
      font-family: 'Open Sans';
      font-style: normal;
      font-weight: 400;
      src: local('Open Sans'), local('OpenSans'), url(//fonts.gstatic.com/s/opensans/v15/mem8YaGs126MiZpBA-UFVZ0e.ttf) format('truetype');

}

> Cortei o resultado para ocupar menos espaço.
>

### Código para enviar e-mails no Oracle usando SSL:

Abaixo um pequeno código de exemplo de como utilizar o que foi apresentado aqui nesse tutorial:


<script charset="UTF-8" src="https://gist-it.appspot.com/github.com/adrianotanaka/scripts/blob/master/oracle/simple_html_mail.sql?footer=minimal"></script>




> Written with [StackEdit](https://stackedit.io/).
