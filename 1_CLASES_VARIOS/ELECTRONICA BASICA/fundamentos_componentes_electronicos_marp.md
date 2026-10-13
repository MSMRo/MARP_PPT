---
marp: true
theme: default
paginate: true
size: 16:9
math: mathjax
style: |
  section {
    font-family: "Aptos", "Arial", sans-serif;
    font-size: 26px;
    padding: 42px;
    color: #172033;
    background: #f7f9fc;
  }
  h1, h2 { color: #123b66; }
  h1 { font-size: 1.7em; }
  h2 { font-size: 1.25em; }
  strong { color: #0b6e69; }
  img { max-height: 260px; object-fit: contain; }
  table { font-size: 0.76em; }
  section.lead { background: #102a43; color: #fff; }
  section.lead h1, section.lead h2 { color: #fff; }
---

<!-- _class: lead -->
# Fundamentos teóricos de los componentes electrónicos
## Física, modelos, símbolos y circuitos de aplicación

**Nivel:** universitario · Electrónica analógica y digital  
**Objetivo:** relacionar el comportamiento físico con modelos de circuito y decisiones de diseño.

![bg right:38% contain](https://upload.wikimedia.org/wikipedia/commons/3/3a/Electronic_components.jpg)

---

## 1. Resultados de aprendizaje

Al finalizar, el estudiante podrá:

- Explicar el principio físico de componentes pasivos y semiconductores.
- Interpretar símbolos esquemáticos y hojas de datos (*datasheets*).
- Calcular magnitudes básicas en circuitos de polarización, filtrado y conmutación.
- Elegir componentes según tensión, corriente, potencia, frecuencia y tolerancia.
- Identificar límites de los modelos ideales y riesgos de implementación.

**Idea central:** un componente real no es solo un símbolo; es un dispositivo físico con parámetros, pérdidas y límites.

---

## 2. Lenguaje común: tensión, corriente y potencia

- Tensión: diferencia de potencial, \(v(t)\), medida en voltios (V).
- Corriente: flujo de carga, \(i(t)=dq/dt\), medida en amperios (A).
- Potencia instantánea: \(p(t)=v(t)i(t)\).
- Energía transferida: \(w=\int p(t)\,dt\).
- Convención pasiva: si la corriente entra por el terminal marcado positivo, \(p>0\) representa potencia absorbida.

### Leyes de Kirchhoff

- **KCL:** suma algebraica de corrientes en un nodo igual a cero.
- **KVL:** suma algebraica de tensiones en una malla igual a cero.

![width:600px](https://upload.wikimedia.org/wikipedia/commons/5/5a/Ohms_law_triangle.svg)

---

## 3. Resistor: relación constitutiva

![width:300px](https://upload.wikimedia.org/wikipedia/commons/1/1b/Resistor.jpg)

Para un resistor óhmico ideal:

\[
v(t)=R\,i(t), \qquad P=VI=I^2R=\frac{V^2}{R}
\]

- \(R\) se mide en ohmios (\(\Omega\)).
- Disipa energía principalmente como calor; no la almacena idealmente.
- Parámetros reales: tolerancia, coeficiente térmico, potencia nominal, tensión máxima y ruido térmico.
- En altas frecuencias aparecen inductancia y capacitancia parásitas.

**Ejemplo:** con \(V=5\,V\) y \(R=1\,k\Omega\), \(I=5\,mA\) y \(P=25\,mW\).

![width:400px](https://upload.wikimedia.org/wikipedia/commons/7/7e/Resistor_symbol.svg)

---

## 4. Resistor: uso como divisor de tensión

![width:260px](https://upload.wikimedia.org/wikipedia/commons/8/8b/Voltage_divider.svg)

Para una salida sin carga:

\[
V_{out}=V_{in}\frac{R_2}{R_1+R_2}
\]

**Diseño ilustrativo:** \(V_{in}=5\,V\), \(R_1=R_2=10\,k\Omega\) → \(V_{out}=2.5\,V\).

- La carga conectada en paralelo con \(R_2\) reduce la resistencia equivalente y altera la salida.
- La corriente de la rama es \(I=V_{in}/(R_1+R_2)\).
- Verificar potencia de cada resistor y tolerancias.

**Aplicaciones:** referencia sencilla, lectura de sensores resistivos y polarización. No es un regulador de tensión robusto.

---

## 5. Capacitor: campo eléctrico y almacenamiento

![width:300px](https://upload.wikimedia.org/wikipedia/commons/8/8e/Capacitors.jpg)

Un capacitor almacena energía en un campo eléctrico:

\[
q=Cv,\quad i=C\frac{dv}{dt},\quad E=\frac{1}{2}CV^2
\]

- \(C\): capacitancia en faradios (F).
- En régimen DC ideal, tras cargarse, la corriente tiende a cero.
- La tensión no cambia instantáneamente si la corriente es finita.
- Parámetros reales: ESR, ESL, fuga, tolerancia y tensión máxima.
- Cerámicos, electrolíticos, película y tantalio tienen usos y restricciones diferentes.

**Precaución:** los electrolíticos polarizados deben conectarse respetando la polaridad y su tensión nominal.

---

## 6. Capacitor: respuesta transitoria RC

![width:400px](https://upload.wikimedia.org/wikipedia/commons/4/4a/RC_Charging_Circuit.svg)

Para carga desde una fuente ideal \(V_S\):

\[
v_C(t)=V_S(1-e^{-t/RC}), \qquad \tau=RC
\]

- En \(t=\tau\), \(v_C\) alcanza aproximadamente el 63.2 % de su valor final.
- En torno a \(5\tau\), el proceso está prácticamente asentado (≈99.3 %).
- En descarga: \(v_C(t)=V_0e^{-t/RC}\).

**Ejemplo:** \(R=10\,k\Omega\), \(C=100\,\mu F\) → \(\tau=1\,s\).

**Aplicaciones:** temporización, desacoplo, filtrado y suavizado de transitorios.

---

## 7. Inductor: energía en el campo magnético

![width:300px](https://upload.wikimedia.org/wikipedia/commons/4/4b/Inductors.jpg)

Modelo ideal:

\[
v_L=L\frac{di}{dt}, \qquad E=\frac{1}{2}Li^2
\]

- \(L\): inductancia en henrios (H).
- La corriente no cambia instantáneamente en un inductor ideal con tensión finita.
- Un inductor se opone a cambios de corriente; no “bloquea siempre” la corriente.
- Componentes reales: resistencia del devanado, saturación del núcleo, pérdidas magnéticas y capacitancia parásita.

**Aplicaciones:** convertidores DC-DC, filtros, fuentes conmutadas, transformadores y adaptación de impedancia.

---

## 8. Inductor: respuesta y protección

![width:460px](https://upload.wikimedia.org/wikipedia/commons/3/3e/Flyback_diode_circuit.svg)

Cuando se interrumpe bruscamente la corriente de una bobina, esta genera la tensión necesaria para intentar mantenerla.

- En una carga inductiva DC, un diodo de rueda libre proporciona un camino alternativo a la corriente.
- La polaridad del diodo debe permitir operación normal y conducir al apagar el interruptor.
- La energía almacenada se disipa en el circuito de recirculación.
- Para relés rápidos o control preciso, considerar TVS, zéner o redes RC; el diodo simple puede aumentar el tiempo de liberación.

**No conectar una bobina directamente a un pin de microcontrolador.**

---

## 9. Diodo semiconductor: unión PN

![width:320px](https://upload.wikimedia.org/wikipedia/commons/4/4b/Diode.jpg)

Al unir regiones tipo P y N se forma una región de agotamiento y una barrera de potencial.

- **Polarización directa:** la barrera disminuye y la corriente crece exponencialmente.
- **Polarización inversa:** circula una corriente pequeña hasta alcanzar ruptura.
- Modelo de Shockley:

\[
I_D=I_S\left(e^{V_D/(nV_T)}-1\right)
\]

\(V_T=kT/q\) es la tensión térmica (≈25.9 mV a 300 K).

El valor de “0.7 V” para silicio es una aproximación útil, no una constante universal.

---

## 10. Diodo: rectificación y protección

![width:440px](https://upload.wikimedia.org/wikipedia/commons/6/6e/Bridge_rectifier.svg)

**Rectificador de media onda:** un diodo deja pasar una semionda y bloquea la opuesta.

**Puente rectificador:** cuatro diodos permiten obtener una salida de polaridad fija en ambas semiondas.

- Revisar corriente media/pico, tensión inversa repetitiva y disipación.
- La salida rectificada todavía tiene rizado; suele requerir un capacitor de filtrado.
- Diodos Schottky: menor caída directa típica y recuperación rápida, con mayor fuga en muchas familias.
- Diodos Zener/TVS: usados para referencia o limitación de tensión dentro de sus condiciones nominales.

---

## 11. LED: conversión de energía eléctrica en luz

![width:280px](https://upload.wikimedia.org/wikipedia/commons/6/60/LED.jpg)

Un LED emite fotones por recombinación de portadores en una unión semiconductora.

- El color depende principalmente de la energía de banda prohibida del material.
- La corriente, no una tensión fija, determina de forma práctica el punto de operación.
- La intensidad luminosa y el color varían con la corriente y la temperatura.

### Resistor limitador

\[
R=\frac{V_S-V_F}{I_F}
\]

Ejemplo: \(V_S=5\,V\), \(V_F=2\,V\), \(I_F=10\,mA\) → \(R=300\,\Omega\). Puede elegirse el valor comercial de 330 Ω para reducir corriente.

---

## 12. Transistor BJT: control por corriente

![width:300px](https://upload.wikimedia.org/wikipedia/commons/8/8b/Transistors.jpg)

En el BJT, una pequeña corriente de base permite controlar una corriente de colector mayor, dentro de sus límites.

En región activa:

\[
I_C \approx \beta I_B
\]

- Terminales: emisor, base y colector.
- Regiones: corte, activa y saturación.
- \(\beta\) varía entre unidades y con corriente, temperatura y tensión.
- Como interruptor, se diseña para saturación y no se debe confiar en un \(\beta\) típico sin margen.

**Aplicaciones:** conmutación de relés, LED y cargas pequeñas; amplificación analógica.

---

## 13. BJT como interruptor de una carga

![width:460px](https://upload.wikimedia.org/wikipedia/commons/3/3b/Transistor_switch_circuit.svg)

En un interruptor NPN de lado bajo:

1. La carga se conecta entre \(+V\) y el colector.
2. El emisor va a tierra común.
3. La base recibe una señal a través de \(R_B\).
4. Para saturación se usa una ganancia forzada conservadora:

\[
I_B \ge \frac{I_C}{\beta_{forzada}},\qquad
R_B \approx \frac{V_{GPIO}-V_{BE}}{I_B}
\]

**Importante:** verificar corriente máxima del GPIO, \(I_C\), \(V_{CE(sat)}\) y potencia. Para cargas inductivas, añadir protección de rueda libre.

---

## 14. MOSFET: control por campo eléctrico

![width:320px](https://upload.wikimedia.org/wikipedia/commons/6/6f/MOSFET.jpg)

El MOSFET controla la conductividad del canal mediante el campo eléctrico creado por la tensión puerta-fuente \(V_{GS}\).

- Terminales: gate (G), drain (D), source (S); existe capacitancia de puerta.
- La puerta consume poca corriente DC ideal, pero requiere corriente transitoria para cargarse.
- \(V_{GS(th)}\) indica el inicio de conducción a una corriente de prueba baja: **no** garantiza encendido completo.
- Para conmutación eficiente, elegir por \(R_{DS(on)}\) especificada al \(V_{GS}\) disponible.
- Considerar carga de puerta, pérdidas de conmutación, SOA y disipación térmica.

---

## 15. MOSFET como interruptor de carga

![width:450px](https://upload.wikimedia.org/wikipedia/commons/5/5a/MOSFET_switch_circuit.svg)

Para un MOSFET N de lado bajo:

- Source a referencia común; drain a la carga; la carga a \(+V\).
- La señal de control debe alcanzar el \(V_{GS}\) adecuado.
- Pérdida de conducción aproximada:

\[
P_{cond}=I_D^2R_{DS(on)}
\]

- Un resistor de *pull-down* evita que la puerta quede flotante.
- Las cargas inductivas requieren una trayectoria de recirculación o un limitador de sobretensión.

**MOSFET “logic-level”** significa que su resistencia está caracterizada a tensiones de puerta compatibles con la lógica indicada; siempre revisar la hoja de datos.

---

## 16. Amplificador operacional: modelo ideal y realimentación

![width:300px](https://upload.wikimedia.org/wikipedia/commons/3/3d/Operational_amplifier_741.jpg)

Modelo ideal:

- Impedancia de entrada infinita; impedancia de salida cero.
- Ganancia en lazo abierto infinita.
- Con realimentación negativa y operación lineal: \(V_+\approx V_-\) y \(I_+\approx I_-\approx0\).

En un amplificador inversor:

\[
\frac{V_{out}}{V_{in}}=-\frac{R_f}{R_{in}}
\]

En el no inversor:

\[
\frac{V_{out}}{V_{in}}=1+\frac{R_f}{R_g}
\]

Los op-amps reales tienen límites de alimentación, ancho de banda, *slew rate*, ruido, offset y rango de modo común.

---

## 17. Op-amp: acondicionamiento de señales

![width:450px](https://upload.wikimedia.org/wikipedia/commons/8/8e/Op-amp-noninverting.svg)

**Cadena típica de adquisición:**

Sensor → protección/filtrado → amplificador → filtro antialias → ADC.

- La ganancia adapta el rango de amplitud del sensor al ADC.
- El filtro limita ruido y componentes fuera de banda.
- Verificar saturación, impedancia de fuente y capacidad de carga.
- Para biopotenciales, el ruido, la impedancia de entrada, el rechazo de modo común y la seguridad eléctrica son críticos.

**Nota:** no todo op-amp es adecuado como amplificador de instrumentación; elegir según el sensor y el entorno.

---

## 18. Regulador lineal y regulador conmutado

![width:300px](https://upload.wikimedia.org/wikipedia/commons/3/3a/Voltage_regulator.jpg)

**Regulador lineal**
- Controla un elemento serie para mantener \(V_{out}\).
- Simple y de bajo ruido; la eficiencia ideal aproximada es \(\eta\approx V_{out}/V_{in}\).
- Pérdida: \(P\approx(V_{in}-V_{out})I_{out}\).

**Regulador conmutado**
- Usa interruptores y almacenamiento en L/C.
- Puede lograr mayor eficiencia, a cambio de rizado, EMI y complejidad.

**Ejemplo:** bajar 12 V a 5 V a 0.5 A con un lineal implica aproximadamente 3.5 W de disipación: requiere análisis térmico.

---

## 19. Sensores resistivos: LDR, termistor y potenciómetro

![width:300px](https://upload.wikimedia.org/wikipedia/commons/6/6b/Thermistor.jpg)

- **LDR:** su resistencia cambia con la iluminancia; respuesta no lineal y dependencia del material.
- **NTC:** la resistencia disminuye al aumentar la temperatura.
- **Potenciómetro:** divisor resistivo ajustable; útil para consigna o posición.

Un divisor convierte la variación resistiva en tensión medible por un ADC. La resistencia equivalente de Thévenin afecta la precisión de la conversión y el tiempo de adquisición.

**Diseño:** seleccionar rango, autocalentamiento, tolerancia, curva de calibración y filtrado.

---

## 20. Sensor de temperatura NTC: modelo y lectura

![width:260px](https://upload.wikimedia.org/wikipedia/commons/4/4b/NTC_Thermistor.jpg)

Aproximación beta:

\[
R(T)=R_0\exp\left[B\left(\frac{1}{T}-\frac{1}{T_0}\right)\right]
\]

\(T\) se expresa en kelvin. \(R_0\) es la resistencia a \(T_0\).

```text
 Vref
  |
 Rfixed
  |------ Vout → ADC
  |
 NTC
  |
 GND
```

Para esta disposición:

\[
V_{out}=V_{ref}\frac{R_{NTC}}{R_{fixed}+R_{NTC}}
\]

La temperatura se estima con la curva del fabricante. La tolerancia de \(B\), la resistencia de referencia y el autocalentamiento limitan la exactitud.

---

## 21. Interruptores, pulsadores y rebote

![width:260px](https://upload.wikimedia.org/wikipedia/commons/3/3b/Pushbutton.jpg)

Un pulsador mecánico no produce necesariamente una transición digital limpia: los contactos pueden rebotar durante milisegundos.

```text
 Vcc
  |
 Rpull-up
  |------ GPIO
  |
 pulsador
  |
 GND
```

- El *pull-up* fija el nivel lógico cuando el pulsador está abierto.
- Al presionar, la entrada pasa a nivel bajo.
- El *debouncing* puede ser por hardware (RC + entrada Schmitt) o software.
- No dejar entradas CMOS flotantes: pueden cambiar por ruido y consumir más corriente.

---

## 22. Relé y aislamiento de cargas

![width:300px](https://upload.wikimedia.org/wikipedia/commons/7/7c/Relay.jpg)

Un relé usa una bobina electromagnética para mover contactos mecánicos.

- Permite controlar un circuito separado, pero el aislamiento real depende de las especificaciones y del diseño de PCB.
- La bobina es inductiva y necesita una etapa de accionamiento.
- Añadir transistor/MOSFET y diodo de rueda libre en bobinas DC, salvo que el módulo ya lo integre.
- Verificar tensión/corriente de bobina, corriente de contacto, categoría de carga, vida mecánica y distancia de aislamiento.

**Advertencia:** la red eléctrica exige diseño, encapsulado, distancias y procedimientos de seguridad apropiados.

---

## 23. Circuitos integrados digitales: lógica y niveles

![width:300px](https://upload.wikimedia.org/wikipedia/commons/2/2e/Integrated_circuit.jpg)

Un circuito integrado reúne transistores y otros elementos en un sustrato semiconductor.

- Familias lógicas tienen rangos de \(V_{IH}\), \(V_{IL}\), \(V_{OH}\) y \(V_{OL}\).
- “5 V” y “3.3 V” no son niveles universales compatibles en todos los casos.
- Verificar corriente de salida, fan-out, tiempos de subida/bajada y tolerancia a sobretensión.
- Desacoplar cada IC con capacitores cerca de sus pines de alimentación.

**Regla práctica:** revisar el datasheet de ambos dispositivos antes de conectar dos interfaces digitales.

---

## 24. ADC y PWM: interfaz entre mundo analógico y digital

![width:400px](https://upload.wikimedia.org/wikipedia/commons/5/5a/Analog-to-digital_converter.svg)

**ADC:** cuantiza una señal analógica en códigos discretos.

Para un ADC ideal de \(N\) bits y rango \(V_{ref}\):

\[
\Delta V \approx \frac{V_{ref}}{2^N}
\]

Un ADC de 10 bits con \(V_{ref}=5\,V\) tiene un tamaño de paso ideal ≈4.88 mV.

**PWM:** codifica el valor medio mediante el ciclo de trabajo \(D\). Para una carga adecuada y conmutación ideal, \(V_{medio}\approx D V_{in}\).

PWM no es un DAC verdadero; un filtro puede suavizarlo, pero introduce ondulación y tiempo de respuesta.

---

## 25. Ejemplo integrado: control de ventilador con Arduino

![width:500px](https://upload.wikimedia.org/wikipedia/commons/5/5a/MOSFET_switch_circuit.svg)

```text
NTC ── divisor ── ADC ┐
Pulsador ────────────┤
                     ▼
                Microcontrolador
                     │ PWM
                     ▼
                Gate MOSFET
                     │
+Vfan ── Ventilador ─ Drain
                     Source ── GND
                         │
             GND común ──┘
```

- El NTC estima la temperatura; el firmware aplica filtrado y una curva de control.
- El PWM modula la potencia media entregada al ventilador.
- El MOSFET se dimensiona por corriente de arranque, \(R_{DS(on)}\) y temperatura.
- Incorporar protección para la carga inductiva según el tipo de ventilador.
- Separar la corriente de potencia de las rutas sensibles de medición y desacoplar la alimentación.

---

## 26. Cómo leer una hoja de datos (datasheet)

Priorizar, en este orden:

1. **Absolute maximum ratings:** límites de supervivencia; no son condiciones normales de operación.
2. **Recommended operating conditions:** rangos para funcionamiento especificado.
3. **Electrical characteristics:** valores mínimos, típicos y máximos, bajo condiciones indicadas.
4. **Curvas:** dependencia con corriente, temperatura, frecuencia y tensión.
5. **Thermal data:** \(R_{\theta JA}\), \(R_{\theta JC}\), límites de unión.
6. **Package / pinout:** encapsulado y asignación de terminales.
7. **Test circuit / conditions:** sin estas condiciones, una cifra puede ser engañosa.

**Diseño robusto:** margen, tolerancias, transitorios y peor caso.

---

## 27. Selección y fallas frecuentes

| Componente | Error habitual | Qué comprobar |
|---|---|---|
| Resistor | Subestimar disipación | \(P\), tolerancia, temperatura |
| Capacitor | Ignorar polaridad/ESR | Tensión, rizado, ESR |
| Diodo | Exceder tensión inversa | \(V_R\), \(I_F\), recuperación |
| BJT | Usar \(\beta\) típico como garantía | Saturación, \(I_B\), SOA |
| MOSFET | Confundir \(V_{GS(th)}\) con encendido | \(R_{DS(on)}\) al \(V_{GS}\) real |
| Op-amp | Ignorar modo común o salida | Alimentación, BW, slew rate |
| Sensor | Suponer una curva ideal | Calibración, ruido, autocalentamiento |

---

## 28. Síntesis: del modelo al prototipo

- **Modelo físico:** explica por qué el componente se comporta así.
- **Modelo de circuito:** simplifica el análisis bajo supuestos explícitos.
- **Datasheet:** define prestaciones y límites garantizados.
- **Simulación:** permite explorar comportamiento; no reemplaza la validación.
- **Prototipo y medición:** revelan parásitos, tolerancias, EMI y errores de montaje.

### Preguntas para discusión

1. ¿Por qué un divisor resistivo no siempre es una buena fuente de alimentación?
2. ¿Por qué \(V_{GS(th)}\) no basta para seleccionar un MOSFET?
3. ¿Qué parámetros dominan la precisión de una lectura de temperatura?
4. ¿Cómo afecta la carga conectada a un filtro RC?

---

## Referencias técnicas para profundizar

- Horowitz, P. & Hill, W. *The Art of Electronics*. Cambridge University Press.
- Sedra, A. S. & Smith, K. C. *Microelectronic Circuits*. Oxford University Press.
- Boylestad, R. L. & Nashelsky, L. *Electronic Devices and Circuit Theory*.
- All About Circuits — libro de texto abierto: https://www.allaboutcircuits.com/textbook/
- Texas Instruments — notas de aplicación y datasheets: https://www.ti.com/
- Analog Devices — recursos de diseño analógico: https://www.analog.com/
- IEC Electropedia — vocabulario electrotécnico: https://www.electropedia.org/

**Nota docente:** comprobar disponibilidad y licencia de cada imagen enlazada antes de distribuir públicamente la presentación. Las fórmulas y circuitos son modelos didácticos; verificar cada diseño con el datasheet del componente concreto.
