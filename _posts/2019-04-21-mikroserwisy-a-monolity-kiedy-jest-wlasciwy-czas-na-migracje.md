---
layout: post
title: Mikroserwisy a Monolity - Kiedy Jest Właściwy Czas Na Migrację?
description: 
image: /images/mikroserwisy.jpg
tags: [mikroserwisy, architektura, praktyki]
---

Od dobrych kilku lat mikroserwisy znajdują się w czołówce tematów rozmów w środowisku programistycznym. Programiści marzą, by pracować w tej architekturze, a konferencje wypełnione są prezentacjami na jej temat. Po pewnym czasie pojawiły się jednak głosy, że mikroserwisy nie są lekiem na wszystko i czasami lepiej jest tworzyć aplikację w formie monolitu. Kiedy zatem nadchodzi ten właściwy moment, by przeprowadzić migrację z jednego dużego serwisu na wiele mniejszych?

**Mikroserwisy** to architektura, w której jedna aplikacja stworzona jest z wielu fizycznie działających programów, współpracujących i komunikujących się ze sobą. Podejście to umożliwia podział systemu na mniejsze części, pozwala na niezależne rozwijanie jego funkcji i tworzenie mniejszych, bardziej zwinnych zespołów. W monolitach zaś cały system obsługiwany jest przez jedną aplikację, będącą efektem pracy wielu osób współpracujących ze sobą. Ze względu na jej rozmiary czas potrzebny na opublikowanie nowej wersji jest dłuższy, a koordynacja prac między zespołami programistycznymi wymaga dużo wysiłku.

Jako dobrą praktykę od kilku lat rekomenduje się rozpoczęcie tworzenia swojego systemu w formie monolitu, a dopiero po pewnym czasie przejście na architekturę mikroserwisową. Ale kiedy właściwie jest odpowiedni moment na taką zmianę? Oto 6 sygnałów świadczących o tym, że właśnie nadszedł.

## 1. Rosnąca złożoność aplikacji

We wczesnej fazie rozwoju prace szybko posuwają się do przodu, a nowe funkcje dostarczane są klientom często. Jesteśmy w stanie wprowadzać nowe wersje systemu co najmniej kilka razy w miesiącu, a czasem i częściej.
Zespół programistyczny nie jest duży, wszyscy dobrze się znają i wspólnie rozwijają aplikację. Z czasem jednak, gdy rośnie liczba klientów, a z nimi liczba wymagań, wzrasta też złożoność aplikacji. Poza podstawowymi funkcjami musi ona wspierać dodatkowe, takie jak raportowanie, administrację systemu, logowanie zdarzeń czy wysyłanie maili. Powoli nasz zespół dzieli się na mniejsze zespoły, które zaczynają się specjalizować w danych obszarach aplikacji. Coraz trudniej jest pracować nad jednym kodem źródłowym, którego złożoność sprawia, że nowi programiści mają problem, by zrozumieć wszystkie zawarte w nim powiązania.

## 2. Tworzenie nowych funkcji jest wolne

Wraz ze wzrostem złożoności wydłuża się czas implementacji nowych funkcji. Codzienne zadania programistyczne zajmują coraz więcej czasu. Budowanie, ładowanie się i testowanie aplikacji z każdą kolejną zmianą trwają dłużej.
Czas od implementacji nowej funkcji do jej uruchomienia wydłuża się, następuje spadek produktywności programistów. To, co kiedyś udawało się dostarczyć w kilka dni albo tygodni, zajmuje teraz miesiące, a niekiedy lata. Konieczność spłaty długu technicznego i poprawy kolejnych błędów nie poprawia sytuacji.

## 3. Czas wdrożenia nowych funkcji jest coraz dłuższy

Kolejnym problemem związanym z monolitami jest wydłużający się czas potrzebny do wdrożenia nowych funkcji na produkcję. Częstą praktyką jest wypuszczanie nowych wersji raz w miesiącu, a nawet rzadziej.
Ponieważ wiele osób modyfikuje ten sam kod źródłowy, aplikacja często znajduje się w stanie niestabilnym.
Mimo wprowadzenia pracy na osobnych gałęziach kodu (ang. *feature branch*) nadal dochodzi do konfliktów podczas łączenia pracy zespołów programistycznych.
Sygnałem ostrzegawczym mogą też być wydłużające się testy automatyczne, które w niektórych przypadkach potrafią trwać godzinami.
Jest to kolejny znak wskazujący na to, że aplikacja przestaje być utrzymywalna w formie monolitu.

## 4. Utrudnione skalowanie aplikacji

Coraz większa aplikacja monolityczna to coraz większe problemy ze skalowalnością. Rosnące wymagania na takie zasoby jak procesor, pamięć RAM czy miejsce na dysku mogą być coraz trudniejsze do spełnienia przy jednej dużej aplikacji.
Kolejne funkcje systemu mają inne potrzeby. Część analityczna będzie bardziej polegać na wydajnym procesorze, a wzrost liczby użytkowników będzie prowadzić do większego zapotrzebowania na pamięć RAM. W efekcie dobranie odpowiednich maszyn, które pozwolą na wydajną pracę całej aplikacji, może być zadaniem nieosiągalnym lub wiązać się z dużymi kosztami. Prowadzi to do wyboru rozwiązań kompromisowych, które nie spełniają satysfakcjonująco żadnego z założeń.

## 5. Utrudnione utrzymywanie niezawodności aplikacji

Tworzenie niezawodnych aplikacji jest trudne. Niektórzy mówią, że nawet niemożliwe. A co, jeśli mamy do czynienia z bardzo dużym, rosnącym każdego dnia kodem źródłowym? Nieuchronnie doprowadzimy do dużej liczby błędów i przerw w działaniu na produkcji. Skomplikowanie aplikacji sprawia, że programiści gubią się we wszystkich zależnościach, testowanie trwa coraz dłużej, a eliminacja 100% błędów staje się niemożliwa. W ten sposób błędy wślizgują się na produkcję. Co więcej, aplikacja nie zapewnia izolacji awarii, co sprawia, że pojawiają się okresy, kiedy cały system nie działa. Programiści coraz częściej muszą być dostępni na dyżurach, a liczba incydentów wraz z upływem kolejnych miesięcy rośnie, zamiast maleć.

## 6. Przywiązanie się do starzejącego się stosu technologicznego

To ostatni ważny problem dotyczący architektury monolitycznej. Wiesz, że coś jest na rzeczy, gdy na konferencjach i spotkaniach lokalnych grup programistycznych zaczynają padać żarty z technologii, w których ty nadal pracujesz. Tworzenie dużej aplikacji przez wiele lat jest bardzo kosztowne. Duży i skomplikowany kod źródłowy utrudnia migrację do nowszych wersji języków programowania, frameworków czy bibliotek. Przepisanie aplikacji na nowsze technologie może trwać latami i oznaczać ogromne koszty. Z jednej strony mamy więc starzejące się technologie, a z drugiej – perspektywę ponoszenia ogromnych wydatków związanych z wdrażaniem ich nowszych wersji. Koszty te w monolitach są o wiele wyższe niż w mikroserwisach.

## Podsumowanie

Tworzenie aplikacji w formie monolitu to dobry pomysł… na początek. Z czasem, gdy Twoja aplikacja odnosi sukces, a jej rozwój zwalnia, warto zastanowić się nad inwestycją w architekturę mikroserwisów. W tym wpisie przedstawiłem 6 sygnałów świadczących o tym, że nadszedł na to czas. A kiedy Twoim zdaniem warto migrować na mikroserwisy? Będę bardzo wdzięczny za słowo komentarza pod tym artykułem ;)
