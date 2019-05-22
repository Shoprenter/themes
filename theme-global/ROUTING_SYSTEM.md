## Tartalomjegyzék
* [Routing rendszer](#routing-rendszer)
* [Route-ok ShopRenter-ben](#route-ok-shoprenter-ben)
* [Hol és mire használhatjuk a route-kat?](#hol-és-mire-használhatjuk-a-route-kat)

### Routing rendszer
 
 A ShopRenter sablon rendszeréhez szorosan kötődik a routing rendszer. A route azt az azonosítót jelenti, ami alapján
  meg lehet állapítani, hogy melyik aloldalon vagyunk. Ez egy sablon készítése során fontos, hiszen ez alapján lehet
   megtalálni a megfelelő sablon fájlt, illetve az adott route-hoz tartozó layout-ot módosítani.
   
### Route-ok ShopRenter-ben

<table>
<tr>
<th>route</th>
<th>leírás</th>
</tr>

<tr>
<td>
product/list
</td>
<td>
Terméklista oldal/kategória oldal
</td>
</tr>

<tr>
<td>
product/manufacturers
</td>
<td>
Összes gyártó lista oldala
</td>
</tr>

<tr>
<td>
filter
</td>
<td>
Terméklista oldal szűrés után
</td>
</tr>

<tr>
<td>
product/product
</td>
<td>
Termék oldal
</td>
</tr>

<tr>
<td>
information/information_list
</td>
<td>
Szöveges tartalom lista oldal
</td>
</tr>

<tr>
<td>
information/information
</td>
<td>
Szöveges tartalom aloldal
</td>
</tr>

<tr>
<td>
information/contact
</td>
<td>
Kapcsolat oldal
</td>
</tr>

<tr>
<td>
information/personaldata
</td>
<td>
Személyes adatok kezelése
</td>
</tr>

<tr>
<td>
information/sitemap
</td>
<td>
Oldaltérkép
</td>
</tr>

<tr>
<td>
information/style_guide
</td>
<td>
Style Guide oldal
</td>
</tr>

<tr>
<td>
error/not_found
</td>
<td>
404-es hiba oldal
</td>
</tr>

<tr>
<td>
account/account
</td>
<td>
Felhasználói fiók oldala
</td>
</tr>

<tr>
<td>
account/edit
</td>
<td>
Fiók adatok módosítása
</td>
</tr>

<tr>
<td>
account/password
</td>
<td>
Jelszócsere oldal
</td>
</tr>

<tr>
<td>
account/address
</td>
<td>
Címadatok módosítása
</td>
</tr>

<tr>
<td>
account/waitinglist
</td>
<td>
Értesítés kéréseim oldal
</td>
</tr>

<tr>
<td>
account/history
</td>
<td>
Korábbi rendeléseim lista oldala
</td>
</tr>

<tr>
<td>
account/invoice
</td>
<td>
Korábbi rendelés részletes aloldala
</td>
</tr>

<tr>
<td>
account/download
</td>
<td>
Letölthető termékek lista oldala
</td>
</tr>

<tr>
<td>
account/newsletter
</td>
<td>
Hírlevél feliratkozása/lemondása aloldal
</td>
</tr>

<tr>
<td>
account/registration_contribution
</td>
<td>
Regisztrációs hozzájárulás elfogadása/elutasítása
</td>
</tr>

<tr>
<td>
account/aaffiliate
</td>
<td>
Affiliate aloldal
</td>
</tr>

<tr>
<td>
wishlist/wishlist
</td>
<td>
Kívánságlistán lévő termékek listája
</td>
</tr>

</table>

A bejelentkezés, regisztráció, kosár és pénztár oldalak sablon fájljaihoz nem lehet hozzáférni a ShopRenter-ben. 
Ezeken az oldalakon a route értéke üres lesz.

### Hol és mire használhatjuk a route-kat?

1. Ha szeretnénk meghatározni, hogy melyik oldalon vagyunk, a
 [ShopRenter.js](https://github.com/Shoprenter/developers/blob/master/frontend-api/SHOPRENTERJS_API.md#page) 
 objektumból ki lehet kérni: `ShopRenter.page.route`

2. A `settings.json` layouts property értékénél meg lehet adni, melyik route-hoz milyen layout tartozzon.

3. A Sablon fájl szerkesztőben a route alapján lehet megtalálni a sablon fájlt. 
Például a product/product route-hoz a product/product.tpl tartozik.

Kivétel ez alól a `product/list`, ahol nem a product/list.tpl a sablon fájl, hanem több sablon fájl is tartozik hozzá:
  
<table>

<tr>
<td>
product/search.tpl
</td>
<td>
Kereső találatok
</td>
</tr>

<tr>
<td>
product/category.tpl
</td>
<td>
Terméklista/kategória oldal
</td>
</tr>

<tr>
<td>
product/manufacturer.tpl
</td> 
<td>
Terméklista egy adott gyártóhoz
</td>
</tr>

<tr>
<td>
product/productspecial.tpl
</td> 
<td>
Akciós termékek lista oldala
</td>
</tr>

<tr>
<td>
product/latestlist.tpl
</td> 
<td>
Legújabb termékek lista oldala
</td>
</tr>

</table>