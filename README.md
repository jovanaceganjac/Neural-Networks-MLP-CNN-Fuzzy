# Neural Networks – Course Project

Course project for the **Neural Networks** course.

**Authors:** Jovana Čeganjac (2022/0423), Milica Eva Stojanović (2022/0361)

Full report (in Serbian): [Neural_Networks_Report.pdf](Neural_Networks_Report.pdf)

The project covers three tasks: tabular data classification with a fully connected network, image classification with a convolutional network, and the design of a fuzzy controller.

---

## 1. Fully Connected Neural Network (MLP) – Rice Classification

Classification of three rice varieties (**Cammeo**, **Osmancik**, **Kecimen**) based on morphological features of each grain (area, perimeter, major/minor axis length, eccentricity, convex area, extent) from the `Rice.csv` dataset.

- **Preprocessing:** `LabelEncoder` + one-hot encoding, standardization (`StandardScaler`), stratified 64/16/20 split (train/validation/test)
- **Class imbalance:** class weights (`compute_class_weight("balanced")`)
- **Architecture:** `Dense(32, ReLU) → Dense(16, ReLU) → Dense(3, Softmax)`
- **Training:** Adam (lr = 0.0004), `categorical_crossentropy`, batch size 32, Early Stopping (patience = 4)
- **Result:** **93%** test accuracy; Kecimen classified without errors, with most mistakes between the morphologically similar Cammeo and Osmancik

## 2. Convolutional Neural Network (CNN) – Rock / Paper / Scissors

Classification of hand gesture images (rock, paper, scissors) from the Kaggle dataset `drgfreeman/rockpaperscissors`, 64×64 images, balanced classes.

- **Preprocessing:** 80/20 split (seed = 123), normalization with `Rescaling(1/255)`
- **Data augmentation:** `RandomFlip`, `RandomRotation(0.1)`, `RandomZoom(0.1)`
- **Architecture:** three `Conv2D (16 → 32 → 64, ReLU) + MaxPooling2D` blocks, followed by `Flatten → Dense(128) → Softmax`
- **Regularization:** Dropout(0.3), L2 (0.01)
- **Training:** Adam (lr = 0.0001), `SparseCategoricalCrossentropy`, up to 50 epochs with Early Stopping (patience = 5)
- **Result:** **97%** test accuracy; most errors occur between *paper* and *scissors*

## 3. Fuzzy Controller

Design of a Mamdani fuzzy controller for a non-minimum phase plant with a 4 s transport delay:

$$G(s) = \frac{-0.2s + 0.01}{(s + 0.1)^2} e^{-4s}$$

- **Inputs:** error `e ∈ [−2, 2]` and change of error `de ∈ [−1, 1]`; **output:** change of control signal `du ∈ [−0.2, 0.2]`
- Trapezoidal and Gaussian membership functions, 7 experimentally tuned rules
- Incremental control: `Gain(0.2) → Integrator → Saturation [−1.5, 1.5]`
- Simulated in **MATLAB/Simulink**, including a robustness test against external disturbances (step disturbances on the control signal)
- **Result:** stable reference tracking, negligible steady-state error and fast disturbance rejection

---

## Technologies

Python · TensorFlow / Keras · scikit-learn · MATLAB · Simulink · Fuzzy Logic Toolbox
