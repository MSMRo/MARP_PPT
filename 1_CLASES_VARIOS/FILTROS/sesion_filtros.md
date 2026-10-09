---
marp: true
theme: dsp
paginate: true
size: 16:9
header: "Procesamiento Digital de Señales"
footer: "Filtros Digitales FIR e IIR"
---

# Filtros Digitales FIR e IIR

## Procesamiento Digital de Señales (DSP)

### FIR • IIR • Biquads • Transformada Bilineal • SciPy

<br><br>
<br><br>

**Mag. Ing. CIP Moises Meza Rodriguez**

---

# Objetivos de la clase

- Comprender el concepto de filtrado digital
- Diferenciar FIR e IIR
- Analizar respuestas en frecuencia
- Comprender el diseño mediante ventanas
- Estudiar familias clásicas IIR
- Comprender biquads
- Entender la transformación bilineal
- Implementar filtros en Python usando SciPy

---

# Motivación

<div class="threecols">

<div class="col">

## Beneficios

- Eliminar ruido
- Restaurar señales
- Separar espectros
- Extraer características

</div>

<div class="col">

## Biomédica

- ECG
- EEG
- EMG
- Ultrasonido

</div>

<div class="col">

## Industria

- Audio DSP
- Telecom
- IoT
- TinyML

</div>

</div>
---

# Concepto de filtro

Un filtro modifica selectivamente componentes frecuenciales.

$$
x[n] \rightarrow H(z) \rightarrow y[n]
$$

## Entrada
- Señal original

## Sistema
- Filtro digital

## Salida
- Señal procesada

---

# Clasificación de filtros

## Según respuesta frecuencial

- Pasa bajas (Low-pass)
- Pasa altas (High-pass)
- Pasa banda (Band-pass)
- Rechaza banda / Notch

---

# Filtro pasa bajas ideal

$$
H(\omega)=
\begin{cases}
1, & |\omega|<\omega_c \\
0, & \text{otro caso}
\end{cases}
$$

## Función
Permite frecuencias bajas y elimina altas.

---

# Filtro pasa altas ideal

## Función
Elimina bajas frecuencias y deja pasar altas.

Aplicaciones:
- Edge detection
- Realce
- Audio

---

# Filtro pasabanda

## Función
Permite únicamente una banda específica.

Aplicaciones:
- Comunicaciones
- ECG
- EEG

---

# Filtro notch

## Función
Elimina una frecuencia específica.

Aplicaciones:
- Ruido 50/60 Hz
- Interferencia industrial

---

# Analógicos vs Digitales

| Analógicos | Digitales |
|---|---|
| Continuos | Discretos |
| RLC / Opamps | DSP / MCU |
| Ecuaciones diferenciales | Ecuaciones en diferencias |
| Sensibles a temperatura | Reproducibles |
| Difícil reconfiguración | Reprogramables |

---

# Convolución

## Base del filtrado FIR

$$
y[n]=x[n]*h[n]
$$

$$
y[n]=\sum_{k=-\infty}^{\infty}x[k]h[n-k]
$$

---

# Respuesta al impulso

Si:

$$
x[n]=\delta[n]
$$

Entonces:

$$
y[n]=h[n]
$$

## Importancia
Caracteriza completamente al sistema LTI.

---

# Respuesta en frecuencia

$$
H(z)=\frac{Y(z)}{X(z)}
$$

$$
H(z)=
\frac{
b_0+b_1z^{-1}+\cdots+b_Mz^{-M}
}{
1+a_1z^{-1}+\cdots+a_Nz^{-N}
}
$$

---

# Polos y ceros

## Ceros
- Atenúan frecuencias

## Polos
- Amplifican / generan resonancia

## Estabilidad

Todos los polos deben estar dentro del círculo unidad.

---

# Especificaciones de filtros

## Parámetros

- Banda pasante
- Banda rechazo
- Ripple
- Atenuación
- Banda transición

---

# Especificaciones típicas

$$
\delta_p,\delta_s,\omega_p,\omega_s
$$

## Interpretación

- $\omega_p$ → frecuencia pasante
- $\omega_s$ → frecuencia rechazo
- $\delta_p$ → ripple pasante
- $\delta_s$ → atenuación rechazo

---

# Filtros FIR

## Finite Impulse Response

Características:

- No recursivos
- Solo ceros
- Fase lineal
- Siempre estables

---

# Ecuación FIR

$$
y[n]=\sum_{k=0}^{M}b_kx[n-k]
$$

## Interpretación

Promedio ponderado del pasado.

---

# Estructura FIR

## Componentes

- Delays
- Multiplicadores
- Sumadores

## Importancia

Muy usados en:
- Audio
- Biomedicina
- Comunicaciones

---

# Ventajas FIR

- Estabilidad garantizada
- Fase lineal
- Robustos numéricamente
- Implementación sencilla

---

# Desventajas FIR

- Orden alto
- Mayor memoria
- Mayor costo computacional

---

