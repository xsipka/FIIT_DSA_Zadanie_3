# DÁTOVÉ ŠTRUKTÚRY A ALGORITMY

## Popolvár


### Vysvetlenie funkcionality


#### Inicializácia potrebných dátových štruktúr
Po spustení funkcie zachran_princezne(), kde ako hlavné argumenty dostaneme mapu a jej rozmery, musíme najprv inicializovať 
prioritný rad, do ktorého sa budú ukladať navštívené susedné vrcholy v grafe, a ktorý je implementovaný pomocou binárnej haldy. 
Inicializujeme ho vo funkcii queue_init(), a pridelíme mu dostatočne veľkú pamäť, aby sa doň zmestili informácie o všetkých 
navštívených vrcholoch. Keď je prioritný rad inicializovaný, tak môžeme inicializovať mapu, kde budú uložené požadované informácie
o všetkých vrcholoch v grafe. Mapa je tvorená dvojrozmerným poľom štruktúr, v ktorej sú všetky potrebné údaje, ako napríklad 
súradnice vrcholu, ukazovateľ na predchádzajúci vrchol, stav, ktorý hovorí, či bol vrchol navštívený a podobne. Vo funkcii 
map_init() pridelíme mape pamäť a vložíme do nej všetky údaje, nastavíme dĺžku prejdenú od začiatku na INFINITY, stav vrcholov 
na UNVISITED a predchádzajúci vrchol na NULL. Okrem toho si každú princeznú označím špecifickým znakom, keďže je dôležité v akom
poradí ich budeme vyhľadávať. Po inicializácii mapy vyrátame všetky spôsoby v ktorých je možné princezné navštíviť. Všetky tieto
spôsoby vyhľadávania si uložíme do dvojrozmerného poľa.

#### Vyhľadanie draka a zistenie spiatočnej cesty
Mapu začíname prehľadávať od súradníc (0, 0). Tento vrchol najprv vložíme do prioritného radu, kde mu ako kľúč nastavíme vzdialenosť
od počiatočného vrcholu. Keďže tento vrchol je počiatočný, tak ho nastavíme na jeho hodnotu (ak H, tak 2, ináč 1).  Taktiež si do 
MAPA *path uložíme jeho súradnice a zmeníme jeho stav v mape na navštívený.

Potom sa spustí funkcia heapify(), ale keďže zatiaľ máme vložený len jeden vrchol, tak sa nemá čo diať. Táto funkcia sa spúšťa vždy
po vložení, alebo odstránení prvku z haldy a dáva haldu do správneho stavu, aby sa na prvom indexe nachádzal prvok s najmenším kľúčom, 
keďže z haldy vyberáme vždy len najmenší.

Po vložení vrcholu sa spustí cyklus, ktorý sa bude vykonávať pokiaľ sa nedostaneme k drakovi. Na jeho začiatku sa vyberie z haldy vrchol
s najmenším ohodnotením a potom sa spustí funkcia dijkstra(), v ktorej prebieha menovaný algoritmus.  V nej sa pozrieme na všetkých
susedov vybraného vrcholu. Zistíme, či sa daný sused nachádza v hraniciach mapy, či už nebol navštívený, alebo či daný vrchol 
nesymbolizuje prekážku N. Ak spĺňa všetky z vyššie menovaných podmienok, tak najprv mu nastavíme predchádzajúci vrchol 
(vrchol vybraný z haldy) a až potom ho do haldy vložíme. Toto vykonáme pre všetky susedné vrcholy, ktoré je možné do haldy vložiť. 
Halda sa medzitým usporiada a na prvom mieste bude vždy uložený vrchol, ktorý je najmenej vzdialený od počiatočného vrcholu. Vyberieme
ten a znova opakujeme funkciu dijkstra(). Keď z haldy vyberieme prvok, a na jeho súradniciach sa nachádza drak, tak sa vyhľadávanie 
draka končí.

