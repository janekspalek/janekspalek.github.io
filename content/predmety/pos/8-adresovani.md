---
title: 8) Adresování
type: docs
---

## IPv4 adresy

- Mají **32 bitů**
- Jsou zapsány jako čtyři desítkové hodnoty oddělené tečkami (`192.168.1.1`)
- Skládají se z **části pro síť** a **části pro zařízení**

## IPv6 adresy

- Mají **128 bitů**
- Jsou zapsány v hexadecimálním formátu (`2001::8a2e:0370:7334`)
- Nemají broadcasty - využívají **multicasty** místo broadcastů
- **SLAAC** (Stateless Address AutoConfiguration) - **automatické přidělování adres** na základě prefixu zadaného routerem a MAC adresou zařízení

## Porty - adresy služeb, procesů

- Porty identifikují jednotlivé služby nebo procesy na síťovém zařízení
- Číslo portu je **16bitové** (**0-65535**)
- Využívá se pro protokoly **TCP** a **UDP**

## MAC adresy

- Mají **48 bitů**
- **Jedinečný** identifikátor přiřazený každému fyzickému zařízení v síti, které podporuje síťové připojení
- Jsou zapsány v hexadecimálním formátu (`0:1A:2B:3C:4D:5E`)
- **První tři oktety jsou ID výrobce, zbylé tři jsou sériové číslo**
