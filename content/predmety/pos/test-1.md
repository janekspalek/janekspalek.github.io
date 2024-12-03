---
title: 1. Praktický test
type: docs
---

## VLAN

### 1. Vytvoření VLAN

- `VLAN_ID` je číslo VLAN (např. 10, 20 atd.)
- `VLAN_NAME` je libovolný název VLAN, třeba Sales, IT

```
Switch(config)# vlan <VLAN_ID>
Switch(config-vlan)# name <VLAN_NAME>
```

### 2. Přiřazení Access portů do VLAN

- `INTERFACE` je název portu, např. FastEthernet 0/1

```
Switch(config)# interface <INTERFACE>
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan <VLAN_ID>
```

### 3. Přiřazení Trunk portů do VLAN

```
Switch(config)# interface <INTERFACE>
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 10,20

(OPTIONAL) - Nastavení nativní VLAN
Switch(config-if)# switchport trunk native vlan <VLAN_ID>
```

### 4. Odstranění VLAN

```
Switch(config)# no vlan <VLAN_ID>
```

### 5. Zobrazení VLAN

```
Switch# show vlan brief
```

## RIPv2

- Před konfigurací musíme všude nastavit `IPv4 adresy` a `gateway`
- Na rozhraních `no shutdown` aby bylo zapnuté
- Nejprve se RIP zapne na celém routeru, vybere se verze 2 a přidají se jednotlivé networky připojené přímo k routeru

### 1. Aktivace RIPv2

```
Router(config)# router rip
Router(config-router)# version 2
```

### 2. Sítě, které se mají propagovat

- Pro každou síť, kterou router zná (má k ní připojené rozhraní), jeden řádek network <ADDRESS>

```
Router(config-router)# network 192.168.1.0
Router(config-router)# network 10.0.0.0
```

### 3. Vypnutí RIP na určitých rozhraních

- Pokud nechceme, aby router posílal RIP aktualizace na určité rozhraní (např. do LAN sítě s klienty)

```
Router(config-router)# passive-interface <INTERFACE>
```

### 4. Statická výchozí cesta

- Všechny pakety které jdou na IP adresu, která nemá cestu v RIP tabulce
- `NEXT_HOP` je IP adresa směrovače, kterým by měl paket projít na cestě k cílové IP adrese

```
Router(config)# ip route 0.0.0.0 0.0.0.0 <NEXT_HOP>

Router(config-router)# default-information originate
```

### 5. Zobrazení směrovací tabulky

```
Router# show ip route

Router# show ip rip database
```

## RIPng

- Router musí mít povolené adresování pomocí IPv6 příkazem `ipv6 unicast-routing`
- Router i PC musí mít nakonfigurované `IPv6 adresy` a `gateway`
- Pak stačí jen zapnout ipv6 RIP na každém rozhraní

### 1. Aktivace RIPng

- RIPng se aktivuje na každém rozhraní zvlášť
- `PROCESS_NAME` je libovolné jméno procesu, např. RIPng1

```
Router(config)# ipv6 router rip <PROCESS_NAME>
```

### 2. Přiřazení RIPng k rozhraní

- Pro každé rozhraní, které chceme přidat do RIPng, aktivujeme RIPng se `stejným` názvem procesu

```
Router(config)# interface <INTERFACE>
Router(config-if)# ipv6 rip <PROCESS_NAME> enable
```

### 3. Aktivace automatické propagace sítí

ASI NENI POTREBA

```
(config-router)# redistribute connected metric 1
```

### 4. Statická výchozí cesta

```
Router(config)# ipv6 route ::/0 <NEXT_HOP>
Router(config)# ipv6 router rip <PROCESS_NAME>
Router(config)# default-information originate
```

### 5. Zobrazení směrovací tabulky

```
Router# show ipv6 route

Router# show ipv6 rip database
```

## OSPF

