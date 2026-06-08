
# R Leren — Eerste Maand

**Doelgroep:** Nieuwe medewerker met data-analyse-ervaring (Excel/SPSS), zonder programmeer­ervaring  
**Werkvorm:** Zelfstudie met wekelijks sparring­moment (±30–60 min)  
**Tijdsbudget:** 4–6 uur per week  
**Voorwaarde:** Installatie­handleiding (document 01) is afgerond en de eindcheck werkt

---

## Overzicht — wat krijg je na 4 weken?

| Week | Thema | Eindresultaat |
|---|---|---|
| 1 | RStudio leren kennen + eerste visualisatie | Je kunt zelfstandig een script schrijven dat data inleest en een grafiek maakt |
| 2 | Data transformeren met dplyr | Je kunt alles wat je in Excel met filters/draaitabellen doet, in R |
| 3 | Data importeren + opschonen | Je kunt een echte Excel/CSV-file inlezen en bruikbaar maken |
| 4 | Eigen mini-project | Je hebt één werk-dataset volledig in R geanalyseerd |

---

## Week 0: Werkomgeving inrichten (1 uur, eenmalig)

### A. Hoe RStudio is opgebouwd

RStudio heeft vier panelen. Leer ze meteen herkennen — dit bespaart later veel verwarring.

```
┌─────────────────────────┬─────────────────────────┐
│                         │                         │
│   1. SOURCE (editor)    │   3. ENVIRONMENT        │
│   Hier schrijf je       │   Welke objecten/data   │
│   scripts (.R / .qmd)   │   zitten in geheugen?   │
│                         │                         │
├─────────────────────────┼─────────────────────────┤
│                         │                         │
│   2. CONSOLE            │   4. FILES / PLOTS /    │
│   Hier draait R.        │   PACKAGES / HELP       │
│   Losse commando's,     │   Bestandsverkenner,    │
│   resultaten, errors    │   grafieken, help-docs  │
│                         │                         │
└─────────────────────────┴─────────────────────────┘
```

**Belangrijkste werkwijze:** je schrijft code in het **Source-paneel** (1) en voert regels uit met **Ctrl+Enter** (Cmd+Enter op Mac). De code verschijnt dan in de **Console** (2) en wordt uitgevoerd. Type **nooit** rechtstreeks in de console voor echt werk — alles wat je daar typt is na het sluiten van RStudio weg.

**Belangrijkste sneltoetsen om meteen te leren:**

| Sneltoets (Windows/Linux) | Sneltoets (Mac) | Wat het doet |
|---|---|---|
| Ctrl+Enter | Cmd+Return | Voer huidige regel uit |
| Ctrl+Shift+M | Cmd+Shift+M | Type de pipe `\|>` |
| Alt+- | Option+- | Type de assign-arrow `<-` |
| Ctrl+Shift+N | Cmd+Shift+N | Nieuw R-script |
| Ctrl+S | Cmd+S | Opslaan |
| Ctrl+Shift+C | Cmd+Shift+C | Maak regel een comment (`#`) |
| Tab | Tab | Auto-complete (probeer dit altijd te gebruiken!) |


### B. R-Projects: waarom en hoe

Een **R-Project** (`.Rproj`-bestand) is een map die zichzelf herkent als project. Voordelen:
- Werkmap (working directory) staat automatisch goed
- Je hoeft nooit `setwd("C:/Users/.../...")` te typen
- Je kunt meerdere projecten naast elkaar open hebben in aparte RStudio-vensters
- Alles is overdraagbaar: zip de map en hij werkt op een andere laptop

**Regel:** elk leerdoel of project krijgt zijn eigen R-Project. Niet alles in één grote map.

### C. Aanbevolen mapstructuur per project

Maak deze structuur aan in je `r-leren` project:

