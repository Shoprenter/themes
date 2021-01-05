## config.data.json

A téma konfigurálásához szükséges adatokat tartalmazza JSON formátumban. 

Két részből áll, az assets és a presets részből:

```json
{
    "assets": {},
    "presets": {}
}
```

### assets

Az assets objektumban vannak nyilvántartva a témához tartozó képek. Ezek például a bélyegképek, amelyek az admin 
felületen megjelennek a téma kiválasztás oldalon. 


Példa:

```json
{
    "assets": {
        "base": {
            "thumb": "tokyo.jpg"
        },
        "blue": {
            "thumb_small": "tokyo_blue.jpg",
            "thumb_big": "tokyo_blue_n.jpg"
        },
        "green": {
            "thumb_small": "tokyo_green.jpg",
            "thumb_big": "tokyo_green_n.jpg"
        }
    }
}
```

### presets

A presets objektumban találhatóak a témához tartozó szín változók. Ezen változók alapján jön létre
 a **Téma testreszabás** oldalon a **Téma színek** fül tartalma. 
 
Minden változóhoz tartozik egy ColorPicker vagy BackgroundPicker.
 Ha a változó nevében a postfix color, akkor ColorPicker jelenik meg, ha a postfix background akkor BackgroundPicker, 
 ha pedig egyik sem, akkor egy text típusú input mező. A ColorPicker-nél egy színt lehet csak kiválasztani, míg a 
 BackgroundPicker segítségével színátmeneteket is lehet megadni. 
 
<table>
  <tr>
    <th>Postfix</th>
    <th>Megjelenés</th>
  </tr> 
  <tr>
    <td>-color</td>
    <td>ColorPicker</td>
  </tr>
  <tr>
    <td>-background</td>
    <td>BackgroundPicker</td>
  </tr>
  <tr>
    <td>egyik sem</td>
    <td>input['type=text']</td>
  </tr>
</table>   
 
 **FONTOS: background postfix-es SCSS változót nem 
 szabad linear-gradient közé rakni, mert hibát okoz és a css fájl nem tud létrejönni!**
 

Példa:

```json
{
    "presets": {
        "base": {
            "cart-icon": "50",
            "global-border-radius": "0",
            "body-background": "#ffffff",
            "global-font-color": "#8e8e8e",
            "global-font-light-color": "#ffffff",
            "global-light-color": "#ffffff",
            "global-dark-color": "#2d2d2d",
            "input-border-color": "#ebebeb",
            "input-color": "#2d2d2d",
            "input-background": "#ffffff"
        },
        "blue": {
            "global-color": "#3da0e3",
            "global-hover-color": "#2487CA",
            "link-color": "#3da0e3",
            "link-hover-color": "#2487CA"
        },
        "green": {
            "global-color": "#68a742",
            "global-hover-color": "#4F8E29",
            "link-color": "#68a742",
            "link-hover-color": "#4F8E29"
        },
        "custom": {
            "global-color": "#ffcc00"
        }
  }
}
```

A presets objektum közvetlen gyerek elemei a különböző színváltozatokat tartalmazza, illetve a **base** és a **custom** 
objektumok speciális foglalt nevek. A **base** objektum tartalmazza az alap változókat, amik minden színváltozatnál megegyeznek, 
a példában a **blue** és a **green** változatok pedig azokat a változókat tartalmazza amikben a színváltozat eltér. 
Az assets és a presets objektumban ugyanazok a változatok szerepelnek. Ha az admin felhasználó a Téma színek 
oldalon módosítja a színeket akkor a módosult érték a **custom** objektumban kerül eltárolásra.

**Téma másolás** használatakor a színváltozatok (blue, green) nem kerülnek átmásolásra, a lemásolt téma, ha például 
a blue volt, akkor a blue objektum változói átkerülnek a **base** objektumon belülre.

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
