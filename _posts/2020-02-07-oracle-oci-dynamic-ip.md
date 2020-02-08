---
title: Script para criar VPN no OCI quando se tem IP público dinâmico.
date: 2020-02-07 21:25:23 Z
categories:
- posts
---



Sabemos que um dos itens para se fechar uma VPN entre dois ambientes é o IP público e que o ideal é que ele seja fixo para que não precisemos estar reconfigurando a VPN toda vez que ele mude, mas e quando isso não é possível?
Pensando em não deixar nenhuma porta aberta no ambiente OCI, criei o seguinte script que identifica o IP público de onde está sendo executado e a partir dai cria todos os recursos necessários para se ter uma VPN (cpe, ipsec, route...) e ao fim exibe as informações do túnel:

![enter image description here](https://i.imgur.com/K3t6c90.png)

A ideia é que ele seja colocado em um crontab ou outro agendador em um servidor linux na sua estrutura local.
Abaixo os requisitos para que ele possa funcionar:

- Ter configurado o oci-cli 
- Ter criado uma vcn e um drg
- Ter os utilitários dig, jq instalados
- Ajustar a função func_vars() com as informações do seu ambiente:

    func_vars () {
    
            var_compartment="ocid do seu compartimento" #Your compartment ocid
            var_drg="ocid do seu drg" #Your drg ocid
            var_routes=""'["Sua rede(s) interna no formato CIDR"]'"" #Your onp routes
            var_list="Sua rede(s) interna no formato CIDR" #The same of route
            var_vcnd_id="ocid da sua vcn" #Your VNC ocid
            var_dev_name="Um nome para ser concatenado ao nome dos recursos" #Name of created resource, reused on all resources
    }
                   
<script charset="UTF-8" src="https://gist-it.appspot.com/github.com/adrianotanaka/scripts/blob/master/oci/vpn-dynamic-ip.sh?footer=minimal"></script>

> Written with [StackEdit](https://stackedit.io/).
