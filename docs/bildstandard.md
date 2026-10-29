# Bildstandard – webbplats och framtida kokbok

**Projekt:** Receptboken  
**Status:** Riktlinje version 1.1, 29 oktober 2026  
**Mål:** Säkra bildernas ursprungliga kvalitet för en eventuell tryckt kokbok, samtidigt som hemsidan är snabb.

## 1. Grundprincip: en bild, två användningsversioner

För varje fotografi ska vi hantera två separata versioner:

1. **Master/original:** Den högsta upplösning som faktiskt finns tillgänglig, helst kamerans originalfil (RAW, HEIC, JPG eller PNG). Behåll den ursprungliga filen, färgprofilen och pixelstorleken. Gör inte en nedskalad eller hårt komprimerad fil till nytt "original".
2. **Webb:** En härledd kopia från masterfilen, normalt WebP. Den används av GitHub Pages i receptarkivet och på receptsidorna. Vid byte av beskärning eller kvalitet skapas en *ny* webbversion från mastern.

**Originalfiler för boktryck ska inte ersättas av webbkopior.** Att ändra DPI/PPI-metadata eller förstora filen i efterhand återställer inte förlorade bilddetaljer.

## 2. Tryckkvalitet

Målsättningen är **300 PPI vid den storlek som bilden placeras i den färdiga boken**. Vissa trycktjänster accepterar **250–300 PPI**, så det slutliga kravet anpassas till tryckeri och bokformat.

| Färdig bildyta i boken | Pixelmått vid 300 PPI (cirka) |
|---|---|
| 10 × 15 cm | 1181 × 1772 px |
| 15 × 20 cm | 1772 × 2362 px |
| 20 × 25 cm | 2362 × 2953 px |
| A4, hel sida | 2480 × 3508 px |

Utgångspunkt för nya huvudfoton: **helst minst 3000–4000 px på längsta sidan**, gärna större. Det är en arbetsmålsättning, inte ett automatiskt godkännande för varje boklayout. Instruktionsbilder som ska tryckas mindre kan fungera med färre pixlar. Behåll originalet med full upplösning oavsett mått.

**Viktigt:** Antalet pixlar och bildens kvalitet avgör utskriftsmöjligheterna – att bara märka en fil ”300 DPI” hjälper inte. Vid beskärning minskar tillgängliga pixlar för tryckytan.

## 3. Format och kvalitet

### Original för arkiv och bok
- **RAW** om det finns (t.ex. DNG/CR3/NEF), och gärna även en färdig högkvalitativ export.
- Annars original-**HEIC**, **JPG/JPEG** eller **PNG** i full upplösning.
- Exportera vid behov ett redigerat tryckoriginal som högkvalitativ JPEG eller TIFF; använd det ursprungliga fotoarkivet som master.
- Bevara färgprofil och metadata om de finns. Ändra inte automatiskt hela arkivet till CMYK. Slutlig CMYK- eller RGB-leverans bestäms med tryckeriet.
- Undvik upprepade JPEG-sparningar och hård komprimering.

### Webbkopior
- **WebP** i sRGB som förstahandsformat.
- För normal receptbild: cirka **1200–1600 px på längsta sidan** om källan räcker. Skapa inte falsk högupplösning när källfilen är mindre.
- Sikta på god visuell kvalitet (typiskt cirka 80–85 på en 0–100-encoder-skala, men granska resultatet visuellt); anpassa filstorleken efter bildinnehållet.
- Behåll originalets bildutsnitt i masterfilen. Beskär vid behov en *separat* webbkopia.
- Små förhandsvisningar kan genereras separat senare om arkivet växer.
- Ingen reklamtext, vattenmärkning eller grafik på fotot.

## 4. Lagring och mappstruktur – beslut: samma repository

**Vi lagrar tills vidare både högupplösta original och optimerade webbversioner i samma repository:** `simon-s90/mina-recept`. Detta förenklar administrationen och gör att materialet till en eventuell bok ligger samlat med recepten.

Rekommenderad struktur:

```text
mina-recept/
  bilder/
    falukorv-caprese/
      original/
        huvudbild.png             <- högsta tillgängliga original, ej nedskalad
      huvudbild.webp              <- komprimerad webbkopia
      extra-01.webp               <- ytterligare webbkopia (om tillämpligt)
      steg-01.webp                <- eventuell bild till instruktionssteg
      bildinfo.md                 <- källa, rättigheter, originalmått, fotograf
  data/
    recept.json                   <- referenser bara till webbkopior
```

