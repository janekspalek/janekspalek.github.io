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

![DHCP Packet Tracer](/images/dhcp.png)

### 3. Kontrolní příkazy pro DHCP

```
Router# show ip dhcp binding

Router# show ip dhcp pool
```

## ACL

- Všechny pakety, které se nerovnaly žádnému pravidlu se vyhodí
- **Vždy je nutnost povolit i** `zpětný směr`
- Každý záznam se do access-listu se přidává **postupně** , **nakonec**
- `any` = **libovolná** IP adresa + wildcard maska 255.255.255.255
- `host x.x.x.x` = IP adresa x.x.x.x + wildcard maska 0.0.0.0
- Když povolujeme cokoliv s TCP, musíme na zpětný směr dát `established`. Tím povolíme jen komunikaci, která byla zahájena z naší vnitřní sítě. Je to kvůli tomu jak TCP navazuje spojení - three-way-handshake
- Pro **ping** použijeme `echo` a `echo-reply` (odpověď z pingu)
- U **SMTP pozor** - může se chovat jako server i jako klient

### 1. Vytvoření access listu

- `PROTOCOL` - TCP, UDP, ICMP nebo IP (pro všechny protokoly)
- `SOURCE_PORT` `DESTINATION_PORT` - porty pro služby jako HTTP (80), před portem musí být `eq`

```
access-list <NUMBER> {permit|deny} <PROTOCOL> <SOURCE_IP> <WILDCARD_MASK> [<SOURCE_PORT>] <DESTINATION_IP> <WILDCARD_MASK> [<DESTINATION_PORT>] [PROTOCOL_PARAMETERS]
```

### 2. Přiřazení ACL na rozhraní

```
Router(config)# interface <INTERFACE>
Router(config-if)# ip access-group <ACCESS_LIST_NUMBER> {in|out}
```

### ACL příklad 1

- Zakažte veškerý provoz, který nesměřuje na proxy server poskytovatele 40.0.0.1
  ![alt text](/images/priklad1.png)

```
-- Odchozí směr (OUT)
Router(config)# access-list 101 permit ip any host 40.0.0.1
Router(config)# interface gi0/0/0
Router(config-if)# ip access-group 101 out
------------------------------------------
-- Příchozí směr (IN)
Router(config)# access-list 102 permit ip host 40.0.0.1 any
Router(config)# interface gi0/0/0
Router(config-if)# ip access-group 102 in
```

### ACL příklad 2

- Povolte do Internetu protokoly DNS a HTTP(S).

```
-- Odchozí směr (OUT)
Router(config)# access-list 103 permit udp any any eq 53
Router(config)# access-list 103 permit tcp any any eq 53
Router(config)# access-list 103 permit tcp any any eq 80
Router(config)# access-list 103 permit tcp any any eq 443
------------------------------------------
-- Příchozí směr (IN)
Router(config)# access-list 104 permit udp any eq 53 any
Router(config)# access-list 104 permit tcp any eq 53 any established
Router(config)# access-list 104 permit tcp any eq 80 any established
Router(config)# access-list 104 permit tcp any eq 443 any established
```

### ACL příklad 3

- Zakažte pro síť 10.0.20.0/24 ICMP provoz s výjimkou použití příkazu ping do vnější sítě. Veškerý další provoz je povolen.

```
-- Odchozí směr (OUT)
Router(config)# access-list 105 permit icmp 10.0.20.0 0.0.0.255 any echo
Router(config)# access-list 105 deny icmp 10.0.20.0 0.0.0.255 any
Router(config)# access-list 105 permit ip any any
------------------------------------------
-- Příchozí směr (IN)
Router(config)# access-list 106 permit icmp any 10.0.20.0 0.0.0.255 echo-reply
Router(config)# access-list 106 deny icmp any 10.0.20.0 0.0.0.255
Router(config)# access-list 106 permit ip any any
```

### ACL příklad 4

- Povolte přístup z vnějšku na POP3 servery v síti 100.10.20.40/30 a SMTP server 100.10.20.45.

```
-- Odchozí směr (OUT)
Router(config)# access-list 107 permit tcp 100.10.20.40 0.0.0.3 eq 110 any established
Router(config)# access-list 107 permit tcp host 100.10.20.45 eq 25 any established
Router(config)# access-list 107 permit tcp host 100.10.20.45 any eq 25
------------------------------------------
-- Příchozí směr (IN)
Router(config)# access-list 108 permit tcp any 100.10.20.40 0.0.0.3 eq 110
Router(config)# access-list 108 permit tcp any host 100.10.20.45 eq 25
Router(config)# access-list 108 permit tcp any eq 25 host 100.10.20.45 established
```

### Seznam portů pro jednotlivé služby

- 22 - SSH
- 23 - TELNET
- 25 - SMTP
- 80 - HTTP
- 110 - POP3
- 143 - IMAP
- Další - FTP (20, 21), HTTPS (443), DHCP (67, 68), DNS (53)
