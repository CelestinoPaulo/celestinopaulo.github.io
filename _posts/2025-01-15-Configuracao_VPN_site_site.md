---
title: 🚧 Conectando Sede e Filial com GRE Puro:. Laboratório Cisco no PNETLab
description: Neste post, o objetivo é demonstrar a configuração de uma VPN Site-to-Site utilizando apenas GRE (Generic Routing Encapsulation) entre roteadores Cisco, conectando duas redes locais — uma na sede (HQ) e outra em uma filial (Branch).
author: Celestino
date: 2025-01-15 17:57:00 +0800
categories: [Networking]
tags: [GRE, VPN, Overlay, WAN]
pin: true
image:
  path: /assets/img/post_images/vpn/gre.png
  lqip:
  alt: 
---

## 🎯Objectivos

A proposta é simular um cenário prático em ambiente de laboratório (como o PNETLab), focando no tunelamento de tráfego IP entre os sites, sem uso de criptografia (IPSec), ideal para fins de teste, simulação de topologias e validação de rotas entre redes separadas.

Você vai aprender:

- Como configurar interfaces de túnel GRE ponto a ponto.
- Como estabelecer conectividade entre redes LAN de sites distintos.
- Como roteadores intermediários (R2 e R3) simulam o caminho pela Internet.
- Como verificar e testar a conectividade pelo túnel.

Esse tipo de configuração é útil para entender o funcionamento básico de túneis GRE e pode servir como base para cenários mais avançados com IPSec, BGP ou OSPF sobre GRE.

## 📋Requisitos

Para acompanhar e reproduzir este laboratório de VPN Site-to-Site com GRE em roteadores Cisco, você vai precisar de:

- 🖥️ **VirtualBox** instalado (ambiente de virtualização).
- 🧪 **PNETLab** instalado e configurado (para simular os roteadores).
- 📦 **Imagens Cisco IOS** compatíveis com interfaces de túnel (por exemplo, IOSv).
- 🧠 **Conhecimentos básicos em:**, endereçamento IP (IPv4), roteamento estático e onterface de linha de comando (CLI).

### 🔧 O que é GRE?

**GRE (Generic Routing Encapsulation)** é um protocolo da Cisco que permite **criar túneis ponto a ponto entre roteadores**. Ele encapsula pacotes IP (ou outros protocolos) para que possam ser enviados através de uma rede intermediária (como a Internet) **como se fosse um link direto**.

### 📦 Características principais:

- Encapsula qualquer pacote IP.
- Permite interligar redes privadas via Internet.
- Suporta protocolos de roteamento dinâmico.
- Não possui criptografia — pode ser combinado com IPSec para segurança.

# 🛰️ Configuração Completa

## 🕸️ Topologia
![Desktop View](/assets/img/post_images/vpn/gre.png){: width="972" height="589" }

Temos dois locais: a **Sede (HQ)** com o roteador R1, e a **Filial (Branch)** com o roteador R4. Eles estão conectados por meio de uma rede intermediária simulada com os roteadores R2 e R3, representando a Internet.

Nosso objetivo é estabelecer um **túnel GRE entre R1 e R4**, encapsulando pacotes para que as redes locais (192.168.1.0/24 e 192.168.4.0/24) possam se comunicar como se estivessem **diretamente conectadas**.

## ⚙️ Endereçamento IP
Inicialmene, vamos configurar os endereços IPs em todos os dispositivos e garantir a conectividade ponto-a-ponto.

### 📌 R1
```bash
R1(config-if)# interface Ethernet0/0
R1(config-if)# ip address 10.0.0.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# interface Ethernet0/1
R1(config-if)# ip address 190.10.10.1 255.255.255.0
R1(config-if)# no shutdown
```

### 📌 R2
```bash
R2(config-if)# interface Ethernet0/0
R2(config-if)# ip address 190.10.10.2 255.255.255.0
R2(config-if)# no shutdown
R2(config-if)# interface Ethernet0/3
R2(config-if)# ip address 190.20.20.1 255.255.255.0
R2(config-if)# no shutdown
```

