# Időjárás-ügyelet

Egyoldalas **időjárás-figyelő**: nem csak a hőmérsékletet mutatja, hanem azt is,
mikor kell tenni valamit — széllökés, csapadék, hőség, UV és tűzveszély szerint.

Teljesen önálló statikus oldal: nincs build-lépés, nincs backend, nincs API-kulcs.
A böngésző közvetlenül az [Open-Meteo](https://open-meteo.com) nyilvános API-ját
hívja, ezért helyben (localhoston), GitHub Pages-en vagy bármilyen statikus
tárhelyen ugyanúgy működik, mint korábban a Claude-artifact nézetben.

## Mit tud

- **Figyelmeztetés-sáv** a lap tetején, küszöbalapú riasztásokkal (lásd lentebb).
- **Aktuális mérés**: hőmérséklet, hőérzet, páratartalom, csapadék, szél, széllökés.
- **Széliránytű + Beaufort-létra**: a tartós szél (▲) és a lökés (◆) egy skálán.
- **Tűzveszély-index** 1–5, komponensekre bontva (hőmérséklet, szél, szárazság,
  csapadékmentes napok).
- **7 napos hőmérséklet-sávok** közös skálán, mellette csapadék, lökésmaximum, UV;
  hover-tooltip és táblázatos nézet.
- **12 órás szélcsík**: oszlop a lökés, belső sáv a tartós szél, alatta a szélirány.
- **Helykeresés és figyelt helyek**: névre keres, a többértelmű találatoknál
  (pl. Budapest/Georgia) felkínálja az alternatívákat; a mentett helyek a
  böngésző `localStorage`-ában maradnak.

## Riasztási küszöbök

| Jelenség | Figyelmeztetés | Riasztás |
|---|---|---|
| Széllökés | 6 Beaufort | 8 Beaufort |
| Napi csapadék | 8 mm | 15 mm |
| Napi maximum | 30 °C | 34 °C |
| Fagy (napi maximum) | 0 °C | −5 °C |
| UV-index | 6 | 8 |
| Tűzveszély | 3/5 | 4/5 |

## Hogyan fut

A lap `fetch()`-csel közvetlenül két Open-Meteo végpontot hív, kliens oldalon:

- **Geokódolás** (`geocoding-api.open-meteo.com`) — helynévből koordináta,
  többértelmű találatoknál (pl. Budapest/Georgia) felkínálja az alternatívákat.
- **Előrejelzés** (`api.open-meteo.com/v1/forecast`) — aktuális mérés, 7 napos
  napi bontás, 12 órás óránkénti bontás, plusz 14 nap visszamenőleg a
  csapadékmentes napok sorozatának számításához (`past_days`).

Nincs szerver, nincs fiók, nincs API-kulcs — az Open-Meteo ingyenes, nem
kereskedelmi használatra kulcs nélkül is elérhető, és CORS-fejléccel engedi a
böngészőből induló hívásokat. A friss adat 15 percenként magától frissül.

A tűzveszély-index ennek az oldalnak a saját, egyszerűsített becslése
(hőmérséklet, szél, páratartalom, csapadékmentes napok súlyozott átlaga) —
tájékoztató érték, nem hatósági fokozat.

## Futtatás helyben

Egyszerűen megnyitható közvetlenül a böngészőben (dupla kattintás az
`index.html`-en), de a megbízhatóbb út egy helyi statikus szerver:

    python3 -m http.server 8000
    # majd: http://localhost:8000

Ugyanígy működik GitHub Pages-ről vagy bármilyen statikus tárhelyről — a
`main` branch gyökeréből kiszolgálva.

## Fájlok

    index.html    a teljes alkalmazás — stílus, jelölés és logika egy fájlban
    README.md     ez a leírás

Nincs build-lépés és nincs függősége: az egyetlen külső erőforrás a Google Fonts
(IBM Plex Sans / Sans Condensed / Mono) és az Open-Meteo API.