Ak máme draka nájdeného, tak si zapamätáme jeho súradnice, keďže ich ešte budeme potrebovať. Okrem toho nastaví premennú dragon_dead
na TRUE. Potom zisťujeme súradnice prejdenej cesty a jej dĺžku. Vykonávame to pomocou rekurzie, kde do funkcie vkladáme súradnice 
predchádzajúceho vrcholu až dovtedy, pokiaľ nebude predchádzajúci vrchol existovať. V tom prípade vieme, že sme sa dostali na začiatok
a cesta k drakovi je kompletná. Uložíme si ju do pomocného poľa, neskôr bude ešte potrebná.

#### Nová inicializácia mapy
Po nájdení draka je potrebné mapu znova inicializovať, keďže teraz už budeme hľadať princezné a údaje, ktoré v mape momentálne existujú,
by nadchádzajúce vyhľadávanie mohli pokaziť. Okrem toho sa v prioritnom rade ešte stále môžu nachádzať vrcholy z predchádzajúceho 
vyhľadávania, takže tie treba odstrániť a nanovo inicializovať aj ten. Na všetky tieto akcie slúži funkcia new_map_init(). Mapu
neinicializujeme nanovo celú, stačí ak len znova nastavíme dĺžky na INFINITY, stav na UNVISITED a predchádzajúci vrchol na NULL. 

 
#### Vyhľadávanie princezien a zistenie najkratšej cesty
Zatiaľ je halda prázdna, teraz do nej ako prvý vložíme vrchol, na ktorom sa nachádza drak a znova do nej len vkladáme susedné vrcholy. 
Od draka nemôžeme ísť hneď k najbližšej princeznej a od nej tiež k najbližšej, lebo to nemusí znamenať, že aj celková výsledná cesta 
by bola najkratšia. Preto vyhľadávame princezné viacerými spôsobmi a výsledné cesty porovnávame. Celkový počet vyhľadaní závisí od 
počtu princezien a je rovný n! (n – počet princezien). Uvažujme, že n = 3. V tomto prípade bude možné princezné vyhľadať šiestimi 
spôsobmi (123, 132, 213, 231, 312, 321 (1; 2; 3 – čísla princezien)). Keďže počet princezien na mape je 3 a spôsobov ako ich vyhľadať
je 6, tak na vyhľadanie sú použité dva for cykly. V prvom ideme od 0, po n! a vo vnorenom cykle ideme od 0 po n. V týchto dvoch cykloch
je vnorený ešte jeden nekonečný while cyklus, v ktorom prebieha samotné vyhľadávanie. To je rovnaké, ako pri drakovi, len podmienka jeho
ukončenia je iná. Ukončí sa vtedy, ak nájdeme požadovanú princeznú. Táto informácia je uložená v dvojrozmernom poli všetkých možných 
permutácii. Index požadovanej princeznej sa zhoduje s aktuálnymi hodnotami oboch for cyklov, takže vždy nájdeme princeznú, ktorú práve
potrebujeme.  

Keď je prvá princezná nájdená, zmení premennú princess_found z FALSE na TRUE a znova pomocou rekurzie zistí cestu naspäť k pôvodnému
vrcholu a súradnice tejto cesty uloží do pomocného poľa. Uloží si súradnice prvej princeznej, znova zmení princess_found na FALSE a
začne vyhľadávať ďalšiu. Predtým je ale znova potrebné nanovo inicializovať mapu a binárnu haldu, keďže ináč by mohlo dôjsť k 
zacykleniu programu. Do haldy vložíme nový počiatočný vrchol, tentokrát vrchol, na ktorom sa nachádza prvá princezná a opakujeme 
vyhľadávanie. To isté sa vykoná aj pre tretiu princeznú.

Po nájdení všetkých troch uložíme súradnice výslednej cesty do poľa. Máme dve pomocné polia, jedno je určené na uloženie cesty a ďalšie
na jej dĺžku. Do pomocného poľa znova uložíme cestu k drakovi, keďže ďalšie vyhľadávanie sa začína odtiaľ. Taktiež je potrebné nanovo
inicializovať mapu, lebo nám ešte ostáva ďalších päť spôsobov vyhľadania. Tie prebiehajú rovnako, ako ten prvý. Po prebehnutí všetkých
vyhľadaní budeme mať v dvoch pomocných poliach uložené súradnice všetkých ciest a taktiež ich dĺžky. Tie porovnáme a vyberieme z nich
najkratšiu, ktorú nám funkcia vráti. Pred vrátením je ešte potrebné nastaviť argument dlzka_cesty na počet párov súradníc.

