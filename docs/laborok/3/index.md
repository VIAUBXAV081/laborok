# MLOps Labor

## Bevezetés

A labor során önállóan fogsz elkészíteni egy egszerű osztályozó modellt, illetve a hozzá tartozó MLOps pipeline-t. A labor célja, hogy tapasztalatot szerezz az MLOps gyakorlati alkalmazásában.

A feladatok megoldásához az alábbi telepített szoftverekre van szükség (alternatívaként használhatjuk a [BME Cloud](https://cloud.bme.hu/) egyik virtuális gépét):

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) vagy egyéb docker konténer futtatására alkalmas környezet
- [Python](https://www.python.org/) futtató környezet
- [VS Code](https://code.visualstudio.com/) vagy egyéb kódszerkesztő
- [Git](https://git-scm.com/)

## Előkészület

A feladatok megoldása során ne felejtsd el követni a feladatbeadás folyamatát [Github](../../tudnivalok/github/GitHub.md).

### Git repository létrehozása és letöltése

1. Moodle-ben keresd meg a laborhoz tartozó meghívó URL-jét és annak segítségével hozd létre a saját repository-dat.
2. Várd meg, míg elkészül a repository, majd checkout-old ki.
3. Hozz létre egy új ágat `megoldas` néven, és ezen az ágon dolgozz.
4. A neptun.txt fájlba írd bele a Neptun kódodat. A fájlban semmi más ne szerepeljen, csak egyetlen sorban a Neptun kód 6 karaktere.

## Az elkészítendő pipeline

A labor során egy egyszerű osztályozó modellt kell készítened. Az egyszerűség kedvéért készítsünk egy Iris osztályozót. A modell bemenetként 4 db számot fog megkapni (sziromlevél és csészelevél szélesség és hosszúság), és a modellnek meg kell mondania, hogy a 3 osztály közül (Iris-setosa, Iris-virginica és Iris-versicolor) melyikhez tartozik. A feladat egy tipikus példa feladat, sok segítséget lehet hozzá találni az internetet, illetve könnyen lehet jó eredményeket elérni.

A modell elkészítésén felül, az alábbi témakörökkel kell megbírkózni:

* Verziókezelés
* Üzembe helyezés
* Monitorozás
* Automatizálás

## 1. feladat: Adatok előfeldolgozása és verziózása

### Adatok beszerzése

A feladat első lépéseként szerezzük be az adatkészletet, amin tanítani fogjuk a modellt. Az Iris adatkészlet elérhető az alábbi linken: [https://archive.ics.uci.edu/dataset/53/iris](https://archive.ics.uci.edu/dataset/53/iris)

1. Töltsük le az adatkészletet és csomagoljuk ki.
2. Másoljuk az `iris.data` fájlt a projekt `data` könyvtárába `data.csv` néven.
3. [DVC](https://dvc.org/doc/start) segítségével állítsunk be verziókövetést az adatfájlra.
   1. Az eszköz a `pip install dvc` paranccsal telepíthető.
   2. A leírásnak megfelelően kövessük a `data.csv` verzióit. Az egyszerűség kedvéért a DVC Remote-nak állítsunk be egy lokális mappát a projekt mappáján kívül.
   3. Figyeljünk rá, hogy a git segítségével csak az adatfájlhoz tartozó `data.csv.dvc` fájlt commitoljuk.
   4. Hozzuk létre az első commitot.

### Adatok előfeldolgozása

Mielőtt felhasználnánk az adatokat, szükségünk van előfeldolgozásra. Ehhez használhatunk valamilyen scriptet, de akár kézzel is megoldhatjuk:

1. Javítsuk ki az adatkészlet 2 hibás sorát. Ezekről az adatkészlet `iris.names` fájljában tudunk olvasni.
2. Cseréljük le az adatkészletben található osztály neveket indexekre 0-2 között. Mentsük el valamilyen formában, hogy melyik index melyik névhez tartozik.
3. Válasszuk szét az adatkészletet véletlenszerűen tanító és teszt adatokra 80%-20% arányban. Figyelj rá, hogy mindkét halmazban közel azonos mennyiségű adat tartozzon mindegyik osztályhoz.
4. Commitoljuk az adatkészlet változásait dvc és git segítségével.

### Beadandó

!!! example "1. feladat beadandó (5 pont)"
    * Kommitold a változtatásokat.
    * Készíts egy képernyőképet a teszt adathalmazról (legyen látható, hogy az adatokat milyen formában tárolod), és mentsd el a repository gyökerébe **`f1-1.png`** néven.
    * Készíts egy képernyőképet a verziókövetett fájlok listájáról (`dvc ls -R --dvc-only .`) és mentsd el a repository gyökerébe **`f1-2.png`** néven.

## 2. feladat: Osztályozó modell elkészítése

### Modell készítés

Készíts egy egyszerű osztályozó modellt, felhasználva az előző lépésben készített adathalmazt. A kódot igyekezz újrahasznosítható kódrészletekből kialakítani, melyeket a `common` mappába helyezz el. A futtatáshoz szükséges részeket a `main.py` fájlba írd meg.

1. Hozzd létre a modell létrehozásához szükséges kódot.
2. Tanítsd be a modellt az előkészített tanító adathalmaz segítségével.
3. Valósíts meg egy egyszerű hiperparaméter optimalizálást.
      1. Válassz ki 2 tetszőleges tanítási paramétert (pl. epoch szám, batch méret).
      2. Adj meg mindkét kiválasztott paraméternek 3 lehetséges értéket.
      3. Kimerítő keresés segítségével taníts be egy-egy modellt minden lehetséges kombinációra és válaszd ki azt a modellt, ami a legjobban teljesít.
4. Értékeld a betanított modell pontosságát a teszt adathalmazon.
5. Írj scriptet a modellek `ONNX` formátumba történő mentésére és mentsd el a legjobban teljesítő modellt a `models` mappába.

??? info "ONNX exportálás"
    Ha tensorflow-t használsz, az onnx modell előallítható a `tf2onnx` modul segítségével. Ehhez további segítséget találhatsz az előadáson bemutatott [demó kódok](https://github.com/VIAUBXAV081/demok/) között.

### Modell verziókezelés

[MLflow](https://mlflow.org/) segítségével kövessük a modellek tanítását és verziózzuk a modelleket.

1. Telepítsük az MLflow-t a `pip install mlflow` parancs segítségével.
2. Futtasuk az MLflow UI-t az alábbi opciók egyikével:
      1. Lokálisan az `mlflow server` parancs segítségével.
      2. Docker-ben a `docker-compose up -d mlflow` parancs segítségével.
      3. A UI-t mindkét esetben a [http://localhost:5000](http://localhost:5000) címen érjük el.
3. Állítsuk be az `autolog` funkciót a modell tanításra és nézzük meg a UI-on, hogy milyen metrikákat követ az MLflow.
4. A legjobban teljesítő modellt regisztráljuk be `IrisModel` néven kézzel, vagy kód segítségével. Utóbbi esetben használjuk a `mlflow.register_model(...)` parancsot.

!!! info "MLflow konfigurálása"
    Az MLflow-nak szüksége lehet a UI elérési útjára. Ezt a `mlflow.set_tracking_uri('http://localhost:5000')` sor segítségével tudjuk megadni a python scriptünk legelején.

### Beadandó

!!! example "1. feladat beadandó (15 pont)"
    * Kommitold a változtatásokat.
    * Készíts egy-egy képernyőképet az MLflow UI `Experiments` és `Models` felületéről és mentsd el **`f2-1.png`** és **`f2-2.png`** néven a repository gyökerébe.