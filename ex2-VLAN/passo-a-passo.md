# VLAN  
Passo a passo para configurar este projeto.  
![img](image.png)

**Objetivo:** Criar uma rede com múltiplas VLANs para segmentar o tráfego entre diferentes grupos de computadores conectados ao mesmo switch. Cada grupo só se comunica internamente, e a troca entre VLANs é controlada via **roteador** (roteamento entre VLANs), ver [ex3](../ex3-VLAN-DHCP/passo-a-passo.md).

## ⚙️ Configurações básicas nos switches e no roteador

Siga os mesmos passos do [ex1](../ex1-DHCP-DNS/passo-a-passo.md) : hostname, banner, senha, salvar e reiniciar.

## 🧩 Criando as VLANs

```bash
Switch>enable
Switch#configure terminal

Switch(config)#vlan 2
Switch(config-vlan)#name TI
Switch(config-vlan)#exit

Switch(config)#vlan 3
Switch(config-vlan)#name VENDAS
Switch(config-vlan)#exit

Switch(config)#do copy running-config startup-config
```
➡️ Isso cria as VLANs com os IDs `2` e `3` e os nomes correspondentes. Servem para agrupar máquinas logicamente.

## 🔌 Atribuindo portas às VLANs

```bash
Switch(config)#interface range fastethernet0/1-9
Switch(config-if-range)#switchport access vlan 2
Switch(config-if-range)#exit

Switch(config)#interface range fastethernet0/10-19
Switch(config-if-range)#switchport access vlan 3
Switch(config-if-range)#exit

Switch#write memory
```
➡️ Isso associa as portas físicas aos grupos. PCs conectados entre `0/1` e `0/9` fazem parte da VLAN 2 (TI), e os entre `0/10` a `0/19` fazem parte da VLAN 3 (VENDAS).

## 🖥️ Configurando os IPs dos computadores

### Manualmente

* Vá em `Desktop` → `IP Configuration`
* Adicione o IP manualmente.
  * Para a VLAN 2: algo como `192.168.2.x`, máscara `255.255.255.0`
  * Para a VLAN 3: algo como `192.168.3.x`, máscara `255.255.255.0`
* Não coloque gateway por enquanto, pois sem roteador as VLANs não se conversam mesmo.

## ✅ Testes

1. Com o **PDU** (ícone de envelope fechado), teste a comunicação entre **dois PCs da mesma VLAN** → o resultado esperado é `✔️ Sucesso`.

2. Depois teste a comunicação entre **dois PCs de VLANs diferentes** → o resultado esperado é `❌ Falha`.

➡️ Isso demonstra que as VLANs estão isoladas e funcionando corretamente.