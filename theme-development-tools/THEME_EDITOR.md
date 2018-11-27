## Theme Editor

A Theme Editor egy olyan új funkció a ShopRenter rendszerében amellyel lehetőséget adunk arra, 
hogy az ügyfelek egyedi design igényeiket önerőből vagy külső segítséggel megvalósíthassák!
A funkció bekapcsolását a ShopRenter [Partner Support](mailto:partnersupport@shoprenter.hu) csapatánál kell kérni.

#### A Theme Editor elérése és az ezzel kapcsolatos általános tudnivalók:
A Theme Editor a **Sablon kiválasztása** oldalon érhető el a már kiválasztott sablonoknál a **Műveletek** gombra kattintva. Az itt megjelenő lenyílóban található **sablon fájlok testreszabása** menüpontban lehet szerkeszteni a template fájlokat.

A Theme Editor úgy van felépítve, hogy a bal oldalon találhatóak a sablonhoz tartozó template fájlok. Ide tartoznak a stíluslapok, illetve a konfigurációs fájlok is. Maga a HTML kódok .tpl kiterjesztésű template fájlokban érhetőek el. Ezekhez a fájlokhoz a [TWIG](https://twig.sensiolabs.org) template motort használja a ShopRenter.

#### Egyedi design készítése, Sablon másolás funkció:
Amennyiben új egyedi designt szeretnénk készíteni, akkor kattintsunk a Műveletek-en belül a **sablon másolása** gombra. A felugró ablakban adjuk meg a sablonunk nevét, majd kattintsunk a sablon másolása gombra. Ekkor létrejön az általunk meghatározott néven az új sablon aminél szintén a sablon fájlok testreszabása gombbal lehet elérni a Theme Editor-t.

#### style.scss
A sablonhoz tartozó stíluslap az **assets/style.scss** ami [SCSS](https://sass-lang.com) előfeldolgozóban íródott. A fájl tetején találhatóak a sablonhoz tartozó meta információk, mint például milyen framework-öt használ a sablon, vagy mi a sablon neve.

A **config/config.data.json** fájl írja le a sablon színváltozóit. Ez azt jelenti, hogy ebben a json objektumban definiált property-kből jönnek létre a Frontend-en, a CSS generálás alatt az SCSS változók. 
Például a body-background property fel van véve, tehát használható a style.scss -ben a $body-background változó.

#### Fájl kereső
A Theme Editor bal felső sarkában található a fájl kereső funkció. Az ide beírt kulcsszavak alapján lehet keresni template fájlokra, ezzel megkönnyítve a fájlok elérését. 

#### Verziók kezelése
Annak érdekében, hogy a Theme Editort használó fejlesztők biztonsággal tudják használni a fájlokat és kipróbálni a módosításokat, minden fájl verzió követés alatt áll. A megnyitott fájlok szerkesztő felülete fölött található egy Korábbi verziók link. 
Ide kattintva a megjelenő legördülő menüből ki lehet választani egy korábbi verziót vagy akár az eredeti is! Ha kiválasztottuk az általunk kívánt verziót a szerkesztő felületbe az adott verzió tartalma betöltődik, a mentés gombbal pedig már a korábbi tartalommal létrejött verzió lesz használatban.

#### Visszaállít gomb
A gomb funkcióját tekintve, külön mentés nélkül, azonnal visszaállítja az adott fáj eredeti verzióját!

#### Új fájl létrehozása
Template fájlokat tartalmazó mappában lehetőség nyílik új fájlok létrehozására is. 
Az új fájl nevénél nem kell megadni kiterjesztést. Ezeket az új fájlokat a Twig include függvényének segítségével lehet beszúrni egy adott template fájlba.

#### Billentyűkombinációk
A szerkesztő felületben a fejlesztői munka megkönnyítésére a következő billentyűkombinációkat érdemes használni:

- **CTRL + F / Command + G**

  Kulcsszó kereső. 
  A felugró keresőbe beírjuk a keresett kulcsszót, majd entert nyomunk.
  Több találat esetén F3-al / Command + G a következő találatra, míg a SHIFT + F3 / Command + Shift + G kombinációval az előző találatra tudunk ugrani.

- **CTRL + G / Control + G**

  A gomb lenyomása után felugró ablakban általunk megadott sorszámú sorához ugrik a rendszer a kódban.
  Például ha megadjuk, hogy 1200, akkor az 1200-adik sorhoz ugrik a rendszer rögtön.

- **F11**

  Az F11 gomb megnyomásával előhozható a Theme Editor full-screen módja, így a teljes képernyőt kihasználva, könnyebben módosíthatunk a kódban.
