# 💻 Comandos Cisco - Resumo Rápido

Todos (eu acho) os comandos usados nos projetos deste repositório, organizados por categoria. Ideal pra copiar e colar na pressa. 🚀

## 🔧 Configuração básica (Switches e Roteadores)

```bash
enable                   # Entra no modo EXEC privilegiado (necessário para configurações globais)
configure terminal       # Entra no modo de configuração global
hostname NOME            # Define o nome do dispositivo
banner motd # MENSAGEM # # Configura uma mensagem de login (Message Of The Day)
line console 0           # Entra no modo de configuração da linha de console
password SENHA           # Define a senha para acesso via console
login                    # Exige a senha para login via console
exit                     # Sai do modo de configuração atual
enable secret SENHA      # Define uma senha criptografada para o modo EXEC privilegiado (mais segura que 'enable password')
service password-encryption # Criptografa senhas de texto simples nas configurações
do write memory          # Salva a configuração atual na NVRAM (memória não volátil)
do reload                # Reinicia o dispositivo
```

## 🌐 Interfaces e IP

```bash
interface fastEthernet 0/0    # Seleciona a interface Fast Ethernet 0/0
ip address 192.168.X.X 255.255.255.0 # Atribui um endereço IP e máscara de sub-rede à interface
no shutdown                   # Ativa a interface (a interface vem desativada por padrão)
exit
```

### Subinterfaces (VLANs)

```bash
interface fastEthernet 0/0.10     # Cria uma subinterface para a VLAN 10
encapsulation dot1Q 10        # Configura o encapsulamento Dot1Q para a VLAN 10
ip address 192.168.10.1 255.255.255.0 # Atribui um endereço IP à subinterface
ip helper-address 192.168.1.2 # Encaminha requisições DHCP para o servidor DHCP especificado
exit
```

## 🔁 RIP

```bash
router rip               # Habilita o protocolo de roteamento RIP
network 192.168.X.0      # Anuncia a rede para o RIP (permite que o RIP compartilhe informações de roteamento sobre essa rede)
exit
```

## 📞 VoIP (CME)

```bash
telephony-service                # Entra no modo de configuração do serviço de telefonia
max-ephones 5                    # Define o número máximo de telefones IP suportados
max-dn 5                         # Define o número máximo de diretórios (números de telefone)
ip source-address 192.168.X.X port 2000 # Define o endereço IP e porta que o CME usará para se comunicar com os telefones
auto assign 1 to 5               # Atribui automaticamente extensões aos telefones
create cnf-files                 # Gera os arquivos de configuração para os telefones IP
exit

ephone-dn 1                      # Entra no modo de configuração do diretório 1
number 2001                      # Atribui o número 2001 ao diretório
exit
```

---

## 🎯 DHCP (Roteador)

```bash
ip dhcp excluded-address 192.168.X.1 192.168.X.9 # Exclui uma faixa de endereços IP do pool DHCP
ip dhcp pool NOME                       # Cria um pool DHCP com o nome especificado
network 192.168.X.0 255.255.255.0       # Define a rede e a máscara para o pool DHCP
default-router 192.168.X.1              # Define o gateway padrão para os clientes DHCP
dns-server 192.168.X.2                  # Define o servidor DNS para os clientes DHCP
option 150 ip 192.168.X.1               # Define o endereço IP do servidor TFTP para telefones IP (usado para provisionamento de VoIP)
exit
```

## 🧱 VLAN

```bash
vlan 10                          # Cria a VLAN 10
name VOZ                         # Atribui o nome "VOZ" à VLAN 10
exit
interface range fa0/1-10         # Seleciona um intervalo de interfaces
switchport mode access           # Define o modo da porta como "access" (para dispositivos finais)
switchport access vlan 10        # Atribui a porta à VLAN 10
exit
```

ou

```bash
switchport mode trunk            # Configura a porta para operar no modo trunk
switchport trunk allowed vlan all # Permite que todas as VLANs passem por esta porta trunk
```

### VLAN com Voz

```bash
switchport voice vlan 10         # Atribui a VLAN de voz (VLAN para tráfego de voz)
mls qos trust cos                # Confia na marcação de QoS (CoS) dos pacotes de voz recebidos
spanning-tree portfast           # Ativa o PortFast para interfaces de acesso (acelera a transição para o estado de encaminhamento)
```

## 🔐 SSH

```bash
hostname R1                      # Define o nome do host
ip domain-name cisco.com         # Define o nome de domínio para geração de chaves RSA
crypto key generate rsa          # Gera um par de chaves RSA para SSH (será solicitado o tamanho da chave, 1024 bits é comum)
username USER secret SENHA       # Cria um usuário local com senha criptografada
ip ssh version 2                 # Habilita a versão 2 do SSH (mais segura)

line vty 0 15                    # Seleciona as linhas VTY (acesso remoto)
transport input ssh              # Permite apenas conexões SSH nessas linhas
login local                      # Autentica usuários usando o banco de dados de usuários local
```

## ❌ Telnet

```bash
line vty 0 4                     # Seleciona as linhas VTY (acesso remoto)
password SENHA                   # Define a senha para acesso Telnet
login                            # Exige a senha para login via Telnet
```

## 📋 Visualização de configuração

```bash
show running-config              # Exibe a configuração atual em execução na RAM
show ip interface brief          # Exibe um resumo do status das interfaces IP
show vlan brief                  # Exibe um resumo das VLANs configuradas
show mac address-table           # Exibe a tabela de endereços MAC
```