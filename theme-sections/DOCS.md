# Tartalomjegyzék
* [A dinamikus modulok manuális és pozícióban való használata](#a-dinamikus-modulok-manuális-és-pozícióban-való-használata)
    * [Manuális használat](#manuális-használat)
    * [Pozícióban való használat](#pozícióban-való-használat)
* [Dinamikus modulok schema használata](#dinamikus-modulok-schema-használata)
    * [attributes](#attributes)
    * [settings](#settings)
    * [blocks](#blocks)
        * [Speciális field-ek a Blocks settings objektumon belül](#speciális-field-ek-a-blocks-settings-objektumon-belül)
    * [presets](#presets)
* [Input típusok](#input-típusok)
    * [Egysoros szöveg beviteli mező](#egysoros-szöveg-beviteli-mező)
    * [Többsoros szöveg beviteli mező](#többsoros-szöveg-beviteli-mező)
    * [HTML Editor](#html-editor)
    * [Képfeltöltés](#képfeltöltés)
    * [Legördülő menü (select)](#legördülő-menü-select)
    * [Checkbox](#checkbox)
    * [Szám beviteli mező](#szám-beviteli-mező)
    * [Pozíció típus](#pozíció-típus)
    * [Dátum idővel](#dátum-idővel)
* [Többnyelvűség használata](#többnyelvűség-használata)

# Dinamikus modulok
A Téma fájl szerkesztőben található **sections** mappában találhatóak a dinamikus modulok.
A dinamikus modulok olyan modulok, amelyekben egyszerre tudjuk a megjelenést (frontend) és a
beállításokat (admin felület) kezelni.

A dinamikus modulokon kívül létrehozott változók a dinamikus modulokon belül nem elérhetőek.
Ehhez hasonlóan, a dinamikus modulokon belül létrehozott változók a dinamikus modulokon kívül nem elérhetőek.

A dinamikus modulokban egy új tag is használható:

``` {% schema %}```

A schema tag-ről bővebben a későbbiekben fogunk írni.

## A dinamikus modulok manuális és pozícióban való használata
A dinamikus modulok a téma fájlokhoz hozzáadhatóak manuálisan, vagy modul pozíciókon keresztül dinamikusan is.

### Manuális használat
Dinamikus modult a téma fájlhoz a **viewHelper.loadModule()** függvény segítségével lehet beilleszteni. Például:

```{{ viewHelper.loadModule('sections/my-first-section') }}```

_A fenti példa szerint beillesztett modult a későbbiekben "manuálisan beillesztett modulnak" hívjuk._

A dinamikus modul több téma fájlba is beilleszthető, ugyanakkor az adott dinamikus modul csak egyszer fordulhat elő
a modulok között, a modul lista oldalon. Ha egy manuálisan beillesztett modulnak módosítjuk a konfigurációját,
a módosítás minden olyan helyen érvényes lesz, ahol az adott modul megtalálható. Fontos megjegyezni, hogy a
dinamikus modulok nem tartalmazhatnak más dinamikus modulokat. Manuális használat esetén elegendő egy
**name** tulajdonság megadása az _attributes_ objektumban.

### Pozícióban való használat
A dinamikus modulokat pozíciókban is el lehet helyezni a **viewHelper.loadPosition()** függvény segítségével. Például:

```{{ viewHelper.loadPosition('home') }}```

Ez azt jelenti, hogy a ShopRenter minden olyan modult megjelenít aminek a pozíciója: _home_.
A pozíciókat a Téma fájl szerkesztőben a **settings.json** fájl **positions** objektumában lehet meghatározni.

:red_circle: **FONTOS**: Ha szeretnénk új pozíciókat létrehozni ide kell felvenni őket.
A dinamikus modulok pozícióját az adminisztrátorok az admin felületen belül a dinamikus modul szerkesztés oldalán
tudják megváltoztatni. A dinamikus moduloknak lennie kell egy **position**, **status** és **sort_order** beállítása a
schema settings objektumán belül különben nem fog megjelenni a modul.

## Dinamikus modulok schema használata
A dinamikus modulok sémája a Twig **{% schema %}** tag-ben van definiálva. A twig comment tag-hez hasonlóan
a sémának nincs kimenete és a schema tag-ben lévő Twig kód nem kerül lefuttatásra.

Minden dinamikus modulban egy schema tag lehet, aminek valid JSON-t kell tartalmaznia.
A schema tag a dinamikus modul téma fájlban bárhol elhelyezhető, kivéve egy másik Twig tag-en belül.

A dinamikus modul schema tag-n belül a következő tulajdonságok határozhatók meg:

attributes
settings
blocks

### attributes
Dinamikus modul schema-jában lennie kell egy **attributes** objektumnak és egy **name** tulajdonságnak.
Van lehetőség egy **helpLink** tulajdonság megadására is, aminek egy url-t kell megadni.
Erre az url-re fog a Segítség gomb mutatni.

Például:

```
{% schema %}
{
    "attributes": {
         "name": "My first section",
         "helpLink": {
              "en": "https://www.shoprenter.hu/blog/1",
              "hu": "https://www.shoprenter.hu/blog/1"
         }
    }
}
{% endschema %}
```

### settings
A dinamikus modulnak a saját beállításai vannak a settings objektumon belül.
A beállításokat arra lehet használni, hogy egy admin felületet biztosítsunk a dinamikus modul szerkesztő felületéhez.
A felhasználó az admin felületen a modul listából érheti el a dinamikus modult a nevére vagy a szerkesztés gombra
kattintva.

Például:

```
{% schema %}
    {
        "attributes": {
            "name": "My first section"
        },
        "settings": [
            {
                "type": "text",
                "name": "title",
                "label": "Title",
                "default": "My first section title"
            }
        ]
    }
{% endschema %}
```

A dinamikus modul settings objektumán belül a **name** tulajdonságnak egyedinek kell lennie az adott
dinamikus modulon belül, de globálisan nem szükséges, hogy egyedi legyen. Az alulvonás karakteren (_) kívül
semmilyen speciális karakter nem használható a name tulajdonságon értékénél.

A beállítás értékét a html-ben a **section Object**-en keresztül lehet elérni, például:

```{{ section.settings.name }}```

### blocks
Vannak olyan esetek amikor bizonyos beállításokból dinamikusan szeretnénk létrehozni több változatot, erre szolgálnak a blokkok.
Ezeket a változatokat a felhasználó tudja dinamikusan létrehozni, anélkül, hogy a programozónak újabb beállításokat/"beállítás csoportokat" kelljen hozzá adnia.

Például:

```
{% schema %}
    {
        "attributes": {},
        "settings": [],
        "blocks": [
            {
                "type": "image",
                "name": "Image",
                "settings": [
                    {
                        "type": "text",
                        "name": "title",
                        "label": "Title of image",
                        "default": ""
                    },
                    {
                        "type": "image",
                        "name": "image",
                        "label": "Image",
                        "default": "no_image.jpg"
                    }
                ]
            }
        ]
    }
{% endschema %}
```

A blocks objektumon belül, a **type** tulajdonságnak megadhatunk bármit, de ennek mindenképp egyedinek kell lennie, mivel ez azonosítja be az adott blokkot. A **type** csak a latin abc kis betűit és számokat tartalmazhat, valamint alulvonás karaktert (_).
A **name** tulajdonság az admin felületen megjelenő **type** felhasználóbarát neve.
A blokkon belüli **settings** objektum ugyanúgy funkciónál, mint ahogy a már fent említett **settings** objektum.

A blocks objektumot a html-ben a **section Object**-en keresztül lehet elérni, például:

```{{ section.blocks }}```

Egy példa a blocks iterációra a Twig html-en belül:

```
<ul class="list">
{% for block in section.blocks %}
    {% if block.type == 'image' %}
    <li>{{ block.settings.title }}</li>
    {% endif %}
{% endfor %}
</ul>
```

#### Speciális field-ek a Blocks settings objektumon belül
Az admin felületen, a "Tartalom" lista megjelenését befolyásoló különleges **name** tulajdonság értékek.

<table>
<tr>
  <th>name</th>
  <th>purpose</th>
</tr>
<tr>
  <td>status</td>
  <td>A listában láthatóvá válik az adott block példány státusza</td>
</tr>
<tr>
  <td>title</td>
  <td>A listában megjelenő adott block példány neve</td>
</tr>
<tr>
  <td>image</td>
  <td>A listában megjelenő adott block példány képe</td>
</tr>
</table>

### presets

Ha egy mező értékét szeretnénk előre meghatározni, a "presets" nevű property lesz segítségünkre. Ebben az objektumban tudjuk meghatározni egy-egy mező értékét.

Vegyük például a fentebb létrehozott blokkot:
```
"presets": {
    "blocks": [
        {
            "type": "image",
            "settings": {
                "title": "Lorem ipsum",
                "image": "no_image.jpg"
            }
        }
    ]
}
```

Ezzel gyakorlatilag annyit tettünk, hogy előre létrehoztunk egy blokkot a modulunkban.

**Fontos!** Ha egy mező többnyelvű, azaz a beállításai közt meghatározzuk, hogy:

```
"multilang": true
```

Ebben az esetben nem string-ként kell a presets mező értékét meghatározni, hanem language code alapján, objektumként.

Példa:

**Blokk schema:**

```
{% schema %}
    {
        "attributes": {},
        "settings": [],
        "blocks": [
            {
                "type": "image",
                "name": "Image",
                "settings": [
                    {
                        "type": "text",
                        "name": "title",
                        "multilang": true
                        "label": {
                            "hu": "Cím",
                            "en": "Title of something"
                        },
                        "default": ""
                    },
                    {
                        "type": "image",
                        "name": "image",
                        "label": "Image",
                        "default": "no_image.jpg"
                    }
                ]
            }
        ]
    }
{% endschema %}
```

**Helyes presets meghatározás:**

```
"presets": {
    "blocks": [
        {
            "type": "image",
            "settings": {
                "title": {
                    "hu": "Magyar preset érték",
                    "en": "Preset value in english"
                },
                "image": "no_image.jpg"
            }
        }
    ]
}
```

**Ha egy többnyelvű mező presets értéke nem objektum, váratlan hibaüzenetet fogunk kapni a modul mentése során. Ugyanez igaz a fordított esetre is, azaz egy NEM többnyelvű mező preset értéke nem lehet objektum, különben hibát fogunk kapni a mentés alkalmával.**

## Input típusok
Jelenleg elérhető input típusok:

text  
textarea  
htmleditor  
image  
select  
checkbox  
number  
position  
datetime

### Egysoros szöveg beviteli mező
Az egysoros szöveg beviteli mezőt akkor használhatjuk, ha rövid szöveget szeretnénk megjeleníteni,
mint például egy cím vagy egy név.

Input

<table>
<tr>
  <th>field</th>
  <th>value</th>
  <th>required</th>
</tr>
<tr>
  <td>type</td>
  <td>"text"</td>
  <td>required</td>
</tr>
<tr>
  <td>name</td>
  <td>Text (abcABC_)</td>
  <td>required</td>
</tr>
<tr>
  <td>label</td>
  <td>Text | Object</td>
  <td>required</td>
</tr>
<tr>
  <td>default</td>
  <td>Text | Object</td>
  <td>required</td>
</tr>
<tr>
  <td>help</td>
  <td>Text | Object</td>
  <td></td>
</tr>
<tr>
  <td>multilang</td>
  <td>Boolean</td>
  <td></td>
</tr>
<tr>
  <td>required</td>
  <td>Boolean</td>
  <td></td>
</tr>
</table>

Példa:
```
{
    "type": "text",
    "name": "title",
    "label": "Title",
    "default": "My first section title",
    "help": "This is my first section title.",
    "multilang": true
}
```

### Többsoros szöveg beviteli mező
A többsoros szöveg beviteli mezőt akkor használhatjuk, ha hosszabb szöveget vagy HTML kódot szeretnénk megjeleníteni.

Input

<table>
<tr>
  <th>field</th>
  <th>value</th>
  <th>required</th>
</tr>
<tr>
  <td>type</td>
  <td>"textarea"</td>
  <td>required</td>
</tr>
<tr>
  <td>name</td>
  <td>Text (abcABC_)</td>
  <td>required</td>
</tr>
<tr>
  <td>label</td>
  <td>Text | Object</td>
  <td>required</td>
</tr>
<tr>
  <td>default</td>
  <td>Text | Object</td>
  <td>required</td>
</tr>
<tr>
  <td>help</td>
  <td>Text | Object</td>
  <td></td>
</tr>
<tr>
  <td>multilang</td>
  <td>Boolean</td>
  <td></td>
</tr>
<tr>
  <td>required</td>
  <td>Boolean</td>
  <td></td>
</tr>
</table>

Példa:
```
{
    "type": "textarea",
    "name": "description",
    "label": "Description",
    "default": "My first section description",
    "help": "This is my first section description.",
    "multilang": true
}
```

### HTML Editor
A HTML editor-t, hasonlóan a többsoros szöveg beviteli mezőhöz, használhatjuk hosszabb szövegek vagy HTML kód szerkesztéséhez.

Input

<table>
<tr>
  <th>field</th>
  <th>value</th>
  <th>required</th>
</tr>
<tr>
  <td>type</td>
  <td>"htmleditor"</td>
  <td>required</td>
</tr>
<tr>
  <td>name</td>
  <td>Text (abcABC_)</td>
  <td>required</td>
</tr>
<tr>
  <td>label</td>
  <td>Text | Object</td>
  <td>required</td>
</tr>
<tr>
  <td>default</td>
  <td>Text | Object</td>
  <td>required</td>
</tr>
<tr>
  <td>help</td>
  <td>Text | Object</td>
  <td></td>
</tr>
<tr>
  <td>multilang</td>
  <td>Boolean</td>
  <td></td>
</tr>
<tr>
  <td>required</td>
  <td>Boolean</td>
  <td></td>
</tr>
</table>

Példa:
```
{
    "type": "htmleditor",
    "name": "description",
    "label": "Description",
    "default": "My first section description",
    "help": "This is my first section description.",
    "multilang": true
}
```

### Képfeltöltés
A képfeltöltésre szolgáló mező az image típusú mező, ahol a képek, fav ikonok, vagy a  slideshow-khoz használt képeket lehet feltölteni.

Input

<table>
<tr>
  <th>field</th>
  <th>value</th>
  <th>required</th>
</tr>
<tr>
  <td>type</td>
  <td>"image"</td>
  <td>required</td>
</tr>
<tr>
  <td>name</td>
  <td>Text (abcABC_)</td>
  <td>required</td>
</tr>
<tr>
  <td>label</td>
  <td>Text | Object</td>
  <td>required</td>
</tr>
<tr>
  <td>default</td>
  <td>Text | Object</td>
  <td>required</td>
</tr>
<tr>
  <td>help</td>
  <td>Text | Object</td>
  <td></td>
</tr>
<tr>
  <td>multilang</td>
  <td>Boolean</td>
  <td></td>
</tr>
<tr>
  <td>required</td>
  <td>Boolean</td>
  <td></td>
</tr>
</table>

Példa:
```
{
    "type": "image",
    "name": "image",
    "label": "Image",
    "default": "My first section image",
    "help": "This is my first section image.",
    "multilang": true
}
```

A Twig templateben az [asset_image_url](https://github.com/Shoprenter/themes/blob/master/theme-global/GLOBAL_FUNCTIONS.md#asset_image_url) függvényt célszerű használni a képfájl megjelenítésére.

### Legördülő menü (select)
A legördülő típus arra használható, hogy a felhasználónak különböző lehetőségeket mutassunk.
Például, kiválaszthatja azokat a termékeket, amelyeket a termék oldalon szeretne megjeleníteni.

A legördülő menünek meg kell adni egy **options** objektumot, ahol felsoroljuk a kiválasztható értékeket.

Input

<table>
<tr>
  <th>field</th>
  <th>value</th>
  <th>required</th>
</tr>
<tr>
  <td>type</td>
  <td>"select"</td>
  <td>required</td>
</tr>
<tr>
  <td>name</td>
  <td>Text (abcABC_)</td>
  <td>required</td>
</tr>
<tr>
  <td>label</td>
  <td>Text | Object</td>
  <td>required</td>
</tr>
<tr>
  <td>default</td>
  <td>Text | Object</td>
  <td>required</td>
</tr>
<tr>
  <td>help</td>
  <td>Text | Object</td>
  <td></td>
</tr>
<tr>
  <td>multilang</td>
  <td>Boolean</td>
  <td></td>
</tr>
<tr>
  <td>required</td>
  <td>Boolean</td>
  <td></td>
</tr>
<tr>
  <td>options</td>
  <td>Object</td>
  <td>required</td>
</tr>
</table>

Példa:
```
{
    "type": "select",
    "name": "columns",
    "label": "Columns",
    "default": "2",
    "help": "This is my first section select.",
    "options": [
        {
            "value": "1",
            "label": "one column"
        },
        {
            "value": "2",
            "label": "two columns"
        },
        {
            "value": "3",
            "label": "three columns"
        }
    ],
    "multilang": true
}
```

### Checkbox
A checkbox típus segítségével beállítások ki-be kapcsolását végezhetjük el, például, az oldalon lévő
elemek megjelenítésére vagy elrejtésére.

Input

<table>
<tr>
  <th>field</th>
  <th>value</th>
  <th>required</th>
</tr>
<tr>
  <td>type</td>
  <td>"checkbox"</td>
  <td>required</td>
</tr>
<tr>
  <td>name</td>
  <td>Text (abcABC_)</td>
  <td>required</td>
</tr>
<tr>
  <td>label</td>
  <td>Text | Object</td>
  <td>required</td>
</tr>
<tr>
  <td>default</td>
  <td>Boolean | Object</td>
  <td>required</td>
</tr>
<tr>
  <td>help</td>
  <td>Text | Object</td>
  <td></td>
</tr>
<tr>
  <td>multilang</td>
  <td>Boolean</td>
  <td></td>
</tr>
<tr>
  <td>required</td>
  <td>Boolean</td>
  <td></td>
</tr>
</table>

Példa:
```
{
    "type": "checkbox",
    "name": "show_element",
    "label": "Show element",
    "default": true,
    "help": "This is my first section checkbox.",
    "multilang": true
}
```

### Szám beviteli mező
A number típusú beviteli mező csak szám karakterek beolvasására alkalmas.

Input

<table>
<tr>
  <th>field</th>
  <th>value</th>
  <th>required</th>
</tr>
<tr>
  <td>type</td>
  <td>"number"</td>
  <td>required</td>
</tr>
<tr>
  <td>name</td>
  <td>Text (abcABC_)</td>
  <td>required</td>
</tr>
<tr>
  <td>label</td>
  <td>Text | Object</td>
  <td>required</td>
</tr>
<tr>
  <td>default</td>
  <td>Number | Object</td>
  <td>required</td>
</tr>
<tr>
  <td>help</td>
  <td>Text | Object</td>
  <td></td>
</tr>
<tr>
  <td>multilang</td>
  <td>Boolean</td>
  <td></td>
</tr>
<tr>
  <td>required</td>
  <td>Boolean</td>
  <td></td>
</tr>
</table>

Példa:
```
{
    "type": "number",
    "name": "number",
    "label": "Number",
    "default": 156,
    "help": "This is my first section number.",
    "multilang": true
}
```

### Pozíció típus
Ha a típust position-re állítjuk, arra használhatjuk, hogy egy legördülő menüből ki tudjuk választani a
rendszer pozíciókat. A különbség a legördülő menü és a position típus között az, hogy a position típusnak
előre meghatározott értékei vannak. Ezek az előre meghatározott értékek a **settings.json** fájl positions
objektumában vannak definiálva.

Input

<table>
<tr>
  <th>field</th>
  <th>value</th>
  <th>required</th>
</tr>
<tr>
  <td>type</td>
  <td>"position"</td>
  <td>required</td>
</tr>
<tr>
  <td>name</td>
  <td>Text (abcABC_)</td>
  <td>required</td>
</tr>
<tr>
  <td>label</td>
  <td>Text | Object</td>
  <td>required</td>
</tr>
<tr>
  <td>default</td>
  <td>Text | Object</td>
  <td>required</td>
</tr>
<tr>
  <td>help</td>
  <td>Text | Object</td>
  <td></td>
</tr>
<tr>
  <td>required</td>
  <td>Boolean</td>
  <td></td>
</tr>
</table>

Példa:
```
{
    "type": "position",
    "name": "position",
    "label": "Position",
    "default": "home",
    "help": "You can change sections position ."
}
```

### Dátum idővel

A `datetime` segítségével egy felhasználóbarát, dátum- és időválasztó felülettel elátott inputot tudunk elhelyezni.  
A `default` mezőben üres karakterláncot, vagy `YYYY-MM-DD HH:mm:ss` formátumú dátumot lehet megadni. (pl.: `"default": ""`
vagy `"default": "2019-01-01 12:00:00"`)  
Mentéskor is ilyen formátumú értékek kerülnek tárolásra.  
Amennyiben nem kötelező a mező (`"required": false`), megjelnik egy törlés gomb is.

Input

|field|value|required|
|:---:|:---|:---:|
|type|`"datetime"`|required|
|name|Text (abcABC_)|required|
|label|Text &#124; Object|required|
|default|Text (`""` &#124; `"2019-01-01 12:00:00"`)|required|
|help|Text &#124; Object||
|required|boolean||


Példa:
```json
{
    "type": "datetime",
    "name": "a_datetime_field",
    "label": {
      "hu": "Válasszon egy időpontot",
      "en": "Select a date and time"
    },
    "default": "2019-01-01 12:00:00",  
    "help": {
      "hu": "Segítség szöveg magyarul",
      "en": "Help text in English"
    }
}
```

**Vigyázat!**  
Twig templateben a `{{ ""|date('Y-m-d H:i:s') }}` az aktuális dátumot és időt fogja visszaadni.  
Tehát ha a mező nem kötelező és üres értéket mentünk el, akkor ezt külön kezelnünk kell!

## Többnyelvűség használata
Többnyelvűség esetén a **multilang** tulajdonságot true értékre kell állítani egy mező esetén.
A label, default, help és options tulajdonságok esetén van lehetőség többnyelvű értékek felvételére.

Példa:
```
{
    "type": "select",
    "name": "status",
    "label": {
      "hu": "Állapot",
      "en": "Status"
    },
    "default": "0",  
    "help": {
      "hu": "Segítség szöveg magyarul",
      "en": "Help text in English"
    },
    "options": [
        {
            "value": "0",
            "label": {
                "hu": "Letiltott",
                "en": "Disabled"
            }
        },
            {
            "value": "1",
            "label": {
                "hu": "Engedélyezett",
                "en": "Enabled"
            }
        }
    ]
}
```
