# Architektúra webových aplikácií 

* Webová aplikácia sú v skutočnosti dve úzko prepojené aplikácie – jedna na strane klienta, jedna na strane serveru
* Klient je GUI aplikácia – prehliadač
* Web server je typicky batchová aplikácia
  * vstup nie z konzoly ale pomocou http
* Musíme synchronizovať stav aplikácie medzi klientom a serverom po nespoľahlivej sieti
  * Stav je napr. keď máme viacjazyčnú aplikáciu, tak aký jazyk má momentálne daný používateľ nastavený – dá sa to pamätať napr. pomocou cookies
* Statická stránka – všetci užívatelia vidia to isté
* Dynamická stránka – obsah je generovaný pre konkrétneho používateľa pomocou skriptu

## Prístupy tvorby web. aplikácií

### Traditional (CGI-like) aplikácie

* Na klientovi je spustený jednoduchý zobrazovač
* Všetka logika je implementovaná na strane serveru
* Server vracia klientovi hotový HTML dokument
* Užívateľ klikne na hyperlink alebo odošle formulár a server vráti HTML stránku

### Single page web applications
* V prehliadači u klienta je načítaná práve jedna HTML stránka
* Na klientovi je spustená JS aplikácia, ktorá so serverom komunikuje ako uzná za vhodné a upravuje DOM zobrazenej stránky
* Odpoveď od serveru je vo forme JSON-u
* Logika je implementovaná v JS na klientovi
* Server slúži ako ľahká nadstavba nad databázou
* Na komunikáciu medzi klientom a serverom slúži AJAX
  * http požiadavky sú vykonávané asynchrónne na pozadí
  
Existujú aj hybridné aplikácie ktoré sú niekde medzi týmito dvoma prístupmi
