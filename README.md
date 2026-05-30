# 📊 SQL Projekt – Analýza mezd, cen potravin a HDP v České republice (2006–2018)

## Zadání projektu

Cílem projektu je analyzovat vývoj mezd, cen potravin a HDP v České republice v období let 2006–2018 a odpovědět na následující výzkumné otázky:

1. Rostou mzdy ve všech odvětvích, nebo v některých odvětvích klesají?
2. Kolik litrů mléka a kilogramů chleba bylo možné koupit za průměrnou mzdu v prvním a posledním srovnatelném období?
3. Která kategorie potravin zdražuje nejpomaleji?
4. Existuje rok, ve kterém byl meziroční nárůst cen potravin výrazně vyšší než růst mezd (o více než 10 %)?
5. Má výška HDP vliv na změny ve mzdách a cenách potravin?

---

## Použité datové zdroje

Projekt pracuje se dvěma finálními tabulkami:

* `t_lenka_trachtova_project_sql_primary_final` – data o mzdách, cenách potravin a HDP v České republice
* `t_lenka_trachtova_project_sql_secondary_final` – evropská makroekonomická data (HDP, GINI koeficient, populace)

---

## Poznámky ke kvalitě dat

### 1. Chybějící region u některých potravin

V tabulce `czechia_price` některé položky neobsahují regionální údaj. Tyto záznamy byly ponechány v analýze, protože se pravděpodobně jedná o celostátní průměry a jejich odstranění by mohlo vést ke zkreslení výsledků.

### 2. Nesrovnalosti v tabulce mezd

V tabulce `czechia_payroll` byly nalezeny nesrovnalosti mezi sloupci `value_type_code` a `unit_code`, například:

* počet zaměstnanců uvedený v korunách,
* mzdy uvedené v tisících osob.

Z tohoto důvodu byly do analýzy zahrnuty pouze záznamy s:

`value_type_code = 5958` (průměrná hrubá mzda)

Sloupec `unit_code` nebyl použit.

### 3. Opakování hodnot HDP

HDP je celostátní ukazatel a v primární tabulce se opakuje u každé kombinace odvětví a potravinové kategorie.

Při výpočtech bylo proto použito agregování pomocí funkce AVG(), aby byla pro každý rok zachována pouze jedna hodnota HDP.

### 4. Různá granularita dat

Datové zdroje pracují s různou úrovní detailu:

* mzdy: odvětví × rok,
* ceny: potravina × rok,
* HDP: rok.

Primární tabulka proto představuje kombinaci těchto úrovní detailu.

---

# Výsledky analýzy

## 1. Vývoj mezd podle odvětví

Mzdy během sledovaného období dlouhodobě rostly, avšak jednotlivá odvětví vykazovala výrazně odlišnou dynamiku.

### Největší meziroční poklesy

| Rok  | Odvětví                      |   Změna |
| ---- | ---------------------------- | ------: |
| 2013 | Peněžnictví a pojišťovnictví | -9,00 % |
| 2009 | Těžba a dobývání             | -4,36 % |
| 2013 | Energetika                   | -4,29 % |

### Největší meziroční růsty

| Rok  | Odvětví                                |    Změna |
| ---- | -------------------------------------- | -------: |
| 2008 | Těžba a dobývání                       | +13,87 % |
| 2008 | Energetika                             | +13,37 % |
| 2008 | Profesní, vědecké a technické činnosti | +13,20 % |

### Závěr

Rok 2008 byl obdobím výrazného růstu mezd. Naopak rok 2013 přinesl největší poklesy. Nejvyšší volatilitu vykazovala odvětví těžby, energetiky a finančnictví.

---

## 2. Kupní síla průměrné mzdy

Porovnány byly roky 2006 a 2018.

| Rok  | Potravina |     Cena | Průměrná mzda |    Množství |
| ---- | --------- | -------: | ------------: | ----------: |
| 2006 | Chléb     | 16,12 Kč |  20 342,38 Kč | 1 261,93 kg |
| 2006 | Mléko     | 14,44 Kč |  20 342,38 Kč |  1 408,75 l |
| 2018 | Chléb     | 24,24 Kč |  31 980,26 Kč | 1 319,32 kg |
| 2018 | Mléko     | 19,82 Kč |  31 980,26 Kč |  1 613,53 l |

### Závěr

Kupní síla obyvatel vzrostla u obou sledovaných komodit. Růst mezd byl rychlejší než růst cen chleba i mléka.

---

## 3. Která potravina zdražuje nejpomaleji?

Výpočet vychází z průměrného meziročního procentního růstu cen.

### Nejpomaleji zdražující potraviny

| Potravina       | Průměrná změna |
| --------------- | -------------: |
| Cukr krystalový |        -1,92 % |
| Rajčata kulatá  |        -0,74 % |
| Banány žluté    |        +0,81 % |

### Nejrychleji zdražující potraviny

| Potravina | Průměrná změna |
| --------- | -------------: |
| Papriky   |        +7,29 % |
| Máslo     |        +6,68 % |
| Vejce     |        +5,56 % |

### Závěr

Vývoj cen potravin nebyl jednotný. Některé komodity dlouhodobě zlevňovaly, zatímco jiné zaznamenaly výrazný růst.

---

## 4. Existuje rok, kdy ceny potravin rostly výrazně rychleji než mzdy?

Bylo zjištěno, že ve sledovaném období neexistoval rok, kdy by meziroční růst cen potravin převýšil růst mezd o více než 10 procentních bodů.

### Závěr

Výrazné zaostávání mezd za růstem cen potravin nebylo ve sledovaném období potvrzeno.

---

## 5. Má HDP vliv na růst mezd a cen potravin?

Analýza meziročních změn HDP, mezd a cen potravin neprokázala jednoznačnou přímou závislost.

Byly identifikovány situace, kdy:

* HDP rostlo a mzdy stagnovaly,
* ceny potravin rostly i při poklesu HDP,
* mzdy rostly výrazně rychleji než HDP.

### Závěr

Výsledky naznačují, že vývoj HDP má na mzdy určitý vliv, avšak tato vazba není jednoznačná. U cen potravin nebyla prokázána stabilní závislost na vývoji HDP.

---

# Celkové shrnutí

Analýza ukázala, že:

* mzdy v České republice dlouhodobě rostly, avšak různým tempem napříč odvětvími,
* kupní síla obyvatel se mezi lety 2006 a 2018 zvýšila,
* některé potraviny dlouhodobě zlevňovaly, zatímco jiné výrazně zdražovaly,
* nebyl nalezen rok, kdy by ceny potravin rostly o více než 10 procentních bodů rychleji než mzdy,
* růst HDP nelze považovat za spolehlivý indikátor budoucího vývoje cen potravin ani mezd.
