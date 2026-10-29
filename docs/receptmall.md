# Mall för att lägga till nya recept

**Projekt:** Receptboken – `simon-s90/mina-recept`  
**Syfte:** Alla recept ska ha samma innehållsstruktur och automatiskt använda webbplatsens gemensamma design.  
**Gäller för:** Startsidan, receptarkivet och enskilda receptsidor.

> **Grundregel:** Lägg till receptdata och bilder, inte nya HTML-sidor. All receptinformation ligger i `data/recept.json`. Layouten kommer från `recept.html`, `app.js` och `styles.css`. Dessa filer ska normalt **inte** ändras när ett nytt recept läggs in.

## 1. Receptmall att fylla i (utan kod)

Kopiera följande struktur när du samlar in ett nytt recept. Den kan också klistras in direkt i en konversation och sedan föras över till JSON-format.

```text
RECEPTTITEL
[Namn på maträtten]

HUVUDKATEGORI
[Exakt en: t.ex. Kött, Fågel, Fisk & skaldjur, Pasta, Potatisrätter, Korv eller Äggrätter]

TAGGAR
[Valfritt antal: t.ex. Husmanskost, Grillat, Marinerat, Skånskt]

ANTAL PORTIONER
[T.ex. 4 portioner]

BESKRIVNING
[2–3 meningar som introducerar rätten]

RECEPTBILDER
[Ingen, en eller flera foton av den färdiga rätten. Ange filnamn om bilder finns.]

INGREDIENSER
- [Mängd + enhet + råvara]
- [Mängd + enhet + råvara]

INSTRUKTIONER
1. [Ett tydligt moment. Bild: valfritt filnamn.]
2. [Nästa moment. Bild: valfritt filnamn.]

ANTECKNINGAR
[Tips, ersättningar, variationer, förberedelser – eller "Inga".]
```

## 2. Fält som finns i den tekniska receptmallen

| Fält | Typ | Regel | Hur det visas |
|---|---|---|---|
| `id` | Text | **Obligatoriskt.** Unikt, beständigt, bara små bokstäver, siffror och bindestreck, utan å/ä/ö. | Del av receptets webbadress. |
| `titel` | Text | **Obligatoriskt.** Kort och tydlig svensk recepttitel. | Stor rubrik på receptsidan och rubrik på kort. |
| `huvudkategori` | Text | **Obligatoriskt.** Exakt **en** kategori av typen maträtt. Återanvänd befintlig stavning. | Kategorietikett och kategori-filter. |
| `taggar` | Lista av texter | En eller flera taggar om de passar; `[]` om inga behövs. Återanvänd exakt stavning och versaler. | Etiketter och fler-val-filter. |
| `portioner` | Text | **Obligatoriskt.** Fast värde, t.ex. `4 portioner`, ingen automatisk uträkning. | Raden "Portioner: …". |
| `beskrivning` | Text | **Obligatoriskt.** Några sammanhängande meningar, undvik att återge hela instruktionen. | Under receptbilderna och på receptkort. |
| `bilder` | Lista av bildsökvägar | `[]` om det saknas bilder, annars en eller flera vägar relativt webbplatsens rot. **Första bilden är huvudbild.** | Stor huvudbild samt miniatyrer vid fler bilder. |
| `ingredienser` | Lista av texter | **Obligatoriskt.** En ingrediens per rad i ordning, med mängd/enhet när den är känd. | Ingrediensruta till vänster på dator. |
| `instruktioner` | Lista av objekt | **Obligatoriskt.** Ett objekt per moment med `text`; valfritt `bild` och `bildtext`. | Numrerade moment till höger på dator. |
| `anteckningar` | Text | Ange tips eller `""` om det inte finns anteckningar. | Egen tonad ruta när texten inte är tom. |

**Viktigt:** Hela webbplatsen använder dessa fältnamn exakt som de står. Skapa inte egna varianter som `receptnamn`, `kategori`, `steps` eller `photos`.

## 3. Kopierbar JSON-mall

Filen [`docs/receptmall.json`](receptmall.json) innehåller samma objekt att utgå från. Den är bara ett exempel och **ska inte visas som ett riktigt recept**. Byt ut alla exempelvärden och gör `id` unikt innan objektet läggs till i receptsamlingen.

