![moner - logo](https://www.getmonero.org/img/monero-logo.png)
# White-Paper-Monero Biała księga Monero

## PRZEGLĄD BIAŁEJ KSIĘGI KRYPTONOTE
## SURAE NOETHER

### Niniejszy artykuł jest dedykowany Emmy Noether, Satoshi Nakamoto i Grupie Bourbaki.

Streszczenie. Ten dokument nie jest przeznaczony jako porada finansowa; jest to jedna z opinii matematyka na temat białej księgi przed zagłębieniem się w kod. Pisząc ten dokument, wiele się nauczyłem. Piszę tę recenzję częściowo po to, aby zwiększyć rygor naukowy białej księgi CryptoNote (CN), która już teraz wyprzedza o mile dostępne alternatywy, ale także aby skrytykować ten dokument za jego niespójności i niekompletność. Dla pełnego ujawnienia, zostałem zatrudniony przez twórców Monero (XMR) do zbadania protokołu CryptoNote oraz bazy kodu ByteCoin, z której rozwidlono Monero. Osoby zaangażowane w zatrudnienie mnie nie brały udziału w moim procesie recenzji poza odpowiadaniem na moje pytania techniczne i sporadycznym wysyłaniem mi pieniędzy, gdy grzecznie ich o to poproszę i pokażę im adnotacje, które zrobiłem. Płacą mi w Bitcoinach, a nie w jakiejkolwiek walucie opartej na CryptoNote. Tak, posiadam i przechowuję waluty CryptoNote.
Ponadto chciałbym wstępnie przeprosić Nicolasa van Saberhagena i twórców CryptoNote, jeśli nadepnąłem Wam na odcisk. Prawdopodobnie siedzicie w tym po kolana od lat. Jeśli powiedziałem coś rażąco niepoprawnego, idiotycznego, niedoinformowanego lub błędnego, z przyjemnością poprawię zapis i zostanę poinformowany. Jestem tu, aby się uczyć, a nie pouczać. Po prostu napisz do mnie e-mail, to porozmawiamy. Ale zostałem zatrudniony do napisania krytyki, jestem matematykiem, a niektóre z waszych indeksów były błędne. Nie bierz tego do siebie.

Data: 14 Lipca 2014.

###Wstęp
Decentralizacja i anonimowość są ważne w świecie finansów ze względu na nieuniknione konflikty interesów między władzą centralizacyjną a użytkownikami. Człowiek musi otrzymać zapłatę, a my potrzebujemy naszej prywatności. Nie jest to nic złego, tak po prostu jest i można to rozwiązać za pomocą technologii. Bitcoin był pierwszą zdecentralizowaną, peer-to-peer, pseudonimową próbą rozwiązania tego problemu. Zgodnie z wiedzą autora, zaproponowano niewiele innych naprawdę unikalnych rozwiązań. Ogólnie rzecz biorąc, protokół CryptoNote (CN) stanowi pierwszy nowy krok w dziedzinie kryptowalut od czasu Bitcoina i zasługuje na tyle samo miejsca, co Bitcoin. Bez wątpienia jest to mocne uderzenie, z kilkoma podstawowymi ulepszeniami w stosunku do protokołu Bitcoin i kilkoma dużymi ulepszeniami. Ten artykuł ma na celu przegląd białej księgi CN, wskazanie przynajmniej niektórych zalet i wad proponowanego protokołu oraz zilustrowanie punktów możliwych ulepszeń protokołu. Jak działa CN? Cóż, tak jak wszystko w świecie kryptografii, co działa dobrze, działa też dziwnie. Możemy sobie wyobrazić protokół CN jako system skrzynek pocztowych. Każdy użytkownik ma zestaw kluczy publicznych i prywatnych, tak jak w protokole BTC. Zamiast wysyłać CrytpoNote bezpośrednio do kluczy publicznych drugiej osoby, użytkownicy przeprowadzają wymianę Diffie-Hellmana i tworzą podpisy pierścieniowe, aby utworzyć nową, jednorazową skrytkę pocztową, w której przechowywane są CN. A kiedy wysyłamy nasze CN, dołączamy nasz obraz klucza, który jest po prostu skrótem prywatnego klucza docelowego, który dał nam prawo do wysłania tych CN w pierwszej kolejności. Jeśli ten obraz klucza nie został jeszcze użyty, to znaczy, że jednorazowy podpis pierścieniowy nie został jeszcze wydany. W pewnym sensie mówimy: "Dobra, widzisz to pudełko? Jest w nim 1.0 CN, a ja mam do niego klucz prywatny K". W ramach wymiany Diffiego-Hellmana ty tworzysz nowy klucz prywatny K∗, a ja tworzę nowy adres publiczny i wysyłam 1.0 CN na nowy adres oraz ogłaszam obraz klucza Im(K) użyty do wykonania tej transakcji. Każdy, kto zobaczy tę transakcję, sprawdzi, czy Im(K) był już wcześniej użyty, aby upewnić się, że w mojej skrzynce nadal znajduje się mój 1.0 CN, a teraz masz nowy klucz prywatny K∗ do swojej własnej skrzynki pocztowej pełnej gotówki!". Albo, tak jak ja lubię o tym myśleć, ktoś mówi: "Zabierzcie ręce od pieniędzy, kiedy są przekazywane! Wystarczy wiedzieć, że klucze mogą otworzyć tę skrzynkę... zaraz, zaraz, nie! Wystarczy wiedzieć, że hash Twoich kluczy jest równoważny położeniu tej skrzynki! I że wiemy, ile pieniędzy jest w tej skrzynce! Nigdy nie zostawi odcisków palców na skrzynce pocztowej ani jej nie używyje, po prostu wymień się hashami kluczy, które odpowiadają lokalizacji skrzynek wypełnionych gotówką! W ten sposób nie wiemy, kto co wysłał! Ale te transakcje są nadal pozbawione tarcia, wymienialne, podzielne i posiadają wszystkie inne cechy pieniądza, których pożądamy w protokole Bitcoina". Choć brzmi to dziwnie, tak właśnie działa CryptoNote. Zastanawiałem się nad umieszczeniem schematu blokowego tego wszystkiego na plakacie w pracy, żeby zobaczyć, co powiedzą inni. Po przeczytaniu całej białej księgi najbardziej niepokoi mnie to, czy parametry krzywej eliptycznej zostały dobrane uczciwie i jak uczciwy twórca monety mógłby je zmienić, gdyby chciał.
Zamierzam uporządkować ten dokument w następującej kolejności:
(I) przegląd literatury,
(II) ulepszenia, jakie oferuje CN w stosunku do BTC,
(III) możliwe problemy z protokołem CN,
(IV) niedociągnięcia w białej księdze,

1. Przegląd literatury
Dobra, będę szczery; jak większość naukowców, tylko pobieżnie przejrzałem przegląd literatury autorstwa van Saberhagena. Ale kurczę, van Saberhagen wybrał dla mnie kilka cholernie dobrych prac. Historia sygnatur pierścieniowych przedstawiona przez van Saberhagena, począwszy od sygnatur grupowych, jest całkiem interesująca. Podpisy grupowe zaczęły się jako pierwszy sposób pozwalający każdemu publicznemu członkowi grupy na anonimowe podpisanie wiadomości w imieniu tej grupy, przy czym istniał menedżer grupy, który mógł, według własnego uznania, odebrać użytkownikowi anonimowość. Nazwa "podpis grupowy" zirytowała algebraistów, którzy w zasadzie wymyślili kryptografię, dziękuję bardzo. Podpisy pierścieniowe zostały opracowane w celu usunięcia punktu centralizacji, jakim jest administrator grupy. Nazwa ta również zirytowała algebraistów. Zasadniczo, jeśli każdy ma klucz publiczny i parę kluczy prywatnych, wszystko, co robimy, to podpisujemy naszą wiadomość naszym kluczem prywatnym w zwykły sposób, a następnie publikujemy zestaw kluczy publicznych naszych przyjaciół wraz z naszym własnym kluczem publicznym i ustanawiamy jakiś protokół, dzięki któremu używamy całego zestawu kluczy publicznych do weryfikacji wiadomości. Proste! W ten sposób uzyskuje się niemożliwość śledzenia. Następnie pojawiły się podpisy pierścieniowe z możliwością łączenia, co jest mylące, ponieważ, używając terminologii stosowanej w tym dokumencie, w rzeczywistości nie są to podpisy niemożliwe do śledzenia, a przynajmniej skalowalnie identyfikowalne. Są to podpisy pierścieniowe, w których nadawca może zasadniczo cofnąć swoją anonimowość, jeśli tylko zechce. Kolejnym zagadnieniem jest identyfikowalny podpis pierścieniowy, który również jest mylący, ponieważ nie ma wiele wspólnego z identyfikowalnością w naszym rozumieniu. Sposób działania podpisów pierścieniowych polega na ustanowieniu jednorazowej metody użycia podpisu pierścieniowego, na przykład w systemach głosowania. Zdecydowanie przydatny w każdym systemie opartym na tokenach. Wreszcie, dochodzimy do podpisów grupowych ad hoc, w których po prostu wybieramy członków grupy w locie. Używając konstrukcji historycznych
- Podpisy grupowe
- Nieśledzalne podpisy pierścieniowe
- Skalowalnie identyfikowalne podpisy pierścieniowe
- Jednorazowe podpisy pierścieniowe o skalowalnej identyfikowalności
- Podpisy grupowe ad hoc
Van Saberhagen połączył to wszystko razem, aby uzyskać CryptoNote. Moim zdaniem to całkiem sprytne. Jednak w dalszej części artykułu nie będziemy w ogóle używać tej terminologii. Po prostu wszystko to pasuje do kontekstu "starej kryptowaluty". Co, jak się okazuje, jest bardzo ważne: jeśli spróbujesz nowej kryptowaluty, zwykle się sparzysz.

2. Ulepszenia w stosunku do Bitcoina
W CryptoNote można znaleźć wiele drobnych usprawnień w stosunku do protokołu Bitcoin. Twoje hasło jest równoważne kluczom prywatnym, a każdy użytkownik potrzebuje tylko jednego adresu. Nie ma kolizji transakcji. Możesz oddać połowę swoich kluczy prywatnych bez obawy o utratę bezpieczeństwa, na przykład firmie obsługującej płatności. Każdy użytkownik może wygenerować adres umożliwiający kontrolę dochodów, generując deterministycznie połowę swoich kluczy. Skrypty transakcyjne są bardzo proste. Globalne zmienne, takie jak rozmiar bloku i nagroda za blok, dynamicznie się dostosowują, więc nie musimy się martwić o konsensus sieciowy przy zmianach kodu przypominających infrastrukturę. Tak naprawdę większość najprzyjemniejszych korzyści to tylko kilka małych rzeczy.
2.1. Brak możliwości śledzenia i powiązania. Niektóre większe rzeczy sprawiają, że czujesz się po prostu dobrze, nawet jeśli nie jest to tak naprawdę cecha: dowód na to, że transakcje są bezwarunkowo niepowiązane (przy założeniu losowej wyroczni). Według van Saberhagena dwie transakcje są niepowiązane, jeśli nie możemy udowodnić, że trafiły do tej samej osoby, a system jest niepowiązany, jeśli dla dowolnych dwóch transakcji są one niepowiązane. Wyrocznia ran - dom zakłada istnienie pewnej doskonałej funkcji haszującej, co jest nieco nierealistyczne, ale nie jest dobrze przybliżone przez obecne funkcje haszujące. Jedynym lepszym dowodem byłby dowód zgodny z modelem standardowym, a prawie żadne modele kryptograficzne nie są udowadniane zgodnie z modelem standardowym. Żaden inny twórca nie udowodnił anonimowości swoich monet. Nie mogę tego wystarczająco podkreślić. Żadna inna moneta nie ma tak mocnego dowodu matematycznego, który stałby za jej produktem. Nawet Bitcoin dopiero niedawno poddał swoje metody rygorystycznej analizie bezpieczeństwa i wiadomo, że Bitcoin nie jest w stanie zapewnić braku powiązań i możliwości śledzenia! To absolutnie wyrzuca z wody każdą konkurencyjną monetę. Najbliższym konkurentem jest Zerocoin/Zerocash. Biała księga CryptoNote nie ma racji, krytykując przy okazji Zerocash, ponieważ w protokole Zerocash dokonał ostatnio przełomu w ograniczeniach rozmiaru; jednak cytuję Maurice'a Plancka, programistę CN: [Maurice P] Zerocoin, Zerocash. Muszę przyznać, że jest to najbardziej zaawansowana technologia. Tak, powyższy cytat [z białego pa - dop. red] pochodzi z analizy poprzedniej wersji protokołu. Według mojej wiedzy nie jest to 288, lecz 384 bajty, ale tak czy inaczej jest to dobra wiadomość [ostatnie zmniejszenie rozmiarów]. Zastosowano zupełnie nową technikę [sic] o nazwie SNARK, która ma pewne wady: na przykład dużą początkową bazę danych parametrów publicznych wymaganych do utworzenia podpisu (ponad 1 GB) i znaczny czas potrzebny do utworzenia transakcji (ponad minutę). Wreszcie, używa się młodej kryptowaluty, o której wspominałem, że jest to pomysł dyskusyjny https://forum.cryptonote.org/viewtopic.php?f=2&t=19#p55.
Zauważmy, że ponieważ nadal przekazujemy informacje za pomocą funkcji (naszej funkcji wyroczni losowej), a nie używamy systemu zerowej wiedzy, nasz system nadal nie jest w pełni anonimowy. Andrew Poelstra opisał wspaniały stan wiedzy na temat anonimowych monet na stronie CryptoStackExchange https://bitcoin.stackexchange.com/questions/29471/are-there-any-true-anonymous-cryptocurrencies#18096?newreg=b484b83920af4fa79cd9b7ac25961abd, w tym dogłębną dyskusję na temat CryptoNote i kilka proponowanych poprawek. Transakcji nie da się również śledzić w sposób skalowalny. Według van Saberhagena transakcja jest niewykrywalna, jeśli wszyscy nadawcy są ekwiprobabilistyczni, co oznacza, że nadawca wybiera zestaw kluczy publicznych do podpisu pierścieniowego, a z punktu widzenia atakującego wszyscy członkowie tego zestawu są ekwiprobabilistyczni jako potencjalni nadawcy. Ponadto funkcja ta jest skalowalna w tym sensie, że można wybrać rozmiar zestawu maskującego (stopień niejasności to liczba kluczy publicznych w zestawie maskującym), a nawet wybrać stopień niejasności n = 0, jeśli ktoś sobie tego życzy. Van Saberhagen wymienia dwie pożądane właściwości kryptowaluty: niemożność śledzenia i niemożność powiązania, a my wspomnieliśmy już o obu. Załóżmy, że podsłuchuję sieć kryptowalutową i porównuję dwie transakcje. Możemy sobie wyobrazić transakcję jako strzałę od jednego użytkownika do drugiego, której długość stanowi kwota. Tak więc mamy dwie strzałki o różnej długości. Problem w tym, że nie wiemy, kto wysyła lub odbiera, prawda? Przynajmniej w idealnym przypadku nie wiemy. Więc po prostu wrzucimy tu losowe nazwiska:
A = Alice --1.101--> Bob = B
C = Charlie --0.0637--> Danielle = D
Van Saberhagen definiuje system jako niemożliwy do prześledzenia, jeśli dla każdej transakcji przychodzącej wszyscy nadawcy są ekwiprobabilistyczni, a niemożliwy do powiązania, jeśli dla dowolnych dwóch transakcji wychodzących nie można udowodnić, że zostały wysłane do tej samej osoby. Słowo "niemożliwe" rozumiem tutaj jako "o prawdopodobieństwie, które można arbitralnie zmniejszyć, jeśli nie do zera". Zatrzymajmy się i zastanówmy nad tymi definicjami, ponieważ są one wspaniałe! Zauważmy, że najlepsza możliwa "anonimowa" moneta nie zawierałaby żadnych informacji o danej transakcji. Oznacza to, że w przypadku danej transakcji każdy nadawca jest tak samo prawdopodobny jak inny. Zatem ta definicja "nieśledzalności" jest ładna i naturalna.

3. Problemy z protokołem
To dużo powiedziane: po przeczytaniu całego artykułu moje największe pytanie brzmi jest "jak oni wybrali stałe krzywej eliptycznej?". Protokół wydaje się solidny; kto wybrał stałe? Czy istnieje plan wyboru nowych stałych w przyszłości, jeśli zajdzie taka potrzeba? Jak mogę wybrać inne stałe, jeśli zdecyduję się na rozwidlenie? Czy NSA wymyśliła CryptoNote i wybrała te stałe, aby każda sieć CryptoNote miała 10% entropii dowolnej innej monety? Kto wie. Prawdopodobnie nie jest to nic wielkiego, a każda moneta ma taki punkt krytyczny. W istocie jest to punkt centralizacji. Jeśli wszyscy będziemy radośnie rozwidlać kod CryptoNote na prawo i lewo, zardzewieją twórcy, którzy podjęli dobre decyzje dotyczące stałych. Kolejna sprawa to wspomniana już nieoczywistość: nie jest to system o zerowej wiedzy więc, po każdym kroku nadal zachowywane są pewne informacje. Andrew Poelstra napisał o tym wspaniały wpis na CryptoStackExchange: [Andrew Poelstra] Ten [schemat jednokrotnego podpisu pierścieniowego] zapewnia dobrą anonimowość, ale nawet z wymienionymi obecnie ulepszeniami nie jest to system o zerowej wiedzy. Oznacza to, że możliwość powiązania jest utrudniona, ale przeciwnik dysponujący dobrymi narzędziami analitycznymi z pewnością będzie w stanie uzyskać niezerową (dosłownie, innity razy więcej niż zero) ilość informacji.
Andrew jest tu trochę hiperboliczny. Non-zero oznacza po prostu "nie zero", a on ma na myśli infinitesimal. Nieważne, sens został osiągnięty! Idea jest następująca: dowód z zerową wiedzą nie wykorzystuje ŻADNEJ informacji do skonstruowania dowodu. Podczas gdy, na przykład, wyrocznia losowa H H(rzeczy prywatne) jest funkcją kluczy. Jest ona "losowa", jest jednoznacznie utożsamiana z kluczami w sposób, którego nikt z zewnątrz nie może powielić itd.
Dlatego gdybym był Bogiem Wszechmogącym, gdybym był greckim Bogiem Entropii i Statystyki, mógłbym przejrzeć tę śmiertelną funkcję H i odzyskać Twoje prywatne informacje.
Anonimowość można naruszyć na kilka sposobów; za każdym razem, gdy wydajesz kwotę wyjściową i ustawiasz stopień nieoznaczoności n = 0, ujawniasz się jako osoba wydająca daną Kryptowalutę i każdy może cofnąć się przez blockchain. Każdy zestaw wyjścia zaciemniającego, którego członkiem jest właśnie wydane przez Ciebie wyjście, staje się mniej niejednoznaczny o stopień 1. Stopień niejednoznaczności maleje monotonicznie z upływem czasu. Jednak użytkownicy nigdy nie muszą ustawiać niskiej liczby niejednoznaczności, ponieważ mogą udowodnić, że dokonali płatności płatności w inny sposób.
Inne wady to: dwukrotnie większe klucze niż w protokole Bitcoin, długotrwały niekontrolowany wzrost niewykorzystanych zasobów transakcji (UTXO) oraz duży blockchain. Niestety, wydaje się, że autor po prostu to zignorował, ale szczerze mówiąc, ja też bym to pewnie zignorował. Wymyślasz coś i jest to naprawdę ciężkie. Przynosisz to na jarmark okręgowy. Czy zamierzasz być jak... "hej chłopaki, patrzcie na moją naprawdę ciężką rzecz?". Czy raczej... "hej chłopaki, to coś obetnie wam włosy i wyprowadzi psa na spacer!".
Jakiś palant może wyjść z tłumu i powiedzieć: "ale, stary, ta rzecz waży z 1000 funtów i staje się cięższa za każdym razem, gdy wciąga twoje włosy lub zbiera kupy twojego psa, a to są krytyczne zadania w odniesieniu do tego twojego dzieła".
A ty po prostu wzruszasz ramionami, bo przecież coś stworzyłeś, prawda?
W każdym razie, może się pojawić jakiś inny facet, który stwierdzi, że to potrzebuje klapy, żeby żebyś mógł go od czasu do czasu rozładować. Podobno Andrew Poelstra i G. Maxwell pracują nad tym teraz, używając drzew Merkle'a i wymaganych prefiksów dla jednej połowy kluczy prywatnych Cryptonote (tej połowy, którą oddałbyś procesorowi płatniczemu). Mam nadzieję, że uda mi się coś wymyślić.

3.1. Nowa technologia. W protokole CN zaimplementowano element kryptografii, którego nie widziano wcześniej w kryptowalutach, w szczególności pomysł wykorzystania obrazów kluczy do ochrony przed podwójnym wydawaniem pieniędzy. Jest to odważne stąpanie po niebezpiecznym gruncie; bez względu na to, jak dokładnie ja lub matematyk przeanalizujemy algorytm w jakiejkolwiek białej księdze, możliwe, że jakiś 16-latek z RPA wymyśli sposób na złamanie szyfrowania. Z drugiej strony, jeśli połączysz kilka bibliotek RSA lub ECDSA. Wiesz, że to działa. Wynika to ze słynnego efektu w matematyce, który nazywa się "To, że go nie widzę, nie znaczy, że go nie ma". Przejrzałem białą księgę CryptoNote i wygląda ona dobrze. Oznacza to tylko, że nie jestem tak sprytny jak ten, kto w końcu złamie CN i BTC. Z drugiej strony, liczba oczu i mózgów próbujących złamać "starą kryptografię" jest o rzędy wielkości większa, a historia jest po jej stronie. Jednak w tym momencie jedyną szansą dla niej jest przetrwanie próby czasu.
3.2. Nowy algorytm. Van Saberhagen doskonale zauważa, że koszt inwestycji powinien rosnąć szybciej niż liniowo wraz z mocą, i opisuje doskonały algorytm pozwalający zrealizować to zadanie. Ale bez podania tego algorytmu jest to tylko stek bzdur. Domyślam się, że dowód jest w kodzie. Jednak implementacja całkowicie nowego algorytmu Proof of Work może być tak samo podatna na wykorzystanie, jak implementacja każdego nowego oprogramowania. Szczerze mówiąc, bez jakiegokolwiek wyraźnego, jasnego wyjaśnienia, jak to zostało zrobione, niekoniecznie można temu ufać. W przypadku Bitcoina zadanie było jasne: znaleźć nonce tak, aby skrót SHA był mały. Z tym algorytmem? Nie mam pojęcia. 
W ciągu ostatnich kilku lat, i prawdopodobnie przez kilka następnych, było tak, że CPU lepiej radzi sobie z rzeczami, które wymagają dużego losowego dostępu do pamięci, podczas gdy GPU i ASIC lepiej radzą sobie z sekwencyjnymi, iterowanymi danymi, które mogą być konstruowane w leniwy sposób. Wygląda więc na to, że van Saberhagen po prostu wziął konstrukcję Scryptu i albo iterował ją tak, by każdy hash zależał od wszystkich poprzednich stanów, a nie tylko od ostatniego poprzedniego stanu, albo konkatenował dane wejściowe, albo coś w tym stylu. W ten sposób wszystkie poprzednie stany muszą być przechowywane w pamięci i losowo dostępne, proces nie może być łatwo sekwencjonowany i miną lata, zanim układy ASIC będą w stanie sobie z nim poradzić. Jednak to tylko moje najlepsze przypuszczenie. Nie mam żadnego powodu, aby tak sądzić, opierając się wyłącznie na lekturze białej księgi.
Potrzebujemy wyraźnego opisu algorytmu i jakiejś analizy tego algorytmu.
3.3. Zmienne dynamiczne. Zmienne dostosowują się dynamicznie w czasie. Jest to, jeśli pamiętasz, pozytywna cecha opisana powyżej. Jest to także minus. Jeśli nie zachowa się należytej ostrożności, może to prowadzić albo do wybuchów, albo do dzikiego dryfowania. Autorzy CryptoNote proponują odrzucanie bloków, jeśli są one zbyt duże (większe niż dwukrotność mediany). W przypadku długotrwałego ataku może to doprowadzić do wykładniczego rozrostu blockchaina. Jest to mało prawdopodobne i kosztowne, ale możliwe. Ponadto, biorąc pod uwagę już i tak nieporęczny rozmiar blockchainów CryptoNote, podwojenie rozmiaru średniego bloku nawet raz czy dwa może doprowadzić do poważnych problemów z siecią. Prawdziwe "wysadzenie" w nieskończoność nie jest konieczne, aby spowodować zakłócenia, a mniejszego ataku można uniknąć z matematycznego punktu widzenia.
Aby zniechęcić do takich zachowań, wprowadzono karę w postaci wyższej opłaty (fees) za bloki o nietypowych rozmiarach; może to jednak prowadzić do wzrostu opłat w okresach wzmożonego ruchu gospodarczego, np. podczas zakupów świątecznych, co byłoby nie do zaakceptowania z punktu widzenia akceptacji monety przez klientów. To skłania nas do znalezienia matematycznego rozwiązania problemu akceptacji bloków, a nie do szukania ekonomicznej zachęty dla użytkowników.

4. Niedoskonałości białej księgi i dalsze pytania
Biała księga jest w większości dobrze zorganizowana pod względem działów, ale wyjątkowo słabo napisana i używa niespójnej terminologii. Ale panowie, dam van Saberhagenowi spokój: w białej księdze jest mnóstwo informacji. Nie może to być podręcznik, ale moim zdaniem powinno tam być przynajmniej tyle informacji, aby można było opracować technologię od podstaw. Istotne informacje są pomijane, ważne równania nie są prawidłowo indeksowane, a notacja pozostaje niewyjaśniona.
Tak zwana "standardowa sekwencja transakcji" w rozdziale 4.3 nie zawiera żadnych informacji o tym, gdzie odbywa się podpis. Niestety, znaczna część tego rozdziału jest... po prostu trudna do czytania i źle objaśniona. Typowa jest zła notacja; czy to jest klucz docelowy? Czy klucz publiczny transakcji? Który z nich jest czym? Gdzie jest obraz klucza? Gdzie jest on w ogóle używany? Sprawdź diagram!
Na przykład dwa poniższe stwierdzenia powinny być połączone razem, a następnie z diagramem objaśniającym i odrobiną pseudokodu.
[Nicolas van Saberhagen] Tożsamość podpisującego jest nierozróżnialna od innych użytkowników, których klucze publiczne znajdują się w zestawie, dopóki właściciel nie złoży drugiego podpisu przy użyciu tej samej pary kluczy.
[Nicolas van Saberhagen] Jeśli Alicja chce udowodnić, że wysłała transakcję na adres Boba, może albo ujawnić r, albo użyć dowolnego protokołu zerowej wiedzy, aby udowodnić, że zna r (np. podpisując transakcję kluczem r).
Ten aspekt CryptoNote (możliwość naruszenia własnej nieśledzoności w celu udowodnienia płatności) ma zasadnicze znaczenie dla wykorzystania tej waluty, dlatego macha się na to ręką.
Absolutnie nieuzasadnione jest wymyślenie nowego "algorytmu dowodu pracy", a następnie niezamieszczenie żadnego pseudokodu opisującego ten algorytm. Na którym. Cała wasza. Moneta. Się. Opiera. Ugh.
5. Podsumowanie
Protokół CryptoNote jest absolutnie spektakularny. Nie można go tak naprawdę porównywać z Bitcoinem, ponieważ pomiędzy publicznymi adresami użytkowników a ich transakcjami występuje warstwa anonimowości. Ponadto w całym systemie zastosowano wiele ulepszonych funkcji. W porównaniu z Bitcoinem jest to zupełnie inny sposób kryptograficznego transferu bogactwa za pomocą Blockchain opartym na Proof of Work. Nie można bezpośrednio powiedzieć, że jest to Bitcoin 2.0, ale raczej zupełnie inny protokół, który ustanawia i osiąga inne cele.
Z CryptoNote wiążą się pewne poważne problemy. Rozmiar całego projektu jest po prostu ogromny. Rozmiary kluczy są dwukrotnie większe niż zwykle. Niewykorzystane transakcje i zestawy obrazów kluczy rosną w niekontrolowany sposób. Najbardziej niepokojący jest punkt centralizacji, który pozwala anonimowej osobie w Internecie dokonać wyboru wszystkich naszych stałych krzywych eliptycznych bez tłumaczenia się.
