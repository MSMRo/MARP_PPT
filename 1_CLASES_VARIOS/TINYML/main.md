---
marp: true
theme: beam
paginate: true
size: 16:9
header: "Reconocimiento de Patrones"
footer: "sistemas embebidos e IA"
---
<!-- _class: title -->
# sistemas embebidos e IA

**Mag. Ing. Moises Meza Rodriguez**
**moises.meza@upch.pe**

---

# What is TinyML?
<br>
TinyML is the deployment of machine learning models on resource-constrained embedded systems.

Constraints:

- Low RAM
- Low Flash
- Low Power
- Real-time inference

Applications:

- EMG classification
- ECG anomaly detection
- Gesture recognition
- Smart wearables

---

# Why TinyML?

Traditional ML:

```text
Sensor → Cloud → Inference → Response
```

TinyML:

```text
Sensor → Embedded Inference → Response
```

Advantages:

✅ Lower latency  
✅ Better privacy  
✅ Lower energy  
✅ Offline operation  

---

# What is ArduTFLite?

ArduTFLite is a lightweight wrapper over TensorFlow Lite for Microcontrollers.

It simplifies:

- Model loading
- Input writing
- Inference execution
- Output reading

GitHub:

https://github.com/spaziochirale/ArduTFLite

---

# ArduTFLite Architecture

