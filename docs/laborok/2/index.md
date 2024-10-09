# FullStack Labor

## Bevezetés

A labor során önállóan fogsz elkészíteni egy LLM-et használó fullstack alkalmazást. A labor célja, hogy tapasztalatot szerezz a fullstack környezetben történő fejlesztésről, illetve gyakorold a prompt írást.

A feladatok megoldásához az alábbi telepített szoftverekre van szükség (alternatívaként használhatjuk a [BME Cloud](https://cloud.bme.hu/) egyik virtuális gépét):

- [Visual Studio](https://visualstudio.microsoft.com/vs/community/)
    - `ASP.NET and web development ` workload
    - `.NET 8.0 Runtime` és `.NET SDK`
- [Node.js](https://nodejs.org/en/download/prebuilt-installer)
- [Git](https://git-scm.com/)

## Előkészület

A feladatok megoldása során ne felejtsd el követni a feladatbeadás folyamatát [Github](../../tudnivalok/github/GitHub.md).

### Git repository létrehozása és letöltése

1. Moodle-ben keresd meg a laborhoz tartozó meghívó URL-jét és annak segítségével hozd létre a saját repository-dat.
2. Várd meg, míg elkészül a repository, majd checkout-old ki.
3. Hozz létre egy új ágat `megoldas` néven, és ezen az ágon dolgozz.
4. A neptun.txt fájlba írd bele a Neptun kódodat. A fájlban semmi más ne szerepeljen, csak egyetlen sorban a Neptun kód 6 karaktere.

### Lokális LLM modell inicializálása

A feladat elkészítése során egy lokális LLM modellt fogunk használni. Ennek a módszernek az előnye, hogy nincs szükséged regisztrációra, nem kell előfizetni semmilyen modellre, és nincsenek korlátok abban, hogy hányszor tesztelsz. Ugyanakkor hátránya, hogy mivel a modellt a saját gépeden kell futtatni, ezért a számítógéped erőforrásai befolyásolják a futás gyorsaságát. A lentebb felsoroltak közül bármelyik modellt használhatod, ezek mindegyike eltérő rendszerkövetelményekkel rendelkeznek. A kisebb modellek esetében kisebb a gépigény, viszont pontatlanabb válaszokat is adhat, míg a nagyobb modellek pontosabbak, de akár több perces válaszidőre is számíthatsz.

A feladat megoldásához egy `LLama` modellt fogunk használni, amihez a modell fájlt `GGUF` formátumban kell beszereznünk. Modelleket a [https://huggingface.co/](https://huggingface.co/) oldalon tudsz böngészni. A feladat megoldásához az alábbi modelleket javasoljuk:

- Kis modell: [https://huggingface.co/bartowski/Llama-3.2-1B-Instruct-GGUF](https://huggingface.co/bartowski/Llama-3.2-1B-Instruct-GGUF)
- Közepes modell: [https://huggingface.co/bartowski/Llama-3.2-3B-Instruct-GGUF](https://huggingface.co/bartowski/Llama-3.2-3B-Instruct-GGUF)
- Nagy modell: [https://huggingface.co/bartowski/Meta-Llama-3.1-8B-Instruct-GGUF](https://huggingface.co/bartowski/Meta-Llama-3.1-8B-Instruct-GGUF)

A felsoroltak közül a közepes modellel tökéletes eredményeket lehet elérni.

Minden modell esetében a leírásban találunk egy összehasonlítást a modellek egyes altípusairól. Ez a táblázat tartalmazza a modellek méreteit is. Javasoljuk a `Q5_K_M` verziók használatát. Miután kiválasztottuk a számunkra megfelelő modellt, töltsük le a hozzá tartozó `GGUF` kiterjesztésű fájlt, majd másoljuk ezt a projekt `AI/Resources` mappájába `llama.gguf` néven.

### Projektek elindítása

A solution 3 projektből áll:
    
- **AI**: Az LLM modell beburkolására szolgáló WebAPI, ami a [http://localhost:5555/](http://localhost:5555/) címen érhető el. Az API leírását a [/swagger/index.html](http://localhost:5555/swagger/index.html) címen tudjuk kiolvasni.
- **Server**: A FullStack alkalmazáshoz tartozó backend. A [http://localhost:5130/](http://localhost:5130/) címen érhető el. Az API leírását a [/swagger/index.html](http://localhost:5130/swagger/index.html) címen tudjuk kiolvasni.
- **Client**: A FullStack alkalmazáshoz tartozó React frontend. A [http://localhost:5173/](http://localhost:5173/) címen érhető el.

A futtatáshoz az `AI` és a `Server` projekteket kell elindítani:

- Menjünk a `Configure Startup Projects...` opcióra.

    ![](assets/configure-startup-projects.jpg)

- Jelöljük be a `Multiple statup projects:` jelölőt, és válasszuk ki a `Start` opciót az `AI` és a `Server` projekteknél.

    ![](assets/multiple-startup-projects.jpg)

- Menjünk az `OK` gombra, majd `Start`.

    ![](assets/start.jpg)

## Az elkészítendő alkalmazás

A labor során egy olyan LLM-et használó fullstack alkalmazást kell önállóan elkészítened, mely a munkaerőtoborzással kapcsolatos egyes feladatokat könnyíti meg. Az alkalmazásban nincs szükség authentikációra, adatbázisra, vagy perzisztenciára. Az alkalmazás állapot mentes. Az elkészülő HR asszisztens alkalmazás célja, hogy támogassa a HR munkatársakat a beérkező jelentkezések feldolgozására. A markdown formátumban megküldött önéletrajzok alapján, a szoftvernek képesnek kell lennie a következő feladatok megoldására:

- A jelentkező kulcs kompetenciáinak kigyűjtése
- Azoknak a pozícióknak a meghatározása, melyekre a jelentkező alkalmas lehet
- A jelentkező legfontosabb adatainak táblázatba rendezése
- Lehetséges interjú kérdések generálása
- Automatikus levél generálása interjúra törénő behíváshoz


## 0. feladat

Az alkalmazás funkcióinak teszteléséhez elengedhetetlen egy markdown önéletrajz. Az első feladat során ezt kell létrehoznod, hogy a későbbiekben fel tudd majd használni. Az önéletrajz mindenképp tartalmazza a jelentkező nevét és egyéb fontos adatait, a tanulmányainak és korábbi munkatapasztalatainak bemutatását, valamint főbb készségeit. Az adatok tetszőlegesen megadhatók, formailag azonban mindenképp kövesse az önéletrajzok főbb szempontjait. Az önéletrajz létrehozására használható a kiinduló kliens *Editor* felülete.

??? info "Tipp"
    A Client/src/App.tsx fájlban megadható default önéletrajz, amivel a tesztelés tovább egyszerűsíthető/gyorsítható.

### Beadandó

!!! example "0. feladat beadandó (0 pont)"
    Készíts képernyőképet az elkészült önéletrajzról, majd f0.png néven másold a repository gyökerébe!


## X. feladat

A feladat leírása ...

Aminek itt kell lennie a feladatokban:

- Markdown CV létrehozása - pipa
- Service létrehozása és regisztrálása (+ modellek)
- Controller létrehozása és service meghívása
- Promptok megírása
- Kliens service megírása

??? info "Segítség"
    Segítség, tippek ...stb.

### Beadandó

!!! example "1. feladat beadandó (?? pont)"
    Egy-egy képernyőképpel alátámasztva demonstrálja az alábbiak megoldását:

    * ...
    * A készített képernyőképet másold **`fx-1.png`** és **`fx-2.png`** néven a repository gyökerébe!

## Opcionális feladat

A feladat leírása ...

Aminek itt kell lennie a feladatokban:

- JSON API lecserélése Stream API-ra.
- Kliensben service lecserélése, az oldalakon a megfelelő metódus meghívása. 

### Beadandó

!!! example "Opcionális feladat beadandó (10 pont)"
    Egy-egy képernyőképpel alátámasztva demonstrálja az alábbiak megoldását:

    * ...
    * A készített képernyőképet másold **`fx-1.png`** és **`fx-2.png`** néven a repository gyökerébe!