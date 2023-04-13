# Bárcsak tudnék rendezni (egy valszámos feladat)

Ebben a cikkben szeretnék egy kicsit bővebben a [Codeforces 1753C: Wish I Knew How to Sort](https://codeforces.com/problemset/problem/1753/C) feladat megoldásairól beszélni.

A feladatban kapunk egy bináris, $n$ elemű $a$ tömböt, amit rendezni szeretnénk, de sajnos az algel tanárunk elfelejtette megtanítani nekünk a rendezési algoritmusokat. :) Jobb ötlet híján egy randomizált módszert választunk, melyben egy lépés a következőképpen néz ki:

- Véletlenszerűen (egyenletes eloszlással, egymástól és a korábbi lépéseinktől függetlenül) rámutatunk két pozícióra a tömbben. Legyenek ezek $i$ és $j$ indexszel jelölve, ahol $i < j$.
- Ha rossz sorrendben vannak ($a_i > a_j$), megcseréljük őket, egyébként nem csinálunk semmit.

Ezt a lépést addig ismételjük, amíg a tömb rendezett nem lesz.

**Kérdés**: Mi a várható értéke a végrehajtott lépéseknek?

A feladatban technikai részlet, hogy a megoldást irreducibilis (tovább már nem egyszerűsíthető alakú) törtként kell kiírni, ennek a módszerére a végén térek ki.  

A feladatnak legalább kettő, egymástól nagyon különböző megoldása van, mindkettőt nagyon hasznos ismerni valszámos feladatok kezeléséhez.

## Megfigyelés

Az első dolog, amit a feladat kapcsán észreveszünk, hogy tudjuk hány darab $0$-ás és $1$-es található az $a$ tömbben, ami alapján be tudunk húzni egy elválasztó vonalat, ami majd a végeredményben a $0$-ák és $1$-esek határa lesz.

![Elválasztó vonal](img/elvalaszto_vonal.png)

Ezután a véletlenül választott $i < j$ pár elhelyezkedése szerint tulajdonképpen két különböző eset van:

1. $i$ az elválasztó vonal előtt, $j$ utána van.
2. Mindkettő az elválasztó vonal előtt / után van.

Az 1. esetben ha történik csere azzal hasznos munkát végeztünk, hiszen növeltük a $0$-ák számát a vonal előtt és az $1$-esek számát a vonal után, ezzel közelebb kerültünk a célállapothoz.

![Hasznos munka](img/hasznos_munka.png)

A 2. esetben ha történik csere azzal nem végeztünk hasznos munkát, hiszen pusztán határokon belül mozgattunk dolgokat, továbbra is ugyanannyi $1$-est kell még a határ bal oldaláról a jobb oldalára mozgatni, miközben a helyükre $0$-ákat hozunk.

![Haszontala munka](img/haszontalan_munka.png)

Tehát elmondható, hogy a feladat szempontjából lényegtelen a $0$-ák és $1$-esek pontos elhelyezkedése, csak az számít, hogy hány darab található belőlük a határvonal egy-egy oldalán. Hasznos lépés csak akkor történik, amikor az $i$ a határ előtt és a $j$ a határ után van, továbbá $a_i = 1$ és $a_j = 0$. Minden más esetben a lépés nem változtat az aktuális állapoton.

## Első megoldási módszer: Geometriai eloszlás és várható érték linearitása

Aki tanult valószínűségszámítást, az ráismerhet a feladatban egy nevezetes eloszlásra: ez a geometriai eloszlás. Ez mindig akkor kerül elő, amikor "addig ismétlünk valamit, amíg nem sikerül".

### Egy kis kitérő: a geometriai eloszlásról (annak, aki még nem ismeri)

Pontosabban:

1. Egy véletlen kísérletsorozatot hajtunk végre.
2. A kísérleteknek lehetséges kimenetelei közül megkülönböztetünk "sikeres" és nem "sikeres" kimeneteleket.
  - A kimenetelek halmazát szoktuk eseménynek hívni, tehát van egy "sikerült" és egy "nem sikerült" eseményünk.
  - A "sikeres" esemény valószínűsége ismert, jelölje $p$, a "nem sikeres" esemény valószínűsége ekkor $1-p$.
4. Az egyes kísérleteket egymástól függetlenül végezzük (azaz a sikeresség valószínűségét nem befolyásolják a korábbi kísérletek eredményei).
5. Akkor állunk meg, amikor sikeres eredményt kapunk.
6. Definiálunk egy (valószínűségi) változót, ami azt mondja meg, hogy hány lépést végeztünk.

