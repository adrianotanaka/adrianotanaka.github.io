---
title: Liberando acesso a máquinas na nuvem Oracle.
date: 2017-05-11 00:18:23 Z
categories:
- posts
---


# Liberando acesso a máquinas na nuvem Oracle.

Para liberar o acesso em máquinas da nuvem Oracle, primeiro precisamos conhecer e entender 3 pontos base:
- Security  Applications
- Security lists 
- Security Rules.

Na aba Security Applications, temos acesso a configuração de portas, por padrão a Oracle já nos ajuda deixando configurada as portas mais comuns tais como ssh, listener e rdp, mas também podemos configurar nossas próprias portas indo em **Network->Security Applications-> Create Security Application**, devemos dar um nome para essa porta, escolher qual tipo de protocolo vai tráfegar por ela e por fim o range ou a porta a ser configurada.
Nesse ponto, ainda não estamos liberando nenhum acesso, só estamos configurando as portas que vão ser liberadas.

Imagine uma caixa onde todos os itens dentro dela(no nosso caso VMs) compartilham as mesmas regras e tem livre acesso , é para isso que serve uma **Security List**, todas as VMs que façam parte da mesma Security List herdam e compartilham as mesmas regras, uma máquina pode participar de até 5 Security Lists.
Essa imagem extraida do site da Oracle exemplifica muito bem isso:

![](https://raw.githubusercontent.com/adrianotanaka/adrianotanaka.github.io/master/assets/images/libera_acesso/seclist-1.png)

As Vms Inst_1, Inst_2 e Inst_3 estão  "dentro" da Security List A que é parametrizada para não permitir conexões externas a essas máquinas, mas permitem que elas enviem comunicação para fora da Security List, entre essas máquinas, elas podem se comunicar pelo IP Privado em qualquer porta.

Uma Security Rule, é básicamente uma regra de firewall, já configuramos as portas (Security Applications) e já vimos como agrupar VMs para que elas tenham livre comunicação e compartilhem regras, agora vamos realmente liberar o acesso a elas.


Indo em Network->Security Rules-> Create Security Rules, temos que entrar o nome da nossa regra(coloque um nome fácil), defina se ela vai estar habilitada ou não, escolha a aplicação que você deseja liberar(no nosso caso ssh) ou selecione all se vai liberar todas as portas(muito cuidado com isso!), em source, configure a origem da conexão, no nosso caso vamos selecionar Security IP List e colocar public-internet e destino como uma Security List.
![](https://raw.githubusercontent.com/adrianotanaka/adrianotanaka.github.io/master/assets/images/libera_acesso/seclist-2.png)
Com essa configuração, todas as máquinas que façam parte da Security list selecionada vão ter o acesso liberado.


