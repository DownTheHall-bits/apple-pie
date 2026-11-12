# Apple Pie — Agenda zilnică (fostă "Focus Flow")

Aplicație single-page (`index.html`), fără build step, găzduită pe GitHub Pages.
Ține task-uri generale și pe client, cu patru moduri de recurență:

- **Daily Recurrent** — apare în fiecare zi
- **Specific Days of Week** — apare doar în zilele bifate
- **Monthly Plan (X times/month)** — definești de câte ori pe lună (ex. 4 articole
  SEO/lună pentru un client), iar aplicația distribuie automat acele apariții
  uniform pe parcursul lunii curente și le arată în ziua potrivită, fără nicio
  intervenție manuală zi de zi. Progresul lunar ("3/4 luna asta") e vizibil pe
  cardul fiecărui client.
- **One-Time Task** / **In the Future** — task unic, imediat sau la o dată fixă

Datele se salvează local (`localStorage`) și, dacă e configurat `API_URL` cu un
Google Apps Script, se sincronizează și într-un Google Sheet, ca să le ai pe
orice dispozitiv.

## Securitate — de făcut o singură dată pe scriptul Apps Script

Acest repo e **public**, deci `API_URL` din `index.html` e vizibil oricui. Codul
trimite deja o cheie (`SYNC_KEY` în `index.html`) ca parametru `key`, dar
scriptul Apps Script trebuie să o verifice, altfel link-ul singur ar fi
suficient ca oricine să citească/suprascrie datele.

1. Deschide scriptul din Apps Script (script.google.com → proiectul folosit de
   acest `API_URL`).
2. Chiar la începutul funcțiilor `doGet(e)` și `doPost(e)`, adaugă:

```js
const SECRET_KEY = "3spkKNdZdeQstmnwgTWmL6FzXC48Lc3J"; // trebuie identică cu SYNC_KEY din index.html

function doGet(e) {
  if (!e || !e.parameter || e.parameter.key !== SECRET_KEY) {
    return ContentService.createTextOutput(JSON.stringify({ error: "unauthorized" }))
      .setMimeType(ContentService.MimeType.JSON);
  }
  // ... restul codului tău existent din doGet rămâne neschimbat mai jos
}

function doPost(e) {
  var body;
  try { body = JSON.parse(e.postData.contents); } catch (err) { body = {}; }
  if (!body || body.key !== SECRET_KEY) {
    return ContentService.createTextOutput(JSON.stringify({ error: "unauthorized" }));
  }
  // ... restul codului tău existent din doPost rămâne neschimbat mai jos,
  // dar folosește `body` (deja parsat mai sus) în loc să mai faci
  // JSON.parse(e.postData.contents) o a doua oară.
}
```

3. **Deploy → Manage deployments → editează deployment-ul activ → New version → Deploy.**
   (Nu crea un deployment nou — editează-l pe cel existent, ca `API_URL` să rămână
   aceeași.)
4. Gata — după acest pas, cineva care doar vede `API_URL` în repo-ul public nu
   mai poate citi sau modifica datele fără `SECRET_KEY`.

Nu distribui `SECRET_KEY`/`SYNC_KEY` în altă parte publică.
