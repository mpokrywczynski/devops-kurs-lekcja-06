# Raport - example.com

Zgłoszenie: strona example.com się nie ładuje, poczta działa.

## 1. Czy nazwa się rozwiązuje

```
$ dig example.com @1.1.1.1
status: NOERROR
example.com.  12  IN  A  172.66.147.243
example.com.  12  IN  A  104.20.23.154
```

Nazwa działa, są dwa adresy IP.

## 2. Ślad od korzenia

```
$ dig +trace example.com
.             NS  a.root-servers.net. (i inne)
com.          NS  a.gtld-servers.net. (i inne)
example.com.  NS  elliott.ns.cloudflare.com.
example.com.  300  IN  A  172.66.147.243
example.com.  300  IN  A  104.20.23.154
```

Zapytanie przeszło przez serwery root, potem .com, potem serwer domeny (Cloudflare). Wszystko odpowiada.

## 3. Rekordy MX

```
$ dig MX example.com @1.1.1.1 +short
0 .
```

MX `0 .` oznacza, że ta domena w ogóle nie przyjmuje poczty. Nie zgadza się to ze zgłoszenie

## 4. Porty

```
$ timeout 5 bash -c 'cat < /dev/null > /dev/tcp/example.com/443' && echo "443 open"
443 open
$ timeout 5 bash -c 'cat < /dev/null > /dev/tcp/example.com/80' && echo "80 open"
80 open
```

Porty 80 i 443 są otwarte.

## 5. Odpowiedź HTTP

```
$ curl -I https://example.com
HTTP/2 200
server: cloudflare
```

Serwer zwraca 200, czyli strona działa.

## Wniosek

Problem nie jest w DNS, sieci ani aplikacji. U mnie strona działa, więc problem jest najpewniej po stronie użytkownika.

## Co dalej

- poprosić użytkownika o `nslookup example.com` i porównać adresy
- sprawdzić u niego plik hosts
- wyczyścic cache przeglądarki i DNS, spróbować w innej przeglądarce
