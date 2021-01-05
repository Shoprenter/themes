## header.tpl

A fejléc reprezentálására szolgáló téma fájl. Általában tartalmazza a fejlécet a kategória menüvel együtt és egyes 
témáknál a bannerkezelő is a része. A **common** mappában található meg a Téma fájl szerkesztőben.

A header.tpl-ben is használható minden függvény, ami globálisan elérhető. Részletek erről a
 [Globális függvények](../theme-global/GLOBAL_FUNCTIONS.md) dokumentációban találhatóak.

### Felépítés

A fejléc a pagehead.tpl után kerül be a forráskódba. Két olyan html elem is található itt, amelynek a záró párja 
nem a header.tpl-ben található. Az egyik a ```<div class="page-wrap">``` a másik pedig a ```<main>```. Ennek a két 
html tagnek a záró része a footer.tpl-ben található.

Maga a fejléc tartalmi része a ```<header>``` tag-en belül van. Mivel a fejléc minden témánál egyedi, így a
**Tokyo témán** keresztül nézzük meg mit tartalmaz.

* **Mobil menü**

A fejlécben található a mobil menü betöltése is, ez található a legelső 3 sorban. A mobil menü csak akkor jelenik meg 
ha a karbantartás nincs bekapcsolva (maintance).

* **Üzenetek**

A page-wrap tetején található a hibaüzenetek vagy rendszer üzenetek kiírása a **shop_warning** változón keresztül. 
Ha admin felhasználóval belépünk akkor az oldal tetején megjelenő piros sáv lesz ez. 

* **Fejléc top pozíció**

A Tokyo témában található egy **header-top** pozíció elhelyezés. Itt jelenik meg például a Top line modul.

* **Navigációs sáv**

A navbar-ban több modul is megjelenítésre kerül. A bal oldalán a telefonszám jelenhet meg (ha be van kapcsolva a 
fejlécben való megjelenítés) és a nyelvváltó, pénznemváltó modulok (mobil eszköz kivételével). A jobb oldalán a 
belépés és a szöveges menüpontok jelennek meg. 

* **Fejléc alsó része**

A navigációs sáv alatt a fejléc alsó része jelenik meg a logóval, kategória menüvel és a kereső, kosár modulokkal. 
A Tokyo témában speciális a kereső modul, mivel csak egy ikon jelenik meg. Az ikonra kattinva egy teljes képernyős 
ablakban lehet a keresést végrehajtani. A kereső és kosár ikon között megjelenhet még a kívánságlista ikon is, ez is 
beállítás függő és akkor jelenik meg ha van a kívánságlistához adva tartalom. 

Ez a sáv egyben a sticky fejléc is, ami azt jelenti, hogy ha görgetünk az ablak tetején marad a fejléc alsó része.

A fejléc top pozíció, a navigációs sáv és a fejléc alsó része karbantartás módban nem jelenik meg, helyette csak a 
logó látszódik. Ennek lekezelése is a header.tpl része.

* **Banner pozíció**

A Tokyo témában a bannerkezelő modulnak dedikált pozíció van létrehozva a fejlécben scroller pozíció néven. 
Ez csak akkor jelenik meg ha a főoldalon vagyunk és nem karbantartás módban.

* **Kenyérmorzsa**

A pathway vagy breadcrumb vagyis a kenyérmorzsa a fejléc alatt és a main tartalom között jelenhet meg a pathway-top 
pozícióban. A pathway modult ugyanakkor egy másik pozícióba is át lehet helyezni, ez a pathway-inside pozíciób.

### Átadott adatok a header.tpl-nek

* **maintance**

Ha az oldal karbantartásra van kapcsolva, akkor a maintance változó értéke true, különben pedig false.

* **shop_warning**

Ha van ilyen üzenet akkor a ```<div id="page-warnings">...</div>``` jelenik meg a megfelelő szöveggel.

* **phone**

A bolt információknál megadott telefonszámot adja vissza.

* **isFrontPage** (deprecated)

Ha a kezdőlapon vagyunk akkor true az értéke, különben pedig false.



