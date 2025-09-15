# Adatvizualizáció labor

## Bevezetés

A labor során a Pythonban elérhető legfontosabb adatvizualizációs könyvtárakkal ismerkedhetünk meg.


## Előkészület

A feladatok megoldása során ne felejtsd el követni a feladatbeadás folyamatát [Github](../../tudnivalok/github/GitHub.md).

### Git repository létrehozása és letöltése

1. Moodle-ben keresd meg a laborhoz tartozó meghívó URL-jét és annak segítségével hozd létre a saját repository-dat.
2. Várd meg, míg elkészül a repository, majd checkout-old ki.
3. Hozz létre egy új ágat `megoldas` néven, és ezen az ágon dolgozz.
4. A neptun.txt fájlba írd bele a Neptun kódodat. A fájlban semmi más ne szerepeljen, csak egyetlen sorban a Neptun kód 6 karaktere.

## Feladat: Feltáró adatelemzés és adatvizualizáció

1. Válassz egy publikusan elérhető adathalmazt (pl. Kaggle-ről), amin a vizualizációkat fogod elkészíteni.
2. Végezz feltáró adatelemzést (EDA) a következők szerint.
    - Készíts 4-4-4 vizualizációt a **matplotlib**, **seaborn** és **plotly** könyvtárakkal
    - Minden vizualizáció legyen egyedi
    - Minden könyvtárnál legyen legalább 2 olyan típusú vizualizáció, amely más könyvtáraknál nem szerepel
    - Minden vizualizáció legyen igényes, tartalmazzon címet, tengelyfeliratokat, stb., illetve hordozzon tényleges információt.
    - Minden vizualizáció után írd le egy Markdown cellába, hogy milyen következtetést lehet levonni az adott grafikonból.


!!! example "Beadandó (40 pont)"
    * Töltsd fel a repoba a vizualizációkat tartalmazó Jupyter Notebook fájlt. A cellakimenetek, tehát a vizualizációk is szerepeljenek benne!


!!! example "Opcionális feladat beadandó (10 pont)"
    * Készíts legalább 3 térképet a Folium python könyvtárral, tetszőleges adathalmazon. Az ábrák legyenek igényesek, tartalmazzanak minél több információt.