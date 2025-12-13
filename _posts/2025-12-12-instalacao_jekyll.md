---
layout: post
title: "Instalação do Jekyll + Chirpy pelo GitHub (100% Web)"
date: 2025-12-12 12:00:00 +0000
categories: [inicial]
tags: [linux, git, devops]
---

## 🧩 INSTALAÇÃO DO JEKYLL + CHIRPY 100% PELO GITHUB (SEM TERMINAL)

PASSO A PASSO PARA CRIAR UM SITE USANDO O JEKYLL E O TEMA CHIRPY, USANDO APENAS O GITHUB.COM — SEM INSTALAR RUBY, NODE OU RODAR LOCALMENTE.

## ✅ ETAPA 1 — CRIAR O REPOSITÓRIO USANDO O STARTER

Acesse o template oficial:  
https://github.com/cotes2020/chirpy-starter

1. Clique em **Use this template**  
2. Selecione **Create a new repository**  
3. Nomeie o novo repositório como:  
   `seu-usuario.github.io`  
   (Exemplo: `SimLinuxLab.github.io`)  
4. Clique em **Create repository**

Isso criará um site no estilo blog com o tema **Chirpy** já configurado, pronto para usar com GitHub Pages.

## ⚙️ ETAPA 2 — CONFIGURAR O ARQUIVO `_config.yml`

Dentro do repositório criado, abra o arquivo `_config.yml` e edite os campos principais:

```yaml
theme: jekyll-theme-chirpy
baseurl: ""
url: "https://seu-usuario.github.io"

title: SimLinuxLab
description: Blog técnico sobre Linux, Git e servidores

theme_mode: dark

github:
  username: SimLinuxLab