---
title: "Configuração Básica do Switch 2600 da Cisco"
description: Neste post, apresentamos de forma detalhada o passo a passo para efectuar uma configuração base no Switch Série 2600.
author: Celestino
date: 2024-11-12 11:33:00 +0800
categories: [Networking]
tags: [Switch, Cisco]
pin: true
image:
  path: /assets/img/post_images/devices-mockup.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Responsive rendering of Chirpy theme on multiple devices.
---

## Objectivo
{: data-toc-skip='' .mt-4 .mb-0 }

Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.

## Público Alvo
- Estudantes;
- Entusiatas;
- Adminsitradores de Redes e Sistemas;
- Administradores de Segurança em Redes.

## Requisitos
Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries

## Topologia

![Desktop View](/assets/img/post_images/topologia.png){: width="972" height="589" }
_Full screen width and center alignment_

## Configurações

#### Passo1: Resetar as configurações iniciais.
```text
Switch#delete flash:vlan
Switch#delete flash:vlan.dat
Delete filename [vlan.dat]?
Delete flash:vlan.dat? [confirm]
```
```text
Switch# delete nvram:startup-config
Delete filename [startup-config]?
Delete nvram:startup-config? [confirm]
```
#### Passo2: Configuração de Hostname.
```text
Switch# configure terminal
Switch(config)# hostname ItLab
ItLab(config)#
```
#### Passo3: Configuração de um endereço IP.
```text
ItLab(config)# interface vlan 1
ItLab(config-if)# ip address 192.168.0.1 255.255.255.0
ItLab(config-if)# do show ip interface brief

Interface              IP-Address      OK? Method Status                Protocol
Vlan1                  192.168.0.1     YES manual up                    down
FastEthernet0/1        unassigned      YES unset  down                  down
FastEthernet0/2        unassigned      YES unset  down                  down
....output omited....
```

