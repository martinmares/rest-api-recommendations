# Jmenné konvence

## Používejte podstatná jména

K pojmenování prostředkú (`resources`) používejte vždy podstatná jména.

| Příklady                                                               |
| :--                                                                    |
| `http://api.example.com/device-management/managed-devices `            |
| `http://api.example.com/device-management/managed-devices/{device-id}` |
| `http://api.example.com/user-management/users/`                        |
| `http://api.example.com/user-management/users/{id}`                    |

Pro větší srozumitelnost můžeme rozdělit archetypy zdrojů do čtyř kategorií (`dokument`, `kolekce`, `úložiště` a `řadič`) a poté byste se měli vždy zaměřit na to, abyste prostředek umístili do jednoho archetypu, a poté důsledně používejte jeho konvenci pojmenování. 

### Dokument (`document`)

Dokument je podobný __instanci objektu__ nebo záznamu databáze. V REST jej můžete zobrazit jako jeden prostředek v kolekci prostředků. Reprezentace stavu dokumentu obvykle zahrnuje obě pole s hodnotami a odkazy na další související zdroje.

Používejte `jednotné číslo` k označení dokumentu.

| Příklady                                                               |
| :--                                                                    |
| http://api.example.com/device-management/managed-devices/`{device-id}` |
| http://api.example.com/user-management/users/`{id}`                    |
| http://api.example.com/user-management/users/`admin`                   |

### Kolekce (`collection`)

Používejte `množné číslo` k označení kolekce.


| Příklady                                                     |
| :--                                                          |
| http://api.example.com/device-management/`managed-devices`   |
| http://api.example.com/user-management/`users`               |
| http://api.example.com/user-management/users/{id}/`accounts` |

### Úložiště (`store`)

Úložiště je úložiště prostředků spravované klientem. Prostředek úložiště umožňuje klientovi přes API vložit prostředky, získat je zpět a rozhodnout se, kdy je odstranit. Úložiště nikdy nevygeneruje nové identifikátory URI. Tipickým příkladem je vytvoření záznamu přes metodu `POST`.

Používejte `množné číslo` pro generování záznamů přes `POST`.
    
| Příklady                                                      |
| :--                                                           |
| http://api.example.com/song-management/users/{id}/`playlists` |

### Řadič (`controller`)

Modeluje procedurální koncept. Prostředky řadiče jsou jako spustitelné funkce s parametry a návratovými hodnotami; vstupy a výstupy.

Používejte `slovesa` při vytváření řadičů.

| Příklady                                                          |
| :--                                                               |
| http://api.example.com/cart-management/users/{id}/cart/`checkout` |
| http://api.example.com/song-management/users/{id}/playlist/`play` |

## Konzistence je klíč

Použijte konzistentní konvence pojmenování prostředků a formátování URI pro minimální nejednoznačnost a maximální čitelnost a udržovatelnost. K dosažení konzistence můžete implementovat níže uvedené návrhové rady.

* __K označení hierarchických vztahů použijte lomítko `/`__

| Příklady                                                                   |
| :--                                                                        |
| http://api.example.com/device-management                                   |
| http://api.example.com/device-management/managed-devices                   |
| http://api.example.com/device-management/managed-devices/{id}              |
| http://api.example.com/device-management/managed-devices/{id}/scripts      |
| http://api.example.com/device-management/managed-devices/{id}/scripts/{id} |

* __Nepoužívejte koncové lomítko `/` v URI__

| Příklady                                                         |         |
| :--                                                              |         |
| http://api.example.com/device-management/managed-devices         |         |
| <s>http://api.example.com/device-management/managed-devices/</s> | __NE!__ |

* __Ke zlepšení čitelnosti identifikátorů URI použijte pomlčky `-`__

| Příklady                                                                                      |         |
| :--                                                                                           |         |
| http://api.example.com/inventory-management/managed-entities/{id}/install-script-location     |         |
| <s>http://api.example.com/inventory-management/managedEntities/{id}/installScriptLocation</s> | __NE!__ |

* __Nepoužívejte podtržítka `_`__

Abyste se tomuto zmatku vyhnuli, použijte místo podtržítka `_` pomlčku `-`.


| Příklady                                                                                         |         |
| :--                                                                                              |         |
| http://api.example.com/inventory-management/managed-entities/{id}/install-script-location        |         |
| <s>http://api.example.com/inventory_management/managed_entities/{id}/install_script_location</s> | __NE!__ |

* __Používejte `lower-case` slova v URI__

| Příklady                                       |         |
| :--                                            |         |
| http://api.example.org/my-folder/my-doc        |         |
| <s>HTTP://API.EXAMPLE.ORG/my-folder/my-doc</s> |         |
| <s>http://api.example.org/My-Folder/my-doc</s> | __NE!__ |

* __Nepoužívejte přípony souborů v URI__

Místo toho použijte `Content-Type` hlavičku HTTP.

| Příklady                                                             |         |
| :--                                                                  |         |
| http://api.example.com/device-management/managed-devices             |         |
| <s>http://api.example.com/device-management/managed-devices.xml </s> | __NE!__ |