```
r-leren/
│
├── r-leren.Rproj         ← Project-bestand (automatisch aangemaakt)
├── README.md             ← Wat staat er in dit project? (voor toekomstige jij)
│
├── data/
│   ├── raw/              ← Ruwe data, NOOIT bewerken
│   └── processed/        ← Opgeschoonde data
│
├── scripts/              ← Je R-scripts (.R bestanden)
│   ├── 01-week1-viz.R
│   ├── 02-week2-dplyr.R
│   └── ...
│
├── output/
│   ├── figures/          ← Opgeslagen grafieken
│   └── tables/           ← Geëxporteerde tabellen
│
└── notes/                ← Eigen aantekeningen, vragen, lessen
```

**Waarom deze structuur?**
- `data/raw/` apart houden = je kunt altijd terug naar de bron
- Gescheiden scripts per onderwerp = makkelijk terug te vinden
- `output/` apart = nooit een grafiek kwijt
- `README.md` = over 3 maanden begrijp je je eigen project nog

**Snelle setup — run dit één keer in de Console:**
```r
# Maakt mapstructuur aan in je huidige project
dirs <- c("data/raw", "data/processed", "scripts", 
          "output/figures", "output/tables", "notes")
sapply(dirs, dir.create, recursive = TRUE, showWarnings = FALSE)

# Maak een lege README aan
writeLines(
  c("# R Leren", "", "Persoonlijk leerproject voor R.", 
    "Start: [vul datum in]"),
  "README.md"
)
```

### D. Eén belangrijke gewoonte: het `here`-package

In plaats van paden hard te coderen (`"C:/Users/marit/.../data.csv"`), gebruik je `here()`. Dat maakt paden relatief aan je project.

```r
library(here)
# Verkeerd: hard pad
# read_csv("C:/Users/marit/R-projects/r-leren/data/raw/sales.csv")

# Goed: werkt op elke laptop
read_csv(here("data", "raw", "sales.csv"))
```

---

## Week 1: Eerste succes — visualisatie

**Doel:** RStudio voelt vertrouwd, je hebt een eigen grafiek gemaakt, je begrijpt de pipe `|>`.

### Materiaal
- **R for Data Science (2e ed.) Een gratis online boek. ** — https://r4ds.hadley.nz/
- Hoofdstukken: **1 (Data visualization), 2 (Workflow: basics), 3 (Data transformation, eerste helft tot §3.3)**
- **Posit cheat sheet: ggplot2** — https://posit.co/resources/cheatsheets/ → uitprinten

### Werkwijze (4–5 uur verdeeld over de week)

1. **Sessie 1 (90 min):** Lees & doe hoofdstuk 1 van R4DS. Type ALLE code-voorbeelden zelf over in een script `scripts/01-week1-viz.R`. Niet copy-pasten — typen bouwt spier­geheugen.
2. **Sessie 2 (60 min):** Hoofdstuk 2 R4DS. Hier leer je over objecten, functies, en de assign-arrow `<-`.
3. **Sessie 3 (60 min):** Hoofdstuk 3 t/m §3.3. Eerste kennismaking met de pipe en `filter()`.
4. **Sessie 4 (60 min):** Eigen oefening (zie hieronder).

### Eigen oefening week 1

