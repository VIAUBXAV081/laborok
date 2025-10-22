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
    A feladat megoldható akár egy prompttal/kérdéssel, és a válasz tartalmainak bemásolásával, további javítások nélkül.

!!! example "2. feladat beadandó (2 pont)"
    * Kommitold a változtatásokat.
    * Készíts egy képernyőképet a futó alkalmazásról *miután valamelyik játékos nyert* (emulátoron vagy saját készüléken), és mentsd el a repository gyökerébe **`f2.png`** néven.

## 3. Feladat: MLKit Objektum Detektálás Android Alkalmazásban

Ebben a laborgyakorlatban egy Android alkalmazást fogunk készíteni, amely valós időben képes objektumokat felismerni a kamera képén. A Google ML Kit beépített objektum detektálási képességeit fogjuk használni, amely előre betanított modellekkel rendelkezik a leggyakoribb objektumok felismeréséhez.

### Projekt létrehozása

#### Új projekt indítása

1. Nyisd meg az Android Studio-t
2. Válaszd a "New Project" opciót
3. Válaszd az "Empty Activity" sablont
4. Konfiguráld a projektet:  
    - **Name**: `ObjectDetectionLab` 
    - **Package name**: `hu.bme.aut.android.objectdetectionlab`  
    - **Save location**: `ObjectDetectionLab` könyvtár a repositoryban  
    - **Minimum SDK**: API 24 (Android 7.0)  
    - **Build configuration language**: Kotlin DSL  

!!!danger "FILE PATH"
	A projekt mindenképpen a repository-ban lévő ObjectDetectionLab könyvtárba kerüljön, és beadásnál legyen is felpusholva! A kód nélkül nem tudunk maximális pontot adni a laborra!

### Projekt struktúra

A létrehozott projekt főbb részei:

```
ObjectDetectionLab/
├── app/
│   ├── src/
│   │   ├── main/
│   │   │   ├── hu/bme/aut/android/objectdetectionlab/
│   │   │   │   └── MainActivity.kt
│   │   │   ├── res/
│   │   │   │   ├── layout/
│   │   │   │   ├── values/
│   │   │   │   └── ...
│   │   │   └── AndroidManifest.xml
│   └── build.gradle.kts
├── build.gradle.kts
└── settings.gradle.kts
```

## Függőségek hozzáadása

Nyisd meg az `app/build.gradle.kts` fájlt és add hozzá az alábbi függőségeket a  *dependencies* részhez:

```kotlin
    // CameraX
    val cameraxVersion = "1.5.1"
    implementation("androidx.camera:camera-core:$cameraxVersion")
    implementation("androidx.camera:camera-camera2:$cameraxVersion")
    implementation("androidx.camera:camera-lifecycle:$cameraxVersion")
    implementation("androidx.camera:camera-view:$cameraxVersion")
    
    // ML Kit Object Detection
    implementation("com.google.mlkit:object-detection:17.0.2")
    
    // Permissions handling
    implementation("com.google.accompanist:accompanist-permissions:0.37.3")

    // Material Icons
    implementation("androidx.compose.material:material-icons-extended:1.7.8")
```
!!! info "Függőségek magyarázata"
    - CameraX: Google kamera könyvtár egyszerűsített API-val
    - ML Kit: Google gépi tanulási könyvtár objektum felismeréshez
    - Accompanist: Segédkönyvtár az engedélyek egyszerű kezeléséhez  
    - Material Icons: Modern ikonkészlet


Kattints a "Sync Now" linkre a felugró sávban, vagy használd a File → Sync Project with Gradle Files menüpontot.

## Engedélyek beállítása

### AndroidManifest.xml módosítása

Nyisd meg az `app/src/main/AndroidManifest.xml` fájlt és add hozzá a kamera engedélyt:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <!-- Kamera engedély -->
    <uses-permission android:name="android.permission.CAMERA" />
    
    <!-- Kamera hardver követelmény -->
    <uses-feature android:name="android.hardware.camera" android:required="true" />
    <uses-feature android:name="android.hardware.camera.autofocus" />

    <application
        ...
