# Skriptovanie na strane serveru a klienta

## Skriptovanie na strane serveru

### Statické stránky
* Klient pošle HTTP request serveru. Webový server si zistí zo svojej konfigurácie kde má na filesystéme hľadať súbory statických stránok, a tam sa pokúsi nájsť žiadaný súbor.

### Dynamické stránky
* Server musí vedieť zistiť, že klient nežiada statickú stránku. V konfigurácii serveru sa dá nastaviť aké súbory sa majú považovať za spustiteľné.
* Server spustí skript, predá mu vstupné parametre.
* Aplikácia dynamicky vygeneruje  HTML dokument, ktorý server pošle naspäť klientovi
* Tento prístup je to čo sa volá CGI

### Common gateway interface (CGI)
* Štandard pre generovanie dynamického obsahu webu
* Nezávislý od jazyka
* Dôležité informácie a hlavičky HTTP požiadavky sú nastavené ako premenné prostredia, telo POST metódy je presmerované na štandardný vstup skriptu ktorý si vyžiada používateľ
* Výstup sa berie zo štandardného výstupu aplikácie (skriptu)
* Nevýhody CGI:
  * Spustenie procesu (skriptu, aplikácie) zaberie nejaký systémový čas
  * Pre každý request je nutné spustiť nový proces, čo zabraňuje optimalizáciám ako je napr. cachovanie dát ktoré sa týkajú užívateľskej relácie – procesy majú každé svoju časť pamäti a navzájom si do nich nevidia. Keď jeden proces skončí, tak uvoľní pridelenú pamäť.

### FastCGI
* FastCGI server beží nezávisle od webového serveru – uchováva si pool procesov a prostriedkov. Procesy sú len pozastavené a keď príde request, tak sú okamžite obnovené
* Môžeme mať viac CGI serverov na jeden webový a naopak – load balancing
* Keď príde request od klienta, tak ho webový server prepošle FactCGI serveru, ten jednému zo svojich procesov predá request na spracovanie. Odpoveď ako výstup skriptu potom ide cez webový server naspäť klientovi. 
* Servery spolu komunikujú cez sockety a TCP

### Jazyky používané pri vývoji web. aplikácií 
* Najčastejšie sa používajú skriptovacie jazyky ako JavaScript, Python, Ruby, PHP, Perl, ...
* Výhody:
  * Rovnaký jazyk na strane klienta aj serveru
  * Rýchlejší vývoj než v kompilovanom jazyku – deployment bez kompilácie, priame patchovanie kódu na serveri
* Nevýhody:
  * Rýchlosť – pomalé kvôli interpretácii kódu – v minulosti boli dôležité hlavne rýchlosť siete a databáz

### Integrovaný skriptovací modul
* Klient pošle request na server, ten rozpozná že chce spustiteľný súbor a preženie ho cez skriptovací modul (napr. PHP). Modul je zodpovedný za spracovanie skriptu – vstup, vykonanie, výstup
* Hlavná výhoda tohto prístupu – web. server a modul sú integrované spoločne, čo urýchľuje spracovanie requestu – vstup a výstup requestu sa predá priamo v pamäti oproti FastCGI

### Web Server Gateway Interface (WSGI)
* Univerzálny interface medzi webovými servermi a webovými aplikáciami navrhnutý pre Python
* Aplikácia implementujúca tento interface musí implementovať určitú funkciu ktorá je zavolaná keď príde na nejaký konkrétny request – dáta je možné predať ako interné dátové štruktúry ako argumenty funkcie
* Frameworky Flask a Django sú postavené na WSGI

### Integrovaný webový server
* Dedikovaný webový server pre aplikáciu
* Webová aplikácia ako samostatná exe aplikácia bez akéhokoľvek serveru – interface HTTP protokolu je zaintegrovaný priamo do exe aplikácie
* Pred touto aplikáciou je ešte proxy server ktorý spracováva statické requesty a preposiela requesty aplikácii cez HTTP 
* Aplikácia môže bežať neustále, čiže si môže cachovať dáta – napr. pre udržanie užívateľskej relácie

### Node.js
* Umožňuje vytvárať JS aplikácie na strane serveru - namiesto skriptov napr. v bash-i
* Obsahuje balíčky potrebné pre serverovú aplikáciu – napr. HTTP – môžeme embedovať server s webovou aplikáciou – vytvoríme aplikáciu, ktorá bude mať HTTP server v sebe
* Jeden jazyk pre klienta aj server

### Skriptovanie na strane klienta
* Pridanie skriptu do web stránky pridáva:
  * Dynamické modifikovanie HTML a CSS
  * Handlovanie user actions v prehliadači
  * Asynchrónna komunikácia so serverom
* Hlavné výzvy:
  * Bezpečnosť - prehliadač sťahuje skripty z internetu a spúšťa ich v počítači.
  * Výkon - je obmedzený, lebo sa používajú skriptovacie jazyky a taktiež kvôli bezpečnostným opatreniam prehliadača

