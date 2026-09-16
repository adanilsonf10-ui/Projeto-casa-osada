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
curl -fsSL [https://tailscale.com/install.sh](https://tailscale.com/install.sh) | sh
