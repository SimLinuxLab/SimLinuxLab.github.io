---
title: "Permissões Linux: O Guia Definitivo do chmod e chown"
description: Domine a arte de proteger seus arquivos. Entenda rwx, modo octal e por que o chmod 777 é um pecado capital.
author: SimLinuxLab
date: 2025-02-14 14:00:00 -0300
categories: [Linux Essentials, Segurança]
tags: [chmod, chown, permissoes, security]
pin: false
math: false
mermaid: false
---

## 🚀 Resumo Rápido (TL;DR)

No Linux, **tudo** é arquivo e todo arquivo tem um "dono" e um conjunto de regras.
Existem apenas 3 tipos de permissões básicas:

* **r (Read/Ler) = 4**
* **w (Write/Escrever) = 2**
* **x (Execute/Executar) = 1**

> **A Matemática:**
> * **7** = 4+2+1 (Lê, Escreve, Executa)
> * **5** = 4+0+1 (Lê e Executa)
> * **0** = Nenhuma permissão
{: .prompt-info }

## 🧠 Explicação Técnica

*Baseado nos manuais LPIC-1 e Red Hat RHCSA.*

Quando você digita `ls -l`, vê algo como `-rwxr-xr--`. Isso divide-se em 3 trios:

1.  **Dono (User/u):** O criador do arquivo.
2.  **Grupo (Group/g):** A equipa do dono.
3.  **Outros (Others/o):** O resto do mundo.

### A Tabela da Verdade (Modo Octal)

| Nível | Binário | Permissão | Significado |
| :--- | :--- | :--- | :--- |
| **7** | 111 | `rwx` | Faz tudo (Perigo!) |
| **6** | 110 | `rw-` | Lê e edita (Padrão para arquivos) |
| **5** | 101 | `r-x` | Lê e roda (Padrão para scripts) |
| **4** | 100 | `r--` | Só leitura (ReadOnly) |
| **0** | 000 | `---` | Bloqueio total |

## 💡 Analogia: A Sua Casa

* **User (Dono):** Você. Tem a chave mestra, entra em qualquer quarto.
* **Group (Grupo):** Sua família. Pode entrar na sala e cozinha, mas talvez não no seu escritório.
* **Others (Outros):** O carteiro ou visitantes. Só podem entrar até o portão (leitura), não podem dormir lá (escrita).

## 💻 Exemplos Práticos (Níveis)

### 1. 👶 Junior (Simbólico)
Tornar um script executável para você (dono):

```bash
touch script.sh
chmod u+x script.sh
# Agora ficou verde no terminal!
```

### 2. 🧑‍💻 Pleno (Numérico & Dono)
Configurar um servidor web onde o dono faz tudo, o grupo lê, e os estranhos não veem nada:

```bash
# Define o dono como 'renato' e o grupo 'devs'
sudo chown renato:devs index.html

# Dono (7), Grupo (4), Outros (0)
chmod 740 index.html
```

### 3. 🧓 Senior (Bulk Fix)
Você copiou arquivos de um backup e as permissões vieram erradas (tudo executável). Vamos arrumar tudo de uma vez sem quebrar os diretórios:

```bash
# Pastas devem ser 755 (para poder entrar nelas)
find /var/www/html -type d -exec chmod 755 {} \;

# Arquivos devem ser 644 (ninguém executa texto)
find /var/www/html -type f -exec chmod 644 {} \;
```

## 🛡️ Olhar de Segurança (O Diferencial)

### O Pecado do `chmod 777`
Nunca, jamais use `chmod 777` para "resolver um erro de permissão".
* **Risco:** Você está dizendo que *qualquer pessoa* (incluindo um hacker que invadiu o site) pode apagar ou injetar vírus naquele arquivo.

### O "Sticky Bit" (O Guardião do /tmp)
Lembra do `/tmp`? Ele tem permissão `1777`.
O **1** na frente é o Sticky Bit. Ele diz: *"Todo mundo pode escrever aqui, mas **só o dono** pode apagar o seu próprio arquivo."*

## ⚠️ Troubleshooting (Erros Comuns)

**Erro:** `bash: ./script.sh: Permission denied`
**Causa:** Faltou o `+x` (bit de execução).
**Solução:** `chmod +x script.sh`

**Erro:** `chown: changing ownership of 'file': Operation not permitted`
**Causa:** Apenas o **root** pode dar arquivos de presente para outros usuários.
**Solução:** Use `sudo`.

## 📝 Nota SimLinux (Dica de Ouro)
Quer copiar as permissões de um arquivo bom para um novo, sem ter que lembrar os números? Use o `--reference`:

```bash
chmod --reference=arquivo_bom.txt arquivo_novo.txt
```
*(Isso clona as permissões instantaneamente).*