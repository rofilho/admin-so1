---
title: "🖥️ 🟢 Aula 04: Virtualização com Hyper-V e Instalação do Ubuntu Server"
---

---
disciplina: Administração de Sistemas Operacionais em Redes I
codigo: "ADM-SO-01"
aula: 4
titulo: "Virtualização com Hyper-V e Instalação do Ubuntu Server"
tipo: pratica
semana: 4
data: 2026-05-09
status: publicado
tags:
  - admin-so
  - hyper-v
  - virtualizacao
  - ubuntu-server
  - linux
  - redes-virtuais
publicar: true
---

# 🟢 Aula 04: Virtualização com Hyper-V e Instalação do Ubuntu Server

**Disciplina:** Administração de Sistemas Operacionais em Redes I
**Curso:** Análise e Desenvolvimento de Sistemas | Uniube
**Semana:** 4 | Sexta-feira
**Professor:** Romualdo Mathias Filho
**Tipo:** 🔬 Teórica + Prática (Laboratório)
**Tópicos:** Hyper-V, Hypervisor Tipo 1, Virtual Switches, Provisionamento de VM, Ubuntu Server, SSH

---

## 🎯 Objetivo da Aula

Ao final desta aula, os alunos serão capazes de:

- Compreender o conceito de virtualização e o papel do Hyper-V como Hypervisor nativo do Windows (Tipo 1 — Bare-Metal)
- Diferenciar os três tipos de Redes Virtuais (Switches) do Hyper-V e aplicar cada um ao cenário correto
- Criar uma Máquina Virtual (VM) do zero e realizar a instalação e configuração inicial do Ubuntu Server
- Acessar um servidor Linux remoto via SSH, simulando um ambiente corporativo ou de Cloud Computing

---

## 🔄 Revisão Rápida (5 min)

| **Conceito (Aula Anterior)** | **Conexão com hoje** |
|---|---|
| Terminal Linux e comandos básicos | Será o ambiente de operação dentro da VM Ubuntu Server |
| Usuários e permissões (`chmod`, `chown`) | Aplicados durante a configuração pós-instalação do servidor |
| Conceito de servidor vs. cliente | O Ubuntu Server que instalaremos é um servidor headless (sem interface gráfica) |

---

## 📌 1. Introdução ao Microsoft Hyper-V

### O que é o Hyper-V?

O **Hyper-V** é o sistema de virtualização baseado em hipervisor da Microsoft, classificado como **Hypervisor do Tipo 1 (Bare-Metal)**. Diferente de soluções como o VirtualBox (Tipo 2), o Hyper-V é executado diretamente sobre o hardware do hospedeiro — antes mesmo do sistema operacional — oferecendo maior **performance e isolamento**.

> 💡 **Analogia prática:** Pense no Hyper-V como o "gerente do condomínio". Ele não é um inquilino como os outros (sistemas operacionais); ele administra o prédio inteiro (hardware) e aloca apartamentos (VMs) para quem precisar. O VirtualBox, por sua vez, é um inquilino que decidiu subarrendar um cômodo do próprio apartamento — funciona, mas com mais overhead.

| **Característica** | **Hyper-V (Tipo 1)** | **VirtualBox (Tipo 2)** |
|---|---|---|
| Executa sobre | O hardware diretamente | O SO do Host |
| Performance | ⭐ Alta | Moderada |
| Overhead | Baixo | Maior |
| Uso típico | Servidores e ambientes corporativos | Desenvolvimento e testes pessoais |
| Integração Windows | ✅ Nativa | Plugins adicionais |

**Presença:** Disponível desde o Windows Server 2008 e nas edições **Pro, Enterprise e Education** do Windows 10/11.

**Vantagens principais:**
- Integração nativa com o ecossistema Windows
- Suporte a **Memória Dinâmica** (ajuste automático de RAM)
- **Checkpoints (Snapshots):** salvar o estado da VM como um "ponto de restauração"
- Suporte a **VHDX** (disco virtual de alta performance)

---

### Requisitos Mínimos para Habilitar o Hyper-V

| **Requisito** | **Detalhe** |
|---|---|
| Sistema Operacional | Windows 10/11 Pro, Enterprise ou Education (64 bits) |
| CPU | Suporte a virtualização assistida por hardware: **Intel VT-x** ou **AMD-V** |
| SLAT | Second Level Address Translation (Intel EPT ou AMD RVI) |
| RAM | Mínimo **4 GB** (recomendado 8 GB para rodar Host + VMs) |

### Como Habilitar o Hyper-V

**Via interface gráfica:** `Painel de Controle → Programas → Ativar ou desativar recursos do Windows → ✅ Hyper-V`