```

!!! info "Manifest beállítások"
    - CAMERA permission: Engedély a kamera használatához
    - camera feature: Jelzi, hogy az app kamerát használ (required=true: csak kamerás eszközökön fut)
    - autofocus: Az autofókusz funkció használata (nem kötelező, de ajánlott)
## Kamera kezelő osztály létrehozása

### CameraManager.kt

Hozz létre egy új Kotlin fájlt `CameraManager.kt` néven a projekt fő package-ében:

```kotlin
package hu.bme.aut.android.objectdetectionlab

import android.content.Context
import androidx.camera.core.*
import androidx.camera.lifecycle.ProcessCameraProvider
import androidx.camera.view.PreviewView
import androidx.compose.runtime.Composable
import androidx.compose.runtime.remember
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.platform.LocalLifecycleOwner
import androidx.compose.ui.viewinterop.AndroidView
import androidx.core.content.ContextCompat
import androidx.lifecycle.LifecycleOwner
import com.google.mlkit.vision.common.InputImage
import com.google.mlkit.vision.objects.ObjectDetection
import com.google.mlkit.vision.objects.ObjectDetector
import com.google.mlkit.vision.objects.defaults.ObjectDetectorOptions
import java.util.concurrent.ExecutorService
import java.util.concurrent.Executors

class CameraManager(
    private val context: Context,
    private val lifecycleOwner: LifecycleOwner,
    private val onObjectsDetected: (List<DetectedObject>) -> Unit
) {
    private lateinit var cameraExecutor: ExecutorService
    private lateinit var objectDetector: ObjectDetector
    private var camera: Camera? = null
    private var preview: Preview? = null
    private var imageAnalyzer: ImageAnalysis? = null
    private var cameraProvider: ProcessCameraProvider? = null

    // Kamera kép méretei
    private var imageWidth: Int = 0
    private var imageHeight: Int = 0

    init {
        setupObjectDetector()
        cameraExecutor = Executors.newSingleThreadExecutor()
    }

    private fun setupObjectDetector() {
        // ML Kit objektum detektor beállítása
        val options = ObjectDetectorOptions.Builder()
            .setDetectorMode(ObjectDetectorOptions.STREAM_MODE) // Valós idejű mód
            .enableClassification() // Osztályozás engedélyezése
            .enableMultipleObjects() // Több objektum detektálása
            .build()

        objectDetector = ObjectDetection.getClient(options)
    }

    fun startCamera(previewView: PreviewView) {
        val cameraProviderFuture = ProcessCameraProvider.getInstance(context)

        cameraProviderFuture.addListener({
            cameraProvider = cameraProviderFuture.get()

            // Preview beállítása
            preview = Preview.Builder()
                .build()
                .also {
                    it.setSurfaceProvider(previewView.surfaceProvider)
                }

            // Képelemző beállítása
            imageAnalyzer = ImageAnalysis.Builder()
                .setBackpressureStrategy(ImageAnalysis.STRATEGY_KEEP_ONLY_LATEST)
                .build()
                .also {
                    it.setAnalyzer(cameraExecutor, ObjectDetectionAnalyzer())
                }

            // Kamera választása (hátsó kamera)
            val cameraSelector = CameraSelector.DEFAULT_BACK_CAMERA

            try {
                // Előző kötések megszüntetése
                cameraProvider?.unbindAll()

                // Kamera kötése az életciklushoz
                camera = cameraProvider?.bindToLifecycle(
                    lifecycleOwner,
                    cameraSelector,
                    preview,
                    imageAnalyzer
                )

            } catch (exc: Exception) {
                exc.printStackTrace()
            }

        }, ContextCompat.getMainExecutor(context))
    }

    private inner class ObjectDetectionAnalyzer : ImageAnalysis.Analyzer {
        @androidx.camera.core.ExperimentalGetImage
        override fun analyze(imageProxy: ImageProxy) {
            val mediaImage = imageProxy.image
            if (mediaImage != null) {
                // Kép méretének mentése
                imageWidth = mediaImage.width
                imageHeight = mediaImage.height

                val image = InputImage.fromMediaImage(
                    mediaImage,
                    imageProxy.imageInfo.rotationDegrees
                )

                // Objektum detektálás végrehajtása
                objectDetector.process(image)
                    .addOnSuccessListener { objects ->
                        // Detektált objektumok feldolgozása
                        val detectedObjects = objects.map { obj ->
                            DetectedObject(
                                label = getObjectLabel(obj.labels.firstOrNull()?.text),
                                confidence = obj.labels.firstOrNull()?.confidence ?: 0f,
                                boundingBox = obj.boundingBox,
                                imageWidth = imageWidth,
                                imageHeight = imageHeight,
                                rotation = imageProxy.imageInfo.rotationDegrees
                            )
                        }
                        onObjectsDetected(detectedObjects)
                    }
                    .addOnFailureListener { e ->
                        e.printStackTrace()
                    }
                    .addOnCompleteListener {
                        imageProxy.close()
                    }
            } else {
                imageProxy.close()
            }
        }
    }

    private fun getObjectLabel(label: String?): String {
        return when (label) {
            "Fashion good" -> "Ruházat"
            "Food" -> "Étel"
            "Home good" -> "Háztartási tárgy"
            "Place" -> "Hely"
            "Plant" -> "Növény"
            else -> label ?: "Ismeretlen"
        }
    }

    fun shutdown() {
        cameraExecutor.shutdown()
        objectDetector.close()
    }
}

