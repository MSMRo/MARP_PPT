---
marp: true
theme: default
paginate: true
header: " "
footer: "Facultad de Ciencias e Ingeniería"
style: |
  section {    font-family: 'Helvetica Neue', Arial, sans-serif;
    background-color: #ffffff;
    padding: 40px;
    justify-content: flex-start;
  }
  h1 { color: #ffffff; font-size: 26px; margin: 0; }
  h2 {
    color: #ffffff;
    background-color: #2b32b2;
    padding: 10px 25px;
    border-radius: 0px 0px 18px 0px;
    font-size: 22px;
    margin-top: -40px;
    margin-left: -40px;
    margin-right: -40px;
    margin-bottom: 25px;
    width: 65%;
    box-shadow: 2px 2px 5px rgba(0,0,0,0.15);
  }
  .title-slide {
    text-align: center;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100%;
  }
  .title-box {
    background-color: #2b32b2;
    color: white;
    padding: 20px 35px;
    border-radius: 16px;
    width: 85%;
    margin: 20px auto;
    box-shadow: 0px 4px 10px rgba(0,0,0,0.2);
  }
  .subtitle { font-weight: bold; font-size: 18px; color: #111111; margin-top: 10px; }
  .author { font-size: 16px; color: #444444; margin-top: 15px; }
  .columns {
    display: grid;
    grid-template-columns: 1.2fr 0.8fr;
    gap: 20px;
    align-items: center;
  }
  .beamer-block {
    background-color: #f0f2f9;
    border-left: 6px solid #2b32b2;
    padding: 12px 18px;
    border-radius: 4px;
    margin-bottom: 15px;
    font-size: 17px;
  }
  ul { font-size: 18px; line-height: 1.5; color: #222222; }
  li { margin-bottom: 10px; }
  img { border-radius: 8px; max-width: 100%; object-fit: cover; }
  footer {
    background-color: #2b32b2;
    color: #ffffff;
    font-size: 12px;
    padding: 4px 20px;
    position: absolute;
    bottom: 0; left: 0; right: 0;
  }
---

<div class="title-slide"><div class="title-box">
  <h1>Protocolos de Hardware y Programación de Sensores</h1>
  <p style="margin: 5px 0 0 0; font-size: 18px; opacity: 0.9;">Sesión 3: Unidad 2 - Ecosistema de Arduino</p></div><div class="subtitle">Curso: Desarrollo de Hardware para Telesalud</div><div class="author" style="font-weight: bold; font-size: 18px; color: #111111; margin-top: 15px;">
  Mag. Ing. CIP Moises Stevend Meza Rodriguez</div><div class="author" style="font-size: 15px; color: #555555; margin-top: 5px;">
  Facultad de Ciencias e Ingeniería</div></div>

---

![bg height:370px](./imgs/Screenshot%202026-10-09%20202606.png)

---

## Introducción a Protocolos de Hardware

<div class="columns">
<div>

<div class="beamer-block">
  <strong>¿Qué son los Protocolos Físicos?</strong>
</div>

* Establecen las normas de transporte para intercambiar datos de bioseñales de forma rápida y fidedigna entre sensores y el microcontrolador central.
* **Abstracción vs. Silicio:** Entornos como Arduino IDE ocultan la complejidad de los registros mediante su capa HAL, pero no alteran las limitaciones eléctricas ni físicas del hardware.

</div>
<div>

<img src="https://hackster.imgix.net/uploads/attachments/1473969/_Un47llkZ61.blob?auto=compress%2Cformat&w=400&h=300&fit=min" alt="Microcontrolador y Componentes" />

</div>
</div>

---

## Ecosistema de Comunicación

<div class="columns">
<div>

<div class="beamer-block">
  <strong>Clasificación Principal</strong>
</div>

* **UART:** Transmisión asíncrona, bidireccional punto a punto.
* **I2C (TWI):** Protocolo síncrono para topologías multiesclavo compacto.
* **SPI:** Estándar síncrono dedicado a operaciones de muy alta velocidad.
* **CAN (TWAI):** Arquitectura automotriz inmune a ruidos extremos.

</div>
<div>

<img src="./imgs/Screenshot 2026-10-09 203140.png" alt="Circuito Electrónico" />

</div>
</div>

---

## Protocolo UART (Comunicación Asíncrona)

<div class="columns">
<div>

<div class="beamer-block">
  <strong>Universal Asynchronous Receiver-Transmitter</strong>
</div>

* **Física:** Opera mediante 2 hilos (TX y RX) y 1 referencia obligatoria (GND).
* **Temporización:** Carece de línea de reloj compartida. Los dispositivos preacuerdan la velocidad o *Baud Rate* (ej. 115200 bps).
* **Aplicación:** GPS, Modems GPRS/LTE y enlaces Bluetooth BLE hacia los teléfonos del paciente.

</div>
<div>

<img src="./imgs/gps.png" alt="Hardware UART" />

</div>
</div>

---

## Protocolo I2C (Inter-Integrated Circuit)

<div class="columns">
<div>

<div class="beamer-block">
  <strong>Bus Síncrono de Drenador Abierto</strong>
</div>

* **Líneas físicas:** SDA (datos bidireccional) y SCL (pulso de reloj del maestro).
* **Restricción Eléctrica:** Se recomienda agregar externamente resistencias *Pull-up* (típicamente de 4.7 kΩ).
* **Aplicación:** Direccionamiento hexadecimal de mútiples sensores biomédicos acoplados en el mismo bus de 2 vías.

</div>
<div>

<img src="./imgs/I2C-diagram.jpg" alt="Bus I2C" />

</div>
</div>

---

![bg height:600px](./imgs/Screenshot%202026-10-09%20204144.png)

---

![bg height:700px](./imgs/promo-image.1637010901.png)

---

## Protocolo SPI (Serial Peripheral Interface)

<div class="columns">
<div>

<div class="beamer-block">
  <strong>Bus Síncrono de Alta Velocidad</strong>
</div>

* **Full-Duplex:** Desplazamiento simultáneo (MOSI y MISO) impulsado por el reloj maestro (SCK) superando rangos de 10-50 MHz.
* **Selección de Esclavo:** Exige un hilo adicional discreto (CS/SS) por cada esclavo activo.
* **Aplicación:** Almacenamiento continuo (*datalogger*) en SD y gestión de pantallas gráficas (TFT) para monitores.

</div>
<div>

<img src="./imgs/spi.png" alt="Conexiones SPI" />

</div>
</div>

---

![bg fit height:680px](./imgs/Gemini_Generated_Image_ljjifxljjifxljji.jpeg)

---

<div align="center">

![height:600px](./imgs/mosi_miso.jpeg)
</div>

---

## Protocolo CAN Bus (TWAI)

<div class="columns">
<div>

<div class="beamer-block">
  <strong>Comunicaciones Críticas Aisladas</strong>
</div>

* **Diferencial:** Protege las tramas de alta prioridad de la interferencia electromagnética severa.
* **Transceptor Externo:** El procesador maneja las capas digitales, pero es crítico incorporar un circuito integrado transceptor para acoplar la señal a las líneas CAN físicas.

</div>
<div>

<img src="https://www.allaboutcircuits.com/uploads/articles/CAN.jpg" alt="Red CAN Bus" />

</div>
</div>

---

## Adaptación Eléctrica (Level Shifting)

<div class="columns">
<div>

<div class="beamer-block">
  <strong>Dominio 3.3V vs 5V</strong>
</div>

* **Riesgo:** Someter pines de placas avanzadas (3.3V) a señales directas de módulos antiguos (5V) destruye sus diodos de seguridad por sobretensión.
* **Translación I2C:** Exige chips bidireccionales basados en MOSFETs BSS138 por su característica de drenador abierto.
* **Translación SPI:** Demanda aceleradores activos de flanco o búferes rígidos debido a las velocidades extremas.

</div>
<div>

<img src="./imgs/level_shiffting.png" alt="Placa de prototipos" />

</div>
</div>

---

## Familia Clásica AVR (Arduino Uno)

<div class="columns">
<div>

<div class="beamer-block">
  <strong>ATmega328P a 5V</strong>
</div>

* **Deficiencia UART:** Cuenta con 1 hardware UART. Acoplar un sensor asíncrono en los pines (0,1) choca con el tráfico USB de carga o depuración.
* **Bit-Banging:** Conlleva al uso oneroso de la biblioteca emuladora `SoftwareSerial`.
* **Hardware Estático:** Pines I2C (A4/A5) y SPI (11-13) no pueden redistribuirse.

</div>
<div>

<img src="./imgs/ardu1.png" alt="Arduino Uno" />

</div>
</div>

---

![bg height:680px](https://www.electrogeekshop.com/wp-content/uploads/2018/12/7c9dfd832eed7972758545f30d9e6ac9.jpeg)

---

## Arduino Mega y Leonardo

<div class="columns">
<div>

<div class="beamer-block">
  <strong>Expansión y Virtualización USB</strong>
</div>

* **ATmega2560:** Incluye 4 transceptores UART por hardware (`Serial1` a `Serial3`) idóneos para redes locales ricas en sensores.
* **ATmega32U4:** Integra el protocolo USB (CDC). Su clase base `Serial` es un puerto host virtualizado, desvinculando la actividad física en su UART secundario.
* **Advertencia Leonardo:** Sus líneas SPI físicas convergen exclusivamente sobre el cabezal ICSP central.

</div>
<div>

<img src="./imgs/mega1.png" alt="Microcontroladores Mega" />

</div>
</div>

---

## Raspberry Pi Pico 2 (RP2350)

<div class="columns">
<div>

<div class="beamer-block">
  <strong>Flexibilidad Mediante Máquinas de Estado</strong>
</div>

* **Matriz Inteligente:** Los controladores UART y SPI internos admiten enrutamiento físico guiado por la multiplexación de la placa.
* **Bloques PIO (I/O Programable):** Co-procesadores en miniatura con los que el Arduino Core (Philhower) emula controladores secundarios fluidos usando acceso a memoria (DMA) puro.

</div>
<div>

<img src="https://www.elecrow.com/media/wysiwyg/products/2024/RPD08112P/Raspberry_pi_pico2_feature.jpg" alt="Raspberry Pi Pico" />

</div>
</div>

---

![bg height:710px](./imgs/pico_pinout.png)

---

## Sistemas ESP32 (Xtensa / RISC-V)

<div class="columns">
<div>

<div class="beamer-block">
  <strong>Plataforma IoT de Alto Desempeño</strong>
</div>

* **Matriz GPIO:** Desvincula geométricamente las señales. Un periférico puede derivarse instantáneamente hacia un pin de salida conveniente para enrutamiento de PCB.
* **Tolerancias de I2C:** Errores de silicio obligan a tener cautela extrema ante el *Clock Stretching* (esclavos lentos).
* **DMA para SPI:** Su motor requiere asignación explícita de buffers alojados estrictamente en DRAM.

</div>
<div>

<img src="./imgs/esp.jpeg" alt="ESP32 IoT" />

</div>
</div>

---

## Arduino Nano 33 BLE Sense en la Telesalud

<div class="columns">
<div>

<div class="beamer-block">
  <strong>Wearables y Hardware de Borde</strong>
</div>

* **Topología Integral:** Diseño optimizado que pre-suelda los sensores más importantes de la materia sobre una placa compacta, sin requerir adaptaciones externas.
* **Sensores de placa (I2C):** Consta de un termómetro/higrómetro HTS221, un sensor inercial 9DOF (IMU) y el chip APDS9960 para gestometría de color/proximidad.
* **Control API:** Se gestionan de manera abstracta interactuando indirectamente con `Wire.h`.

</div>
<div>

<img src="https://http2.mlstatic.com/D_NQ_NP_847896-MEC88313156132_072025-O.webp" alt="Wearable Sensors" />

</div>
</div>

---

![bg height:570](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQvyAc3LTkHcEkmMfOCP-7o_XyLm9Af1PV8xdJiC9NA15e4vnB34-_hmHc&s=10)

---

## Programación de Protocolos en Arduino IDE

<div class="columns">
<div>

<div class="beamer-block">
  <strong>Estructura Universal de Llamadas</strong>
</div>

* **Asíncrono:** `Serial.begin(115200)`. La adquisición emplea buffers y requiere validación mediante `Serial.available()`.
* **Esclavos de Red:** `Wire.begin()` configura I2C y `Wire.requestFrom()` demanda bloques de bytes a dispositivos específicos.
* **Alto Flujo:** `SPI.beginTransaction(SPISettings())` configura formato y sincronía de los paquetes MSB.

</div>
<div>

<img src="https://europe1.discourse-cdn.com/arduino/original/4X/5/2/7/527385c7d85eaef5f7216061d1ee83de449b930a.png" alt="Programación en C" />

</div>
</div>

---

## Metodología de diseño 

<div class="columns">
<div>

<div class="beamer-block">
  <strong>Adquisición, Procesamiento y Tramas</strong>
</div>

* Diagrama lógico
* Diagrama esquema
* Diagrama C4 (extenso)

</div>
<div>

<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTtxMcCHvnG4HtpnYeq4oB_fG-T4q76NSujZ8P8bhIgvKWToNBzzziWHmmb&s=100" alt="Laboratorio Universitario" />

</div>
</div>

---

Diagrama
lógico

![bg height:750px](https://www.researchgate.net/publication/393093461/figure/fig1/AS:11431281520865907@1751044586743/Architecture-of-the-Arduino-Based-strength-sensing-and-automation-system_W640.jpg)

---

Esquemático

![bg height:700px](./imgs/sch1.png)

---
Diagrama
de flujo

![bg height:650](https://aprendiendoarduino.wordpress.com/wp-content/uploads/2015/04/proyecto1-diagrama-flujo.png)

---

Diagrama
 de casos

![bg height:700](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSsyKytngOgkbc3ruu6wF-MNZCqjRzcguV61w7CYgFy3dWaXWIRVNFtD1VD&s=10)

---
## Actividad 1 (Básica): El I2C Scanner

<div class="columns">
<div>

<div class="beamer-block">
  <strong>Mapeo del Bus Multiesclavo</strong>
</div>

* **Propósito:** Inyectar rutinas de ping `Wire.beginTransmission()` hacia todos los registros 1–127 posibles en el Nano 33 BLE.
* **Resultado Esperado:** Detectar las firmas MAC/Hex decimales estáticas integradas en el silicio de cada sensor local y constatar el enrutamiento físico sano en I2C.

</div>
<div>

<img src="https://cdn-icons-png.flaticon.com/512/12313/12313460.png" alt="Hardware de Medición" />

</div>
</div>

---

## Actividad 2 (Intermedia): Acondicionamiento Analítico

<div class="columns">
<div>

<div class="beamer-block">
  <strong>Atenuación de Fluctuaciones Sensoriales</strong>
</div>

* **Extracción de Variables:** Solicitar datos del sensor nativo de temperatura/humedad (HTS221).
* **Filtro Promedio Móvil:** Operar un lazo `for` simple en C++ que acumule veinte ciclos, devolviendo el promedio lineal para suprimir el ruido blanco térmico antes de ser procesado como signo vital.

</div>
<div>

<img src="https://cdn-icons-png.flaticon.com/512/12313/12313460.png" alt="Señales Biomédicas" />

</div>
</div>

---

## Actividad 3 (Avanzada): Data Framing para Telesalud

<div class="columns">
<div>

<div class="beamer-block">
  <strong>Construcción de Tramas de Telemetría JSON</strong>
</div>

* **Interconexión Dinámica:** Fusionar los datos de temperatura y acelerometría de 9 ejes en un formato estructurado legible (`{"dev_id": "BED01", "bpm": 72}`).
* **Disparo Serial:** Ejecutar la transmisión optimizada sobre la capa física del UART para derivar el arreglo a la sesión posterior del sistema IoT con Node-RED.

</div>
<div>

<img src="https://cdn-icons-png.flaticon.com/512/12313/12313460.png" alt="Datos en Tiempo Real JSON" />

</div>
</div>