Például ha az $X$ változó jelöli a lépések számát, akkor feltehetjük azt a kérdést, hogy mennyi a valószínűsége annak, hogy pontosan $i$ darab lépést végeztünk, azaz $P(X=i) = ?$.

Ehhez kellett $i-1$ darab sikertelen, majd $1$ darab sikeres kísérlet, tehát

$$P(X=i) = (1-p)^{i-1}p$$.

$i$ lehetséges értékei pedig $1$ és $\infty$ között bármilyen lépésszám.

A nevezetes eloszlások ismeretének előnye például, hogy tanuljuk a várható értéküket. Ezt az $X$ változóhoz $E(X)$-el jelöljük az angol "expectation value" kifejezésből. Definíció szerint a várható értéke egy változónak a lehetséges értékeinek a valószínűségükkel súlyozott összege, azaz

$$E(X) = \sum\limits_{i} i \cdot{} P(X=i).$$

(Aztán lehet később arról beszélni, hogy ha mintavételezzük $X$-et sokszor, azaz végrehajtjuk a véletlen kísérletsorozatot és felírjuk hány lépést végeztünk, akkor a kapott lépésszámok átlaga egyre jobban megközelíti majd ezt az elméleti várható értéket, de ez már a statisztika világa.)

#### A geometriai eloszlás várható értéke

A geometriai eloszlás elméleti várható értéke például $\frac{1}{p}$, ezt a fenti képletbe helyettesítéssel könnyen ki is számolhatjuk:

$$E(X) = \sum\limits_{i} i \cdot{} P(X=i) = \sum\limits_{i} i \cdot{} (1-p)^{i-1}p = p \sum\limits_{i=1}^{\infty} i \cdot{} (1-p)^{i-1}$$

Ilyen végtelen összegekkel már sokszor találkoztunk és sokféleképpen el lehet bánni velük. Most főleg az az $i$ szorzó zavar minket, anélkül mértani sor lenne, tehát ettől kellene megszabadulni.

Például kiindulunk az eredeti egyenlőségből és nézzük meg hogy néz ez ki ha elkezdjük kifejteni:

$$E(X) = p \sum\limits_{i=1}^{\infty} i \cdot{} (1-p)^{i-1} = p + 2p(1-p) + 3p(1-p)^2 + 4p(1-p)^3 + \cdots{}$$

Szorozzuk meg mindkét oldalt $(1-p)$-vel:

$$(1-p) E(X) = p \sum\limits_{i=1}^{\infty} i \cdot{} (1-p)^{i} = p(1-p) + 2p(1-p)^2 + 3p(1-p)^3 + 4p(1-p)^4 + \cdots{}$$

Észrevehetjük, hogy ha kivonjuk egymásból a kettőt, akkor azzal pont az $i$-s szorzóktól szabadulunk meg és egy végtelen mértani sor összegét kapjuk:

$$E(X) - (1-p) E(X) = p + p(1-p) + p(1-p)^2 + p(1-p)^3 + p(1-p)^4 + \cdots{} = p \sum\limits_{i=0}^{\infty} (1-p)^{i} = p \cdot{} \frac{1}{1-(1-p)} = 1$$

Azaz

$$E(X) - E(X) + pE(X) = 1,$$

$$pE(X) = 1,$$

$$E(X) = \frac{1}{p}.$$

Érdemes erre az eredményre emlékezni.

### Vissza a feladathoz

Valamilyen "addig csináljuk amíg nem sikerül" érzésünk van a feladattal kapcsolatban. Próbáljuk meg modellezni!

Hamar érezhető, hogy egyetlen változó kevés ahhoz, hogy modellezzük a problémát. Gondoljunk csak bele: ekkor a megállási feltétel az lenne, hogy "rendezetté vált" a tömb, ennek kellene egy fix $p$ valószínűséget meghatározni, ami minden lépésben ráadásul ugyanannyi kellene hogy legyen, hiszen a lépések függetlenek. Ezt így nehéz lenne kivitelezni...


Itt is valami ilyesmiről van szó, de a sinem elég egyetlen változóval modellezni a problémát, mivel 

és pont ennek a változónak a várható értékét kérdezi a feladat

de sajnos a (3.) függetlenségi feltétel nem teljesül


## Második megoldási módszer: Markov lánc dinamikus programozással


## Megoldás kiírása irreducibilis tört alakban
