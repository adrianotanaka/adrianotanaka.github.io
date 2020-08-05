---
title: Gerenciando instancias no OCI usando oci cli e uma instância Always free.
date: 2020-08-04 21:25:23 Z
categories:
- posts
---

Esse artigo tem com objetivo mostrar uma forma simples e prática de se utilizar o *oci cli* em conjunto com uma instância *Always Free* para controlar o lifecycle de suas instâncias no OCI, ao final você será capaz de entender um pouco mais do *oci cli* e também agendar o stop/start da instância para economizar créditos.

**Itens necessários:**

Uma máquina do tipo Always free ou linux com o oci cli instalado e configurado para se autenticar no seu ambiente.

Comandos disponíveis
Para gerenciar uma instância usando o oci cli vamos usar o comando oci compute instance com o parâmetro action com os seguintes valores:

**START:** Inicia a instância
**STOP:** Para a instância
**RESET:** Reinicia a instância
**SOFTSTOP:** Emite comandos de sistema operacional para a VM e ela é parada
**SOFTRESET:** Emite um comando de reiniciar para a VM nos mesmos moldes do SOFTSTOP

Note que até o momento apenas as opções SOFTSTOP e  SOFTRESET estão disponíveis via dashboard:

[![](https://i.imgur.com/LBvfgg6.png)](https://i.imgur.com/LBvfgg6.png)

Obrigatoriamente precisamos passar um valor para o parâmetro *- -action* e também o *ocid* da instância a ser manipulada, para retornar todas as instâncias de um compartimento você pode executar o seguinte comando passando o *ocid* do compartimento:

```shell
oci compute instance list -c ocid1.compartment.oc1..XXXXXX | jq -r '.data[] | "Inst: \(."display-name") | Status: \(."lifecycle-state") | OCID: \(.id)"'
```

Ele vai te mostrar a seguinte informação:

[![](https://i.imgur.com/qI882Xo.png)](https://i.imgur.com/qI882Xo.png)

#### Parando a instância:

Para desligar uma máquina você pode utilizar os valores **STOP** ou **SOFTSTOP** no parâmetro *- -action*:

```shell
oci compute instance action --action STOP --instance-id ocid1.instance.oc1.sa-saopaulo-1.XXXXXXXXXXX
```


```shell
oci compute instance action --action SOFTSTOP --instance-id ocid1.instance.oc1.sa-saopaulo-1.XXXXXXXXXXX
```

[![](https://i.imgur.com/KPgUgNj.png)](https://i.imgur.com/KPgUgNj.png)


Perceba que ao usar o valor **STOP** (marcado de vermelho) para a máquina seria o mesmo que puxar o cabo de energia, enquanto que ao usar **SOFTSTOP** (em azul) a máquina realmente recebe um comando de reiniciar (shutdown), por isso o uso de **STOP** não é recomendado a não ser que a máquina esteja travada e não possa receber o comando.

#### Iniciando a instância:

Para colocar a instância no ar, basta executar o comando com o parâmetro *- -action* e o valor **START**:

```shell
oci compute instance action --action START --instance-id ocid1.instance.oc1.sa-saopaulo-1.XXXXXXXXXXX
```


#### Agendando o desligamento automático de instâncias:

Depois de descobrir a dinâmica do comando oci compute instance action fica fácil de configurar um desligamento/ligamento automático das VMs:

Crie um script chamado *shutdown_vm.sh* com o seguinte conteúdo para desligar a instância:

```shell
export OCI_CLI_AUTH=instance_principal
/root/bin/oci compute instance action --action SOFTSTOP --instance-id $1 >> /home/opc/shutdown_$1.log 2>&1 
```

Crie um script chamado *startup_vm.sh* com o seguinte conteúdo:

```shell
export OCI_CLI_AUTH=instance_principal
/root/bin/oci compute instance action --action START --instance-id $1 >> /home/opc/shutdown_$1.log 2>&1 

```

E agende no crontab passando o ocid para o script:


[![](https://i.imgur.com/I1VGJeW.png)](https://i.imgur.com/I1VGJeW.png)

Dessa forma a sua máquina vai ser desligada e ligada no horário agendado além de gerar um log para cada uma das máquinas.