**Viktigt att känna till:**
- Repositoryt är **publikt**. Alla original som sparas där kan läsas och laddas ned av andra, även om recepten inte länkar till dem. De kan även ingå i publicerade GitHub Pages-filer när Pages publicerar hela rotmappen. Lägg därför inte in privata eller rättighetsbegränsade original som inte får vara offentliga.
- GitHub rekommenderar repositories under cirka **1 GB**. GitHub Pages har dessutom ett tak på **1 GB för den publicerade webbplatsen**. När antalet högupplösta original ökar måste vi följa storleken och vid behov publicera enbart webbkopior via separat byggflöde, eller flytta originalarkivet till en särskild gren eller lagringsplats.
- Vanlig Git har en hårdgräns på **100 MiB per enskild fil**. Git LFS kan användas för stora filer i ett repository men kan **inte** användas som direkt bildkälla i GitHub Pages.
- **GitHub är inte vår enda backup.** Ta en separat säkerhetskopia av originalbilderna, exempelvis lokalt eller i molnet. Om original raderas eller ersätts på GitHub ska en oförändrad kopia fortfarande finnas kvar.
- Vi ska inte automatiskt förstora eller komprimera original som bara finns i låg upplösning; dokumentera i stället deras begränsningar för tryck.

Den här lösningen används tills projektets storlek eller integritets-/rättighetskrav motiverar en annan arkitektur.

## 5. Spårbarhet mellan bild och recept

Använd samma unika recept-id för bilder och receptdata. För varje bild bör vi dokumentera:
- Receptets `id`, t.ex. `falukorv-caprese`
- Originalets filnamn och var mastern arkiverats
- Pixelmått och filformat för originalet
- Filnamn på härledd webbversion
- Bildtyp: huvudbild, extra receptbild eller bild till ett specifikt steg
- Fotograf/ursprung, rättigheter för webbpublicering **och** boktryck samt eventuella begränsningar
- Eventuella redigeringar eller beskärningar

I `data/recept.json` används fortsatt `bilder` för receptbilder och `instruktioner[].bild` för stegbilder. Där anges enbart relativa sökvägar till webbkopiorna. Den gemensamma receptlayouten behöver inte ändras.

## 6. Kontroller vid nya receptbilder

1. Spara användarens högsta tillgängliga original **före** storleksändring eller komprimering.
2. Inspektera originalets upplösning, skärpa och eventuell text/reklam i motivet.
3. Kontrollera att publicerings- och eventuella framtida tryckrättigheter finns.
4. Lägg master i `bilder/<recept-id>/original/` på GitHub, med tydlig koppling till recept-id och separat säkerhetskopia, **förutsatt att offentlig lagring är tillåten**.
5. Skapa separat webbkopia; optimera försiktigt och kontrollera resultatet.
6. Ladda upp webbkopia till `bilder/<recept-id>/` på GitHub.
7. Uppdatera receptets `bilder` eller `instruktioner[].bild`.
8. Kontrollera webbsidan samt dokumentera tryckbegränsningar om källan är liten.

## 7. Befintligt foto: Falukorv caprese

- Bifogad rena källbild: **1122 × 1402 px**, PNG (ca 3,96 MB).
- Nuvarande webbversion: **600 × 750 px**, WebP (ca 66 kB).
- Vid **300 PPI** räcker källfilens pixelmått till ungefär **9,5 × 11,9 cm** *utan ytterligare beskärning*. Webbversionen räcker till ungefär **5,1 × 6,4 cm** vid samma täthet.
- Webbversionen är **inte** ett tryckoriginal. Om vi senare vill trycka bilden större bör en större originalkälla efterfrågas i stället för att bara förstora webbfilen.
- Fotots ursprung och rättigheter för publicering i en framtida bok är **inte fastställda**. Ta reda på detta innan tryckning. Bilder från reklam eller andras receptmaterial kräver särskild kontroll.

## 8. Öppna beslut

- Exakt metod och plats för den separata säkerhetskopian av originalbilderna.
- Om den framtida kokboken ska vara t.ex. A5, A4 eller annat format, vilket bestämmer den verkliga bildytan.
- Färghantering, utfall och tryckprofiler när tryckeri har valts.

**Referenser:** [Adobe om 300 PPI](https://helpx.adobe.com/photoshop/desktop/crop-resize-transform/resize-adjust-resolution/resolution-specs-for-printing-images.html), [Blurb om upplösning vid tryckstorlek](https://support.blurb.com/hc/en-us/articles/33366159811981-What-size-should-my-image-be-Understanding-image-resolution-for-Blurb), [GitHub om stora filer](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github), [GitHub om Git LFS och Pages](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-git-large-file-storage).
