#### style.scss
A témához tartozó stíluslap az **assets/style.scss** ami [SCSS](https://sass-lang.com) előfeldolgozóban íródott.
A fájl tetején találhatóak a témához tartozó meta információk, mint például milyen framework-öt használ a téma,
vagy mi a téma neve.

A változókat a témában fel lehet használni a **style.scss** stílusleíró fájlban. Minden változó ami a
**config.data.json** fájl presets objektumán belül meg van adva, elérhető **SCSS** változóként!

Például a global-font-color értéke a JSON -ben #8e8e8e, a style.scss-ben felhasználható a $global-font-color:

```scss
body {
    color: $global-font-color; // #8e8e8e
}
```

Tipp: Ha a style.scss-ben nem találjuk meg hol van a kezdeti értékadása egy változónak, akkor az nagy valószínűséggel
a config.data.json fájlban lesz definiálva.

#### Téma fájlok
- [header.tpl](theme-templates/HEADER_TPL.md)
- [pagehead.tpl](theme-templates/PAGEHEAD_TPL.md)
