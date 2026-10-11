# Denník vývoja hier

Erik Starovský

5ZYR23

## Stručný opis projektu
Webová aplikácia Denník vývoja hier bude určená začínajúcim a nezávislým vývojárom, ktorí potrebujú na jednom mieste evidovať svoje herné projekty, plánované funkcie, chyby a priebeh vývoja. Rieši problém roztrúsených poznámok a neprehľadných zoznamov úloh. Autor si vytvorí projekt, zaznamená úlohy a prostredníctvom vývojových príspevkov dokumentuje dosiahnutý pokrok.

Aplikácia zároveň umožní prezentovať verejné projekty ostatným používateľom. Návštevníci si budú môcť prezerať ich vývoj a registrovaní používatelia sledovať vybrané projekty a komentovať príspevky. Autor bude môcť projekt ponechať súkromný; zoznam pracovných úloh zostane prístupný iba jemu. Hlavným účelom aplikácie je spojiť jednoduchú organizáciu vývoja s prezentáciou výsledkov a získavaním spätnej väzby.

## Role v projekte
Návštevník: Neprihlásený používateľ, ktorý si prezerá a vyhľadáva verejné projekty, číta ich príspevky a komentáre. Nemôže meniť obsah.

Registrovaný používateľ a autor projektu: Môže sledovať verejné projekty a pridávať komentáre. Vytvorením vlastného projektu sa stáva jeho autorom a spravuje jeho údaje, úlohy a príspevky. Autorstvo je vzťah ku konkrétnemu projektu, nie samostatná systémová rola.

Správca: Spravuje používateľské účty a moderuje verejný obsah. Môže zablokovať účet a skryť nevhodný verejný projekt, príspevok alebo komentár. V bežnom rozhraní nemá prístup k súkromným projektom ani pracovným úlohám autorov.

## Prípady použitia podľa rolí
Návštevník: Vyhľadá verejný projekt podľa názvu, zobrazí jeho detail a príspevky, prečíta komentáre a vytvorí si používateľský účet.

Registrovaný používateľ: Prihlási sa, upraví svoj profil, začne alebo prestane sledovať projekt, zobrazí sledované projekty a pridá, upraví alebo odstráni vlastný komentár pri verejnom príspevku.

Autor projektu: Vytvorí, upraví alebo odstráni vlastný projekt a nastaví jeho viditeľnosť. Pridá úlohu typu funkcia alebo chyba, nastaví jej prioritu a stav. Vytvorí koncept príspevku, upraví ho, zverejní alebo odstráni.

Správca: Vyhľadá používateľský účet a zablokuje alebo obnoví jeho prístup. Skontroluje verejný obsah a skryje alebo obnoví jeho zobrazenie.

## Plánované entity
Databáza bude obsahovať šesť hlavných entít. Každá bude mať vlastný primárny kľúč; odkazy na súvisiace záznamy budú realizované cudzími kľúčmi.

Používateľ: Reprezentuje registrovaného používateľa alebo správcu. Atribúty: ID, používateľské meno, jedinečný e-mail, hash hesla, krátky profilový opis, systémová rola, stav účtu a dátum registrácie. Používateľ môže vlastniť projekty, písať komentáre a sledovať verejné projekty.

Projekt: Reprezentuje vyvíjanú hru. Atribúty: ID, ID autora, názov, opis, žáner, použitý herný engine, stav vývoja, viditeľnosť, stav moderovania a dátum vytvorenia. Stav vývoja môže byť plánovaný, vo vývoji, pozastavený alebo dokončený. Viditeľnosť bude verejná alebo súkromná. Projekt obsahuje úlohy a vývojové príspevky.

Úloha: Eviduje plánovanú funkciu alebo chybu v projekte. Atribúty: ID, ID projektu, názov, opis, typ, priorita, stav, voliteľný termín dokončenia a dátum vytvorenia. Typ bude funkcia alebo chyba; stav bude plánovaná, rozpracovaná alebo dokončená. Úlohu spravuje výhradne autor príslušného projektu.

Vývojový príspevok: Dokumentuje pokrok, zmeny alebo dokončené časti hry. Atribúty: ID, ID projektu, nadpis, obsah, stav publikovania, stav moderovania, dátum vytvorenia a voliteľný dátum zverejnenia. Príspevok môže byť koncept alebo zverejnený. Patrí jednému projektu a môže obsahovať viacero komentárov; jeho autor sa určuje cez projekt.

Komentár: Predstavuje spätnú väzbu k vývojovému príspevku. Atribúty: ID, ID používateľa, ID príspevku, text, stav moderovania, dátum vytvorenia a dátum poslednej úpravy. Každý komentár má jedného autora a patrí k jednému príspevku. V základnej verzii nebudú vnorené odpovede.

Sledovanie projektu: Prepája používateľa s projektom, ktorý chce sledovať. Atribúty: ID, ID používateľa, ID projektu a dátum začatia sledovania. Dvojica používateľ a projekt bude jedinečná, aby používateľ nemohol ten istý projekt sledovať opakovane.

Hodnoty ako rola, priorita a stav budú mať vopred definované možnosti. Pre základnú verziu nebudú potrebné samostatné databázové entity pre žánre ani herné enginy.

## Vzťahy medzi entitami
Používateľ – Projekt (1:N): jeden používateľ môže vlastniť viacero projektov; každý projekt má práve jedného autora.

Projekt – Úloha (1:N): projekt môže obsahovať viacero úloh; každá úloha patrí práve jednému projektu.

Projekt – Vývojový príspevok (1:N): projekt môže mať viacero príspevkov; každý príspevok patrí práve jednému projektu.