Gebruik de ingebouwde dataset `mpg` (auto's en brandstof­verbruik). Maak in een eigen script:
1. Een scatterplot van `displ` (cilinder­inhoud) tegen `hwy` (verbruik snelweg)
2. Kleur de punten op `class` (type auto)
3. Geef de grafiek een Nederlandse titel, x-as label en y-as label
4. Sla de grafiek op in `output/figures/week1-mpg.png` met `ggsave()`

### Reflectievragen voor sparring­moment
- Wat was de meest verwarrende foutmelding deze week?
- Welk concept klikte pas op het tweede moment?
- Welk verschil voel je met Excel/SPSS?

---

## Week 2: Data wrangling met dplyr

**Doel:** alles wat je in Excel doet met filters, sorteren, nieuwe kolommen en draaitabellen, kun je nu in R.

### De Excel/SPSS → dplyr vertaal­tabel

| Excel/SPSS doe je... | In dplyr met... |
|---|---|
| AutoFilter aanzetten en filteren | `filter(data, kolom == "waarde")` |
| Sorteren op kolom | `arrange(data, kolom)` of `arrange(data, desc(kolom))` |
| Nieuwe kolom met formule | `mutate(data, nieuw = oud * 2)` |
| Kolommen verbergen | `select(data, kolom1, kolom2)` |
| Draaitabel met som per groep | `group_by(data, groep) \|> summarise(totaal = sum(waarde))` |
| VLOOKUP / INDEX-MATCH | `left_join(data1, data2, by = "id")` |
| Aantal rijen tellen per groep | `count(data, groep)` |
| Unieke waarden | `distinct(data, kolom)` |

### Materiaal
- **R4DS hoofdstuk 3 (volledig) + hoofdstuk 4 (workflow: code style) + hoofdstuk 5 (data tidying)**
- **Posit cheat sheet: dplyr** — uitprinten en naast scherm leggen

### Werkwijze (5 uur)
1. **Sessie 1 (90 min):** R4DS hoofdstuk 3 volledig. Pak per dplyr-werkwoord (filter, arrange, mutate, select, summarise, group_by) een voorbeeld.
2. **Sessie 2 (60 min):** Hoofdstuk 4 — code style. Installeer `styler`-package, gebruik Ctrl+Shift+A om automatisch op te maken.
3. **Sessie 3 (90 min):** Hoofdstuk 5 — tidy data concept. Dit is een aha-moment voor SPSS-gebruikers (lang vs. breed formaat).
4. **Sessie 4 (60 min):** Eigen oefening.

### Eigen oefening week 2

Gebruik dataset `nycflights13::flights` (installeer `install.packages("nycflights13")`):
1. Welke 5 luchtvaart­maatschappijen hadden gemiddeld de meeste vertraging?
2. Hoeveel vluchten vertrokken er per maand?
3. Maak een nieuwe kolom `vertraging_categorie` met waarden "op tijd", "klein", "groot" (gebruik `case_when()`)
4. Voor de top-3 bestemmingen: wat is de mediane vluchttijd?

Schrijf alles in `scripts/02-week2-dplyr.R`, met comments die uitleggen wat elke stap doet.

---

## Week 3: Data importeren en opschonen

**Doel:** je kunt een echte rommelige Excel/CSV inlezen en bruikbaar maken. Dit is waar 70% van het werk zit in de praktijk.

### Materiaal
- **R4DS hoofdstuk 7 (Data import) + hoofdstuk 6 (Workflow: scripts and projects) + hoofdstuk 19 (Joins)**
- **Posit cheat sheet: readr + tidyr** — uitprinten

### Belangrijke onderwerpen
- `read_csv()`, `read_excel()` (uit `readxl`-package)
- Kolomtypes specificeren (`col_types`)
- Missing values (`NA`) herkennen en afhandelen
- Datums parsen met `lubridate`
- Strings opschonen met `stringr`
- Joins: `left_join`, `inner_join`, `full_join`

### Werkwijze (5 uur)
1. **Sessie 1 (90 min):** R4DS hoofdstuk 7. Importeer een CSV met expres een paar problemen (verkeerde decimalen, datums als tekst).
2. **Sessie 2 (60 min):** Lubridate basics — datums zijn altijd een gedoe.
3. **Sessie 3 (90 min):** R4DS hoofdstuk 19 — joins. Teken op papier wat een left_join doet — dat helpt enorm.
4. **Sessie 4 (60 min):** Eigen oefening met écht werk-bestand.

### Eigen oefening week 3

Zoek (of vraag) een **echt Excel-bestand uit je werk** dat je normaal in Excel zou openen. Liefst iets met:
- Meerdere tabbladen, OF
- Datums in een rare format, OF
- Een join nodig (twee bestanden combineren)

Schrijf in `scripts/03-week3-import.R`:
1. Inlezen van het bestand met `read_excel()`
2. Opschonen: kolomnamen netjes (probeer `janitor::clean_names()`), datums correct, types kloppend
3. Bewaar het opgeschoonde resultaat in `data/processed/` als RDS-bestand (`write_rds()`)
4. Eén beschrijvende analyse + grafiek

---

## Week 4: Mini-project — eigen werk-vraag beantwoorden

**Doel:** transfer naar echt werk. Dit is waar het leren beklijft.

### Opzet

Kies één concrete vraag uit je werk die je normaal in Excel zou beantwoorden. Voorbeelden:
- "Hoe ontwikkelt [KPI] zich over de afgelopen 12 maanden per [dimensie]?"
- "Welke groepen [studenten/klanten/etc.] vallen het meeste uit, en wat zijn hun kenmerken?"
- "Klopt onze aanname dat X correleert met Y?"

### Werkwijze (6 uur, week vrij in te delen)

1. **Sessie 1 (60 min):** Vraag formuleren, data verzamelen in `data/raw/`. Schrijf in `README.md` wat de vraag is en welke bestanden je hebt.
2. **Sessie 2 (90 min):** Importeren + opschonen (`scripts/04-mini-project-01-prep.R`).
3. **Sessie 3 (90 min):** Analyseren (`scripts/04-mini-project-02-analyse.R`).
4. **Sessie 4 (90 min):** Visualiseren + één tabel-export (`scripts/04-mini-project-03-output.R`). Sla figuren op in `output/figures/`.
5. **Sessie 5 (60 min):** Reflectie schrijven in `notes/reflectie-maand1.md`: wat ging snel, wat duurde lang, waar liep je vast?

### Eindproduct

Het project moet reproduceerbaar zijn: een collega moet het project kunnen openen en alle scripts zonder handmatige stappen kunnen uitvoeren:
- Wat de vraag was (README)
- Waar de data vandaan komt (data/raw/)
- Welke stappen zijn gedaan (scripts/, in volgorde)
- Wat de uitkomst is (output/)

---

## Sparring­ritme

| Moment | Wat bespreken |
|---|---|
| Start week 1 | Verwachtingen, hoe vragen stellen tussendoor |
| Eind week 1 | Eerste indrukken RStudio, foutmeldingen uitleggen |
| Eind week 2 | Pipe-moment? dplyr-werkwoorden zitten? |
| Eind week 3 | Importeer-frustraties — wat normaal is en wat niet |
| Eind week 4 | Demo mini-project, vooruitblik maand 2 |

---

## Hulpmiddelen tijdens het leren

- **Help in RStudio:** type `?functienaam` in de console (bijv. `?filter`) → opent help rechtsonder
- **Vastgelopen?** Plak de foutmelding letterlijk in Google. R-community op StackOverflow is enorm.
- **ChatGPT/Claude:** pas vanaf week 3 gebruiken. Eerder houdt het mentale modellen tegen.
- **Sneltoetsen-overzicht:** Alt+Shift+K in RStudio

---

## Veelgemaakte beginners­fouten (en waarom ze normaal zijn)

| Fout | Wat het betekent | Oplossing |
|---|---|---|
| `object 'x' not found` | Variabele niet aangemaakt of typo | Check spelling, run de regel die `x` maakt eerst |
| `could not find function "..."` | Package niet geladen | `library(pakketnaam)` bovenaan script |
| `unexpected symbol` | Syntax-fout, meestal ontbrekende komma of haakje | Tel haakjes; RStudio markeert vaak de plek |
| `non-numeric argument` | Type-mismatch: tekst waar getal verwacht wordt | `as.numeric()` of check waarom kolom tekst is |
| Vergeten `library()` na restart | Packages vergeet R bij restart | Zet alle `library()` calls bovenaan elk script |

---

## Na de eerste maand: vooruitblik maand 2

Optionele richtingen, afhankelijk van wat de medewerker gaat doen:
- **Quarto / R Markdown** voor reproduceerbare rapporten
- **Statistiek in R** (Learning Statistics with R — Navarro)
- **Versie­beheer met Git** in RStudio

Deze keuze maken we eind week 4 op basis van wat het werk vraagt.
