# Bevezető

Ezen a hétvégén zajlott le a 2021-es Google Code Jam [Qualification Round](https://codingcompetitions.withgoogle.com/codejam/round/000000000043580a)-ja. Ennek a fordulónak a hossza 30 óra, így bármelyik időzónában élünk is a világban, bőven van 1 teljes napunk megoldani a feladatokat. A továbbjutáshoz 30 pontot kell szerezni, ennek az összeszedéséhez a szokásos módon nem volt szükség az összes feladat megoldására.

Az idei feladatsor a következő volt:
- [Reversort](https://codingcompetitions.withgoogle.com/codejam/round/000000000043580a/00000000006d0a5c) (7 pont)
- [Moons and Umbrellas](https://codingcompetitions.withgoogle.com/codejam/round/000000000043580a/00000000006d1145) (5, 11, 1 pont)
- [Reversort Engineering](https://codingcompetitions.withgoogle.com/codejam/round/000000000043580a/00000000006d12d7)  (7, 11 pont)
- [Median Sort](https://codingcompetitions.withgoogle.com/codejam/round/000000000043580a/00000000006d1284) (7, 11, 10 pont)
- [Cheating Detection](https://codingcompetitions.withgoogle.com/codejam/round/000000000043580a/00000000006d1155) (11, 20 pont)

Nézzük hogyan lehet továbbjutni... :)

1\. Feladat
----------

**Feladatkiírás**

A Reversort egy olyan algoritmus, ami egy listát tud növekvő sorrendbe állítani, a "Reverse" operáció használatával.

Ez az operációa listának egy összefüggő részét tudja megfordítani, a következő pszeudokód alapján:

(A tömbök indexelése 1-től kezdődik.)

```
Reversort(L):
  for i := 1 to len(L) - 1
    j := L[i..]-ben a minimum érték indexe
    Reverse(L[i..j])
```

A fenti kód végigiterál a tömbön és minden lépésben az aktuális pozíción lévő számot kicseréli a még hátralévő tömbben lévő minimális számmal, mindezt úgy, hogy a közöttük lévő számokat is fordított sorrendbe teszi, megfordítja ezt az egész listarészt.

Látható, hogy az iteráció végén a tömbben lévő számok növekvő sorrendben fognak állni.

Ez az algoritmus eléggé pazarló, a feladat az, hogy kiszámoljuk hogy mennyire. A bemeneten kapunk egy listát, össze-vissza sorrendben, nekünk pedig össze kell adni iterációnként a megfordított listák hosszait és ezt az összeget kiírni a kimenetre.

**Megoldás**

Egyetlen teszthalmaz van, ami rögtön látszik rajta, hogy nagyon kicsi: T=100 db teszteset, legfeljebb N=100 db számmal a listában. Ha a fenti algoritmust megnézzük, nagy vonalakban ~N-szer hajtja végre a ciklust, belül a minimum megtalálása ~N lépés, a string megfordítása szintén ~N lépés, tehát ~2\*N^2=20000 lépésben fut. Fontos tudni, hogy 1 mp körülbelül 10^7 - 10^8 darab utasításnak felel meg programozási versenyeken, tehát bőven 1 mp alatt vagyunk. A teszthalmazra 10 mp time limit van és 1 GB memóriát használhatunk, mindkettőbe bele fogunk férni az implementációval.



2. Feladat


3. Feladat
