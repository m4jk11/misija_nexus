# Misija Nexus – Analitički pipeline i geoprostorna analiza

## A. Izvršni sažetak (Executive Summary)

Ovaj projekt fokusiran je na analizu geoprostornih i kemijskih podataka prikupljenih iz kratera Jezero na Marsu. Ulazni podaci sastoje se od dvije CSV datoteke koje sadrže informacije o lokacijama uzoraka i njihovim kemijskim svojstvima. Cilj projekta je identificirati validne lokacije za kretanje autonomnog robota te generirati automatizirani navigacijski nalog u JSON formatu.

---

## B. Metodologija obrade podataka (Data Wrangling)

Podaci su učitani iz dvije odvojene CSV datoteke i spojeni pomoću zajedničkog identifikatora `ID_Uzorka`. Nakon spajanja, primijenjeno je filtriranje podataka korištenjem booleanskih uvjeta.

Uklonjene su vrijednosti koje predstavljaju senzorski šum:

* temperature izvan raspona (-80°C do 20°C)
* pH vrijednosti izvan intervala (6–8)
* uzorci s premalim udjelom vode

Ovakav pristup osigurava da se u daljnjoj analizi koriste samo fizički mogući i znanstveno relevantni podaci.

---

## C. Geoprostorna analiza i vizualizacija

### Korelacija parametara

![Korelacija](assets/graf1.png)

Graf prikazuje odnos između temperature i koncentracije metana. Boja (hue) predstavlja pH vrijednost, dok veličina točaka prikazuje udio vode.

---

### Toplinska karta dubine

![Toplinska karta](assets/graf2.png)

Vizualizacija prikazuje raspodjelu dubine uzoraka i identificira zone s potencijalno značajnim geološkim karakteristikama.

---

### Satelitska mapa (extent mapiranje)

![Mapa](assets/mapa.png)

Za prikaz geoprostornih podataka korištena je satelitska slika uz primjenu parametra `extent`, koji definira granice prikaza na temelju minimalnih i maksimalnih GPS koordinata.

Time su analitički podaci precizno poravnati s realnim geografskim kontekstom, što omogućuje pouzdanu navigaciju robota.

---

## D. Komunikacijski protokol (JSON Uplink)

Navigacijski nalozi generiraju se automatski iteracijom kroz filtrirane podatke.

Primjer JSON strukture:

```json
{
  "id": 101,
  "akcija": "MOVE",
  "koordinate": {
    "lat": -18.4,
    "lon": 77.5
  }
}
```

Podaci se generiraju pomoću petlje kroz DataFrame, čime se izbjegava hardkodiranje i omogućuje dinamičko prilagođavanje rezultata.

---

## E. Inženjerski dnevnik (Troubleshooting Log)

**Problem 1: Neuspješno učitavanje CSV datoteka**

* Uzrok: pogrešan separator (`;` umjesto `,`)
* Rješenje: korištenje `delimiter=';'` u funkciji `read_csv()`

**Problem 2: Rušenje skripte zbog tipova podataka**

* Uzrok: numeričke vrijednosti učitane kao string
* Rješenje: konverzija pomoću `astype(float)`

**Problem 3: Neispravno poravnanje mape**

* Uzrok: pogrešno definirane granice extent parametra
* Rješenje: korištenje min/max koordinata iz dataset-a

---

## Zaključak

Projekt demonstrira cjelovit analitički pipeline: od obrade podataka, preko vizualizacije, do generiranja automatiziranih navigacijskih naredbi. Struktura repozitorija i dokumentacija omogućuju jednostavno razumijevanje i daljnju nadogradnju sustava.
