---
layout: post
title: 15 Zasad Przy Budowie REST API, Za Kt贸re Deweloperzy Ci臋 Pokochaj膮 馃グ
description: 
image: /images/15rest.png
tags: [spring, rest, api]
---

Kiedy pierwszy raz spotka艂em si臋 z REST API od razu je pokocha艂em. Czysty, czytelny spos贸b komunikacji mi臋dzy us艂ugami z jasno okre艣lonymi zasadami. Nic dziwnego, 偶e od jakiego艣 ju偶 czasu jest to najch臋tniej wybierane rozwi膮zanie przez deweloper贸w aplikacji webowych, czy mobilnych. W tym wpisie prezentuj臋 Ci 15 zasad, dzi臋ki kt贸rym zbudujesz REST API, za kt贸re deweloperzy Ci臋 pokochaj膮.

## 1. Stosuj rzeczowniki w adresach
Adresy w API REST-owym powinny by膰 RZECZOWNIKAMI - wskazywa膰 na konkretne zasoby, do kt贸rych chcesz si臋 dosta膰. Na przyk艂ad:

```
/books
/books/once-upon-a-time-in-a-hollywod
/books/831231201
/cars/audi/a4/b6/2009/diesel/2.0
/invoices/google-analytics/2019/03/12/151
```

W REST API nie stosuje si臋 czasownik贸w i wszystkie tego rodzaju adresy s膮 b艂臋dne, jak na przyk艂ad:

```
/getBooks
/books/once-upon-a-time-in-a-hollywod/update/status
/invoices/google-analytics/2019/03/12/151/markAsPaid
```

Dzi臋ki stosowaniu hierarchicznej struktury obiekt贸w i prostej - RZECZOWNIKOWEJ - adresacji zasob贸w API budowane za pomoc膮 REST-a jest du偶o bardziej intuicyjne, skalowalne i prostsze w obs艂udze przez wszystkich zainteresowanych.

## 2. U偶ywaj metod zgodnie z przeznaczeniem
REST opiera si臋 na wzorcach wypracowanych w protokole HTTP. Dlatego korzysta z metod znanych z HTTP.

Je艣li Twoja aplikacja udost臋pnia jedynie metody GET i POST to znak, 偶e nie do ko艅ca poprawnie implementujesz komunikacj臋 REST-ow膮.

Jak wi臋c powinno wygl膮da膰 to w rzeczywisto艣ci? Poka偶臋 na przyk艂adach.

1. `GET /books` - pobiera list臋 ksi膮偶ek,
2. `POST /invoices/google-analytics` - dodaje faktur臋,
3. `PUT /books/once-upon-a-time-in-a-hollywod` - nadpisuje konkretn膮 ksi膮偶k臋,
4. `PATCH /books/831231201` - aktualizuje konkretn膮 ksi膮偶k臋,
5. `DELETE /invoices/google-analytics/2019/03/12/151` - usuwa faktur臋 z danej 艣cie偶ki,
6. `HEAD /cars/audi/a4/b6/2009/diesel/2.0` - zwraca informacj臋 czy dany zas贸b istnieje (bez przesy艂ania tre艣ci zasobu)
7. `OPTIONS /books` - zwraca list臋 metod, kt贸re s膮 dost臋pne na danym zasobie.

Przy wykorzystaniu powy偶szych metod, oraz odpowiedniej adresacji zasob贸w (patrz punkt 1) jeste艣my w stanie wykona膰 wszystkie operacje na obiektach aplikacji bez potrzeby tworzenia potwork贸w w stylu `/offer/321312/markAsExpired`, w kt贸rych *niepoprawnie* korzystamy z czasownik贸w.

## 3. Zapoznaj si臋 ze statusami i ZAWSZE z nich korzystaj.
Protok贸艂 HTTP, na kt贸rym opiera si臋 REST, to nie tylko adresy i metody - ale tak偶e kody status贸w. Tych jest ponad 50. Ich najwi臋ksz膮 zalet膮 jest to, 偶e s膮 ustandaryzowane i wsz臋dzie znacz膮 to samo. Dzi臋ki temu komunikuj膮c si臋 z dowoln膮 us艂ug膮 REST-ow膮 od razu wiesz, co dana informacja oznacza. 

