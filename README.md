# Misija Nexus – Tehnička dokumentacija projekta

## Izvršni sažetak (Executive Summary)

Projekt *Misija Nexus* bavi se analizom podataka prikupljenih u krateru Jezero na Marsu s ciljem pronalaska najboljih lokacija za bušenje. Sustav obrađuje telemetrijske podatke sa senzora i pokušava prepoznati uvjete koji mogu ukazivati na prisutnost vode ili organskih tvari.

Podaci dolaze iz dvije CSV datoteke: jedna sadrži geoprostorne koordinate, a druga senzorska mjerenja. Korištenjem Python alata (Pandas, Matplotlib i Seaborn) napravljen je analitički proces koji:

* spaja podatke u jednu cjelinu
* uklanja netočne i nelogične vrijednosti
* prikazuje rezultate pomoću grafova
* generira JSON naloge za robota

Na kraju se dobije lista lokacija koje zadovoljavaju uvjete za daljnju analizu.

---

## Arhitektura repozitorija

Projekt je organiziran u nekoliko mapa radi lakšeg snalaženja:

* `data/`
  Sadrži ulazne CSV datoteke (`mars_lokacije.csv`, `mars_uzorci.csv`)

* `src/`
  Python skripte za obradu i analizu podataka

* `assets/`
  Grafovi i slike dobivene analizom

* `README.md`
  Glavna dokumentacija projekta

Ovakva podjela olakšava rad i čini projekt preglednijim.

---

## Metodologija obrade podataka (Data Wrangling)

### Učitavanje podataka

Podaci su učitani pomoću Pandas biblioteke. Prilikom učitavanja bilo je važno postaviti:

* separator `;`
* decimalni zapis `,`

Bez toga bi podaci bili krivo učitani.

---

### Spajanje podataka

Dvije tablice spojene su preko stupca `ID_Uzorka`, čime je dobiven jedan zajednički DataFrame koji sadrži sve potrebne informacije.

---

### Uklanjanje anomalija

Zbog mogućih grešaka senzora, bilo je potrebno ukloniti nelogične vrijednosti. Korišteni su sljedeći uvjeti:

* temperatura mora biti između -100 i 40
* pH između 0 i 14
* udio vode između 0 i 100

Podaci koji ne zadovoljavaju ove uvjete izdvojeni su kao anomalije.

---

### Rezultati obrade

Dobivene su dvije datoteke:

* `cisti_podaci.csv` – podaci spremni za analizu
* `anomalije.csv` – podaci s greškama

---

## Geoprostorna analiza i vizualizacija

Vizualizacije su korištene za lakše razumijevanje podataka.

---

### Graf 1 – Temperatura i voda

![Graf 1](assets/graf1_temperatura_voda(1).png)

Graf prikazuje odnos temperature i količine vode. Boja označava prisutnost metana.

---

### Graf 2 – Dubina bušenja

![Graf 2](assets/graf2_karta_dubine(1).png)

Prikazuje raspodjelu dubine uzoraka po lokacijama.

---

### Graf 3 – Metan

![Graf 3](assets/graf3_metan(1).png)

Crvena boja označava prisutnost metana, a plava odsutnost.

---

### Graf 4 – Kandidati za bušenje

![Graf 4](assets/karta_kandidata(1).png)

Prikazane su lokacije koje zadovoljavaju uvjete za bušenje.

---

### Graf 5 – Satelitska mapa

![Graf 5](assets/jezero_mission_map.jpg)

Za prikaz je korišteno **extent mapiranje**, kojim se slika usklađuje s GPS koordinatama.

Extent definira granice prikaza:

```
[X_min, X_max, Y_min, Y_max]
```

Na taj način podaci su pravilno postavljeni na kartu.

---

## Identifikacija kandidata

Kandidati su određeni pomoću uvjeta:

* metan mora biti pozitivan
* organske molekule moraju biti prisutne

---

## Komunikacijski protokol (JSON Uplink)

Nakon obrade podataka generira se JSON koji robot koristi za kretanje.

Primjer:

```json
{
  "kandidati": [
    {
      "ID_Uzorka": 12345,
      "GPS_LAT": 12.3456,
      "GPS_LONG": 98.7654,
      "akcije": [
        { "tip": "NAVIGACIJA" },
        { "tip": "SONDIRANJE" },
        { "tip": "SLANJE_PODATAKA" }
      ]
    }
  ]
}
```

Podaci se generiraju automatski pomoću petlje kroz DataFrame.

---

## Inženjerski dnevnik (Troubleshooting Log)

### Problem 1: Učitavanje CSV datoteka

Uzrok: pogrešan separator
Rješenje: korištenje `sep=";"`

---

### Problem 2: Spajanje tablica

Uzrok: različiti tipovi podataka
Rješenje: usklađivanje tipova prije spajanja

---

### Problem 3: Graf bez legende

Uzrok: nedostaje label
Rješenje: dodan `plt.legend()`

---

### Problem 4: Pogrešna mapa

Uzrok: krivo postavljen extent
Rješenje: korištenje min i max koordinata

---

## Zaključak

Projekt pokazuje kako se podaci mogu obraditi, analizirati i prikazati na razumljiv način. Dobiveni rezultati mogu se koristiti za donošenje odluka i daljnju analizu.
Projekt pokazuje kako se podaci mogu obraditi, analizirati i prikazati na razumljiv način. Dobiveni rezultati mogu se koristiti za donošenje odluka i daljnju analizu.