# Métodos de diseño FIR

## Métodos clásicos

- Ventanas
- Muestreo en frecuencia
- Remez (óptimo)

---

# Filtro ideal y sinc

La respuesta impulsiva ideal es:

$$
h[n]=2f_c \text{sinc}(2f_cn)
$$

## Problema
- Infinita
- No causal

---

# Fenómeno de Gibbs

## Problema del truncamiento

Aparecen oscilaciones cerca del corte.

Consecuencias:
- Ripple
- Distorsión espectral

---

# Método de ventanas

## Idea

$$
h[n]=h_d[n]w[n]
$$

La ventana suaviza el truncamiento.

---

# Ventana rectangular

## Características

- Mainlobe estrecho
- Poor sidelobe attenuation
- Ripple elevado

---

# Ventana Hann

$$
w[n]=0.5-0.5\cos\left(\frac{2\pi n}{N}\right)
$$

## Características

- Buen suavizado
- Ripple moderado

---

# Ventana Hamming

$$
w[n]=0.54-0.46\cos\left(\frac{2\pi n}{N}\right)
$$

## Características

- Muy usada
- Mejor rechazo lateral

---

# Ventana Blackman

$$
w[n]=0.42-0.5\cos\left(\frac{2\pi n}{N}\right)
+0.08\cos\left(\frac{4\pi n}{N}\right)
$$

## Características

- Excelente atenuación
- Transición más ancha

---

# Ventana Kaiser

## Ventana configurable

Parámetro:

$$
\beta
$$

Permite controlar:
- Ripple
- Atenuación
- Transición

---

# Comparación de ventanas

| Ventana | Ripple | Atenuación |
|---|---|---|
| Rectangular | Alto | Baja |
| Hann | Medio | Media |
| Hamming | Bajo | Alta |
| Blackman | Muy bajo | Muy alta |
| Kaiser | Configurable | Configurable |

---

# Filtros IIR

## Infinite Impulse Response

Características:

- Recursivos
- Feedback
- Polos y ceros
- Menor orden

---

# Ecuación IIR

$$
y[n]
=
\sum_{k=0}^{M}b_kx[n-k]
-
\sum_{k=1}^{N}a_ky[n-k]
$$

---

# Ventajas IIR

- Menor orden
- Más eficientes
- Menor memoria
- Diseño desde filtros analógicos

---

# Desventajas IIR

- Posible inestabilidad
- Fase no lineal
- Más sensibles numéricamente

---

# FIR vs IIR

| FIR | IIR |
|---|---|
| Estables | Posible inestabilidad |
| Fase lineal | No lineal |
| Mayor orden | Menor orden |
| Más costo | Más eficiente |

---

# Familias IIR

## Principales tipos

- Butterworth
- Chebyshev I
- Chebyshev II
- Elíptico
- Bessel

---

# Butterworth

## Características

- Máximamente plano
- Sin ripple
- Respuesta suave

---

# Chebyshev Tipo I

## Características

- Ripple en banda pasante
- Transición abrupta
- Menor orden

---

# Chebyshev Tipo II

## Características

- Banda pasante plana
- Ripple en rechazo

---

# Filtros Elípticos

## Características

- Ripple en ambas bandas
- Máxima selectividad
- Orden mínimo

---

# Comparación IIR

| Tipo | Ripple Passband | Ripple Stopband |
|---|---|---|
| Butterworth | No | No |
| Chebyshev I | Sí | No |
| Chebyshev II | No | Sí |
| Elíptico | Sí | Sí |

---

# Estabilidad

## Condición

$$
|p_k|<1
$$

Todos los polos deben estar dentro del círculo unidad.

---

# Biquads

## Secciones de segundo orden

$$
H(z)=
\frac{
b_0+b_1z^{-1}+b_2z^{-2}
}{
1+a_1z^{-1}+a_2z^{-2}
}
$$

---

# ¿Por qué usar biquads?

## Ventajas

- Mejor estabilidad numérica
- Implementación robusta
- Uso industrial estándar
- Ideal para DSP embebido

---

# Formas directas

## Direct Form I
- Más memoria

## Direct Form II
- Menor memoria

## DF-II Transposed
- Más robusta

---

# Cascada de biquads

$$
H(z)=H_1(z)H_2(z)\cdots H_n(z)
$$

## Ventajas

- Mejor precisión
- Menor error de cuantización

---

# Diseño IIR desde analógicos

## Flujo clásico

1. Diseñar filtro analógico
2. Convertirlo a digital

---

# Transformación bilineal

$$
s=
\frac{2}{T}
\frac{1-z^{-1}}{1+z^{-1}}
$$

## Ventajas

- Preserva estabilidad
- Evita aliasing

---

# Frequency warping

## Problema

La transformación no es lineal en frecuencia.

## Solución

Prewarping:

$$
\Omega=
\frac{2}{T}
\tan\left(\frac{\omega}{2}\right)
$$

---

# Python + SciPy

## Librerías

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy import signal