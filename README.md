# TelecomX — Análisis de Evasión de Clientes (Churn Analysis)

> **Proyecto de Ciencia de Datos** | Análisis Exploratorio · Visualización · Generación de Insights  
> *Alura LATAM — Challenge 2 · Grupo 9*

---

## Índice

1. [Descripción del Proyecto](#1-descripción-del-proyecto)
2. [Estructura del Repositorio](#2-estructura-del-repositorio)
3. [Dataset](#3-dataset)
4. [Requisitos del Sistema](#4-requisitos-del-sistema)
5. [Instalación y Configuración](#5-instalación-y-configuración)
6. [Ejecución del Proyecto](#6-ejecución-del-proyecto)
7. [Metodología](#7-metodología)
8. [Variables del Dataset](#8-variables-del-dataset)
9. [Resultados Principales](#9-resultados-principales)
10. [Problemas Conocidos y Soluciones](#10-problemas-conocidos-y-soluciones)
11. [Estándares y Buenas Prácticas](#11-estándares-y-buenas-prácticas)
12. [Licencia](#12-licencia)

---

## 1. Descripción del Proyecto

TelecomX enfrenta un índice elevado de **evasión de clientes (Customer Churn)**, lo que impacta directamente en los ingresos y en los costos de adquisición de nuevos usuarios. El objetivo de este proyecto es realizar un análisis exploratorio de datos (EDA) para:

- Identificar las **variables con mayor correlación** con la evasión.
- Caracterizar el **perfil del cliente evasor** frente al no evasor.
- Generar **insights accionables** que orienten estrategias de retención.

El análisis sigue el flujo estándar de ciencia de datos: **extracción → limpieza → transformación → EDA → conclusiones → recomendaciones**.

---

## 2. Estructura del Repositorio

```
TelecomX/
├── telecomx.ipynb          # Notebook principal con el análisis completo
├── README.md               # Documentación del proyecto (este archivo)
└── .venv/                  # Entorno virtual Python (no incluido en el repositorio)
```

---

## 3. Dataset

| Atributo          | Detalle                                                                                     |
|-------------------|---------------------------------------------------------------------------------------------|
| **Nombre**        | TelecomX_Data.json                                                                          |
| **Fuente**        | [GitHub — ingridcristh/challenge2-data-science-LATAM](https://raw.githubusercontent.com/ingridcristh/challenge2-data-science-LATAM/main/TelecomX_Data.json) |
| **Formato**       | JSON con estructura anidada (normalizado con `pd.json_normalize`)                           |
| **Registros**     | 7 043 clientes (7 032 tras eliminación de nulos en variable objetivo)                       |
| **Variables**     | 21 atributos (demográficos, servicios contratados, datos de cuenta y facturación)           |
| **Variable objetivo** | `Evasor` — `True` si el cliente canceló el servicio, `False` si permanece activo       |
| **Desbalance**    | ~26.5% evasores · ~73.5% no evasores                                                        |

---

## 4. Requisitos del Sistema

| Componente       | Versión mínima recomendada |
|------------------|---------------------------|
| Python           | 3.10 o superior            |
| Jupyter Notebook | 6.5 o VS Code + extensión Jupyter |
| pip              | 23.0 o superior            |
| SO               | Windows 10/11 · macOS 12+ · Ubuntu 20.04+ |

### Dependencias Python

```
pandas>=2.0
numpy>=1.24
matplotlib>=3.7
seaborn>=0.12
plotly>=5.14
requests>=2.28
```

---

## 5. Instalación y Configuración

### 5.1 Clonar el repositorio

```bash
git clone https://github.com/Edbeto13/TelecomXAluraG9.git
cd TelecomXAluraG9
```

### 5.2 Crear el entorno virtual

```bash
# Windows
python -m venv .venv
.venv\Scripts\activate

# macOS / Linux
python3 -m venv .venv
source .venv/bin/activate
```

### 5.3 Instalar dependencias

```bash
pip install pandas numpy matplotlib seaborn plotly requests
```

> **Nota:** Si deseas fijar versiones exactas para reproducibilidad, genera un archivo `requirements.txt`:
> ```bash
> pip freeze > requirements.txt
> pip install -r requirements.txt
> ```

---

## 6. Ejecución del Proyecto

### Opción A — Visual Studio Code

1. Abrir la carpeta del proyecto en VS Code.
2. Instalar la extensión **Jupyter** de Microsoft (si no está instalada).
3. Abrir `telecomx.ipynb`.
4. Seleccionar el kernel `.venv` (Python).
5. Ejecutar todas las celdas: `Ctrl + Shift + P` → *Jupyter: Run All Cells*.

### Opción B — Jupyter Notebook / JupyterLab en terminal

```bash
# Asegurar que el entorno virtual está activado
jupyter notebook telecomx.ipynb
# o bien
jupyter lab telecomx.ipynb
```

En el navegador, ir a `Kernel → Restart Kernel and Run All Cells`.

### Opción C — Google Colab

1. En Colab: `Archivo → Subir notebook` → seleccionar `telecomx.ipynb`.
2. El dataset se descarga automáticamente desde la URL pública; no se requiere carga manual de archivos.

---

## 7. Metodología

El notebook sigue el pipeline estándar de ciencia de datos alineado con **CRISP-DM** (Cross-Industry Standard Process for Data Mining):

```
┌──────────────────────────────────────────────────────────────┐
│  1. Extracción     │  Descarga del JSON via requests · json_normalize  │
│  2. Exploración    │  .info() · .describe() · .value_counts()          │
│  3. Limpieza       │  Nulos en Evasor · Tipos de dato · Renombrado      │
│  4. Transformación │  Booleans · Categories · Float · String            │
│  5. EDA            │  Countplots · Barplots · Boxplots                  │
│  6. Insights       │  Tasas de evasión por variable                    │
│  7. Conclusiones   │  Ranking de variables predictivas                  │
│  8. Recomendaciones│  Acciones estratégicas por segmento                │
└──────────────────────────────────────────────────────────────┘
```

### Tipos de análisis realizados

| Tipo de variable | Técnica de visualización | Variables analizadas |
|---|---|---|
| Categórica nominal | `sns.countplot` agrupado por `Evasor` | Género, Tipo_Contrato, Método_Pago |
| Categórica ordinal binaria | `sns.countplot` | Con_Pareja, Con_Dependientes |
| Booleana (servicio) | `sns.barplot` de proporción | 9 servicios contratados |
| Numérica continua | `sns.boxplot` | Cargos_Mensuales, Cargos_Totales, Meses_como_cliente |
| Numérica discreta | `sns.boxplot` | Numero_Servicios |

---

## 8. Variables del Dataset

| Nombre en el análisis            | Tipo        | Descripción                                                   |
|----------------------------------|-------------|---------------------------------------------------------------|
| `ID_Cliente`                     | string      | Identificador único del cliente                               |
| `Evasor`                         | boolean     | **Variable objetivo** — `True` si canceló el servicio         |
| `Género`                         | category    | `Hombre` / `Mujer`                                            |
| `Adulto_Mayor`                   | int64       | 1 si el cliente es adulto mayor, 0 si no                     |
| `Con_Pareja`                     | boolean     | Tiene pareja o conviviente                                    |
| `Con_Dependientes`               | boolean     | Tiene personas a cargo                                        |
| `Meses_como_cliente`             | int64       | Antigüedad en meses                                           |
| `Servicio_Telefonico`            | boolean     | Dispone de servicio telefónico                                |
| `Multiples_Líneas_Telefonicas`   | boolean     | Tiene más de una línea telefónica                             |
| `Servicio_Internet`              | boolean     | Tiene servicio de internet (DSL o fibra óptica)               |
| `Seguridad_Online`               | boolean     | Servicio de seguridad en línea activo                         |
| `Copia_Seguridad_Online`         | boolean     | Servicio de backup en la nube activo                          |
| `Protección_Dispositivos`        | boolean     | Seguro de dispositivos activo                                  |
| `Soporte_Técnico`                | boolean     | Acceso a soporte técnico prioritario                          |
| `Servicio_Streaming_TV`          | boolean     | Suscripción a streaming de televisión                         |
| `Servicio_Streaming`             | boolean     | Suscripción a streaming de películas                          |
| `Tipo_Contrato`                  | category    | `Month-to-month` / `One year` / `Two year`                   |
| `Facturación_Sin_Papel`          | boolean     | Factura electrónica activada                                  |
| `Método_Pago`                    | category    | Forma de pago (4 opciones)                                    |
| `Cargos_Mensuales`               | float64     | Monto mensual facturado (USD)                                 |
| `Cargos_Totales`                 | float64     | Acumulado total facturado (USD)                               |

---

## 9. Resultados Principales

### Variables con mayor poder predictivo

| Variable | No Evasor | Evasor | Diferencia |
|---|---|---|---|
| Tipo_Contrato (Month-to-month) | — | **42.7%** | El más determinante |
| Meses_como_cliente (media) | 37.6 meses | 18.0 meses | −52% |
| Método_Pago (Electronic check) | — | **45.3%** | +29 pp sobre métodos automáticos |
| Soporte_Técnico (adopción) | 33.5% | 16.6% | −50% en evasores |
| Seguridad_Online (adopción) | 33.3% | 15.8% | −53% en evasores |
| Con_Dependientes (False) | — | **31.3%** | vs. 15.5% con dependientes |
| Cargos_Mensuales (media) | $61.3 | $74.4 | +$13/mes |

### Variables sin impacto relevante

- **Género**: diferencia < 1 pp (Hombre 26.2% vs. Mujer 26.9%).
- **Número de servicios**: diferencia de 0.1 servicios (4.17 vs. 4.07) — la *cantidad* no discrimina, sí el *tipo*.

---

## 10. Problemas Conocidos y Soluciones

| Problema | Causa | Solución |
|---|---|---|
| `AttributeError: Can only use .cat accessor with a 'category' dtype` | La columna no se convirtió a `category` antes de llamar `.cat.categories` | Usar `.unique()` en su lugar o re-ejecutar la celda de transformación de tipos |
| `KeyError: 'Numero_Servicios'` al ejecutar la última celda | La celda de creación de la columna no se ejecutó | Ejecutar las celdas en orden secuencial desde el inicio |
| Descarga del dataset falla (timeout / sin conexión) | El entorno no tiene acceso a internet | Descargar manualmente el archivo JSON y cargarlo con `pd.read_json('TelecomX_Data.json')` |
| Kernel muerto en VS Code | Conflicto de entorno o falta de memoria | Verificar que el entorno `.venv` está seleccionado y tiene las dependencias instaladas |
| `FutureWarning` en `groupby().mean()` con columnas booleanas | Versión de pandas ≥ 2.0 | Agregar `.astype(float)` antes del `.mean()` o ignorar la advertencia; no afecta los resultados |

---

## 11. Estándares y Buenas Prácticas

Este proyecto sigue las siguientes referencias normativas:

| Norma / Estándar | Aplicación en el proyecto |
|---|---|
| **ISO/IEC 25010:2011** — Calidad del software | Mantenibilidad: código estructurado en funciones reutilizables (`plot_categorica`, `plot_boolean`, `plot_numerica`) |
| **ISO/IEC 25012:2008** — Calidad de datos | Completitud: tratamiento explícito de valores nulos; Consistencia: conversión y validación de tipos de dato |
| **IEEE Std 1012-2016** — Verificación y validación | Verificación de resultados en cada etapa (`.info()`, `.describe()`, `.value_counts()`) |
| **CRISP-DM** (ISO/IEC TR 29119-4) | Estructura del análisis: comprensión del negocio → datos → preparación → modelado → evaluación |
| **PEP 8** (Python) | Convenciones de estilo: nombres descriptivos en español, funciones bien delimitadas, comentarios en línea |
| **Reproducibilidad (principio FAIR)** | Las celdas están ordenadas secuencialmente y el dataset se recarga desde una URL pública fija |

---

## 12. Licencia

Este proyecto se desarrolla con fines **educativos** en el marco del programa Alura LATAM — Challenge 2. El uso del dataset está sujeto a los términos del repositorio fuente.

---

*Documentación generada conforme a IEEE Std 830-1998 (Especificación de Requisitos de Software) y las directrices de documentación de proyectos de ciencia de datos de la comunidad de práctica de Alura LATAM.*
