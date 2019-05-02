---
title: Jak pisać kod wysokiej jakości w języku Java
last_modified_at: 2019-04-24 00:37:30 +0200
permalink: /jak-pisac-kod-wysokiej-jakosci-w-jezyku-java/
header:
    teaser: /assets/images/2019/04/20_jak_pisac_dobry_kod_w_javie_artykul.jpeg
    overlay_image: /assets/images/2019/04/20_jak_pisac_dobry_kod_w_javie_artykul.jpeg
    caption: "[&copy; Javier Allegue Barros](https://unsplash.com/photos/C7B-ExXpOIE)"
excerpt: W artykule przeczytasz o tym czym jest konwencja nazewnicza. Dowiesz się jak jej stosować. Na przykładach pokażę Ci najczęściej popełniane błędy wraz z propozycjami ich poprawienia.
---

Powtórzę to po raz kolejny. Uważam, że nauka przez praktykę to najlepsze rozwiązanie. Właśnie z tego powodu artykuły na Samouczku bardzo często zawierają zadania z przykładowymi rozwiązaniami. Sporo Czytelników rozwiązuje te zadania prosząc później o spojrzenie na kod krytycznym okiem.

Tego typu praktyka spotykana jest także w codziennej pracy programisty. Przeglądy kodu (ang. _code review_) to bardzo dobry sposób na poznawanie projektu i naukę. Najlepsze w tym wszystkim jest to, że uczy się zarówno osoba, która sprawdza kod jak i ta której kod jest sprawdzany. 

Na przestrzeni kilku lat prowadzenia Samouczka widziałem już różne przypadki. W tym artykule zbieram najczęściej popełniane błędy wraz z propozycją ich rozwiązania.

Część proponowanych tu rozwiązań jest subiektywna. Nie jest poparta żadną specyfikacją czy dokładnym opisem „u źródła”. Masz prawo nie zgadzać się z moją opinią, z chęcią usłyszę Twój punkt widzenia w komentarzach.
{:.notice--info}

## Ogólne uwagi dotyczące kodu

### Konwencja nazewnicza

Zanim zacznę opisywać jakiekolwiek standardy muszę zaznaczyć jedną bardzo ważną rzecz. Jeśli w projekcie, z którym pracujesz istnieje już jakaś konwencja proponuję nadal ją stosować. Jeśli wejdziesz między wrony, musisz krakać jak i one.

Jeśli Twoim zdaniem ta konwencja jest bez sensu porozmawiaj o tym z innymi członkami zespołu. Każdy przypadek powinien być rozpatrywany indywidualnie, a konsensus może usprawiedliwić zmianę istniejącej konwencji.

W języku Java „obowiązuje” konwencja nazewnicza. Kompilator nie będzie marudził jeśli kod, który napiszesz nie będzie jej przestrzegał. Będzie marudziła kolejna osoba, która z tym kodem będzie pracowała. W praktyce często jest tak, że raz napisany kod czytany jest wielokrotnie. Często przez kogoś innego niż autor. Stosowanie konwencji nazewniczej pozwala na łatwiejsze zorientowanie się w kodzie, z którym się pracuje.

Mimo tego, że pisownia jest ważna to nie jest najważniejsza. Najbardziej istotne jest nadanie poszczególnym elementom dobrej nazwy. Pracuję w IT od 2007 roku, nadal nie potrafię tego robić dobrze. W branży IT panuje obiegowa opinia: 

> There are only two hard things in Computer Science: cache invalidation and naming things.

Istotne jest aby nazwy elementów (typów, parametrów, atrybutów, metod itd.) oddawały to co dany element zawiera/robi. Złe nazwy mogą wprowadzić w błąd, co może utrudnić zrozumienie kodu.

#### Typy

