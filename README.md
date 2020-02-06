# Podstawowa wiedza BACKENDOWA 

# Java 

## Frameworki
- Spring 
- JEE (Java Enterprise Edition)
- Play
- Ktor (dla Kotlina)

## Mikroserwisy
(https://feras.blog/microservices-architecture-to-be-or-not-to-be/)

Generalnie mikroserwis to apka, ma swoja baze danych, wykonuje operacje itd.

Kazdy serwis powinien miec swoja baze danych, moga miec tez wspolna i nierzadko maja - ale to nie jest dobra praktyka. 

Serwis wyciaga dane ze swojej bazy danych i serwuje to dalej.

Przyklady mikroserwisow Spring: https://spring.io/guides

### Komunikacja miedzy serwisami
- REST - wysylaja JSON
- SOAP - wysylaja XML
- kolejka komunikatow (message broker) - czyli coś jak Redux miedzy serwisami - jak cos sie wydarzyło to generowany jest event a uslugi zainteresowane subskrybuja sie na to
- inne...

### API Gateway
Jest to apka, w ktorej konfiguruje się gdzie uderzyc jak wpadnie dany adres (jak request wpadnie to potem leci do odpowiedniego serwisu). 

Chodzi tez o to, by z frontu nie strzelac w rozne miejsca.

Przykłady:
- Nginx
- Varnish

Więc w mikroserwisach API Gateway musi sprawdzić gdzie to żądanie ma trafić, znaleźć usługę, bo nie wiadomo gdzie obecnie stoi i przekazać to bezposrednio do niej.

### Autentykacja
Mozna zrobic oddzielny serwis, ale jest tez mnostwo gotowych. 

Spring ma metode na stworzenie SSO -  Single Sign On. 

Czyli nie moge logowac sie do kazdego mikroserwisu osobno, wiec loguje sie w jednym miejscu i przekazuje token Bearer. 

Kazdy kto sie nim okaze, dziala w imieniu zalogowanego uzytkownika.

Wiec jak jak uderze do serwisu z tokenem to potem ten serwis przekaże go do innego i będzie wiadomo, że chodzi o twoje dane.

### Inne
Kazdy mikroserwis moze byc napisany w innej technologii/jezyku, jednakze generuje to problemy konfiguracyjne. 
Im wiecej technologii, tym wiecej utrzymania. 

Do mikroserwisów musze mieć: 
- gateway, 
- service discovery (apka wstaje na losowej maszynie), 
- oAuth (KeyCloak)

Jak taka apka się uruchamia to musi pobrać konfigurację z Config Service, zarejestrować się w Service Discovery i dopiero można do niej uderzyć,
bo jak pytasz o coś via REST z innej usługi to ona najpierw odpytuje Discovery gdzie są te usługi, a potem dopiero uderza pod adres.

Do tego dochodzi Load Balancer, Circuit Breaker, więc im więcej technologii tym ciężej zrobić upgrade.

## Inne

### Terminologia 
Java SE - to zwykła Java

Maven - menager paczek, coś a'la NPM w JSie

JVM - Java Virtual Machine, czyli wirtualizacja komputera, dzięki temu mogę pisać aplikacje na Linux, Windows. 
    Niezalenie jak działa system, jvm bierze na siebie obsługę programu w roznych srodowiskach.

Tomcat - czyli serwer. Java sama w sobie nie ma serwera, stawiam ją na Tomcat (odpowiednik Apache dla PHP).

Spring Boot - nakładka która ogarnia całą konfigurację za mnie, ma wbudowanego Tomcata. 
    Uruchamia apkę, która uruchamia wewnatrz serwer i sam sie do niego wdraza. (https://start.spring.io/)

Aplikacja Javy domyślnie generuje plik JAR (raczej wszyscy uzywaja w swiecie mikroserwisow), jednakze moge zmienic na WAR i wtedy potrzebuje Tomcat do odpalenia takiej aplikacji.

JAR - Java Archive - wszystko co napisalem w Java jest skompilowane i spakowane do takiego archiwum, wlacznie z bibliotekami, one potem sa zaciagane przez Gradle lub Maven. Jest to taki koncowy bundle. Webowe apki moga byc jeszcze pakowane do WAR albo EAR ale w Spring Boot najczesciej jest JAR.

Hibernate - framework zmieniający model kolumnowy z baz danych (?) na klasy javowe

### Logi
Kibana - jest częścią stacku ELK, służy do zbierania logów aplikacji - mozemy sprawdzac czy cos sie gdzies nie wypieprzylo

Prometheus - zbiera metryki np. czasy odpowiedzi, ilość 500, 200 itd.

Grafana - prezentuje metryki

Więc właśnie do logowania jest stack ELK, gdzie:
- E - Elasticsearch, tam są ładowane logi
- L - Logstash - on jest po stronie serwisu skonfigurowany i wypycha logi do elastika
- K - Kibana to przeglądarka wygodna do elastika.
