# Vyspělost API

V roce 2008 Leonard Richardson navrhl následující model _vyspělosti_ pro webová rozhraní API.

[Pěkně popsaný zdroj v angličtině](https://martinfowler.com/articles/richardsonMaturityModel.html) od Martina Fowlera.

Úroveň 3 odpovídá skutečnému rozhraní RESTful API podle definice Fieldinga.
V praxi hodně publikovaných webových rozhraní API leží někde kolem úrovně 2.

| Level   | Reprezentace | Implementace                                                   |
|:--------|--------------|:---------------------------------------------------------------|
| LEVEL 0 | XML          | Jedno URI, jedna metoda (většinou `POST`) - SOAP, XML, RPC     |
| LEVEL 1 | URI          | Hodně URI, jedna metoda (většinou `POST`)                      |
| LEVEL 2 | HTTP         | Hodně URI, hodně metod (`GET`, `POST`, `PUT`, `DELETE`) - CRUD |
| LEVEL 3 | HYPERMEDIA   | LEVEL 2 + Hypermedia (odkazy)                                  |

## Level 0 - všechno v POST

Je definováno jedno URL, jeden prostředek, který je schopen obsluhovat všechny operace přes POST rozhranní.

Ukázka, máme jednu službu:
* appointmentService

A dvě operace:
* openSlotList
* appointmentRequest

Request pro operaci `openSlotReques`:

```http
POST /appointmentService HTTP/1.1

<openSlotRequest date = "2010-01-04" doctor = "mjones"/>
```

Response:

```http
HTTP/1.1 200 OK

<openSlotList>
  <slot start = "1400" end = "1450">
    <doctor id = "mjones"/>
  </slot>
  <slot start = "1600" end = "1650">
    <doctor id = "mjones"/>
  </slot>
</openSlotList>
```
 
 
Request pro operaci `appointmentRequest`:

```http
POST /appointmentService HTTP/1.1

<appointmentRequest>
  <slot doctor = "mjones" start = "1400" end = "1450"/>
  <patient id = "jsmith"/>
</appointmentRequest>
```

Response:

```http
HTTP/1.1 200 OK

<appointment>
  <slot doctor = "mjones" start = "1400" end = "1450"/>
  <patient id = "jsmith"/>
</appointment>
```

## Level 1 - rozdělení na Prostředky (_resources_)

Místo toho, abychom si povídalo s jednou službou, rozdělíme služby na více zdrojů. Každá služba má pak svoje kompetence.

Ukázka, máme dva prostředky (_resources_):
* /doctors
* /slots
 
Request na `/doctors` pak vypadá nějak takto:

```http
POST /doctors/mjones HTTP/1.1

<openSlotRequest date = "2010-01-04"/>
```

Response:

```http
HTTP/1.1 200 OK

<openSlotList>
  <slot id = "1234" doctor = "mjones" start = "1400" end = "1450"/>
  <slot id = "5678" doctor = "mjones" start = "1600" end = "1650"/>
</openSlotList>
```

Request na `/slots` pak vypadá takto:


```http
POST /slots/1234 HTTP/1.1

<appointmentRequest>
  <patient id = "jsmith"/>
</appointmentRequest>
```

Response:

```http
HTTP/1.1 200 OK

<appointment>
  <slot id = "1234" doctor = "mjones" start = "1400" end = "1450"/>
  <patient id = "jsmith"/>
</appointment>
```

## Level 2 - metody HTTP protokolu (_verbs - slovesa_)

Místo použití metody `POST` pro všechny operace se použijí přímo metody HTTP protokolu.

Ukázka, pro získání seznamu slotů voláme např. takto:

```http
GET /doctors/mjones/slots?date=20100104&status=open HTTP/1.1
Host: api.example.com
```

Response:

```http
HTTP/1.1 200 OK

<openSlotList>
  <slot id = "1234" doctor = "mjones" start = "1400" end = "1450"/>
  <slot id = "5678" doctor = "mjones" start = "1600" end = "1650"/>
</openSlotList>
```

K rezervaci schůzky použijeme sloveso `POST`, např. takto:

```http
POST /slots/1234 HTTP/1.1

<appointmentRequest>
  <patient id = "jsmith"/>
</appointmentRequest>
```

A response vypadá takto:


```http
HTTP/1.1 201 Created
Location: slots/1234/appointment

<appointment>
  <slot id = "1234" doctor = "mjones" start = "1400" end = "1450"/>
  <patient id = "jsmith"/>
</appointment>
```

## Level 3 - použití odkazů (_hypermedia controls_)

Ve výstupu (response) jsou použity odkazy na další hypermédia (HATEOAS - Hypertext As The Engine Of Application State).

Ukázka, např. zavoláme metodu `GET` takto:

```http
GET /doctors/mjones/slots?date=20100104&status=open HTTP/1.1
Host: api.example.com

```

A response:

```http
HTTP/1.1 200 OK

<openSlotList>
  <slot id = "1234" doctor = "mjones" start = "1400" end = "1450">
     <link rel = "/linkrels/slot/book" 
           uri = "/slots/1234"/>
  </slot>
  <slot id = "5678" doctor = "mjones" start = "1600" end = "1650">
     <link rel = "/linkrels/slot/book" 
           uri = "/slots/5678"/>
  </slot>
</openSlotList>
```

A další ukázka, poukud k rezervaci schůzky použijeme sloveso `POST`, např. takto:

```http
POST /slots/1234 HTTP/1.1

<appointmentRequest>
  <patient id = "jsmith"/>
</appointmentRequest>
```

Response vypadá takto:


```http
HTTP/1.1 201 Created
Location: http://api.example.com/slots/1234/appointment

<appointment>
  <slot id = "1234" doctor = "mjones" start = "1400" end = "1450"/>
  <patient id = "jsmith"/>
  <link rel = "/linkrels/appointment/cancel"
        uri = "/slots/1234/appointment"/>
  <link rel = "/linkrels/appointment/addTest"
        uri = "/slots/1234/appointment/tests"/>
  <link rel = "self"
        uri = "/slots/1234/appointment"/>
  <link rel = "/linkrels/appointment/changeTime"
        uri = "/doctors/mjones/slots?date=20100104&status=open"/>
  <link rel = "/linkrels/appointment/updateContactInfo"
        uri = "/patients/jsmith/contactInfo"/>
  <link rel = "/linkrels/help"
        uri = "/help/appointment"/>
</appointment>
```

Velkou výhodoou použití hypermedií je pak možnost změny logiky uvnotř služby bez toho, aniž bychom rozbily klienta.

> V současné době nejsou k dispozici žádné standardy pro obecné účely, které definují způsob modelování principu HATEOAS.
