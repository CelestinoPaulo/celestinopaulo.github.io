---
title: Configuração de Virtual LAN (VLAN)
description: Sua rede está lenta, insegura ou difícil de gerenciar? A segmentação com VLANs pode ser a solução que falta. Neste post, você vai entender os desafios de redes sem segmentação, os benefícios do uso de VLANs e como aplicá-las na prática para criar uma rede mais eficiente, segura e organizada.
author: Celestino
date: 2025-05-01 17:57:00 +0800
categories: [Switching]
tags: [Switch, Cisco, Vlan, Trunk]
pin: true
image:
  path: /assets/img/post_images/vlan/vlan0.png
  lqip:
  alt: 
---

# VLANs na Prática: Conceitos, Desafios e Como Configurar sua Rede

## 1. O Problema: Um Único Domínio de Broadcast

### 🔄 Desafios de redes sem segmentação

Em redes onde todos os dispositivos estão no mesmo domínio de broadcast:

- Todo dispositivo recebe os broadcasts;
- Há aumento do tráfego desnecessário;
- Pode haver lentidão, congestionamentos e até tempestades de broadcast;
- A comunicação direta entre dispositivos representa um risco de segurança.

> Quanto maior a rede, maior o impacto de um único domínio de broadcast.

---

## 2. A Solução: Segmentação com VLANs

### 🧩 O que são VLANs?

As VLANs (Virtual Local Area Networks) permitem dividir logicamente a rede em múltiplos domínios de broadcast, mesmo que os dispositivos estejam fisicamente conectados ao mesmo switch.

### ✅ Benefícios do uso de VLANs

- 📉 **Redução do tráfego de broadcast**  
- 🔒 **Aumento da segurança por isolamento lógico**  
- 🛠️ **Organização por departamentos ou funções**  
- ⚙️ **Facilidade de gerenciamento e escalabilidade**

---

## 3. Boas Práticas no Uso de VLANs

### 📌 Planejamento e implementação inteligente

- Planeje a segmentação com base em funções, departamentos ou níveis de segurança;
- Evite VLANs muito grandes para não recriar o problema de broadcast excessivo;
- Configure VLANs nativas com segurança para evitar ataques como VLAN hopping;
- Implemente **ACLs** (Access Control Lists) entre VLANs para controlar o tráfego;
- Documente bem a estrutura das VLANs;
- Monitore o tráfego entre VLANs, principalmente quando há roteamento (inter-VLAN routing).

---

## 4. Como Configurar VLANs na Prática
![Desktop View](/assets/img/post_images/vlan/vlan0.png){: width="972" height="589" }

## ⚙️ Ambiente Utilizado e Requisitos

- **Emulador de Rede**: PnetLab.
- **Virtualização**: Virtual Box.
- **Virtualização**: Imagens de switchs instalados.
- **Conhecimento:** conhecimentos básicos sobre CLI do IOS.

---
## ⚙️ Configuração das Interfaces(SVI da Vlan1) dos Switchs S1 e S2

### Interface do S1
```bash
S1(config)#interface vlan 1
S1(config-if)#ip add 10.10.10.4 255.255.255.0
```
### Interface do S2
```bash
S2(config)#interface vlan 1
S2(config-if)#ip add 10.10.10.5 255.255.255.0
```

## ⚙️ Configuração dos Ips nos Clientes
![Desktop View](/assets/img/post_images/vlan/vlan1.png){: width="972" height="589"}

## ⚙️ Configuração das Vlans, os Acessos e a Intercace Trunk

