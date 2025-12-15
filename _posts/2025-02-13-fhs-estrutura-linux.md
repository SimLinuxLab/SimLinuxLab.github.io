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

## Resumo Rápido (TL;DR)

O Linux não "joga" arquivos em qualquer lugar. O **FHS (Filesystem Hierarchy Standard)** é a "Constituição" que define onde cada arquivo deve morar.

> **Por que isso importa para DevSecOps?**
> * **Segurança:** Se você sabe onde o binário *deve* estar, você sabe quando um malware está no lugar errado.
> * **Automação:** Seus scripts Ansible/Bash vão quebrar se você não seguir esse padrão.
{: .prompt-tip }

## Explicação Técnica

*Baseado no standard FHS 3.0 e materiais RHCSA.*

O diretório raiz `/` (root) é o pai de tudo. Não existe `C:` ou `D:`. Tudo é um arquivo ou diretório pendurado na raiz.

## Os Diretórios Críticos

Visão geral dos diretórios fundamentais do Filesystem Hierarchy Standard (FHS) do Linux.

#### /boot
* (Inicialização)
* Contém os arquivos estáticos necessários para iniciar o sistema operacional, antes que o kernel comece a executar programas do usuário.
* Aspecto Crítico: Se este diretório for danificado ou as permissões estiverem incorretas, o sistema não conseguirá inicializar.
* Exemplos de Conteúdo: Arquivos de configuração do carregador de inicialização (como o GRUB) e a imagem do kernel (`vmlinuz`).

#### /dev
* (Arquivos de Dispositivos)
* Contém arquivos especiais que representam dispositivos de hardware (periféricos, discos rígidos, terminais, memória).
* Importância: É o mecanismo que o kernel do Linux usa para se comunicar com o hardware. Esses "arquivos" não contêm dados, mas sim interfaces.
* Exemplos de Conteúdo: `/dev/sda` (primeiro disco rígido), `/dev/tty1` (console), `/dev/zero` (fonte de caracteres nulos), `/dev/random` (fonte de aleatoriedade).

#### /etc
* (Configurações Centrais)
* O coração da configuração do sistema, contendo arquivos estáticos para todos os programas e serviços.
* Regra de Ouro: Não deve conter binários ou diretórios home. Apenas arquivos de texto editáveis (`.conf`, `.cfg`).
* Exemplos de Conteúdo Crítico: `/etc/passwd` (usuários), `/etc/shadow` (senhas criptografadas), `/etc/fstab` (montagem de *filesystems*), e arquivos de configuração de serviços como SSH e Apache.

#### /sbin
* (Binários do Sistema)
* Contém comandos de administração do sistema que geralmente exigem permissões de **root**.
* Exemplos de comandos: `iptables` (firewall), `fdisk` (particionamento), `reboot` (reinicialização), `ifconfig` (rede).

#### /bin
* (Binários Essenciais)
* Contém comandos essenciais que **qualquer usuário** pode rodar.
* Exemplos de comandos: `ls` (listar), `cp` (copiar), `cat` (concatenar e exibir), `mv` (mover), `rm` (remover).

#### /usr
* (Recursos do Sistema Unix)
* Contém a maior parte dos utilitários e aplicações de uso não essencial do sistema, incluindo bibliotecas, documentação e executáveis.
* É onde moram a maioria dos programas e bibliotecas instalados pelos gerenciadores de pacotes (como o `Program Files` do Windows ou `C:\Program Files`).
* Exemplos de Subdiretórios: `/usr/bin` (binários não essenciais), `/usr/lib` (bibliotecas), `/usr/share` (arquivos de dados estáticos).

#### /var
* (Arquivos Variáveis)
* Contém arquivos que mudam constantemente durante o funcionamento normal do sistema, como *spools*, arquivos temporários de programas e logs.
* Aspecto Crítico: Este diretório pode crescer muito rapidamente, exigindo monitoramento e gerenciamento de espaço em disco.
* Exemplos de Conteúdo: `/var/log` (arquivos de log de sistema e aplicações), `/var/www` (conteúdo de sites hospedados), `/var/spool` (filas de impressão e e-mail).

#### /tmp
* (Temporário)
* Projetado para arquivos temporários que não precisam ser preservados entre sessões ou reinicializações.
* Aspecto Crítico de Segurança: É um "Velho Oeste" onde qualquer usuário pode escrever e ler, tornando-o um alvo comum para ataques que buscam escalada de privilégios ou armazenamento de *malware*.
* Manutenção: O sistema limpa o conteúdo do `/tmp` a cada reinicialização ou periodicamente (dependendo da distribuição/configuração do `systemd-tmpfiles`).

#### /root
* (Diretório Home do Root)
* O diretório pessoal exclusivo do superusuário (`root`).
* Aspecto Crítico de Segurança: É um dos diretórios mais sensíveis do sistema. Deve ter permissões estritas para garantir que apenas o `root` possa acessar seus dados e arquivos de configuração.

#### /home
* (Diretórios Pessoais dos Usuários)
* Contém os diretórios pessoais (ou *home directories*) de todos os usuários comuns do sistema (ex: `/home/renato`, `/home/outro_usuario`).
* Importância: Armazena arquivos, documentos e todas as configurações específicas de cada usuário (os chamados "dotfiles", como `.bashrc` ou `.vimrc`).
* Aspecto de Segurança: As permissões aqui são cruciais, garantindo que um usuário não possa ler ou modificar os arquivos de outro.

## 💻 Exemplos Práticos (Níveis)

### 1. Junior (Exploração)

Listar os diretórios para ver a estrutura:

```bash
ls -F /
# Dica: As barras (/) indicam diretórios, @ indica links.
```

### 2. 🧑Pleno (Investigação)

Descobrir onde exatamente um comando está "morando":

```bash
type -a ls
type -a useradd
# 'useradd' geralmente fica em /sbin porque é administrativo.
```

### 3. Senior (Auditando com Script)

Um "one-liner" para encontrar arquivos grandes "perdidos" em `/var` que podem estar lotando o disco:

```bash
# Script de auditoria rápida
sudo find /var -type f -size +100M -exec ls -lh {} \; | awk '{ print $9 ": " $5 }'
```
{: file='audit_var.sh'}

## Olhar de Segurança (O Diferencial)

### Risco Crítico: `/tmp`
Como o `/tmp`{: .filepath} permite que qualquer um escreva (permissão 777), hackers adoram jogar scripts de ataque lá e executá-los.

### Hardening (Blindagem)
Em servidores de produção, montamos a partição `/tmp`{: .filepath} com a flag `noexec`. Isso impede que qualquer programa rode a partir de lá.

> **Cuidado com o PATH!**
> Se um atacante colocar um arquivo chamado `ls` na pasta `/tmp` e conseguir manipular seu PATH, você pode rodar o vírus dele achando que é o comando listar.
{: .prompt-danger }

## Troubleshooting (Erros Comuns)

**Erro:** `bash: ifconfig: command not found`

**Causa:** O comando está em `/sbin`{: .filepath} e seu usuário normal não tem `/sbin` no PATH.

**Solução:** Use o caminho completo ou vire root:

```bash
/sbin/ifconfig
# ou
sudo ifconfig
```

## Nota SimLinux (Dica de Ouro)

Quer saber a função oficial de qualquer pasta sem abrir o PDF? Digite no terminal:

```bash
man hier
```