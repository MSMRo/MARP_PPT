---
marp: true
theme: default
paginate: true
header: " "
footer: "Facultad de Ciencias e Ingenieria"
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
  li { margin-bottom: 10px; }
  img { border-radius: 8px; max-width: 100%; object-fit: contain; background-color: #f8f9fa; padding: 10px;}
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
  <h1>Dispositivos en Electrónica Analógica Avanzada</h1>
  <p style="margin: 5px 0 0 0; font-size: 18px; opacity: 0.9;">Principios, Análisis de Señal y Operación</p></div><div class="subtitle">Análisis de Circuitos Analógicos</div><div class="author" style="font-weight: bold; font-size: 18px; color: #111111; margin-top: 15px;">
  Mag. Ing. CIP Moisés Stiglitz Rodríguez</div><div class="author" style="font-size: 15px; color: #555555; margin-top: 5px;">
  Facultad de Ciencias e Ingenieria</div></div>

---

## Elementos Pasivos: Resistores y Capacitores

<div class="columns">
<div>

<div class="beamer-block">
  <strong>Comportamiento en CD y Frecuencia</strong>
</div>

* **Resistores:** Siguen la Ley de Ohm en CD. A altas frecuencias, la resistencia de la fuente y de la carga impactan severamente en la respuesta del sistema y la ganancia.
* **Capacitancia:** Fundamental para el análisis temporal. El voltaje no cambia de forma instantánea. 
* **Acoplo/Desacoplo:** En análisis de ca de pequeña señal, los capacitores se asumen típicamente como cortocircuitos para aislar la polarización de CD de la señal variable.

</div>
<div>

<img src="https://images.unsplash.com/photo-1580828369019-33923c896e00?fm=jpg&w=500" alt="Resistores y capacitores" />

</div>
</div>

---

## El Inductor y Efectos Transitorios

<div class="columns">
<div>

<div class="beamer-block">
  <strong>Almacenamiento y Protección</strong>
</div>

* **Reactancia:** Se opone a los cambios bruscos de corriente. Su comportamiento dinámico está regido por la inducción electromagnética.
* **Transitorios y Peligros:** En estado estático actúa casi como un cable, pero la apertura abrupta del circuito genera altos voltajes transitorios inducidos (picos de tensión).
* **Mitigación:** En el diseño de hardware, se requiere control estricto (como circuitos *snubber* o diodos de protección) para evitar daño a transistores de conmutación.

</div>
<div>

<img src="https://images.unsplash.com/photo-1530982011887-3cc11cc85693?fm=jpg&w=500" alt="Inductor en circuito" />

</div>
</div>

---

## Transistor BJT: Polarización en Continua (CD)

<div class="columns">
<div>

<div class="beamer-block">
  <strong>Zonas de Operación y Control</strong>
</div>

* **Corriente de Base:** El BJT (npn o pnp) es un dispositivo controlado por corriente. Pequeñas variaciones en $I_B$ controlan grandes flujos en $I_C$.
* **Punto de Operación (Q):** Las redes de polarización de CD establecen voltajes y corrientes fijos. Se ubica el punto Q en la región activa lineal para amplificación.
* **Configuraciones:** La configuración de base común ofrece baja impedancia de entrada y