Wa偶ne by projektuj膮c w艂asne API REST-owe korzysta膰 z tych kod贸w i zwraca膰 te najbardziej odpowiadaj膮ce zdarzeniu.
Ja najcz臋艣ciej korzystam ze strony [httpstatuses.com](https://httpstatuses.com) aby podgl膮da膰, kt贸ry status powinienem u偶y膰.

Zajrzyj na ni膮, gdy nast臋pny raz b臋dziesz si臋 zastanawia膰 czy stosowa膰 kod 200, 201 a mo偶e 204.

## 4. Zwracaj dane w kopercie
Serwisy REST-owe, odr贸偶nia od serwis贸w SOAP-owych brak modelu kanonicznego - innymi s艂owy - narzuconej i wymaganej struktury danych. Z jednej strony jest to zaleta - bo daje du偶膮 elastyczno艣膰, a z drugiej problem, bo nie mo偶emy 艂atwo zdefiniowa膰 kontraktu, wed艂ug kt贸rego powinny by膰 przesy艂ane dane. 

Niezale偶nie od tego jak膮 struktur臋 danych przyjmiesz pami臋taj o tym, by zawsze przesy艂a膰 je w kopercie jak poni偶ej.

Pole `data` w przypadku poprawnej odpowiedzi.

```json
{
    "data": []
}
```

Oraz pole `error` w przypadku b艂臋d贸w.

```json
{
    "error": {
        "code": "",
        "message": "",
        "details": ""
    }
}
```


## 5. Zwracaj warto艣ciowe odpowiedzi b艂臋d贸w
Gdy co艣 w komunikacji z twoim API posz艂o nie tak staraj si臋 zwraca膰 szczeg贸艂owe informacje. Wiadomo艣膰 `Oops, something went wrong` mo偶e nie by膰 najlepszym pomys艂em ;) Stosuj kopert臋 z punktu 4 i umieszczaj w niej takie informacje jak:

* `code` - wewn臋trzny kod b艂臋du Twojej aplikacji, kt贸ry pozwoli Ci zidentyfikowa膰 problem,
* `msg` - wiadomo艣膰 tekstowa dotycz膮ca b艂臋du,
* `details` - opcjonalnie informacje ze szczeg贸艂ami danego problemu.

Jak mo偶e wygl膮da膰 taka wiadomo艣膰? Na przyk艂ad:

```json
{
    "error": {
        "code": "SK30",
        "message": "Insufficient account balance",
        "details": "Unable to transfer 100 PLN. Not enough funds on the account"
    }
}
```


## 6. Stosuj paginacj臋...
Masz endpoint do zwracania listy obiekt贸w, na przyk艂ad ksi膮偶ek w sklepie internetowym. Zwracasz wi臋c wszystkie obiekty. A co je艣li obiekt贸w nie jest 20, a 200? Jeszcze powinno by膰 ok. A 2 tysi膮ce? Albo鈥? 2 miliony?

Za ka偶dym razem stosuj paginacj臋 i zwracaj tylko podzbi贸r wszystkich obiekt贸w. Podzi臋kuj膮 Ci za to klienci, frontendowcy oraz szef鈥? kt贸ry b臋dzie p艂aci艂 mniejsze rachunki za serwery backendowe.

## 7. ...ale r贸b to M膭DRZE!
Standardowa metoda paginacji opiera si臋 na parze: `FROM` i `SIZE`. Przegl膮dasz wi臋c zasoby z backendu wykonuj膮c kolejno zapytania:

1. `from=0, size=10`
2. `from=10, size=10`
3. `from=20, size=10`
4. `from=30, size=10`
5. `from=40, size=10`

I tak dalej. Co je艣li mi臋dzy zapytaniem 3 i 4 doszed艂 nowy obiekt do systemu? A co je艣li jaki艣 obiekt zosta艂 z niego usuni臋ty? W Twoich wynikach pojawi si臋 niesp贸jno艣膰.

