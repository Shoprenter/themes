## Tartalomjegyzék
* [ShopRenter Twig függvények](#shoprenter-twig-függvények)
  * [asset](#asset)
  * [asset_image_url](#asset_image_url)
  * [viewHelper.loadModule](#viewhelperloadmodule)
  * [viewHelper.loadPosition](#viewhelperloadposition)
  * [viewHelper.isPositionEmpty](#viewhelperispositionempty)
  * [isDeviceType](#isdevicetype)
  * [config.get](#configget)
* [ShopRenter Twig filterek](#shoprenter-twig-filterek)

## Globális függvények

A sablon fájlokhoz a [TWIG](https://twig.sensiolabs.org) template motort használja a ShopRenter. Így minden tpl 
kiterjesztésű fájl TWIG szintaxisra épül. A TWIG dokumentációban látható alap függvények és filterek mellett vannak a 
ShopRenter által biztosított függvények és filterek is. Ezek azért készültek, hogy a frontend fejlesztést megkönnyítsék.

### ShopRenter TWIG függvények

* #### asset()

Az asset függvény az elérési útvonalat kiegészíti a cdn-es domain névvel és verziót is kap.

##### Szintaxis

```
asset(filePath)
```

##### Argumentumok

**filePath**: A fájl útvonala a gyökér könyvtártól.

##### Visszatérési érték

A teljes url -el tér vissza, string típusként.

Példa:

```twig
<script src="{{ asset('catalog/view/javascript/filter/mobile_filter.js') }}"></script>
```

Kimenet:

```
<script src="https://boltnev.cdn.shoprenter.hu/catalog/view/javascript/filter/mobile_filter.js?v=12345678"></script>
```


* #### asset_image_url()

Az asset_image_url függvénynek a kép fájl nevét kell csak megadni és kiegészíti a cdn-es domain névvel és verziót is kap.
Célszerű a [dinamikus modulok](../theme-sections/DOCS.md) képfeltöltésénél használni.

##### Szintaxis

```
asset_image_url(fileName)
```

##### Argumentumok

**fileName**: A fájl neve amit szeretnénk teljes url-el megjeleníteni.

##### Visszatérési érték

A teljes url -el tér vissza, string típusként.

Példa:

```twig
<img src="{{ asset_image_url('kepfajl.jpg') }}" />
```

Kimenet:

```html
<img src="https://boltnev.cdn.shoprenter.hu/custom/boltnev/image/kepfajl.jpg?v=12345678" />
```


* #### viewHelper.loadModule()

Modul vagy dinamikus modul beillesztésére szolgál. Ha egy modul hozzá van rendelve pozícióhoz és a viewHelper.loadModule
segítségével is behúzzuk, akkor mind a pozícióban, mind azon a helyen ahol meghívtuk meg fog jelenni a modul. Azok a 
modulok jelennek meg ilyenkor amiknél a státusz engedélyezett.  

##### Szintaxis

```
viewHelper.loadModule(moduleRoute)
```

##### Argumentumok

**moduleRoute**: A modul elérés, ahogy az admin felületen is hivatkozunk rá.

##### Visszatérési érték

A loadModule a hivatkozott modul html kódjával tér vissza.

Példa 1:

```twig
{{ viewHelper.loadModule('module/logo') }}
```

Kimenet 1:

```html
<div id="logo" class="module content-module header-position logo-module logo-text hide-top">
    <a href="/">Tokyo</a>
</div>
```

Példa 2:

```twig
{{ viewHelper.loadModule('sections/section-name') }}
```

Kimenet 2:

```html
<div id="section-section-name" class="section-wrapper module-editable">
    ...
</div>
```


* #### viewHelper.loadPosition() 

Az adott pozícióban megjelenő összes modult jeleníti meg, amik engedélyezett állapotra vannak állítva. A pozíciók 
amik a sablonban definiálva vannak a **config/settings.json** fájlban találhatóak.

##### Szintaxis

```
viewHelper.loadPosition(positionName)
```

##### Argumentumok

**positionName**: A pozíció nevét kell megadni.

##### Visszatérési érték

Minden modul htmljét visszaadja ami az adott pozícióban megjelenhet.

Példa:

```twig
{{ viewHelper.loadPosition('home') }}
```

Kimenet:

```html
<div id="section-section-name" class="section-wrapper module-editable">...</div>
<div id="module_news_wrapper" class="module-news-wrapper">...</div>
<div id="module_latest_wrapper" class="module-latest-wrapper">...</div>
<div id="module_kickerimage_wrapper" class="module-kickerimage-wrapper">...</div>
```


* #### viewHelper.isPositionEmpty()

Azt vizsgálja, hogy egy adott pozícióhoz van-e modul hozzárendelve.

##### Szintaxis

```
viewHelper.loadPosition(positionName)
```

##### Argumentumok

**positionName**: A pozíció nevét kell megadni.

##### Visszatérési érték

Boolean értékkel tér vissza: true vagy false.

Példa:

```twig
{% if viewHelper.isPositionEmpty('home') %}
    {{ viewHelper.loadPosition('home') }}
{% endif %}
```

Kimenet:

```html
<div id="section-section-name" class="section-wrapper module-editable">...</div>
<div id="module_news_wrapper" class="module-news-wrapper">...</div>
<div id="module_latest_wrapper" class="module-latest-wrapper">...</div>
<div id="module_kickerimage_wrapper" class="module-kickerimage-wrapper">...</div>
```


* #### isDeviceType()

Ha szeretnénk mobil eszközön más megjelenést készíteni és már a html kódot se szeretnénk megjeleníteni, akkor az 
isDeviceType függvény segítségével lehet feltételeket készíteni a templateben.

##### Szintaxis

```
isDeviceType(deviceType)
```

##### Argumentumok

**deviceType**: A device nevét kell megadni. Lehetséges értékek: mobile, tablet, desktop.

##### Visszatérési érték

Boolean értékkel tér vissza: true vagy false.


Példa:

```twig
{% if isDeviceType('desktop') %}
    Csak asztalon jelenik meg
{% else if isDeviceType('desktop') %}
    <div>[-.-]</div>
{% endif %}
```

Kimenet ha PC-ről nézzük:

```html
Csak asztalon jelenik meg
```

Kimenet ha Mobiltelefonról nézzük:

```html
<div>[-.-]</div>
```


* #### config.get()

A beállítások kikérésére szolgáló függvény.

##### Szintaxis

```
config.get(configName)
```

##### Argumentumok

**configName**: A beállítás neve

##### Visszatérési érték

Visszatérhet több típussal is, lehet String, Boolean vagy Integer is.


Példa:

```twig
{% if config.get('config_show_header_phone') == 1 %}
    <div class="header-phone">{{ phone }}</div>
{% endif %}
```

Kimenet:

```html
<div class="header-phone">06-30-123-4567</div>
```
### ShopRenter TWIG filterek

