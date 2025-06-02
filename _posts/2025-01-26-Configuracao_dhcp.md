---
title: 🚀 Como Instalar e Configurar um Servidor DHCP no RHEL 10
description: Mostrar como instalar e configurar um servidor DHCP no RHEL 10 usando o VMware, distribuindo endereços IP automaticamente para clientes Windows.
author: Celestino
date: 2025-01-26 17:57:00 +0800
categories: [Server]
tags: [Linux, RHEL, VMware, Server, Service, DHCP]
pin: true
image:
  path: /assets/img/post_images/dhcp/dhcp0.png
  lqip:
  alt: 
---

Se você está montando um laboratório virtual (como no PNETLab ou VirtualBox) ou quer entender como automatizar a distribuição de endereços IP na sua rede, configurar um servidor DHCP no **Rocky Linux** é uma excelente forma de começar.

Neste post rápido, vamos apresentar o **conceito do DHCP**, suas principais **características**, como **instalar, configurar, testar e validar** um servidor DHCP funcional.

---

## 📘 O que é o DHCP?

O **DHCP (Dynamic Host Configuration Protocol)** é um protocolo de rede que permite que dispositivos em uma rede obtenham automaticamente configurações de rede, como:

- Endereço IP
- Máscara de sub-rede
- Gateway padrão (roteador)
- Servidores DNS
- Tempo de concessão (lease)

Sem DHCP, seria necessário configurar manualmente essas informações em cada dispositivo da rede — o que é impraticável em redes grandes.

---

## ✅ Objetivo

Configurar um **servidor DHCP** no **Red Hat Enterprise Linux** para distribuir automaticamente endereços IP em uma rede local.
---

## 📦 Requisitos

Para este laboratório, o ambiente utilizado é o seguinte:

- 💻 **Hypervisor:** VMware Workstation ou VMware Player  
- 🖥️ **Servidor DHCP:** RHEL 10 (Red Hat Enterprise Linux)
- 🖥️ **Cliente DHCP:** Windows 10
- 🌐 **Tipo de rede:** NAT (Network Address Translation), usado por ambas m
- 🔌 **Conectividade:** Ambas as máquinas estão acessíveis entre si e com acesso à internet
- 🔐 **Permissões:** Acesso root ou usuário com privilégios de `sudo` na máquina RHEL
- 📁 **Pacote necessário:** `kea kea-doc kea-libs` (instalado via DNF)

> ⚠️ **Atenção:** O serviço DHCP embutido no modo NAT do VMware pode conflitar com o servidor DHCP que você irá configurar no RHEL.  
> Desactive **serviço DHCP do VMware NAT** ou criar uma rede NAT personalizada sem DHCP ativo.

## 📦 Topologia
De acordo ao tipo de rede (NAT) configurada nas duas máquinas, o servidor e o cliente Windows encontra-se conectados de acordo a topologia abaixo:

> **Nota**: O DHCP da conexão NAT foi desactivado.

![Desktop View](/assets/img/post_images/dhcp/dhcp2.png){: width="972" height="589" }


### 🔧  1. Instalar o Kea e seus componentes:

Execute o seguinte comando para instalar o pacote Kea:
```bash
sudo dnf install kea kea-doc kea-libs -y
```
![Desktop View](/assets/img/post_images/dhcp/dhcp3.png){: width="972" height="589" }


### 🔧 2. Configuração do Servidor DHCP (Kea)

Edite o arquivo principal de configuração:  **/etc/kea/kea-dhcp4.conf** 
Defina os enderecos a serem excluidos da atribuicao, incluindo o ip do servidor, defina o gateway, a mascara, o DNS, lease time e o mas lease time.
```bash
sudo nano /etc/kea/kea-dhcp4.conf
```

```bash
{
  "Dhcp4": {
    "interfaces-config": {
      "interfaces": [ "ens160" ]
    },
    "subnet4": [
      {
        "subnet": "192.168.224.0/24",
        "pools": [
          {
            "pool": "192.168.224.20 - 192.168.224.100"
          }
        ],
        "option-data": [
          {
            "name": "routers",
            "data": "192.168.224.2"
          },
          {
            "name": "domain-name-servers",
            "data": "8.8.8.8, 192.168.224.2"
          },
          {
            "name": "subnet-mask",
            "data": "255.255.255.0"
          }
        ]
      }
    ],
    "lease-database": {
      "type": "memfile",
      "persist": true,
      "name": "/var/lib/kea/dhcp4.leases"
    },
    "loggers": [
      {
        "name": "kea-dhcp4",
        "output_options": [
          {
            "output": "/var/log/kea-dhcp4.log"
          }
        ],
        "severity": "INFO"
      }
    ]
  }
}
```
![Desktop View](/assets/img/post_images/dhcp/dhcp4.png){: width="972" height="589" }

### 🔧 3. Iniciar e habilitar o serviço Kea
Agora defina qual interface será usada pelo servidor DHCP:
```bash
sudo systemctl enable --now kea-dhcp4.service
```
Verifique o status:
```bash
sudo systemctl status kea-dhcp4.service
```

## 🔧 4. Habilite e inicie o serviço DHCP:
```bash
sudo systemctl enable --now kea-dhcp4.service
```
Verifique o status:
```bash
sudo systemctl status kea-dhcp4.service
```
![Desktop View](/assets/img/post_images/dhcp/dhcp5.png){: width="972" height="589" }

## 🔧 5. Ajustando o Firewall do RHEL para o DHCP (Kea)

Habilite e inicie o serviço DHCP:
```bash
 Ajustando o Firewall do RHEL para o DHCP (Kea)
 
```
Após liberar, recarregue as regras:
```bash
sudo firewall-cmd --reload
```
![Desktop View](/assets/img/post_images/dhcp/dhcp6.png){: width="972" height="589" }

## 🔧 Teste de recepção de IP no Windows 10 e Conectividade

Configuração do cliente Windows para obter IP automaticamente (DHCP).
Limpamos inicialmente a configuração da placa e desabilitamos a mesma.
![Desktop View](/assets/img/post_images/dhcp/dhcp7.png){: width="972" height="589" }

A seguir, habilitamos a placa para obter o IP automaticamente (a placa está configurada para receber IP automaticamente)

O comando ipconfig, mostra o endereco IP recebido do servidor DHCP(192.168.224.10), o gateway(192.168.224.2 e os enderecos do DNS (192.168.224.2 e 8.8.8.8)
![Desktop View](/assets/img/post_images/dhcp/dhcp8.png){: width="972" height="589" }

Teste de conectividade com o servidor DHCP 192.168.224.10 e o gateway 192.168.224.2 realizado co sucesso.
![Desktop View](/assets/img/post_images/dhcp/dhcp8.png){: width="972" height="589" }

## ✅ Conclusão

