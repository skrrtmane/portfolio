# cubadunn.com — Portfolio

Statische Portfolio-Website (eine HTML-Datei pro Seite, kein Build-Schritt).

**Zwei getrennte Seiten, zwei Repos:**

| Domain | Inhalt | Repo |
|---|---|---|
| `cubadunn.com` | dieses Portfolio | dieses Repo |
| `cubas.work` | Gameunication-Projektseite | eigenes Repo (siehe unten) |

Das Portfolio verlinkt von der Karte „gameunication" nach `https://cubas.work`.

## Ordnerstruktur

```
Portfolio-Website/
├── index.html          Hauptseite
├── impressum.html      Impressum
├── datenschutz.html    Datenschutzerklärung
├── CNAME               Custom Domain für GitHub Pages (cubadunn.com)
├── .nojekyll           schaltet Jekyll-Verarbeitung bei Pages ab
├── .gitignore          hält .DS_Store und *.orig.* aus dem Repo
├── img/                Bilder und Clips (siehe unten)
└── fonts/              Courier Prime als woff2 (siehe unten)
```

## Bilder und Video-Clips

Einfach Dateien mit genau diesen Namen nach `img/` legen — die Platzhalter
auf der Seite erscheinen dann automatisch (fehlende Dateien werden komplett
ausgeblendet, es bleibt kein leerer Rahmen):

| Datei | Typ | erscheint bei |
|---|---|---|
| `img/portrait.jpg` | Bild | [04] Über mich |
| `img/ityt_station.mp4` | **Clip** | [02] It takes you two (Galerie links) |
| `img/ityt_reflexion.mp4` | **Clip** | [02] It takes you two (Galerie rechts) |
| `img/kinderfotopreis.jpg` | Bild | [03] Praxis (rechte Spalte) |

Bilder: Querformat ca. 1200×800px (Porträt 800×1000px), JPG mit ~80% Qualität.

### Die zwei Video-Clips

Die beiden Clips laufen stumm in Dauerschleife, ohne Bedienelemente, und
starten erst, wenn sie ins Bild scrollen. Ist im System „Bewegung reduzieren"
aktiv, wird stattdessen ein Play-Button gezeigt.

**.mov bitte in .mp4 umwandeln.** QuickTime-Dateien spielen nicht in allen
Browsern (Firefox z. B. gar nicht). Die Seite versucht der Reihe nach
`.mp4` → `.webm` → `.mov`, aber verlassen sollte man sich nur auf mp4.

Mit ffmpeg (`brew install ffmpeg`), Ton wird dabei gleich entfernt:

```bash
cd ~/Documents/Portfolio-Website/img
ffmpeg -i clip.mov -an -vcodec libx264 -crf 26 -preset slow \
  -pix_fmt yuv420p -movflags +faststart -vf "scale=1280:-2" ityt_station.mp4
```

Ohne ffmpeg tut es auch CloudConvert o. ä. — Einstellungen: **MP4 / H.264**,
Breite 1280px, Ton entfernen.

Faustregeln: höchstens ~5 MB pro Clip (sonst ruckelt der Seitenaufbau auf
Mobilfunk), 5–15 Sekunden reichen für eine Schleife, und der Schnittpunkt
sollte halbwegs unauffällig sein, weil der Clip hart neu beginnt.

Optional: Legst du zusätzlich `img/ityt_station.jpg` dazu, wird das als
Standbild angezeigt, solange der Clip noch lädt.

**Hochkant-Clips:** Beim jeweiligen `<figure class="shot">` einfach
`class="shot tall"` schreiben, dann steht der Rahmen im 9:16-Format statt 3:2.

**Weitere Slots auf Video umstellen:** Den `<img …>` durch denselben
`<video class="clip" autoplay muted loop playsinline …>`-Block ersetzen —
um alles andere kümmert sich das Skript am Seitenende.

Auf Aufnahmen mit fremden Personen (v. a. Kindern!) nur veröffentlichen, was
ausdrücklich freigegeben ist — bei Videos gilt das erst recht.

## Schrift (einmalig)

Courier Prime ist lokal eingebunden (DSGVO-sauber, kein Abruf von
Google-Servern). Bis die Dateien da sind, greift automatisch Courier New.

1. https://gwfh.mranftl.com/fonts/courier-prime?subsets=latin öffnen
2. Styles auswählen: **regular**, **italic**, **700**
3. ZIP herunterladen und die drei `.woff2`-Dateien nach `fonts/` entpacken.
   Erwartete Dateinamen (ggf. umbenennen):
   - `courier-prime-v9-latin-regular.woff2`
   - `courier-prime-v9-latin-italic.woff2`
   - `courier-prime-v9-latin-700.woff2`

## Auf GitHub Pages veröffentlichen

1. Neues Repository auf GitHub anlegen (z. B. `portfolio`), **Public**
2. Diesen Ordner hochladen:
   ```bash
   cd ~/Documents/Portfolio-Website
   git init
   git add .
   git commit -m "Portfolio"
   git branch -M main
   git remote add origin https://github.com/DEIN-USERNAME/portfolio.git
   git push -u origin main
   ```
3. Im Repo: **Settings → Pages** → Source: „Deploy from a branch",
   Branch: `main`, Ordner: `/ (root)` → Save
4. Die Seite liegt dann erst unter `https://DEIN-USERNAME.github.io/portfolio/`

## Domain cubadunn.com verbinden

1. Beim Domain-Anbieter im DNS setzen:
   - **A-Records** für `cubadunn.com` (Apex) auf alle vier:
     `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - **CNAME-Record** für `www` → `DEIN-USERNAME.github.io`
2. In **Settings → Pages → Custom domain**: `cubadunn.com` eintragen → Save
   (die Datei `CNAME` im Repo hält das fest — Inhalt muss identisch sein)
3. Sobald der DNS-Check grün ist: Haken bei **Enforce HTTPS** setzen
   (kann ein paar Stunden dauern, bis das Zertifikat da ist)

## Gameunication auf cubas.work (zweites Repo)

Genau dasselbe Vorgehen, nur mit eigenem Repo:

1. Neuen Ordner anlegen, die Gameunication-HTML darin als `index.html` ablegen
2. Datei `CNAME` mit einer Zeile: `cubas.work`
3. Leere Datei `.nojekyll` daneben
4. Als eigenes Repo (z. B. `gameunication`) pushen, Pages aktivieren
5. DNS für `cubas.work`: dieselben vier A-Records wie oben,
   `www` als CNAME auf `DEIN-USERNAME.github.io`

Beide Repos können problemlos unter demselben GitHub-Account laufen.

## Noch offen

- [ ] Clips zusätzlich als **.mp4** exportieren (nur .webm = kein Video auf älterem iOS)
- [ ] `img/portrait.jpg` und `img/kinderfotopreis.jpg` ergänzen
- [ ] Fonts nach `fonts/` legen
- [ ] Impressum: zweiter Kontaktweg neben der E-Mail (Telefon oder Kontaktformular)
- [ ] Impressum prüfen: Adresse aktuell? Bei Umzug sofort anpassen
