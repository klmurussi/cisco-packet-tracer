# Duas redes, um servidor DNS e uma página HTML  
Passo a passo para configurar este projeto.  
![img](image.png)

**Objetivo:** Fazer as duas redes se comunicarem. Uma com servidor DHCP/DNS, a outra configurada manualmente.

## ⚙️ Configurações básicas nos switches (e roteador)

### 🔠 Definindo um Hostname

```bash
Switch>enable
Switch#configure terminal
Switch(config)#hostname s1  # ou s2, sterrio, s1piso, r1, etc.
```
➡️ Isso muda o nome do dispositivo, pra facilitar a identificação na topologia.

### 🧾 Colocando um Banner

```bash
Switch(config)#banner motd # ACESSO RESTRITO #
```
➡️ O banner aparece na hora que alguém acessa o dispositivo pelo terminal. Ideal pra avisos.

### 🔒 Colocando uma Senha

```bash
Switch(config)#line console 0
Switch(config-line)#password 1234  # (pode ser uma senha mais forte)
Switch(config-line)#login
Switch(config-line)#exit
```
➡️ Isso protege o acesso via console com senha. 

### 💾 Salvando as configurações

```bash
Switch(config)#do copy running-config startup-config
```
➡️ Salva a configuração atual para que ela não se perca se o dispositivo reiniciar.

### 🔁 Reiniciando

```bash
Switch(config)#do reload
```
➡️ Reinicia o dispositivo. 

## 🌐 Configurando os IPs

### No roteador (interfaces para as redes)

```bash
Router>enable
Router#configure terminal

Router(config)#interface gigabitethernet0/0/0
Router(config-if)#ip address 192.168.5.1 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#exit

Router(config)#interface gigabitethernet0/0/1
Router(config-if)#ip address 192.168.10.1 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#exit
```
➡️ Define IPs nas interfaces do roteador que vão se conectar às duas redes. São os gateways.

## 🖥️ No servidor

### `Desktop` → `IP Configuration`

* Configure manualmente um IP na mesma rede do switch (ex: `192.168.5.2`)
* Gateway: IP do roteador (ex: `192.168.5.1`)
* DNS: o **próprio IP do servidor**

### `Services` → `DHCP`

* `Service: ON`
* `Gateway` e `DNS Server`: os mesmos definidos acima
* `Start IP Address`: por exemplo, `192.168.5.3`
* Clique em `Save`
➡️ Isso permite ao servidor distribuir IPs automaticamente para os PCs da rede.

### `Services` → `DNS`

* `DNS Service: ON`
* `Name`: por exemplo, `www.meuservidor.com`
* `Address`: IP do próprio servidor (ex: `192.168.5.2`)
* Clique em `Add`
➡️ Agora você pode acessar o servidor pelo nome e não só pelo IP.

### (Opcional) `Services` → `HTTP`

* Configure uma página HTML simples para que os PCs possam acessá-la via navegador.

## 💻 Configurando os PCs

### Manualmente (para a rede sem DHCP: `192.168.10.0`)

* `Desktop` → `IP Configuration`
* IP: ex: `192.168.10.2` -> mudar os IPs para cada computador
* Subnet: `255.255.255.0`
* Gateway: `192.168.10.1`
* DNS: `192.168.5.2` (IP do servidor da outra rede)

### Via DHCP (para a rede `192.168.5.0`)

* `Desktop` → `IP Configuration`
* Selecione `DHCP`
* Deve aparecer IP automático. Se funcionar, tá tudo certo!

## ✅ Testes

1. Use o **PDU** (ícone de envelope) para testar conectividade entre PCs e servidores.
2. Nos PCs, vá em `Desktop` → `Web Browser` → digite o domínio que você configurou (ex: `www.meuservidor.com`) — se abrir, sucesso!