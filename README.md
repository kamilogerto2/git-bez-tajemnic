# Git bez tajemnic

Jakiś czas temu prowadząc szkolenie z systemu kontroli wersji GIT zdałem sobie sprawę, że jest wiele rzeczy, których sam muszę się jeszcze poduczyć i które nie są traktowane priorytetowo podczas nauki wspomnianego systemu. Postanowiłem wtedy poszerzyć swój zakres wiedzy o to, jak dokładnie ten system działa i z jakich rozwiązań korzysta. Dziś w przystępny sposób chciałbym pokazać to, co czasem może wyglądać strasznie (na przykład po otwarciu katalogu .git naszego projektu). Jeżeli więc jesteś osobą, która korzysta z GITa, a chciałaby go lepiej zrozumieć lub gdy po prostu chciałbyś się dowiedzieć czym w zasadzie on jest - ten artykuł jest właśnie dla ciebie.

Rozpoczynając pracę w IT jednym z najczęstszych wymagań odnośnie kandydata jest znajomość systemu kontroli wersji GIT. Tak więc - jedni potrafią go bardziej, drudzy mniej. Bardzo często owa “znajomość” GITa ogranicza się do tego, że:

* wiem, że GIT to system kontroli wersji
* umożliwia nam pracę w zespole, porównywanie wersji etc.
* potrafię wykonać parę podstawowych komend takich jak commit, push, pull, merge

Mówiąc szczerze - to bardzo często wystarcza. Jest jednak jedno małe “ale”. Głęboko wierzę, że wśród czytelników niniejszego artykułu są osoby, które nie tylko chcą wykorzystywać podstawowe funkcjonalności jakiegoś narzędzia, ale także chcą zrozumieć narzędzie z którym pracują. Postaram zapoznać Cię z tym jak działa system kontroli wersji GIT i w jaki sposób przechowuje pliki.  Chciałbym pokazać ci, że to co może wyglądać na skomplikowane - jest takim tylko z pozoru. Pod spodem kryją się dobrze znane nam mechanizmy. Po przeczytaniu tego artykułu może się okazać, że doskonale zrozumiałeś to, z czym pracowałeś przez dobrych parę lat. Spróbujemy na własną rękę “pobawić” się w gita, żeby zrozumieć co nam oferuje, mniej lub bardziej znany, system GIT.

## Spis treści
* [Stwórzmy sobie backup](first-backup/README.md)
* Cykliczny backup danych (czyli nasz pierwszy commit)
