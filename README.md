# Receptboken

Första versionen av en recepthemsida i gammaldags skånsk allmogestil, avsedd för **GitHub Pages**. Webbplatsens namn är ett arbetsnamn.

## Ingår

- **Startsida** (`index.html`) med sökfält och utvalt recept.
- **Receptarkiv** (`arkiv.html`) med sökning på titel, en huvudkategori, flera valfria taggar och OCH-filtrering.
- **Enskild receptsida** (`recept.html?id=...`) med titel, valfritt bildgalleri, beskrivning, portionsantal, ingredienser, stegvisa instruktioner och anteckningar.
- **Responsiv layout:** två kolumner för recept på dator och en kolumn på mobil.
- Stöd för **en bild per tillagningssteg**, men inga tomma bildytor om en bild saknas.
- **Inget** portionsreglage eller avbockning av steg.
- Första receptet: **Citronmarinerade kycklingspett**.

## Recept och foton

All receptdata finns i `data/recept.json`. Varje recept är ett objekt i listan.

- `huvudkategori` = exakt en huvudkategori, baserad på typ av maträtt.
- `taggar` = en lista med valfritt många extra taggar.
- `bilder` = en lista av sökvägar för receptbilder, till exempel `bilder/citronmarinerade-kycklingspett/klart.jpg`.
- `instruktioner` = en lista med `text` och ett valfritt `bild`-fält (och eventuellt `bildtext`) per steg.

Lägg bilder i `bilder/<receptets-id>/` och ange sökvägarna i receptfilen. När `bilder` är tom visas en **tydligt märkt illustration** som tillfällig platshållare; något receptfotografi har ännu inte levererats. Tillagningssteg utan `bild` visar endast text. I den här första versionen ingår inga egentliga matfotografier.

Citronmarinerade kycklingspett har preliminärt huvudkategorin `Fågel` och taggarna `Grillat` och `Marinerat`.

## Publicering på GitHub Pages

Lägg filerna i roten på det repository som ska vara webbplatsen. Gå till **Settings → Pages**, välj publicering från branch (vanligen `main`) och mappen **`/(root)`**. GitHub Pages kan också vara konfigurerat via GitHub Actions; anpassa efter repositoryts befintliga inställningar.

Webbplatsen använder relativa sökvägar och fungerar både som `användare.github.io` och under en projektsökväg `användare.github.io/repository/`.

### Lokal test

Kör i webbplatsens mapp:

```bash
python3 -m http.server 8000
```

Öppna sedan `http://localhost:8000/` i webbläsaren. Att dubbelklicka direkt på HTML-filerna via `file://` fungerar inte alltid eftersom receptfilen läses med `fetch`.

## Beslutsunderlag

Se `docs/designbeslut.md` för design- och funktionsbeslut från projektets tidigare diskussion.