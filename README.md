# 🚀 Projeto CasaOsada: Edge Computing Hub (RK322x)
## Da Reciclagem de Hardware ao Servidor Profissional (Mobile, IoT & IA)

Este projeto documenta a jornada técnica de transformação de uma TV Box baseada no chip **Rockchip RK322x** em um servidor Linux robusto. Uma prova de conceito de que hardware acessível pode rodar infraestruturas modernas com **Segurança Zero Trust**.

---

## 🛠️ Especificações de Hardware (Low-Level)
O "coração" do projeto é o SOC (System on a Chip) **Rockchip RK3229/RK3228a**:
- **CPU:** Quad-Core ARM Cortex-A7 (ARMv7).
- **RAM:** 1GB DDR3.
- **Storage:** eMMC Interna (Wipe total para remoção do Android Bloatware).
- **Arquitetura:** 32-bit com suporte a instruções NEON (essencial para acelerar IA leve).

---

## 🏗️ Metodologia de Transformação

### 1. Do Android ao Armbian (O Nascimento)
O primeiro passo foi o *Unbricking* e a remoção das limitações do fabricante:
- **Ferramenta:** Multitool (via MicroSD).
- **Processo:** Limpeza (Wipe) das partições originais para instalar o **Armbian (Debian-Based)** diretamente na eMMC.
- **Vantagem:** Liberamos 100% dos recursos do hardware para o kernel Linux.

### 2. Virtualização e Rede Mesh Segura
- **CasaOS & Docker:** Camada de abstração para rodar microserviços isolados.
- **Tailscale (Mesh VPN):** Túnel reverso que estabelece conexões diretas via WireGuard.
- **MagicDNS:** Mapeamento de nome de host interno, permitindo acesso universal pelo endereço `http://casaosadan`.
- **Zero Trust:** Acesso mundial sem abrir portas no roteador (*Zero Port Forwarding*), mantendo o IP público e a rede local seguros.

---
# 🛠️ Guia de Instalação e Configuração

## 1. Preparação e Arquivos Necessários

### 📥 Downloads e Recursos
* **Armbian (Atualizado):** [Release Oficial Armbian Community](https://github.com/armbian/community/releases)
* **Armbian (Maior Compatibilidade):** [Arquivo de Imagens HostHatch RK322x](https://armbian.hosthatch.com/archive/rk322x-box/archive/)
* **Multitool:** [Download Multitool Image](https://www.mediafire.com/file/2wzb3y4er4zdmld/multitool.img/file)
* **Balena Etcher:** [Download Balena Etcher v2.1.4](https://github.com/balena-io/etcher/releases/download/v2.1.4/balenaEtcher-2.1.4.Setup.exe)

### 📋 Resumo do Processo
1. **Preparação:** Baixe as imagens e utilize o **Balena Etcher** para gravar o **Multitool** no cartão de memória MicroSD.
2. **Configuração da TV Box:** Insira o cartão, formate a memória flash da TV Box (*Erase Flash*) e depois transfira a imagem do Armbian para a pasta `images` no cartão.
3. **Instalação do Armbian:** Utilize o Multitool para gravar a imagem do Armbian na memória flash (eMMC). Após o boot, configure a senha de `root` e crie o novo usuário.
4. **Ajustes de Rede e Sistema:** Atualize o sistema com `sudo apt update` e `sudo apt upgrade`, configure o IP estático via `armbian-config` e ajuste o layout do teclado.
5. **CasaOS e Servidor:** Instale o CasaOS, formate o HD externo conectado à TV Box e compartilhe a pasta de arquivos para acesso na rede local.

---

## 2. Comandos no Terminal (Após o boot do Armbian)

Estes comandos foram utilizados para atualizar e configurar o servidor:

* **Atualizar repositórios:**
```bash
  sudo apt update
```
* **Atualizar pacotes do sistema:**
```bash
sudo apt upgrade
```
* **Menu de configurações do sistema (rede, teclado, etc):**
```bash
sudo armbian-config
```
* **Navegar até o diretório de rede:**
```bash
cd /etc/net
```
* **Listar arquivos do diretório:**
```bash
ls
```
* **Renomear arquivo de rede (desativar padrão):**
```bash
sudo mv 10-xxx.yaml 10-xxx.old
```
* **Sair da sessão do terminal:**
exit
* **Desligar o sistema com segurança:**
```bash
sudo shutdown
```
### 3. Instalação do CasaOS
Com o sistema preparado, execute o comando oficial de instalação do CasaOS:
```bash
curl -fsSL [https://get.casaos.io](https://get.casaos.io) | sudo bash
```
## 🛠️ Mão na Massa: Configuração de Rede & Comandos

### 🌐 Configuração do Tailscale & Redes

#### 1. Instalação do serviço oficial:
```bash
curl -fsSL [https://tailscale.com/install.sh](https://tailscale.com/install.sh) | sh 
```

### 2. Autenticação e vinculação
Gera a URL de autenticação para vincular o dispositivo à sua conta Tailscale. O token de sessão é salvo e reutilizado de forma autônoma após qualquer reinicialização do sistema.
```bash
sudo tailscale up
```

### Mapeamento de DNS Interno (MagicDNS)
Ativado diretamente no painel administrativo do Tailscale para resolver o nome da máquina (casaosadan) de qualquer dispositivo autorizado na VPN, permitindo acesso simplificado aos serviços locais via navegador ou SSH sem precisar memorizar IPs.

### 4. Diagnóstico e verificação de rota
Garante que a comunicação entre o servidor e os dispositivos remotos ocorre via rota direta **(direct)**, sem degradação de performance por relay (DERP).
```bash
tailscale status
```

### Gestão e persistência do serviço (Systemd Daemon)
O Tailscale é registrado nativamente como um serviço do sistema para garantia de alta disponibilidade (24/7), garantindo reconexão automática em quedas de energia ou reinicializações:
```bash
sudo systemctl status tailscaled
```
  
### 🔮 Roadmap: O Próximo Nível (Em Desenvolvimento)
O projeto está em constante evolução. Os próximos grandes passos incluem:

- **Otimização de RAM (ZRAM):** Ativação de bloco de memória RAM comprimida para aumentar o aproveitamento dos 1GB sem exigir I/O excessivo do armazenamento interno.
- **Migração para SSD Externo:**
Boot e Armazenamento: Manter o boot pela eMMC e direcionar os volumes pesados do Docker/CasaOS para um SSD via porta USB.
- **Performance:** Eliminar o gargalo de I/O da eMMC para dar suporte a modelos de IA Leve (TinyML/Ollama) e Media Centers (Jellyfin).

### 🧠 Engenharia com Suporte de IA
- Este projeto utilizou o Gemini (IA da Google) como ferramenta de suporte e co-piloto de desenvolvimento. A IA auxiliou em:
Otimização de scripts de automação e shell script.
- Pesquisa e análise de viabilidade de Kernel para a arquitetura RK322x.
Estruturação dos protocolos de rede, VPN Mesh e segurança de ponta a ponta.