### 📌 R3
```bash
R3(config-if)# interface Ethernet0/3
R3(config-if)# ip address 190.20.20.1 255.255.255.0
R3(config-if)# no shutdown
R3(config-if)# interface Ethernet0/0
R3(config-if)# ip address 190.30.30.1 255.255.255.0
R3(config-if)# no shutdown
```

### 📌 R4
```bash
R4(config-if)# interface Ethernet0/0
R4(config-if)# ip address 190.30.30.1 255.255.255.0
R4(config-if)# no shutdown
R4(config-if)# interface Ethernet0/1
R4(config-if)# ip address 20.0.0.1 255.255.255.0
R4(config-if)# no shutdown
```

## ⚙️ Configuração do Roteamento (estatico)
A seguir, vamos configurar a roteamento estatico entre os routers (R1 ao R4) que compoem a nossa internet simulada, este roteamento visa garantir que os routers consigam alcansar uma a outra, no entanto, sem enxergarem as redes locais (HQ e Branch).

### 📌 R1
```bash
R1(config)# ip route 190.20.20.0 255.255.255.0 Ethernet0/1
R1(config)# ip route 190.30.30.0 255.255.255.0 Ethernet0/1
```

### 📌 R2
```bash
R2(config)# ip route 190.30.30.0 255.255.255.0 Ethernet0/3
```

### 📌 R3
```bash
R3(config)# ip route 190.10.10.0 255.255.255.0 Ethernet0/3
```

### 📌 R4
```bash
R4(config)# ip route 190.10.10.0 255.255.255.0 Ethernet0/1
R4(config)# ip route 190.20.20.0 255.255.255.0 Ethernet0/1
```

## 📌 Configuração do Tunnel
Agora, vamos configurar o Tunnel e o roteamento estatico entre os routers R1 e R4, essa configuracao fara com que todo trafego entre da Lan HQ para Branch seja tunelado, ou seja, que passe pelo Tunnel como se estivessem directamente conectados.

### 📌 R1
```bash
R1(config)# interface Tunnel0
R1(config-if)#  description Tunnel para R4 Out:190.30.30.2 In:20.0.0.1
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# tunnel source 190.10.10.1
R1(config-if)# tunnel destination 190.30.30.2
R1(config)#  tunnel mode gre ip
R1(config)# ip route 20.0.0.0 255.255.255.0 Tunnel0
```
### 📌 R4 - Filial (Branch)
```bash
R4(config)# interface Tunnel0
R4(config)# ip address 192.168.1.2 255.255.255.0
R4(config)# tunnel source 190.30.30.2
R4(config)# tunnel destination 190.10.10.1
R4(config)#  tunnel mode gre ip
R4(config)# ip route 10.0.0.0 255.255.255.0 Tunnel0
```

## ⚙️ Configuração dos Hosts, Cliente na Filial e Server na Sede.
![Desktop View](/assets/img/post_images/vpn/hosts.png){: width="972" height="589" }

##  ✅Testes e Validação

### ✅Estado do Tunnel GRE
Conforme destacado o tunel esta esstabelecido.
![Desktop View](/assets/img/post_images/vpn/check1.png){: width="972" height="589" }

### ✅Verificar a Quantidade de Hops (Saltos)
Vamos verificar a quantidade de saltos entre o cliente da Filial (Branch) para Server da Sede (HQ), conforme a imagem estamos a distância 2 saltos (menos o host).

![Desktop View](/assets/img/post_images/vpn/traceroute.png){: width="972" height="589" }

No entanto, do ponto de vista do R4 ao IP do tunnel do R1, estamos a distancia de um salto.
![Desktop View](/assets/img/post_images/vpn/traceroute2.png){: width="972" height="589" }

### ✅Teste de conectividade
![Desktop View](/assets/img/post_images/vpn/ping.png){: width="972" height="589" }

## 🥳🏁 Conclusão

A configuração do túnel GRE foi concluída com sucesso, permitindo comunicação entre as redes locais do HQ (R1) e da filial (R4) através da "internet simulada" (R2 e R3). A conectividade foi validada com sucesso. :) 