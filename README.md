# Songbattle

Reszponzív, magyar nyelvű zenei párbajoldal. A Supabase tárolja a közös szavazatokat, az eredményeket pedig élőben frissíti minden megnyitott böngészőben. Az oldal GitHub Pagesen fut, a háttérszolgáltatáshoz ingyenes Supabase-projekt használható.

## Beállítás és közzététel

### 1. Hozz létre egy Supabase-projektet

1. Hozz létre egy projektet a [supabase.com](https://supabase.com) oldalon.
2. A projekt **Connect** vagy **Settings → API Keys** oldalán másold ki a projekt URL-jét és a **publishable** kulcsot. A régi projektekben ez az `anon` kulcs.
3. A `supabase-config.js` fájlban cseréld ki a `YOUR_PROJECT_ID` és `YOUR_SUPABASE_PUBLISHABLE_KEY` értékeket a saját adataidra.

A publishable/anon kulcs böngészőben használható, nyilvános kulcs. **Soha ne másolj ide `service_role` vagy `secret` kulcsot**; az adminisztrátori kulcs teljes hozzáférést ad az adatbázishoz. Az adatbázis-hozzáférést a séma Row Level Security szabályai korlátozzák.

### 2. Hozd létre az adatbázist

1. Nyisd meg a Supabase-projekt **SQL Editor** oldalát.
2. Másold be a projektben található [`supabase-schema.sql`](./supabase-schema.sql) teljes tartalmát, majd futtasd le.
3. A séma három mintapárbajt hoz létre, védi a szavazatokat, az ismételt szavazást adatbázis-szinten utasítja el, és bekapcsolja az élő eredményfrissítéshez szükséges Realtime publikációt.

A mintadalok cseréjéhez módosítsd a sorokat a Supabase `songbattle_battles` táblájában. Egy párbaj két dalból áll; az oldal az aktív párbajokat a `position` mező sorrendjében jeleníti meg.

### 3. Tedd közzé GitHub Pagesen

1. Töltsd fel a projekt fájljait a GitHub-repozitóriumod `main` ágára. A `supabase-config.js` a nyilvános publishable kulcsot tartalmazza, nem adminisztrátori titkot.
2. A repó **Settings → Pages → Build and deployment → Source** beállításánál válaszd a **GitHub Actions** lehetőséget.
3. A [`pages.yml`](./.github/workflows/pages.yml) munkafolyamat automatikusan közzéteszi az oldalt minden `main` ágra történő feltöltéskor.
4. Az oldal címét a **Settings → Pages** oldalon találod.

## A szavazás működése és korlátai

- A szavazatok és az összesített eredmények Supabase-ben tárolódnak; az élő eredmény minden látogatónál frissül.
- A böngésző egy véletlenszerű, helyben tárolt azonosítót használ, az adatbázis pedig párbajonként egy szavazatot enged ehhez az azonosítóhoz.
- Bejelentkezés nélküli nyilvános szavazásnál ez nem bizonyítja a látogató személyazonosságát: a böngésző tárhelyének törlésével vagy másik eszköz használatával új szavazóazonosító hozható létre. Nagyobb tétű szavazáshoz fiók, CAPTCHA és szerveroldali visszaélésvédelem szükséges.
- A mintapárbajok és daladatok a saját Supabase-adatbázisodban vannak; a zenelejátszás és a látogatók által beküldött dalok nem részei ennek az alapverziónak.

## Helyi előnézet

GitHub Pagesen HTTPS-en működik. Helyi kipróbáláshoz indíts webszervert a projekt mappájában (például `py -m http.server 8000`), majd nyisd meg a `http://localhost:8000` címet. Az élő szavazáshoz a Supabase-beállítást helyben is meg kell adni.
