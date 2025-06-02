---
title: OSPF Multi Área na Prática - Configuração Profissional em Ambiente Cisco
description: Neste post, você vai entender os desafios de redes OSPF sem segmentação, os benefícios da abordagem multi-área e como aplicá-la na prática em equipamentos Cisco para construir uma rede mais eficiente, escalável e organizada.
author: Celestino
date: 2025-05-15 17:57:00 +0800
categories: [Routing]
tags: [Routing, OSPF, IGP, Cisco]
pin: true
image:
  path: /assets/img/post_images/ospf/topo2.png
  lqip:
  alt: 
---

## 🌐 Por que usar OSPF?

À medida que redes crescem em tamanho e complexidade, torna-se inviável configurar rotas manualmente entre todos os roteadores. Além disso, protocolos mais simples como o RIP rapidamente se tornam ineficientes, pois possuem limitações como:

- Número máximo de saltos (hop count);
- Convergência lenta;
- Falta de suporte para grandes redes hierárquicas.

É aí que entra o **OSPF (Open Shortest Path First)** — um protocolo de roteamento dinâmico de **estado de enlace**, que calcula o melhor caminho com base em métricas como custo e largura de banda, e que se adapta rapidamente a mudanças na rede.

## 🧩 O desafio da escalabilidade

Embora o OSPF seja altamente eficiente, em redes grandes ele pode sofrer com:

- Alto consumo de CPU para processar LSAs;
- Tabelas de roteamento extensas;
- Convergência mais lenta em redes planas.

Para resolver isso, o OSPF suporta um design **multiárea**, que permite **dividir a rede em domínios menores de roteamento**, aumentando a eficiência e a escalabilidade.

---
# 🛰️ OSPF Multiárea - Configuração Completa

## 🎯 Topologia
![Desktop View](/assets/img/post_images/ospf/topo.png){: width="972" height="589" }

## 🎯 Objetivo

Configurar e validar o roteamento OSPF Multiárea com topologia baseada em cinco áreas: Área 0 (Backbone), Área 1, Área 2, Área 3 e Área 4. A topologia interliga 10 roteadores, com ABRs entre áreas e anúncios de rota default. O objetivo é garantir comunicação eficiente, segmentação adequada por área e redistribuição controlada de rotas.

## ⚙️ Configuração dos Roteadores

### 📌 R1 (Área 3 - Roteador de Acesso)

```bash
Router> Enable
Router# Configure terminal
Router(Config)# hostname R1
Router(Config)# interface Loopback0
R1(config-if)# ip address 1.1.1.1 255.255.255.255
R1(config-if)# interface Ethernet0/0
R1(config-if)# ip address 192.168.80.2 255.255.255.0
R1(config-if)# ip ospf network point-to-point
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)#router ospf 1
R1(config-router)# router-id 1.1.1.1
R1config-router)# network 1.1.1.1 0.0.0.0 area 3
R1(config-router)# network 192.168.80.0 0.0.0.255 area 3
```

### 📌 R2 (Área 1)

```bash
Router> enable
Router# configure terminal
Router(config)# hostname R2
R2(config)# interface Loopback0
R2(config-if)# ip address 2.2.2.2 255.255.255.255
R2(config-if)# exit
R2(config)# interface Ethernet0/0
R2(config-if)# ip address 192.168.20.1 255.255.255.0
R2(config-if)# ip ospf network point-to-point
R2(config-if)# no shutdown
R2(config-if)# exit
R2(config)# router ospf 1
R2(config-router)# router-id 2.2.2.2
R2(config-router)# network 2.2.2.2 0.0.0.0 area 1
R2(config-router)# network 192.168.20.0 0.0.0.255 area 1
```

### 📌 R3 (Área 4)