### ⚙️ Criação das Vlans no S1
```bash
! Criação das VLANs (mesmas do S1)
vlan 10
 name RH
vlan 20
 name Sales
vlan 30
 name Marketing

! Adiconar as interfaces do tipo access nas Vlans 10 e 20
interface e0/2
 switchport mode access
 switchport access vlan 10
 description VPC3 (RH)

interface e0/3
 switchport mode access
 switchport access vlan 10
 description VPC6 (RH)

interface e1/0
 switchport mode access
 switchport access vlan 20
 description VPC9 (Sales)

interface e1/1
 switchport mode access
 switchport access vlan 20
 description VPC10 (Sales)

# Interfaces Trunk (ligação com S1)
interface e0/0
 switchport trunk encapsulation dot1q
 switchport trunk allowed vlan 1,10,20,30
 switchport mode trunk
 description Trunk para S1 (e0/0)

interface e0/1
 switchport trunk encapsulation dot1q
 switchport trunk allowed vlan 1,10,20,30
 switchport mode trunk
 description Trunk para S1 (e0/1)
```
### ⚙️ Criação das Vlans no S2
```bash
! Criação das VLANs
vlan 10
 name RH
vlan 20
 name Sales
vlan 30
 name Marketing

! Adiconar as interfaces do tipo access nas Vlans 10 e 20
interface e0/2
 switchport mode access
 switchport access vlan 20
 description (Sales)

interface e0/3
 switchport mode access
 switchport access vlan 20
 description (Sales)

interface e1/0
 switchport mode access
 switchport access vlan 30
 description (Marketing)

interface e1/1
 switchport mode access
 switchport access vlan 30
 description VPC10 (Marketing)

# Interfaces Trunk (ligação com S1)
interface e0/0
 switchport trunk encapsulation dot1q
 switchport trunk allowed vlan 1,10,20,30
 switchport mode trunk
 description Trunk para S1 (e0/0)

interface e0/1
 switchport trunk encapsulation dot1q
 switchport trunk allowed vlan 1,10,20,30
 switchport mode trunk
 description Trunk para S1 (e0/1)
```
## ⚙️ Verificar as Configurações no S1
### Verificar as interfaces configuradas dos Switchs
![Desktop View](/assets/img/post_images/vlan/vlan6.png){: width="972" height="589"}

### Verificar as Interfaces Trunk configuradas dos Switchs
![Desktop View](/assets/img/post_images/vlan/vlan7.png){: width="972" height="589"}

### Verificar as Vlans configuradas dos Switchs
![Desktop View](/assets/img/post_images/vlan/vlan6.png){: width="972" height="589"}

## ⚙️ Testes de Conectividade

Os Switchs S1 e S2, conseguem se comunicar atraves vlan de gestão (Vlan1, rede 10.10.10.0/24)
![Desktop View](/assets/img/post_images/vlan/vlan2.png){: width="972" height="589"}

O Computador VPC3 consegue se comunicar apena com VPC8 por pertencerem a mesma Vlan 10/RH, mas não consegue com VPC4 por pertencer a Vlan 20/Salees
![Desktop View](/assets/img/post_images/vlan/vlan3.png){: width="972" height="589"}

O Computador VPC4 da Vlan 20/Sales consegue se comunicar com VPC5, VPC6 e VPC7 por pertencerem a mesma Vlan 20/Sales, apesar de estarem em Switchs diferentes O Trunk configurado permite que os computadores se comuniquem.
![Desktop View](/assets/img/post_images/vlan/vlan4.png){: width="972" height="589"}

O Computador VPC9 da Vlan 20/Marketing consegue se comunicar com VPC10, VPC6 e VPC7 por pertencerem a mesma Vlan 30/Marketing
![Desktop View](/assets/img/post_images/vlan/vlan5.png){: width="972" height="589"}

## ⚙️ Conclusão
A configuração e documentação da topologia de rede foram concluídas com sucesso. A rede foi corretamente segmentada por VLANs, refletindo os diferentes departamentos, com as seguintes garantias:

- ✅ Segmentação lógica completa: Cada grupo de dispositivos está isolado em sua respectiva VLAN.
- ✅ Trunks ativos entre switches: Permitem a propagação de VLANs como a Sales (VLAN 20) entre S1 e S2.
- ✅ Sem roteamento entre VLANs: A topologia respeita o isolamento, pois não há roteador nem switch de camada 3.