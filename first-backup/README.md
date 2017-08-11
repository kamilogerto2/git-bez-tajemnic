# Stwórzmy sobie backup

Na początek przygotujmy sobie nasze środowisko pracy - prostą strukturę katalogów i plików. Będzie to nasz punkt wyjścia - projekt nad którym będziemy pracować.

```
git_bez_tajemnic
├── catalog1
│   ├── sample2.txt
│   └── image.png
├── catalog2
└── sample.txt
```

![Struktura projektu](catalog.png?raw=true "Struktura projektu")

Utworzyliśmy katalog git_bez_tajemnic, a w nim plik tekstowy sample.txt oraz kolejne dwa podkatalogi - catalog1 i catalog2. Wewnątrz folderu catalog1 znajdują się dwa pliki - sample2.txt oraz image.png. Treść plików jest dowolna (ale różna dla plików) - jednak niech to nie będą puste pliki - jest tak dlatego, ponieważ może nam to przysporzyć potem trochę problemów (puste pliki będą interpretowane jako takie same) ;) 

Zaprezentowana struktura jest standardowym zapisem plików w postaci drzewa jakie znamy z naszych ulubionych systemów operacyjnych. Wyobraźmy sobie jednak sytuację w której to chcielibyśmy mieć poglądową listę wszystkich plików i katalogów  na jednym poziomie, bez konieczności wchodzenia do każdego z podfolderów, ale także, żebym możliwym było jej szybkie przejrzenie i znalezienie interesujących obiektów. Tworzymy więc małą “bazę danych” - będzie to swego rodzaju backup naszych plików. Załóżmy, że kluczem będzie nazwa pliku, a wartością - zawartość pliku. W naszym katalogu stwórzmy folder objects, w którym będziemy zapisywać naszą spłaszczoną listę obiektów. 

W przypadku pojedynczych plików sprawa będzie dość prosta - kluczem (a więc nazwą tworzonego pliku) będzie nazwa oryginalnego pliku, wartością jego zawartość. Co jednak w przypadku folderów? Tak samo jak w przypadku plików - będziemy podążać zgodnie z zasadami - kluczem będzie nazwa katalogu, a wartością lista nazw plików, które do niego należą (zawartość pliku jest wylistowana kawałek dalej). Tymczasowo foldery będą zapisane w postaci plików tekstowych.

Po wykonaniu przedstawionych operacji nasz folder objects wygląda następująco:

![Struktura projektu](catalog2.png?raw=true "Struktura projektu")

Pliki tekstowe oraz plik graficzny nie zmieniły swojej zawartości. W pliku tekstowym git_bez_tajemnic - reprezentującym nasz katalog główny - możemy znaleźć następującą zawartość:

```
catalog1
catalog2
sample.txt
```

Czyli w skrócie zawartość tego katalogu w naszym drzewie projektu. Dla porównania w pliku catalog1 znajdziemy następującą zawartość:

```
sample2.txt
image.png
```

Mając taką płaską strukturę obiektów reprezentowaną w postaci klucz-wartość - jesteśmy w stanie w prosty sposób odwzorować naszą strukturę projektu w sposób jaki go stworzyliśmy na początku - idąc plik po pliku i budując drzewo plików i katalogów (zakładamy, że na ten moment znamy plik od którego powinniśmy zacząć analizę - w naszym przypadku git_bez_tajemnic).
     
Przy tworzeniu takiej struktury plików może pojawić się problem duplikujących się plików - ot na przykład index.php, który pojawia się parę razy w katalogu wordpressa. Co wtedy? Standardowo system będzie chciał nadpisać zmiany - to nas jednak nie urządza. Wypadałoby wymyślić jakiś system kluczy. Użyjemy do tego funkcji hashującej SHA-1. Stwórzmy sobie szablon według którego będziemy tworzyć nasze klucze. Niech wygląda tak:

```
<type> <size>\0<content>
``` 

W naszym szablonie użyto trzech składowych - type, size, content. Type to nic innego jak typ obiektu, który tworzymy. Uznajmy, że wszelkie pliki - tekstowe, graficzne będą typu ‘blob’ (o tym czym jest blob będzie trochę później). Drugim typem obiektów będą nasze katalogi - nazwijmy je ‘tree’ czyli drzewami. Size to rozmiar obiektu, który zapisujemy, a content to zawartość naszego pliku.

Mając więc plik tekstowy o zawartości ‘text’ będzie to na przykład:
```
blob 4\000test
```

Na takim ciągu znaków wykonujemy operacje hashowania (można skorzystać na przykład z konwertera online, np. http://www.sha1-online.com/ ) SHA-1 i otrzymujemy czterdziesto znakowy klucz:
591ea0e6b95a5bfb864dab0487ab312d4beff999

Ponadto podczas konwersji plików, po uzyskaniu klucza, będziemy go zapisywali w pliku tekstowym katalogu obok nazwy pliku. Będzie to więc wyglądało następująco:

```
591ea0e6b95a5bfb864dab0487ab312d4beff999 catalog1
591ea0e6b95a5bfb864dab0487ab312d4beff999 catalog2
591ea0e6b95a5bfb864dab0487ab312d4beff999 sample.txt
```

W ten sam sposób będziemy postępowali dla wszystkich plików i katalogów. Nasz katalog objects wygląda teraz w następujący sposób:

![Struktura projektu](catalog3.png?raw=true "Struktura projektu")

Na chwilę obecną przedstawiliśmy nasz katalog w drugiej alternatywnej wersji listowej, gdzie kluczem jest odpowiedni ciąg znaków przekonwertowany poprzez algorytm SHA-1,  a wartością - zawartość pliku. Znając więc klucz SHA-1 głównego katalogu projektu - mimo zaszyfrowanego nazewnictwa plików jesteśmy w stanie krok po kroku odtworzyć strukturę katalogową naszego projektu.

Kolejną rzeczą, którą będziemy chcieli zrobić to optymalizacja plików. Na tym etapie wyjaśnimy sobie czym w zasadzie jest blob wspomniany wcześniej. Jest to nic innego jak binarna, zoptymalizowana wersja pliku. Dzięki takiemu zabiegowi możemy osiągnąć znacznie mniejsze rozmiary plików przy zachowaniu ich początkowej zawartości. Osiągniemy to przy użyciu kompresji zlib (link). Nie będziemy skupiać się na algorytmie kompresji biblioteki, bo na ten moment jest to niepotrzebne. 

Co uzyskaliśmy na tym etapie?

W naszym katalogu mamy zunifikowaną listę obiektów (blobów)  skompresowanych o nieokreślonym typie (pliki binarne). Ich kluczem jest wartość SHA-1 uzyskana na podstawie ustalonego szablonu przedstawionego powyżej, a zawartość to skompresowana zawartość pliku. Mamy dwa miejsca, gdzie przechowujemy nasze dane - pierwszy niezoptymalizowany zapis w postaci zwykłej struktury katalogowej i drugi zoptymalizowany w postaci spłaszczonej listy klucz-wartość. Z praktycznego punktu widzenia - stworzyliśmy całkiem przyzwoity backup naszych danych (naszego projektu). Całkiem dużo prawda? Skoro nasze drugie źródło danych jest tak bardzo zoptymalizowane - czemu by tego nie wykorzystać?  