```bash
Router> enable
Router# configure terminal
Router(config)# hostname R3
R3(config)# interface Loopback0
R3(config-if)# ip address 3.3.3.3 255.255.255.255
R3(config-if)# exit
R3(config)# interface Ethernet0/0
R3(config-if)# ip address 192.168.90.2 255.255.255.0
R3(config-if)# ip ospf network point-to-point
R3(config-if)# no shutdown
R3(config-if)# exit
R3(config)# router ospf 1
R3(config-router)# router-id 3.3.3.3
R3(config-router)# network 3.3.3.3 0.0.0.0 area 4
R3(config-router)# network 192.168.90.0 0.0.0.255 area 4
```

### 📌 R4 (Área 0/2)

```bash
Router> enable
Router# configure terminal
Router(config)# hostname R4
R4(config)# interface Loopback0
R4(config-if)# ip address 4.4.4.4 255.255.255.255
R4(config-if)# exit
R4(config)# interface Ethernet0/0
R4(config-if)# ip address 192.168.40.2 255.255.255.0
R4(config-if)# ip ospf network point-to-point
R4(config-if)# no shutdown
R4(config-if)# exit
R4(config)# interface Ethernet0/2
R4(config-if)# ip address 192.168.70.1 255.255.255.0
R4(config-if)# ip ospf network point-to-point
R4(config-if)# no shutdown
R4(config-if)# exit
R4(config)# router ospf 1
R4(config-router)# router-id 4.4.4.4
R4(config-router)# network 4.4.4.4 0.0.0.0 area 0
R4(config-router)# network 192.168.40.0 0.0.0.255 area 0
R4(config-router)# network 192.168.70.0 0.0.0.255 area 2
```

### 📌 R5 (Área 0/3/4)

```bash
Router> enable
Router# configure terminal
Router(config)# hostname R5
R5(config)# interface Loopback0
R5(config-if)# ip address 5.5.5.5 255.255.255.255
R5(config-if)# exit
R5(config)# interface Ethernet0/0
R5(config-if)# ip address 192.168.60.1 255.255.255.0
R5(config-if)# ip ospf network point-to-point
R5(config-if)# no shutdown
R5(config-if)# exit
R5(config)# interface Ethernet0/2
R5(config-if)# ip address 192.168.80.1 255.255.255.0
R5(config-if)# ip ospf network point-to-point
R5(config-if)# no shutdown
R5(config-if)# exit
R5(config)# interface Ethernet0/3
R5(config-if)# ip address 192.168.90.1 255.255.255.0
R5(config-if)# ip ospf network point-to-point
R5(config-if)# no shutdown
R5(config-if)# exit
R5(config)# router ospf 1
R5(config-router)# router-id 5.5.5.5
R5(config-router)# network 5.5.5.5 0.0.0.0 area 0
R5(config-router)# network 192.168.60.0 0.0.0.255 area 0
R5(config-router)# network 192.168.80.0 0.0.0.255 area 3
R5(config-router)# network 192.168.90.0 0.0.0.255 area 4
```

### 📌 R6 (Área 4)

```bash
Router> enable
Router# configure terminal
Router(config)# hostname R6
R6(config)# interface Loopback0
R6(config-if)# ip address 6.6.6.6 255.255.255.255
R6(config-if)# exit
R6(config)# interface Ethernet0/0
R6(config-if)# ip address 192.168.90.2 255.255.255.0
R6(config-if)# ip ospf network point-to-point
R6(config-if)# no shutdown
R6(config-if)# exit
R6(config)# router ospf 1
R6(config-router)# router-id 6.6.6.6
R6(config-router)# network 6.6.6.6 0.0.0.0 area 4
R6(config-router)# network 192.168.90.0 0.0.0.255 area 4
```

### 📌 R7 (Área 2)

```bash
Router> enable
Router# configure terminal
Router(config)# hostname R7
R7(config)# interface Loopback0
R7(config-if)# ip address 7.7.7.7 255.255.255.255
R7(config-if)# exit
R7(config)# interface Ethernet0/0
R7(config-if)# ip address 192.168.70.2 255.255.255.0
R7(config-if)# ip ospf network point-to-point
R7(config-if)# no shutdown
R7(config-if)# exit
R7(config)# router ospf 1
R7(config-router)# router-id 7.7.7.7
R7(config-router)# network 7.7.7.7 0.0.0.0 area 2
R7(config-router)# network 192.168.70.0 0.0.0.255 area 2
```

