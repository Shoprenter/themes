# Objektumok

## Leírás
Hamarosan

## config

Hamarosan

---
## currency
### Leírás
Az objektum információkat tartalmaz a webáruházban beállított pénznemekről.
### Tulajdonságok

<table>
<tr>
    <th>Tulajdonság</th>
    <th>Leírás</th>
    <th>Típus</th>
</tr>
<tr>
    <td>currencyId</td>
    <td>aktuális pénznem id, pl. 4</td>
    <td>String</td>
</tr>
<tr>
    <td>currencyCode</td>
    <td>aktuális pénznem code, pl. HUF</td>
    <td>String</td>
</tr>
<tr>
    <td>currencyTitle</td>
    <td>aktuális pénznem title, pl. Hungarian Forint</td>
    <td>String</td>
</tr>
<tr>
    <td>availableCurrencies</td>
    <td>tömb, amely az aktív pénznemeket tartalmazza</td>
    <td>Array</td>
</tr>
</table>

### Példakód
Az aktív pénznemek 1. elemének a title-je:
``` 
{{ shop.availableCurrencies[0].getCurrencyTitle }}
```
---

##
<tab><tab>code/text here

## image

Hamarosan

---

## page

Hamarosan

---

## routes

Hamarosan

---

## settings
A témához beállított színeket tartalmazza. A színek a config.data.json fájlban vannak definiálva.

### Példakód
A config.data.json fájlban beállított "global-color" színt adja vissza. Ha nem található meg a "global-color", akkor a fallback: #000
``` 
{{ settings.get('global-color', '#000') }}
```

---

## shop

Hamarosan

