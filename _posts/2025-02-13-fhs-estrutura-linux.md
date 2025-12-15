---
title: "FHS: A Estrutura de Diretórios e a Segurança do Linux"
description: Entenda a anatomia do Linux, onde guardar seus arquivos e como isso impacta a segurança e automação.
author: SimLinux
date: 2025-02-13 12:00:00 -0300
categories: [Linux, Arquitetura]
tags: [fhs, lpi, rhcsa, hardening]
pin: true
math: false
mermaid: false
---

## 🚀 Resumo Rápido (TL;DR)

O Linux não "joga" arquivos em qualquer lugar. O **FHS (Filesystem Hierarchy Standard)** é a "Constituição" que define onde cada arquivo deve morar.

> **Por que isso importa para DevSecOps?**
> * **Segurança:** Se você sabe onde o binário *deve* estar, você sabe quando um malware está no lugar errado.
> * **Automação:** Seus scripts Ansible/Bash vão quebrar se você não seguir esse padrão.
{: .prompt-tip }

## 🧠 Explicação Técnica

*Baseado no standard FHS 3.0 e materiais RHCSA.*

O diretório raiz `/` (root) é o pai de tudo. Não existe `C:` ou `D:`. Tudo é um arquivo ou diretório pendurado na raiz.

### Os Diretórios Críticos

`/bin`
: Comandos essenciais que *qualquer usuário* pode rodar (ex: `ls`, `cp`, `cat`).

`/sbin`
: Binários de **S**istema. Comandos que alteram o sistema e geralmente exigem root (ex: `iptables`, `fdisk`, `reboot`).

`/etc`
: (Etcetera/Editable Text Configurations). O coração da configuração. Aqui vivem os arquivos `.conf`. **Regra de Ouro:** Nunca coloque binários aqui.

`/var`
: (Variable). Arquivos que crescem dinamicamente: Logs (`/var/log`), Sites (`/var/www`), Banco de Dados (`/var/lib/mysql`).

`/tmp`
: (Temporary). O "Velho Oeste". Qualquer um pode escrever aqui. É limpo a cada reboot (geralmente).

`/usr`
: (Unix System Resources). Onde moram os programas instalados pelos gerenciadores de pacotes (como o `Program Files` do Windows).

## 💡 Analogia: O Prédio Corporativo

> * **`/` (Root):** A portaria principal.
> * **`/home`:** As mesas dos funcionários.
> * **`/etc`:** A sala de elétrica/fiação. Se mexer errado aqui, o prédio apaga.
> * **`/tmp`:** O quadro de avisos público. Todo mundo joga coisa lá.
> * **`/var/log`:** As câmeras de segurança gravando tudo.
{: .prompt-info }

## 💻 Exemplos Práticos (Níveis)

### 1. 👶 Junior (Exploração)

Listar os diretórios para ver a estrutura:

```bash
ls -F /
# Dica: As barras (/) indicam diretórios, @ indica links.
```

### 2. 🧑‍💻 Pleno (Investigação)

Descobrir onde exatamente um comando está "morando":

```bash
type -a ls
type -a useradd
# 'useradd' geralmente fica em /sbin porque é administrativo.
```

### 3. 🧓 Senior (Auditando com Script)

Um "one-liner" para encontrar arquivos grandes "perdidos" em `/var` que podem estar lotando o disco:

```bash
# Script de auditoria rápida
sudo find /var -type f -size +100M -exec ls -lh {} \; | awk '{ print $9 ": " $5 }'
```
{: file='audit_var.sh'}

## 🛡️ Olhar de Segurança (O Diferencial)

### Risco Crítico: `/tmp`
Como o `/tmp`{: .filepath} permite que qualquer um escreva (permissão 777), hackers adoram jogar scripts de ataque lá e executá-los.

### Hardening (Blindagem)
Em servidores de produção, montamos a partição `/tmp`{: .filepath} com a flag `noexec`. Isso impede que qualquer programa rode a partir de lá.

> **Cuidado com o PATH!**
> Se um atacante colocar um arquivo chamado `ls` na pasta `/tmp` e conseguir manipular seu PATH, você pode rodar o vírus dele achando que é o comando listar.
{: .prompt-danger }

## ⚠️ Troubleshooting (Erros Comuns)

**Erro:** `bash: ifconfig: command not found`

**Causa:** O comando está em `/sbin`{: .filepath} e seu usuário normal não tem `/sbin` no PATH.

**Solução:** Use o caminho completo ou vire root:

```bash
/sbin/ifconfig
# ou
sudo ifconfig
```

## 📝 Nota SimLinux (Dica de Ouro)

Quer saber a função oficial de qualquer pasta sem abrir o PDF? Digite no terminal:

```bash
man hier
```