---
layout: post
title: Realizando acesso a uma máquina no OCI via console
date: 2019-04-28 00:00:23 Z
categories:
- posts
---

# Realizando acesso a uma máquina no OCI via console

Um dos recursos mais solicitados do OCI-C era a possibilidade de realizar uma conexão direto ao console da máquina, quantas vezes alguém já colocou um disco que não estava na orquestração no fstab de uma máquina e ao reiniciar a máquina não subiu? Ou então bagunçou o firewall do Windows?
Agora no OCI é possível realizar a conexão direto a console da máquina de forma bem simples.

**IMPORTANTE** ressaltar que apesar de garantir um acesso via console, você ainda precisa das credências de acesso dessa máquina.


## 1.Gerando uma chave SSH

Existem diversas formas de gerar uma chave SSH, se você estiver em um ambiente Windows, basta baixar o utilitário puttygen ([aqui](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)) , e ao abrir, passar o mouse na área onde a seta vermelha está apontando para poder gerar uma chave randômica:

![](https://i.imgur.com/Lsg5auV.png)

Depois disso basta copiar todo o código gerado:

![enter image description here](https://i.imgur.com/ra9AbM3.png)

Clique em **Save private Key** para salvar o par da chave gerada, é recomendado colocar uma senha nela no campo **Key passphrase** 

## 2.Softwares necessários no ambiente Windows
Caso você esteja tentando acessar o console a partir de uma máquina Windows, você vai precisar dos seguintes Softwares:

 - [plink](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
  - [Vnc
   Viewer]([https://www.realvnc.com/pt/connect/download/viewer/](https://www.realvnc.com/pt/connect/download/viewer/))

**Recomendo colocar a localização dos arquivos baixados na variável $PATH do Windows para facilitar sua vida.**


## 3.Ativando o acesso ao console via Dashboard

Parar conseguir acesso a sua máquina via console, você precisa ter uma regra no "firewall" (Security List) que permita a conexão na porta 443 de sua instância. 
Segue um exemplo bem simples que permite essa conexão de qualquer lugar vindo da internet .
***MUITO CUIDADO COM REGRAS DESSE TIPO***
![](https://i.imgur.com/2fLYxaG.png)

Depois da regra criada, acesse a página de sua instância e procure pela opção **Console Connection**

![](https://i.imgur.com/ghjJmEH.png)

Na tela que abrir, clique em **(1)** Create Console Connection,  **(2)** pegue a chave gerada no passo 1 e cole no campo de texto que apareceu (Lembre-se de copiar todo o texto da chave) e **(3)** clique em Create Console Connection.

![](https://i.imgur.com/OnQ8ESv.png)

Após Clicar em Create Console Connection, aguarde alguns segundos enquanto a conexão é configurada.
Quando ela estiver pronta o status vai mudar para active:

![](https://i.imgur.com/YuA24Tn.png)

Na área marcada de vermelho devemos escolher como queremos conectar na máquina, nesse caso, escolha a opção Connect With VNC e a seguinte janela vai ser exibida, copie TODO o código que ela gerou (Você pode usar o botão copy em baixo dela para fazer isso)

![](https://i.imgur.com/4tRZlq1.png)

### Dica
Esse comando gerado vem com alguns diretórios que talvez na sua máquina não esteja presentes, então você precisa alterar algumas coisas:

Código gerado pela Oracle:

> Start-Job { Echo N | plink.exe -i
> $env:homedrive$env:homepath\oci\console.ppk -N -ssh -P 443 -l
> ocid1.instanceconsoleconnection.oc1.iad.abuwcljre2f4z7llzpqwi6jw75w2pxukkumxqvfejd733lwbpxuecwiviq5a
> -L 5905:ocid1.instance.oc1.iad.abuwcljr4kmcdbdlz4zv2sdqu25qg3ym6vmgi4xdvds3ggqgfe2uy2prczcq:5905
> instance-console.us-ashburn-1.oraclecloud.com }; sleep 5; plink.exe -i
> $env:homedrive$env:homepath\oci\console.ppk -N -L 5900:localhost:5900
> -P 5905 localhost -l ocid1.instance.oc1.iad.abuwcljr4kmcdbdlz4zv2sdqu25qg3ym6vmgi4xdvds3ggqgfe2uy2prczcq

Código alterado para funcionar na minha máquina:

>  Start-Job { Echo N | plink.exe -i
> **C:\Users\Adriano\Desktop\private.ppk** -N -ssh -P 443 -l ocid1.instanceconsoleconnection.oc1.iad.abuwcljre2f4z7llzpqwi6jw75w2pxukkumxqvfejd733lwbpxuecwiviq5a
> -L 5905:ocid1.instance.oc1.iad.abuwcljr4kmcdbdlz4zv2sdqu25qg3ym6vmgi4xdvds3ggqgfe2uy2prczcq:5905
> instance-console.us-ashburn-1.oraclecloud.com }; sleep 5; plink.exe -i
> **C:\Users\Adriano\Desktop\private.ppk**  -N -L 5900:localhost:5900 -P 5905 localhost -l
> ocid1.instance.oc1.iad.abuwcljr4kmcdbdlz4zv2sdqu25qg3ym6vmgi4xdvds3ggqgfe2uy2prczcq

Perceba que no meu caso, eu apontei diretamente o arquivo de chave privada gerada no passo 1 e que também não estou passando o caminho do utilitário plink.exe pois o mesmo já está adicionado na minha variável PATH.

Esse código deve ser executado no PowerShell:

![](https://i.imgur.com/UaXQj81.png)

Caso esse seja a sua primeira conexão à essa máquina, você vai ser questionado se deseja salvar o hash dessa conexão, basta confimar ( Y )  e quando o acesso for garantido (Access granted. Press Return to begin session.) você deve dar um enter para iniciar o túnel.

Abra o Vnc Viewer e inicie uma conexão no endereço localhost:5900 

![](https://i.imgur.com/DyyeXJh.png)


E seja bem vindo a sua máquina:
![](https://i.imgur.com/jAyWcBo.png)

### Dica
Dependendo do programa de vnc que esteja usando, talvez ele não possua a opção de enviar um CTRL + ALT + DEL para desbloquear o Windows, então você pode clicar no ícone de acessibilidade no canto inferior esquerdo e usar o teclado virtual para isso.
