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
  ul { font-size: 16px; line-height: 1.5; color: #222222; }
  li { margin-bottom: 8px; }
  table { width: 100%; border-collapse: collapse; font-size: 14px; margin-bottom: 15px; }
  th { background-color: #2b32b2; color: white; padding: 8px; text-align: left; }
  td { background-color: #f0f2f9; padding: 8px; border-bottom: 1px solid #ddd; color: #222; }
  img { border-radius: 8px; max-width: 100%; object-fit: cover; }
  pre { font-size: 13px; background-color: #f6f8fa; padding: 12px; border-radius: 6px; border-left: 4px solid #2b32b2; }
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
  <h1>Protocolos de Hardware y Programación de Sensores</h1>
  <p style="margin: 5px 0 0 0; font-size: 18px; opacity: 0.9;">Sesión 3: Unidad 2 - Ecosistema de Arduino</p></div><div class="subtitle">Curso: Desarrollo de Hardware para Telesalud</div><div class="author" style="font-weight: bold; font-size: 18px; color: #111111; margin-top: 15px;">
  Mag. Ing. CIP Moises Meza Rodriguez</div><div class="author" style="font-size: 15px; color: #555555; margin-top: 5px;">
  Facultad de Ciencias e Ingeniería</div></div>

---

## Fundamentos de Comunicación Hardware

<div class="columns">
<div>

<div class="beamer-block">
  <strong>Capa Física y Abstracción (HAL)</strong>
</div>

* El entorno Arduino simplifica la programación mediante clases (`Serial`, `Wire`, `SPI`), abstrayendo la manipulación de registros a nivel de bit.
* **Limitaciones Físicas:** El código no exime al ingeniero de respetar voltajes lógicos, topologías de bus y reglas de ruteo de señales.
* **Importancia Clínica:** Una elección errónea de protocolo induce ruido, pérdida de tramas y retrasos en el monitoreo de pacientes.

</div>
<div>

<img src="https://images.unsplash.com/photo-1518932945647-7a3c969f414b?fm=jpg&w=500" alt="Microchip PCB" />

</div>
</div>

---

## Tabla Comparativa de Protocolos

<div class="columns" style="grid-template-columns: 1fr;">
<div>

<div class="beamer-block">
  <strong>Análisis Estructural para Instrumental Médico</strong>
</div>

| Criterio | UART | I2C | SPI | CAN Bus |
| :--- | :--- | :--- | :--- | :--- |
| **Sincronía** | Asíncrono | Síncrono | Síncrono | Asíncrono |
| **Hilos Físicos** | 2 (TX, RX) | 2 (SDA, SCL) | 4+ (MOSI, MISO, SCK, CS) | 2 (CAN-H, CAN-L) |
| **Topología** | Punto a Punto | Bus Multiesclavo | Multiesclavo (Pines CS) | Red Multimaestro |
| **Modo** | Full-Duplex | Half-Duplex | Full-Duplex | Diferencial |
| **Velocidad** | 9.6 - 115.2 kbps | 100 - 400 kbps | 10 - 50 MHz | Hasta 1 Mbps |
| **Aplicación** | Telemetría GPRS/BLE | Sensores (Temp, SpO2) | TFTs y MicroSD (Holter) | Redes robustas y EMI |

</div>
</div>

---

## Protocolo UART (Universal Asynchronous Receiver-Transmitter)

<div class="columns">
<div>

<div class="beamer-block">
  <strong>Comunicación Asíncrona Serial</strong>
</div>

* **Arquitectura:** Interfaz de 2 hilos (`TX` transmite, `RX` recibe). Requiere referencia de tierra (`GND`) compartida.
* **Baud Rate:** Al carecer de línea de reloj, ambos procesadores deben preconfigurar idéntica velocidad de muestreo.
* **Conflicto en AVR:** En placas como Arduino Uno, el hardware UART (pines 0,1) está atado al puerto USB, impidiendo conectar sensores directamente sin usar emulación por software.

</div>
<div>

<img src="https://images.unsplash.com/photo-1517077304055-6e89abbf09b0?fm=jpg&w=500" alt="Cables TX RX" />

</div>
</div>

---

## Código: Manejo de UART en Arduino IDE

<div class="columns" style="grid-template-columns: 1.4fr 0.6fr;">
<div>

<div class="beamer-block">
  <strong>Instanciación y Lectura de Buffers</strong>
</div>

```cpp
void setup() {
  // Inicializa el puerto hardware a 115200 bps
  Serial.begin(115200);
  
  // En placas nativas USB (Pico/Leonardo),
  // se debe esperar la enumeración del puerto COM
  while (!Serial) { delay(10); } 
}

void loop() {
  // Verificación no destructiva del buffer
  if (Serial.available() > 0) {
    String trama = Serial.readStringUntil('\n');
    Serial.print("Trama recibida: ");
    Serial.println(trama);
  }
}