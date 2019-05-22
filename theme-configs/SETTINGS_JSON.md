## Tartalomjegyzék
* [layouts](#layouts)
* [positions](#positions)

## settings.json

Az adott sablonhoz tartozó sablon beállításokat tartalmazza, mint például a layout-ok és pozíciók.

```json
{
    "layouts": {},
    "positions": {}
}
```

### layouts

A layouts tömbben lehet beállítani azt hogy az adott [route](../theme-configs/SETTINGS_JSON.md)-hoz milyen elrendezés 
tartozzon. A layoutokhoz tartozó sablon fájlokat a sablon fájl szerkesztő layout mappájában lehet elérni.

Példa:

```json
{
    "layouts": [
        {
           "name": "layout/1-column-layout",
           "route": "error/not_found"
        },
        {
           "name": "layout/1-column-home-layout",
           "route": "common/home"
        },
        {
           "name": "layout/1-column-product-layout",
           "route": "product/product"
        },
        {
           "name": "layout/2-column-left-layout",
           "route": "default"
        }
    ]
}
```

A példa alapján a `default` route esetén a `layout/2-column-left-layout.tpl` fájl alapján épül fel a frontend. 
Ha nincs megadva egy route-hoz egyedi layout, akkor mindig a default-nál beállított layout lesz érvényes.

Egy másik elem a példában a `common/home` route, vagyis a kezdőlap esetén már egy másik layout fájl lesz használva:
 `layout/1-column-home-layout`.

### positions

A positions objektumban az adott sablonhoz tartozó modul pozíciókat lehet beállítani. A positions objektumban lévő 
értékek tulajdonság nevei lesznek a pozíciók nevei, az értéke pedig egy objektum, aminél két tulajdonságot lehet 
definiálni:

<table>
<tr>
<td>
deniedfor
</td>
<td>
Tömb
</td>
<td>
Fel lehet sorolni azokat a modulokat amiket szeretnénk letiltani ebben a pozícióban
</td>
<tr>
<td>
allowedfor
</td>
<td>
Tömb
</td>
<td>
Fel lehet sorolni azokat a modulokat amik engedélyezettek ebben a pozícióban
</td>
</tr>

Példa:
```json
"positions": {
        "home": {
            "deniedfor": ["paf_filter", "compare", "manufacturer", "searchlog"]
        },
        "footer-top-1": {
            "deniedfor": ["paf_filter", "compare", "manufacturer", "searchlog"]
        },
        "footer-top-2": {
            "deniedfor": ["paf_filter", "compare", "manufacturer", "searchlog"]
        },
        "product-before-tabs": {
            "allowedfor": ["customcontents"]
        },
        "product-after-tabs": {
            "allowedfor": ["customcontents"]
        },
        "loginpage": {
            "allowedfor": ["sociallogin"]
        },
        "left": {},
        "right": {},
    }
```

A példa alapján a `home` pozícióban tiltva van a `paf_filter`, `compare`, `manufacturer` és `searchlog` modul. 

Másik példát megnézve a `product-before-tabs` pozícióban csak az egyedi html modulok vannak engedélyezve. 
Ezeket nem kell felsorolni egyesével, hogy `customcontent1`, `customcontent2`, stb., hanem létezik egy foglalt 
kulcsszó erre: `customcontents`. 

Harmadik példát nézve a `loginpage` pozícióban csak a `sociallogin` modul engedélyezett.

Ha nem szeretnénk megszorításokat, akkor üresen hagyhatjuk az objektumot, mint a `left` és `right` pozícióknál is látható.

A pozíciók használata a ShopRenter egyik leggyakrabban használt része. Érdemes tisztában lenni velük, 
ha a [dinamikus modulokat](../theme-sections/DOCS.md) használjuk.