```json
{
  "id": "receptets-id",
  "titel": "Receptets namn",
  "huvudkategori": "Fågel",
  "taggar": [
    "Grillat",
    "Marinerat"
  ],
  "portioner": "4 portioner",
  "beskrivning": "En kort beskrivning av rätten, smaken och när den passar att laga.",
  "bilder": [],
  "ingredienser": [
    "500 g råvara",
    "2 msk ingrediens",
    "Salt och peppar"
  ],
  "instruktioner": [
    {
      "text": "Beskriv det första steget i en tydlig mening."
    },
    {
      "text": "Beskriv nästa steg. Lägg till en bild om det finns en bild av just detta moment."
    }
  ],
  "anteckningar": "Tips, variationer och sådant som är bra att komma ihåg."
}
```

**Observera:** I den tekniska JSON-filen ska alla fältnamn och textvärden ha dubbla citattecken. Skriv inte kommentarer inne i JSON och avsluta inte det sista objektet eller sista listvärdet med ett extra kommatecken.

## 4. Bilder – huvudbilder och bilder per instruktionssteg

Rekommenderad mappstruktur för varje recept:

```text
bilder/
└── receptets-id/
    ├── huvudbild.jpg
    ├── extra-01.jpg
    ├── steg-01.jpg
    └── steg-02.jpg
```

- **Huvudbild och extra bilder:** Ange sökvägarna i `bilder`. Den första visas stor och används också som bild på receptkortet. Ytterligare bilder visas som miniatyrer på receptsidan.
- **Stegbilder:** En instruktionsbild kopplas till just sitt steg med `bild`. Den valfria `bildtext` visas under fotot.
- **Utan stegbild:** Utelämna både `bild` och `bildtext`. Då visas endast stegets text utan tomt bildutrymme.
- **Utan receptfoto:** Använd `"bilder": []`. Nuvarande version av webbplatsen visar då en tillfällig illustration. Lägg helst till ett riktigt receptfoto när ett finns.
- **Filnamn:** Använd små bokstäver, bindestreck och helst `jpg` eller `webp`. Undvik mellanslag, svenska tecken och ändringar i filnamn efter publicering.
- **Kvalitet:** Använd tydliga, korrekt beskurna matbilder. Komprimera dem för webb så att sidorna laddar snabbt. Samma typ av bildutsnitt för receptens huvudbilder ger ett mer enhetligt arkiv.
- **Bildrättigheter:** Lägg bara till egna bilder eller sådana ni har rätt att publicera.

Exempel: recept med två huvudbilder och bilder till **endast vissa** steg:

```json
{
  "bilder": [
    "bilder/receptets-id/huvudbild.jpg",
    "bilder/receptets-id/extra-01.jpg"
  ],
  "instruktioner": [
    {
      "text": "Förbered ingredienserna.",
      "bild": "bilder/receptets-id/steg-01.jpg",
      "bildtext": "Ingredienserna före tillagning."
    },
    { "text": "Tillaga rätten enligt anvisningen." },
    {
      "text": "Lägg upp och servera.",
      "bild": "bilder/receptets-id/steg-02.jpg"
    }
  ]
}
```

Exemplet ovan är ett **utdrag** som visar fälten `bilder` och `instruktioner`; det är inte ett fullständigt recept.

## 5. Kategorier och taggar – konsekvent filtrering

- Varje recept får **en** `huvudkategori`, baserad på typ av maträtt, inte på om det är lunch eller middag.
- Välj helst en kategori som redan finns i `data/recept.json`. Första receptet använder `Fågel`. Övriga föreslagna kategorier: `Kött`, `Fisk & skaldjur`, `Pasta`, `Potatisrätter`, `Korv`, `Äggrätter` och `Vegetariskt`. Dessa förslag är inte en låst lista.
- Använd `taggar` för egenskaper som tillagningssätt, ursprung eller karaktär, t.ex. `Grillat`, `Marinerat`, `Skånskt`, `Husmanskost`, `Långkok` eller `Vardagsmat`. Dessa är exempel, inte en fast lista.
- Använd samma stavning överallt. `Grillat` och `grillat` skulle annars bli **två olika filterval**. Undvik synonymer för samma sak och upprepa inte samma tagg inom receptet.
- Välj bara relevanta taggar; två–fyra är en praktisk riktlinje, men inget tekniskt krav.
- Arkivets filtrering kombinerar vald huvudkategori, sökning och **alla** aktiverade taggar (OCH-logik). Därför ska taggar tilldelas genomtänkt.
- Kategorier och taggknappar skapas automatiskt ur receptdata: du behöver inte lägga till nya knappar i HTML.

## 6. Språk och redaktionell enhetlighet

