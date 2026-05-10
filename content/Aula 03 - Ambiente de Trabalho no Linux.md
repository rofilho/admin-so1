---
title: "🖥️ Aula - 03: Ambiente de Trabalho no Linux: Terminal, Comandos Básicos e Permissões"
---


# 🖥️ Aula - 03: Ambiente de Trabalho no Linux: Terminal, Comandos Básicos e Permissões

# 🖥️ Aula - 03: Ambiente de Trabalho no Linux: Terminal, Comandos Básicos e Permissões

Na aula passada, vimos a **história do Unix, o movimento do software livre e o nascimento do Linux**.

Agora é hora de dar um passo essencial: aprender a **navegar e interagir com o sistema Linux**.

Grande parte da administração em servidores é feita via **linha de comando** (terminal).

Por isso, entender comandos básicos e o conceito de **usuários e permissões** é o alicerce para qualquer administrador de sistemas.

---

# 🖥️ Aula - 03: Ambiente de Trabalho no Linux: Terminal, Comandos Básicos e Permissões

# 🖥️ Aula - 03: Ambiente de Trabalho no Linux: Terminal, Comandos Básicos e Permissões

- Interface de texto para interação com o sistema.
- Permite controlar o computador com maior precisão que o ambiente gráfico.
- Em servidores, muitas vezes **só o terminal está disponível**.

# 🖥️ Aula - 03: Ambiente de Trabalho no Linux: Terminal, Comandos Básicos e Permissões

- Navegação entre diretórios:
    
    ```bash
    pwd    # Mostra o diretório atual
    ls     # Lista arquivos e diretórios
    cd     # Acessa diretórios
    ```
    
- Manipulação de arquivos e diretórios:
    
    ```bash
    touch arquivo.txt      # Cria arquivo
    mkdir pasta            # Cria diretório
    cp origem destino      # Copia
    mv origem destino      # Move ou renomeia
    rm arquivo.txt         # Remove
    ```
    
- Visualização de conteúdo:
    
    ```bash
    cat arquivo.txt        # Mostra conteúdo
    less arquivo.txt       # Navega no conteúdo
    head -n 10 arquivo.txt # Primeiras linhas
    tail -n 10 arquivo.txt # Últimas linhas
    
    ```
    

# 🖥️ Aula - 03: Ambiente de Trabalho no Linux: Terminal, Comandos Básicos e Permissões

- Linux é **multiusuário**.
- Cada usuário tem **UID (User ID)** e pertence a um ou mais **grupos**.
- Permissões básicas:
    - `r` → leitura
    - `w` → escrita
    - `x` → execução
- Representação:
    
    ```bash
    -rw-r--r-- 1 aluno aluno  1234 ago 20 14:32 arquivo.txt
    ```
    
    Aqui, o dono pode **ler e escrever**, o grupo só pode **ler**, e outros só podem **ler**.
    

# 🖥️ Aula - 03: Ambiente de Trabalho no Linux: Terminal, Comandos Básicos e Permissões

- Verificar permissões:
    
    ```bash
    ls -l
    ```
    
- Alterar permissões:
    
    ```bash
    chmod 755 script.sh
    ```
    
- Alterar dono e grupo:
    
    ```bash
    chown usuario:grupo arquivo.txt
    ```
    

---

# 🖥️ Aula - 03: Ambiente de Trabalho no Linux: Terminal, Comandos Básicos e Permissões

1. Criar uma pasta chamada `projeto` e dentro dela um arquivo `teste.txt`.
2. Listar os arquivos e verificar as permissões.
3. Alterar as permissões para que apenas o dono possa editar.
4. Mostrar a diferença entre rodar comandos como usuário comum e como **root** (comando `sudo`).

---

# 🖥️ Aula - 03: Ambiente de Trabalho no Linux: Terminal, Comandos Básicos e Permissões

- Documentação oficial do Linux (kernel.org)
- Guia Foca Linux – Módulo Básico
- The Linux Documentation Project
- Livro: **"How Linux Works" – Brian Ward**

---

# 🖥️ Aula - 03: Ambiente de Trabalho no Linux: Terminal, Comandos Básicos e Permissões

**Objetivo:** Fixar comandos básicos e permissões.

**Passos:**

1. Criar um diretório chamado `meu_usuario`.
2. Dentro dele, criar:
    - Um arquivo de texto com seu nome.
    - Uma subpasta chamada `dados`.
3. Alterar as permissões para que apenas o seu usuário possa ler e escrever nos arquivos.

---




