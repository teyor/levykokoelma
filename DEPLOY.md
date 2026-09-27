# Levykokoelma – julkaisu GitHub Pagesiin

Live: https://teyor.github.io/levykokoelma/
Repo: https://github.com/teyor/levykokoelma (julkinen, Pages = main-haara, juuri)

## Rakenne
- `index.html` – sovellus + sisäänrakennettu data (`<script id="seed-data">`), kuvat viitataan polulla `images/xxx.jpg`
- `images/` – kansikuvat erillisinä tiedostoina (`artisti-albumi-1.jpg`, `-2.jpg`, ...)
- `data-backup.json` – sama data kuin index.html:ssä (kuvapolut, ei base64)

## Levyn lisääminen sisäänrakennettuun dataan
1. Kopioi kuvat kansioon `images/` (esim. `artisti-albumi-1.jpg`, max ~1200 px, JPEG).
2. Lisää tietue `seed-data`-JSONiin index.html:ssä (ja `data-backup.json`iin):
   `{"artist":"..","album":"..","purchaseDate":"","price":"","catalogNumber":"","country":"","condition":"","marketPrice":"","notes":"","dateAdded":"27.9.2026","images":["images/artisti-albumi-1.jpg"]}`
3. Nosta `const DATA_VERSION = N;` yhdellä – vain silloin selaimet yhdistävät uudet levyt/tyhjät kentät omaan dataansa
   (selaimen muutokset voittavat, tyhjät/puuttuvat kentät täytetään sisäänrakennetusta datasta).
4. Tarkista: `sed -n '/<script>/,/<\/script>/p' index.html | sed '1d;$d' > /tmp/app.js && node --check /tmp/app.js`
5. Julkaise:
   ```
   cd /workspace/levykokoelma-repo
   git add -A
   git commit -m "Lisätty levy X, DATA_VERSION N"
   git push
   ```
   Pages päivittyy yleensä 1–2 minuutissa (`gh api repos/teyor/levykokoelma/pages/builds/latest`).

## Selaimessa lisätyt levyt / kuvat
- Tallentuvat vain kyseisen selaimen localStorageen (avaimet `lpRecords`, `lpRecordsVersion`).
- Selaimessa lisätyt kuvat pienennetään (max 1000 px, JPEG) ja tallennetaan data-URLeina.
- Varmuuskopio: "Vie tiedot (JSON)". Palautus: "Lataa tiedot" (myös vanhat base64-varmuuskopiot toimivat;
  sisäänrakennetut kuvat muunnetaan automaattisesti `images/`-poluiksi).
- Vanhat v16–v22 `seedimg:`-viittaukset muunnetaan automaattisesti uusiin polkuihin.

Netlifyyn EI enää julkaista.