// Detektált objektum adatszerkezet
data class DetectedObject(
    val label: String,
    val confidence: Float,
    val boundingBox: android.graphics.Rect,
    val imageWidth: Int,
    val imageHeight: Int,
    val rotation: Int
) {
    fun getScaledBoundingBox(screenWidth: Float, screenHeight: Float): android.graphics.RectF {
        // Rotáció figyelembevétele
        val sourceWidth = if (rotation == 90 || rotation == 270) imageHeight else imageWidth
        val sourceHeight = if (rotation == 90 || rotation == 270) imageWidth else imageHeight

        // Skálázási faktor számítása (PreviewView FILL_CENTER móddal)
        val widthRatio = screenWidth / sourceWidth
        val heightRatio = screenHeight / sourceHeight
        val scaleFactor = maxOf(widthRatio, heightRatio)

        // Eltolás számítása (center)
        val scaledWidth = sourceWidth * scaleFactor
        val scaledHeight = sourceHeight * scaleFactor
        val offsetX = (screenWidth - scaledWidth) / 2f
        val offsetY = (screenHeight - scaledHeight) / 2f

        // Koordináták átszámítása
        return android.graphics.RectF(
            boundingBox.left * scaleFactor + offsetX,
            boundingBox.top * scaleFactor + offsetY,
            boundingBox.right * scaleFactor + offsetX,
            boundingBox.bottom * scaleFactor + offsetY
        )
    }
}
```
!!! info "CameraManager konstruktor"
    - context: Android rendszer szolgáltatások eléréséhez
    - lifecycleOwner: Automatikus kamera életciklus kezelés (start/stop)
    - onObjectsDetected: Callback amikor objektumot talál
    - A változók a kamera komponenseket tárolják (preview, analyzer, stb.)

!!! info "Objektum detektor konfigurálása"
    - STREAM_MODE: Valós idejű videó feldolgozás (vs. SINGLE_IMAGE_MODE)
    - enableClassification: Nem csak detektál, hanem kategorizál is
    - enableMultipleObjects: Egyszerre több objektumot is felismer

!!! info "Kamera inicializálás folyamata"
    1. CameraProvider: Aszinkron módon szerzi be a kamera szolgáltatást
    2. Preview: Létrehoz egy előnézetet és összeköti a UI elemmel
    3. ImageAnalyzer: Beállítja a képfeldolgozást (csak a legfrissebb képet tartja meg)
    4. bindToLifecycle: Összeköti a kamerát az Activity/Fragment életciklusával

!!! info "Képfeldolgozó logika"
    1. ImageProxy: Kamera képkocka wrapper, fontos a close() hívása
    2. InputImage: ML Kit formátumra konvertálás a rotációval együtt
    3. process: Aszinkron objektum detektálás
    4. Callbacks: Sikeres/sikertelen feldolgozás kezelése
    5. imageProxy.close(): Felszabadítja a memóriát

!!! info "ML Kit beépített kategóriák"
    Az ML Kit 5 fő kategóriába sorolja az objektumokat angol nyelven. Ez a függvény magyarra fordítja őket a jobb felhasználói élmény érdekében.

!!! info "Detektált objektum reprezentációja"
    - label: Az objektum kategóriája (pl. "Étel")
    - confidence: Megbízhatósági érték 0-1 között (0.8 = 80% biztos)
    - boundingBox: Az objektum pozíciója és mérete a képen (x, y, szélesség, magasság)

## Compose UI komponensek létrehozása

### CameraPreview Composable

Hozz létre egy új fájlt `CameraScreen.kt` néven:

```kotlin
package hu.bme.aut.android.objectdetectionlab

