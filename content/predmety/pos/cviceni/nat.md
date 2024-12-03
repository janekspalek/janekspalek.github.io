---
title: NAT
type: docs
---

## K čemu slouží NAT (Network Address Translation)

- Toto nastavení na routeru umožňuje zařízením v **lokální síti** (`LAN`) komunikovat s **externími sítěmi**, jako je internet, pomocí **jedné nebo několika veřejných IP adres**
- `NAT` překládá `privátní` IP adresy vnitřní sítě na `veřejné` IP adresy
- Zvyšuje bezpečnost a šetří veřejný adresní prostor

## Konfigurace dynamického NAT

### 1. (optional) Nastavení IP adresy PC + gateway

**Nastavení IP adresy PC**

```
ip address add <adresa>/<maska> dev eth<cislo>

ip address add 10.0.0.2/24 dev eth1
```

**Nastavení gateway** (gateway = port na routeru)

```
ip route add default via <brana>

ip address add default via 10.0.0.1
```

### 2. Identifikace rozhraní

- **Vnitřní rozhraní** (`inside`) - rozhraní připojené k lokální síti
- **Vnější rozhraní** (`outside`) - rozhraní připojené k externí síti, například k internetu

```
(config)# interface GigabitEthernet0/0
(config-if)# ip nat inside
(config-if)# exit
(config)# interface GigabitEthernet0/1
(config-if)# ip nat outside
```

### 3. Definice rezervoáru adres

- **Na co překládám**

```
(config)# ip nat pool <nazev> <start_IP> <end_IP> netmask <maska>

(config)# ip nat pool MujNATPool 20.0.0.1 20.0.0.100 netmask 255.255.255.0
```

### 4. Vybrání překládaných adres

- **Co se má překládat**
- `wildcard` = kde jsou nuly, musí být adresa stejná jako adresa sítě

```
(config)# access-list <ACL číslo 1-99> permit <IP> <wildcard>

(config)# access-list 1 permit 10.0.0.0 0.0.0.255
```

### 5. Samotný překlad na adresy z poolu

```
(config)# ip nat inside source list <ACL číslo> pool <NAZEV> overload

(config)# ip nat inside source list 1 pool MujNATPool overload
```

### Zobrazení překladové tabulky

Zobrazení překladové tabulky

```
Router# show ip nat translations
```

Vymazání překladové tabulky

```
Router# clear ip nat translations *
```
