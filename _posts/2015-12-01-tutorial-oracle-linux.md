---
title: "[Tutorial] Instalando e preparando o Oracle Linux 6 para rodar o Oracle 11g
  ou 12c"
date: 2015-12-01 16:40:23 Z
categories:
- posts
---

## [Tutorial] Instalando e preparando o Oracle Linux 6 para rodar o Oracle 11g ou 12c

Para esse tutorial, iremos usar um host Windows e criar uma máquina virtual no VirtualBox rodando Oracle Linux 6.5 e já prepara-la para rodar um banco 11g ou 12c.

**Configuração da máquina virtual:**
![](http://i.imgur.com/ZogFkmL.png)
**- Nome da máquina:** [OracleLinux]

**- Sitema operacional:** Oracle(64bit)

**- Memória:** 1024Mb

**- CPU:** 1 core

**- Tamanho do disco:** 42Gb

## Iniciando a instalação


**1-**Essa é a tela de boas vindas de instalação do Oracle Linux 6.5, nela devemos selecionar a opção **Install or upgrade an existing system** .
![](http://i.imgur.com/H06AEza.png)



**2-**Caso queiram verificar a integridade do disco gravado, selecione a opção **Ok**, mas aviso que essa operação é muito demorada e não recomendada, por isso vamos selecionar a opção **Skip** .
![](http://i.imgur.com/NlfWNAo.png)



**3-**Existem diversas máquinas que são suportadas de forma oficial pelo Oracle Linux, em nosso ambiente essa mensagem vai ser apresentada, informando que nossa máquina não é homologada.

Em um ambiente de produção, isso deve  ser levado em conta, se vamos usar o Oracle linux ou instalar uma outra distribuição que seja homologada para o banco de dados Oracle.

No nosso ambiente de teste, vamos dar um enter e prosseguir com a instalação.
![](http://i.imgur.com/nqA4ieh.png)

**4-** Depois dos passos inicias, somos apresentados a tela do inicio da instalação do Oracle Linux, ela é semelhante as telas de outras distribuições(Red Hat, CentOS, etc).
![](http://i.imgur.com/pF91SBx.png)

**5-** Agora devemos selecionar o idioma e o layout do teclado, aqui recomendo já selecionar a opção ABNT2, pois depois do sistema instalado é meio confuso para trocar essa configuração.


![](http://i.imgur.com/spop0Xg.png)![](http://i.imgur.com/F5LnMNx.png)

**6-** Nesse momento, as configurações vão ser especificas do seu ambiente, agora iremos configurar opções de disco.

Em nosso ambiente, iremos selecionar a primeira opção **Basic storage devices**, clique nela e depois em **Next**
![](http://i.imgur.com/dRtUmyg.png)

**7-** Um alerta de que estamos zerando o disco vai ser exibido, clique em **Yes, discard any data** e prossiga
![](http://i.imgur.com/QkgcM8C.png)

**8-** Devemos selecionar o nome da nossa máquina, caso você possua um dominio, siga esse padrão para definir o nome da máquina: **nome_da_maquina.dominio**

Opcionalmente a rede já pode ser configurada, em nosso ambiente isso não é necessário pois iremos pegar um ip a partir de um dhcp.
![](http://i.imgur.com/VWAt7yA.png)

**9-** Selecione o local onde o servidor estará, isso irá ajustar o fuso horário da máquina.
Selecione a cidade e clique em Next.
![](http://i.imgur.com/Lvixsmi.png)

**10-** 
## IMPORTANTE
Aqui a senha do usuário root deve ser entrada, coloque uma senha que você vai se lembrar, apesar de todo o gerenciamento do banco de dados ser feito com o usuário Oracle(por padrão), iremos precisar do usuário root para realizar algumas configurações.
![](http://i.imgur.com/SlT7tUG.png)

Caso a senha seja fraca, um alerta vai ser exibido, basta clicar em **Use anyway** que a instalação vai continuar.
![](http://i.imgur.com/IVarXov.png)

**11-** Nesse momento iremos criar o layout das nossas partições, isso aqui vai variar bastante em ambiente de produção, em nosso ambiente de teste, iremos criar uma partição de **swap** que é obrigatória para a instalação do banco de dados e depois iremos alocar o espaço restante para o **/**.

**11a-** Selecione a opção **Create custom layout** e clique em next.
![](http://i.imgur.com/44d8WSp.png)
**11b-** Clique no disco desejado e depois em **Create**
![](http://i.imgur.com/i3KgUpv.png)
**11c-**Devemos selecionar a opção **Standard partition**, mas como sempre, isso vai variar de ambiente para ambiente.
![](http://i.imgur.com/LFtUR8G.png)
**11c-**Selecione no **File System type** a opção swap e em **Size(MB)** configure de acordo com a quantidade de memória ram.

[Aqui](http://docs.oracle.com/cd/B28359_01/install.111/b32002/pre_install.htm) você encontra uma tabela com as configurações ideais de swap.
![](http://i.imgur.com/oS18my5.png)

**11d-** Depois vamos alocar o espaço restante para a partição **/**.

Clique novamente no disco e agora o tipo da partição deve ser ext4, marque as opções **Fill to maximum allowable size** e **Force to be a primary partition**
![](http://i.imgur.com/pDxEPbo.png)

**11e-** Esse deve ser o layout final
![](http://i.imgur.com/QpFNw72.png)

**12-** Nessa tela não iremos mudar nada, apenas clique em next
![](http://i.imgur.com/KiKeHkC.png)

**13-** A partir desse momento, iremos selecionar alguns pacotes necessários para o banco de dados, o que vai facilitar bastante a nossa vida.

Marque a opção **Desktop** e depois a opção **Customize now** e depois **Next**

**14-** Vá em **Server** e marque a opção **System administration tools**.
![](http://i.imgur.com/ZCHDPP7.png)

**15-** Clique em **Optional Package** e procure pelo pacote **oracle-rdbms-server-11gr2-preinstall** marque ele e depois em ok e **Next**
![](http://i.imgur.com/4O5CGzP.png) 

**16-** Espere os pacotes serem instalados, já estamos quase terminando.
![](http://i.imgur.com/RV2HShp.png)

**17-** Depois da espera, a tela de sucesso.

Clique em reboot e aguarde.
![](http://i.imgur.com/btFgPBY.png)


## Se tudo tiver corrido com sucesso, a tela de boas vindas vai ser apresentada e agora vamos para a parte final da nossa instalação

![](http://i.imgur.com/XLFrLeK.png)

**18-** Aceite o contrato de instalação
![](http://i.imgur.com/ssuX50V.png)

**19-** Caso você possua contrato de suporte  com a Oracle, vá na opção **Yes**, como não possuimos, vamos na opção **No** e clique em **forward**
![](http://i.imgur.com/HO7FlIG.png)

**20-** Como selecionamos o pacote pre-install, não vamos precisar criar nenhum usuário, apenas clique em **forward**
![](http://i.imgur.com/ebbxpgv.png)

**21-**Confirme  as opções de hora e fuso
![](http://i.imgur.com/P07esOq.png)

**22-** Caso a quantidade de memória seja suficiente, podemos usar o kdump nesse caso ele vem até desativado, clique em finish e finalizamos
![](http://i.imgur.com/oId3cd6.png)

## A tela mais aguardada, caso esteja vendo ela, estamos prontos para poder instalar o banco de dados, mas isso fica para um próximo tutorial.

![](http://i.imgur.com/WNwz638.png)

Qualquer duvida na instalação, podem me enviar e-mails para adriano.tanakaa@gmail.com