import android.Manifest
import android.annotation.SuppressLint
import android.graphics.Rect
import androidx.camera.view.PreviewView
import androidx.compose.foundation.Canvas
import androidx.compose.foundation.background
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.CameraAlt
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.draw.clip
import androidx.compose.ui.geometry.Offset
import androidx.compose.ui.geometry.Size
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.graphics.drawscope.Stroke
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.platform.LocalLifecycleOwner
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp
import androidx.compose.ui.viewinterop.AndroidView
import com.google.accompanist.permissions.ExperimentalPermissionsApi
import com.google.accompanist.permissions.isGranted
import com.google.accompanist.permissions.rememberPermissionState

@OptIn(ExperimentalPermissionsApi::class)
@Composable
fun CameraScreen() {
    // Kamera engedély kezelése
    val cameraPermissionState = rememberPermissionState(Manifest.permission.CAMERA)

    // Detektált objektumok állapota
    var detectedObjects by remember { mutableStateOf<List<DetectedObject>>(emptyList()) }

    // Kontextus és életciklus
    val context = LocalContext.current
    val lifecycleOwner = LocalLifecycleOwner.current

    // Kamera manager
    val cameraManager = remember {
        CameraManager(
            context = context,
            lifecycleOwner = lifecycleOwner,
            onObjectsDetected = { objects ->
                detectedObjects = objects
            }
        )
    }

    // Cleanup
    DisposableEffect(Unit) {
        onDispose {
            cameraManager.shutdown()
        }
    }

    Box(modifier = Modifier.fillMaxSize()) {
        when {
            cameraPermissionState.status.isGranted -> {
                // Kamera előnézet
                CameraPreview(
                    cameraManager = cameraManager,
                    modifier = Modifier.fillMaxSize()
                )

                // Objektum dobozok rajzolása
                ObjectBoundingBoxes(
                    objects = detectedObjects,
                    modifier = Modifier.fillMaxSize()
                )

                // Detektált objektumok listája
                DetectionResults(
                    objects = detectedObjects,
                    modifier = Modifier.align(Alignment.BottomCenter)
                )
            }
            else -> {
                // Engedély kérése
                PermissionRequest(
                    onRequestPermission = { cameraPermissionState.launchPermissionRequest() }
                )
            }
        }
    }
}

@Composable
fun CameraPreview(
    cameraManager: CameraManager,
    modifier: Modifier = Modifier
) {
    AndroidView(
        factory = { context ->
            PreviewView(context).also { previewView ->
                cameraManager.startCamera(previewView)
            }
        },
        modifier = modifier
    )
}

@SuppressLint("UnusedBoxWithConstraintsScope")
@Composable
fun ObjectBoundingBoxes(
    objects: List<DetectedObject>,
    modifier: Modifier = Modifier
) {
    // BoxWithConstraints használatával megkapjuk a Canvas tényleges méretét
    BoxWithConstraints(modifier = modifier) {
        val screenWidth = constraints.maxWidth.toFloat()
        val screenHeight = constraints.maxHeight.toFloat()

        Canvas(modifier = Modifier.fillMaxSize()) {
            objects.forEach { obj ->
                // Koordináta transzformáció
                val scaledBox = obj.getScaledBoundingBox(
                    screenWidth = screenWidth,
                    screenHeight = screenHeight
                )

                // Doboz rajzolása
                drawRect(
                    color = Color.Green,
                    topLeft = Offset(
                        scaledBox.left,
                        scaledBox.top
                    ),
                    size = Size(
                        scaledBox.width(),
                        scaledBox.height()
                    ),
                    style = Stroke(width = 3.dp.toPx())
                )
            }
        }
    }
}