Zamiast tego sortuj zawsze obiekty wed艂ug sta艂ych atrybut贸w - na przyk艂ad `ID` a w trakcie paginacji stosuj raczej parametry:

1. `size=10`
2. `greaterThanOrEqual=<LAST_ITEM_ID> size=10`
3. `greaterThanOrEqual=<LAST_ITEM_ID_PAGE_2> size=10`
4. `greaterThanOrEqual=<LAST_ITEM_ID_PAGE_3> size=10`

Wstawiaj膮c do pola `greaterThanOrEqual` ostatnie ID obiektu ostatnio zwr贸conego z backendu.

## 8. Wersjonuj API
Gdy wprowadzasz 艂ami膮ce zmiany ;) (ang. `breaking changes`) do swojego API nigdy nie r贸b tego na opublikowanych ju偶 endpointach. Wspieraj stare API a nowe wprowad藕 obok ju偶 istniej膮ce.

Z czasem mo偶esz te偶 monitorowa膰 u偶ycie starego API i z chwil膮 zaniku jego u偶ywania dopiero je usun膮膰.

## 9. Wystaw Open API
Je艣li buduj膮c API na bie偶膮co wsp贸艂pracujesz z jego klientami warto wystawi膰 Open API. Jest to specjalny plik w formacie YML, kt贸ry dok艂adnie opisuje jakie endpointy wystawia Twoje API. Z jego pomoc膮 integracja staj臋 si臋 du偶o 艂atwiejsza. W j臋zykach JVM-owym uzyskasz Open API poprzez u偶ycie [Swaggera](https://swagger.io).

Przyk艂ady opublikowanych API znajdziesz za艣 pod linkiem [https://app.swaggerhub.com/search](https://app.swaggerhub.com/search)

## 10. Wystaw skomplikowane filtrowanie jako osobny search endpoint
Je艣li potrzebujesz przefiltrowa膰 wyniki zwracane przez API mo偶esz doda膰 zwyk艂y parametr zapytania, na przyk艂ad:

```
GET /books?title=Nad+niemnem
```

Co zrobi je艣li jednak chcesz przeszuka膰 wi臋cej parametr贸w jak na przyk艂ad: gatunek literacki, narodowo艣膰 autora, liczba pozytywnych opinii, dost臋pno艣膰 w sklepach internetowych, i tak dalej?

Wystaw specjalny endpoint - `_search` do przeszukiwania swoich zasob贸w. Potraktuj go jako specjalny dodatkowy zas贸b. Przyk艂adowe zapytanie mo偶e wtedy wygl膮da膰 tak:

```
POST /books/_search

{
  "genre": "lyric",
  "country": "Netherlands",
  "era": "renaissance",
  "rating": 4.5
}
```

## 11. Aktualizuj obiekty z metod膮 PATCH
Do aktualizacji zasobu s艂u偶膮 dwie metody - `PUT` i `PATCH`. Czym si臋 r贸偶ni膮? `PUT` - powinno nadpisywa膰 ca艂y obiekt, a `PATCH` nadpisywa膰 tylko wysy艂ane atrybuty.

Poni偶sze 偶膮danie powinno nadpisa膰 tylko tytu艂 ksi膮偶ki.

```
PATCH /books/134

{
    "title": "Nad Niemnem"
}
```

Natomiast `PUT` nadpisze ca艂膮 tre艣膰 pod podanym zasobem.

## 12. Pami臋taj, 偶e GET nie powinien przesy艂a膰 payloadu
Zauwa偶y艂e艣 mo偶e jakiej metody HTTP u偶y艂em w punkcie 9 przy definicji endpointu `_search`? Tak - `POST`. Chocia偶 wydawa艂oby si臋, 偶e powinien by膰 `GET`.

Z t膮 metod膮 jest jeden problem. Z definicji nie powinna ona zawiera膰 偶adnego *payloadu*. I chocia偶 cz臋艣膰 aplikacji i us艂ug po艣rednicz膮cych je akceptuje - to nie wszystkie musz膮 to robi膰.

Mo偶e si臋 zdarzy膰 tak, 偶e jedna sk艂adowa ca艂ego ci膮gu wywo艂a艅 pominie przes艂anie tre艣ci 偶膮dania `GET` i serwer nie otrzyma Twojego zapytania - tym samym zwr贸ci Ci tylko podstawowe dane umieszczone pod 偶膮danym zasobem.

## 13. Pami臋taj, 偶e POST zawsze tworzy nowy obiekt
Metody `POST` u偶ywaj zawsze do tworzenia nowych obiekt贸w. To znaczy:

* `POST /books` - utworzy ksi膮偶k臋 w systemie,
* `POST /books/123/comment` - doda komentarz do ksi膮偶ki

i tak dalej. Nawet je艣li zdarzy Ci si臋 przes艂a膰 pole `id` w tre艣ci 偶膮dania - w przypadku `POST`-a mo偶e ono by膰 swobodnie pomini臋te przez serwer, kt贸ry i tak utworzy nowy zas贸b.

## 14. Pami臋taj, 偶e PUT, DELETE i PATCH powinny by膰 IDEMPOTENTNE
Gdy wysy艂asz 偶膮dania modyfikuj膮ce REST-em zawsze przesy艂aj stan, a nie 偶膮danie. Ka偶de wywo艂anie REST-owego API powinno by膰 idempotente - to znaczy, zmieni膰 dany zas贸b maksymalnie jeden raz - nawet wykonane wielokrotnie.

Dlatego takie 偶膮danie ma sens:

```
PATCH /games/ships/turn
{
  "field": "A3"
}
```

A takie nie:

```
PUT /stats/counter

{
    "incrementBy": 1
}
```

Gdy z powodu problem贸w na sieci 偶膮danie numer dwa b臋dzie ponowione, w贸wczas otrzymamy niepoprawny stan naszego systemu.

## 15. Korzystaj z nag艂贸wk贸w do wysy艂ania meta-danych
Je艣li potrzebujesz wys艂a膰 jakie艣 meta-dane do dostawcy API nie rozszerzaj obiektu, kt贸ry wysy艂asz. Zamiast tego skorzystaj z nag艂贸wk贸w HTTP i tam umie艣膰 informacje s艂u偶膮ce do uwierzytelnienia, w艂膮czenia specjalnych flag czy przes艂ania danych diagnostycznych. Tre艣膰 偶膮da艅 pozostaw czystym. 

Pami臋taj te偶 o punkcie 11 - `GET` nie powinien mie膰 tre艣ci. W jego przypadku MUSISZ wys艂a膰 metadane w nag艂贸wkach. Wi臋c i tak nie masz wyboru :) 

## 16. BONUS - Korzystaj z test贸w kontraktowych w Springu
Teraz co艣 stricte ze [Springa](/webinar).

Co zrobi膰 偶eby upewni膰 si臋, 偶e API wystawiane przez naszego dostawc臋 dzia艂a tak jakby艣my si臋 spodziewali? Wystarczy napisa膰 testy.

Ale! Nie chodzi o testy, w kt贸rych sami definiujemy jak dzia艂a API dostawcy, ale takie w kt贸rych on sam udost臋pnia nam specjaln膮 bibliotek臋, kt贸ra jest 100% kompatybilna ze stanem faktycznym.

S艂u偶膮 do tego testy kontraktowe, kt贸re w [Springu](/spring) mo偶emy pisa膰 przy pomocy Spring Cloud Contract. Prezentacja pokazuj膮ca jego mo偶liwo艣ci znajdziecie [tutaj](https://www.youtube.com/watch?v=MDydAqL4mYE).

## Podsumowanie
Uf, to by by艂o na tyle. Mam nadziej臋, 偶e ta dawka informacji pomo偶e Ci rozwija膰 swoje REST-owe serwisy.

Dzi臋ki za wszystko!