1. **Titel:** Skriv som en vanlig svensk maträtt, till exempel `Citronmarinerade kycklingspett`. Undvik onödigt stora bokstäver.
2. **Beskrivning:** Cirka 1–3 meningar. Förmedla rättens smak eller sammanhang. Lägg inte in steg-för-steg-anvisningar här.
3. **Portioner:** Ange ett fast antal, helst som `4 portioner`. Behåll särskilda preciseringar om originalreceptet behöver dem.
4. **Ingredienser:** En ingrediens per rad. Välj konsekvent skrivsätt: `0,5 dl`, `2 msk`, `3 vitlöksklyftor`. Hitta inte på mängder som saknas i originalet.
5. **Steg:** Ett logiskt moment per steg, i uppmaningsform (t.ex. `Skär`, `Blanda`, `Grilla`). Numreringen skapas automatiskt; skriv inte `1.` i `text`.
6. **Anteckningar:** Placera variationer, förvaringstips och alternativa ingredienser här i stället för att blanda ihop dem med grundstegen.
7. **Fakta:** Om ett recept saknar viktig information, behåll det som okänt och kontrollera i stället för att hitta på tid, temperatur, mängd eller portionsantal.

## 7. Så lägger du till ett recept i GitHub

1. Öppna repositoryt `simon-s90/mina-recept` och därefter filen **`data/recept.json`**.
2. Kopiera objektet från `docs/receptmall.json`, eller låt det nya receptet skrivas i samma fältstruktur.
3. Ändra samtliga exempelvärden. **Ge receptet ett unikt `id`** som inte ska ändras senare. Exempel: `skansk-aggakaga`. Använd helst receptets namn, med å/ä ersatt av a och ö ersatt av o.
4. Lägg till eventuella bilder i **`bilder/<id>/`**, och fyll i de exakta relativa bildsökvägarna.
5. Lägg det nya objektet **inne i den befintliga listan** (`[` och `]`). Sätt komma mellan recepten, men inte efter det sista receptet.
6. Kontrollera att JSON-filen är giltig. Lokalt kan du köra `python3 -m json.tool data/recept.json`. Den ska inte ge fel.
7. Spara/committa ändringarna. GitHub Pages publicerar sedan de uppdaterade filerna när dess bygg-/publiceringsflöde har slutförts.
8. Öppna **`arkiv.html`** på den publicerade webbplatsen och kontrollera att receptet syns, att sökning och filter hittar det samt att receptsidan visar rätt text och rätt bilder på dator och mobil.

**Webbadress:** Receptsidan använder `recept.html?id=<receptets-id>`. Ingen separat HTML-fil eller manuell länk i navigeringen behövs.

### Exempel: JSON-lista med två recept

```json
[
  { "id": "forsta-receptet", "titel": "Första receptet" },
  { "id": "andra-receptet", "titel": "Andra receptet" }
]
```

Detta visar **bara listsyntaxen**. Riktiga recept måste innehålla alla relevanta fält från mallen ovan.

## 8. Checklista före publicering

- [ ] Titeln stämmer och `id` är unikt.
- [ ] En huvudkategori finns, med samma stavning som motsvarande tidigare recept.
- [ ] Taggarna är relevanta, enhetligt stavade och inte duplicerade.
- [ ] Portionsuppgift, beskrivning, ingredienser och alla tillagningssteg är med.
- [ ] Stegen står i rätt ordning och instruktionstexten är tillräckligt tydlig.
- [ ] Bilder ligger i rätt mapp och sökvägarna stämmer; stegbilderna sitter på rätt steg.
- [ ] Anteckningar ligger i `anteckningar`, inte ihopblandade med tillagningsstegen.
- [ ] JSON är giltig och inga kommatecken eller citattecken saknas.
- [ ] Receptet syns i arkivet, går att filtrera fram och fungerar på mobil samt dator.

## 9. Det här ska *inte* göras för varje nytt recept

- Skapa **inte** en separat HTML-sida för varje recept.
- Ändra **inte** färger, typsnitt, rutornas utseende eller placeringen av innehåll för ett enstaka recept.
- Lägg **inte** in portionsräkning eller avbockning; de funktionerna har valts bort.
- Lägg **inte** till dekorationsbilder som instruktionsfoton; stegbilder ska visa själva momentet.

Om alla recept följer denna mall blir sidornas design enhetlig och nya recept blir automatiskt sökbara och filtrerbara.

---

**Tekniska källor i projektet:** `data/recept.json`, `app.js`, `recept.html`, `arkiv.html`, `styles.css`. Se även `docs/designbeslut.md`.