@Composable
fun DetectionResults(
    objects: List<DetectedObject>,
    modifier: Modifier = Modifier
) {
    Card(
        modifier = modifier
            .fillMaxWidth()
            .padding(16.dp),
        shape = RoundedCornerShape(16.dp),
        colors = CardDefaults.cardColors(
            containerColor = Color.Black.copy(alpha = 0.7f)
        )
    ) {
        Column(
            modifier = Modifier.padding(16.dp)
        ) {
            Text(
                text = "Detektált objektumok",
                color = Color.White,
                fontSize = 18.sp,
                fontWeight = FontWeight.Bold,
                modifier = Modifier.padding(bottom = 8.dp)
            )

            if (objects.isEmpty()) {
                Text(
                    text = "Nincs detektált objektum",
                    color = Color.Gray,
                    fontSize = 14.sp
                )
            } else {
                objects.take(5).forEach { obj ->
                    ObjectItem(obj)
                }
            }
        }
    }
}

@Composable
fun ObjectItem(obj: DetectedObject) {
    Row(
        modifier = Modifier
            .fillMaxWidth()
            .padding(vertical = 4.dp),
        horizontalArrangement = Arrangement.SpaceBetween
    ) {
        Text(
            text = obj.label,
            color = Color.White,
            fontSize = 14.sp
        )
        Text(
            text = "${(obj.confidence * 100).toInt()}%",
            color = Color.Green,
            fontSize = 14.sp,
            fontWeight = FontWeight.Bold
        )
    }
}

