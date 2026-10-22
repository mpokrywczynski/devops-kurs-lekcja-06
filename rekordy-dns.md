# Rekordy DNS

| Typ | Przykład | Do czego | Pułapka |
|---|---|---|---|
| A | `example.com. IN A 203.0.113.10` | nazwa na adres IPv4 | DNS nie sprawdza, czy serwer działa |
| AAAA | `example.com. IN AAAA 2001:db8::10` | nazwa na adres IPv6 | dodawać tylko, jak strona działa po IPv6 |
| CNAME | `www.example.com. IN CNAME example.com.` | alias na inną nazwę | nie może być razem z innymi rekordami ani na samym example.com |
| MX | `example.com. IN MX 10 mail.example.com.` | serwer poczty | podaje się nazwę, nie IP; mniejsza liczba = ważniejszy |
| TXT | `example.com. IN TXT "v=spf1 ip4:203.0.113.10 ~all"` | tekst, np. SPF | tylko jeden rekord SPF |
| NS | `example.com. IN NS ns1.example.net.` | serwery DNS domeny | musi się zgadzać z tym u rejestratora |
| SOA | `example.com. IN SOA ns1.example.net. admin.example.com. 2026102201 7200 1800 1209600 3600` | ustawienia strefy | po zmianie trzeba podbić serial |
| PTR | `10.113.0.203.in-addr.arpa. IN PTR mail.example.com.` | adres IP na nazwę | ustawia go hosting, a nie rejestrator domeny |
| SRV | `_sip._tcp.example.com. IN SRV 10 60 5060 sip.example.com.` | host i port usługi | przeglądarki go nie używają |
| CAA | `example.com. IN CAA 0 issue "letsencrypt.org"` | kto może wystawić certyfikat | bez CAA może każdy |
