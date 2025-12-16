---
layout: post
title: "Tutorial: Site Jekyll + Chirpy em 5 Minutos (Sem Instalar Nada)"
description: Guia passo a passo para lançar seu blog DevSecOps usando apenas o navegador e o GitHub, sem necessidade de Ruby ou terminal local.
author: SimLinux
date: 2025-02-14 15:00:00 -0300
categories: [Web Dev,Jekyll,Chirpy]
tags: [jekyll, github-pages, chirpy, cloud-native]
pin: true
# image:
  # path: https://chirpy-img.netlify.app/commons/devices-mockup.png
  # alt: Logo do tema Chirpy
---

## Conceito (Cloud-Native)

Por que instalar Ruby, Gemfiles e dependências na sua máquina se o **GitHub** pode fazer tudo isso por você?

Este método usa a abordagem **"Cloud-Native"**: editamos o código no navegador e deixamos a esteira de automação (CI/CD) do GitHub construir e publicar o site.

> **Vantagens:**
> * Zero instalação local.
> * Funciona em qualquer computador (até no iPad).
> * Deploy automático a cada "Save".
{: .prompt-tip }

---

## Etapa 1: Clonar o Template (O Fork)

Não comece do zero. Vamos usar o "esqueleto" oficial do Chirpy.

1.  Acesse o repositório oficial: [**chirpy-starter**](https://github.com/cotes2020/chirpy-starter).
2.  Clique no botão verde **Use this template**.
3.  Selecione **Create a new repository**.
4.  **Nome do Repositório (Crucial):**
    * Deve ser exatamente: `seu-usuario.github.io`
    * *Exemplo:* `SimLinuxLab.github.io`
5.  Clique em **Create repository**.

---

## Etapa 2: A Configuração Vital (`_config.yml`)

Agora que o repositório é seu, precisamos colocar o seu nome na porta.

1.  No seu novo repositório, abra o arquivo `_config.yml`.
2.  Clique no ícone de lápis (✏️) para editar.
3.  Altere **apenas** as linhas abaixo:

```yaml
# _config.yml

# O tema visual (não mexa)
theme: jekyll-theme-chirpy

# Se o site for seu-usuario.github.io, deixe vazio
baseurl: ""

# O endereço final do seu site
url: "[https://SimLinuxLab.github.io](https://SimLinuxLab.github.io)"

# Metadados
title: SimLinux Lab
tagline: Jornada DevSecOps
description: Blog técnico sobre Linux, Git, Automação e Segurança.

# Sua identidade no GitHub
github:
  username: SimLinuxLab
```
{: file='_config.yml'}

4.  Role até o final da página e clique em **Commit changes**.

---

## Etapa 3: O Primeiro Deploy (A Mágica)

Assim que você fez o commit acima, o GitHub detectou a mudança e iniciou um "Robô" (Action) para construir o site.

1.  Clique na aba **Actions** no topo do repositório.
2.  Você verá um processo rodando (amarelo 🟡 ou verde 🟢).
3.  Aguarde ficar **Verde**. Isso significa que o site foi construído e uma nova branch chamada `gh-pages` foi criada.

> **Atenção:** Se der erro (vermelho 🔴), geralmente é porque você escreveu algo errado no `_config.yml` (como dois pontos sem espaço).
{: .prompt-warning }

---

## Etapa 4: Ativar o Site (Settings)

O GitHub precisa saber qual pasta mostrar para o mundo.

1.  Vá na aba **Settings** (Configurações) do repositório.
2.  No menu lateral esquerdo, clique em **Pages**.
3.  Em **Build and deployment** > **Source**, selecione `Deploy from a branch`.
4.  Em **Branch**, mude de `main` (ou none) para **`gh-pages`** / `root`.
5.  Clique em **Save**.

---

## Etapa 5: Acessar

Aguarde cerca de 1 a 2 minutos. Atualize a página e você verá o link no topo:

`Your site is live at https://simlinuxlab.github.io/`

**Parabéns!** Você tem uma infraestrutura de blog profissional rodando 100% na nuvem.

---

## Próximos Passos
Para criar novos posts, basta ir na pasta `_posts` e criar arquivos seguindo o padrão `ANO-MES-DIA-titulo.md`.