# Uvjetovana primopredaja vođena umjetnom inteligencijom (AI-CHO) u LEO NTN mrežama

Izvorni kod simulacije iz diplomskog rada:

Simulacija vrednuje uvjetovanu primopredaju u kojoj odluku donosi model strojnog učenja (AI-CHO), uz konvencionalni okidač A3 kao rezervu. Pristup se uspoređuje s primopredajom temeljenom na mjerenjima (A3) i uvjetovanom primopredajom s lokacijskim okidačem (D1-CHO). Model je LEO konstelacija Walker-delta 53°: 880/40/1 na visini od 600 km iznad ruralnog područja u Lici (polumjer 30 km, 20 korisnika). Kanal je modeliran prema 3GPP TR 38.811.

Detaljan opis modela nalazi se u poglavlju 5 rada, rezultati u poglavlju 6, a popis parametara i inačica u Prilogu A.

## Sadržaj repozitorija

| Datoteka | Sadržaj |
|---|---|
| `NTN_AI_CHO_simulacija.ipynb` | Cjelokupan izvorni kod: konstelacija, kanal, metode primopredaje, model strojnog učenja, vrednovanje, tablice i slike |
| `requirements.txt` | Inačice Pythona i biblioteka s kojima su dobiveni rezultati u radu |
| `rezultati/parametri.json` | Svi konfiguracijski parametri korišteni pri pokretanju (Prilog A, Tablica A.3) |
| `rezultati/verzije.txt` | Inačice Pythona i biblioteka zapisane pri pokretanju (Prilog A, Tablica A.2) |
| `rezultati/tablica_*.csv` | Tablice iz poglavlja 5 i 6 |
| `rezultati/slika_*.png` | Slike iz poglavlja 5 i 6 |

Datoteke u mapi `rezultati/` dobivene su pokretanjem bilježnice s kojim su dobiveni rezultati u radu, pa se mogu izravno usporediti s novim pokretanjem.

## Pokretanje

### Google Colab (preporučeno)

1. Otvoriti [Google Colab](https://colab.research.google.com/) i odabrati **File → Open notebook → GitHub**, pa upisati adresu ovog repozitorija (ili preuzeti bilježnicu i odabrati **File → Upload notebook**).
2. Pokrenuti sve ćelije redom: **Runtime → Run all**.

Dodatna instalacija nije potrebna jer su sve biblioteke u Colabu već dostupne. Cijeli postupak traje oko 15 minuta.

### Lokalno

Potreban je Python 3.10 ili noviji.

```bash
git clone https://github.com/[korisnik]/[repozitorij].git
cd [repozitorij]
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook NTN_AI_CHO_simulacija.ipynb
```

Zatim pokrenuti sve ćelije redom.

## Što bilježnica radi

1. Generira razvojne scenarije 101 i 102 te testne scenarije 201–210 (svaki traje 1200 s uz korak od 0,1 s).
2. Na scenariju 101 podešava parametre metoda A3 i D1-CHO.
3. Odabire trošak primopredaje C treniranjem na scenariju 101 i vrednovanjem na scenariju 102. Odabrane su dvije radne točke: AI-CHO (C = 20) i AI-CHO-U (C = 5).
4. Izračunava referentne odluke Viterbijevim algoritmom i trenira modele (`HistGradientBoostingClassifier`) na scenarijima 101 i 102.
5. Vrednuje sve metode (A3, D1-CHO, Rezerva, AI-CHO, AI-CHO-U) na testnim scenarijima 201–210 i provodi Wilcoxonov test.
6. Sprema tablice, slike, `parametri.json` i `verzije.txt` u mapu `rezultati/` i arhivu `rezultati.zip`. U Colabu se arhiva automatski preuzima.

## Promjena scenarija

Svi parametri nalaze se u rječniku `CFG` na početku bilježnice (visina i oblik konstelacije, područje, broj i kretanje korisnika, parametri kanala, pragovi veze, parametri metoda). Za drugačiji scenarij dovoljno je izmijeniti vrijednosti u `CFG`; ostatak koda nije potrebno mijenjati. Parametri svakog pokretanja spremaju se u `rezultati/parametri.json`.

## Ponovljivost

Svi slučajni procesi (početni položaj konstelacije, raspored i kretanje korisnika, kanal) određeni su sjemenima scenarija, pa ponovno pokretanje s istim parametrima daje iste scenarije. S drugim inačicama biblioteka, osobito scikit-learna, rezultati se mogu neznatno razlikovati. Za iste rezultate kao u radu treba koristiti inačice iz `requirements.txt`.

## Ograničenja

Rezultati su dobiveni simulacijom jedne konfiguracije konstelacije i jednog područja, uz statistički model kanala i pojednostavljen model propusnosti. Apsolutne vrijednosti propusnosti nisu prognoza brzina u stvarnoj mreži; smislene su razlike između metoda u istim simuliranim uvjetima. Detaljnije u odjeljku 6.4 rada.
