---
title: Criando uma rede própria no ambiente Oracle Cloud
date: 2017-05-11 00:18:23 Z
categories:
- posts
---

## Criando uma rede própria no ambiente Oracle Cloud

No Dashboard de Compute do Oracle Cloud, temos acesso a nossas configurações de rede.
Essas configurações dividem-se em duas partes: Shared Network e IP Network, na primeira opção, a alocação de IPs vai ser realizada usando um pool de ips disponibilizados pela Oracle.
Na segunda opção(IP Network) temos mais liberdade para configurar a faixa de IP de acordo com a nossa necessidade.
Vale ressaltar ques uma VM pode utilizar tanto Shared Network quanto IP Network ao mesmo tempo, mas que na opção Shared Network o IP privado vai ser alterado toda vez que a máquina for reiniciada enquanto que na IP Network ele vai ficar fixo de acordo com a sua configuração.

Nesse exemplo, vamos criar um IP Network com nossa faixa de IP e criar uma VM que vai usar também a Shared Network para liberar acesso a internet, segundo a documentação da Oracle, poderiamos liberar acesso pela IP Network mas até a presente data (11/05/2017) essa opção não esta disponível(em ambiente trial).


Acesse a aba de Network e procure pela opção IP Network e clique em Create IP Network
![](https://raw.githubusercontent.com/adrianotanaka/adrianotanaka.github.io/master/assets/images/rede_fixa/ip_network_1.png)

Dê um nome para essa rede, preencha a faixa de ip desejada, aqui vou usar a 40.40.40.0/24, a opção IP Exchange serve para liberar a comunicação entre duas faixas diferentes, no meu ambiente possuo a rede_fixa na faixa 50.50.50.1/24, caso eu quisesse que as duas se comunicassem, deveria ir em IP Network-> IP Exchange, criar uma nova entrada e atribuir nas duas redes.
![](https://raw.githubusercontent.com/adrianotanaka/adrianotanaka.github.io/master/assets/images/rede_fixa/ip_network_2.png)

Agora vamos criar uma nova VM e atribuir essa faixa de ip criada a ela.

Acesse a opção Instances e clique em Create Instance 

Selecione o sistema operacional desejado:
![](https://raw.githubusercontent.com/adrianotanaka/adrianotanaka.github.io/master/assets/images/rede_fixa/instance_1.png)
Selecione o shape desejado:
![](https://raw.githubusercontent.com/adrianotanaka/adrianotanaka.github.io/master/assets/images/rede_fixa/instance_2.png)
Por ser uma máquina Linux(no meu caso) precisamos vincular uma chave ssh para permitir a conexão, caso nunca tenha feito isso, pode usar esse tutorial aqui  http://www.oracle.com/webfolder/technetwork/tutorials/obe/cloud/compute-iaas/creating_an_ssh_enabled_user/creating_an_ssh_enabled_user.html 

Dê um nome fácil para a vm e clique em Next(seta no canto superior direito)
![](https://raw.githubusercontent.com/adrianotanaka/adrianotanaka.github.io/master/assets/images/rede_fixa/instance_3.png)

Chegamos na parte onde vamos atribuir a nossa rede criada a essa VM.

Deixe a opção Shared Network marcada(1) e a opção IP Network também marcada (2), clique em Configure interface para fazer o vinculo(3).
![](https://raw.githubusercontent.com/adrianotanaka/adrianotanaka.github.io/master/assets/images/rede_fixa/instance_4.png)
Na tela que vai abrir, selecione a rede criada anteriormente(rede_fixa2), você também pode selecionar qual vai ser a placa de rede vinculada a esse ip (eth0) e definir o ip fixo que essa máquina vai receber.
![](https://raw.githubusercontent.com/adrianotanaka/adrianotanaka.github.io/master/assets/images/rede_fixa/instance_5.png)

Depois de preencher de acordo com a sua necessidade, clique em Review and Create e depois em Create.
Depois de preencher de acordo com a sua necessidade, clique em Review and Create e depois em Create. 

Quando a instância estiver pronta, você pode conferir que ela vai ter uma vNIC vinculada a ela na faixa que configuramos:

E a nível de SO ela também vai ser exibida:

{% highlight bash %}
[opc@bc28d1 ~]$ ifconfig
eth0      Link encap:Ethernet  HWaddr C6:B0:28:28:28:0A
          inet addr:40.40.40.10  Bcast:40.40.40.255  Mask:255.255.255.0
          inet6 addr: fe80::c4b0:28ff:fe28:280a/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:8900  Metric:1
          RX packets:143 errors:0 dropped:0 overruns:0 frame:0
          TX packets:302 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:22904 (22.3 KiB)  TX bytes:33342 (32.5 KiB)

eth1      Link encap:Ethernet  HWaddr C6:B0:71:BF:DF:2D
          inet addr:100.65.1.18  Bcast:100.65.1.19  Mask:255.255.255.252
          inet6 addr: fe80::c4b0:71ff:febf:df2d/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:9000  Metric:1
          RX packets:828 errors:0 dropped:0 overruns:0 frame:0
          TX packets:822 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:102147 (99.7 KiB)  TX bytes:74253 (72.5 KiB)
{% endhighlight %}


