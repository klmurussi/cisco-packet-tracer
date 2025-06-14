# 📚 Conceitos de Redes
Resumo dos principais conceitos utilizados nos projetos do repositório. Ideal pra revisões rápidas. 😉

## 🌐 IP (Internet Protocol)
Endereço único de identificação de um dispositivo em uma rede. Pode ser:
- Estático (configurado manualmente)
- Dinâmico (atribuído automaticamente por um servidor DHCP)

## 🚪 Gateway
É o "portão de saída" da sua rede local. Geralmente é o IP do roteador que conecta sua rede a outras.

## 🌎 DNS (Domain Name System)
Serve para traduzir nomes de domínio (como `google.com`) em endereços IP. Pode ser público (ex: 8.8.8.8) ou local (ex: um servidor interno).

## 🎭 Subnet Mask (Máscara de Sub-rede)
Define o tamanho da rede e onde termina o endereço da rede e começa o do host. Exemplo clássico: `255.255.255.0` (rede com até 254 hosts).

## 🚦 Roteador (Router)
Dispositivo que conecta **redes diferentes** e decide o melhor caminho para os pacotes de dados. Também pode oferecer serviços como:
- DHCP
- NAT
- CME (para VoIP)
- Firewall básico

## 🔀 Switch
Dispositivo que conecta **dispositivos da mesma rede**. Atua na camada 2 (endereços MAC) e pode ser configurado com VLANs.

## 📡 Servidores
São dispositivos (reais ou simulados) que fornecem serviços para a rede, como:
- DHCP (atribuição automática de IP)
- DNS (resolução de nomes)
- TFTP, HTTP, FTP
- VoIP (em alguns casos)

## 🧱 VLAN (Virtual LAN)
Segmentação lógica dentro de um switch. Permite isolar grupos de dispositivos em redes separadas mesmo usando o mesmo equipamento físico.

Exemplo:  
- VLAN 10 → Voz (VoIP)  
- VLAN 20 → TI  
- VLAN 30 → Vendas

## 🎯 DHCP (Dynamic Host Configuration Protocol)
Protocolo que permite que dispositivos **recebam IPs automaticamente** ao se conectar à rede. Ele fornece:
- IP
- Gateway
- DNS
- Máscara de sub-rede

➡️ Sem DHCP, o IP precisa ser configurado manualmente.

## ☎️ VoIP (Voice over IP)
Tecnologia que permite a **transmissão de voz pela rede** (como chamadas telefônicas).

No Packet Tracer, usamos:
- Telefones IP
- VLAN de voz
- Roteador com CME (Cisco Unified Communications Manager Express)

➡️ Requer DHCP com **option 150** para informar aos telefones onde está o servidor de voz.

## 📞 CME (Cisco Unified Communications Manager Express)
Funciona como a “central telefônica” dos telefones IP. Permite:
- Criar ramais (`ephone-dn`)
- Registrar telefones (`ephone`)
- Atribuir automaticamente números com `auto assign`

➡️ Disponível no roteador Cisco **2811**.

## 🔁 RIP (Routing Information Protocol)
Protocolo de **roteamento dinâmico** usado entre roteadores. Cada roteador compartilha suas rotas com os vizinhos periodicamente.

- RIP v1: não suporta sub-redes
- ✅ RIP v2: suporta sub-redes (use sempre `version 2`)

## 🔒 SSH (Secure Shell)
Protocolo de acesso remoto **seguro e criptografado** para switches e roteadores.

Permite:
- Acesso ao terminal de outro dispositivo
- Transferência segura de arquivos (SCP/SFTP)
- Tunelamento de conexões
- Autenticação por usuário/senha ou por chaves (🔑 pública/privada)

➡️ Substitui o Telnet em ambientes reais.

## ❌ Telnet
Protocolo antigo de acesso remoto. Igual ao SSH, **mas sem criptografia**.  
➡️ Nunca deve ser usado em ambientes reais. Apenas para laboratório e estudo.

## 🧪 Comandos úteis

```bash
# Acesso remoto
telnet 192.168.1.1         # Telnet
ssh -l user 192.168.1.1    # SSH

# Exibir a configuração atual do dispositivo Cisco
show running-config

# Entrar no modo de configuração CME para VoIP
telephony-service

# Atribuir ramais automaticamente
auto assign 1 to 6