# Jak zaliczyć projekt z Pythona
## Poradnik dla studentów przedmiotu Języki Symboliczne

Zamiast powtarzać podobne komentarze w wielu projektach,
spisałem je tutaj. Uwagi do tego dokumentu można zgłaszać
przez *Issues*.

### 1. Pliki

* Repozytorium nie powinno zawierać zbędnych katalogów
`__pycache__`, `.idea`, `venv` itp. ani zbędnych plików,
np. `desktop.ini`.

* Wskazane jest za to dodanie pliku `.gitignore`
o treści odpowiedniej dla projektów pisanych w Pythonie.
Kto nie dodał tego pliku przy tworzeniu repozytorium,
[tutaj](https://github.com/github/gitignore/blob/master/Python.gitignore)
znajdzie odpowiednią treść.

* Nazwy bibliotek zewnętrznych niezbędnych do działania
programu należy umieścić w pliku `requirements.txt`.
Dzięki temu przez polecenie `pip install -r requirements.txt`
można je wszystkie naraz zainstalować.
[Tutaj](https://pip.pypa.io/en/stable/reference/pip_install/#example-requirements-file)
znajduje się przykładowa treść tego pliku. Sądzę, że
w większości przypadków wystarczy wymienienie nazw bibliotek
bez uściślania ich wersji, np.

    ```
    numpy
    pygame
    ```

### 2. Uwagi drobiazgowe

Proszę się zapoznać z treścią
[poradnika dla programistów Pythona w firmie Google](https://google.github.io/styleguide/pyguide.html)
i stosować do jego zasad. Nie wymagam tylko podawania
w docstringach funkcji i metod sekcji `Args:`, `Returns:`
i `Raises:` ani w docstringach klas sekcji `Attributes:`
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

* Proszę używać instrukcji `import` wyłącznie
z nazwami pakietów i modułów, a nie poszczególnych
klas czy funkcji, a już broń Boże nigdy nie pisać
`from spam import *`. Dzięki temu unikną Państwo
zaśmiecania przestrzeni nazw nie wiadomo czym
([punkt 2.2](https://google.github.io/styleguide/pyguide.html#22-imports)).

* Można rozdzielać pustymi wierszami sąsiednie sekcje
złożone z instrukcji `import`, które powinnny dotyczyć
kolejno: biblioteki standardowej, bibliotek zewnętrznych
i naszych własnych modułów. Wewnątrz każdej sekcji nazwy
pakietów i modułów należy uporządkować leksykograficznie
([punkt 3.13](https://google.github.io/styleguide/pyguide.html#313-imports-formatting)).

* Proszę się wystrzegać zmiennych globalnych
([punkt 2.5](https://google.github.io/styleguide/pyguide.html#25-global-variables)).
Często da się ich pozbyć przez zmianę architektury
kodu z proceduralnej na obiektową tak, żeby stały
się atrybutami instancji klasy.

* Globalne stałe na poziomie modułów są natomiast
mile widziane, a nawet wymagane zamiast magicznych
liczb, napisów czy większych obiektów. Na przykład
zamiast `7` należy zdefiniować stałą `BOARD_WIDTH = 7`,
zamiast odróżniania kierunku przy użyciu zmiennej
o wartości `'left'` albo `'right'` — zdefiniować
stałe `LEFT, RIGHT = range(2)` (miłośnikom nowości
polecam
[`enum.Enum`](https://docs.python.org/3/library/enum.html#creating-an-enum)),
zamiast krotki `(0, 43, 54)` — stałą
`BACKGROUND_COLOR = (0, 43, 54)`. Stałe zwyczajowo
zapisujemy `WIELKIMI_LITERAMI_Z_PODKREŚLNIKAMI`
([punkt 3.16.4](https://google.github.io/styleguide/pyguide.html#3164-guidelines-derived-from-guidos-recommendations)).
Nie od rzeczy będzie przypomnieć, że XXI wiek
trwa już dość długo i w identyfikatorach można swobodnie
korzystać z polskich liter, jeśli ktoś ma taką ochotę.

* Nazwy funkcji i zmiennych w `styluWielbłądzim`
(*`camelCase`*) są niepytoniczne. Powinny być w
`stylu_wężowym` (*`snake_case`*)
([punkt 3.16.4](https://google.github.io/styleguide/pyguide.html#3164-guidelines-derived-from-guidos-recommendations)).

* Warto pamiętać, że czytelnik programu nie musi
znać tych samych skrótów, co my. Wniosek 1: żadne
skróty nie są dozwolone, zarówno w identyfikatorach,
jak w docstringach i komentarzach, chyba że będą
zrozumiałe dla każdego średnio rozgarniętego pięciolatka
([punkt 3.16](https://google.github.io/styleguide/pyguide.html#316-naming)).
Wniosek 2: tym bardziej niedozwolone są jednoliterowe
nazwy zmiennych
([punkt 3.16.1](https://google.github.io/styleguide/pyguide.html#s3.16-naming)).
Wyjątki to `i`, `j`, `k` jako liczniki pętli
oraz litery pasujące do typu elementów w wyrażeniach
listowych, słownikowych, zbiorowych i generatorowych,
czyli po naszemu *list/dict/set comprehensions*
i *generator expressions* (często cholera wie,
jaka litera pasuje, ale jeszcze nikomu
nie stała się krzywda za użycie `x`).

* Wymagane są docstringi do modułów, klas, metod
i funkcji, chyba że są jednowierszowe lub w inny
sposób oczywiste
([punkt 3.8](https://google.github.io/styleguide/pyguide.html#3164-guidelines-derived-from-guidos-recommendations)).
Należy się zapoznać z zasadami pisania docstringów
i ich przestrzegać. W skrócie: pierwszy wiersz ma zawierać
pełne a krótkie zdanie (na końcu zdania stawiamy kropkę)
opisujące działanie metody lub funkcji, np.
`"""Zwraca przycisk naciśnięty przez użytkownika."""`
Jeśli potrzebne jest dłuższe objaśnienie, należy
je podać poniżej, po pustym wierszu.

* Sklejanie napisów przez `+` jest nieładne.
Ładne są za to f-stringi
([punkt 3.10](https://google.github.io/styleguide/pyguide.html#310-strings)).

* Wiersze programu nie powinny być za długie.
Jeśli trzeba je połamać na krótsze kawałki,
dobrze wiedzieć, że kawałki otoczone dowolnym
rodzajem nawiasów (`()`, `[]`, `{}`) same się
ze sobą łączą i wyglądają lepiej niż ze znakiem obciachu
`\` na końcach
([punkt 3.2](https://google.github.io/styleguide/pyguide.html#32-line-length)).

### 3. Inne uwagi

* Dobrym zwyczajem są przyrostki — jednostki miary —
w nazwach stałych i zmiennych, które wyrażają wielkości fizyczne:
`FRAME_INTERVAL_SECONDS`, `document_age_days`, `page_width_mm`.

* Zamiast `r'spam\ham.png'`, co działa tylko pod Windows,
lepiej pisać `'spam/ham.png'`, co działa również
pod Windows.

* Dłuższe ścieżki do plików dobrze skleja
[`os.path.join()`](https://docs.python.org/3/library/os.path.html#os.path.join).
Nie trzeba pamiętać, które kawałki ścieżki się kończą na `'/'`,
a które nie.

* Chociaż pomysłowość studentów w domorosłych
sposobach sklejania napisów nie zna granic,
wewnątrz `sqlite3.Cursor.execute()` itp.
zmienne parametry wolno wstawiać do SQL-a tylko przez
[`?`, `?42`, `:spam`, `$spam` lub `@spam`](https://docs.python.org/3/library/sqlite3.html#sqlite3.Cursor.execute).
Wyjaśnienie
[tutaj](https://xkcd.com/327/).
Ku pamięci: w sposobach z pytajnikiem parametry
po stronie Pythona muszą być w krotce lub liście.
Jak jest jeden, pisać `(spam,)` lub `[spam]`.

* Z drugiej strony przy zapytaniach z dynamicznymi nazwami kolumn
lub z `IN (?, ?,...)` o zmiennej liczbie pytajników
nie da się obejść bez f-stringów lub `.format()`.

* Python to nie Java. Zbyteczne jest tworzenie
osobnych plików na małe klasy w stylu
`PrzechowywaczMonet` tylko po to, żeby później
było trzeba pisać `import PrzechowywaczMonet` i
`przechowywacz_monet = PrzechowywaczMonet.PrzechowywaczMonet()`.

* Należy odróżniać atrybuty klasy, które są
inicjalizowane tak:
```python
    class Spam:
        ham = 42
```
od atrybutów instancji, które są inicjalizowane
w konstruktorze:
```python
    class Spam:
        def __init__(self):
            self.ham = 42
```
Te pierwsze mają wartość początkową nadawaną tylko raz
i są wspólne dla wszystkich instancji klasy
(można się do nich odwoływać przez `Spam.ham` lub
`self.ham`), a te drugie są osobne w każdej instancji
(`self.ham`). Jeśli atrybut klasy jest stałą,
to wszystko jest w porządku. Natomiast jeśli atrybut
klasy zmienia wartość w czasie działania programu,
to prosimy się o kłopoty. Nawet gdy przewidujemy
istnienie podczas działania programu tylko jednej
instancji klasy (np. `Game`), to przy testowaniu
zupełnie normalne jest tworzenie jedna po drugiej
coraz nowszych instancji, a każda będzie bazgrać
po jedynym egzemplarzu atrybutu. Dlatego stosowanie
zmiennych atrybutów klas jest prawie zawsze błędem.
Poniekąd powiązany z tym zagadnieniem dekorator
[`@dataclasses.dataclass`](https://docs.python.org/3/library/dataclasses.html#dataclasses.dataclass)
jest nowy i nadobowiązkowy.

* Statyczne typy dobre, dynamiczne typy złe.
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
[`enum.Enum`](https://docs.python.org/3/library/enum.html#creating-an-enum).

* Koniec głównego modułu programu powinien się
przedstawiać jak poniżej. Oczywiście nie wszystkie
sekcje funkcji `main()` muszą wystąpić w każdym
programie.

    ```python
    def main():
        ... [inicjalizacja bibliotek zewnętrznych]
        ... [tworzenie obiektów odpowiednich klas]
        ... [wywołanie funkcji lub metody wprawiającej te obiekty w ruch]
        ... [sprzątanie]


    if __name__ == '__main__':
        main()
    ```

* Dokładne testy do wszystkich klas i funkcji programu
nie są wymagane na zaliczenie, ale trochę testów — tak.
Chodzi o to, żeby Państwo przećwiczyli ich pisanie.
W plikach z testami wystarczy jeden docstring na początku
i nie trzeba zamieniać magicznych liczb itp. na
zdefiniowane stałe. Ogólny wzór pliku `spam_test.py`
do testowania zawartości pliku `spam.py` pokazano
poniżej. Pełna dokumentacja modułu `unittest` znajduje się
[tutaj](https://docs.python.org/3/library/unittest.html).

    ```python
    """Testy modułu spam."""

    import unittest

    import spam


    class HamTest(unittest.TestCase):

        def setUp(self):
            self.ham = spam.Ham(price=42)

        def test_open(self):
            self.assertTrue(self.ham.open())

        def test_default_price(self):
            self.assertEqual(self.ham.price, 42)

        def test_set_price(self):
            self.ham.set_price(39)
            self.assertEqual(self.ham.price, 39)


    class EggsTest(unittest.TestCase):

        ...


    if __name__ == '__main__':
        unittest.main()
    ```

* Widok kolorów w stylu `(255, 0, 0)` przypomina mi czasy
butelek z mlekiem za progiem mieszkania, magnetofonów kasetowych
i 16-kolorowych trybów graficznych. Z 16.777.216 barw da się
wybrać ciekawsze. Ja lubię paletę
[Solarized](https://ethanschoonover.com/solarized/);
jest też mnogość innych.

### 4. Rady wyższego poziomu

* Dobry konstruktor poznaje się po tym, że tylko
łączy w całość przekazane mu inne, wcześniej
skonstruowane obiekty. Dzięki takiemu „wstrzykiwaniu
zależności” (*dependency injection*) znacznie łatwiej
jest testować daną klasę.
* Zły konstruktor poznaje się po:
    * wywołaniach innych konstruktorów;
    * wywołaniach funkcji, które zmieniają globalny
      stan programu;
    * instrukcjach warunkowych lub pętlach;
    * inicjalizacji atrybutów bardziej skomplikowanej
      niż zwykłe przypisania;
    * tworzeniu obiektu, który potem trzeba jeszcze
      doinicjować inną metodą;
    * dziwnych obejściach, nie korzystających z metody
      `__init__()` po to, żeby móc zwracać kod błędu.
* Złe metody poznaje się po:
    * korzystaniu ze swoich argumentów nie wprost,
      tylko po to, żeby się dostać do innych obiektów;
    * naruszaniu
      [prawa Demeter](https://pl.wikipedia.org/wiki/Prawo_Demeter),
      czyli przechodzeniu przez więcej niż jeden obiekt,
      np. `self.pies.ogon.merdaj()` zamiast poprawnego
      `self.pies.merdaj()`, przy czym zmyłka polega na tym,
      że błąd daje o sobie znać tutaj,
      a leży w niedorobionej klasie `Pies`.
* Złe klasy poznaje się po:
    * opisie zawierającym wyraz „i”;
    * rozłącznych zbiorach metod, które operują
      na rozłącznych zbiorach atrybutów;
    * atrybutach, których wartości są zmieniane
      przez bezpośrednie przypisania do obiektu
      zamiast korzystania po bożemu z jego metod.

### Miłego programowania!
