# HTTP kódy

Seznam status kódů HTTP protokolu

Když klient pošle požadavek na server prostřednictvím API, měl by klient znát zpětnou vazbu, ať už se to povedlo nebo nepovedlo, případně, že požadavek byl špatný. Stavové kódy HTTP jsou standardizovány, mají různá vysvětlení při různých scénářích. Server by měl vždy vrátit správný stavový kód.

## 2xx (success)

Tyto stavové kódy představují, že požadovaná akce byla přijata a úspěšně zpracována serverem.

| Kód | Text       | Význam                                                                                                                                  |
|----:|:-----------|:---------------------------------------------------------------------------------------------------------------------------------------:|
| 200 | Ok         | Úspěšně provedená operace pro `GET`, `PUT` nebo `POST`                                                                                  |
| 201 | Created    | Měl být vrácen při každém vytvoření nové instance. Např. při vytváření nové instance pomocí metody `POST` by měl server vždy vrátit 201 |
| 204 | No content | Požadavek je úspěšně zpracován, ale server nevrátil žádný obsah. Dobrým příkladem toho může být `DELETE`.                               |

> DELETE `/companies/43/employees/2` smaže zaměstnance 2 a na oplátku nepotřebujeme žádná data v těle odpovědi API, protože jsme výslovně požádali systém o vymazání. Pokud dojde k nějaké chybě, například pokud zaměstnanec 2 v databázi neexistuje, pak by kód odpovědi neměl být kategorie `2xx`, ale spíše kategorie `4xx`, tedy chyba.

## 3xx (redirection)

| Kód | Text         | Význam                                                                                            |
|----:|:-------------|:-------------------------------------------------------------------------------------------------:|
| 304 | Not modified | Klient má odpověď již ve své mezipaměti. Není tedy nutné znovu přenášet stejná data. |

## 4xx (client error)

| Kód | Text         | Význam                                                                                                                                          |
|----:|:-------------|:-----------------------------------------------------------------------------------------------------------------------------------------------:|
| 400 | Bad Request  | Požadavek klienta nebyl zpracován, protože server nerozuměl požadavku.                                                                          |
| 401 | Unauthorized | Klient nemá povolený přístup k prostředkům, a měl by poslat požadované přihlašovací údaje                                                       |
| 403 | Forbidden    | Požadavek je platný a klient je ověřen, ale klientovi není z jakéhokoli důvodu povolen přístup na stránku nebo ke zdroji. Např. chybějící role. |
| 404 | Not Found    | Zdroj není nyní k dispozici.                                                                                                                    |
| 410 | Gone         | Zdroj již není k dispozici a byl záměrně přesunut.                                                                                              |

## 5xx (server error)

| Kód | Text                  | Význam                                                                                                                |
|----:|:----------------------|:---------------------------------------------------------------------------------------------------------------------:|
| 200 | Internal Server Error | Požadavek je platný, ale došlo k neočekáváané chybě na straně serveru.                                                |
| 503 | Service Unavailable   | Server je nefunkční nebo není k dispozici pro přijetí a zpracování požadavku. Většinou pokud server prochází údržbou. |
