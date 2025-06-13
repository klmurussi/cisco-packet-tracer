# VLAN com DHCP
Passo a passo para configurar este projeto.
![img](image.png).

Objetivo: Criar uma rede com múltiplas VLANs, utilizando `modo trunk` entre switches e servidor DHCP para fornecer IPs automaticamente às VLANs. 

## Configurações básicas em cada switche (e roteador)
Hostname, banner, senha da console.

Seguir os passos de configurações básicas no [ex1](../ex1-DHCP-DNS/passo-a-passo.md/#configurações-básicas-em-cada-switche-e-roteador)

## Criando as VLANs
Seguir os passos de criação de VLAN no [ex2](../ex2-VLAN/passo-a-passo.md/#criando-as-vlans).

+

<p>Switch(config)#interface gigabitethernet 0/1
<p>Switch(config-if)#switch mode trunk
<p>Switch(config-if)#switch trunk allowed vlan all
<p>Switch(config-if)#do write memory

### Atribuindo portas as VLANs
Seguir os passos de atribuição de portas as VLANs no [ex2](../ex2-VLAN/passo-a-passo.md/#atribuindo-portas-as-vlans).

## No roteador
<p>Router>enable
<p>Router#configure terminal
<p>Router(config)#interface gigabitethernet0/0/0
<p>Router(config-if)#no ip address
<p>Router(config-if)#no shutdown
<p>Router(config-if)#interface gigabitethernet0/0/0.1
<p>Router(config-subif)#encapsulation dot1q 1
<p>Router(config-subif)#ip address 192.168.1.1 255.255.255.0
<p>Router(config-subif)#ip helper-address 192.168.1.2
<p>Router(config-subif)#interface gigabitethernet0/0/0.2
<p>Router(config-subif)#encapsulation dot1q 2
<p>Router(config-subif)#ip address 192.168.5.1 255.255.255.0
<p>Router(config-subif)#ip helper-address 192.168.1.2
<p>Router(config-subif)#exit
<p>Router(config)#interface gigabitethernet0/0/0.3
<p>Router(config-subif)#encapsulation dot1q 3
<p>Router(config-subif)#ip address 192.168.10.1 255.255.255.0
<p>Router(config-subif)#ip helper-address 192.168.1.2
<p>Router(config-subif)#exit
<p>Router(config)#do write memory

## No Servidor
<p>IP 192.168.1.2
<p>DNS 192.168.1.2
<p>GATEWAY 192.168.1.1

<p>Vá para `Services` -> `DHCP`:

<p>1º POOL
<p>Service `ON`.
<p>Adicione em Gateway e DNS Server, o mesmo que foi configurado anteriormente em IP configuration.
<p>Clique em `Save`.

<p>2º POOL
<p>Service `ON`.
<p>Pool name-Vlan2
<p>Adicione em DNS Server, o mesmo que foi configurado anteriormente em IP configuration.
<p>No Gateway, coloque o endereço da VLAN 2, neste caso 192.168.5.1
<p>Clique em `Add`.

<p>3º POOL
<p>Service `ON`.
<p>Pool name-Vlan3
<p>Adicione em DNS Server, o mesmo que foi configurado anteriormente em IP configuration.
<p>No Gateway, coloque o endereço da VLAN 3, neste caso 192.168.10.1
<p>Clique em `Add`.

## Configurando os IPs nos computadores
<p>Em cada computador, vá para `Desktop` -> `IP Configuration`
<p>Adicione o IPv4 via DHCP.
<p>Confira se os IPs são os esperados.

## Teste
1. Testar com PDU (o ícone de email fechado na barra de ferramentas) a comunicação entre dois computadores vlans diferentes. O resultado esperado é `sucess`.