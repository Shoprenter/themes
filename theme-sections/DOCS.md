## Tartalomjegyzék
* [A dinamikus modulok manuális és pozícióban való használata](#a_dinamikus_modulok_manuális_és_pozícióban_való_használata)
  * [Manuális használat](#manuális_használat)
  * [Pozícióban való használat](#pozícióban_való_használat)
* [Dinamikus modulok schema használata](#dinamikus_modulok_schema_használata)
  * [attributes](#attributes)
  * [settings](#settings)
  * [blocks](#blocks)
    * [Speciális field-ek a Blocks settings objektumon belül](#speciális_field-ek_a_blocks_settings_objektumon_belül)
* [Input típusok](#input_típusok)
  * [Egysoros szöveg beviteli mező](#egysoros_szöveg_beviteli_mező)
  * [Képfeltöltés](#képfeltöltés)
  * [Legördülő menü (select)](#legördülő_menü_(select))
  * [Checkbox](#checkbox)
  * [Szám beviteli mező](#szám_beviteli_mező)
  * [Pozíció típusa](#pozíció_típusa)
* [Többnyelvűség használata](#többnyelvűség_használata)

## Dinamikus modulok

A Sablon fájl szerkesztőben található **sections** mappában találhatóak a dinamikus modulok.
A dinamikus modulok olyan modulok, amelyekben egyszerre tudjuk a megjelenést (frontend) és a 
beállításokat (admin felület) kezelni.

A dinamikus modulokon kívül létrehozott változók a dinamikus modulokon belül nem elérhetőek. 
Ehhez hasonlóan, a dinamikus modulokon belül létrehozott változók a dinamikus modulokon kívül nem elérhetőek.

A dinamikus modulokban egy új tag is használható:

``` {% schema %}```

A schema tag-ről bővebben a későbbiekben fogunk írni.

### A dinamikus modulok manuális és pozícióban való használata

A dinamikus modulok a sablon fájlokhoz hozzáadhatóak manuálisan, vagy modul pozíciókon keresztül dinamikusan is. 

#### Manuális használat

Dinamikus modult a sablon fájlhoz a **viewHelper.loadModule()** függvény segítségével lehet beilleszteni. Például:

```{{ viewHelper.loadModule('sections/my-first-section') }}```

_A fenti példa szerint beillesztett modult a későbbiekben "manuálisan beillesztett modulnak" hívjuk._

A dinamikus modul több sablon fájlba is beilleszthető, ugyanakkor az adott dinamikus modul csak egyszer fordulhat elő
 a modulok között, a modul lista oldalon. Ha egy manuálisan beillesztett modulnak módosítjuk a konfigurációját, 
 a módosítás minden olyan helyen érvényes lesz, ahol az adott modul megtalálható. Fontos megjegyezni, hogy a 
 dinamikus modulok nem tartalmazhatnak más dinamikus modulokat. Manuális használat esetén elegendő egy 
 **name** tulajdonság megadása az _attributes_ objektumban. 
 
#### Pozícióban való használat

A dinamikus modulokat pozíciókban is el lehet helyezni a **viewHelper.loadPosition()** függvény segítségével. Például:

```{{ viewHelper.loadPosition('home') }}```

Ez azt jelenti, hogy a ShopRenter minden olyan modult megjelenít aminek a pozíciója: _home_. 
A pozíciókat a Sablon fájl szerkesztőben a **settings.json** fájl **positions** objektumában lehet meghatározni.

**Fontos**: Ha szeretnénk új pozíciókat létrehozni ide kell felvenni őket. 
A dinamikus modulok pozícióját az adminisztrátorok az admin felületen belül a dinamikus modul szerkesztés oldalán
tudják megváltoztatni. A dinamikus moduloknak lennie kell egy **position**, **status** és **sort_order** beállítása a 
schema settings objektumán belül különben nem fog megjelenni a modul. 

### Dinamikus modulok schema használata

A dinamikus modulok sémája a Twig **{% schema %}** tag-ben van definiálva. A twig comment tag-hez hasonlóan 
a sémának nincs kimenete és a schema tag-ben lévő Twig kód nem kerül lefuttatásra. 

Minden dinamikus modulban egy schema tag lehet, aminek valid JSON-t kell tartalmaznia. 
A schema tag a dinamikus modul sablon fájlján belül bárhol elhelyezhető, kivéve egy másik Twig tag-en belül. 

A dinamikus modul schema tag-n belül a következő tulajdonságok határozhatók meg:

attributes
settings
blocks

#### attributes

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

#### settings

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

#### blocks

Vannak olyan esetek amikor bizonyos beállításokból dinamikusan szeretnénk létrehozni több változatot, erre szolgálnak a blokkok. 
Ezeket a változatokat a felhasználó tudja dinamikusan létrehozni, anélkül, hogy a programozónak újabb beállításokat/"beállítás csoportokat" kelljen hozzá adnia.

Például:

```
{% schema %}
    {
        "attributes": {},
        "settings": [],
        "blocks": [
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

##### Speciális field-ek a Blocks settings objektumon belül
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

### Input típusok

Jelenleg elérhető input típusok: 

text
image
select
checkbox
number
position

#### Egysoros szöveg beviteli mező

Az egysoros szöveg beviteli mezőt akkor használhatjuk, ha rövid szöveget szeretnénk megjeleníteni, 
mint például egy cím vagy egy név.

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
  <td>Text | Number | Boolean | Object</td>
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

Példa egy szöveges beviteli mező objektumára:

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

#### Képfeltöltés

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

#### Legördülő menü (select)

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

#### Checkbox

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

Például: 

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

#### Szám beviteli mező

A number típusú beviteli mező csak szám karakterek beolvasására alkalmas. 


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

#### Pozíció típusa

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

### Többnyelvűség használata

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
