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
  ul { font-size: 17px; line-height: 1.5; color: #222222; }
  li { margin-bottom: 10px; }
  img { border-radius: 8px; max-width: 100%; object-fit: cover; }
  pre { font-size: 14px; background-color: #f6f8fa; padding: 12px; border-radius: 6px; border-left: 4px solid #2b32b2; }
  code { font-family: 'Courier New', monospace; }
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
  <h1>Análisis Exhaustivo de Protocolos de Hardware en Microcontroladores</h1>
  <p style="margin: 5px 0 0 0; font-size: 18px; opacity: 0.9;">Sesión 3: Unidad 2 - Ecosistema de Arduino</p></div><div class="subtitle">Desarrollo de Hardware para Telesalud</div><div class="author" style="font-weight: bold; font-size: 18px; color: #111111; margin-top: 15px;">
  Mag. Ing. CIP Moisés Stiglitz Rodríguez</div><div class="author" style="font-size: 15px; color: #555555; margin-top: 5px;">
  Facultad de Ciencias e Ingeniería</div></div>

---

## Introducción a la Abstracción de Hardware

<div class="columns">
<div>

<div class="beamer-block">
  <strong>Capa HAL (Hardware Abstraction Layer)</strong>
</div>

* El ecosistema Arduino introdujo clases unificadas (`Serial`, `Wire`, `SPI`) que ocultan la manipulación directa de registros a nivel de bit.
* **El Peligro de la Abstracción:** El software no elimina las barreras físicas. Ignorar la topología de ruteo, las capacidades de DMA o los voltajes lógicos (5V vs 3.3V) resulta en fallas catastróficas.
* Es imperativo comprender la arquitectura subyacente de cada familia (AVR, Xtensa, RP2040) para instrumentación médica.

</div>
<div>

<img src="https://images.unsplash.com/photo-1518932945647-7a3c969f414b?fm=jpg&w=500" alt="Microchip PCB" />

</div>
</div>

---

## Protocolo UART (Universal Asynchronous Receiver-Transmitter)

<div class="columns">
<div>

<div class="beamer-block">
  <strong>Comunicación Asíncrona Punto a Punto</strong>
</div>

* **Arquitectura Física:** Emplea 2 hilos cruzados (`TX` transmite, `RX` recibe) y una referencia común (`GND`).
* **Sincronización:** No existe señal de reloj. Ambos extremos deben pre-configurar la misma velocidad (Baud Rate, ej. 115200 bps).
* **Aplicación Clínica:** Enlaces de telemetría hacia módems GSM/LTE o módulos Bluetooth (HC-05, BLE) para aplicaciones de paciente.

</div>
<div>

<img src="https://images.unsplash.com/photo-1517077304055-6e89abbf09b0?fm=jpg&w=500" alt="Cables TX RX" />

</div>
</div>

---

## Sintaxis Arduino: Uso Genérico de UART

<div class="columns" style="grid-template-columns: 1.4fr 0.6fr;">
<div>

<div class="beamer-block">
  <strong>Inicialización y Manejo de Buffers (Clase Serial)</strong>
</div>

```cpp
void setup() {
  // Inicialización a 115200 baudios
  Serial.begin(115200);
  
  // Bloqueo de seguridad para arquitecturas nativas USB 
  // (ej. Arduino Leonardo, RP2040)
  while (!Serial) { ; }
}

void loop() {
  // Lectura no destructiva desde el buffer UART
  if (Serial.available() > 0) {
    char incomingByte = Serial.read();
    Serial.print("Byte recibido: ");
    Serial.println(incomingByte);
  }
}

```

---

## Protocolo I2C (Inter-Integrated Circuit)

* **Arquitectura:** Solo 2 hilos bidireccionales: `SDA` (Datos) y `SCL` (Reloj generado por el Maestro).
* **Restricción Física:** Requiere obligatoriamente resistencias *Pull-up* (típicamente 4.7 kΩ) hacia $V_{CC}$. Ningún nodo inyecta voltaje, solo "jalan" la línea a GND.
* **Direccionamiento:** Soporta decenas de esclavos usando direcciones Hexadecimales de 7 bits (ej. `0x3C`).

---

## Sintaxis Arduino: Configuración y Uso de I2C

```cpp
#include <Wire.h>

void setup() {
  Wire.begin();           // Únete al bus como Maestro
  Wire.setClock(400000);  // Fast Mode: 400 kHz
}

void loop() {
  // Solicitar 2 bytes al sensor en dirección 0x48
  Wire.requestFrom(0x48, 2); 
  
  if (Wire.available() >= 2) {
    uint8_t msb = Wire.read();
    uint8_t lsb = Wire.read();
    int data = (msb << 8) | lsb; // Reensamblar datos
  }
}

```

---

## Protocolo SPI (Serial Peripheral Interface)

* **Arquitectura:** Requiere 4 hilos: `MOSI` (M->S), `MISO` (S->M), `SCK` (Reloj) y `CS/SS` (Chip Select).
* **Capacidad:** Puede operar por encima de los 20-50 MHz. Utiliza conducción activa (no requiere pull-ups).
* **Aplicación Clínica:** Pantallas gráficas TFT de monitores de signos vitales o Dataloggers de tarjetas MicroSD (ECG Holter).

---

## Sintaxis Arduino: Transacciones Seguras en SPI

```cpp
#include <SPI.h>
const int CS_PIN = 10;

void setup() {
  pinMode(CS_PIN, OUTPUT);
  digitalWrite(CS_PIN, HIGH); // Esclavo inactivo (Activo en LOW)
  SPI.begin();
}

void loop() {
  // Bloquear bus para evitar colisiones de interrupción
  SPI.beginTransaction(SPISettings(14000000, MSBFIRST, SPI_MODE0));
  
  digitalWrite(CS_PIN, LOW);   // Seleccionar esclavo
  uint8_t response = SPI.transfer(0xAA); // Enviar y recibir
  digitalWrite(CS_PIN, HIGH);  // Liberar esclavo
  
  SPI.endTransaction();
}

```

---

## Protocolo CAN Bus (Implementación TWAI)

* **Señalización Diferencial:** Inmune a interferencia electromagnética (EMI). Transiciona agresivamente entre 1.5V y 3.5V (CAN-H y CAN-L).
* **Implementación:** Requerido en sillas de ruedas eléctricas o escáneres médicos pesados.
* **Física Crítica:** El microcontrolador solo procesa la lógica (RX/TX); se requiere **obligatoriamente un transceptor CAN externo** (ej. TJA1050) para no destruir el chip.

---

## Compatibilidad Eléctrica: 3.3V vs 5V (Level Shifting)

* **El Problema:** Conectar señales de 5V (sensores antiguos) a arquitecturas modernas (ESP32, RP2350 operando a 3.3V) destruye los diodos ESD de entrada.
* **Solución I2C:** Uso de transistores MOSFET (BSS138) bidireccionales con pull-ups integrados.
* **Solución SPI:** El BSS138 deforma el reloj. SPI exige búferes unidireccionales activos (HC4050) o aceleradores de flanco (TXB0108).

---

## Análisis de Arquitectura: Familia AVR (5V)

* Hardware estrictamente mapeado: UART fijado en pines 0 y 1. I2C fijado en A4 y A5.
* **Cuello de botella:** Posee un único UART por hardware. Si se conecta un sensor serial, interfiere con la programación USB.
* Obliga al uso de `SoftwareSerial` (bit-banging), lo cual bloquea interrupciones y corrompe tiempos de ejecución.

---

## Expansión AVR: Mega y Leonardo

* **Arduino Mega (ATmega2560):** Integra 4 puertos UART físicos (`Serial`, `Serial1`, `Serial2`, `Serial3`). Ideal para telemetría multicanal.
* **Arduino Leonardo (ATmega32U4):** Integra un controlador USB nativo en el silicio (CDC). Su clase `Serial` apunta al USB, dejando los pines RX/TX (físicos) 100% libres bajo la clase `Serial1`.

---

## Arquitecturas Modernas: Raspberry Pi Pico (RP2350)

* Arquitectura ARM/RISC-V a 3.3V. Rompe el paradigma de pines estáticos.
* **Multiplexación:** Los periféricos I2C y SPI pueden enrutarse a múltiples grupos de pines definidos en hardware.
* **PIO (I/O Programable):** Co-procesadores que emulan puertos UART adicionales por hardware sin penalizar a la CPU principal.

---

## Sintaxis Arduino: Reasignación en RP2350

```cpp
#include <Wire.h>

void setup() {
  // Reasignar los pines físicos del controlador I2C0
  // GP4 (SDA) y GP5 (SCL)
  Wire.setSDA(4);
  Wire.setSCL(5);
  Wire.begin();
  
  // Reasignar UART1 físico a nuevos pines
  Serial1.setRX(9);
  Serial1.setTX(8);
  Serial1.begin(115200);
}

```

---

## Familia ESP32 (Arquitectura IoT 3.3V)

* **Matriz GPIO:** Permite redirigir cualquier señal UART, I2C o SPI a prácticamente cualquier pin del encapsulado.
* **Precaución I2C:** Su hardware sufre anomalías con el *Clock Stretching* de esclavos lentos; a veces requiere implementaciones I2C por software.
* **SPI por DMA:** Exige que los arreglos de datos residan en memoria interna (DRAM) mediante `MALLOC_CAP_DMA`.

---

## Sintaxis Arduino: Matriz GPIO en ESP32

```cpp
#include <HardwareSerial.h>

// Evitar pines Input-Only (34-39) y Flash (6-11)
#define RXD2 16
#define TXD2 17
#define I2C_SDA 21
#define I2C_SCL 22

void setup() {
  // Formato: begin(Baudios, Config, RX_Pin, TX_Pin)
  Serial2.begin(9600, SERIAL_8N1, RXD2, TXD2);
  
  // Re-enrutamiento del bus I2C
  Wire.begin(I2C_SDA, I2C_SCL);
}

```

---

## Taller Práctico: Arduino Nano 33 BLE Sense

* **Topología:** Placa todo-en-uno operando a 3.3V, idónea para instrumentación *Wearable*.
* Sensores soldados en la placa comunicados internamente vía I2C:
* IMU de 9 ejes (LSM9DS1).
* Temperatura/Humedad (HTS221).


* **Dinámica (36 Alumnos / Aforo):** Trabajo grupal enfocando la transición desde hardware hacia empaquetado JSON.

---

## Actividad 1: Escáner de Bus I2C

* **Propósito:** Auditar el bus I2C inyectando tramas de sondeo a las 127 direcciones posibles.
* **Relevancia:** Paso obligatorio en ingeniería para descartar fallas eléctricas (ausencia de Pull-ups) antes de programar lógicas de lectura.
* En el Nano 33 BLE, revelará las firmas de hardware estáticas de los biosensores integrados.

---

## Código: Rutina Core del Escáner I2C

```cpp
#include <Wire.h>

void setup() {
  Wire.begin();
  Serial.begin(115200);
  while (!Serial); // Esperar USB CDC
}

void loop() {
  for (byte address = 1; address < 127; address++) {
    Wire.beginTransmission(address);     // Iniciar contacto
    byte error = Wire.endTransmission(); // Evaluar estado de retorno (ACK/NACK)
    
    if (error == 0) {
      Serial.print("Sensor I2C hallado en Hex: 0x");
      Serial.println(address, HEX);
    }
  }
  delay(5000);
}

```

---

## Actividad 2: Acondicionamiento de Señal

* Extraer crudos del sensor I2C térmico o analógico.
* **Problema:** Las variables fisiológicas y electrónicas presentan ruido blanco de alta frecuencia.
* **Solución:** Implementar un algoritmo de Promedio Móvil mediante arreglos o sumatorias simples para suavizar la curva antes del análisis clínico.

---

## Código: Filtrado por Promedio Móvil

```cpp
const int PIN_SENSOR = A0;
const int MUESTRAS = 20;

void setup() { Serial.begin(115200); }

void loop() {
  long suma = 0;
  
  // Adquisición de ráfaga para mitigar varianza eléctrica
  for (int i = 0; i < MUESTRAS; i++) {
    suma += analogRead(PIN_SENSOR);
    delay(5); // Tiempo de asentamiento del ADC
  }
  
  float voltaje = ((float)suma / MUESTRAS) * (3.3 / 1024.0);
  float temperaturaC = voltaje * 100.0; // Escalado para sensor analógico
  
  Serial.println(temperaturaC);
  delay(1000);
}

```

---

## Actividad 3: Empaquetado (Data Framing)

* **Regla de Oro:** En telemetría, no se transmiten textos sueltos ni tramas incomprensibles. Se estructuran cargas útiles (Payloads).
* **Formato JSON:** Estándar ligero que permite inyectar los datos adquiridos por I2C/SPI directamente vía UART hacia un Gateway IoT (ej. ESP32 o Raspberry Pi con Node-RED).

---

## Código: Transmisión de Trama JSON

```cpp
void loop() {
  // Simulación o adquisición de sensores reales
  float temperatura = 36.6 + (random(-1, 2) * 0.05);
  int bpm = 72 + random(-2, 3);
  
  // Ensamblaje del string JSON evadiendo uso masivo de memoria dinámica
  String jsonPayload = "{";
  jsonPayload += "\"dev_id\":\"NANO33_BED_01\",";
  jsonPayload += "\"temp\":" + String(temperatura, 1) + ",";
  jsonPayload += "\"bpm\":" + String(bpm);
  jsonPayload += "}";
  
  // Transmisión asíncrona hacia el Gateway IoT
  Serial.println(jsonPayload);
  delay(2000);
}

```