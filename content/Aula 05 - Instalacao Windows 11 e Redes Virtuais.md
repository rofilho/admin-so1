---
disciplina: Administração de Sistemas Operacionais em Redes I
codigo: "ADM-SO-01"
aula: 5
titulo: "Instalação do Windows 11 e Redes Virtuais Privadas no Hyper-V"
tipo: pratica
semana: 5
data: 2026-05-15
status: publicado
tags:
  - admin-so
  - hyper-v
  - windows-11
  - redes-virtuais
  - linux
publicar: true
---

# 🖥️ Aula 05: Instalação do Windows 11 e Redes Virtuais Privadas no Hyper-V

**Disciplina:** Administração de Sistemas Operacionais em Redes I
**Curso:** Sistemas de Informação / Redes de Computadores, Uniube
**Semana:** 5
**Professor:** Romualdo Mathias Filho
**Tipo:** 🔬 Prática
**Tópicos:** Máquinas Virtuais (Geração 2), TPM, Virtual Switches (Redes Virtuais), Configuração de IP Estático (Windows e Linux).

---

## 🎯 Objetivo da Aula

Ao final desta aula, os alunos serão capazes de:
- Compreender os requisitos avançados para virtualização de Sistemas Operacionais modernos (Geração 2, TPM, Secure Boot).
- Instalar e configurar uma Máquina Virtual com Windows 11 no Hyper-V.
- Entender as diferenças entre os três tipos de Comutadores Virtuais (Virtual Switches) do Hyper-V: Externo, Interno e Privado.
- Criar um **Virtual Switch Privado** e conectar duas VMs distintas (Windows e Linux).
- Configurar IPs estáticos em ambos os sistemas e estabelecer comunicação de rede básica (PING) entre eles.

---

## 🔄 Revisão Rápida (5 min)

| **Conceito (Aula Anterior)** | **Conexão com hoje** |
| --- | --- |
| **Virtualização (Hyper-V)** | Usamos o Hyper-V para criar um servidor Ubuntu na última aula. Hoje criaremos um cliente Windows. |
| **VM Geração 2** | O Ubuntu Server foi instalado usando UEFI e Secure Boot. O Windows 11 exigirá esses mesmos recursos. |
| **Servidor Linux sem Interface Gráfica** | Temos um Linux rodando no fundo (apenas terminal). Como vamos provar que ele funciona? Acessando-o através da rede via Windows! |

---

## 📌 1. Instalação do Windows 11 no Hyper-V

Diferente de sistemas operacionais mais antigos, o Windows 11 possui requisitos rígidos de segurança para ser instalado, mesmo em ambientes virtuais. Precisamos emular essas características de hardware no Hyper-V.

### 🛠️ Passo a Passo da Criação da VM

1. **Baixar a ISO:** Tenha em mãos o arquivo `.iso` oficial do Windows 11.
2. **Nova Máquina Virtual:** No Gerenciador do Hyper-V, clique em *Ação > Novo > Máquina Virtual*.
3. **Nome e Local:** Nomeie como `VM-Windows11-Cliente`.
4. **Geração:** ⚠️ **Crucial:** Escolha **Geração 2**. Isso habilita UEFI, necessário para o Windows 11.
5. **Memória:** Aloque pelo menos **4096 MB** (4 GB) de RAM. Desmarque "Usar Memória Dinâmica" para garantir estabilidade durante a instalação.
6. **Rede:** Por enquanto, deixe como "Não Conectado" ou use o "Default Switch".
7. **Disco Rígido Virtual:** Crie um disco de pelo menos **64 GB**.
8. **Mídia de Instalação:** Aponte para o arquivo `.iso` do Windows 11 baixado.

### 🔐 Habilitando os Requisitos de Segurança (TPM e Secure Boot)

Antes de ligar a VM, você **precisa** alterar as configurações, caso contrário, a instalação do Windows 11 falhará dizendo que "O PC não atende aos requisitos mínimos".

1. Clique com o botão direito na `VM-Windows11-Cliente` e vá em **Configurações**.
2. Na aba **Segurança**:
   - Confirme que **Habilitar Inicialização Segura (Secure Boot)** está marcado (Template: Microsoft Windows).
   - Marque a caixa **Habilitar Trusted Platform Module (TPM)**. Isso emula o chip criptográfico exigido pelo Windows 11.
3. Na aba **Processador**, aumente o número de processadores virtuais para pelo menos **2**.

### 🚀 Instalação do Sistema
- Ligue a VM. "Pressione qualquer tecla para iniciar do CD ou DVD..."
- Siga as etapas padrão (Idioma, Instalar Agora, Não tenho a chave do produto, Windows 11 Pro, Aceitar termos).
- Escolha a instalação **Personalizada (Avançada)** e selecione o disco não alocado.
- Conclua as configurações de usuário. *(Dica de sala de aula: Como não estamos usando o switch externo ainda, crie uma conta local. Se o Windows 11 exigir a conta Microsoft no OOBE, aperte `Shift + F10` para abrir o Prompt de Comando e digite: `start ms-cxh:localonly` — isso abre diretamente a tela de criação de conta local. Em versões mais antigas do Windows 11, o comando era `oobe\bypassnro`, mas foi **removido no Windows 11 24H2**. O `start ms-cxh:localonly` funciona nas versões atuais).*

