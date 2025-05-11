---
title: Configuração Básica de um Switch Cisco Catalyst 2960.
description: Neste post, apresentamos detalhadamente o passo a passo para realizar uma configuração básica no switch da série 2960.
author: Celestino
date: 2024-12-26 17:57:00 +0800
categories: [Networking]
tags: [Switch, Cisco]
pin: true
image:
  path: /assets/img/post_images/Topologia_small_size.png
  lqip: data:image/jpeg;base64,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
  alt: 
---
[![Hits](https://hits.sh/celestinopaulo.github.io.svg?style=for-the-badge&label=N%C3%BAmero%20de%20Visitas&extraCount=10)](https://hits.sh/celestinopaulo.github.io/)

## Objectivo

{: data-toc-skip='' .mt-4 .mb-0 }

Este post tem como objetivo fornecer uma orientação prática para a configuração básica de um switch Cisco Catalyst 2960. Ao final, será capaz de realizar as configurações iniciais essenciais, como definir nome do dispositivo, configurar senhas de acesso, habilitar a gestão remota via SSH e configurar interfaces básicas, garantindo assim que o switch esteja preparado para ser integrado a uma rede funcional de forma segura e eficiente.

## Público Alvo

- Profissionais iniciantes em redes de computadores;
- Técnicos de TI;
- Estudantes de cursos relacionados à área de redes e infraestrutura;
- Entusiastas.

## Requisitos
- Conhecimento básico de comandos da interface de linha de comando (CLI) do Cisco IOS.
- Switch Cisco Catalyst 2960: O equipamento a ser configurado;
- Computador com Windows 10: para acessar e configurar o switch via emulador de terminal;
- Cabo console USB/serias: para conectar o PC ao Switch;
- [Driver USB-to-Serial](https://drive.google.com/file/d/1Dyx60MOdtm--_zpP_rylNYN6wmIW81DY/view?usp=drive_link): para utilizar a conexão console;
- [Tera Term](https://github.com/TeraTermProject/teraterm/releases/tag/v5.3): Software de emulação de terminal para comunicação com o switch.

## Topologia

![Desktop View](/assets/img/post_images/Topologia_small_size.png){: width="972" height="589" }

## Configurações

#### Passo 1: Conectar-se ao Switch.
- Utilize um cabo console e um software de terminal (ex.: PuTTY ou Tera Term).
- Acesse o CLI do switch e entre no modo privilegiado e de seguida ao modo de configuração global. 
```text
Switch> enable
Switch# configure terminal
```

#### Passo 2: Configuração de Hostname.
Defina o nome para identificar o switch na rede:
```text
Switch(config)# hostname ItLab
ItLab(config)#
```
#### Passo 4: Configuração de um endereço IP.
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

### Passo 5: Configurar o Nome de Domínio
Usado para associar o switch a um domínio específico e é essencial para habilitar o SSH.
```text
ItLab(config)#ip domain-name celestinoitlab.com
```

### Passo 6: Configurar o Gateway
O gateway padrão permite que o switch envie tráfego para outras redes. Configure o endereço IP do roteador ou gateway da sua rede:
```text
ItLab(config)#ip default-gateway 192.168.10.1
```

### Passo 7: Configurar o DNS Server
O servidor DNS permite que o switch resolva nomes de domínio para endereços IP. Configure o endereço IP do servidor DNS disponível na sua rede:
```text
ItLab(config)#ip name-server 192.168.10.1
```

### Passo 8: Configurar Senhas de Acesso
- **Console**: Proteja o acesso físico ao switch
```text
ItLab(config)#enable secret 123
```

- **Privilegiado (Enable)**: Configure uma senha para o modo privilegiado
```text
ItLab(config)# line console 0
ItLab(config-line)# password 123
ItLab(config-line)# exit
```

### Passo 9:  Habilitar o acesso remoto via SSH
- Gere um par de chaves RSA:
```text
ItLab(config)#crypto key generate
```
- Configure as linhas de terminal virtual (VTY):
```text
ItLab(config)# line vty 0 15
ItLab(config-line)# transport input ssh
ItLab(config-line)# login local
ItLab(config-line)# exit
```
- Crie um usuário:
```text
ItLab(config)# username celestino privilege 15 secret 123
ItLab(config)# exit
```

### Passo 10:  Guardar as configurações
Para garantir que as configurações sejam mantidas após a reinicialização:
```text
ItLab# copy running-config startup-config
```

### Passo 11:  Verificar as configurações efectuadas
```text
ItLab# show running-config
Building configuration...

Current configuration : 1590 bytes
!
version 12.2
no service pad
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname ItLab
!
boot-start-marker
boot-end-marker
!
enable secret 5 $1$K1TY$5nmIZjtNvU5BE5BCpx9/l/
!
username celestino secret 5 $1$CkF6$7KSLm3XOEQgckv2lMrznP0
no aaa new-model
system mtu routing 1500
ip subnet-zero
!
ip domain-name celestinoitlab.com
ip name-server 192.168.0.1
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
vlan internal allocation policy ascending
!
!
interface FastEthernet0/1
!
interface FastEthernet0/2
!
Omitted
!
interface FastEthernet0/24
!
interface GigabitEthernet0/1
!
interface GigabitEthernet0/2
!
interface Vlan1
 ip address 192.168.0.2 255.255.255.0
 no ip route-cache
!
ip default-gateway 192.168.0.1
ip http server
ip http secure-server
!
control-plane
!
!
line con 0
 password 123
line vty 0
 login
line vty 1
 login local
 transport input all
line vty 2 4
 login
line vty 5 15
 login
!
end
```

### Passo 12:  Configurar um endereço IP no Host(Windows 10)
![Desktop View](/assets/img/post_images/windows_ip_config.png){: width="972" height="589" }
![Desktop View](/assets/img/post_images/ipconfig.png){: width="972" height="589" }

### Passo 13:   Testar Conexão
Verifique a conectividade,  use o comando _ping_ para testar o acesso ao gateway ou a outro dispositivo na rede.
- Ping do PC para o Switch:
![Desktop View](/assets/img/post_images/ping_pc_switch.png){: width="972" height="589" }
- Ping do Switch para o PC:
![Desktop View](/assets/img/post_images/ping_switch_pc.png){: width="972" height="589" }

### Passo 14:  Acesso Remoto via SSH
- Acesse o switch remotamente com Putty ou Tera Term via SSH ou Telnet (se configurado):

![Desktop View](/assets/img/post_images/putty.png){: width="972" height="589" }
![Desktop View](/assets/img/post_images/ssh_con1.png){: width="972" height="589" }
![Desktop View](/assets/img/post_images/ssh_con2.png){: width="972" height="589" }

_Com esses passos, o switch estará configurado e pronto para integrar a rede._ 🚀🚀😄😄
