# Úvod

Slovník pojmů, někde je zmiňuji anglicky, někde česky, vždy ve stejném významu:

| Anglický originál | Český překlad, použitý dále v textu                                                      |
|:------------------|:-----------------------------------------------------------------------------------------|
| Resource          | Prostředek                                                                               |
| URL               | Identifikátor prostředku včetně protokolu a hostname (např. tak jak je vidět v browseru) |
| API path          | Část URL adresy, tedy to co je za http://hostname/__{API path}__                         |
| API endpoint      | Koncový bod API, vlastně URL adresa                                                      |

## Výhody REST API

 * __Nezávislost platformy__ - Rozhraní API by měl být schopný volat jakýkoli klient bez ohledu na to, jak je rozhraní API implementované interně.
 * __Rozvoj služby__ - Webové rozhraní API by mělo mít možnost se rozvíjet a přidávat funkce nezávisle na klientských aplikacích. Jak se rozhraní API vyvíjí, existující klientské aplikace by měly být nadále schopné fungovat bez úprav.

REST je architektonický přístup k navrhování webových služeb. REST je styl architektury pro vytváření distribuovaných systémů.
REST je nezávislý na všech podřízených protokolech. Většina běžných implementací REST ale jako aplikační protokol používá protokol HTTP.
Primární výhodou REST přes HTTP je to, že používá otevřené standardy a neváže implementaci rozhraní API nebo klientských aplikací na žádnou konkrétní implementaci.

## Prostředek

Prostředek je __objekt__ nebo reprezentace něčeho, k čemuž jsou přidružena data.
Pro tento __objekt__ může být vystavena nějaká metoda (typicky `GET`, `DELETE` ad.).

Např. Zákazník, Adresa a Uživatel jsou __resources__ a __mazání__, __přidávání__ a __aktualizace__ jsou operace, které mohou být s těmito prostředky prováděny.

> Kolekce je sada prostředků, např. Zákazníci jsou kolekcí Zákazníků (_resources_).

## Výměnný formát

Klienti komunikují se službou tak, že si vyměňují reprezentace prostředků. Mnoho webových rozhraní API používá jako formát výměny JSON. 
Příklad reprezentace prostředku:

```JSON
{"orderId":1,"orderValue":99.90,"productId":1,"quantity":1}
```

## URL

Je celá adresa prostředku, tedy včetně hostname a identifikace cesty (__path__) k prostředku (__resource__).
Na kombinaci __path__ + __resource__ je pak zavolána nějaká akce, typicky __GET__, __DElETE__, __POST__ atd.

Příklad URL:

```http
https://api.example.com/orders/1
```
## Operace

Rozhraní REST API používají jednotné rozhraní, které pomáhá oddělit implementace klienta a služby. Pro rozhraní REST API postavená na protokolu HTTP zahrnuje jednotné rozhraní k provádění operací s prostředky standardní příkazy HTTP. Nejběžnější operace jsou `GET`, `POST`, `PUT`, `PATCH` a `DELETE`.

## Odkazy

Součástí reprezentace prostředku můžou být __odkazy__. Následující příklad ukazuje reprezentaci objednávky JSON. Obsahuje odkazy pro získání nebo aktualizaci zákazníka přidruženého objednávce:

```json
{
    "orderID":3,
    "productID":2,
    "quantity":4,
    "orderValue":16.60,
    "_links": [
        {"rel":"product","href":"https://example.com/customers/3", "action":"GET" },
        {"rel":"product","href":"https://example.com/customers/3", "action":"PUT" }
    ]
}
```

## Koncový bod API 

Zkusme napsat API pro __Zákazníky__, kteří mají __Zaměstnance__, abychom tomu porozuměli více.

| Metoda              | Co provádí                      |
| :-------------      | :----------                     |
| /getAllEmployees    | Vrací seznam zaměstnanců        |
| /addNewEmployee     | Přidává zaměstnance             |
| /updateEmployee     | Aktualizuje údaje o zaměstnanci |
| /deleteEmployee     | Smaže zaměstnance               |
| /deleteAllEmployees | Smaže všechny zaměstnance       |

A spousta dalších koncových bodů API, jako jsou tyto, pro různé operace.
Všechny budou obsahovat mnoho nadbytečných akcí.
Proto by všechny tyto koncové body API bylo obtížné udržovat, když se počet API zvýší!.

### Co je špatně s tímto přístupem k API?

Adresa URL by měla obsahovat pouze prostředky (podstatná jména), __nikoli akce nebo slovesa__
Metoda _addNewEmployee_ obsahuje akci __addNew__ spolu s názvem prostředku __Zaměstnanec__, a to je špatně.

### Jak je to tedy správně?

Dobrým příkladem je Endpoint __/companies__, tento název v sobě neobsahuje žádnou akci! Otázka tedy zůstává, jak se server dozví, co s tímto prostředkem dělat. V tomto momentě vstupuje do role HTTP protokol a metody, které jsou součástí tohoto protokolu!

Prostředek by měl být u koncového bodu API __vždy v množném čísle__ a pokud chceme získat přístup k jedné instanci prostředku, můžeme jeho ID vždy předat v adrese URL.

| Metoda         | Cesta (URL path) | Význam                    |
| :------------- | :----------      | :---                      |
| GET            | /companies       | Vrací _kolekci_ zákazníků |
| GET            | /companies/34    | Vrací detail zákazníka    |
| DELETE         | /companies/34    | Smaže konrétního zák.     |

Několik dalších ukázek, pro situaci, kdy máme další prostředek pod cestou __/companies__. Např. budeme chtít zveřejnit údaje o zaměstnancích. API bude vypadat nějak takto:

| Metoda | Cesta (URL path)          | Význam                             |
| :--    | :--                       | :--                                |
| GET    | /companies/3/employees    | Vrací kolekci zaměstnanců          |
| GET    | /companies/3/employess/45 | Vrací detail o zamestnanci s ID 45 |
| DELETE | /companies/3/employess/45 | Smaže zamestnance                  |
| POST   | /companies                | Založí nového zákazníka            |