#### Fungovanie prioritného radu
V programe bolo potrebné mať využitý prioritný rad, ktorý som implementoval pomocou binárnej haldy. V tomto prípade bolo potrebné mať
na prvom indexe radu najmenší prvok, lebo tie bolo potrebné vyberať pri zisťovaní najkratšej cesty k požadovanému vrcholu. Pri vkladaní
nastaví kľúčovú hodnotu, podľa ktorej sa bude halda upravovať. Tá sa vždy rovná predchádzajúcej hodnote, ku ktorej prirátame 
ohodnotenie daného vrcholu. Okrem toho sa uložia aj súradnice každého vloženého vrcholu. Po vložení sa spustí funkcia heapify(),
v ktorej dochádza k upraveniu poradia prvkov. Novovložený prvok sa uloží do poľa na posledný index a potom sa začne preusporiadanie.
To prebieha tak, že porovnávame detské prvky s rodičovskými a ak je hodnota detského prvku nižšia, ako hodnota rodiča, tak ich medzi
sebou vymeníme. Postupne postupujeme hore, k rodičovským prvkom a znova ich porovnávame. Toto opakujeme dovtedy, pokým sa nedostaneme
na nultý index poľa. Čiže pri vkladaní prvku preusporadúvame  od posledného a postupujeme hore. Z haldy vyberáme vždy len najmenší 
prvok, ktorý sa vždy bude nachádzať na nultom indexe poľa. Pri vymazávaní vymeníme prvok na nultom indexe s posledným, takže teraz 
sa na nultom indexe bude nachádzať posledný prvok. Znova sa spustí funkcia heapify(), a prebehne preusporiadanie. Tentokrát 
postupujeme smerom dole a prvok z nultého indexu porovnávame s jeho deťmi. Porovnávanie a preusporiadávanie prebieha dovtedy, pokým
rodičovský prvok nebude menší ako jeho deti. Ešte predtým sa zastaví a binárna halda bude v správnom tvare.

#### Testovanie programu 
Program som testoval už počas jeho samotnej tvorby. Najprv som testoval funkcionalitu prioritného radu pridávaním prvkov rôznych
veľkostí. Pomocou vypísania celého radu som zistil, či usporiadanie prebehlo správne, a až potom som sa presunul na ďalší krok. 
V tom som testoval odoberanie minima a znova som pomocou výpisov testoval, či sa na prvom mieste bude nachádzať prvok s najmenšou
hodnotou. Na ďalšiu tvorbu programu som sa presunul až keď bola binárna halda plne funkčná. Potom som testoval funkčnosť 
Dijkstrovho algoritmu. Ako vstup som použil vlastné mapy a pomocou vypísaných súradníc som testoval, či sa program dokáže na vybraný
vrchol dostať najkratšou cestou. Plne funkčný program som testoval na rôznych vstupoch s rôznym počtom princezien a vypisoval som si
súradnice vrátenej cesty. 

#### Záver
Vo svojom riešení som implementoval Dijkstrov algoritmus s využitím prioritného radu, ktorý som vytvoril pomocou binárnej haldy,
na vyhľadanie najkratšej cesty ku všetkým princeznám v grafe. Dané riešenie bolo implementované dvojrozmerným poľom štruktúr, ktoré
symbolizovalo všetky vrcholy v mape. Do binárnej haldy som vkladal susedné vrcholy vrcholu v ktorom som sa nachádzal. Vrchol s
najmenším ohodnotením som z haldy vybral a zas vkladal jeho susedov. Opakoval som to dovtedy, pokým som nedorazil k požadovanému
vrcholu. Keďže môj postup bol riešený len dvojrozmerným poľom, tak po každom vyhľadaní bolo potrebné pole znova čiastočne 
inicializovať. Spôsob vyhľadania najkratšej cesty som riešil tak, že som vyskúšal všetky možné spôsoby v ktorých je možné vrcholy
s princeznami navštíviť. Tieto cesty som si ukladal do poľa, aj spolu s ich dĺžkami a po vyskúšaní všetkých možností som ich medzi
sebou porovnal a vrátil najkratšiu. 
