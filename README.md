# Neuralne mreže – Projektni zadatak

Projektni zadatak iz predmeta **Neuralne mreže**.

**Autori:** Jovana Čeganjac (2022/0423), Milica Eva Stojanović (2022/0361)

📄 Kompletan izveštaj: [Neuralne_mreze_izvestaj.pdf](Neuralne_mreze_izvestaj.pdf)

Projekat obuhvata tri zadatka: klasifikaciju tabelarnih podataka potpuno povezanom mrežom, klasifikaciju slika konvolucionom mrežom i projektovanje fuzzy regulatora.

---

## 1. Potpuno povezana neuralna mreža (MLP) – klasifikacija pirinča

Klasifikacija tri sorte pirinča (**Cammeo**, **Osmancik**, **Kecimen**) na osnovu morfoloških osobina zrna (površina, obim, ose elipse, ekscentricitet, površina konveksnog omotača, extent) iz skupa `Rice.csv`.

- **Pretprocesiranje:** `LabelEncoder` + one-hot kodiranje, standardizacija (`StandardScaler`), stratifikovana podela 64/16/20 (trening/validacija/test)
- **Neizbalansirane klase:** težine klasa (`compute_class_weight("balanced")`)
- **Arhitektura:** `Dense(32, ReLU) → Dense(16, ReLU) → Dense(3, Softmax)`
- **Obuka:** Adam (lr = 0.0004), `categorical_crossentropy`, batch 32, Early Stopping (patience = 4)
- **Rezultat:** tačnost **93%** na test skupu; Kecimen prepoznat bez greške, greške uglavnom između morfološki sličnih sorti Cammeo i Osmancik

## 2. Konvoluciona neuralna mreža (CNN) – Rock / Paper / Scissors

Klasifikacija slika gestova ruke (kamen, papir, makaze) iz Kaggle skupa `drgfreeman/rockpaperscissors`, slike 64×64, uravnotežene klase.

- **Pretprocesiranje:** podela 80/20 (seed = 123), normalizacija `Rescaling(1/255)`
- **Augmentacija:** `RandomFlip`, `RandomRotation(0.1)`, `RandomZoom(0.1)`
- **Arhitektura:** tri bloka `Conv2D (16 → 32 → 64, ReLU) + MaxPooling2D`, zatim `Flatten → Dense(128) → Softmax`
- **Regularizacija:** Dropout(0.3), L2 (0.01)
- **Obuka:** Adam (lr = 0.0001), `SparseCategoricalCrossentropy`, do 50 epoha uz Early Stopping (patience = 5)
- **Rezultat:** tačnost **97%** na test skupu; najviše grešaka između klasa *paper* i *scissors*

## 3. Fuzzy regulator

Projektovanje Mamdani fuzzy regulatora za neminimalno fazni objekat sa transportnim kašnjenjem od 4 s:

$$G(s) = \frac{-0.2s + 0.01}{(s + 0.1)^2} e^{-4s}$$

- **Ulazi:** greška `e ∈ [−2, 2]` i promena greške `de ∈ [−1, 1]`; **izlaz:** promena upravljanja `du ∈ [−0.2, 0.2]`
- Trapezne i Gausove funkcije pripadnosti, 7 eksperimentalno podešenih pravila
- Inkrementalno upravljanje: `Gain(0.2) → Integrator → Saturation [−1.5, 1.5]`
- Simulacija u **MATLAB/Simulink**-u, uključujući test otpornosti na spoljašnje poremećaje (step smetnje na upravljačkom signalu)
- **Rezultat:** stabilno praćenje reference, zanemarljiva greška u stacionarnom stanju i brzo potiskivanje poremećaja

---

## Tehnologije

Python · TensorFlow / Keras · scikit-learn · MATLAB · Simulink · Fuzzy Logic Toolbox
