## 📝 Desafio Proposto

O projeto consiste na resolução da avaliação prática final do módulo **CCNA v7 - Introduction to Networks (ITN)**. O cenário exige o planejamento de um esquema de endereçamento IP híbrido (IPv4 e IPv6) e a configuração prática de ativos Cisco (Roteador e Switch) via CLI para garantir a segurança da infraestrutura e a conectividade ponta a ponta.

### 🎯 Objetivos Principais:
* **Subnetting Avançado (VLSM):** Dividir uma rede base de forma eficiente para atender a diferentes escopos de hosts, minimizando o desperdício de endereços.
* **Hardening de Roteador:** Aplicar práticas recomendadas de segurança no IOS, incluindo requisitos de complexidade de senha, criptografia local e acesso remoto seguro via SSH.
* **Gerenciamento de Switch:** Habilitar e configurar a Interface Virtual do Switch (SVI) para gerenciamento em banda (In-Band Management).
* **Conectividade Dual-Stack:** Configurar hosts finais e servidores utilizando simultaneamente pilhas de protocolos IPv4 e IPv6.

---

### 📋 Requisitos Técnicos por Dispositivo

#### 1. Roteador (CS-Department)
* **Configurações Iniciais:** Alteração do hostname, mensagem de aviso (Banner MOTD) e desativação de texto claro para senhas corporativas.
* **Políticas de Senha:** Impor comprimento mínimo de 10 caracteres para novas credenciais.
* **Segurança de Acesso:** Proteção das linhas de Console e VTY, exigindo autenticação local com privilégios administrativos.
* **Gerenciamento Seguro:** Bloqueio do protocolo Telnet, habilitando exclusivamente acessos via **SSH** com chaves RSA de 1024 bits.
* **Interfaces:** Ativação e endereçamento das interfaces `GigabitEthernet0/0` (LAN 124-C) e `GigabitEthernet0/1` (LAN 214-A), incluindo a definição manual de endereços Link-Local (`fe80::1`) e descrições textuais.

#### 2. Switch (LAB 214-A)
* **Interface Virtual (SVI):** Ativação e configuração de IP na `VLAN 1` para gerência interna.
* **Roteamento de Gerência:** Configuração de gateway padrão para permitir o acesso administrativo vindo de sub-redes externas.
* **Acesso Remoto:** Habilitação de linhas VTY para gerenciamento em banda.

#### 3. Hosts e Servidores (PCs & TFTP Server)
* Configuração manual de endereçamento de rede (IPv4 estático e IPv6 Global Unicast).
* Associação correta dos respectivos gateways padrão (IPv4 e Link-Local IPv6).

---

### 🗺️ Regras de Negócio para o Esquema IP (Subnetting)
O desenho da rede deve seguir estritamente os critérios lógicos fornecidos pelo exame:

1. **Primeira Divisão (LAN 124-C):** Sub-redirecionar a rede base `192.168.1.0/24` para blocos que suportem exatamente **30 hosts** úteis. A **4ª sub-rede** gerada deve ser atribuída a esta LAN.
   * *Regra de Gateway:* O último IP válido desta sub-rede deve ser aplicado na interface `G0/0` do Roteador.
2. **Segunda Divisão / VLSM (LAN 214-A):** A partir da **5ª sub-rede** criada no passo anterior, realizar um novo desmembramento (VLSM) para gerar blocos menores que atendam a **14 hosts** úteis. A **2ª nova sub-rede** gerada deve ser atribuída a esta LAN.
   * *Regra de Gateway:* O último IP válido deste novo bloco deve ser aplicado na interface `G0/1` do Roteador.
   * *Regra do Switch:* O penúltimo IP válido deste bloco deve ser o endereço de gerenciamento do Switch `LAB 214-A`.
