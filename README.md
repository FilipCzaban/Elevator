# 🛗 Simulátor výtahu — Školní projekt

[![Status](https://img.shields.io/badge/Status-Hotovo%20%2F%20Odevzd%C3%A1no-success)](#)
[![Performance](https://img.shields.io/badge/Zrychlen%C3%AD-84.4%25-blueviolet)](#)
[![Zadání](https://img.shields.io/badge/Zad%C3%A1n%C3%AD-B%C4%9B%C5%BEn%C3%BD%20den%20ve%20%C5%A1kole%20(400%20lid%C3%AD)-blue)](#)
[![Otisk](https://img.shields.io/badge/Otisk-B746--8C6D-green)](#)

Projekt optimalizace řízení výtahu ve vícepodlažní budově. Cílem úlohy bylo upravit algoritmus v souboru `vytah.html` tak, aby lidé čekali a jeli co nejkratší dobu, a nahradit původní pomalý panelákový výtah (FIFO) vlastním efektivním algoritmem.

---

## 📁 Soubory projektu

* **[`vytah.html`](vytah.html)** — **Hlavní odevzdávaný soubor.** Obsahuje simulátor výtahu s novým optimalizovaným algoritmem v bloku `<script id="algoritmus">`.
* **[`zadani-bezny-den-ve-skole-B746-8C6D.json`](zadani-bezny-den-ve-skole-B746-8C6D.json)** — Oficiální školní zadání: 400 lidí, 8 pater (0 až 7), kapacita kabiny 6 osob (otisk `B746-8C6D`).

---

## ⚡ Rychlé spuštění a ověření

Simulátor běží přímo ve webovém prohlížeči (žádný server ani instalace není potřeba):

1. Otevři soubor **[`vytah.html`](vytah.html)** v libovolném prohlížeči (Chrome, Edge, Firefox).
2. V sekci **1 · Zadání** klikni na tlačítko:
   * **`🏫 Načíst Běžný den ve škole (400 lidí)`** *(nebo přetáhni myší soubor `zadani-bezny-den-ve-skole-B746-8C6D.json` do rámečku)*.
3. Simulace ihned proběhne a zobrazí kompletní statistiky a animaci.

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

## 🧠 Jak nový algoritmus funguje

Algoritmus je implementován v souboru [`vytah.html`](vytah.html) uvnitř `<script id="algoritmus">` a stojí na 3 klíčových principech:

### 1. Plynulý sběr po trase (LOOK / SCAN) — `rozhodni(stav)`
* Výtah nejezdí jako soukromý taxík sem a tam pro jednotlivce, ale funguje jako expresní sběrný autobus.
* Jede daným směrem a cestou:
  1. Zastavuje v nejbližších patrech, kde má vystupovat pasažér z kabiny.
  2. Pokud je v kabině volné místo (`stav.volno > 0`), přibírá další čekající jedoucí **stejným směrem**.
  3. Směr jízdy obrací až po obsloužení všech cílů ve směru jízdy.

### 2. Směrový zámek nástupu — `nastup(stav, osoba)`
* Zamezuje protisměrnému blokování kapacity kabiny.
* Pokud už v kabině někdo jede, smějí nastoupit pouze lidé jedoucí stejným směrem.
* Když je kabina prázdná, **první nastupující uzamkne směr jízdy** pro celou tuto zastávku, aby se nepomíchaly protichůdné směry.

### 3. Chytré vysílání s ochranou proti hladovění (Starvation Protection)
* Když je kabina prázdná, vybere optimální patro podle bodovací funkce:
  $$\text{Skóre} = (\text{Doba čekání} \times \text{váha}) + (\text{Počet lidí ve frontě} \times 30) - (\text{Vzdálenost} \times 15)$$
* Pokud člověk v patře čeká déle než 120 sekund, váha jeho čekání se zdvojnásobí, takže výtah nezapomene na osamocené lidi v odlehlých patrech ani při ranní špičce.

---

## ⚖️ Dodržení formálních pravidel zadání

* ✅ **Algoritmus nezná budoucnost:** Rozhoduje se pouze na základě aktuálního objektu `stav`.
* ✅ **Respektuje neznámé cíle (`destVisible = false`):** Zná pouze směr čekajících (`osoba.smer`), nikoliv jejich cílové patro (stejně jako u tlačítka se šipkou na chodbě).
* ✅ **Bleskový běh:** Výpočet každého rozhodnutí je $O(N)$ bez zbytečných vnořených smyček.
* ✅ **Čistý reset:** Funkce `zacatek(stav)` nuluje stavové proměnné před každou simulací.

---

## 👥 Odevzdání

* **Odevzdávaný soubor:** [`vytah.html`](vytah.html)
* **Přiřazené zadání:** [`zadani-bezny-den-ve-skole-B746-8C6D.json`](zadani-bezny-den-ve-skole-B746-8C6D.json) (otisk `B746-8C6D`).
