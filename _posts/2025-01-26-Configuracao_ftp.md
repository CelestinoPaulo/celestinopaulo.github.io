---
title: 🐧 Como configurar um servidor FTP no RHEL 10 com vsftpd – Guia Completo
description: Este post tem como objetivo ensinar, passo a passo, como configurar um servidor FTP no Red Hat Enterprise Linux 10 (RHEL 10) utilizando o vsftpd, garantindo funcionalidade e segurança na transferência de arquivos entre sistemas.
author: Celestino
date: 2025-02-01 17:57:00 +0800
categories: [Server]
tags: [Linux, RHEL, VMware, Server, Service, FTP]
pin: true
image:
  path: /assets/img/post_images/ftp/ftp0.png
  lqip:
  alt: 
---

## 📌 O que é FTP?
O FTP (File Transfer Protocol) é um protocolo de rede usado para transferência de arquivos entre cliente e servidor por meio de uma conexão TCP/IP. É uma solução comum em servidores de hospedagem, ambientes de desenvolvimento e redes corporativas.

Embora existam opções mais modernas (como SFTP e SCP), o FTP ainda é amplamente utilizado quando se busca simplicidade e compatibilidade com sistemas legados.

## 🤔 Por que configurar um servidor FTP?
✅ Para permitir que usuários façam upload e download de arquivos remotamente
✅ Para integrar um sistema de transferência de arquivos interno em redes empresariais
✅ Para disponibilizar repositórios ou backups centralizados
✅ Para ambientes de desenvolvimento e teste

## Pré-requisitos
Antes de iniciar, verifique se você possui:
- ✅ Um sistema com RHEL 10 instalado e acesso de superusuário (root ou sudo)
- ✅ Windows 10 como client com WinSCP instalado para testes.
- ✅ VMware Workstation para virtualização do servidor Rhel e do cliente Windows 10
- ✅ Acesso à internet (para instalação de pacotes)
- ✅ Porta 21 liberada no firewall
---

## 🛠️ Passo a passo: Instalando e configurando FTP no RHEL 10

### 📦 Topologia
De acordo ao tipo de rede (NAT) configurada nas duas máquinas, o servidor e o cliente Windows encontra-se conectados de acordo a topologia abaixo:

![Desktop View](/assets/img/post_images/ftp/ftp2.png){: width="972" height="589" }

### 1️⃣ Instale o vsftpd (Very Secure FTP Daemon)
```bash
sudo dnf install vsftpd -y
```
![Desktop View](/assets/img/post_images/ftp/ftp3.png){: width="972" height="589" }

### 2️⃣ Inicie e habilite o serviço
```bash
sudo systemctl enable --now vsftpd
sudo systemctl status vsftpd
```
![Desktop View](/assets/img/post_images/ftp/ftp4.png){: width="972" height="589" }

### 3️⃣ Libere o FTP no firewall
```bash
sudo firewall-cmd --permanent --add-service=ftp
sudo firewall-cmd --reload
```
![Desktop View](/assets/img/post_images/ftp/ftp5.png){: width="972" height="589" }

### 4️⃣ Edite o arquivo de configuração
```bash
sudo vi /etc/vsftpd/vsftpd.conf
```
Altere ou adicione as seguintes linhas:
```bash
anonymous_enable=NO
local_enable=YES
write_enable=YES
chroot_local_user=YES
allow_writeable_chroot=YES
pasv_enable=YES
pasv_min_port=30000
pasv_max_port=31000
```

### 5️⃣ Criar Usuários e Directorios FTP
```bash
sudo useradd ftpuser
sudo passwd ftpuser
sudo mkdir -p /home/ftpuser/ftp/upload
sudo chown -R ftpuser:ftpuser /home/ftpuser/ftp
sudo chmod 550 /home/ftpuser/ftp
sudo chmod 750 /home/ftpuser/ftp/upload
```
![Desktop View](/assets/img/post_images/ftp/ftp6.png){: width="972" height="589" }

## 6️⃣ Teste de Acesso ao Ficheiros do Servidor

Acedendo com conta criada a partir do Windows 10. utilizando o Win SCP
![Desktop View](/assets/img/post_images/ftp/ftp7.png){: width="972" height="589" }

Servidor acedido com sucesso
![Desktop View](/assets/img/post_images/ftp/ftp8.png){: width="972" height="589" }

Arquivos carregados no Servidor FTP a partir do cliente
![Desktop View](/assets/img/post_images/ftp/ftp9.png){: width="972" height="589" }

Ficheiros carregados no Servidor
![Desktop View](/assets/img/post_images/ftp/ftp10.png){: width="972" height="589" }


## ✅ Conclusão

A configuração do servidor FTP no RHEL 10 foi realizada com sucesso. Após instalar e configurar o vsftpd, criar o usuário e ajustar permissões, o acesso foi testado com êxito utilizando um cliente FTP.

Os arquivos enviados foram armazenados corretamente no servidor, no diretório configurado para o usuário (/home/ftpuser/ftp/upload), confirmando que o ambiente está funcional e pronto para uso em redes internas.