---

## 📌 2. Redes Virtuais (Virtual Switches) no Hyper-V

O Hyper-V possui um "Gerenciador de Comutador Virtual" (Virtual Switch Manager). Antes de conectarmos o Linux e o Windows, precisamos entender como essas redes virtuais funcionam.

| Tipo de Switch | Conexão com a Internet? | Conexão com o Host (Máquina Física)? | Conexão entre as VMs? | Cenário de Uso Ideal |
|---|---|---|---|---|
| **Externo** | ✅ Sim | ✅ Sim | ✅ Sim | A VM funciona como se estivesse conectada no cabo da sua rede física. Pega IP do seu roteador. |
| **Interno** | ❌ Não | ✅ Sim | ✅ Sim | Testar laboratórios no seu próprio computador sem expor as VMs para a rede real. |
| **Privado** | ❌ Não | ❌ Não | ✅ Sim | **Isolamento total**. Uma rede isolada apenas para as VMs conversarem entre si. (O que usaremos hoje). |

---

## 📌 3. Conectando o Windows 11 e o Ubuntu Linux em Rede Privada

Nosso objetivo é simular uma rede local corporativa onde um cliente Windows precisa acessar os serviços de um servidor Linux interno. 

### ⚙️ Etapa A: Criar o Comutador Privado
1. No painel direito do Hyper-V, clique em **Gerenciador de Comutador Virtual**.
2. Escolha **Privado** e clique em **Criar Comutador Virtual**.
3. Nomeie como: `Rede-Isolada-AdminSO`.
4. Clique em **Aplicar** e **OK**.

### ⚙️ Etapa B: Conectar as VMs no novo Switch
1. Vá nas **Configurações** da VM Windows 11. Em "Adaptador de Rede", mude o Comutador Virtual para `Rede-Isolada-AdminSO`.
2. Vá nas **Configurações** da VM Ubuntu Server (criada na aula passada). Em "Adaptador de Rede", mude também para `Rede-Isolada-AdminSO`.
3. Ligue ambas as VMs.

> 💡 **O que aconteceu agora?** As duas máquinas estão ligadas por um "cabo de rede virtual" isolado do resto do mundo. Porém, como é uma rede privada, não existe um Servidor DHCP distribuindo IPs automaticamente. Precisamos colocar os endereços IPs na mão!

---

## 📌 4. Configurando Endereços IP Estáticos

Vamos usar a rede `192.168.100.0/24`.
- **Ubuntu Linux (Servidor):** `192.168.100.10`
- **Windows 11 (Cliente):** `192.168.100.20`

### 🐧 Configurando IP no Linux (Netplan)
O Ubuntu Server usa o utilitário `netplan` para configuração de rede.

1. Faça login no Ubuntu Server.
2. Descubra o nome da sua placa de rede com o comando: `ip a`. (Geralmente é `eth0` ou `enp0s3`).
3. Edite o arquivo de configuração (requer privilégios root):
   ```bash
   sudo nano /etc/netplan/00-installer-config.yaml
   ```
4. Apague o conteúdo (ou adapte) para ficar exatamente assim, respeitando os espaços (YAML é sensível a indentação!):
   ```yaml
   network:
     ethernets:
       enp0s3:        # ⚠️ Substitua pelo nome real da sua interface (veja o resultado do 'ip a')
         dhcp4: false
         addresses:
           - 192.168.100.10/24
     version: 2
   ```
5. Salve (`Ctrl+O`, `Enter`) e saia (`Ctrl+X`).
6. Aplique as configurações:
   ```bash
   sudo netplan apply
   ```
7. Confirme se o IP mudou digitando `ip a`.

### 🪟 Configurando IP no Windows 11
1. No Windows 11, aperte a tecla `Win` + `R`, digite `ncpa.cpl` e dê Enter (Abre as Conexões de Rede).
2. Clique com o botão direito no adaptador "Ethernet" e vá em **Propriedades**.
3. Selecione **Protocolo IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.
4. Escolha "Usar o seguinte endereço IP" e preencha:
   - Endereço IP: `192.168.100.20`
   - Máscara de sub-rede: `255.255.255.0`
5. Clique em OK e Fechar.

---

## 📌 5. Testando a Conectividade (Ping)

O último passo de um bom administrador de redes é validar se tudo funcionou.

1. **Do Linux para o Windows:**
   - No terminal do Ubuntu, digite: `ping 192.168.100.20 -c 4`
   - *Nota:* Se não responder, o Firewall do Windows 11 pode estar bloqueando pacotes ICMP (Ping). Ensine os alunos a ir no "Firewall do Windows com Segurança Avançada" e habilitar a regra de entrada para "Compartilhamento de Arquivos e Impressoras (Solicitação de Eco - ICMPv4-In)".

