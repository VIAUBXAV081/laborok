# MLOps Labor

## Bevezetés

A labor során önállóan fogsz elkészíteni egy egyszerű osztályozó modellt, illetve a hozzá tartozó MLOps pipeline-t. A labor célja, hogy tapasztalatot szerezz az MLOps gyakorlati alkalmazásában.

A feladatok megoldásához az alábbi telepített szoftverekre van szükség (alternatívaként használhatjuk a [BME Cloud](https://cloud.bme.hu/) egyik virtuális gépét):

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) vagy egyéb docker konténer futtatására alkalmas környezet
- [Python](https://www.python.org/) futtató környezet (pl.: 3.12-es verzió)
- [VS Code](https://code.visualstudio.com/) vagy egyéb kódszerkesztő
- [Git](https://git-scm.com/)
- [Poetry](https://python-poetry.org/) vagy egyéb függőség kezelő (opcionális)

## Előkészület

A feladatok megoldása során ne felejtsd el követni a feladatbeadás folyamatát [Github](../../tudnivalok/github/GitHub.md).

### Git repository létrehozása és letöltése

1. Moodle-ben keresd meg a laborhoz tartozó meghívó URL-jét és annak segítségével hozd létre a saját repository-dat.
2. Várd meg, míg elkészül a repository, majd checkout-old ki.
3. Hozz létre egy új ágat `megoldas` néven, és ezen az ágon dolgozz.
4. A neptun.txt fájlba írd bele a Neptun kódodat. A fájlban semmi más ne szerepeljen, csak egyetlen sorban a Neptun kód 6 karaktere.
5. Már most érdemes létrehozni egy virtuális környezetet a projekt gyökerében, és azt aktiválni.
      1. Adjuk ki a `poetry install` parancsot, ami létrehozza az előre konfigurált virtuális környezetet a `.venv` mappában.
      2. A környezet aktiválásához a `.\.venv\Scripts\activate` parancsot kell futtatni. (Megjegyzés: A környezettől függően ez eltérő lehet, tájékozodjunk az opciókról a [dokumentációban](https://python-poetry.org/docs/managing-environments/#activating-the-environment) )
      3. Sikeres aktiváció után a parancssorban megjelenik a virtuális környezet neve a sor elején (pl.: ```(lab03-mlops-py3.12) ... > ```)
      4. Ezután minden parancsot ebben a parancssorban adjunk ki, különben egyes komponenseket lehet, hogy nem fog megtalálni a rendszer.
   
   
!!! tip "Tipp"
    A virtuális környezet létrehozásához egyéni preferencia szerint lehet használni `Poetry`-t, `VENV`-t vagy `Anaconda`-t is. Az alábbi leírás a poetry-t javasolja, amennyiben ettől eltértek, használjátok a csomagok telepítéséhez az aktuális környezetre jellemző parancsokat. Erről példákat találhattok az előadás diákon, vagy az adott eszköz dokumentációjában.

## Az elkészítendő pipeline

A labor során egy egyszerű osztályozó modellt kell készítened. Az egyszerűség kedvéért készítsünk egy Iris osztályozót. A modell bemenetként 4 db számot fog megkapni (sziromlevél és csészelevél szélesség és hosszúság), és a modellnek meg kell mondania, hogy a 3 osztály közül (Iris-setosa, Iris-virginica és Iris-versicolor) melyikhez tartozik. 

!!! tip "Tipp"
    A feladat egy tipikus példa feladat, sok segítséget lehet hozzá találni az interneten, illetve könnyen lehet jó eredményeket elérni. Egy hasonlópéldát lehet találni a [demó kódok](https://github.com/VIAUBXAV081/demok/) között is. Nem érdemes ugyanakkor nagyon sok energiát a model elkészítésébe beleölni, hiszen a labor elsősorban a modellek futtatására és üzembe helyezésére fejezi a hangsúlyt, az eredmények pontossága másodlagos szempont.

A modell elkészítésén felül, az alábbi témakörökkel kell megbírkózni:

* Verziókezelés
* Üzembe helyezés
* Monitorozás
* Automatizálás

!!! success "Megoldások"
    Néhány feladatnál használni kell olyan új technológáit is, amivel előadáson ugyan foglalkoztunk, de nem biztos, hogy mindenki számára triviális a használata. Ezek azért kerültek be, hogy minél színesebb, és minél hasznosabb legyen a labor, és meg tudjuk mutatni az MLOps ezen részét is, de a rendelkezésre álló idő lehet nem mindenkinek elegendő az ismeretek stabil elsajátításához. Ezért helyenként a megoldás egyből meg van adva (rejtve, egy ilyen zöld box-ban), viszont arra kérünk mindenkit, hogy először próbálja meg a feladatokat saját maga megcsinálni, és csak utána nézzük meg a megoldást.

## 1. feladat: Adatok előfeldolgozása és verziózása

### Adatok beszerzése

A feladat első lépéseként szerezzük be az adatkészletet, amin tanítani fogjuk a modellt. Az Iris adatkészlet elérhető az alábbi linken: [https://archive.ics.uci.edu/dataset/53/iris](https://archive.ics.uci.edu/dataset/53/iris)

1. Töltsük le az adatkészletet a `Download` gomb segítségével és csomagoljuk ki. Az archívum több fájlt is tartalmaz, ezek közül csak néhányra lesz szükségünk.
2. Másoljuk az `iris.data` fájlt a projekt `data` könyvtárába `data.csv` néven.
3. [DVC](https://dvc.org/doc/start) segítségével állítsunk be verziókövetést az adatfájlra.
      1. Az eszköz a `poetry add dvc` paranccsal telepíthető.
      2. A `dvc init` segítségével tudjuk inicializálni a projektet, ez létrehoz nekünk egy `.dvc` mappát, amit majd fontos lesz git commit-olni is, hisz ez tartalmazza majd az adatfájlok tényleges elérési útvonalát.
      3. A `dvc remote add -d myremote C:\Users....` parancs segítségével lehet beállítani egy tetszőleges lokális mappát az adatok tárának. Olyan mappát válasszunk, ami a projekt mappáján kívül helyezkedik el. A parancsban a `myremote` név tetszőlegesen változtatható. 
      (Megjegyzés: A valóságban adattárnak javasolt valamilyen távoli szerver útvonalat, vagy Google Drive tárhelyet megadni, de ezek konfigurálása komplexebb, ezért használjuk a lokális mappát.)
      4. A `dvc add data/data.csv`-vel tudjuk az adott fájlra beállítani a verziókövetést.
      5. A `dvc push` illetve `dvc pull` parancsokkal tudjuk az adatot felölteni/letölteni a git-hez hasonlóan.
      6. Hozzuk létre az első commitot git segítségével. Figyeljünk rá, hogy az adatfájlok esetén csak az adatfájlhoz tartozó `*.dvc` kiterjesztésű fájlokat commitoljuk, magát az adatot ne.

### Adatok előfeldolgozása

Mielőtt felhasználnánk az adatokat, szükségünk van előfeldolgozásra. Ehhez használhatunk valamilyen scriptet, de akár kézzel is megoldhatjuk:

1. Javítsuk ki az adatkészlet 2 hibás sorát. Ezekről az adatkészlet `iris.names` fájljában tudunk olvasni.
2. Cseréljük le az adatkészletben található osztály neveket indexekre 0-2 között. Mentsük el valamilyen formában, hogy melyik index melyik névhez tartozik (pl.: egy json kiterjesztésű fájlba az adatok mellé).
3. Válasszuk szét az adatkészletet véletlenszerűen tanító és teszt adatokra 80%-20% arányban. Figyelj rá, hogy mindkét halmazban közel azonos mennyiségű adat tartozzon mindegyik osztályhoz. (Megjegyzés: Ha validációs adatokat is szeretnénk használni, akkor azt a tanító adatok közül válasszuk ki véletlenszerűen)
4. Commitoljuk az adatkészlet változásait dvc és git segítségével.

### Beadandó

!!! example "1. feladat beadandó (5 pont)"
    * Kommitold a változtatásokat.
    * Készíts egy képernyőképet a teszt adathalmazról (legyen látható, hogy az adatokat milyen formában tárolod), és mentsd el a repository gyökerébe **`f1-1.png`** néven.
    * Készíts egy képernyőképet a verziókövetett fájlok listájáról (`dvc ls -R --dvc-only .`) és mentsd el a repository gyökerébe **`f1-2.png`** néven.

## 2. feladat: Osztályozó modell elkészítése

### Modell készítés

Készíts egy egyszerű osztályozó modellt, felhasználva az előző lépésben készített adathalmazt. A kódot igyekezz újrahasznosítható kódrészletekből kialakítani, melyeket a `common` mappába helyezz el. A futtatáshoz szükséges részeket a `main.py` fájlba írd meg. A feladat megoldásához tetszőleges keretrendszert lehet használni.

1. Hozzd létre a modell létrehozásához szükséges kódot.
2. Tanítsd be a modellt az előkészített tanító adathalmaz segítségével.
3. Valósíts meg egy egyszerű hiperparaméter optimalizálást.
      1. Válassz ki 2 tetszőleges tanítási paramétert (pl. epoch szám, batch méret).
      2. Adj meg mindkét kiválasztott paraméternek 3 lehetséges értéket.
      3. Kimerítő keresés segítségével taníts be egy-egy modellt minden lehetséges kombinációra és válaszd ki azt a modellt, ami a legjobban teljesít.
4. Értékeld a betanított modell pontosságát a teszt adathalmazon.
5. Írj scriptet a modellek `ONNX` formátumba történő mentésére és mentsd el a legjobban teljesítő modellt a `models` mappába.

!!! info "ONNX exportálás"
    Ha tensorflow-t használsz, az onnx modell előallítható a `tf2onnx` modul segítségével. Ehhez további segítséget találhatsz az előadáson bemutatott [demó kódok](https://github.com/VIAUBXAV081/demok/) között, illetve az előadás diákon.

### Modell verziókezelés

[MLflow](https://mlflow.org/) segítségével kövessük a modellek tanítását és verziózzuk a modelleket. Az eszköz alkalmas hogy részletes metrikákat mutasson a tanítási folyamatokról és a modellek pontosságáról, valamint a legjobban teljesítő modelleket egy központi tárban is tudjuk menteni és így később könnyen visszakereshető lesz.

1. Telepítsük az MLflow-t a `poetry add mlflow` parancs segítségével.
2. Futtasuk az MLflow UI-t az alábbi opciók egyikével, a UI-t mindkét esetben a [http://localhost:5000](http://localhost:5000) címen érjük el:
      1. Lokálisan az `mlflow server` parancs segítségével.
      2. Docker-ben a `docker-compose up -d mlflow` parancs segítségével.
3. Állítsuk be az `autolog` funkciót a modell tanításra és nézzük meg a UI-on, hogy milyen metrikákat követ az MLflow.
4. A legjobban teljesítő modellt regisztráljuk be `IrisModel` néven kézzel, vagy kód segítségével. Utóbbi esetben használjuk a `mlflow.register_model(...)` parancsot.

!!! info "Modell regisztrálása"
    A `register_model(...)` függvény egy `run-id`-t vár `runs:/<run-id>/model` formában és egy választott modell nevet. Az aktuális tanításhoz tartozó `run-id`-t úgy tudjuk megkapni, ha a tanítást egy `with mlflow.start_run() as run:` kontextuson belül futtatjuk és a `run.info.run_id`-vel kérjük el. További infók a [dokumentációban](https://mlflow.org/docs/latest/model-registry.html).


!!! info "MLflow konfigurálása"
    Az MLflow-nak szüksége lehet a UI elérési útjára. Ezt a `mlflow.set_tracking_uri('http://localhost:5000')` sor segítségével tudjuk megadni a python scriptünk legelején.

### Beadandó

!!! example "2. feladat beadandó (15 pont)"
    * Kommitold a változtatásokat.
    * Készíts egy-egy képernyőképet az MLflow UI `Experiments` és `Models` felületéről és mentsd el **`f2-1.png`** és **`f2-2.png`** néven a repository gyökerébe.

## 3. feladat: Modell üzembe helyezése és futtatása

A kiexportált `ONNX` modellünket felhasználva készítsünk egy egyszerű API-t FastAPI segítségével, majd pedig futtassuk az API-t egy Docker konténer segítségével.

### Üzembe helyezés

A projekt gyökerében már találunk egy előre elkészített `api` mappát, ami tartalmaz egy `api.py` fájlt. Ebbe a fájlba készítsük el az API-hoz szükséges kódot.

1. Telepítsük a FastAPI futtatásához szükséges csomagot a `poetry add fastapi[standard]` paranccsal.
2. Futtassuk az API-t a `fastapi dev api/api.py` paranccsal, és nézzük meg a [http://localhost:8000/docs](http://localhost:8000/docs) oldalt.
3. Egészítsük ki az API-t, hogy segítségével használni tudjuk az előző feladatban létrehozott modellünket.
      1. Már elő van készítve egy `predict` endpoint, ami bemenetként 4 float típusú számot vár, kimenetként pedig a meghatározott osztálynevet adja szöveges formátumban. 
      2. Használjuk a már elkészített, ONNX formátumba mentett modellt a predikció megvalósításához.
      3. ONNX modell futtatásához szükségünk lesz az `onnxruntime` csomagra. Ennek használatáról információkat kaphatunk a [projekt oldaláról](https://onnxruntime.ai/).
      4. Teszteljük a kód helyes működését, ehhez használhatjuk a korábban megismert Swagger interfészt.

### Docker konténer létrehozása

Az api futtatásához használjunk egy docker konténert. A docker konténer létrehozásához szükségünk lesz egy docker image-re, amit egy `Dockerfile` leíróval, és annak buildelésével fogunk tudni létrehozni.

1. Hozzunk létre egy `requirements.txt` fájlt az `api` mappán belül. Ide vegyük fel a projekt függőségeit kézzel (akár egyszerű felsorolásként, verziószámok nélkül), vagy a `poetry export --without-hashes --output api/requirements.txt` segítségével.
2. Hozzunk létre egy `Dockerfile`-t szintén az `api` mappán belül az alábbiak szerint (az elérhető parancsokról [itt](https://docs.docker.com/reference/dockerfile/) tudunk tájékozódni):
      1. Származzunk le a `python:3.12` vagy más verziójú python képfájlból.
      2. Válasszuk ki a `/code` mappát, mint munkakönyvtár.
      3. Másoljuk át a szükséges mappákat ebbe a könyvtárba (pl.: `common`, `api`, `data`, `models`).
      4. Telepítsük a függőségeket a korábban létrehozott csomaglista leíró fájllal.
      5. Indítsuk el a FastAPI-t production módban (`fastapi run ...`) a konténer utasításaként.

??? success "Segítség a Dockerfile létrehozásához (megoldással)"
    ```dockerfile
    FROM python:3.12

    WORKDIR /code

    COPY common ./common
    COPY data ./data
    COPY models ./models
    COPY api ./api
    COPY __init__.py .

    RUN pip install --no-cache-dir -r ./api/requirements.txt

    EXPOSE 8000

    CMD ["fastapi", "run", "./api/api.py", "--port", "8000"]
    ```

### Konténer futtatása

1. Vegyük fel az új konténert a `docker-compose.yaml` fájlba. Ez a fájl a futtatot rendszereket írja le, amiket könnyen tudunk konfigurálni. A formátumról [itt](https://docs.docker.com/reference/compose-file/services/) tudunk bővebben olvasni.
      1. Hozzunk létre egy új szolgáltatást `api` néven.
      2. A build contextnek adjuk meg a gyökérkönyvtárat.
      3. A dockerfile-nak válasszuk ki az `api` mappán belül létrehozott `Dockerfile`-t.
      4. Rendeljük a `8000`-es portot a konténer `8000`-es portájához.
2. Futtasuk és teszteljük a szolgáltatást a `docker-compose up -d api` paranccsal.

??? success "Segítség a szolgáltatás felvételéhez (megoldással)"
    ```yaml
    services:
        # ...

        api:
            build:
                context: .
                dockerfile: api/Dockerfile
            ports:
            - '8000:8000'
    ```

### Beadandó

!!! example "3. feladat beadandó (10 pont)"
    * Kommitold a változtatásokat.
    * Készíts egy képernyőképet az api Swagger interfészéről és mentsd el **`f3-1.png`** néven a repository gyökerébe.
    * Készíts egy képernyőképet a Docker Desktop felületéről, vagy a `docker ps -a` parancs kimenetéről és mentsd el **`f3-2.png`** néven a repository gyökerébe.
  
## 4. feladat: API monitorozása

A futó konténer metrikáit jelenítsük meg egy grafikus dashboardon. A metrikák gyűjtéséhez használjunk Prometheus-t, a megjelenítéshez pedig Grafana-t.

### API metrikák publikálása

1. Egészítsük ki a meglévő API-t, hogy a saját metrikáit ki tudja írni:
      1. Használjuk a [prometheus-fastapi-instrumentator](https://github.com/trallnag/prometheus-fastapi-instrumentator) csomagot.
      2. Ezt a `poetry add prometheus-fastapi-instrumentator` paranccsal tudjuk telepíteni, majd pedig a `Instrumentator().instrument(app).expose(app)` sor hozzáadásával tudjuk aktiválni az `app` létrehozása után.
      3. A csomag a metrikákat a `/metrics` endpoint-on fogja publikálni.
      4. Egészítsük ki a metrikákat egy tetszőleges, a modellhez köthető saját metrikával (pl.: a modell hányszor prediktálta az egyes osztályokat). Ehhez a [dokumentációban](https://github.com/trallnag/prometheus-fastapi-instrumentator?tab=readme-ov-file#creating-new-metrics) találunk részletes leírást.
      5. Teszteljük az API `/metrics` végpont kimenetét.

??? success "Segítség új metrika létrehozásához (megoldással)"
    ```python
        def predicted_class_name() -> Callable[[Info], None]:
            # Számláló objektum, ami label-ekhez társít egy számot
            METRIC = Counter(
                "predicted_class_name",
                "Number of times a certain class predicted",
                labelnames=("class_name",)
            )

            # A számlálóhoz tartozó callback ami inkremetálja az adott label-hez tartozó számot, ha a válasz ezt tartalmazza
            def instrumentation(info: Info) -> None:
                if info.request.url.path == '/predict' and info.response.status_code == 200:
                    class_name = json.loads(info.response.body)['class_name']
                    METRIC.labels(class_name).inc()

            return instrumentation

        # Regisztráljuk az új metrikát
        (
        Instrumentator(body_handlers=[r".*"])
            .instrument(app)
            .add(predicted_class_name())
            .expose(app)
        )
    ```

### Prometheus konfigurálása

A Prometheus fogja gyűjteni az adott metrikákat. A szolgáltatás képes akár több rendszer metrikáit is gyűjteni, és ezeket egységesen tárolni. A szolgáltatást a `prometheus.yml` fájl segítségével lehet konfigurálni.

1. Adjunk hozzá egy új `scrape_configs` bejegyzést a `prometheus.yml` fájlba:
      1. Adjunk meg neki egy tetszőleg `job_name` nevet (pl.: api)
      2. A `metrics_path` legyen `/metrics`
      3. A `static_configs` alá vegyünk fel egy `targets` bejegyzést ami mutasson az api-ra. (Docker compose esetén úgy tudunk hivatkozni egy másik futú szolgáltatásra, ha megadjuk a docker-compose-ban található szolgáltatás nevét)
2. Futtassuk a `prometheus` docker konténert a `docker-compose up -d prometheus` parancs segítségével
3. Vizsgáljuk meg a Prometheus UI felületét a [http://localhost:9090](http://localhost:9090) linken.
4. Írjunk néhány egyszerű lekérdezést, és próbáljuk ki, hogy hogyan jelennek meg az API metrikái a Prometheus felületén. Példákat láthatunk a [dokumentációban](https://prometheus.io/docs/prometheus/latest/querying/examples/).

??? success "Segítség a prometheus konfigurálásához és teszteléséhez (megoldásokkal)"
    Konfigurációs fájl:
    ```yaml
    global:
        scrape_interval: 5s

        scrape_configs:
        - job_name: 'api'
          metrics_path: /metrics
          static_configs:
            - targets: ['api:8000']
    ```
    Példa query: 
    ```
    predicted_class_name_total{class_name="Iris-setosa"}
    ```

### Grafana beállítása

Ahogy láttuk, a prometheus is képes a gyűjtött metrikák vizualizálására, de nagyon limitáltak a lehetőségek. Komplexebb vizualizációkhoz használjuk a Grafana-t.

1. Futtassuk a Grafana UI felületét a `docker-compose up -d grafana` paranccsal.
2. Látogassuk meg a [http://localhost:3000](http://localhost:3000) linket (Alapértelmezetten a felhasználónév és a jelszó is `admin`).
3. Adjuk hozzá a UI-hoz a Prometheus adatforrást.
      1. A bal oldali menüben válasszuk a `Connections > Add new connection` menüpontot.
      2. Válasszuk ki a `Prometheus` adatforrást és menjünk az `Add new data source` gombra a jobb felső sarokban.
      3. A `Connection` alatt adjuk meg a prometheus szolgáltatás elérési útját (A szolgáltatás a `http://prometheus:9090` címen érhető el a Grafanából).
      4. Mentsük el a konfigurációt a `Save & test` gombbal az oldal alján.
4. Hozzunk létre néhány tetszőleges vizualizációt a Prometheus álltal összegyűjtött adatokra és mentsük el egy dashboard-ra.
      

A beállításról további információt lehet találni a [Grafana dokumentációban](https://grafana.com/docs/grafana/latest/datasources/prometheus/configure-prometheus-data-source/).

??? success "Segítség vizualizáció készítéséhez (megoldással)"

      1. Menjünk a `+` ikonra a jobb felső sarokban és válasszuk a `New dashboard` opciót.
      2. Menjünk az `Add visualization` gombra, majd válasszuk a `prometheus` adatforrást.
      3. Alul a metrikák közül a lenyíló ablakban válasszuk a `predicted_class_name_total` metrikát, a szűrőnél pedig a `class_name`-et. (Megjegyzés: a metrika csak akkor fog megjelenni, ha a prometheus tartalmazza, ehhez először csinálni kell pár predikciót az api segítségével.)
      4. Az `Options` alatt a `Legend`-hez írjunk `{{class_name}}`-et.
      5. A grafikon felett jobb felső sarokban tudjuk kiválasztani az adatok időtartományát és a frissítési rátát.
      6. Mentsük el a dashboard-ot a `Save dashboard` opcióval.

### Beadandó

!!! example "4. feladat beadandó (10 pont)"
    * Kommitold a változtatásokat.
    * Készíts egy képernyőképet az elkészült Grafana dashboard-ról és mentsd el **`f4.png`** néven a repository gyökerébe.
 
## Opcionális feladat

Automatizáljuk a tanítási és üzembe helyezési folyamatot. Készítsünk egy végpontot, amire ha a felhasználó elküldi a szükséges 4 paramétert, és a várt osztály nevet, akkor az adat automatikusan bekerül a tanító halmazra, majd pedig erre betanít egy új modellt.

1. Hozzunk létre egy új végpontot az API-n, ahol 4 double és 1 string paramétert várunk. A kapott értékeket mentsük el a tanító adatok közé. (Megjegyzés: Ahhoz, hogy ez működjön, elképzelhető, hogy új volume-okat kell hozzáadni az api konténerhez.)
2. Automatizáljuk a tanítás folyamatát [Prefect](https://docs.prefect.io/3.0/get-started/index) segítségével.
   1. Telepítsük fel a Prefect szolgáltatást `poetry add prefect` parancsal segítségével.
   2. Indítsunk el egy lokális Prefect szervert a `prefect server start` parancsal, amihez tartozó UI-t a [http://localhost:4200](http://localhost:4200) linken fogjuk elérni.
   3. Módosítsuk a meglévő tanító kódot, az egyes függvényekhez adjuk hozzá a `@task` és `@flow` annotációkat.
   4. Cseréljük le a konténerben futó modell fájlt a tanítás utolsó lépéseként legenerált modell fájlra (pl.: volume segítségével).
   5. Futtassuk a pipeline-t trigger esemény definiálása nélkül.
3. Az API végpontban manuálisan küldjön triggert a folyamat indítására (Megjegyzés: egy konténer belsejéből a `localhost` kiszolgálót a `host.docker.internal` host segítségével tudjuk elérni).

??? success "Segítség a Prefect használatához"
    **Prefect pipeline futtatás**
    ```python
    @task
    def train():
        # ...

    @task
    def deploy():
        # ...


    @flow
    def pipeline():
        train()
        deploy()


    if __name__ == '__main__':
        pipeline.serve(
            name="pipeline",
            description="train and deploy pipeline",
            tags=["pipeline"],
        )
    ```

    **Prefect trigger készítés:**
    ```python
    deployment_id = '...'
    api_url = f'http://host.docker.internal:4200/api/deployments/{deployment_id}/create_flow_run'
    try:
        payload = {
            "name": "Call from API",
            "state": {
                "type": "SCHEDULED",
                "name": "run-flow",
                "message": "Run started"
            },
            "tags": [
                "dev"
            ]
        }
        response = requests.post(api_url, json=payload)
    except requests.exceptions.RequestException as e:
        print(f'Error: {e}')
    ```

    A `deployment_id`-t megadhatjuk kézzel, de sokkal jobb ha dinamikusan kérjük el a prefect `/api/deployments/name/pipeline` végpontjától.

### Beadandó

!!! example "Opcionális feladat beadandó (10 pont)"
    * Kommitold a változtatásokat.
    * Készíts egy képernyőképet a Prefect UI felületéről, ahol látszik, hogy lefutott egy pipeline és mentsd el **`fo.png`** néven a repository gyökerébe.