**Via PowerShell (Administrador):**

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```

> ⚠️ **Atenção:** É necessário **reiniciar o computador** após a ativação. O Hyper-V passa a ser o hypervisor nativo, e o próprio Windows passará a rodar como uma VM privilegiada sobre ele.

---

## 📌 2. Redes no Hyper-V — Virtual Switches

A comunicação de rede no Hyper-V é gerenciada pelo **Gerenciador de Comutador Virtual (Virtual Switch Manager)**. Compreender os tipos de switches é fundamental para a segurança e funcionalidade dos servidores virtualizados.

### Os Três Tipos de Switch Virtual

| **Tipo** | **Quem se comunica?** | **Acesso à Internet?** | **Caso de uso típico** |
|---|---|---|---|
| **Externo** | VM ↔ Rede física ↔ Host | ✅ Sim (IP do roteador real) | Servidor web exposto na rede local |
| **Interno** | VM ↔ Host (sem rede física) | ❌ Não (sem NAT manual) | Lab de estudo: acesso do Host à VM, sem exposição externa |
| **Privado** | VM ↔ VM apenas | ❌ Não | Sub-rede isolada (ex: backend de banco de dados) |

#### Switch Externo (External)

Conecta a VM diretamente à **placa de rede física do Host** (cabeada ou Wi-Fi). A VM recebe um IP do mesmo roteador/DHCP da rede local — outros computadores da rede física podem acessá-la.

> 💡 **Exemplo:** Um servidor web que precisa ser acessado por outros PCs do laboratório.

#### Switch Interno (Internal)

Permite comunicação entre as VMs e **entre as VMs e o Host**, mas **sem acesso à rede física externa** (a menos que você configure NAT manualmente no Windows).

> 💡 **Exemplo:** Ambiente de lab onde você acessa a VM do seu PC, mas não quer expô-la à rede da sala.

#### Switch Privado (Private)

Permite comunicação **apenas entre as VMs** conectadas a ele. O Host não consegue se comunicar com essas VMs.

> 💡 **Exemplo:** Simular uma sub-rede de banco de dados que só pode ser acessada por um servidor de aplicação (outra VM), nunca diretamente do exterior.

---

### 🔑 Default Switch — O Atalho Inteligente

O Windows 10/11 cria automaticamente o **"Default Switch"**. Ele funciona como um switch **interno com NAT automático** configurado pelo Hyper-V, garantindo que a VM tenha acesso à internet usando a conexão do Host — sem nenhuma configuração adicional. É o ideal para laboratório e instalações de SO.

---

## 📌 3. Laboratório Prático — Instalação do Ubuntu Server no Hyper-V

### Passo 3.1 — Download da Imagem (ISO)

1. Acesse: **[ubuntu.com/download/server](https://ubuntu.com/download/server)**
2. Baixe o **Ubuntu Server 24.04 LTS** (Long Term Support — suporte até Abril de 2029)

> 💡 **Por que LTS?** Versões LTS recebem atualizações de segurança por 5 anos. Em ambientes de servidor corporativo, estabilidade e suporte prolongado valem mais do que ter o recurso mais novo.

---

### Passo 3.2 — Criando a Máquina Virtual

1. Abra o **Gerenciador do Hyper-V**
2. No painel **Ações** (direita), clique em **Novo → Máquina Virtual...**
3. Siga o assistente conforme a tabela:

| **Etapa do Assistente** | **Valor Recomendado** | **Motivo** |
|---|---|---|
| Nome e Local | `SRV-UBUNTU-01` | Nomenclatura padrão de servidor |
| Geração | **Geração 2** | Suporta UEFI, boot mais rápido, segurança aprimorada |
| Memória | **2048 MB** + ✅ Memória Dinâmica | Permite que o Hyper-V gerencie RAM de forma inteligente |
| Rede | **Default Switch** | Garante acesso à internet durante a instalação |
| Disco Rígido Virtual | **20 GB a 30 GB** (novo disco) | Suficiente para um servidor de estudo |
| Opções de Instalação | Apontar para o `.iso` do Ubuntu Server | Boot pela imagem de instalação |

---

### Passo 3.3 — Configurando o Secure Boot (CRÍTICO para Linux na Geração 2)

Por padrão, a Geração 2 exige assinatura da **Microsoft** para o boot seguro. Como usaremos Linux, é necessário alterar o modelo de certificado:

1. Clique com o botão direito na VM → **Configurações**
2. Menu lateral → **Segurança**
3. Em **Inicialização Segura**, mantenha habilitado, mas altere o **Modelo** para:
   **`Autoridade de Certificação UEFI da Microsoft`** *(Microsoft UEFI Certificate Authority)*
4. Clique em **OK**

> ⚠️ **Sem essa configuração, a VM não conseguirá iniciar o Ubuntu e exibirá uma tela preta com erro de Secure Boot.**

---

### Passo 3.4 — Instalando o Ubuntu Server

1. Clique com o botão direito na VM → **Conectar...** → **Iniciar**
2. Siga o instalador conforme o roteiro:

| **Tela do Instalador** | **Ação** |
|---|---|
| Idioma | Selecione **English** (recomendado para servidores — facilita troubleshooting com erros em inglês) |
| Atualizar instalador | Aceite se solicitado |
| Keyboard Layout | Selecione seu layout (ex: `Portuguese (Brazil)`) |
| Network Connections | Verifique que `eth0` recebeu IP via DHCP ✅ |
| Storage | Use o disco inteiro com **LVM** (opção guiada) e confirme o particionamento |
| Profile Setup | Preencha conforme a tabela abaixo |
| **SSH Setup** | ✅ **MARQUE "Install OpenSSH server"** — essencial! |
| Featured Server Snaps | Nenhum por enquanto |
| Finalização | **Reboot Now** → pressione ENTER quando pedir para remover a mídia |

**Dados do Profile Setup:**

| **Campo** | **Valor (Exemplo)** |
|---|---|
| Your name | Seu Nome Completo |
| Server name | `srv-ubuntu-01` |
| Username | `aluno` |
| Password | `senha123` (apenas para laboratório) |

> 💡 **Por que OpenSSH?** SSH (Secure Shell) é o protocolo padrão de administração remota de servidores Linux no mundo inteiro — em nuvens (AWS EC2, Azure VMs) ou on-premises. Instalar agora já nos coloca no fluxo real de trabalho de um sysadmin.
> (TANENBAUM, Andrew S. *Redes de Computadores*. 5ª ed. Pearson, 2011. p. 767)

---

### Passo 3.5 — Pós-Instalação e Teste de Conectividade

Após a VM reiniciar, faça login com o usuário e senha criados e execute:

```bash
# Descobrir o IP da VM
ip a
```

Anote o endereço IP exibido na interface `eth0` (ex: `172.18.x.x`).

**Teste de Acesso Remoto via SSH:**

No **Windows Terminal** ou **PowerShell** do Host:

```bash
ssh aluno@<IP-DA-VM>
```

- Aceite a chave criptográfica digitando `yes`
- Insira a senha quando solicitado

> 🎯 **Parabéns!** Você está administrando um servidor Linux virtualizado via SSH — exatamente como sysadmins fazem ao gerenciar instâncias AWS EC2, Azure VMs ou servidores físicos em datacenters corporativos.

---

## 📋 Resumo Estrutural

| **Conceito** | **Definição em Uma Frase** |
|---|---|
| Hypervisor Tipo 1 (Bare-Metal) | Roda diretamente sobre o hardware, gerenciando os SOs como VMs |
| Hypervisor Tipo 2 (Hosted) | Roda dentro de um SO hospedeiro (ex: VirtualBox no Windows) |
| Switch Externo | Conecta a VM à rede física real, com IP do roteador da LAN |
| Switch Interno | Conecta VM ao Host apenas; sem acesso à rede física |
| Switch Privado | Isola VMs entre si; o Host não tem acesso |
| Default Switch | Switch interno + NAT automático; garante internet sem configuração |
| Geração 2 (VM) | Suporta UEFI, Secure Boot e boot mais rápido para SOs modernos |
| LVM | Gerenciador lógico de volumes: permite redimensionar partições sem reinstalar |
| OpenSSH | Protocolo padrão de acesso remoto seguro a servidores Linux |

---

## 🧪 Exercício Avaliativo

### Desafio — Integração de Competências

**Objetivo:** Consolidar o provisionamento de VM, redes virtuais e administração Linux remota.

**Passos:**

1. ✅ Crie a VM `SRV-UBUNTU-01` com o Ubuntu Server conforme o roteiro desta aula
2. ✅ No Hyper-V, crie um **Switch Interno** chamado `Rede_Host_Only`
3. ✅ Adicione essa nova interface de rede à VM do Ubuntu (em **Configurações → Adaptador de Rede → Adicionar Hardware**)
4. ✅ Descubra o IP da interface do switch interno (`ip a`) e acesse via SSH usando esse IP
5. ✅ No servidor Ubuntu, execute:

```bash
# Atualizar pacotes do sistema
sudo apt update && sudo apt upgrade -y