@Composable
fun PermissionRequest(
    onRequestPermission: () -> Unit
) {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .background(MaterialTheme.colorScheme.background)
            .padding(24.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Icon(
            imageVector = Icons.Filled.CameraAlt,
            modifier = Modifier.size(100.dp),
            tint = MaterialTheme.colorScheme.primary,
            contentDescription = "Camera"
        )

        Spacer(modifier = Modifier.height(24.dp))

        Text(
            text = "Kamera engedély szükséges",
            style = MaterialTheme.typography.headlineMedium,
            color = MaterialTheme.colorScheme.onBackground
        )

        Spacer(modifier = Modifier.height(16.dp))

        Text(
            text = "Az objektum detektáláshoz szükségünk van a kamera használatára.",
            style = MaterialTheme.typography.bodyMedium,
            color = MaterialTheme.colorScheme.onBackground.copy(alpha = 0.7f),
            modifier = Modifier.padding(horizontal = 32.dp)
        )

        Spacer(modifier = Modifier.height(32.dp))

        Button(
            onClick = onRequestPermission,
            modifier = Modifier
                .fillMaxWidth()
                .padding(horizontal = 48.dp)
        ) {
            Text("Engedély megadása")
        }
    }
}
```
!!! info "Fő képernyő komponens"
    - rememberPermissionState: Engedély állapot követése
    - remember + mutableStateOf: Compose state management a detektált objektumokhoz
    - LocalContext/LocalLifecycleOwner: Hozzáférés az Android kontextushoz
    - DisposableEffect: Cleanup amikor a komponens eltűnik

!!! info "Kamera előnézet Compose-ban"
    AndroidView: Híd a hagyományos Android View rendszer és Compose között. A PreviewView egy speciális CameraX View, amit itt integrálunk a Compose UI-ba.

!!! info "Objektum keretek rajzolása"
    Canvas: Compose rajzfelület, ahol zöld kereteket rajzolunk a detektált objektumok köré. A Stroke() csak körvonalat rajzol, nem töltött téglalapot.

!!! info "Eredmények kártya"
    - Card: Material Design komponens félig átlátszó fekete háttérrel
    - take(5): Maximum 5 objektumot jelenítünk meg a teljesítmény érdekében
    - Column: Vertikális elrendezés a tartalom számára

!!! info "Egyedi objektum megjelenítése"
    Row: Horizontális elrendezés a címke és százalék megjelenítésére. A SpaceBetween a két elemet a sor két végére helyezi.

!!! info "Permission UI"
    Középre igazított layout kamera ikonnal és gombbal. A MaterialTheme használata biztosítja a konzisztens megjelenést light/dark mode-ban.

## MainActivity módosítása

### MainActivity.kt

Módosítsd a `MainActivity.kt` fájlt:

```kotlin
package hu.bme.aut.android.objectdetectionlab

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Surface
import androidx.compose.ui.Modifier
import hu.bme.aut.android.objectdetectionlab.ui.theme.ObjectDetectionLabTheme

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            ObjectDetectionLabTheme {
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    CameraScreen()
                }
            }
        }
    }
}
```
!!! info "Alkalmazás belépési pont"
    - setContent: Compose UI beállítása Activity-ben
    - Theme: Alkalmazás téma wrapper
    - Surface: Material Design alap konténer
    - CameraScreen: A fő képernyő komponens

## Alkalmazás tesztelése

**Figyelem!** Az ML Kit alapértelmezett Object Detection API-ja csak 5 általános kategóriát támogat (Fashion good, Food, Home good, Place, Plant), és nem ad részletes osztályozást, ezért a legtöbb objektum ismeretlenként jelenhet meg a képernyőn.

### Futtatás fizikai eszközön

1. Engedélyezd a fejlesztői módot az Android eszközön:
   - Beállítások → A telefonról → Többször koppints a Build számra
2. Engedélyezd az USB hibakeresést:
   - Beállítások → Fejlesztői beállítások → USB hibakeresés
3. Csatlakoztasd az eszközt USB kábellel
4. Válaszd ki az eszközt az Android Studio-ban
5. Kattints a Run gombra (zöld háromszög)

### Futtatás emulátorban

1. Indítsd el az emulátort
2. Válaszd ki az emulátort az Android Studio-ban
3. Kattints a Run gombra

**Megjegyzés**: Az emulátor virtuális kamerát használ, ami korlátozottabb, mint a fizikai eszköz kamerája.

### Az alkalmazás használata

1. Első indításkor megjelenik az engedélykérő képernyő
2. Kattints az "Engedély megadása" gombra
3. A rendszer kéri a kamera engedélyt - fogadd el
4. Irányítsd a kamerát különböző objektumokra
5. Az alkalmazás valós időben detektálja és címkézi az objektumokat
6. A képernyő alján megjelennek a detektált objektumok és azok megbízhatósági értékei

!!! example "3. feladat beadandó (2 pont)"
    * Kommitold a változtatásokat.
    * Készíts egy képernyőképet a futó alkalmazásról *legalább egy detektált objektummal* (emulátoron vagy saját készüléken), és mentsd el a repository gyökerébe **`f3.png`** néven.

## Gyakori problémák és megoldások

### Build hibák

**Probléma**: "Unresolved reference" hibák  
**Megoldás**: Ellenőrizd, hogy minden függőség helyesen van-e hozzáadva és szinkronizáld a projektet

**Probléma**: Version conflict  
**Megoldás**: Frissítsd a függőségeket a legújabb kompatibilis verziókra

### Futásidejű hibák

**Probléma**: Az alkalmazás összeomlik indításkor  
**Megoldás**: Ellenőrizd az engedélyeket az AndroidManifest.xml fájlban

**Probléma**: Nem jelennek meg az objektumok  
**Megoldás**: 
- Ellenőrizd, hogy megfelelő fényviszonyok vannak-e
- Próbálj nagyobb, tisztábban látható objektumokat detektálni

## Opcionális feladat (megajánlott jegyért):

Módosítsd az alkalmazást, hogy egy saját, LiteRT alapú modellt használjon, amely pontosabb objektum detektálást tesz lehetővé!

### Egyéni modellek használata - Példa

Az ML Kit támogatja egyéni TensorFlow Lite modellek használatát:

```kotlin
val localModel = LocalModel.Builder()
    .setAssetFilePath("model.tflite")
    .build()

val customObjectDetectorOptions = CustomObjectDetectorOptions.Builder(localModel)
    .setDetectorMode(CustomObjectDetectorOptions.STREAM_MODE)
    .enableClassification()
    .setMaxPerObjectLabelCount(3)
    .build()
```

!!! example "Opcionális feladat beadandó (10 pont)"
    * Kommitold a változtatásokat.
    * Készíts egy képernyőképet az elkészült alkalmazás felületéről, ahol látszik, hogy mi a megvalósított funkció és mentsd el **`fo.png`** néven a repository gyökerébe.
