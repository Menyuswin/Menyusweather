# Időjárás-ügyelet

Egyoldalas **időjárás-figyelő**: nem csak a hőmérsékletet mutatja, hanem azt is,
mikor kell tenni valamit — széllökés, csapadék, hőség, UV és tűzveszély szerint.

Publikált változat: <https://claude.ai/code/artifact/7a14dd17-1bd1-4c7a-aead-a503707ba70f>

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

A lap az artifact-futtató `window.claude` felületén keresztül hívja a **CST**
connector `weather_context` eszközét (mögötte Open-Meteo), a megnyitó
felhasználó nevében. Ezért:

- **Élő adat csak a claude.ai artifact-nézetében van.** Helyben megnyitva vagy
  máshova beágyazva nincs connector-hozzáférés — a lap ilyenkor kiírja, és a
  panel felépítése, a küszöbök olvashatók maradnak.
- Az első hívásnál a claude.ai engedélyt kér a connector használatára.
- A friss adat 15 percenként magától megérkezik (`watchTool` + `refetchInterval`).

## Fájlok

    index.html    a teljes alkalmazás — stílus, jelölés és logika egy fájlban
    README.md     ez a leírás

Nincs build-lépés és nincs függősége: az egyetlen külső erőforrás a Google Fonts
(IBM Plex Sans / Sans Condensed / Mono).

## Publikálás

Claude Code-ból, ugyanarra az URL-re:

    Artifact(file_path="index.html", url="https://claude.ai/code/artifact/7a14dd17-1bd1-4c7a-aead-a503707ba70f")

Az MCP-manifest a publikált verzióban van eltárolva (`CST` / `weather_context`);
ha újat kell deklarálni, a `capabilities` paraméterrel megy.

## Megjegyzés az adatról

A tűzveszélyességi index a connector egyszerűsített számítása (hőmérséklet, szél,
szárazság, csapadékmentes napok) — tájékoztató érték, nem hatósági fokozat.
