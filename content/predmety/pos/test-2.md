---
title: 2. Praktický test
type: docs
---

## DHCP

- Před konfigurací musíme nastavit na rozhraní routeru `IPv4 adresy`

```
Router(config)# interface <INTERFACE>
Router(config-if)# ip address <IP_ADDRESS> <SUBNET_MASK>
Router(config-if)# no shutdown
```

### 1. Konfigurace DHCP poolu

- `POOL_NAME` je název DHCP poolu (např. mujPool)
- `NETWORK` a `SUBNET_MASK` definují obsluhovanou síť
- `DEFAULT_ROUTER` je gateway (výchozí brána) pro klienty
- `DNS_SERVER` je **volitelný** server DNS

```
Router(config)# ip dhcp pool <POOL_NAME>
Router(dhcp-config)# network <NETWORK> <SUBNET_MASK>
Router(dhcp-config)# default-router <DEFAULT_ROUTER>
Router(dhcp-config)# dns-server <DNS_SERVER>

Příklad:
Router(config)# ip dhcp pool mujPool
Router(dhcp-config)# network 192.168.1.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.1.1
Router(dhcp-config)# dns-server 8.8.8.8
```

### 2. Vyloučení adres z DHCP poolu

- Pokud máme některé adresy, které chceme **zarezervovat** (např. pro servery nebo tiskárny)

```
Router(config)# ip dhcp excluded-address <START_IP> <END_IP>

Příklad:
Router(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.10
```

### 3. Aktivace DHCP na jednotlivých PC

- V Packet Traceru musíme u každého PC povolit DHCP kliknutím na tlačítko DHCP v sekci IP configuration

![DHCP Packet Tracer](image.png)

### 3. Kontrolní příkazy pro DHCP

```
Router# show ip dhcp binding

Router# show ip dhcp pool
```
