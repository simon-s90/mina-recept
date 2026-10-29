# Recepthemsidan – design- och funktionsbeslut

**Projekt:** Recepthemsida på GitHub Pages  
**Datum:** 29 oktober 2026  
**Status:** Arbetsdokument, version 1.0  
**Syfte:** Samla det vi har kommit överens om så att design och implementation kan utgå från samma underlag. Detta dokument ersätter inte den separata listan över planerade recept.

> **Läsanvisning:** **Beslutat** = överenskommet; **Preliminärt** = visat eller föreslaget men inte slutligt fastställt; **Öppet** = behöver beslutas senare.

## 1. Övergripande inriktning – beslutat

- Webbplatsen ska vara en receptsamling som kan publiceras via **GitHub Pages**.
- Visuell stil: **skånsk allmoge** kombinerad med **ett gammalt receptarkiv** (designriktning C).
- Sidan ska kännas nostalgisk och handgjord men vara tydlig, praktisk och lätt att läsa.
- Allmogedetaljer används framför allt som dekoration, inte som bakgrund bakom recepttext.
- Konceptnamnet **Receptboken** används i skisserna, men det slutliga namnet är inte bestämt.

## 2. Visuell design

### 2.1 Uttryck – beslutat

- Varm, ljus papperskänsla och receptkort mot en beige bakgrund.
- Mörkgrön sidhuvudsyta och varmt mörkbrun text.
- Dämpat tegelrött/vinrött till rubriker, numrering och detaljer.
- Små blomsterornament inspirerade av skånsk allmoge, med återhållsam användning.
- Receptarkivet har **vanliga filter ovanför receptkort**, inte kategoriflikar som en fysisk receptlåda. Det senaste förslaget med flikar har uttryckligen valts bort.

### 2.2 Färger och typografi – preliminärt

| Användning | Exempelfärg |
|---|---|
| Mörkgrön (sidhuvud) | `#345047` |
| Varm beige (sidbakgrund) | `#E8DDC6` |
| Ljust papper (receptkort) | `#FFF9ED` |
| Gammalröd (accenter) | `#934A3E` |
| Mörkbrun (text) | `#4D372C` |

Rubriker bör ha ett klassiskt, bokliknande serif-utseende och brödtexten vara lättläst. Exakta typsnitt och färgkoder är **inte** låsta.

## 3. Webbplatsens delar

- **Startsida:** Ska finnas, men slutlig struktur och detaljer har inte fastställts.
- **Alla recept / receptarkiv:** Sök, filtrering och kort/lista över recepten.
- **Enskild receptsida:** Gemensam mall för samtliga recept.
- **Om sidan:** Förekommer i skisserna som möjligt menyval; innehållet är ännu inte bestämt.

## 4. Receptarkivet – beslutad funktion

### 4.1 Filtrering och sökning

1. **Fritextsökning** för att hitta recept (i prototypen söks på recepttitel).
2. **En huvudkategori åt gången** via rullista, samt alternativet ”Alla kategorier”.
3. **Flera extra taggar samtidigt** kan väljas.
4. Vald huvudkategori, valda taggar och eventuell söktext kombineras. När flera extra taggar väljs ska receptet matcha **samtliga** dessa taggar (OCH-logik).
5. En **Rensa filter**-funktion återställer urvalet.
6. Recepten visas som **receptkort** med titel, huvudkategori och taggar, inom den valda retrodesignen.

### 4.2 Huvudkategorier – princip beslutad, namn preliminära

Huvudkategorin ska beskriva **typen av maträtt**, inte måltidstyp (förrätt, huvudrätt osv.). Varje recept har **en** huvudkategori. Följande är arbetsförslag:

**Kött · Fågel · Fisk & skaldjur · Pasta · Potatisrätter · Korv · Äggrätter · Vegetariskt**

Fler eller andra kategorier kan införas när vi går igenom samtliga recept.

### 4.3 Extra taggar – struktur beslutad, lista preliminär

Varje recept kan ha **flera extra taggar** som beskriver karaktär, tillagning eller ursprung. Arbetsförslag:

**Husmanskost · Skånskt · Vardagsmat · Festmat · Långkok · Ugnsrätt · Snabblagat · Traditionellt**

Exempel: ”Skånsk äggakaga” kan ha huvudkategorin **Äggrätter** och taggarna **Skånskt**, **Husmanskost** och **Traditionellt**.

## 5. Enskild receptsida – beslutad innehållsmall

**Varje receptsida ska kunna innehålla följande sju delar:**

1. **Recepttitel**
2. **En eller flera receptbilder**
3. **Beskrivning av receptet**
4. **Antal portioner** (en fast uppgift)
5. **Ingredienser**
6. **Instruktioner** (numrerade steg)
7. **Anteckningar** (egen visuellt avskild sektion)

### 5.1 Bilder – viktigt krav

- Det ska gå att ange **flera bilder för ett recept**.
- **Varje instruktionssteg ska kunna ha en tillhörande bild**. Bilden visas i anslutning till just det steget.
- Steg utan bild visas som vanlig text utan reserverad, tom bildyta.
- Samma sidmall ska fungera för recept med inga instruktionsbilder, några instruktionsbilder eller bilder vid samtliga steg.
- Exakt utseende för bildgalleri när flera huvudbilder finns återstår att bestämma (t.ex. miniatyrer eller bildväxling).

### 5.2 Layout – beslutad riktning

- **Datorskärm:** Två kolumner. Vänster: titel, bilder, beskrivning, portionsantal och ingredienser. Höger: instruktioner med valfria stegbilder och anteckningar i anslutning till instruktionerna.
- **Mobil:** Innehållet läggs i en lättläst kolumn.
- **Ingredienser:** I en egen, svagt tonad ruta.
- **Instruktioner:** Numrerade, tydligt åtskilda steg.
- **Anteckningar:** Egen tonad ruta, visuellt skild från tillagningsstegen.

### 5.3 Funktioner som uttryckligen inte behövs

- **Ingen automatisk portionsomräkning.** Antal portioner visas som text.
- **Ingen avbockning av instruktionssteg.**

## 6. Teknisk inriktning – beslutad plattform, föreslagen implementation

Webbplatsen ska publiceras på **GitHub Pages**. Den överenskomna sökningen och filtreringen samt de valfria instruktionsbilderna kan implementeras i en statisk lösning med HTML, CSS och JavaScript. En server/databas behövs inte för den här grundfunktionen.

**Föreslagen datastruktur** (fält och sökvägar kan justeras när vi börjar utveckla):

```json
{
  "id": "skansk-aggakaga",
  "titel": "Skånsk äggakaga",
  "huvudkategori": "Äggrätter",
  "taggar": ["Skånskt", "Husmanskost"],
  "portioner": "4 portioner",
  "beskrivning": "...",
  "bilder": ["bilder/aggakaga-1.jpg", "bilder/aggakaga-2.jpg"],
  "ingredienser": ["..."],
  "instruktioner": [
    { "text": "Blanda smeten ...", "bild": "bilder/steg-1.jpg" },
    { "text": "Servera ..." }
  ],
  "anteckningar": "..."
}
```

Ovanstående är endast ett **illustrativt exempel**, inte ett fastställt recept. Den verkliga receptlistan hanteras i ett separat stöddokument.

## 7. Kvar att bestämma

- Webbplatsens slutliga namn och eventuell logotyp.
- Slutliga färgkoder, typsnitt och ornamentens omfattning.
- Startsidan och slutlig navigering.
- Exakt uppsättning huvudkategorier och extra taggar.
- Hur flera huvudbilder visas (galleri/miniatyrer/bildväxling).
- Om fritextsökningen även ska omfatta annat än recepttiteln.
- Hur receptdata och bilder ska organiseras i GitHub-repot.
- Slutligt receptinnehåll, portionsantal och fotografier.

## 8. Designreferens

En datorskärmsskiss av receptsidan med två kolumner togs fram i designarbetet. Den visar **stil och layout**, inte fastställda receptuppgifter. Bilden finns som separat fil: `designreferens_receptsida_dator.png`.

---

*Sammanställt utifrån projektets design- och funktionsdiskussion den 29 oktober 2026.*