![bg right:40% fit](https://raw.githubusercontent.com/spaziochirale/ArduTFLite/master/extras/logo.png)

```text
ArduTFLite
    ↓
TensorFlow Lite Micro
    ↓
MicroInterpreter
    ↓
Tensor Arena
    ↓
MCU
```

---

# Deployment Pipeline

![bg right:45% contain](https://www.tensorflow.org/images/tf_logo_social.png)

```text
Train model
    ↓
Convert to .tflite
    ↓
Convert to .h
    ↓
Deploy on MCU
    ↓
Run inference
```

This is the standard TinyML pipeline.

---

# Hardware Target

## Raspberry Pi Pico (RP2040)

![bg right:45% contain](https://magazine.raspberrypi.com/assets/_1200x630_crop_center-center_82_none/pico-hero.jpg)

Specs:

- ARM Cortex-M0+
- 264 KB SRAM
- 2 MB Flash
- 133 MHz

Ideal for TinyML.

Source:
https://magazine.raspberrypi.com/articles/introducing-raspberry-pi-pico

---

# Simulation Environment

## Wokwi

![bg right:45% contain](https://docs.wokwi.com/img/parts/wokwi-pi-pico.svg)

Wokwi allows:

- Hardware-free testing
- Sensor simulation
- Serial debugging

Source:
https://docs.wokwi.com/parts/wokwi-pi-pico

---

# Step 1: Train the Model

```python
import tensorflow as tf

model = tf.keras.Sequential([
    tf.keras.layers.Dense(8, activation='relu', input_shape=(2,)),
    tf.keras.layers.Dense(8, activation='relu'),
    tf.keras.layers.Dense(3, activation='softmax')
])

model.compile(
    optimizer='adam',
    loss='sparse_categorical_crossentropy'
)
```

---

# Model Architecture

```text
Input(2)
   ↓
Dense(8) + ReLU
   ↓
Dense(8) + ReLU
   ↓
Dense(3) + Softmax
```

Output:

```text
[ p0, p1, p2 ]
```

---

# Step 2: Convert to TFLite

```python
converter = tf.lite.TFLiteConverter.from_keras_model(model)
tflite_model = converter.convert()

with open("model.tflite", "wb") as f:
    f.write(tflite_model)
```

Output:

```text
model.tflite
```

---

# Step 3: Convert to Header File

Use:

```bash
xxd -i model.tflite > model.h
```

Result:

```cpp
const unsigned char model_tflite[] = {...};
const unsigned int model_tflite_len = ...;
```

This stores the model in Flash.

---

# Install ArduTFLite

Arduino IDE:

```text
Library Manager → ArduTFLite
```

Repo:

https://github.com/spaziochirale/ArduTFLite

---

# Basic Includes

```cpp
#include <Arduino.h>
#include <ArduTFLite.h>
#include "model.h"
```

These are the only required includes.

---

# Tensor Arena

```cpp
constexpr int kTensorArenaSize = 25000;
alignas(16) uint8_t tensor_arena[kTensorArenaSize];
```

Tensor arena stores:

- Activations
- Intermediate tensors
- Scratch buffers

Important:

```text
Arena ≠ Model weights
```

---

# Model Initialization

```cpp
if (!modelInit(model_tflite, tensor_arena, kTensorArenaSize)) {
    Serial.println("Model initialization failed!");
}
```

Internally:

```text
GetModel()
AllocateTensors()
```

---

# Setting Inputs

```cpp
modelSetInput(200.0, 0);
modelSetInput(305.0, 1);
```

General form:

```cpp
modelSetInput(value, index);
```

---

# Run Inference

```cpp
modelRunInference();
```

Equivalent to:

```text
Invoke()
```

This performs forward propagation.

---

# Read Outputs

```cpp
float y0 = modelGetOutput(0);
float y1 = modelGetOutput(1);
float y2 = modelGetOutput(2);
```

Output:

```text
Softmax probabilities
```

---

# Full Code

```cpp
#include <Arduino.h>
#include <ArduTFLite.h>
#include "model.h"

constexpr int kTensorArenaSize = 25000;
alignas(16) uint8_t tensor_arena[kTensorArenaSize];

void setup() {
    Serial.begin(115200);

    if (!modelInit(model_tflite, tensor_arena, kTensorArenaSize)) {
        Serial.println("Model init failed!");
        while(true);
    }
}
```

---

# Loop Code

```cpp
void loop() {
    modelSetInput(200.0, 0);
    modelSetInput(305.0, 1);

    if (!modelRunInference()) {
        Serial.println("Inference failed!");
        return;
    }

    float y0 = modelGetOutput(0);
    float y1 = modelGetOutput(1);
    float y2 = modelGetOutput(2);

    Serial.println(y0);
    Serial.println(y1);
    Serial.println(y2);

    delay(1000);
}
```

---

# Multi-class Prediction

Argmax:

```cpp
int pred = 0;
float maxVal = y0;

if (y1 > maxVal) {
    maxVal = y1;
    pred = 1;
}

if (y2 > maxVal) {
    maxVal = y2;
    pred = 2;
}
```

Mathematically:

\[
\hat{y} = \arg\max_i p(y=i|x)
\]

---

# Measuring Latency

```cpp
unsigned long t0 = micros();

modelRunInference();

unsigned long t1 = micros();

Serial.println(t1 - t0);
```

Constraint:

\[
T_{infer} < T_{sample}
\]

---

# Real Sensor Example

```cpp
float x1 = analogRead(26);
float x2 = analogRead(27);

x1 /= 1023.0;
x2 /= 1023.0;
```

Important:

Use the same preprocessing as training.

---

# Common Errors

## Model initialization failed

Cause:

- Arena too small

Solution:

```cpp
constexpr int kTensorArenaSize = 40000;
```

---

# Common Errors

## Wrong predictions

Cause:

- Missing normalization
- Wrong feature order

Solution:

Replicate training preprocessing.

---

# Common Errors

## Inference failed

Cause:

Unsupported operators.

Use Netron to inspect:

https://netron.app

---

# Educational Advantages of ArduTFLite

✅ Minimal boilerplate  
✅ Faster learning curve  
✅ Works with Wokwi  
✅ Compatible with Pico  
✅ Ideal for teaching TinyML  

---

# Recommended Workflow

```text
TensorFlow
   ↓
TFLiteConverter
   ↓
xxd -i
   ↓
ArduTFLite
   ↓
Wokwi
   ↓
Hardware deployment
```

This is the fastest path from AI to embedded inference.

---

# Resources

ArduTFLite:
https://github.com/spaziochirale/ArduTFLite

TensorFlow Lite:
https://www.tensorflow.org/lite

Wokwi:
https://wokwi.com/

Netron:
https://netron.app

---

# Thank you
## Questions?