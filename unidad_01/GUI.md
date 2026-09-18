# Práctica Introductoria: Interfaces Gráficas de Usuario (GUI / HMI) para Ingeniería

Este repositorio/documento contiene el material de inicio para la materia de **Interfaces Gráficas de Usuario**, orientado a estudiantes de **Ingeniería en Electrónica**.

El objetivo de esta práctica es comprender la transición de la programación secuencial a la **Programación Orientada a Eventos (EDP)** mediante un prototipo simple de Panel de Control y Monitoreo (HMI Industrial).

---

## Parte 1: Práctica en Python (Google Colab)

En esta primera parte, utilizaremos **Google Colab** y la librería `ipywidgets` para simular un panel HMI industrial directamente en un entorno de cuaderno interactivo.

### Código de la Práctica

Copia y pega el siguiente código en una celda de código de tu cuaderno en Google Colab y ejecútalo:

```python
import ipywidgets as widgets
from IPython.display import display, clear_output
import random
import time

# -------------------------------------------------------------------
# 1. COMPONENTES VISUALES (Widgets)
# -------------------------------------------------------------------
# Título principal del panel
titulo = widgets.HTML(value="<h2>📡 Panel de Control y Monitoreo (HMI Simulado)</h2>")

# Botón de acción (Entrada de evento)
btn_leer = widgets.Button(
    description="Leer Sensor",
    button_style='primary', # Estilos: 'primary', 'success', 'info', 'warning', 'danger'
    icon='play'
)

# Control deslizante numérico (Ajuste de parámetro PWM)
potenciometro = widgets.IntSlider(
    value=50,
    min=0,
    max=100,
    step=1,
    description='PWM Motor (%):',
    disabled=False,
    continuous_update=False,
    orientation='horizontal'
)

# Área de salida de datos (Pantalla o consola visual)
salida = widgets.Output()

# -------------------------------------------------------------------
# 2. MANEJADORES DE EVENTOS (Callbacks / Slots)
# -------------------------------------------------------------------
def al_hacer_clic(b):
    """Función Callback que se ejecuta únicamente al presionar el botón"""
    with salida:
        clear_output(wait=True)
        # Simulamos la lectura de un sensor de temperatura industrial
        temp_simulada = round(random.uniform(20.0, 85.0), 2)
        pwm_val = potenciometro.value
        
        print(f" Timestamp: {time.strftime('%H:%M:%S')}")
        print(f" Temperatura leída: {temp_simulada} °C")
        print(f" Duty Cycle PWM configurado: {pwm_val}%")
        
        # Lógica de toma de decisiones (Alerta de ingeniería)
        if temp_simulada > 70.0:
            print(" ¡ALERTA: Sobrecalentamiento detectado en el sistema!")
        else:
            print(" Sistema en rangos normales de operación.")

# Enlazamos el evento 'on_click' del botón con nuestra función Callback
btn_leer.on_click(al_hacer_clic)

# -------------------------------------------------------------------
# 3. LAYOUT Y PRESENTACIÓN DE LA INTERFAZ
# -------------------------------------------------------------------
# Organizamos los controles de forma vertical en un contenedor (VBox)
panel_control = widgets.VBox([
    titulo,
    widgets.HTML(value="<b>Ajustes de Entrada:</b>"),
    potenciometro,
    btn_leer,
    widgets.HTML(value="<hr><b>Monitoreo en Tiempo Real:</b>"),
    salida
])

# Mostramos el panel completo en pantalla
display(panel_control)
```

---

## Parte 2: Prompt para Generar la Misma Interfaz en Desarrollo Web (HTML, CSS y JS)

Para comprender cómo se construyen las interfaces en entornos web reales y cómo interactúan los tres pilares de la programación web (`HTML`, `CSS` y `JavaScript`), utiliza el siguiente **prompt** con un asistente de inteligencia artificial (LLM):

### Prompt de Generación (Copiar y usar)

```text
Actúa como un profesor de ingeniería electrónica y desarrollo web. Necesito crear una interfaz HMI / Dashboard Industrial para el monitoreo de un sensor y control de un motor (similar a un panel de control con lectura de temperatura y control PWM).

Genera el código completo estructurado en 3 archivos separados:
1. index.html (Estructura y contenido visual)
2. styles.css (Diseño, colores y layout industrial)
3. script.js (Lógica orientada a eventos e interacción)

Requisitos de la interfaz:
- Un título principal: "Panel de Control y Monitoreo (HMI Simulado)".
- Un control deslizante (slider) de 0 a 100% para ajustar el "PWM Motor".
- Un botón con el texto "Leer Sensor".
- Un área de consola/pantalla para desplegar los resultados (Timestamp, Temperatura simulada, PWM actual y estado de Alerta de sobrecalentamiento si la temperatura supera los 70°C).

Requisitos de la explicación:
- Explica qué función cumple cada uno de los 3 archivos dentro de la arquitectura web.
- Explica el código paso a paso y LÍNEA POR LÍNEA (o bloque por bloque detallado) usando comentarios directos en el código y explicaciones sencillas.
- Explica claramente cómo se conecta el botón con JavaScript mediante el evento 'click' (Programación Orientada a Eventos).
```

---

## Preguntas de Reflexión para el Alumno

1. **Programación Secuencial vs. Eventos:** ¿Por qué el código dentro de la función `al_hacer_clic` no se ejecuta inmediatamente al correr la celda en Python?
2. **Separación de Capas:** En la versión web (HTML/CSS/JS), ¿cuál de los tres archivos equivale a la estructura de la ventana, cuál al estilo visual y cuál a la lógica de control?
3. **Manejo en Tiempo Real:** Si el sensor enviara 1,000 datos por segundo desde un puerto serie, ¿qué problema ocurriría al intentar actualizar la interfaz gráfica en cada lectura? ¿Cómo se solucionaría?