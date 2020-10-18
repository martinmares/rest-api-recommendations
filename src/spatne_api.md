# Špatné API

Aneb, co nedělat!

## Nepoužívejte slovesa v názvech zdrojů

Tohle všechno je špatně:

* `/getAllCars`
* `/createNewCar`
* `/deleteAllRedCars`

Místo toho použijte sloveso v HTPP metodě `GET`, `POST`, `DELETE`...

## Nepoužívejte jednotné číslo.

Použijte:

* `/cars` místo `/car`
* `/users` místo `/user`
* `/products` místo `/product`
* `/settings` místo `/setting`

Snažte se to zjednodušit a pro všechny zdroje používejte pouze podstatná jména v množném čísle.
