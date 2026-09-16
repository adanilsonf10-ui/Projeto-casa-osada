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

## 🛠️ Mão na Massa: Configuração de Rede & Comandos

### 🌐 Configuração do Tailscale & Redes

#### 1. Instalação do serviço oficial:
```bash
curl -fsSL [https://tailscale.com/install.sh](https://tailscale.com/install.sh) | sh ```

### 2. Autenticação e vinculação
Gera a URL de autenticação para vincular o dispositivo à sua conta Tailscale. O token de sessão é salvo e reutilizado de forma autônoma após qualquer reinicialização do sistema.
```bash
sudo tailscale up

### Mapeamento de DNS Interno (MagicDNS)
Ativado diretamente no painel administrativo do Tailscale para resolver o nome da máquina (casaosadan) de qualquer dispositivo autorizado na VPN, permitindo acesso simplificado aos serviços locais via navegador ou SSH sem precisar memorizar IPs.
4. Diagnóstico e verificação de rota
Garante que a comunicação entre o servidor e os dispositivos remotos ocorre via rota direta **(direct)**, sem degradação de performance por relay (DERP).
```bash
tailscale status

### Gestão e persistência do serviço (Systemd Daemon)
O Tailscale é registrado nativamente como um serviço do sistema para garantia de alta disponibilidade (24/7), garantindo reconexão automática em quedas de energia ou reinicializações:
```bash
sudo systemctl status tailscaled
  
### 🔮 Roadmap: O Próximo Nível (Em Desenvolvimento)
O projeto está em constante evolução. Os próximos grandes passos incluem:

- **Otimização de RAM (ZRAM): Ativação de bloco de memória RAM comprimida para aumentar o aproveitamento dos 1GB sem exigir I/O excessivo do armazenamento interno.
- **Migração para SSD Externo:
Boot e Armazenamento: Manter o boot pela eMMC e direcionar os volumes pesados do Docker/CasaOS para um SSD via porta USB.
- **Performance: Eliminar o gargalo de I/O da eMMC para dar suporte a modelos de IA Leve (TinyML/Ollama) e Media Centers (Jellyfin).

### 🧠 Engenharia com Suporte de IA
- **Este projeto utilizou o Gemini (IA da Google) como ferramenta de suporte e co-piloto de desenvolvimento. A IA auxiliou em:
Otimização de scripts de automação e shell script.
- **Pesquisa e análise de viabilidade de Kernel para a arquitetura RK322x.
Estruturação dos protocolos de rede, VPN Mesh e segurança de ponta a ponta.