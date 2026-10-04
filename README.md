# Songbattle

Magyar nyelvű zenei párbajoldal közös, élő szavazással és privát zeneszobákkal. A GitHub Pages szolgáltatja a weboldalt; a Supabase tárolja a párbajokat és a szobák adatait.

## Élő zeneszobák

- A host létrehoz egy szobát, és megosztja a hatkarakteres belépőkódot.
- A résztvevők névvel csatlakoznak, majd Spotify- vagy YouTube-linkeket küldenek be.
- A host látja a szoba teljes zenelistáját és résztvevőit, törölhet zenét, eltávolíthat résztvevőt vagy bezárhatja a szobát.
- A csatlakozó kizárólag a saját beküldéseit látja; a többi résztvevő zenéi és nevei nem kerülnek vissza a böngészőjébe.
- A szobalista három másodpercenként frissül. A host jogosultságát adatbázisban tárolt véletlenszerű bearer token védi; a token csak a host saját böngészőjében tárolódik.
- A kiléptetett résztvevő az adott azonosítóval nem tud újra csatlakozni. Új böngészőazonosítóval megkerülhető; fiók vagy CAPTCHA nélkül a teljes személyazonosság nem ellenőrizhető.

A szoba beküldése Spotify- és YouTube-linket fogad el, de a számok lejátszását nem építi be: a link külön lapon nyílik meg. A linkek nem jelennek meg más résztvevőknek.

## Supabase-beállítás

Ez a projekt már a `mylven/songbattle` GitHub-repozitóriumhoz és a hozzá tartozó Supabase-projekthez van bekötve. Ha másik Supabase-projektet szeretnél használni, a `supabase-config.js` fájlban állítsd be annak URL-jét és publikus publishable/anon kulcsát.

A jelenlegi Supabase-projekthez a szobák adatbázisát így telepítsd:

1. Nyisd meg a Supabase-projekt **SQL Editor** oldalát.
2. Futtasd le a `supabase-schema.sql` teljes tartalmát. A sémát újrafuttathatóra terveztük; a szobafunkciókat és jogosultságokat hozzáadja a meglévő szavazási adatbázishoz.
3. Ellenőrizd a weboldalon, hogy a „Szoba létrehozása” gombbal létrehozott szobakódot egy másik böngészőből meg tudod nyitni.

A böngészőben kizárólag a projekt URL-je és a publishable/anon kulcs használható. **`service_role` vagy `secret` kulcsot soha ne tegyél a weboldal kódjába.** A szobatáblák közvetlen nyilvános olvasása tiltott; az alkalmazás jogosultság-ellenőrzött adatbázis-függvényeken keresztül éri el őket.

## GitHub Pages

Az oldal a `main` ágra feltöltés után automatikusan települ a GitHub Actions segítségével. A GitHub Pages forrása **GitHub Actions** legyen. A publikus oldal címe: <https://mylven.github.io/songbattle/>.

Helyi előnézethez indíts webszervert a projekt mappájában (például `py -m http.server 8000`), majd nyisd meg a `http://localhost:8000` címet.
