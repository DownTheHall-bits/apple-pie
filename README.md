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
