# NXDOMAIN

## 1. Nazwa istnieje, ma rekord A

```
$ dig A wikipedia.org @8.8.8.8
status: NOERROR
ANSWER: wikipedia.org.  16  IN  A  185.15.59.224
AUTHORITY: brak
```

## 2. Nazwa istnieje, nie ma rekordu AAAA

```
$ dig AAAA github.com @dns1.p08.nsone.net +norecurse
status: NOERROR
ANSWER: brak
AUTHORITY: github.com.  3600  IN  SOA  dns1.p08.nsone.net. hostmaster.nsone.net. 1656468023 43200 7200 1209600 3600
```

## 3. Nazwa nie istnieje

```
$ dig A nieistnieje-xyz.wikipedia.org @ns0.wikimedia.org +norecurse
status: NXDOMAIN
ANSWER: brak
AUTHORITY: wikipedia.org.  3600  IN  SOA  ns0.wikimedia.org. hostmaster.wikimedia.org. 2026060420 43200 7200 1209600 3600
```

## Porównanie

| | status | ANSWER | AUTHORITY | TTL negatywny |
|---|---|---|---|---|
| ma A | NOERROR | adres | pusto | - |
| brak AAAA | NOERROR | pusto | SOA | 3600 |
| nie istnieje | NXDOMAIN | pusto | SOA | 3600 |

## Dlaczego w 2. jest NOERROR

Bo github.com istnieje, ma np. rekord A. Brakuje tylko rekordu AAAA. NXDOMAIN jest tylko wtedy, gdy samej nazwy nie ma.

## Skąd TTL odpowiedzi negatywnej

Z rekordu SOA w sekcji AUTHORITY. Bierze się mniejszą liczbę z dwóch: TTL rekordu SOA i ostatnie pole SOA (minimum). Tutaj obie to 3600, więc wynik to 3600 sekund. Tyle czasu resolwer pamięta, że nazwy albo rekordu nie ma.
