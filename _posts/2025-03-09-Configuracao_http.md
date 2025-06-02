---
title: 🌐 Como Configurar um Servidor HTTP (Apache) no RHEL
description: Este guia tem como objetivo mostrar como instalar e configurar um servidor web Apache (httpd) em uma máquina com Red Hat Enterprise Linux (RHEL), criando um ambiente funcional para hospedar páginas web locais ou em rede.
author: Celestino
date: 2025-03-09 17:57:00 +0800
categories: [Server]
tags: [Linux, RHEL, VMware, Server, Service, HTTP]
pin: true
image:
  path: /assets/img/post_images/http/http0.png
  lqip:
  alt: 
---

# 🌐 Como Configurar um Servidor HTTP (Apache) no RHEL

---

## 🎯 Objetivo

Este guia tem como objetivo mostrar como instalar e configurar um **servidor web Apache (httpd)** em uma máquina com **Red Hat Enterprise Linux (RHEL)**, criando um ambiente funcional para hospedar páginas web locais ou em rede.

---

## 📋 Requisitos

### 🖥️ Ambiente utilizado:

- 💻 **Máquina virtual com RHEL 8+**, criada no **VMware Workstation ou Player**
- 🪟 **Cliente Windows 10** na mesma rede da VM (usado para testar via navegador)
- 🌐 Acesso à internet para instalar pacotes e atualizar o sistema
- 🔐 Firewall ativo com `firewalld` (gerenciador padrão no RHEL)

### 🧠 Conhecimentos necessários:

- Comandos básicos de terminal Linux
- Uso de `sudo` para executar tarefas administrativas
- Conhecimento básico de redes (IP, porta, teste de conectividade)
- Familiaridade com acesso a máquinas virtuais e troca de arquivos entre host e VM

---

## ⚙️ Passo a Passo: Instalando e Configurando o Apache


### 📦 Topologia
De acordo ao tipo de rede (NAT) configurada nas duas máquinas, o servidor e o cliente Windows encontra-se conectados de acordo a topologia abaixo:

![Desktop View](/assets/img/post_images/http/http1.png){: width="972" height="589" }

### 1️⃣ Instale o Apache (httpd)

```bash
sudo dnf install httpd -y
```
![Desktop View](/assets/img/post_images/http/http2.png){: width="972" height="589" }

### 2️⃣ Ative e inicie o serviço Apache

```bash
sudo systemctl enable httpd
sudo systemctl start httpd
```
Verifique o status:
```bash
sudo systemctl status httpd
```
![Desktop View](/assets/img/post_images/http/http3.png){: width="972" height="589" }

### 3️⃣ Libere a porta 80 (HTTP) no firewall

```bash
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload
```
![Desktop View](/assets/img/post_images/http/http4.png){: width="972" height="589" }

### 4️⃣ Crie uma página HTML de teste

```bash
echo "<h1>Servidor CELESTINO IT LAB Apache no RHEL funcionando!</h1>" | sudo tee /var/www/html/index.html
```
![Desktop View](/assets/img/post_images/http/http5.png){: width="972" height="589" }

### 5️⃣ Acedendo ao Servidor Web pela Windows 10
Acesso ao Servidor Web confiigurado com sucesso
![Desktop View](/assets/img/post_images/http/http6.png){: width="972" height="589" }
![Desktop View](/assets/img/post_images/http/http7.png){: width="972" height="589" }

## ✅ Conclusão
A configuração do servidor Apache no RHEL foi concluída com sucesso. Foi possível acessar a página web de teste a partir do cliente Windows 10, confirmando que o servidor está funcionando e acessível na rede.
