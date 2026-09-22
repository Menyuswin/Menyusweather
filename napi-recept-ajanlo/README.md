# Napi recept ajánló

Egyoldalas, statikus alkalmazás: minden nap ajánl egy-egy **reggelit, ebédet
és vacsorát**, hét konyha közül válogatva — olasz, francia, amerikai, lengyel,
délszláv, görög és magyar. Nincs build-lépés, nincs backend, nincs
API-kulcs — ugyanaz a filozófia, mint a repó másik alkalmazásánál
(`../index.html`, Időjárás-ügyelet).

## Hogyan válogat

A mai dátumból (az év hányadik napja) az alkalmazás determinisztikusan
kiválaszt 3 különböző konyhát — egyet reggelire, egyet ebédre, egyet
vacsorára —, úgy, hogy egy hét alatt mind a 7 konyha egyenlő eséllyel
előkerüljön. Ugyanaznap újratöltve ugyanazt az ajánlást mutatja, éjfélkor
változik. A „Másik ötletet ebből a konyhából” gomb ugyanabból a konyhából
kínál egy másik fogást, dátum-váltás nélkül is.

## Adatforrások

Nincs egyetlen ingyenes adatbázis, amely mind a hét konyhát lefedné — ezért
az alkalmazás két réteget kever:

- **Élő lekérdezés — 6 konyha.** Az oldal közvetlenül a böngészőből hívja a
  [TheMealDB](https://www.themealdb.com) ingyenes, kulcs nélkül elérhető
  API-ját (`filter.php`, `lookup.php`) az olasz, francia, amerikai, lengyel,
  görög és délszláv (a TheMealDB-ben horvátként szereplő) receptekhez.
  Mivel a TheMealDB nem bont mindent kifejezetten reggeli/ebéd/vacsora
  kategóriára, reggelire csak akkor jelenik meg dedikált "Breakfast"-címkés
  recept, ha van ilyen az adott konyhához — ha nincs, egy általános fogás
  kerül a helyére.
- **Kézzel válogatott adat — magyar konyha.** Erre nincs ingyenes, élőben
  lekérdezhető API, ezért ezt az oldal 9 kézzel összeállított, valódi magyar
  recepttel (3-3 reggelire, ebédre, vacsorára) pótolja, a Wikibooks Cookbook
  (CC BY-SA 3.0) és a Magyar Elektronikus Könyvtár közkincs szakácskönyveinek
  ihletésével.

Kiegészítő háttérforrások a válogatáshoz:

1. [TheMealDB](https://www.themealdb.com) — ingyenes recept-API
2. [Wikibooks Cookbook](https://en.wikibooks.org/wiki/Cookbook) — CC BY-SA 3.0
3. Pellegrino Artusi: *La Scienza in cucina e l'Arte di mangiar bene* (1891) — közkincs
4. Auguste Escoffier: *Le Guide Culinaire* (1907) — közkincs
5. Lucyna Ćwierczakiewiczowa: *365 obiadów za pięć złotych* (1858) — közkincs

## Korlátok, amiket érdemes tudni

- A TheMealDB-ből érkező hozzávalók és elkészítési leírás **angol nyelvű**
  (mivel az API angolul szolgáltatja az adatot) — csak a magyar recept-készlet
  van magyarul megírva.
- Ha a TheMealDB nem érhető el (hálózati hiba, vagy a szolgáltatás
  leállt), az érintett kártya hibaüzenetet és "Újra" gombot mutat — a magyar
  recepteket ez nem érinti, azok mindig helyben, azonnal betöltődnek.
- A délszláv konyhát a TheMealDB-ben elérhető horvát recept-készlet
  képviseli, mert nincs ingyenes API kifejezetten "délszláv" bontásra.

## Futtatás helyben

    python3 -m http.server 8000
    # majd: http://localhost:8000/napi-recept-ajanlo/

Dupla kattintással is megnyitható közvetlenül a böngészőben, illetve
ugyanígy működik GitHub Pages-ről vagy bármilyen statikus tárhelyről.

## Fájlok

    index.html    a teljes alkalmazás — stílus, jelölés és logika egy fájlban
    README.md     ez a leírás
