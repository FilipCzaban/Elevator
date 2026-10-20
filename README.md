# 🛗 Simulátor výtahu — Školní projekt
 
Projekt optimalizace řízení výtahu, který dosáhl celkového zrychlení **84,4 %** oproti původnímu FIFO algoritmu.
 
---
 
## 👥 Autorský tým
 
* **Filip Czaban** — Program a algoritmus
* **Tomáš Vrabec** — Dokumentace (`README.md`)
* **Ondra Góla** — Kontrola a revize
 
---
 
## 📁 Klíčové soubory
 
* **[`vytah.html`](vytah.html)** — Hlavní odevzdávaný soubor s novým optimalizovaným algoritmem.
* **[`zadani-bezny-den-ve-skole-B746-8C6D.json`](zadani-bezny-den-ve-skole-B746-8C6D.json)** — Oficiální zadání: 400 lidí, 8 pater (otisk `B746-8C6D`).
 
---
 
## ⚡ Rychlé spuštění
 
1. Otevři **[`vytah.html`](vytah.html)** v prohlížeči.
2. Načti soubor se zadáním (`zadani-bezny-den-ve-skole-B746-8C6D.json`).
3. Simulace proběhne automaticky.

---

## 📊 Výsledky měření: Původní vs. Nový výtah

Měřeno na oficiálním zadání *Běžný den ve škole* (400 lidí, 8 pater, kapacita 6 osob, otisk `B746-8C6D`):

```
Původní panelákový výtah:  ████████████████████████████████████████ 1 262 s (~21 min)
Nový chytrý algoritmus:    ████ 196 s (~3,2 min)  [-84.4 %]
```

| Metrika v simulaci | Původní panelák (FIFO) | **Nový chytrý výtah (LOOK)** | Výsledek |
| :--- | :---: | :---: | :---: |
| **Průměrně celkem** *(hlavní hodnocení)* | **1 262,1 s** (~21 min) | **196,8 s** (~3,2 min) | **🚀 zrychlení o 84,4 %** |
| **Průměrné čekání** | 1 250,7 s | **176,2 s** | **o 85,9 % kratší čekání** |
| **Průměrná jízda** | 11,4 s | **20,5 s** | Plynulý sběr po trase |
| **Nejdelší čekání** *(nejhorší případ)* | 3 493,6 s (~58 min!) | **852,2 s** (~14 min) | **Zkráceno o 44 minut** |
| **Ujetá dráha výtahu** | 2 922 m | **2 175 m** | **Úspora 747 metrů** |
| **Rozvezeno do 600 s** | 27 lidí | **51 lidí** | **+88 % větší propustnost** |
| **Celkem rozvezeno** | 400 z 400 (100 %) | **400 z 400 (100 %)** | Splněno bez chyb |

---


