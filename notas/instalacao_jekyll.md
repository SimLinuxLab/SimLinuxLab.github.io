# 🧩 INSTALAÇÃO DO JEKYLL + CHIRPY 100% PELO GITHUB (SEM TERMINAL)

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
```

Opcional: edite também o campo `social`, `email`, `avatar`, e adicione Google Analytics se quiser.

## 📝 ETAPA 3 — PUBLICAR O PRIMEIRO POST

1. Vá até a pasta `_posts/`  
2. Crie um novo arquivo com o padrão:  
   `ANO-MES-DIA-nome.md`  
   Exemplo: `2025-12-13-bem-vindo.md`  
3. Conteúdo de exemplo:

```markdown
---
layout: post
title: "Bem-vindo ao SimLinuxLab"
date: 2025-12-13 12:00:00 +0000
categories: [inicial]
tags: [linux, jekyll]
---

Este é o primeiro post do SimLinuxLab.  
Aqui você encontrará tutoriais, anotações e experimentos práticos em Linux, Git, automações e servidores.
```

4. Dê commit nas alterações diretamente pelo GitHub Web ou usando o [github.dev](https://github.dev)

## 🚀 ETAPA 4 — ACESSAR O SITE PUBLICADO

Após o commit, o GitHub Pages irá automaticamente:
- Rodar o Jekyll remotamente
- Gerar o site
- Publicar em:

```
https://seu-usuario.github.io
```

No seu caso:  
[https://simlinuxlab.github.io](https://simlinuxlab.github.io)

Você também pode acessar diretamente o post:  
[https://simlinuxlab.github.io/posts/bem-vindo](https://simlinuxlab.github.io/posts/bem-vindo)

## ✅ RESULTADO FINAL

- 🔧 Nenhuma instalação local de Ruby, Jekyll ou Node  
- 🎨 Tema Chirpy ativado com modo escuro por padrão  
- 📝 Postagens escritas em Markdown na pasta `_posts`  
- 🌍 Site gerado via GitHub Pages (gratuito e automático)

## 💡 DICA EXTRA

Se quiser editar os arquivos com mais conforto, use:  
[https://github.dev](https://github.dev)  
É o VS Code no navegador — basta pressionar a tecla `.` quando estiver em um repositório no GitHub.

---

Feito por: Renato  
Data: Dezembro 2025  
Tema: Chirpy Starter – GitHub Pages  
Projeto: [https://github.com/SimLinuxLab/SimLinuxLab.github.io](https://github.com/SimLinuxLab/SimLinuxLab.github.io)