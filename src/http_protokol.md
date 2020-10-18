# HTTP protokol

HTTP protokol definuje několik metod, které označují typ akce, která má být na prostředcích provedena.

## GET

Metoda požádá o data ze zdroje a na výstupu vrací požadované údaje. Neměla by způsobovat žádné vedlejší účinky.
Např. `/companies/3/employees` vrací seznam zaměstnanců pro zákazníka číslo 3.

## POST

Metoda vyžaduje, aby server vytvořil nový prostředek (např. v databázi), většinou při odeslání webového formuláře.
Např. `/companies/3/employees` vytvoří nového zaměstnance společnosti 3.

Metoda není idempotentní, což znamená, že více požadavků bude mít různé účinky.

## PUT

Metoda vyžaduje, aby server aktualizoval zdroj nebo jej vytvořil, pokud neexistuje.
Např. `/companies/3/employees/john` požádá server o aktualizaci nebo vytvoření zdroje `john` v kolekci zaměstnanců ve společnosti 3, pokud neexistuje.

Metoda je idempotentní, což znamená, že více požadavků bude mít stejné účinky.

## DELETE

Metoda požaduje, aby prostředky nebo jeho instance byly odebrány z databáze.
Např. `/companies/3/employees/john` bude požadovat, aby server odstranil prostředek `john` z kolekce zaměstnanců ve společnosti 3.


