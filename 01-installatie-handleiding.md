
# Installatie­handleiding: R + RStudio + Rtools

**Organisatie:** De Haagse Hogeschool  
**Auteur:** Monika Vaheoja  
**Jaar:** 2026 
**Doelgroep:** Nieuwe medewerker HHS die R gaat leren  
**Tijdsindicatie:** 30–60 minuten als alles meewerkt, langer als IT betrokken moet worden  
**Belangrijk:** Lees eerst sectie 1 (IT-vragen) vóórdat je begint te installeren

---

## 1. Vragen voor de HHS ICT-servicedesk

Stuur deze vragen vóóraf, zodat je niet halverwege vastloopt. De meeste hiervan zijn met één ticket op te lossen.

### 1.1 Rechten en installatie

1. **Heb ik admin-rechten op mijn werklaptop om software te installeren, of moet alles via Software Center / een ICT-ticket?**
2. **Staan R, RStudio Desktop en Rtools al in het Software Center / de softwarecatalogus?** Zo ja: welke versies?
3. **Als ze er niet in staan: kunnen ze toegevoegd worden, of krijg ik eenmalig admin-rechten om ze zelf te installeren?**
4. **Mag ik installeren in een custom pad** (bijv. `C:\R\` in plaats van `C:\Program Files\R\`) **om problemen met spaties in paden te voorkomen?**

### 1.2 Netwerk en proxy
De meeste medewerkers kunnen sectie 1 overslaan tenzij zij weten dat hun laptop beheerd wordt door ICT.
5. **Zit er een proxy of firewall tussen mijn laptop en internet die package-downloads van CRAN kan blokkeren?** (CRAN-mirrors zoals `cloud.r-project.org` en GitHub moeten bereikbaar zijn.)
6. **Zo ja: wat zijn de proxy-instellingen** (server, poort, authenticatie) **en hoe stel ik die in R in via `.Renviron`?**
7. **Wordt SSL-inspectie toegepast?** Dit kan certificaat-fouten geven bij `install.packages()`. Zo ja: is er een interne CA-certificaat­bundel die ik moet gebruiken?

### 1.3 Opslag en bestandslocaties

8. **Waar mag ik mijn R-projectmap aanmaken die NIET door OneDrive/SharePoint wordt gesynchroniseerd?** (Synchronisatie geeft file-lock conflicten met RStudio.)
9. **Is er een netwerkschijf of lokale schijf die back-up heeft maar geen real-time sync?**
10. **Hoeveel ruimte mag mijn lokale gebruikersmap innemen?** (Een R-installatie + tidyverse + projecten = al snel 2–4 GB.)

### 1.4 Specifiek voor Rtools (Windows)

11. **Rtools installeert een mini-compiler (gcc, make) onder `C:\rtools44\`. Geeft de endpoint-security software (Defender, CrowdStrike, etc.) daar problemen mee?** Dit is een bekende valkuil.
12. **Mag Rtools het systeem-`PATH` aanpassen?** Dit is nodig voor source-package compilatie.

### 1.5 Mac-specifiek (alleen als van toepassing)

13. **Mag XQuartz geïnstalleerd worden?** (Vereist voor sommige grafische R-packages.)
14. **Is Xcode Command Line Tools beschikbaar?** (Equivalent van Rtools op Mac, nodig voor source-installaties.)

---

## 2. Installatie­volgorde

> ⚠️ **Volgorde is belangrijk:** eerst R, dán RStudio, dán Rtools. RStudio is alleen een editor — die heeft R nodig om te draaien.

### Stap 1: R installeren

1. Ga naar **https://cran.r-project.org/**
2. Kies je besturingssysteem:
   - **Windows** → "Download R for Windows" → "base" → "Download R-4.x.x for Windows"
   - **Mac** → "Download R for macOS" → kies de juiste `.pkg` voor je chip (Apple Silicon = M1/M2/M3/M4, of Intel)
3. Voer de installer uit.
4. **Windows-specifiek:** installeer in `C:\R\R-4.x.x\` in plaats van het standaard `C:\Program Files\R\` (mits IT dit toestaat — zie vraag 4). Spaties in paden geven later problemen.
5. **Voor deze cursus werken we altijd via RStudio.** Je hoeft R niet zelf te openen. 

### Stap 2: RStudio Desktop installeren

1. Ga naar **https://posit.co/download/rstudio-desktop/**
2. De pagina detecteert je OS automatisch — klik op de download­knop.
3. Voer de installer uit met standaard­instellingen.
4. Open RStudio. Als alles goed is, zie je linksonder in het "Console"-paneel iets als:
   ```
   R version 4.5.2 (...) -- "..."
   ```
   Dit betekent dat RStudio R succesvol heeft gevonden. ✅

### Stap 3: Rtools installeren (alleen Windows)

> Niet strikt nodig in week 1, maar wel handig om meteen te installeren zodat je later niet vastloopt bij package-installaties die vanaf source compileren.

1. Ga naar **https://cran.r-project.org/bin/windows/Rtools/**
2. Kies de Rtools-versie die bij jouw R-versie past:
   - R 4.4.x → **Rtools44**
   - R 4.3.x → Rtools43
   - R 4.2.x → Rtools42
3. Download de installer en voer uit met standaard­instellingen.
4. Vink aan dat het systeem-`PATH` mag worden aangepast (zie IT-vraag 12).

**Controleren of Rtools werkt:** open RStudio, type in de console:

```r
Sys.which("make")
```

Als dit een pad teruggeeft (bijv. `C:\rtools44\usr\bin\make.exe`), werkt het. ✅

### Stap 4: Mac-equivalent van Rtools

Open Terminal en run:

```bash
xcode-select --install
```

Dit installeert de Xcode Command Line Tools. Geen Rtools nodig.

---

## 3. Eerste configuratie van RStudio

Dit doe je één keer, en bespaart je later veel kopzorgen.


### 3.1 Global Options

**Tools → Global Options:**

| Tab | Instelling | Waarde | Waarom |
|---|---|---|---|
| General | Restore .RData into workspace at startup | **uit** | Voorkomt dat oude data stiekem meekomt |
| General | Save workspace to .RData on exit | **Never** | Dwingt reproduceerbaarheid af |
| Code → Editing | Use native pipe operator | **aan** | Geeft `\|>` met Ctrl+Shift+M |
| Code → Display | Show line numbers | **aan** | Handig bij foutmeldingen |
| Code → Saving | Default text encoding | **UTF-8** | Voorkomt rare tekens bij ë/é/ï |
| Appearance | Editor theme | Naar smaak (Tomorrow Night = donker, populair) | Oog-comfort |

### 3.2 Proxy-instelling (alleen als IT dit aangeeft)

Pas de bestand `.Renviron` aan in je gebruikersmap (`C:\Users\[naam]\.Renviron`) met daarin:

```
http_proxy=http://proxyserver:poort
https_proxy=http://proxyserver:poort
```
Daarnaast zijn er project_specifieke folders op Nextcloud die met je gedeelt dienen te worden als je gebruik wilt maken van de project/lectoraat data. 

---

## 4. Eerste R-project aanmaken

Vanaf nu werk je **altijd via projecten**. Geen losse scripts in willekeurige mappen.

1. **File → New Project → New Directory → New Project**
2. Directory name: `r-leren`
3. Create as subdirectory of: kies een lokale map **buiten OneDrive/SharePoint** (zie IT-vraag 8). Bijvoorbeeld `C:\Users\[naam]\R-projects\`.
4. Klik **Create Project**.

Je hebt nu een map `r-leren` met daarin een bestand `r-leren.Rproj`. Voortaan open je RStudio door op dit `.Rproj`-bestand te dubbelklikken — dan zet RStudio automatisch de juiste werkmap.

---

## 5. Eerste packages installeren

In de RStudio console (linksonder), type:

```r
install.packages("tidyverse")
install.packages("here")
install.packages("usethis")
```

**Wat te verwachten:**
- Duurt 5–15 minuten (tidyverse bevat ~30 packages)
- Veel output, deels in rood — **rood ≠ fout** in R. Pas als er letterlijk `Error:` staat is er een probleem.
- Als er een vraag verschijnt over "Do you want to install from sources?", antwoord **No** (tenzij IT-vraag 11 expliciet OK is).

**Controleren of het werkt:**
```r
library(tidyverse)
```
Geen foutmelding = ✅

---

## 6. Troubleshooting: bekende valkuilen

| Probleem | Oorzaak | Oplossing |
|---|---|---|
| `Error in install.packages: unable to access index for repository` | Proxy/firewall blokkeert CRAN | IT-vragen 5–7 |
| `Warning: package was built under R version X.Y.Z` | Package nieuwer dan jouw R | Geen probleem, meestal werkt het |
| RStudio toont alleen een grijs scherm | Grafische driver-issue | RStudio opnieuw installeren met "Run as administrator" |
| `cannot create file ... permission denied` | Project staat in OneDrive | Project verplaatsen buiten sync-map |
| `Error: package or namespace load failed` | Package half geïnstalleerd | `remove.packages("naam")` dan opnieuw installeren |
| Console toont vreemde tekens i.p.v. ë/é | Encoding verkeerd | Global Options → UTF-8 |

---

## 7. Eindcheck — klaar om te beginnen?

Run dit in de console; alles moet zonder errors werken:

```r
# 1. R-versie checken
R.version.string

# 2. Tidyverse laden
library(tidyverse)

# 3. Eerste grafiek
ggplot(mtcars, aes(x = wt, y = mpg)) + 
  geom_point() + 
  labs(title = "Eerste grafiek!")
```

Zie je rechtsonder een scatter-plot? **Klaar.** 🎉 Door naar het cursus­materiaal.
