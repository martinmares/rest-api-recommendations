# Jak to nedělat

Čeho se vyvarovat při tvorbě API.

## Nepoužívejte slovesa v názvech zdrojů

| Tohle všechno je špatně |
| :--                     |
| `/getAllCars`           |
| `/createNewCar`         |
| `/deleteAllRedCars`     |

Místo toho použijte sloveso v HTPP metodě `GET`, `POST`, `DELETE`...

## Nepoužívejte jednotné číslo.

| Použijte    | Místo      |
| :--         |            |
| `/cars`     | `/car`     |
| `/users`    | `/user`    |
| `/products` | `/product` |
| `/settings` | `/setting` |

Snažte se to zjednodušit a pro všechny zdroje používejte pouze podstatná jména v množném čísle.

## Nepoužívejte koncové lomítko v URI

| Příklady                                                           |    |
| :--                                                                |    |
| http://api.example.com/device-management/managed-devices           | OK |
| <s>http://api.example.com/device-management/managed-devices`/`</s> | NE! |

## Nepoužívejte `lowerCamelCase` ani žádný jiný `CamelCase` v URI

| Příklady                                                                                      |     |
| :--                                                                                           |     |
| http://api.example.com/inventory-management/managed-entities/{id}/install-script-location     | OK  |
| <s>http://api.example.com/inventoryManagement/managedEntities/{id}/installScriptLocation</s> | NE! |

## Nemíchejte velikosti písmen v URI

| Příklady                                       |     |
| :--                                            |     |
| http://api.example.org/my-folder/my-doc        | OK  |
| <s>HTTP://API.EXAMPLE.ORG/my-folder/my-doc</s> | NE! |
| <s>http://api.example.org/My-Folder/my-doc</s> | NE! |

## Nepoužívejte podtržítka

Abyste se tomuto zmatku vyhnuli, použijte místo podtržítka `_` pomlčku `-`.

| Příklady                                                                                                 |     |
| :--                                                                                                      |     |
| http://api.example.com/inventory-management/managed-entities/{id}/install-script-location                | OK  |
| <s>http://api.example.com/inventory`_`management/managed`_`entities/{id}/install`_`script`_`location</s> | NE! |

## Nepoužívejte přípony souborů v URI

Místo toho použijte `Content-Type` hlavičku HTTP.

| Příklady                                                             |     |
| :--                                                                  |     |
| http://api.example.com/device-management/managed-devices             | OK  |
| <s>http://api.example.com/device-management/managed-devices.xml </s> | NE! |
