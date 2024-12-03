---
title: Teorie grafů
type: docs
---

https://homel.vsb.cz/~dor028/Teorie_grafu.pdf

## Definice grafu

- **Graf G** je uspořádaná dvojice `G = (V, E)`, kde V je **neprázdná množina vrcholů** a E je **množina hran** - množina (některých) dvouprvkových podmnožin množiny
- Prvky množiny V nazýváme **vrcholy**
- Prvky množiny E nazýváme **hrany**

## Orientované grafy a multigrafy

- Orientovaných grafem rozumíme uspořádanou dvojici `G = (V, E)`, kde V je neprázdná množina vrcholů a **množina orientovaných hran** je `E ⊆ V x V`
- V nakreslení znázorňujeme orientované hrany **šipkami**
- Obecnější pojem než orientovaný graf je **multigraf**

## Princip sudosti

- Součet stupňů v grafu je vždy **sudý** a je roven **dvojnásobku počtu hran**

![alt text](image-1.png)

## Věta Havlova-Hakimiho

![alt text](image.png)

## Indukovaný podgraf

- Vznikne tak, že vezmu vybrané uzly z původního grafu a zachovám jen ty **hrany**, které mezi nimi už v původním grafu **existovaly**
- Žádné nové hrany neměním, jen "zdědím" vztahy mezi těmi vybranými uzly

![alt text](image-2.png)

## Isomorfismus grafů

- Grafy, které jsou navzájem **ekvivalentní**, liší se pouze **jiným označením** vrcholů a hran a **jiným způsobem zakreslení**
- **Počty vrcholů** grafů musí být stejné
- **Počty hran** grafů musí být shodné
- **Počty vrcholů daných stupňů** musí být stejné

![alt text](image-3.png)

![alt text](image-4.png)

## Sled grafu

- "Procházka" po uzlech, kde jdu z jednoho uzlu do druhého po hranách
- Můžu chodit **dokola**, **vracet se na stejné uzly** a také přecházet stejné hrany vícekrát
- **Vrchol V je dosažitelný z vrcholu U, jestliže v grafu existuje sled z vrcholu U do vrcholu V**
- Souvislý graf musí mít pro každé 2 vrcholy U, V je vrchol V dosažitelný z vrcholu U

![alt text](image-5.png)

![alt text](image-6.png)

![alt text](image-7.png)

## Tah a cesta grafu

- Tah grafu je sled, ve kterém se **neopakují žádné hrany**
- Cesta je sled, ve kterém se neopakují žádné vrcholy, tedy ani hrany

![alt text](image-8.png)

## Komponenta grafu

- Samostatná část grafu, kde jsou všechny uzly propojené, ale na žádné uzly mimo tuto část se nedostanu
- Je to každý maximální souvislý podgraf grafu G

![alt text](image-9.png)

## Vyšší stupně souvislosti

- V praxi nás může zajímat nejen, jestli existuje spojení (cesta) mezi vrcholy, ale také, jestli bude existovat spojení v případě výpadků některých vrcholů

![alt text](image-10.png)

![alt text](image-11.png)

![alt text](image-12.png)

## Eulerovský graf

- Pokud začínám a končím ve stejném vrcholu = **uzavřený eurelovský tah = eulerovský graf**
- Pokud ale končím jinde než jsem začal = **otevřený eulerovský tah**
- Říkáme, že takový graf lze **nakreslit jedním tahem**

![alt text](image-13.png)

![alt text](image-14.png)

![alt text](image-15.png)

![alt text](image-16.png)

## Hamiltonovský graf

![alt text](image-17.png)

## Vzdálenost grafu

- Pro neohodnocené grafy můžeme předpokládat, že **délka každé hrany je 1**
- Nejkratší sled je vždy **cestou**
- **Ohodocený graf** je ten, která má **každou hranu ohodnocenou**
- **Kladně ohodnocený graf** je ten, který má **všechny ohodnocení kladné**
- **Délka ohodnoceného sledu** je **součet vah** všech jeho hran

![alt text](image-18.png)

## Dijkstrův algoritmus

- Využívá se pro **hledání nejkratší** (ohodnocené) **cesty** mezi **dvěma vrcholy** váženého (kladně ohodnoceného) grafu
- Najde vzdálenosti z jednoho vrcholu do ostatních (místo mezi všemi dvojicemi vrcholů)
- Reálné využití například pro vyhledávání autobusových spojení
