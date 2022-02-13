# O dobrym programowaniu w Pythonie

W tym dokumencie prostuję najczęściej spotykane uchybienia
wobec dobrego stylu programowania w Pythonie 3.x

Jestem wdzięczny zarówno programistom, od których nauczyłem się
poniższych zasad, jak i studentom, których uczyłem:
poniżej streszczam te zasady, których nieznajomość
najczęściej dostrzegałem w programach zaliczeniowych.

![Znak domeny publicznej](http://i.creativecommons.org/p/mark/1.0/88x31.png)

Przekazuję niniejszy dokument do domeny publicznej.
Wolno go zwielokrotniać, zmieniać i rozpowszechniać,
nawet w celach komercyjnych, bez pytania o zgodę.

Do czytelników, którym przydadzą się poniższe rady,
mam osobistą prośbę: pomyślcie o jakiejś osobie,
która okazała wam dobroć, i podziękujcie jej osobiście.

### 1. Pliki

* Repozytorium nie powinno zawierać zbędnych katalogów
`__pycache__`, `.idea`, `venv` itp. ani zbędnych plików,
np. `desktop.ini`

* Wskazane jest za to dodanie pliku `.gitignore`
o treści odpowiedniej dla projektów pisanych w Pythonie.
Osoby, które nie dodały tego pliku przy zakładaniu repozytorium,
[tutaj](https://github.com/github/gitignore/blob/master/Python.gitignore)
znajdą odpowiednią treść.

* Nazwy bibliotek zewnętrznych niezbędnych do działania programu
umieszczamy w pliku `requirements.txt`
(zewnętrzne znaczy „nie te, które zainstalowaliśmy razem z Pythonem”;
nie zakładamy pliku `requirements.txt`, jeśli byłby pusty).
Dzięki temu przez polecenie `pip install -r requirements.txt`
można je wszystkie naraz zainstalować.
[Tutaj](https://pip.pypa.io/en/stable/reference/pip_install/#example-requirements-file)
jest przykładowa treść pliku `requirements.txt`. Przypuszczam,
że w większości Państwa projektów wystarczy wymienić
nazwy bibliotek bez uściślania ich wersji, np.
    ```
    numpy
    pygame
    ```

### 2. Uwagi drobiazgowe

Zachęcam do zapoznania się z treścią
[poradnika dla programistów Pythona w firmie Google](https://google.github.io/styleguide/pyguide.html)
i stosowania się do jego zasad. Większa część tego poradnika
powtarza i objaśnia treść dokumentu
[PEP 8](https://www.python.org/dev/peps/pep-0008/),
który uznają wszystcy programiści Pythona.

Nie wymagam tylko podawania w docstringach funkcji
i metod sekcji `Args:`, `Returns:` i `Raises:`
ani w docstringach klas sekcji `Attributes:`
([punkty 3.8.3 i 3.8.4](https://google.github.io/styleguide/pyguide.html#383-functions-and-methods)).
Nie obowiązują również zasady związane z Pythonem 2,
czyli nie należy pisać `from __future__ import ...`,
dziedziczyć z `object` ani korzystać z biblioteki `six`.

Najczęściej ignorowane zasady powtarzam poniżej.

* [Pylint](https://pypi.org/project/pylint/)
jest Państwa przyjacielem. Proszę go zainstalować,
przepuszczać przez niego swój kod
i poprawiać wskazane przez niego miejsca
([punkt 2.1](https://google.github.io/styleguide/pyguide.html#21-lint)).
Gdyby wszyscy z Państwa słuchali rad Pylinta,
większość poniższych punktów byłaby niepotrzebna.

* Proszę używać instrukcji `from ... import ...`
tylko do importowania modułów z pakietów,
a nie poszczególnych klas, funkcji czy stałych z modułów
([punkt 2.2](https://google.github.io/styleguide/pyguide.html#22-imports)),
bo długi wykaz potrzebnych identyfikatorów
`from grocery import spam, ham, eggs, cheese...`
jest niewygodny, a krótki nie ma przewagi nad `import grocery`.
A już zdecydowanie proszę nigdy nie pisać `from grocery import *`,
bo zaśmiecanie przestrzeni nazw nieokreśloną liczbą
nieokreślonych identyfikatorów dezorientuje czytelnika programu.
Rozumiem, że ta zasada może budzić Państwa opór,
ale my tu symulujemy pracę w firmie.
W projektach tworzonych przez zespół
lepsze są zbyt sztywne reguły od braku reguł.
Poza tym standardy firm mniejszego kalibru niż Google
bywają bardziej niedorzeczne. Proszę się przyzwyczajać.

* Można rozdzielać pustymi wierszami sąsiednie sekcje
złożone z instrukcji `import`. Sekcje powinnny dotyczyć
kolejno: biblioteki standardowej, bibliotek zewnętrznych
i naszych własnych modułów. Nazwy plików i modułów
w każdej sekcji porządkujemy leksykograficznie
([punkt 3.13](https://google.github.io/styleguide/pyguide.html#313-imports-formatting)).

* Nie ma przymusu pisania czegokolwiek zaraz po nawiasie
okrągłym otwierającym listę parametrów
([punkt 3.4](https://google.github.io/styleguide/pyguide.html#s3.4-indentation)).
Zamiast
```python
    bardzo_długa_nazwa = bardzo_długi_tasiemiec.zjedz(mielonka,
                                                      szynka,
                                                      jaja)
```
korzystniej wygląda
```python
    bardzo_długa_nazwa = bardzo_długi_tasiemiec.zjedz(
        mielonka, szynka, jaja)
```

* Wystrzegamy się zmiennych globalnych
([punkt 2.5](https://google.github.io/styleguide/pyguide.html#25-global-variables)).
Często da się ich pozbyć przez zmianę architektury
kodu z proceduralnej na obiektową, dzięki czemu
stają się atrybutami instancji klasy.

* Globalne stałe są natomiast mile widziane,
a nawet wymagane zamiast magicznych liczb, napisów
i większych obiektów
(artykuł [Magic number](https://en.wikipedia.org/wiki/Magic_number_(programming)#Unnamed_numerical_constants) w Wikipedii).
Na przykład zamiast `7` definiujemy stałą
`BOARD_WIDTH = 7`, zamiast odróżniania kierunku
przy użyciu zmiennej o wartości `'left'` albo `'right'`
— definiujemy stałe `LEFT, RIGHT = range(2)`
(miłośnikom nowości polecam
[`enum.Enum`](https://docs.python.org/3/library/enum.html#creating-an-enum)),
zamiast krotki `(38, 139, 210)` — definiujemy stałą
`BACKGROUND_COLOR = (38, 139, 210)`.
Nazwa stałej powinna odpowiadać jej zastosowaniu,
a nie zawartości: gdyby ostatnia stała nosiła nazwę `BLUE`,
doszłoby do absurdu, gdybyśmy kiedyś chcieli
zmienić tło na pomarańczowe. Stałe zwyczajowo
zapisujemy `WIELKIMI_LITERAMI_Z_PODKREŚLNIKAMI`
([punkt 3.16.4](https://google.github.io/styleguide/pyguide.html#3164-guidelines-derived-from-guidos-recommendations)).
Nie od rzeczy będzie przypomnieć, że XXI wiek
trwa już dość długo i w identyfikatorach można swobodnie
korzystać z polskich liter, jeśli ktoś ma taką ochotę.

* Nazwy funkcji i zmiennych w `styluWielbłądzim`
(*`camelCase`*) są niepytoniczne.
Powinny być w `stylu_wężowym` (*`snake_case`*)
([punkt 3.16.4](https://google.github.io/styleguide/pyguide.html#3164-guidelines-derived-from-guidos-recommendations)).

* Ponieważ z nazw plików `*.py` i katalogów z nimi
powstają nazwy modułów i pakietów,
do nich też lepiej pasuje `styl_wężowy`
([punkt 3.16](https://google.github.io/styleguide/pyguide.html#316-naming)).

* Warto pamiętać, że czytelnik programu nie musi
znać tych samych skrótów, co my. Wniosek pierwszy:
nie stosujemy skrótów, ani w identyfikatorach,
ani w docstringach i komentarzach, chyba że są one
zrozumiałe dla każdego pięciolatka
([punkt 3.16](https://google.github.io/styleguide/pyguide.html#316-naming)).
Przykład zrozumiałego i powszechnie używanego
skrótu to `num_` zamiast `number_of_`.
Wniosek drugi: tym bardziej nie stosujemy jednoliterowych
identyfikatorów
([punkt 3.16.1](https://google.github.io/styleguide/pyguide.html#s3.16-naming)).
Pierwszy wyjątek to `i`, `j`, `k` jako liczniki pętli,
czyli zmienne całkowite generowane przez funkcję `range()`.
Drugi wyjątek to litery pasujące do typu danych
w funkcjach anonimowych i wyrażeniach listowych, słownikowych, zbiorowych i generatorowych,
czyli po naszemu *lambda expressions*,
*list/dict/set comprehensions* i *generator expressions*.
Na przykład litera `c` pasuje do pojedynczych znaków (`character`),
litera `s` do napisów (`string`) itp.
Jeśli trudno wyczuć, jaka litera pasuje do typu danych,
można użyć litery `x`, np. `squares = [x**2 for x in collection]`

* Poniżej podaję wzorce i antywzorce instrukcji warunkowych
([punkt 2.14.4](https://google.github.io/styleguide/pyguide.html#2144-decision)
i [2.8.4](https://google.github.io/styleguide/pyguide.html#284-decision)).
```python
    # LEPIEJ                                       # GORZEJ
    #### Istnieje jeden egzemplarz stałej |None|.  ####
    if spam is None:                               if spam == None:
        frobnicate()                                   frobnicate()
    ####                                           ####
    if spam is not None:                           if spam != None:
        frobnicate()                                   frobnicate()
    ####                                           ####
    if spam:                                       if spam is True:
        frobnicate()                                   frobnicate()
    ####                                           ####
    if not spam:                                   if spam is False:
        frobnicate()                                   frobnicate()
    #### Listy i krotki.                           ####
    if spam_sequence:                              if len(spam_sequence) > 0:
        frobnicate()                                   frobnicate()
    ####                                           ####
    if spam_string:                                if spam_string != '':
        frobnicate()                                   frobnicate()
    ####                                           ####
    if needle in haystack_dict:                    if needle in haystack_dict.keys():
        frobnicate()                                   frobnicate()
    ####                                           ####
    if spam == ham == eggs:                        if spam == eggs and ham == eggs:
        frobnicate()                                   frobnicate()
    ####                                           ####
    if 0 <= spam < 10:                             if spam >= 0 and spam < 10:
        frobnicate()                                   frobnicate()
    #### Poniższy warunek działa też bez nawiasów. ####
    if not (0 <= spam < 10):                       if spam < 0 or spam >= 10:
        frobnicate()                                   frobnicate()
```

* Sklejanie napisów przez `+` jest nieładne.
Ładne są za to f-stringi
([punkt 3.10](https://google.github.io/styleguide/pyguide.html#310-strings)).

* Dodajemy docstringi do modułów, klas, metod
i funkcji, chyba że są to metody lub funkcje jednowierszowe
lub w inny sposób oczywiste.
Proszę się zapoznać z zasadami pisania docstringów
([punkt 3.8](https://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings))
i ich przestrzegać. W skrócie: w pierwszym wierszu
docstringa wpisujemy zwięzłe zdanie
opisujące działanie metody lub funkcji, np.
`"""Zwraca indeks naciśniętego przycisku."""`
(na końcu zdania stawiamy kropkę).
Jeśli potrzebne jest dłuższe objaśnienie,
dodajemy je wewnątrz tego samego napisu,
oddzielone pustym wierszem.

* Wiersze programu nie powinny być zbyt długie.
Jeśli trzeba je połamać na krótsze kawałki,
dobrze wiedzieć, że kawałki otoczone dowolnym
rodzajem nawiasów (`()`, `[]`, `{}`) same się
ze sobą łączą i wyglądają lepiej niż ze znakiem obciachu
`\` na końcach
([punkt 3.2](https://google.github.io/styleguide/pyguide.html#32-line-length)).

### 3. Rozmaite rady

* Dobrym zwyczajem są przyrostki z jednostkami w nazwach wszystkiego,
co można mierzyć na różne sposoby:
`FRAME_INTERVAL_SECONDS`, `document_age_days`,
`page_width_mm`, `calculate_vat_pln()`.
Ku przestrodze: artykuł [Mars Climate Orbiter](https://pl.wikipedia.org/wiki/Mars_Climate_Orbiter#Utrata_sondy) w Wikipedii.

* Zamiast `r'spam\ham.png'` lub `'spam\\ham.png'`,
co działa tylko pod Windows, lepiej jest napisać
`'spam/ham.png'`, co działa pod każdym systemem
operacyjnym, w tym pod Windows.

* Zamiast sklejać dłuższe ścieżki do plików przez `+`,
lepiej używać
[`os.path.join()`](https://docs.python.org/3/library/os.path.html#os.path.join),
bo wtedy nie trzeba pamiętać, które kawałki ścieżki
kończą się na `'/'`, a które nie.

* Wewnątrz `sqlite3.Cursor.execute()` itp.
zmienne parametry wstawiamy do SQL-a tylko przez
[`?`, `?42`, `:spam`, `$spam` lub `@spam`](https://docs.python.org/3/library/sqlite3.html#sqlite3.Cursor.execute),
a nigdy przez `+`, f-stringi ani `.format()`.
[O tej zasadzie w komiksie XKCD nr 327](https://xkcd.com/327/).
Ku pamięci: w sposobach z pytajnikiem parametry
po stronie Pythona muszą być w krotce lub liście,
więc kiedy parametr jest jeden, piszemy `(spam,)` lub `[spam]`.

* Z drugiej strony przy zapytaniach z dynamicznymi nazwami kolumn
lub z `IN (?, ?,...)` o zmiennej liczbie pytajników
nie da się obejść bez f-stringów lub `.format()`.

* Polecam poniższe szablony czytania z bazy danych.
O metodzie `.fetchall()` lepiej zapomnieć.
```python
    # O ile kolumn prosi SELECT, tyle elementów mają
    # krotki generowane przez |cursor.execute()|.
    # 1-elementowe krotki trzeba rozpakowywać swoiście.
    spam_list = []
    for (spam,) in cursor.execute(  # Działa też spam, lub [spam].
            """
            SELECT spam
            FROM SpamTable
            WHERE ham = ?
            AND eggs = ?
            """,
            (ham, eggs)
    ):
        spam_list.append(spam)

    # Przy rozpakowywaniu dłuższych krotek nie ma niespodzianek.
    for spam, ham in cursor.execute(
            """
            SELECT spam, ham
            FROM SomeTable
            """
    ):
        frobnicate(spam, ham)
```

* Żeby się dowiedzieć, jakie kolumny zwraca `SELECT *`,
trzeba znaleźć kod tworzący tabelę.
Dlatego w programach należy jawnie wymieniać kolumny po `SELECT`.

* Python to nie C. Poniżej wzorce i antywzorce pętli.
```python
    # PYTONICZNIE                                  # NIEPYTONICZNIE
    ####                                           ####
    for spam in spam_list:                         for i in range(len(spam_list)):
        frobnicate(spam)                               frobnicate(spam_list[i])
    ####                                           ####
    for i, spam in enumerate(spam_list):           i = 0
        frobnicate(i, spam)                        for spam in spam_list:
                                                       frobnicate(i, spam)
                                                       i += 1
    ####                                           ####
    for spam, ham in zip(spam_list, ham_list):     for i in range(len(spam_list)):
        frobnicate(spam, ham)                          frobnicate(spam_list[i], ham_list[i])
    ####                                           ####
    COINS = [                                      COINS = [
        ('1 grosz', 0.01),                             ('1 grosz', 0.01),
        ('2 grosze', 0.02),                            ('2 grosze', 0.02),
        ...,                                           ...,
    ]                                              ]

    for name, value_pln in COINS:                  for coin in COINS:
        frobnicate(name, value_pln)                    frobnicate(coin[0], coin[1])
    ####                                           ####
    ham = '\n'.join(spam_list)                     ham = ''
                                                   for i, spam in enumerate(spam_list):
                                                       if i:
                                                           ham += '\n'
                                                       ham += spam
    ####                                           ####
    ham = '\n'.join(                               ham = ''
        frobnicate(x) for x in spam_list)          for i, spam in enumerate(spam_list):
                                                       if i:
                                                           ham += '\n'
                                                       ham += frobnicate(spam)
```

* Python to nie Java, odsłona pierwsza.
Nie tworzymy osobnych plików
na małe klasy w stylu `PrzechowywaczMonet`
tylko po to, żeby później pisać
`import PrzechowywaczMonet` i
`przechowywacz_monet = PrzechowywaczMonet.PrzechowywaczMonet()`

* Python to nie Java, odsłona druga. Zamiast pobieraczy
(*getters*) i ustawiaczy (*setters*) robiących tylko
`return self._spam` i `self._spam = spam` wystarczy
nazwać ten atrybut `self.spam` i bezpośrednio go
odczytywać i zapisywać. Uwaga: wewnątrz metod danego
obiektu możemy robić z jego atrybutami, co się nam żywnie
podoba; jest też w porządku, gdy kod poza obiektem
bezpośrednio odczytuje jego atrybuty; natomiast
bezpośrednie gmeranie z zewnątrz przy wartościach atrybutów
jest w złym guście — tylko w tym wypadku warto stosować
ustawiacze.

* Jeśli potrzebny jest pobieracz, który robi coś więcej,
pomoże nam dekorator
[`@property`](https://docs.python.org/3/library/functions.html#property):
```python
    @property
    def total_spam(self):
        """Używać bez nawiasów: ham = self.total_spam"""
        return sum(self._spam_list)
```

* Proszę odróżniać atrybuty klasy, które inicjalizujemy tak:
```python
    class Spam:
        ham = 42
```
od atrybutów instancji, które inicjalizujemy
w konstruktorze:
```python
    class Spam:
        def __init__(self):
            self.ham = 42
```
Atrybuty klas mają wartość początkową nadawaną tylko raz
i są wspólne dla wszystkich instancji klasy
(możemy się do nich odwoływać przez `Spam.ham` lub
`self.ham`). Atrybuty instancji są osobne w każdej instancji
(`self.ham`). Jeśli atrybut klasy jest stałą,
to wszystko jest w porządku. Natomiast jeśli atrybut
klasy zmienia wartość w trakcie działania programu,
to prosimy się o kłopoty. Nawet gdy przewidujemy
istnienie podczas działania programu tylko jednej
instancji klasy (np. `Game`), to przy testowaniu
zupełnie normalne jest tworzenie jedna po drugiej
coraz nowszych instancji. Jeśli te instancje
będą zmieniać wartość współdzielonego atrybutu,
to może dojść do niepożądanych skutków.
Osoby ciekawe nowości zachęcam do zapoznania się
z dekoratorem
[`@dataclasses.dataclass`](https://docs.python.org/3/library/dataclasses.html#dataclasses.dataclass),
który jest powiązany z tym zagadnieniem.

* Statyczne nazwy dobre, dynamiczne nazwy złe.
Dlatego zmienne w stylu
```python
    point = {'x': 42, 'y': 56}
```
wyglądają lepiej jako
[`collections.namedtuple`](https://docs.python.org/3/library/collections.html#collections.namedtuple).

* Podobnie stałe w rodzaju
```python
    COLORS = {
        'yellow': (181, 137, 0),
        'orange': (203, 75, 22),
        ...,
    }
```
zyskują po zmianie na
```python
    class Colors:
        """Paleta barw."""
        # pylint: disable=too-few-public-methods
        YELLOW = (181, 137, 0)
        ORANGE = (203, 75, 22)
        ...
```

* Importowanie modułu nigdy nie powinno mieć skutków ubocznych
— takich jak wypisywanie tekstu, otwieranie okien,
wczytywanie (zapisywanie?!) plików, wystrzeliwanie
pocisków balistycznych itp. Są to zawsze skutki
kodu lewitującego poza funkcjami. Taki kod wkładamy
do funkcji, a te wywołujemy z funkcji `main()`

* Na wczytywane z dysku obrazki, fonty, dźwięki itp.
proponuję założyć plik `assets.py` o poniższej treści.
W funkcji `main()` należy wywołać funkcję `pygame.init()`,
a następnie metodę `assets.Assets.load()`
    ```python
    """Zasoby potrzebne do gry."""

    import pygame


    class Assets:
        """Przechowuje zasoby."""
        # pylint: disable=too-few-public-methods

        @staticmethod
        def load():
            """Wczytuje zasoby z dysku."""
            Assets.SPAM_IMAGE = pygame.image.load('assets/spam.png')
            ...
            Assets.LARGE_FONT = pygame.font.Font('assets/Delicious-Roman.otf', 48)
            ...
    ```
Podobnie można zgrupować wczytywanie obrazków przez
`tkinter.PhotoImage`

* Koniec głównego modułu programu powinien wyglądać
jak poniżej. Oczywiście nie wszystkie sekcje funkcji `main()`
muszą wystąpić w każdym programie.

    ```python
    def main():
        [inicjalizacja bibliotek zewnętrznych]
        [tworzenie obiektów odpowiednich klas]
        [wywołanie funkcji lub metody wprawiającej te obiekty w ruch]
        [sprzątanie]


    if __name__ == '__main__':
        main()
    ```

* W plikach z testami wystarczy jeden docstring na początku
(pozostałe docstringi podpadają pod zasadę „lub w inny sposób
oczywistych” powyżej) i nie trzeba zamieniać magicznych liczb itp.
na zdefiniowane stałe. Szablon pliku `numbers_test.py`
do testowania zawartości fikcyjnego pliku `numbers.py`
zamieszczono poniżej. Pełną dokumentację modułu `unittest`
można znaleźć [tutaj](https://docs.python.org/3/library/unittest.html).

    ```python
    """Testy modułu numbers."""

    import unittest
    import numbers


    class ReaderTest(unittest.TestCase):

        def setUp(self):
            self.reader = numbers.Reader()

        def test_read_0(self):
            self.assertEqual(self.reader.read(0), 'zero')

        def test_read_7(self):
            self.assertEqual(self.reader.read(7), 'siedem')

        def test_read_20(self):
            self.assertEqual(self.reader.read(20), 'dwadzieścia')

        def test_read_42(self):
            self.assertEqual(self.reader.read(42), 'czterdzieści dwa')

        ...

        def test_read_negative(self):
            self.assertEqual(self.reader.read(-14), 'minus czternaście')

        def test_read_raises_on_invalid_input(self):
            with self.assertRaises(numbers.InvalidNumberError):
                self.reader.read('spam')


    class WriterTest(unittest.TestCase):

        ...


    if __name__ == '__main__':
        unittest.main()
    ```

* Widok kolorów w stylu `(255, 0, 0)` przypomina mi czasy
16-kolorowych trybów graficznych. Wśród 16.777.216 barw możemy
znaleźć ciekawsze. Ja lubię paletę
[Solarized](https://ethanschoonover.com/solarized/);
jest też mnogość innych palet.

### 4. Rady wyższego poziomu

* [Kopiuj-wklejoza](https://en.wikipedia.org/wiki/Copy-and-paste_programming)
(w najostrzejszych przypadkach mająca miejsce
między różnymi plikami) nie przystoi prawdziwym programistom.
Po to są moduły, dziedziczenie, metody, funkcje, pętle itp.,
żeby z nich korzystać. W łagodniejszych przypadkach,
kiedy po wklejeniu trzeba coś pozmieniać, można stosować zasadę
[„do trzech razy sztuka”](https://en.wikipedia.org/wiki/Rule_of_three_(computer_programming)).
* Lepszy jest program, który ma za dużo klas,
niż program, który ma ich za mało. Oto przykład:

    ```python
    # LEPIEJ, mimo że kod jest dłuższy:            # GORZEJ, chociaż krócej:
    # osobne klasy robią osobne rzeczy             # pomieszanie z poplątaniem
    class DictList:                                class Frobnicator:
        def __init__(self):                            def __init__(self):
            self.n2x = []                                  self.n2x = []
            self.x2n = {}                                  self.x2n = {}
                                                           [inicjalizacja innych pól]
        def add(self, x):
            if x not in self.x2n:                      def frobnicate(self, x):
                self.x2n[x] = len(self.n2x)                if x not in self.x2n:
                self.n2x.append(x)                             self.x2n[x] = len(self.n2x)
            return self.x2n[x]                                 self.n2x.append(x)
                                                           n = self.x2n[x]
                                                           [właściwa treść tej metody]
    class Frobnicator:
        def __init__(self):
            self.dictlist = DictList()
            [inicjalizacja innych pól]

        def frobnicate(self, x):
            n = self.dictlist.add(x)
            [dalsza treść tej metody]
    ```

* Łatwiej jest zrozumieć sprawdzanie wyrażeń logicznych,
gdy się unika negacji: zarówno operatora `not`,
jak wartości o zanegowanym sensie. Na przykład zamiast
pisać `if not is_invalid_email(field):` można przerobić program
i napisać `if x.is_valid_email(field):`
* Dziedziczenie niepotrzebnie komplikuje programy.
Łatwiejsze i ogólniejsze jest
[składanie obiektów](https://en.wikipedia.org/wiki/Composition_over_inheritance).
* Dobre konstruktory poznajemy po tym, że tylko
łączą w całość przekazane mu inne, wcześniej
skonstruowane obiekty. Dzięki takiemu
[„wstrzykiwaniu zależności”](https://pl.wikipedia.org/wiki/Wstrzykiwanie_zale%C5%BCno%C5%9Bci)
(*dependency injection*) znacznie łatwiej
jest testować klasy.
* Złe konstruktory poznajemy po:
    * wywołaniach innych konstruktorów
      (`super().__init__()` jest OK);
    * wywołaniach funkcji, które zmieniają globalny
      stan programu;
    * instrukcjach warunkowych lub pętlach;
    * inicjalizacji atrybutów bardziej skomplikowanej
      niż zwykłe przypisania;
    * tworzeniu obiektu, który potem trzeba jeszcze
      doinicjować inną metodą;
    * dziwnych obejściach, nie korzystających z metody
      `__init__()` po to, żeby móc zwracać kod błędu.
* Złe metody poznajemy po:
    * naruszaniu
      [reguły Demeter](https://pl.wikipedia.org/wiki/Prawo_Demeter),
      czyli przechodzeniu przez więcej niż jeden obiekt,
      np. `self.pies.ogon.merdaj()` zamiast poprawnego
      `self.pies.merdaj()`, przy czym zmyłka polega na tym,
      że błąd daje o sobie znać tutaj, a leży w klasie `Pies`
      (gwoli jasności: w regule Demeter nie chodzi o liczenie kropek,
      tylko o nierozmawianie z obiektami oddalonymi od `self`;
      takie odwołania jak `constants.SpamEnum.HAM` są koszerne).
* Złe klasy poznajemy po:
    * opisie zawierającym spójnik „i”;
    * rozłącznych zbiorach metod, które działają
      na rozłącznych zbiorach atrybutów;
    * atrybutach zmienianych z zewnątrz obiektu
      przez bezpośrednie przypisania do obiektu
      zamiast naturalnego korzystania z jego metod.

### Miłego programowania!

*Ekspert to osoba, która popełniła wszystkie błędy,
które można popełnić wewnątrz ograniczonej dziedziny*
— Niels Bohr
