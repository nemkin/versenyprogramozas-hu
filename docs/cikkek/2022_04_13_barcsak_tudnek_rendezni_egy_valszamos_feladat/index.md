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

Valamilyen "addig csináljuk amíg nem sikerül" érzésünk van a feladattal kapcsolatban, tehát valamilyen geometriai valószínűségi változó lesz a háttérben, aminek mostmár ismerjük a várható értékét. Próbáljuk meg valahogy így modellezni a feladatot!

Hamar érezhető, hogy egyetlen változó kevés ahhoz, hogy modellezzük a problémát. Gondoljunk csak bele: ekkor a megállási feltétel az lenne, hogy "rendezetté vált" a tömb, ennek kellene egy fix $p$ valószínűséget meghatározni, ami minden lépésben ráadásul ugyanannyi kellene hogy legyen, hiszen a lépések függetlenek. Ez így nem fog működni.

Helyette megtehetjük azt, hogy a lépéseket szétosztjuk több valószínűségi változó között: legyen a sikeres esemény inkább az, amikor a véletlenül választott $i$ és $j$ indexeink a határvonal két oldalára esnek és végrehajtódik egy csere. Ebből az eseményből több is lesz, vagyis több valószínűségi változóra lesz szükségünk. Azonban szerencsére már a kiindulási $a$ tömbből meg tudjuk mondani, hogy pontosan hány változóra van szükség: annyira, ahány sikeres eseményre szükség van, azaz ahány $1$-es van a határvonal bal oldalán: tegyük fel, hogy ez a szám $k$!

Ekkor a szükséges lépések számát leírhatjuk $k$ darab változó összegeként. Legyen $X_i$ az a változó, ami azon lépések számát írja le, ami ahhoz kell, hogy a bal oldalon $i$ darab $1$-esből $i-1$ darab $1$-est csináljunk (tehát az utolsó lépés volt az egyetlen sikeres és hasznos csere a lépéssorozatban).

Ekkor a lépések számát mostmár az $X_k + X_{k-1} + \cdots{} + X_1$ változók összege fogja leírni, nekünk pedig ennek az összegnek a várható értékét, azaz $E(X_k + X_{k-1} + \cdots{} + X_1)$-et kell megmondanunk.

Itt van szükség arra a nagyon fontos valszám tételre, hogy **a várható érték lineáris**, azaz 

$$E(X_k + X_{k-1} + \cdots{} + X_1) = E(X_k) + E(X_{k-1}) + \cdots{} + E(X_1).$$

Ami nagyon meglepő ebben az összefüggésben az az, hogy ez még akkor is igaz, ha az egyes változók egymástól **nem függetlenek** és vannak is olyan vprog feladatok, ahol ezt a tulajdonságot nagyon erősen kihasználjuk. Ez most nem ilyen, itt a változóink függetlenek lesznek, ezért ez talán kevésbé meglepő, de akit érdekel a téma, például itt olvashat ezzel kapcsolatban: https://brilliant.org/wiki/linearity-of-expectation/. Lehet hozok majd erre építő feladatot is önképzőkörre. :)

Ekkor már csak egy $E(X_i)$-t kell megmondanunk. Amennyiben $i$ darab $1$-es van a bal oldalon, akkor a $p_i$ sikeres csere valószínűséget felírhatjuk a kedvező / összes eset darabszám hányadossal a következőképpen:

- Mivel $n$ elemű a tömb, az összes eset az, hogy hányféleképpen tudok egy rendezett indexpárt kiválasztani az elemek közül, ez $\binom{n}{2}$.
- A kedvező esetek száma az, hogy hányféleképpen tudok $1$-est választani a határvonal bal oldalán és $0$-ást a jobb oldalán.
- Amennyiben $i$ darab $1$-es van bal oldalt, tuti hogy ugyanennyi $0$-ás van jobb oldalt, ezekből akarunk egy párt választani, azaz $i\cdot{}i = i^2$ a kedvező esetek száma.

Tehát

$$p_i = \frac{i^2}{\binom{n}{2}}.$$

Ebből pedig következik, hogy a várható értéke egy változónak

$$E(X_i) = \frac{1}{p_i} = \frac{\binom{n}{2}}{i^2}.$$

Az összeg várható értéke pedig

$$E = E(X_k) + E(X_{k-1}) + \cdots{} + E(X_1) = \sum\limits_{i=1}^{k}\frac{\binom{n}{2}}{i^2}.$$

Tulajdonképpen ezzel meg is oldottuk a feladatot elméleti szinten, ennek az összegnek a kiszámítását kell leprogramozni.

Mielőtt ezt megtennénk, nézzünk meg először egy teljesen másik megoldást:

## Második megoldási módszer: Markov lánc dinamikus programozással


## Megoldás kiírása irreducibilis tört alakban