- Před konfigurací musíme všude nastavit `IPv4 adresy` a `gateway`
- Na rozhraních `no shutdown` aby bylo zapnuté
- Nejprve se OSPF zapne na celém routeru a přidají se jednotlivé networky připojené přímo k routeru

### 1. Aktivace OSPF

- `PROCESS_ID` je libovolné ID procesu pro lokální směrovač, nemusí se shodovat mezi různými směrovači

```
Router(config)# router ospf <PROCESS_ID>
```

### 2. Přidání sítí do OSPF

- `WILDCARD_MASK` je invertovaná maska sítě

```
Router(config-router)# network <NETWORK_ADDRESS> <WILDCARD_MASK> area 0
Router(config-router)# network 10.0.0.0 0.0.0.255 area 0
```

### 3. Vypnutí OSPF na určitých rozhraních

```
Router(config-router)# passive-interface <INTERFACE>
```

### 4. Statická výchozí cesta

- `NEXT_HOP` je IP adresa směrovače, kterým by měl paket projít na cestě k cílové IP adrese

```
Router(config)# ip route 0.0.0.0 0.0.0.0 <NEXT_HOP>

Router(config-router)# default-information originate
```

### 5. Zobrazení směrovací tabulky

```
Router# show ip route ospf
```

## OSPFv3

- Router musí mít povolené adresování pomocí IPv6 příkazem `ipv6 unicast-routing`
- Router i PC musí mít nakonfigurované `IPv6 adresy` a `gateway`
- Pak stačí vytvořit OSPFv3 proces a tento proces zapnout na jednotlivých rozhraních

### 1. Aktivace OSPFv3

- `PROCESS_ID` je libovolné ID procesu pro lokální směrovač, nemusí se shodovat mezi různými směrovači

```
Router(config)# ipv6 router ospf <PROCESS_ID>
```

### 2. Přiřazení OSPF k jednotlivým rozhraním

```
Router(config)# interface <INTERFACE>
Router(config-if)# ipv6 ospf <PROCESS_NAME> area 0
```

### 3. Vypnutí OSPFv3 na určitých rozhraních

```
(config-router)# passive-interface <INTERFACE>
```

### 4. Statická výchozí cesta

```
Router(config)# ipv6 route ::/0 <NEXT_HOP>
Router(config)# ipv6 router ospf <PROCESS_NAME>
Router(config)# default-information originate
```

## NAT

- Před konfigurací musíme všude nastavit `IPv4 adresy` a `gateway`
- Na rozhraních `no shutdown` aby bylo zapnuté
- Nastaví se rozhraní `inside`
- Nastaví se rozhraní `outside`
- Vytvoří se `Access list` adres vnitřní sítě, které se budou překládat
- Vytvoří se `NAT pool` adres vnější sítě, na které se adresy vnitřní sítě budou překládat
- Zapne se dynamický NAT

### 1. Nastavení inside a outside rozhraní

```
Router(config)# interface <INTERFACE>
Router(config-if)# ip nat inside

Router(config)# interface <INTERFACE>
Router(config-if)# ip nat outside
```

### 2. Vytvoření Access listu adres, které se mají překládat

```
Router(config-if)# access list 1 permit <NETWORK_ADRESS> <WILDCARD_MASK>

Router(config-if)# access-list 1 permit 10.0.0.0 0.0.0.255
```

### 3. Vytvoření NAT poolu adres, na které se má adresy překládat

```
Router(config)# ip nat pool <POOL_NAME> <START_IP> <END_IP> netmask <MASK>

Router(config)# ip nat pool MujNATPool 20.0.0.1 20.0.0.100 netmask 255.255.255.0
```

### 4. Zapnutí samotného překladu NAT

```
Router(config)# ip nat inside source list 1 interface <INTERFACE>

Router(config)# ip nat inside source list 1 pool MujNATPool
```

## Tabulka masek

![Tabulka masek](/images/subnet-mask.png)