Klasy, typy wyliczeniowe, interfejsy powinny być nazwane zgodnie z [PascalCase](https://pl.wikipedia.org/wiki/PascalCase). Oznacza to tyle, że nazwy powinny być jednym ciągiem znaków, w którym każde kolejne słowo zaczyna się od wielkiej litery. Dobrze, jeśli te nazwy są rzeczownikami. Problem jest z akronimami, nawet JDK nie zachowuje tu konwencji – część akronimów pisana jest wielkimi literami (na przykład `URL`), część używając PascalCase (na przykład `Http`). W tym przypadku proponuję Ci używanie pierwszego podejścia.

Moim zdaniem przykłady poniżej pokazują nazwy, które można poprawić:

```java
// incorrect
class anonymousUser {
}

interface Bus_driver {
}

enum color {
}
```

Poprawnymi przykładami nazw mogą być:

```java
// correct
class User {
}

interface PageCollector {
}

enum URLSchema {
}
```

#### Metody, parametry, atrybuty

Metody w języku Java zwykło się nazywać używając [camelCase](https://pl.wikipedia.org/wiki/CamelCase). Oznacza to tyle, że pierwsze słowo pisane jest małą literą. Każdy kolejny wyraz zaczyna się wielką literą. Przykładami poprawnych nazw mogą być:

```java
// correct
class CodeExecutor {
    String snippet;
    int returnCode;

    Future<Integer> executeAsynchronously() {
        // ...
    }
}
```

#### Stałe

Swego rodzaju wyjątkiem od reguły są stałe – atrybuty przypisane do klasy oznaczone słowem kluczowym `final`. Te powinny być pisane wyłącznie wielkimi literami używając [SCREAMING_SNAKE_CASE](https://en.wikipedia.org/wiki/Snake_case). Poszczególne słowa pisane wielkimi literami powinny być oddzielone symbolem `_`. Na przykład:

```java
// correct
class Temperature {
    public static final double BOILING_WATER_CELSIUS = 100;
}
```

#### Pakiety

Mimo tego, że Java pozwala na używanie domyślnego pakietu (brak deklaracji `package`) nie jest to zalecane. Przyjęło się, że nazwa pakietu składa się z małych liter oddzielonych kropkami. Każdy z członów opisuje bardziej szczegółowo swoją zwartość.

Przyjęło się, że pakiety mają postać „odwróconej domeny”:

```java
// incorrect
package pckg.pl;
```
```java
// correct
package pl.samouczekprogramisty.kursjava.loops;
```

{% include newsletter-srodek.md %}

### Formatowanie kodu

Nie chcę rozpoczynać świętej wojny. Niektórzy programiści bronią formatowania, do którego są przyzwyczajeni, jak niepodległości. Mam do tego bardziej pragmatyczne podejście. Używaj formatowania kodu. Niech IDE robi to za Ciebie, nie zastanawiaj się nad tym dopóki nie zacznie Ci ono przeszkadzać. Nie chcę się tu rozpisywać nad wyższością jednego formatowania nad drugim, to nie ma sensu. Istotne jest to, że brak formatowania kodu można traktować jako złą praktykę.

Moim zdaniem dobrym podejściem jest włączenie automatycznego formatowania kodu w IDE[^wyjatek]. W zależności od tego jakiego IDE używasz ta akcja może być wykonywana na przykład przed każdym zapisem pliku czy przed każdym commit'em do repozytorium. Dzięki temu możesz w ogóle zapomnieć o formatowaniu i skupić się na innych rzeczach. IDE zrobi to za Ciebie.

Istotne jest to, żeby wszystkie osoby, które pracują w danym projekcie używały spójnego formatowania kodu. Wachlowanie się commit'ami, które polegają tylko na zmianach w formatowaniu kodu nie jest dobrym pomysłem. Formatowanie kodu to konwencja, która musi być ustalona wspólnie przez cały zespół i konsekwentnie stosowana.

[^wyjatek]: Potrafię sobie wyobrazić wyjątki od tej reguły. Załóżmy, że pracujesz nad projektem, który nie jest pierwszej młodości. Znajdują się w nim pliki mające kilka lat i kilka tysięcy linii. Musisz poprawić błąd, który sprowadza się do zmiany kilku linijek. Łączenie tej zmiany z formatowaniem całego pliku przeważnie nie jest dobrym pomysłem.

### Bloki z jedną linią

Język Java pozwala na opuszczanie nawiasów `{ }` jeśli blok ma jedną linię. Tego typu konstrukcja może być na przykład użyta po warunku `if` czy pętli. Proszę spójrz na przykład poniżej:

```java
// incorrect
if (activeUser.isAnAdmin())
    allowedActions.add(Action.DELETION);

System.out.println("Some log message");
```

Moim zdaniem to bardzo zła praktyka. Może prowadzić do trudnych do znalezienia błędów. Co jeśli tylko użytkownik, który jest administratorem powinien móc dokonywać modyfikacji? Ktoś mógłby wprowadzić drobną zmianę:

```java
// incorrect
if (activeUser.isAnAdmin())
    allowedActions.add(Action.DELETION);
    allowedActions.add(Action.MODIFICATION);

System.out.println("Some log message");
```

Problem polega na tym, że taki fragment kodu powoduje, że każdy użytkownik mógłby wykonać modyfikację. Dlatego nawet przy jednoliniowych blokach należy używać nawiasów:

```java
// correct
if (activeUser.isAnAdmin()) {
    allowedActions.add(Action.DELETION);
}

System.out.println("Some log message");
```

### Flagi

Na początku mojej przygody z programowaniem pracowałem w Eurobanku. Nie zapomnę do końca życia strony w intranecie opisującej „kwiatki w kodzie”. Kwiatki w kodzie czyli radosną twórczość programistów, która po dłuższym zastanowieniu nie ma sensu. Dość dużą część tej strony zajmowały przykłady kodu z wyrażeniami logicznymi.

Proszę spójrz na kilka złych przykładów wraz z propozycjami jak można je poprawić:

```java
// incorrect
boolean parameter = // ...
if (parameter == true) {
    // ...
}
```
```java
// correct
boolean parameter = // ...
if (parameter) {
    // ...
}
```

Podobnie wyglądać może sytuacja z odwróceniem warunku
```java
// incorrect
if (parameter == false) {
    // ...
}
```
```java
// correct
if (!parameter) {
    // ...
}
```

Spotkałem się też z uzależnieniem wartości zwracanej od zmiennej typu `boolean`:

```java
// incorrect
if (parameter == true) {
    return false;
}
else {
    return true;
}
```
```java
// correct
return !parameter;
```

Warunki logiczne często urastają do sporych potworków. Jeśli zauważysz jeden z nich, który ma zawsze taką samą wartość warto uprościć takie wyrażenie. Dzięki temu kod będzie bardziej czytelny. W przykładzie poniżej zakładam, że `variableThatAlwaysIsNull` w wyniku różnych operacji zawsze ma wartość `null`:

```java
// incorrect
Object variableThatAlwaysIsNull = methodAlwaysReturningNull();
boolean someMagicFlag = // ...

if (variableThatAlwaysIsNull == null && someMagicFlag) {
    // ...
}
```

```java
// correct
boolean someMagicFlag = // ...

if (someMagicFlag) {
    // ...
}
```

Spotkałem się też z kodem tego typu:

```java
// incorrect
if (someMagicFlag) {
}
else {
    // code to execute
}
```

Blok `if` nie zawierał żadnej linijki. Kod do wykonania znajdował się wewnątrz bloku `else`:

```java
// correct
if (!someMagicFlag) {
    // code to execute
}
```

Przykłady tego typu można mnożyć. Ważne, żeby zwracać uwagę na wyrażenia logiczne – bardzo często można je uprościć. Jeśli nie znasz [praw De Morgana](https://pl.wikipedia.org/wiki/Prawa_De_Morgana), to najwyższy czas je poznać ;).

### Duplikacja kodu

Jakiś czas temu pisałem o [regule _Don't Repeat Yourself_]({% post_url 2018-09-28-jakosc-kodu-a-oschle-pocalunki-jagny %}). Można ją zastosować na wielu poziomach. Jednym z nich jest kod źródłowy programu. Duplikacja w kodzie jest zła. Należy ją eliminować (jestem gorącym zwolennikiem usuwania kodu). Poniższy przykład pokazuje duplikację w bardzo wąskim zakresie:

```java
// incorrect
class MagicNumber {
    private int value;

    public boolean isEven() {
        return value % 2 == 0;
    }

    public boolean isOdd() {
        return value % 2 == 1;
    }
}
```
```java
// correct
class MagicNumber {
    private int value;

    public boolean isEven() {
        return value % 2 == 0;
    }

    public boolean isOdd() {
        return !isEven();
    }
}
```

### Unikanie zbędnych zagnieżdżeń

Moim zdaniem unikanie zbędnych zagnieżdżeń jest dobre. Mam tu na myśli pomijanie bloku `else`, jeśli kod wewnątrz bloku `if` na pewno zakończy działanie metody. Może się tak stać na przykład w sytuacji kiedy wewnątrz bloku `if` znajduje się `return`:

```java
// „incorrect”
if (someFlag) {
    // return/throw/break/continue
}
else {
    // something else
}
```

Moim zdaniem pomięcie `else` poprawia czytelność:

``` java
// correct
if (someFlag) {
    // return/throw/break/continue
}
// something else
```

### Metody statyczne

Metody statyczne są przypisane do klasy. Moim zdaniem warto o tym pamiętać i wywoływać metody statyczne posługując się klasą a nie jej instancją:

```java
// incorrect
SomeClass instance = new SomeClass();
instance.staticMethod();
```

```java
// correct
SomeClass.staticMethod();
```

### `import *`

Kolejny subiektywny punkt. Nie podchodzą mi klasy/metody statyczne importowane przy pomocy `*`. Pewnie wynika to trochę z filozofii jaką proponuje Python –  [_explicit is better than implicit_](https://www.python.org/dev/peps/pep-0020/).

```java
// incorrect
import static java.lang.Math.*;
```

```java
// correct
import static java.lang.Math.sqrt;
import static java.lang.Math.pow;
```

## Projekt

Problemy i złe praktyki na poziomie poszczególnych plików to czubek góry lodowej. Pod spodem kryją się większe problemy. Problemy związane z podejściem do samego projektu.

### Brak systemu kontroli wersji

Piszesz kod bez używania systemu kontroli wersji? Robisz błąd. System kontroli wersji jest narzędziem niezbędnym w pracy każdego programisty. Polecam Ci [Git'a]({{ '/kurs-git/' | absolute_url }}), który moim zdaniem jest standardem w branży.

### Brak testów jednostkowych

Piszesz kod bez testów jednostkowych? Robisz błąd. Moim zdaniem automatyczne testy jednostkowe w wielu przypadkach są niezbędne. Nie będę się tu rozwodził nad tematyką testów. Zachęcam Cię do przeczytania artykułów:

* [Wprowadzenie do tematyki testów jednostkowych na przykładzie JUnit4]({% post_url 2016-10-29-testy-jednostkowe-z-junit %}) – jeśli nigdy nie udało Ci się pracować z testami zacznij od tego artykułu, 
* [Opis biblioteki JUnit5]({% post_url 2018-04-13-testy-jednostkowe-z-junit5 %}) – ten artykuł opisuje bibliotekę JUnit5,
* [Test Driven Development]({% post_url 2016-11-21-test-driven-development-na-przykladzie %}) – jak poznasz już bibliotekę do pisania testów czas zabrać się za TDD.

### Zła organizacja kodu

Na ten temat powstają mądre książki. Dobrym początkiem będzie zapoznanie się z zasadami [SOLID]({% post_url 2017-11-27-programowanie-obiektowe-solid %}) i ich świadome stosowanie w pracy z kodem.

### Brak standardowego mechanizmu budowania

W idealnym świecie zbudowanie projektu powinno składać się z dwóch etapów:

1. Pobrania źródeł projektu, na przykład z [repozytorium Git'a]({{ '/kurs-git/' | absolute_url }}),
2. Uruchomienia narzędzia do budowania, które na podstawie plików konfiguracyjnych zbuduje projekt. 

Oba etapy powinny działać niezależnie od środowiska programisty. Drugi punkt rozwiązywany jest przez narzędzia takie jak Maven, Make, Rake, Gradle, Ant, Grunt itp. Jeśli do tej pory nie udało Ci się pracować z narzędziami tego typu zachęcam Cię do zajrzenia do artykułów opisujących Gradle:

* [Wstęp do Gradle]({% post_url 2017-01-19-wstep-do-gradle %}),
* [Pierwszy projekt z Gradle]({% post_url 2019-03-22-pierwszy-projekt-z-gradle %}).

### Niestandardowa struktura projektu

Organizacja plików w projekcie jest ważna. Podobnie jak z nazewnictwem czy formatowaniem kodu istnieje pewna konwencja, która pozwala na szybkie zorientowanie się w strukturze projektu. Niejako powiązane z tym tematem jest używanie narzędzie wspomagającego budowanie projektu, które „narzucają” używanie pewnych konwencji. Standardową strukturę projektu opisałem we [wstępie do Gradle]({% post_url 2017-01-19-wstep-do-gradle %}).

## Martwy kod

Historia w repozytorium jest od tego, żeby pamiętać co działo się w projekcie. Fragmenty kodu w komentarzu, które „może kiedyś się przydadzą” moim zdaniem powinny od razu wylecieć w kosmos. Nie są potrzebne, jedynie zaciemniają obraz. 

Kilka poniższych podpunktów opisuje różne przypadki, które można podsumować w jednym zdaniu: nie jest sztuką napisać dużo kodu, sztuką jest napisać jak najmniej czytelnego i zrozumiałego kodu, który robi to samo. Jeśli masz możliwość usunięcia czegoś, co nie jest używane zrób to! :) Mniej kodu oznacza mniej potencjalnych błędów. Mniej kodu, to niższy koszt jego utrzymania[^czytelny].

[^czytelny]: Jak napisałem wcześniej – zakładam, że kod jest napisany w sposób czytelny i zrozumiały. Nie chodzi mi tu o sytuację, w której używasz jednoliterowych nazw metod, żeby „było mniej kodu”.

Często jest tak, że fragmenty martwego kodu narastają z czasem – wynikają z kilku zmian wprowadzonych na przestrzeni życia projektu. Odwaga do usuwania danej linijki kodu jest odwrotnie proporcjonalna do jej wieku ;).

### Kod, który nigdy nie będzie wykonany

Ten punkt jest powiązany z flagami, które poruszałem wcześniej. Po uproszczeniu warunków logicznych możesz czasami zauważyć, że dotarcie do pewnych fragmentów kodu jest po prostu niemożliwe. W podstawowych przypadkach IDE potrafi pokazać takie fragmenty kodu jako martwe. Dobrym pomysłem jest usunięcie śmieci tego typu.

### Niepotrzebne parametry i atrybuty

Widzisz metodę, która ma nieużywany parametr? Zastanów się czy możesz go usunąć. Jeśli tak, to wiesz co masz zrobić ;). Podobną regułę trzeba stosować przy atrybutach klas.

Zwróć szczególną uwagę na zmianę sygnatury metody. Tego typu zmiany mogą prowadzić do „dziwnych zachowań”. Mam tu na myśli sytuację, w której metoda nadpisywała inną w klasie bazowej. Tu drobne ćwiczenie dla Ciebie – czym może skończyć się takie usunięcie parametru?

Usuwanie atrybutów, to też coś co wymaga pewnej analizy. W zależności od [modyfikatora dostępu]({% post_url 2017-10-29-modyfikatory-dostepu-w-jezyku-java %}) może, ale nie musi, łamać kompatybilność wsteczną.

### Zbędne metody

Nie zrozum mnie źle. Uważam, że nieduże metody są dobre. Jednak także i tutaj trzeba zachować zdrowy rozsądek. Proszę spójrz na przykład poniżej, używa on klasy [`Math`](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/Math.html):

```java
// incorrect
double someVeryImportantCalculation(double argument0, double argument1) {
    return argument0 + sqrt(argument1);
}

double sqrt(double argument) {
    return Math.sqrt(argument);
}
```

Moim zdaniem w tym przypadku wprowadzenie metody `sqrt` nic nie wnosi. Równie dobrze w miejscu jej wywołania można byłoby użyć `Math.sqrt`.

```java
// correct
double someVeryImportantCalculation(double argument0, double argument1) {
    return argument0 + Math.sqrt(argument1);
}
```

### Nieużywana wartość zwracana

Widziałem przypadki, w których metoda wywoływana dla efektów ubocznych[^uboczne] zwracała wartość. Ta zwrócona wartość nie była w ogóle wykorzystywana. Moim zdaniem warto uprościć taką metodę usuwając wartość zwracaną:

[^uboczne]: Abstrahując od tego czy metody posiadające efekty uboczne są w porządku czy nie.

```java
// incorrect
class User {
    String login() {
        try {
            callingExternalServiceToLogin();
        }
        catch (LoginException e) {
            // handling exception
        }
        return "logged in";
    }
}
```
```java
// correct
class User {
    void login() {
        try {
            callingExternalServiceToLogin();
        }
        catch (LoginException e) {
            // handling exception
        }
    }
}
```

## Wydajność

### Przedwczesna optymalizacja

Tutaj nie mam przykładu z zadań na blogu, jednak nadal warto wspomnieć o tym błędzie. W świecie programistów panuje przekonanie, że „przedwczesna optymalizacja jest źródłem całego zła”[^knuth]. Podpisuję się pod tym obiema rękami. Kompilator Java jest na tyle zaawansowany, że potrafi zrobić cuda, tak żeby nasz kod był bardziej wydajny.

[^knuth]: Cytat pochodzi z książki autorstwa [Donalda Knuth'a](https://en.wikiquote.org/wiki/Donald_Knuth#Computer_Programming_as_an_Art_(1974)).

Zacznij od pisania zrozumiałego i czytelnego kodu. Dopiero gdy zauważysz, że pojawiają się problemy wydajnościowe wprowadzaj optymalizacje. Istotne jest żeby wprowadzać takie zmiany na podstawie twardych dowodów – przeprowadzonych testów wydajnościowych.

Jest to ważne, bo może zdarzyć się tak, że intuicja nawet doświadczonych programistów nie sprawdza się w praktyce. Przez co wprowadzona optymalizacja ma znikomy (zerowy?) wpływ na wydajność, a sprawia, że kod jest zupełnie niezrozumiały.

### Tworzenie nadmiarowych obiektów

Im mniej obiektów, tym mniej zajętej pamięci. Jeśli możesz użyć obiektu wielokrotnie zrób to, nie ma sensu tworzyć nowej instancji dla każdego wywołania. Tutaj sprawa trochę się komplikuje. Wszystko przez [wątki]({% post_url 2019-02-11-watki-w-jezyku-java %}) i współdzielenie instancji pomiędzy nimi. Jeśli instancja obiektu będzie współdzielona pomiędzy wątkami należy upewnić się, że kod jej klasy napisany jest w wielowątkowo bezpieczny sposób.

Uproszczony przykład tworzenia nadmiarowej instancji:

```java
// incorrect
class UserInput {
    public String get(String prompt) {
        System.out.println(prompt);
        Scanner scanner = new Scanner(System.in);
        return scanner.next();
    }
}
```
```java
// correct
class UserInput {
    private Scanner scanner = new Scanner(System.in);

    public String get(String prompt) {
        System.out.println(prompt);
        return scanner.next();
    }
}
```

## Znajomość JDK

Znajomość bibliotek i API przychodzi z czasem. Nie ma sensu uczyć się tego na pamięć. Poniżej zebrałem najczęściej spotykane błędy powiązane z klasami dostarczonym wraz z JDK.

### `System.in`, `System.out`, `System.err`

Wspomniany wyżej [`Scanner`](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/Scanner.html) jest bardzo często używany do pobierania danych od użytkownika. Jednym ze sposobów utworzenia instancji tej klasy jest przekazanie jej instancji [`InputStream`](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/io/InputStream.html). Możesz na przykład użyć `System.in`. Proszę spójrz na przykład poniżej:

```java
// incorrect
public static void main(String[] args) {
    try(Scanner s = new Scanner(System.in)) {
        System.out.println(s.next());
    }
    try(Scanner s = new Scanner(System.in)) {
        System.out.println(s.next());
    }
}
```

Ten kod jest zły z dwóch powodów. Pierwszy to wyżej wspomniane tworzenie dwóch instancji klasy `Scanner`, w tym przypadku spokojne wystarczy jeden obiekt i jego użycie wiele razy. Drugim, poważniejszym błędem jest zamykanie `System.in`. Dzieje się tak, ponieważ po wyjściu z bloku [_try with resources_]({% post_url 2016-08-25-konstrukcja-try-with-resources-w-jezyku-java %}) na instancji `s` wywoływana jest metoda `close`. Powoduje to zamknięcie `System.in`. W ramach ćwiczenia uruchom powyższy kod i zobacz jaki będzie jego efekt.

Nie jest to dobra praktyka. To wirtualna maszyna Javy otwiera ten strumień i to ona jest odpowiedzialna za jego zamknięcie. Sprawa wygląda podobnie w przypadku strumieni `System.out` czy `System.err`.

Jeśli chcesz przeczytać więcej o stdout, stderr i stdin w trochę innym kontekście zapraszam do przeczytania artykułu opisującego [początki pracy z linią poleceń]({% post_url 2019-03-12-poczatki-pracy-z-konsola %}#standardowe-wej%C5%9Bcie-i-wyj%C5%9Bcie).

Poniżej możesz zobaczyć poprawiony fragment kodu:

```java
// correct
public static void main(String[] args) {
    Scanner s = new Scanner(System.in);
    System.out.println(s.next());
    System.out.println(s.next());
}
```

### Znak końca linii

Java pozwala tworzyć programy, które mogą być uruchamiane na różnych systemach operacyjnych. Żeby programy te działały w pełni poprawnie trzeba brać pod uwagę różnice, które występują pomiędzy nimi. 

Sztandarowym przykładem jest tutaj znak końca linii. W zależności od systemu operacyjnego inny ciąg znaków odpowiedzialny jest za łamanie linii. Poniższy przykład pokazuje błąd i jego rozwiązanie:

```java
// incorrect
System.out.println("This is a list:\n- item.");
```
```java
// correct
System.out.println("This is a list:" + System.lineSeparator() + "- item.");
```

### Kompilacja wyrażenia regularnego

[Wyrażenia regularne]({% post_url 2016-11-28-wyrazenia-regularne-w-jezyku-java %}) i [bardziej zaawansowane wyrażenia regularne]({% post_url 2017-01-06-wyrazenia-regularne-czesc-2 %}) były już poruszane na blogu.

Tutaj chciałbym zwrócić na jeden drobny szczegół. Proszę rzuć okiem na kod poniżej:

```java
// incorrect
class Postcode {
    public static boolean isValid(String postcode) {
        Pattern postcodePattern = Pattern.compile("^\\d{2}-\\d{3}$");
        Matcher matcher = postcodePattern.matcher(postcode);
        return matcher.find();
    }
}
```

Kompilacja wyrażenia regularnego jest procesem długotrwałym. Jeśli jest taka możliwość to warto wykonywać tę czynność tylko raz:

```java
// correct
class Postcode {
    public static final Pattern PATTERN = Pattern.compile("^\\d{2}-\\d{3}$");

    public static boolean isValid(String postcode) {
        Matcher matcher = PATTERN.matcher(postcode);
        return matcher.find();
    }
}
```

### Znajomość wbudowanych wyjątków

Java dostarcza cały szereg gotowych klas [wyjątków]({% post_url 2016-01-31-wyjatki-w-jezyku-java %}). Czasami nie ma sensu tworzenie własnego dedykowanego wyjątku – warto użyć jednego z istniejących. Dobrym przykładem jest użycie wyjątku [`IllegalArgumentException`](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/IllegalArgumentException.html) jeśli chcesz zasygnalizować niepoprawny argument.

Dodatkowo ważne jest żeby rzucać wyjątki, które pasują do danej sytuacji. Na przykład rzucenie wyjątku [`IllegalStateException`](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/IllegalStateException.html) w sytuacji gdy podano błędny argument nie jest najlepszym rozwiązaniem.

### `java.util.Date` i spółka

Gdzie tylko się da omijaj stare API do zarządzania datami szerokim łukiem. Na przykład instancje `java.util.Date` nie są wielowątkowo bezpieczne, API jest zagmatwane, obsługa stref czasowych wymaga więcej pracy.

Skup się na poznaniu [`LocalDateTime`](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/time/LocalDateTime.html) i jej podobnych.

### Konstrukcje języka

Konstrukcje języka nie są związane z API a składnią jaką język oferuje. Java ewoluuje jak każdy język. W kolejnych wersjach wprowadza nowe elementy. Warto z nich korzystać. Za przykład mogą tu posłużyć [wyrażenia lambda](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/IllegalStateException.html), wyrażenia `switch`, zmienne lokalne przy użyciu `var`, konstrukcja [_try with resources_]({% post_url 2016-08-25-konstrukcja-try-with-resources-w-jezyku-java %}) i tak dalej ;).

## Dodatkowe materiały do nauki

Do tej pory nie nazwałem tego wprost. Wprowadzanie zmian, które nie modyfikują zachowania programu to tak zwana refaktoryzacja. Zacznij od przeczytania czym jest refaktoryzacja w [artykule na Wikipedii](https://pl.wikipedia.org/wiki/Refaktoryzacja). Później możesz sięgnąć po książkę [_Refactoring_ autorstwa Martin'a Fowler'a](https://martinfowler.com/articles/refactoring-2nd-ed.html). Pierwsza edycja zawiera przykłady w Javie, druga w JavaScript.

Możesz też rzucić okiem na dość stary dokument opisujący [konwencja nazewnicza w języku Java](https://www.oracle.com/technetwork/java/codeconventions-150003.pdf). Opisuje on też zalecane formatowanie kodu.

W treści artykułu wspomniałem o [prawach De Morgana](https://pl.wikipedia.org/wiki/Prawa_De_Morgana). To podstawa, jak już je poznasz warto poczytać więcej o [algebrze Boole'a](https://pl.wikipedia.org/wiki/Algebra_Boole%E2%80%99a) i wzorach pozwalających na upraszczanie wyrażeń logicznych.

## Podsumowanie

Moją motywacją do napisania tego artykułu było zebranie w jednym miejscu błędów i propozycji ich rozwiązania. Temat bynajmniej nie jest wyczerpany. Większość z tych punktów można rozbudować podając więcej przykładów. 

Jednak nawet w obecnej formie artykuł pokazał Ci większość klas „podstawowych błędów”. Po jego lekturze wiesz jak można je poprawić. Stosując się do zaleceń, które tu zebrałem Twój kod będzie na pewno wyższej jakości. Z góry gratuluję ;). 

Jeśli znasz kogoś dla kogo ten artykuł byłby pomocny proszę podziel się linkiem. Dzięki temu pomożesz mi dotrzeć do nowych Czytelników, za co od razu bardzo dziękuję! 

Jeśli nie chcesz pomiąć kolejnych artykułów polub [Samouczka na Facebooku](https://www.facebook.com/SamouczekProgramisty/) i dopisz się do samouczkowego newslettera. To tyle na dzisiaj, trzymaj się i do następnego razu!