2. **Do Windows para o Linux:**
   - No Windows 11, abra o Prompt de Comando (cmd).
   - Digite: `ping 192.168.100.10`
   - Se os pacotes retornarem com tempo em milissegundos (ex: `Tempo<1ms`), o laboratório foi concluído com sucesso! Suas máquinas virtuais estão oficialmente conversando.

---

## 📋 Resumo Estrutural

| **Conceito** | **Definição em Uma Frase** |
| --- | --- |
| **Geração 2 (Hyper-V)** | Formato de VM moderna que suporta firmware UEFI e é obrigatória para Windows 11. |
| **vTPM** | Emulação do chip de segurança no Hyper-V para permitir instalação de SOs modernos com criptografia. |
| **Switch Externo** | Rede da VM conectada à placa de rede física da máquina host, recebendo IP do roteador real. |
| **Switch Privado** | Rede "fechada" que permite que apenas as VMs conectadas a ela conversem entre si. |
| **Netplan** | Utilitário usado nas distribuições Linux modernas para gerenciar e aplicar configurações de rede (`.yaml`). |

---

## ❓ Banco de Questões

> 🔒 Esta seção é visível apenas no Obsidian do professor. Não publicada.

### Questão 1: Prática (Múltipla Escolha — Nível: Básico)
**Enunciado:** Um administrador de sistemas instalou com sucesso uma VM Windows 11 e uma VM Ubuntu Server no Hyper-V. Ele configurou um Virtual Switch do tipo "Privado" e conectou ambas as máquinas a ele. O objetivo é que as duas máquinas consigam se comunicar entre si. Ao abrir o prompt do Windows, o administrador tenta realizar um "ping" no Google (8.8.8.8) e não obtém resposta. Por que isso acontece?

- [ ] A) O firewall do Windows bloqueia conexões de saída por padrão em redes privadas.
- [x] B) ✅ Virtual Switches do tipo "Privado" não fornecem acesso externo (Internet) ou à máquina física, permitindo apenas a comunicação entre as VMs ali conectadas.
- [ ] C) O Windows 11 requer que o Hyper-V utilize Switches do tipo Interno para suportar requisições DNS.
- [ ] D) Falta configurar o adaptador em modo "Bridge", que é a única forma de habilitar ICMP.

**Justificativa:** O comutador Privado isola completamente as máquinas virtuais, criando uma rede estritamente local entre elas, sem roteamento para a placa física hospedeira ou para a Internet.

---

### Questão 2: Prática (Múltipla Escolha — Nível: Intermediário)
**Enunciado:** Durante a criação da VM para o Windows 11 no Hyper-V, um aluno não consegue passar da tela de verificação de requisitos do Windows durante o *boot* da imagem ISO. Qual configuração do Hyper-V é **estritamente obrigatória** para superar esse erro?

- [ ] A) Selecionar uma Máquina Virtual de Geração 1 e habilitar o suporte legado a BIOS.
- [ ] B) Atribuir no mínimo 8 GB de RAM Dinâmica nas configurações de memória.
- [ ] C) Habilitar a configuração de "Switch Externo" na placa de rede, pois o instalador precisa baixar os requisitos.
- [x] D) ✅ Escolher Geração 2 ao criar a VM e marcar a opção "Habilitar Trusted Platform Module (TPM)" na guia Segurança.

**Justificativa:** O Windows 11 exige obrigatoriamente a arquitetura UEFI (disponível apenas na Geração 2 do Hyper-V) e um módulo TPM 2.0 (emulado ativando a opção de segurança na VM).

---

## 📄 Artigo de Aprofundamento

- [Virtual Switch do Hyper-V (Documentação Oficial da Microsoft)](https://learn.microsoft.com/pt-br/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines)
> *Resumo prático: Excelente material de leitura para os alunos aprofundarem nos conceitos arquiteturais de redes definidas por software no ambiente Microsoft.*

## 📚 Referências Bibliográficas e Citações

- **TANENBAUM, Andrew S.; WOODHULL, Albert S.** *Sistemas Operacionais: Projeto e Implementação*. 3ª ed. Bookman, 2008. **Páginas citadas: Cap. 8 — Virtualização, p. 487–512**
- **TANENBAUM, Andrew S.** *Redes de Computadores*. 5ª ed. Pearson, 2011. **Páginas citadas: Cap. 5 — Camada de Rede, p. 340–378** (Endereçamento IP, Sub-redes, ICMP)
- **Microsoft Learn.** *Tipos de Switch Virtual do Hyper-V*. Disponível em: [https://learn.microsoft.com/pt-br/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines](https://learn.microsoft.com/pt-br/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines). Acesso em: Maio 2026.
- **Canonical Ltd.** *Ubuntu Server — Netplan Configuration*. Disponível em: [https://netplan.io/reference](https://netplan.io/reference). Acesso em: Maio 2026.

---
*Última atualização: 2026-05-15 | Status: publicado*