### 📌 R8 (Área 0)

```bash
Router> enable
Router# configure terminal
Router(config)# hostname R8
R8(config)# interface Loopback0
R8(config-if)# ip address 8.8.8.8 255.255.255.255
R8(config-if)# exit
R8(config)# interface Ethernet0/0
R8(config-if)# ip address 192.168.30.2 255.255.255.0
R8(config-if)# ip ospf network point-to-point
R8(config-if)# no shutdown
R8(config-if)# exit
R8(config)# interface Ethernet0/1
R8(config-if)# ip address 192.168.50.1 255.255.255.0
R8(config-if)# ip ospf network point-to-point
R8(config-if)# no shutdown
R8(config-if)# exit
R8(config)# interface Ethernet0/3
R8(config-if)# ip address 192.168.1.2 255.255.255.0
R8(config-if)# no shutdown
R8(config-if)# exit
R8(config)# ip route 0.0.0.0 0.0.0.0 192.168.1.1
R8(config)# router ospf 1
R8(config-router)# router-id 8.8.8.8
R8(config-router)# network 8.8.8.8 0.0.0.0 area 0
R8(config-router)# network 192.168.30.0 0.0.0.255 area 0
R8(config-router)# network 192.168.50.0 0.0.0.255 area 0
R8(config-router)# default-information originate
```

### 📌 R9 (Área 1)

```bash
Router> enable
Router# configure terminal
Router(config)# hostname R9
R9(config)# interface Loopback0
R9(config-if)# ip address 9.9.9.9 255.255.255.255
R9(config-if)# exit
R9(config)# interface Ethernet0/0
R9(config-if)# ip address 192.168.10.1 255.255.255.0
R9(config-if)# ip ospf network point-to-point
R9(config-if)# no shutdown
R9(config-if)# exit
R9(config)# router ospf 1
R9(config-router)# router-id 9.9.9.9
R9(config-router)# network 9.9.9.9 0.0.0.0 area 1
R9(config-router)# network 192.168.10.0 0.0.0.255 area 1
```

### 📌 R10 (Área 1)

```bash
Router> enable
Router# configure terminal
Router(config)# hostname R10
R10(config)# interface Loopback0
R10(config-if)# ip address 10.10.10.10 255.255.255.255
R10(config-if)# exit
R10(config)# interface Ethernet0/0
R10(config-if)# ip address 192.168.10.2 255.255.255.0
R10(config-if)# ip ospf network point-to-point
R10(config-if)# no shutdown
R10(config-if)# exit
R10(config)# router ospf 1
R10(config-router)# router-id 10.10.10.10
R10(config-router)# network 10.10.10.10 0.0.0.0 area 1
R10(config-router)# network 192.168.10.0 0.0.0.255 area 1
```

## ✅ Testes e Validação

### Neighbors do Router R3

- `show ip ospf neighbor`: Verificar adjacências OSPF entre roteadores conectados diretamente.
![Desktop View](/assets/img/post_images/ospf/neibors.png){: width="972" height="589" }

### Verificar a tabela de roteamento do Router R3
- `show ip route`: Validar o recebimento das rotas de outras áreas.
![Desktop View](/assets/img/post_images/ospf/tabela_roteamento.png){: width="972" height="589" }

### Teste de Conectividade

- `ping` e `traceroute` entre loopbacks: Confirmar conectividade fim-a-fim.
![Desktop View](/assets/img/post_images/ospf/tracert.png){: width="972" height="589" }

![Desktop View](/assets/img/post_images/ospf/ping.png){: width="972" height="589" }

## ✅ Conclusão

A configuração OSPF Multiárea foi concluída com sucesso, permitindo roteamento segmentado, eficiente e com redistribuição controlada. A estrutura modular melhora a escalabilidade e a manutenção da rede. O uso do tipo de rede ponto-a-ponto evita eleição de DR/BDR e simplifica a operação.