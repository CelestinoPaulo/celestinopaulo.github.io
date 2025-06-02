---
title: 🖧 Compartilhamento de Arquivos com Samba no RHEL 10
description: Este post tem como objetivo demonstrar a configuração do Samba em um ambiente de laboratório com RHEL 10, uma máquina virtual no VMware, e acesso a partir de um cliente Windows 10. A ideia é criar um ambiente de compartilhamento de arquivos entre sistemas Linux e Windows de forma prática e funcional.
author: Celestino
date: 2025-03-09 17:57:00 +0800
categories: [Server]
tags: [Linux, RHEL, VMware, Server, Service, Samba, SMB]
pin: true
image:
  path: /assets/img/post_images/samba/samba0.png
  lqip:
  alt: 
---

# 🖧 Compartilhamento de Arquivos com Samba no RHEL 10

## 🎯 Objetivo

Este post tem como objetivo demonstrar a configuração do **Samba** em um ambiente de laboratório com **RHEL 10**, uma **máquina virtual no VMware**, e acesso a partir de um cliente **Windows 10**. A ideia é criar um ambiente de compartilhamento de arquivos entre sistemas Linux e Windows de forma prática e funcional.

---

## 🧠 O que é o Samba e por que usá-lo?

O **Samba** é uma implementação livre do protocolo SMB/CIFS que permite a interoperabilidade entre sistemas Linux/Unix e Windows. Ele permite que máquinas Linux atuem como servidores de arquivos ou impressoras em redes com clientes Windows, além de possibilitar autenticação em domínios.

### ✅ Importância resumida:
- Compartilhamento de arquivos e pastas entre Linux e Windows.
- Integração em ambientes heterogêneos.
- Suporte a autenticação e controle de permissões.

---

## 🧰 Requisitos

### Sistema:
- RHEL 10 com subscrição ativa (registro via Red Hat Subscription Manager).
- Cliente Windows 10 (na mesma rede da VM Linux).
- Virtualização usando VMware Workstation/Player ou VMware ESXi.

### Conhecimentos:
- Familiaridade com o terminal (CLI) no Linux.
- Noções básicas de rede e permissões de arquivos.
- Acesso root ou `sudo` habilitado.

---

## 🛠️ Etapas de Configuração

### Atualize o sistema
De acordo ao tipo de rede (NAT) configurada nas duas máquinas, o servidor e o cliente Windows encontra-se conectados de acordo a topologia abaixo:

![Desktop View](/assets/img/post_images/samba/samba1.png){: width="972" height="589" }

### 1. Instale o Samba
```bash
sudo dnf install samba samba-client samba-common -y
```
![Desktop View](/assets/img/post_images/samba/samba2.png){: width="972" height="589" }

### 2. Crie uma pasta para compartilhar
```bash
sudo mkdir -p /srv/compartilhado
sudo chown nobody:nobody /srv/compartilhado
sudo chmod 0777 /srv/compartilhado
```
![Desktop View](/assets/img/post_images/samba/samba3.png){: width="972" height="589" }

### 3. Configure o arquivo `/etc/samba/smb.conf`
Adicione ao final:

```ini
[Compartilhado]
   path = /srv/compartilhado
   browsable = yes
   writable = yes
   guest ok = yes
   read only = no
```
![Desktop View](/assets/img/post_images/samba/samba4.png){: width="972" height="589" }

### 4. Habilite e inicie o serviço
```bash
sudo systemctl enable smb --now
sudo systemctl enable nmb --now
```
![Desktop View](/assets/img/post_images/samba/samba5.png){: width="972" height="589" }

### 6. Configure o firewall
```bash
sudo firewall-cmd --permanent --add-service=samba
sudo firewall-cmd --reload
```
![Desktop View](/assets/img/post_images/samba/samba6.png){: width="972" height="589" }

### 7. Teste o compartilhamento

Do Windows 10, acesse `\IP-do-RHEL\Compartilhado` via Explorador de Arquivos.
![Desktop View](/assets/img/post_images/samba/samba7.png){: width="972" height="589" }

Pasta do Samba acessada com sucesso
![Desktop View](/assets/img/post_images/samba/samba8.png){: width="972" height="589" }

Arquivos foram alojados com sucesso no Servidor Samba
![Desktop View](/assets/img/post_images/samba/samba9.png){: width="972" height="589" }

Arquivos no Servidor Samba
![Desktop View](/assets/img/post_images/samba/samba10.png){: width="972" height="589" }

> **Nota:** não é recomendável habilitar o acesso anónimo, em ambiente de produção é necessário configurar contas para reforçar a segurança dos dados.

Acesso a pasta partilha a a partir do client Samba do Linux
![Desktop View](/assets/img/post_images/samba/samba11.png){: width="972" height="589" }

---

## ✅ Resumo

Neste post, configuramos um servidor Samba no **RHEL 10**, com uma pasta compartilhada acessível a partir do **Windows 10**, usando uma **máquina virtual no VMware**. Essa configuração é útil para ambientes de teste, pequenas redes mistas ou integração em escritórios com Linux e Windows.

O Samba continua sendo uma solução poderosa, simples e eficiente para compartilhamento de arquivos entre sistemas diferentes.

---

Se você gostou deste conteúdo e quer mais tutoriais como esse, fique à vontade para comentar ou sugerir temas!
