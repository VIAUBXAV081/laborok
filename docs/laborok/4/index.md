# Android labor

## Bevezetés

A labor során a mobilon elérhető MI szolgáltatásokkal ismerkedhetünk meg.

A feladatok megoldásához az alábbi telepített szoftverekre van szükség:

- [Android Studio](https://developer.android.com/studio)
- [Git](https://git-scm.com/)

## Előkészület

A feladatok megoldása során ne felejtsd el követni a feladatbeadás folyamatát [Github](../../tudnivalok/github/GitHub.md).

### Git repository létrehozása és letöltése

1. Moodle-ben keresd meg a laborhoz tartozó meghívó URL-jét és annak segítségével hozd létre a saját repository-dat.
2. Várd meg, míg elkészül a repository, majd checkout-old ki.
3. Hozz létre egy új ágat `megoldas` néven, és ezen az ágon dolgozz.
4. A neptun.txt fájlba írd bele a Neptun kódodat. A fájlban semmi más ne szerepeljen, csak egyetlen sorban a Neptun kód 6 karaktere.

## 1. feladat: Fejlesztőkörnyezet összeállítása

### Android Studio telepítése

Töltsd le és telepítsd az Android Studiot és a benne foglalt Android SDK-t!  

!!! info "Ha még nem használtad az Android Studiot"
    Ha még nem ismered ezt a fejlesztőkörnyezetet, [ezen a linken](https://viauad02.github.io/laborok/laborok/01-android-hello-world/) találsz róla hasznos információkat.

### Projekt létrehozása

Indítsuk el az Android Studio-t, majd:

1. Hozzunk létre egy új projektet, válasszuk az *Empty Activity* lehetőséget.
1. A projekt neve legyen `GeminiTicTacToe`, a kezdő package pedig `hu.bme.aut.android.geminitictactoe`.
1. Nyelvnek válasszuk a *Kotlin*-t.
1. A minimum API szint legyen API24: Android 7.0.
1. A *Build configuration language* Kotlin DSL legyen.

!!!danger "FILE PATH"
	A projekt mindenképpen a repository-ban lévő GeminiTicTacToe könyvtárba kerüljön, és beadásnál legyen is felpusholva! A kód nélkül nem tudunk maximális pontot adni a laborra!

Ellenőrizd, hogy elérhető-e a Gemini a projektből! *(View > Tool Windows > Gemini)*  
Szükség esetén jelentkezz be a Google Accountoddal, valamint engedélyezd a projektben a kontextus használatát.

!!! example "1. feladat beadandó (1 pont)"
    * Kommitold a változtatásokat.
    * Készíts egy képernyőképet a fejlesztőkörnyezetről, úgy, hogy látszódjon a Gemini nézete, és mentsd el a repository gyökerébe **`f1.png`** néven.

## 2. feladat: Amőba alkalmazás elkészítése Gemini segítségével

Az előbb létrehozott projektben valósíts meg egy amőba játékot a Gemini segítségével!

!!! info "Nem kell túlbonyolítani"
    A feladat megoldható akár egy kérdéssel, és a válasz tartalmainak bemásolásával, további javítások nélkül.

!!! example "2a. feladat beadandó (1 pont)"
    * Kommitold a változtatásokat.
    * Készíts egy képernyőképet a futó alkalmazásról *játék közben* (emulátoron vagy saját készüléken), és mentsd el a repository gyökerébe **`f2a.png`** néven.

!!! example "2b. feladat beadandó (1 pont)"
    * Kommitold a változtatásokat.
    * Készíts egy képernyőképet a futó alkalmazásról *miután valamelyik játékos nyert* (emulátoron vagy saját készüléken), és mentsd el a repository gyökerébe **`f2b.png`** néven.

## 3. feladat: Generatív modellt használó alkalmazás

### Projekt létrehozása

Mentsük el a korábbi projektet, majd

1. Hozzunk létre egy új projektet, válasszuk a *Gemini API Starter* lehetőséget.
1. A projekt neve legyen `GeminiApiDemo`, a kezdő package pedig `hu.bme.aut.android.geminiapidemo`.
1. Nyelvnek válasszuk a *Kotlin*-t.
1. A minimum API szint legyen API24: Android 7.0.
1. A *Build configuration language* Kotlin DSL legyen.
1. A következő ablakban a linkre kattintva generáljunk magunknak API kulcsot, majd másoljuk be.

!!!danger "FILE PATH"
	A projekt mindenképpen a repository-ban lévő GeminiApiDemo könyvtárba kerüljön, és beadásnál legyen is felpusholva! A kód nélkül nem tudunk maximális pontot adni a laborra!

Vizsgáljuk meg a létrejött projekt kódját, majd próbáljuk meg futtatni a létrejött projektet!

A projekt nagy valószínűséggel nem fog elindulni, mert több helyen elavult hivatkozásokat tartalmaz.  

A fejlesztőkörnyezet által kiírt Error üzenetek alapján próbáld meg kijavítani a projekt működését!  
Futás idejű hibák esetén használd a *Logcat* nézetet!

!!! tip "Model overload"
    Ha az alkalmazás használata során az API-tól a "Model overloaded" hibaüzenet kapod vissza, próbálkozz egy későbbi időpontban. Ha ez sem segít, készítsd el így a képernyőképeket.

!!! example "3a. feladat beadandó (1 pont)"
    * Kommitold a változtatásokat.
    * Készíts egy képernyőképet a futó alkalmazásról *az első kép alapján generált szöveggel* (emulátoron vagy saját készüléken), és mentsd el a repository gyökerébe **`f3a.png`** néven.

!!! example "3b. feladat beadandó (1 pont)"
    * Kommitold a változtatásokat.
    * Készíts egy képernyőképet a futó alkalmazásról *saját promptot kipróbálva* (emulátoron vagy saját készüléken), és mentsd el a repository gyökerébe **`f3b.png`** néven.

## Opcionális feladat (megajánlott jegyért):

Készíts egy új alkalmazást, amely vagy LiteRT alapú modellt, vagy egy tetszőleges MLKit szolgáltatást használ bármilyen MI alapú funkció megvalósítására!  
A megvalósítás során használhatod segítségként a Geminit, vagy más nagy nyelvi modellt.  

!!!danger "FILE PATH"
	A projekt mindenképpen a repository-ban lévő OptionalProject könyvtárba kerüljön, és beadásnál legyen is felpusholva! A kód nélkül nem tudunk maximális pontot adni a laborra!

!!! example "Opcionális feladat beadandó (10 pont)"
    * Kommitold a változtatásokat.
    * Készíts egy képernyőképet az elkészült alkalmazás felületéről, ahol látszik, hogy mi a megvalósított funkció és mentsd el **`fo.png`** néven a repository gyökerébe.