# Instalar o servidor web Apache
sudo apt install apache2 -y
```

6. ✅ Acesse o IP do servidor pelo **navegador do Windows Host** e tire um **print da tela inicial do Apache** funcionando

**Entrega:** Relatório em PDF com os prints das etapas, identificando: nome da VM, tipo de switch usado, IP obtido e tela do Apache no navegador.

---

## ❓ Banco de Questões

> 🔒 Esta seção é visível apenas no Obsidian do professor. Não publicada.

### Questão 1 — Múltipla Escolha (Nível: Básico)

**Enunciado:** Um aluno precisa criar uma VM no Hyper-V que possa ser acessada por outros computadores da rede do laboratório, como se fosse um servidor real. Qual tipo de switch virtual deve ser utilizado?

- [ ] A) Switch Privado
- [ ] B) Default Switch
- [ ] C) Switch Interno
- [x] D) Switch Externo ✅

**Justificativa:** O Switch Externo conecta a VM à placa de rede física do Host, fazendo com que ela receba um IP do roteador da rede real e seja visível para todos os dispositivos na LAN.

---

### Questão 2 — Múltipla Escolha (Nível: Intermediário)

**Enunciado:** Ao tentar iniciar uma VM Linux de Geração 2 no Hyper-V, o aluno recebe uma tela preta com erro de boot. Qual é a causa mais provável e a solução correta?

- [ ] A) A ISO está corrompida; deve-se baixar novamente
- [x] B) O Secure Boot está configurado com o modelo errado; deve-se alterar para "Microsoft UEFI Certificate Authority" ✅
- [ ] C) A VM não tem memória suficiente; deve-se aumentar para 4 GB
- [ ] D) O Default Switch não suporta Geração 2; deve-se usar Switch Externo

**Justificativa:** A Geração 2 usa Secure Boot com modelo padrão da Microsoft para Windows. Para Linux, é obrigatório mudar o modelo para "Autoridade de Certificação UEFI da Microsoft".

---

### Questão 3 — Dissertativa (Nível: Intermediário)

**Enunciado:** Explique a diferença entre um Hypervisor do Tipo 1 e um Hypervisor do Tipo 2, citando um exemplo de cada e descrevendo em qual cenário cada um é mais adequado.

**Resposta esperada:** O Hypervisor Tipo 1 (Bare-Metal), como o Hyper-V, executa diretamente sobre o hardware sem depender de um SO hospedeiro, oferecendo maior desempenho e menor latência — ideal para ambientes de produção e servidores corporativos. O Hypervisor Tipo 2 (Hosted), como o VirtualBox, roda dentro de um SO existente, sendo mais simples de instalar e ideal para desenvolvimento e testes pessoais, mas com overhead adicional.

---

### Questão 4 — Dissertativa (Nível: Avançado)

**Enunciado:** Um analista de sistemas precisa montar um laboratório com três VMs: um servidor web, um servidor de banco de dados e uma VM de roteamento. O servidor de banco de dados não deve ser acessado diretamente pela internet nem pelo computador do analista — apenas pelo servidor web. Proponha uma configuração de Virtual Switches para este cenário e justifique cada escolha.

**Resposta esperada:** VM Servidor Web: Switch Externo (acesso da LAN/internet) + Switch Privado (comunicação com o banco de dados). VM Banco de Dados: apenas Switch Privado (isolado, só acessível pela VM do servidor web). VM Roteamento (opcional): Switch Interno (comunicação com o Host para gerenciamento). Esta arquitetura implementa segmentação de rede similar à usada em ambientes de produção com zonas DMZ.

---

## 📄 Artigo de Aprofundamento

- [Microsoft Documentation — Hyper-V on Windows Server](https://learn.microsoft.com/pt-br/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)
  > *Relevância: Documentação oficial da Microsoft cobrindo todos os tipos de switches virtuais, requisitos e boas práticas de configuração do Hyper-V em ambientes de produção.*

- [Ubuntu Server Guide — OpenSSH](https://documentation.ubuntu.com/server/en/latest/how-to/security/openssh-server/)
  > *Relevância: Guia oficial da Canonical para instalação, configuração e hardening do servidor OpenSSH no Ubuntu — base para o acesso remoto desta aula.*

---

## 📚 Referências Bibliográficas e Citações

- **TANENBAUM, Andrew S.; WOODHULL, Albert S.** *Sistemas Operacionais: Projeto e Implementação*. 3ª ed. Bookman, 2008. **Páginas citadas: Cap. 8 — Virtualização, p. 487–512**
- **TANENBAUM, Andrew S.** *Redes de Computadores*. 5ª ed. Pearson, 2011. **Página citada: p. 767** (Protocolo SSH)
- **Microsoft Learn.** *Tipos de Switch Virtual do Hyper-V*. Disponível em: [https://learn.microsoft.com/pt-br/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines](https://learn.microsoft.com/pt-br/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines). Acesso em: Maio 2026.
- **Canonical Ltd.** *Ubuntu Server 24.04 LTS — Installation Guide*. Disponível em: [https://ubuntu.com/server/docs](https://ubuntu.com/server/docs). Acesso em: Maio 2026.

---

*Última atualização: 2026-05-09 | Status: publicado*
