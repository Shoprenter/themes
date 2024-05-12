# Tartalomjegyzék
* [CSS felépítés](#css-felépítés)
* [CSS válzotók](#css-változók)
* [Képek](#képek)
* [Kiemelt TPL fájlok](#kiemelt-tpl-fájlok)
  * [pagehead.tpl](#pageheadtpl-commonpageheadtpl)
      * [content_for_header](#content_for_header)
      * [színek](#színek)
      * [betűtípusok](#betűtípusok)
  * [header.tpl](#headertpl-sectionsheadertpl)
  * [footer_scripts.tpl](#footer_scriptstpl-commonfooter_scriptstpl)
* [3rd party megoldások](#3rd-party-megoldások)

# Starter 2.0

A Starter 2.0 egy kiinduló téma a Shoprenter rendszerében, amelyet kifejezetten partnereknek fejlesztettünk, akik egyedi arculatot készítenek. A fejlesztőknek lehetőségük van teljesen testreszabni a témát.

---

# CSS felépítés

A téma optimalizált módon tölti be a CSS fájlokat annak érdekében, hogy minimalizálja az oldalbetöltési időt és javítsa a felhasználói élményt.

A CSS fájlokat csak akkor töltjük be, ha az az adott oldalon vagy funkcióban valóban szükséges.

Általánosságban arra törekszünk, hogy csak azok a CSS fájlok töltődjenek be, amelyeket valóban használunk az alkalmazás különböző részein, néhány kivétel azonban mégis létezik. Ezek a kivételek olyan alapvető stílusfájlok és harmadik féltől származó CSS-ek, melyek elengedhetetlenek az oldal megjelenítéséhez és a funkciók megfelelő működéséhez.
1. <b>base.css</b>: Az alapvető alapstílusokat és formázásokat tartalmazza, amelyek elengedhetetlenek az oldal strukturálásához és az általános megjelenéséhez. 
2. <b>header.css és footer.css</b>: A fejléc és lábléc formázásait tartalmazó css-ek.
3. <b>product-card.css</b>: A termékkártyák formázását tartalmazó fájl.
3. <b>harmadik féltől származó CSS-ek</b>: Bizonyos esetekben olyan harmadik féltől származó stílusokat használunk, amelyek elengedhetetlenek bizonyos funkciók vagy komponensek megfelelő működéséhez.

A CSS fájlok az <b>assets</b> mappában találhatóak, téma fájl szerkesztő segítségével itt tudjuk móodsítani.

A CSS fájlok betöltése a TPL-ekben történik a [stylesheet_tag](../theme-global/GLOBAL_FILTERS.md#stylesheet_tag) filter használatával. Példa:<br>
``` {{ 'base.css' | asset_url | stylesheet_tag(preload = true) }} ```

---

# CSS változók
Az általános css tulajdonságokat(pl. gap, gutter ) a base.css-ben írtuk meg, míg a színek beállítása a pagehead.tpl-ben történik.
Minta egy általános szabály hozzáadására:
``` 
:root {
    --custom-property: red;
}
```
használat:
``` 
body {
    color: var(--custom-property);
}
```

---

# Képek
A témában található kép elemeket [image_tag](../theme-global/GLOBAL_FILTERS.md#image_tag) vagy [image_url](../theme-global/GLOBAL_FILTERS.md#image_url) filterek használatával oldottuk meg. A hajtás alatti (below the fold) képeket minden esetben elláttuk loading="lazy" attribútummal.

---

# Kiemelt TPL fájlok
## pagehead.tpl (common/pagehead.tpl)

A `<head>` és a `<body>` nyitó elemeket tartalmazza a pagehead.tpl, ide kerülnek a meta tagek és a scriptek.

### content_for_header
Kötelező ezt a változót megadni a pagehead.tpl-ben, enélkül nem működik a rendszer.
A `<head>` nyitó és a `</head>` záró tagek közé kell elhelyezni.
Ez a változó tartalmazza a szükséges ShopRenter scripteket, amik a rendszer működéséhez kellenek.

### színek
A :root CSS pseudo-osztályon keresztül itt injektáljuk a dokumentumba CSS változók segítségével a színeket, melyek a [config.data.json](../theme-configs/CONFIG_DATA_JSON.md) fájlban vannak definiálva. Így a színek használhatóak tpl és css szinten is. A színek megfelelő kontrasztjának biztosítása érhető el a [color_contrast](../theme-global/GLOBAL_FILTERS.md#color_contrast) filter alkalmazásával.

### betűtípusok
A snippets/fonts.tpl fájlon keresztül töltjük be @font-face szabályt használva a témához tartozó betűtípust, amelyet a base.css-ben állítjuk be a body elemre. Alapértelmezetten saját szerveren tárolt betűtípust használunk.

---

## header.tpl (sections/header.tpl)
A fejléc megjelenítését és funkcionalitását szolgáló [dinamikus modul](../theme-sections/DOCS.md). Tartalma:
1. telefonszám, email cím
2. fejléc linkek
3. nyelvváltó és pénznem váltó modulok
4. hamburger/mobil menü
5. logó
6. keresés modul
7. kívánságlista modul
8. belépéshez tartozó link
9. kosár modul
10. kategória modul

<i>A dinamikus modulban található logó csak itt a fejlécben jelenik meg. Adminon az általános beállításoknál található logó jelenik meg minden más esetben (pl. rendelés visszaigazoló e-mailek).</i>

---

## footer_scripts.tpl (common/footer_scripts.tpl)
A témához tartozó JS fájlok betöltését szolgáló fájl. Az itt található JS-kódok a dokumentum törzsének záró ``</body>`` tagja előtt kerülnek beillesztésre a layout/base.tpl-fájlban az [include twig tag](https://twig.symfony.com/doc/1.x/tags/include.html) segítségével.

---

# 3rd party megoldások
A harmadik féltől származó eszközöket saját szerveren hosztoljuk és a [script_tag](../theme-global/GLOBAL_FILTERS.md#script_tag) filterrel töltjük be.
1. [jQuery 3.7.1](https://jquery.com/)
2. [FancyBox 3.5.7](https://fancyapps.com/)
3. [Mmenu JS 9.3.0](https://mmenujs.com/)
4. [Slick JS 1.9.0](https://kenwheeler.github.io/slick/)
5. [Tippy JS 6.3.7](https://atomiks.github.io/tippyjs/)
6. [Popper JS 2.11.8](https://floating-ui.com/?utm_source=popper.js.org)
7. [jQuery.countdown](https://github.com/hilios/jQuery.countdown)
