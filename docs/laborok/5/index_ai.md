# Cloud labor

## Bevezetés

A labor során a mobilon elérhető MI szolgáltatásokkal ismerkedhetünk meg.

A feladatok megoldásához az alábbi telepített szoftverekre van szükség:

- BME EDU Account
- [Git](https://git-scm.com/)

## Előkészület

A feladatok megoldása során ne felejtsd el követni a feladatbeadás folyamatát [Github](../../tudnivalok/github/GitHub.md).

### Git repository létrehozása és letöltése

1. Moodle-ben keresd meg a laborhoz tartozó meghívó URL-jét és annak segítségével hozd létre a saját repository-dat.
2. Várd meg, míg elkészül a repository, majd checkout-old ki.
3. Hozz létre egy új ágat `megoldas` néven, és ezen az ágon dolgozz.
4. A neptun.txt fájlba írd bele a Neptun kódodat. A fájlban semmi más ne szerepeljen, csak egyetlen sorban a Neptun kód 6 karaktere.

## Google Cloud hozzáférés beállítása

1. A Moodle-ben található linken keresztül igényelj kupont a `@edu.bme.hu` végződésű e-mail címedre.
2. Erősítsd meg az igénylést a kapott e-mailben.
3. A következő beérkező e-mail alapján aktivált a krediteket.
4. A megjelenő *Billing* oldalon állíts be *Budget Alert*-et 30 dollárra. Így értesítészt fogsz kapni, ha a rendelkezésre álló 50 dollárból 30-et már elhasználtál.

!!!danger "Kreditek használata"
	Az ezután megjelenő *Billing* oldalon tudod követni a labor során felhasznált krediteket. Ezt az oldalt figyeld folyamatosan, és ha szükséges szüntess meg használt erőforrásokat! Ha pontosan követed az útmutatót, nem fogod túllépni a rendelkezésre álló kreditmennyiséget. Az útmutató nem pontos követéséből fakadó kredittúllépés a hallgató felelőssége.

A következőkben egy AutoML tanítást fogunk elvégezni a Google Cloud Vertex AI szolgáltatása segítségével. Ha bármikor elakadnál, [itt](https://cloud.google.com/vertex-ai/docs/tutorials/image-classification-automl/overview?authuser=0) tudsz további segítséget találni.


## Projekt létrehozása

1. Navigálj a Google Cloud [projekt választó felületére](https://console.cloud.google.com/projectselector2/home/dashboard?authuser=0).
2. Hozz létre egy új projektet ```AMI Labor NEPTUN``` néven, ahol a NEPTUN helyett a saját Neptun kódod szerepel. **Fontos, hogy az a Billing Account for Education legyen kiválasztva, ahol a kreditek találhatók!**  
   Az oldal tetején található keresőjébe az *Account management* keresőszót beírva és az *Account management(Billing)* opciót kiválasztva ellenőrizheted, hogy valóban a megfelelő Billing Accounthoz jött-e létre a projekt. 
4. Nyissuk meg a [Cloud Shellt](https://ssh.cloud.google.com/cloudshell/editor?authuser=0), amely egy böngészőben használható interaktív shell az erőforrások és projektek menedzselésére.
5. Másold be az alábbi kódot a konzolba, lecserélve a **PROJECT_ID**  a saját projekted azonosítójára. Ha nem találod a Projekt ID-t, [itt](https://cloud.google.com/vertex-ai/docs/tutorials/tabular-bq-prediction/prerequisites?authuser=0#find-project-id) találsz segítséget.

```bash
    gcloud config set project PROJECT_ID &&
    projectid=PROJECT_ID &&
    echo $projectid
```

A kód elmenti a projektünk azonosítóját a *projectid* változóba.  

5. Engedélyezd az IAM, Notebooks, Storage és Vertex AI API szolgáltatásokat az alábbi parancs kiadásával:
```bash
gcloud services enable iam.googleapis.com  compute.googleapis.com notebooks.googleapis.com storage.googleapis.com aiplatform.googleapis.com
```

6. Adj jogokat a felhasználói fiókodnak. Az alábbi kódban cseréld le
    - a **PROJECT_ID**-t a saját projekt azonosítódra,
    - a **USER_IDENTIFIER**-t a saját bejelentkezett usered e-mail címére (a jobb felső sarokban vagy az *IAM & Admin* oldalon tudod ellenőrizni),
    - a **ROLE**-t először a ```roles/aiplatform.user``` értékre, majd a következő futtatásnál a ```roles/storage.admin``` értékre.

```bash
gcloud projects add-iam-policy-binding PROJECT_ID --member="user:USER_IDENTIFIER" --role=ROLE
```

A fenti kód Vertex AI erőforrásokhoz és Cloud Storagehoz ad hozzáférést a megadott felhasználónak.


!!! example "1. feladat beadandó (1 pont)"
    * Készíts egy képernyőképet a Cloud Shellben lefutott parancsokról, és mentsd el a repository gyökerébe **`f1.png`** néven.

## Adathalmaz létrehozása és adat importálása

1. Navigáljunk a [Vertex AI kezdőoldalára](https://console.cloud.google.com/vertex-ai/?authuser=0) a konzolban.
2. A *Prepare Data* csempén kattintsunk a **Create dataset** gombra.
3. Az adahalmaznak adjuk az ```ami-labor-dataset``` nevet.
4. Adattípusnak az **Image**, célnak a **Single-label classification** legyen kiválasztva.
5. Válasszunk egy hozzánk közel eső régiót az erőforrásainknak (pl. **europe-west4**).
6. A **Create** gomb megnyomásával hozzuk létre az üres adathalmazt.
7. Az adathalmaz létrejötte után a *Data Import* nézetben válasszuk ki a **Select import files from Cloud Storage** opciót, és adjuk meg az alábbi csv fájl elérési útvonalát az **Import file path** mezőben (a csv fájl egy Google Cloud Bucketben található):
```
gs://cloud-samples-data/ai-platform/flowers/flowers.csv
```
8. Kattintsunk a **Continue** gombra az importálás befejezéséhez. Az importálás után betöltő oldalon megnézhetjük az adathalmazunk képeit.

!!! info "Error/Warning"
    Az adathalmaz betöltésekor több hibát is talál a rendszer. Betöltés után tekintsük meg az *Unable to import data due to errors* hibaüzenet részleteit. A későbbiekben ezzel nem kell foglalkoznunk.

!!! example "2. feladat beadandó (1 pont)"
    * Készíts egy képernyőképet a betöltött adathalmazról, és mentsd el a repository gyökerébe **`f2.png`** néven.

## AutoML Modell tanítása

1. Kattinsuk a jobb oldalon a **Train New Model** gombra.
2. Fogadjuk el az alapértelmezett beállításokat a **Continue** gombra kattintva.
3. Adjuk a modellnek a következő nevet: ```ami-labor-model```, majd kattintsunk újra a **Continue** gombra.
4. A *Training options* fülön is jók a default értékek, kattintsunk a **Continue** gombra.
5. A *Select Compute and pricing* fülön állítsuk be a Budget értékét **8 node órára**. Kattintsunk a **Start training** gombra.

!!! info "Training"
    A tanítás várhatóan körülbelül 2 óráig is eltarthat, a sikeres futásról e-mail értesítőt fogsz kapni.

!!! example "3. feladat beadandó (1 pont)"
    * Készíts egy képernyőképet a bal oldali sávon a **MODEL DEVELOPMENT/Training** fülre kattintva, ahol látszódik a tanítás alatt álló modelled, és mentsd el a repository gyökerébe **`f3.png`** néven.

## Eredmények értékelése

A tanítás végeztével a modell automatikusan kiértékelésre kerül a teszt adaton. (Mivel nem adtunk meg kézzel teszt adatot, a rendszer automatikusan elvégezte a felosztást.)  

1. Nyissuk meg a tanítás eredményeit mutató oldalt. (Ezt többféleképpen is elérhetjük, pl. a *Model Registry* fülön keresztül.)
2. Vizsgáljuk meg az eredményeket!
3. A **Labels** pont alatt kattintsuk bele egy konkrét kategóriába (pl. *roses*)! Vizsgáljuk meg a kapott eredményeket!
4. A kategórián belül egy konkrét képre is kattinsunk rá, és nézzük meg ott is a rendelkezésre álló adatokat!  

A kiértékelő felület segítségével további megfigyeléseket tehetünk az adatunkra és modellünkre vonatkozóan, például láthatjuk, hogy hol hibázik gyakran a modell, vagy mely adatpontokat tekinthetjük outlier-nek.

!!! example "4. feladat beadandó (1 pont)"
    * Készíts egy képernyőképet egy konkrét kép kiértékeléséről, és mentsd el a repository gyökerébe **`f4.png`** néven.


## Modell deploy és tesztelés

1. A modellünk oldalán válasszuk ki a **DEPLOY & TEST** tabot.
2. Kattintsunk a **Deploy to endpoint** gombra.
3. Endpoint névnek adjuk meg a következőt: ```ami-labor-automl```, majd kattintsunk a **Continue** gombra.
4. A beállításoknál a **Number of compute nodes** értékét állítsuk **1**-re, és kattintsunk a **Done**, majd a **Deploy** gombokra.
5. A **DEPLOY & TEST** tabon várjuk meg az endpoint létrejöttét, majd válasszuk ki az **Upload Image** opciót.
6. Itt egy saját képpel (pl. [ezzel](https://unsplash.com/photos/sunflower-field-under-blue-sky-during-daytime-2IzoIHBgYAo), de az Unsplashről másik képes is választhatunk) tesztejük a modellt.

!!! example "5. feladat beadandó (1 pont)"
    * Készíts egy képernyőképet a feltöltött képen kapott eredményekről, és mentsd el a repository gyökerébe **`f5.png`** néven.

!!!danger "Még nem végeztél!"
	Ha minden képernyőképet elkészítettél, akkor kövesd az alábbi lépéseket az erőforrások felszabadításához/törléséhez!

## Erőforrások felszabadítása

1. **DEPLOY & TEST** tabon a modellünk sávjának jobb szélén a 3 pöttyös menüből válasszuk az **Undeploy model** opciót.
2. Az [Endpoints](https://console.cloud.google.com/vertex-ai/endpoints?authuser=0) oldalon szintén a 3 pöttyös menüt használva válasszuk a **Remove Endpoint** opciót.
3. A [Models](https://console.cloud.google.com/vertex-ai/models?authuser=0) oldalon a 3 pöttyös menün a **Delete model** opcióval töröljük a modellünket.
4. A [Datasets](https://console.cloud.google.com/vertex-ai/datasets?authuser=0) oldalon a 3 pöttyös menün válasszuk a **Delete dataset** lehetőséget.
5. Végül a [Buckets](https://console.cloud.google.com/storage/browser?authuser=0) oldalon töröljük az adatbázisunk létrehozásakor keletkezett tárolókat.

## Opcionális feladat (megajánlott jegyért):

Futass AutoML modellt az előzőhöz hasonló módon egy másik, tetszőleges adathalmazon.  

Képernyőképekkel dokumentáld a betöltött adathalmazt, valamint a modell értékelését és az endpoint tesztelését!

!!! example "Opcionális feladat beadandó (10 pont)"
    * A képernyőképeket mentsd el **`fo1.png`**,**`fo2.png`**, stb. néven a repository gyökerébe.
