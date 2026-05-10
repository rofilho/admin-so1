---
title: "🖥️ Aula  – História do Unix, do Software Livre, Open Source e Introdução ao Linux"
---


![Capa](assets/capa_aula02.png)

# Aula-02: História do Unix, do Software Livre, Open Source e Introdução ao Linux

Pessoal, hoje vamos dar o primeiro passo técnico na nossa jornada pela administração de sistemas operacionais em redes.

Antes de colocar a “mão na massa”, é fundamental entender **de onde vieram** os sistemas que usamos e **por que** eles funcionam do jeito que funcionam.

Saber a história do Unix, do Software Livre e do Open Source não é só curiosidade — isso nos ajuda a compreender a filosofia por trás do Linux, que influencia até hoje a forma como administramos servidores e redes.

---

### **Explicação Teórica**

### **1. Origem do Unix**

- Criado no final dos anos 1960 e início dos anos 1970 nos laboratórios Bell Labs por **Ken Thompson** e **Dennis Ritchie**.
- Desenvolvido inicialmente em **Assembly**, depois reescrito em **C** para portabilidade.
- Serviu de base para inúmeros sistemas modernos.

### **2. O Movimento do Software Livre**

- Conceito formalizado por **Richard Stallman** em 1983 com o **Projeto GNU**.
- As 4 liberdades do Software Livre:
    1. **Usar** o software para qualquer finalidade.
    2. **Estudar** o funcionamento e adaptá-lo às necessidades.
    3. **Distribuir** cópias.
    4. **Aprimorar** e disponibilizar as melhorias à comunidade.
- Criou a **Licença GPL** para garantir juridicamente essas liberdades.

### **3. Open Source (Código Aberto)**

- Termo popularizado a partir de 1998 pela **Open Source Initiative (OSI)**.
- Mais foco no **modelo de desenvolvimento colaborativo** e benefícios práticos, menos na filosofia política.
- Diferença sutil:
    - **Software Livre** enfatiza **liberdade**.
    - **Open Source** enfatiza **abertura e colaboração**.
- Todo Software Livre é Open Source, mas nem todo Open Source garante as mesmas liberdades (depende da licença).

### **4. Licenciamento no Mundo Linux**

- **GPL (GNU General Public License)**: Copyleft forte — quem modifica e redistribui deve manter a mesma licença.
- **LGPL (Lesser GPL)**: Mais permissiva para uso em bibliotecas.
- **MIT License**: Extremamente permissiva, permitindo uso comercial e código fechado.
- **Apache 2.0**: Permissiva, mas com cláusulas de patente.
- **BSD Licenses**: Permissivas, com poucas restrições.
- **Copyleft x Permissiva**:
    - **Copyleft** protege a liberdade exigindo que derivativos mantenham a mesma licença (ex.: GPL).
    - **Permissiva** permite reuso até em software proprietário (ex.: MIT, BSD).

### **5. O Nascimento do Linux**

- Criado em 1991 por **Linus Torvalds** como um kernel livre.
- Unido às ferramentas GNU, formou o **GNU/Linux**.
- Tornou-se o sistema mais usado em servidores e infraestrutura de redes.

---

### **Exemplos Práticos**

- Mostrar como identificar a licença de um software no repositório:
    
    ```bash
    apt show bash | grep License
    ```
    
- Demonstrar como encontrar licenças no GitHub:
    - Ir até um repositório → Arquivo `LICENSE`.
- Exemplo real: **Código-fonte do kernel Linux** está sob **GPLv2** — qualquer modificação distribuída precisa manter a licença.

---

### **Atividade Prática**

**Objetivo:** Explorar licenças de software e distribuições Linux.

**Passos:**

1. Escolher **dois softwares livres** usados no Linux.
2. Identificar:
    - Nome
    - Função
    - Licença utilizada
    - Link para o repositório oficial
3. Pesquisar uma distribuição Linux e descobrir qual licença cobre a maior parte de seus pacotes.
4. Criar um pequeno quadro comparativo.
5. Entregar em PDF até a próxima aula.

---

A empresa Microsoft, que no passado chamava o Linux de “câncer” (frase famosa de 2001 do ex-CEO Steve Ballmer), hoje é uma das maiores contribuidoras do kernel Linux e mantém diversos projetos open source, como o VS Code e o TypeScript.

## **Referências e Leituras Recomendadas**

### **Oficiais e Institucionais**

1. **Free Software Foundation – O que é Software Livre?**
    
    https://www.fsf.org/about/what-is-free-software.pt-br.html
    
    Explicação direta das quatro liberdades do software livre, em português.
    
2. **GNU Project – Filosofia e Licenças**
    
    https://www.gnu.org/licenses/licenses.pt-br.html
    
    Página oficial sobre licenças GNU, GPL e LGPL, com explicações e exemplos.
    
3. **Open Source Initiative – Definição de Open Source**
    
    https://opensource.org/osd
    
    Documento oficial com os critérios para um software ser considerado de código aberto.
    
4. **Choose a License**
    
    https://choosealicense.com/
    
    Guia interativo para entender e escolher licenças open source.
    

---

### **Artigos Técnicos**

1. **"The Cathedral and the Bazaar" – Eric S. Raymond**
    
    http://www.catb.org/~esr/writings/cathedral-bazaar/
    
    Clássico sobre o modelo de desenvolvimento open source, comparando-o ao modelo proprietário.
    
2. **Linux Foundation – Open Source Guides**
    
    https://opensource.guide/
    
    Coleção de guias práticos sobre como contribuir e manter projetos open source.
    
3. **Red Hat – Open Source Explained**
    
    https://www.redhat.com/en/topics/open-source/what-is-open-source
    
    Explicação simples e visual sobre o que é open source, suas vantagens e desafios.
    

---

### **Vídeos Recomendados**

1. **Richard Stallman – "Free Software, Free Society" (Palestra legendada)**
    
    [https://youtu.be/Ag1AKIl_2GM](https://youtu.be/Ag1AKIl_2GM)
    
    Palestra do criador do GNU explicando a filosofia do software livre.
    
2. **Linus Torvalds – TED Talk: The Mind Behind Linux**
    
    https://www.ted.com/talks/linus_torvalds_the_mind_behind_linux
    
    Linus fala sobre a criação do Linux e sua visão de colaboração.
