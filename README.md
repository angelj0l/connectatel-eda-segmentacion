# connectatel-eda-segmentacion
Análisis exploratorio de datos (EDA) y segmentación de clientes para ConnectaTel: limpieza de datos, detección de valores inválidos/sentinels, y segmentación por edad y nivel de uso para generar insights accionables de negocio.

# ConnectaTel — Análisis de Comportamiento de Usuarios y Segmentación de Clientes

##  Objetivo del proyecto

Este proyecto realiza un análisis exploratorio de datos (EDA) sobre la actividad de los usuarios de **ConnectaTel**, una empresa de telecomunicaciones, con el fin de:

- Diagnosticar y corregir problemas de calidad de datos (valores nulos, inválidos y sentinels).
- Comprender los patrones de uso del servicio (mensajes y llamadas).
- Segmentar a los clientes según edad y nivel de uso.
- Traducir los hallazgos en recomendaciones accionables para el negocio, enfocadas en la oferta de planes y oportunidades comerciales.

##  Datasets utilizados

El proyecto trabaja con dos datasets principales:

### `users`
Contiene la información demográfica y de suscripción de cada usuario:

| Columna | Descripción |
|---|---|
| `user_id` | Identificador único del usuario |
| `age` | Edad del usuario |
| `city` | Ciudad de residencia |
| `plan` | Tipo de plan contratado (Básico / Premium) |
| `reg_date` | Fecha de registro del usuario |
| `churn_date` | Fecha de cancelación del servicio (nulo si el usuario sigue activo) |

### `usage`
Contiene el registro de eventos de uso del servicio (mensajes y llamadas):

| Columna | Descripción |
|---|---|
| `id` | Identificador único del registro/evento |
| `user_id` | Identificador del usuario asociado al evento |
| `type` | Tipo de evento (`text` o `call`) |
| `date` | Fecha del evento |
| `duration` | Duración de la llamada (solo aplica a `type == call`) |
| `length` | Longitud del mensaje de texto (solo aplica a `type == text`) |

## 🧩 Etapas del análisis

El notebook está organizado en los siguientes pasos:

1. **Carga y exploración inicial** — revisión de la estructura, tipos de datos y dimensiones de ambos datasets.
2. **Diagnóstico de valores nulos** — identificación de columnas con datos faltantes y su porcentaje de afectación.
3. **Detección de valores inválidos y sentinels** — identificación de valores como `-999` en `age` y `"?"` en `city`, que representan datos faltantes disfrazados de valores válidos.
4. **Revisión y estandarización de fechas** — detección de fechas fuera de rango (por ejemplo, registros de `reg_date` posteriores al periodo cubierto por `usage`).
5. **Análisis de tipo de ausencia (MAR)** — verificación de si los nulos en `duration` y `length` dependen de la variable `type` (Missing At Random).
6. **Limpieza y tratamiento de datos** — imputación de sentinels, estandarización de fechas, y creación de variables derivadas (`is_churned`, `is_text`, `is_call`).
7. **Análisis estadístico y visual** — histogramas, boxplots y conteos para explorar la distribución de variables clave (`age`, `cant_mensajes`, `cant_llamadas`, `cant_minutos_llamada`).
8. **Segmentación de clientes** — clasificación de usuarios en grupos de edad (`Joven`, `Adulto`, `Adulto Mayor`) y nivel de uso (`Bajo uso`, `Uso medio`, `Alto uso`).
9. **Detección de outliers** — identificación de patrones de uso extremo y su implicación para el negocio.
10. **Insight ejecutivo para stakeholders** — conclusiones y recomendaciones de negocio basadas en los hallazgos del análisis.

## ▶️ Cómo ejecutar el notebook

### Opción 1: Google Colab (recomendado, sin instalación)

1. Ve al repositorio en GitHub y abre el archivo `.ipynb`.
2. Haz clic en el botón **"Open in Colab"** (o copia la URL del notebook y ábrela manualmente en [Google Colab](https://colab.research.google.com/) usando `Archivo > Abrir notebook > GitHub`).
3. Sube los archivos de datos (`users.csv`, `usage.csv`, o los que correspondan) a la sesión de Colab, o móntalos desde Google Drive.
4. Ejecuta las celdas en orden desde el inicio (`Entorno de ejecución > Ejecutar todas`).

### Opción 2: Ejecución local con Jupyter Notebook

1. Clona este repositorio:
   ```bash
   git clone https://github.com/tu-usuario/connectatel-user-behavior-analysis.git
   cd connectatel-user-behavior-analysis
   ```

2. Crea un entorno virtual (opcional pero recomendado):
   ```bash
   python -m venv venv
   source venv/bin/activate   # En Windows: venv\Scripts\activate
   ```

3. Instala las dependencias necesarias:
   ```bash
   pip install pandas numpy seaborn matplotlib jupyter
   ```

4. Abre Jupyter Notebook:
   ```bash
   jupyter notebook
   ```

5. Abre el archivo `.ipynb` del proyecto y ejecuta las celdas en orden.

##  Guía rápida de reproducción

Para reproducir el análisis completo desde cero:

1. Asegúrate de tener los datasets `users` y `usage` en la ruta esperada por el notebook (revisa la celda de carga de datos al inicio).
2. Ejecuta las celdas en orden secuencial — el notebook depende de que cada paso se ejecute antes del siguiente (por ejemplo, la limpieza de `age` debe correr antes de los histogramas de edad).
3. Verifica que las librerías `pandas`, `numpy`, `seaborn` y `matplotlib` estén instaladas y actualizadas.
4. Revisa la sección final del notebook (**Insight Ejecutivo para Stakeholders**) para ver el resumen de conclusiones y recomendaciones de negocio.

##  Librerías utilizadas

```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
```

##  Principales hallazgos

- Se identificaron y corrigieron valores sentinel (`-999` en `age`, `"?"` en `city`) que afectaban hasta un 14.1% de los registros en algunas columnas.
- Los nulos en `duration` y `length` resultaron ser un patrón **MAR** (Missing At Random), dependiente de la variable `type`, por lo que se dejaron como nulos en lugar de imputarlos.
- Se identificó un segmento pequeño (~7%) pero altamente valioso de usuarios de **Alto uso**, concentrado en el plan Premium, con un consumo de minutos por llamada significativamente mayor al resto de la base.
- El segmento **Joven** representa una oportunidad de crecimiento, actualmente subrepresentado en la base de usuarios.

## 📄 Licencia

Este proyecto se comparte con fines educativos y de portafolio.