Vývojový príspevok – Komentár (1:N): príspevok môže mať viacero komentárov; každý komentár patrí práve jednému príspevku.

Používateľ – Komentár (1:N): používateľ môže napísať viacero komentárov; každý komentár má práve jedného autora.

Používateľ – Projekt (M:N cez Sledovanie projektu): používateľ môže sledovať viacero projektov a projekt môže sledovať viacero používateľov.

## Hlavné stránky aplikácie
1. Úvodná stránka a katalóg projektov: Zobrazí verejné projekty a umožní vyhľadávanie podľa názvu a filtrovanie podľa stavu vývoja alebo žánru. Poskytne vstup do detailu vybraného projektu.

2. Registrácia a prihlásenie: Umožní vytvorenie účtu a prihlásenie existujúceho používateľa. Formuláre budú kontrolovať povinné polia, formát e-mailu a pri registrácii jeho jedinečnosť.

3. Moje projekty: Zobrazí vlastné projekty prihláseného používateľa vrátane súkromných. Umožní vytvoriť nový projekt a prejsť na jeho správu.

4. Detail projektu: Predstaví názov hry, opis, autora, žáner, engine a stav vývoja. Zobrazí dostupné vývojové príspevky. Prihlásený používateľ bude môcť verejný projekt sledovať a autor prejde na jeho úpravu alebo úlohy.

5. Vytvorenie a úprava projektu: Poskytne autorovi formulár na zadanie a zmenu údajov projektu, viditeľnosti a stavu vývoja. Odstránenie projektu bude vyžadovať potvrdenie.

6. Úlohy projektu: Ponúkne autorovi zoznam úloh s filtrovaním podľa typu, priority a stavu. Autor tu vytvorí alebo upraví úlohu, zmení jej stav a odstráni nepotrebný záznam.

7. Detail vývojového príspevku: Zobrazí obsah príspevku a jeho komentáre. Prihlásenému používateľovi umožní komentovať verejný príspevok a spravovať vlastné komentáre.

8. Editor vývojového príspevku: Umožní autorovi napísať a upraviť príspevok, uložiť ho ako koncept alebo zverejniť. Koncept uvidí iba autor projektu.

9. Sledované projekty: Zobrazí používateľovi sledované projekty a ich najnovšie dostupné príspevky. Umožní otvoriť detail projektu alebo zrušiť sledovanie.

10. Profil a nastavenia účtu: Umožní používateľovi upraviť profilové údaje a zmeniť heslo. Verejný profil autora zobrazí jeho opis a verejné projekty.

11. Administrácia: Poskytne správcovi prehľad používateľských účtov a verejného obsahu. Umožní blokovanie účtov a moderovanie projektov, príspevkov a komentárov.

Prístup k stránkam aj jednotlivým záznamom sa bude overovať na serveri. Verejne bude dostupný iba zverejnený a neskrytý obsah verejných projektov. Zmena projektu na súkromný zruší jeho dostupnosť ostatným používateľom aj cez priame odkazy a zoznam sledovaných projektov. Pri odstránení projektu sa odstránia aj jeho úlohy, príspevky, komentáre a záznamy sledovania.

## Rozdelenie funkcionality
Základné funkcie

Registrácia a prihlásenie: Používateľ si vytvorí účet, prihlási sa a odhlási sa. Aplikácia rozlíši bežného používateľa a správcu a overí oprávnenia pri práci s údajmi.

Správa vlastných projektov: Autor vytvorí, zobrazí, upraví a odstráni projekt. Zadá jeho základné údaje, nastaví stav vývoja a verejnú alebo súkromnú viditeľnosť.

Evidencia úloh a chýb: Autor pridá, upraví a odstráni úlohu vlastného projektu. Určí jej typ, prioritu a stav a označí ju ako dokončenú. Úlohy zostanú dostupné iba autorovi.

Vývojové príspevky: Autor vytvorí, upraví a odstráni príspevok dokumentujúci pokrok. V základnej verzii sa uložený príspevok zobrazí podľa viditeľnosti projektu; samostatné koncepty budú rozšírením.

Prezeranie a vyhľadávanie: Návštevník si prezrie katalóg verejných projektov, vyhľadá projekt podľa názvu a otvorí jeho detail a vývojové príspevky.

Komentovanie: Prihlásený používateľ pridá komentár k dostupnému verejnému príspevku a upraví alebo odstráni vlastný komentár. Ostatní si komentáre môžu prečítať.

Základná administrácia: Správca si zobrazí verejný obsah a skryje nevhodný projekt, príspevok alebo komentár. Skrytý obsah nebude dostupný ostatným používateľom.

Rozširujúce funkcie

Sledovanie projektov: Používateľ začne alebo prestane sledovať projekt a na samostatnej stránke si prezrie sledované projekty a ich najnovšie dostupné príspevky.

Koncepty príspevkov: Autor uloží rozpracovaný príspevok bez zverejnenia a neskôr ho publikuje. Koncept bude dostupný iba jemu.

Rozšírené filtrovanie: Používateľ filtruje projekty podľa žánru a stavu vývoja; autor filtruje úlohy podľa typu, priority a stavu.

Profily a nastavenia účtu: Používateľ upraví profilový opis a heslo. Verejný profil autora zobrazí jeho opis a verejné projekty.

Termíny úloh: Autor nastaví voliteľný termín dokončenia úlohy a aplikácia zvýrazní nedokončené úlohy po termíne.

Rozšírená administrácia: Správca vyhľadá používateľský účet, zablokuje alebo obnoví jeho prístup a obnoví zobrazenie predtým skrytého obsahu.
