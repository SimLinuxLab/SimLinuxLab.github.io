---
layout: page
title: "Git Básico: O Guia de Sobrevivência"
permalink: /tutoriais/git-basico/
icon: fab fa-git-alt
---

# 🕹️ Git Básico

> **Resumo:** O Git é a "Máquina do Tempo" do teu código. Ele permite salvar versões, voltar atrás se der erro e trabalhar em equipe sem sobrescrever o trabalho do outro.

## 1. ⚙️ Configuração Inicial (A Identidade)
Antes de qualquer coisa, você precisa dizer ao Git quem você é. Isso ficará gravado para sempre no histórico.

```bash
git config --global user.name "Seu Nome"
git config --global user.email "seu.email@exemplo.com"
```

## 2. 🏁 Iniciando um Projeto

### Criar um repositório do zero
```bash
# Transforma a pasta atual num repositório Git
git init
```

### Baixar um projeto existente
```bash
# Clona (copia) um repositório da nuvem para sua máquina
git clone [https://github.com/usuario/projeto.git](https://github.com/usuario/projeto.git)
```

## 3. 🔄 O Ciclo de Vida (O Dia a Dia)

O fluxo de trabalho do Git tem 3 estágios:
1.  **Working Dir:** Onde você edita os arquivos.
2.  **Staging Area:** Onde você escolhe o que vai ser salvo (`git add`).
3.  **Repository:** Onde a versão é gravada permanentemente (`git commit`).

```bash
# 1. Verifica o status (o que mudou?)
git status

# 2. Adiciona arquivos à área de preparação (Staging)
git add nome_do_arquivo.txt  # Um arquivo específico
git add .                    # Todos os arquivos (Cuidado!)

# 3. Grava a versão (Tira a foto)
git commit -m "feat: adiciona nova funcionalidade de login"
```

> **Dica de Ouro:** Nunca use mensagens vagas como "ajustes" ou "update". Diga **o que** você fez.
{: .prompt-tip }

## 4. 🚀 Sincronizando com o GitHub

```bash
# Envia suas alterações para a nuvem (branch main)
git push origin main

# Baixa alterações que seus colegas fizeram
git pull origin main
```

## 5. 🛡️ Segurança (DevSecOps)

O maior erro de iniciantes é subir senhas, chaves de API ou arquivos de configuração (`.env`) para o GitHub público.

### O Arquivo `.gitignore`
Crie um arquivo chamado `.gitignore` na raiz do projeto e liste o que o Git deve **ignorar**:

```text
# .gitignore
.env
node_modules/
*.log
.DS_Store
```

> **Perigo:** Se você der `git add .` sem ter um `.gitignore` configurado, você pode vazar credenciais. Verifique sempre o `git status` antes de fazer o commit.
{: .prompt-danger }

## 6. 🚑 SOS (Comandos Úteis)

* `git log`: Mostra o histórico de commits.
* `git diff`: Mostra o que você alterou no código antes de adicionar.
* `git checkout .`: Desfaz todas as alterações locais (perigoso, apaga o que você não salvou!).

---
*Manual SimLinux - Baseado nas melhores práticas de mercado.*