# Detección de Tráfico Web Anómalo - Mercado Libre

## Descripción del Problema

Este proyecto aborda el desafío de detectar tráfico web automatizado (bots, scrapers) en la plataforma de Mercado Libre. El tráfico automatizado puede generar costos innecesarios en infraestructura, comprometer la integridad de datos, y afectar la experiencia de usuario en la plataforma.

### Objetivos
- Diseñar e implementar una solución de machine learning para identificar tráfico web anómalo
- Desarrollar un sistema capaz de distinguir entre comportamiento humano y automatizado
- Proporcionar explicaciones claras sobre los patrones de tráfico detectados
- Sugerir acciones de mitigación basadas en el tipo de tráfico automatizado

## Metodología

El proyecto sigue un enfoque híbrido con múltiples capas de detección:

1. **Exploración y Análisis de Datos**
   - Análisis de patrones temporales (distribución por hora, día, intervalos entre solicitudes)
   - Investigación de User-Agents y URLs visitadas
   - Identificación de características distintivas entre tráfico normal y anómalo

2. **Ingeniería de Características**
   - Desarrollo de features temporales (uniformidad de intervalos, velocidad de navegación)
   - Características de comportamiento (entropía de navegación, patrones repetitivos)
   - Indicadores técnicos (User-Agent, tamaño de respuestas, códigos de error)

3. **Modelado Multi-capa**
   - **Capa 1**: Sistema basado en reglas para casos evidentes
   - **Capa 2**: Detección de anomalías no supervisada (Isolation Forest, HBOS, KNN)
   - **Capa 3**: Modelo supervisado XGBoost entrenado con etiquetas sintéticas

4. **Aplicación de GenAI**
   - Implementación de LLMs para análisis semántico y explicabilidad
   - Generación de reglas adaptativas basadas en patrones emergentes
   - Contextualización de alertas para equipo de seguridad

## Resultados Principales

### Métricas de Desempeño
- **Precisión**: 95.27%
- **Recall**: 92.07%
- **F1-Score**: 93.64%
- **Accuracy**: 96.78%
- **AUC-ROC**: 0.994

### Hallazgos Clave

1. **Identificación de perfiles de tráfico automatizado**:
   - **Scrapers de alta velocidad** (7.28%): Caracterizados por user-agents de navegadores reales y operación en horario laboral
   - **Bots API** (61.86%): Solicitudes directas sin navegador convencional, con alta predictibilidad en patrones de acceso
   - **Crawlers lentos/distribuidos** (29.78%): Uso intensivo de parámetros de búsqueda y consulta, comportamiento sistemático
   - **Bots de navegación repetitiva** (1.08%): Alta variabilidad en patrones temporales simulando comportamiento humano

2. **Estrategia de evasión sofisticada detectada**:
   - 99.64% de las sesiones realizan una única solicitud, una estrategia para evadir detección basada en comportamiento
   - En las sesiones multi-solicitud (0.36%), se identificaron intervalos temporales con uniformidad algorítmica incompatible con navegación humana
   - Identificación de user-agents automatizados (python-requests, Go-http-client, Scrapy) que representan aproximadamente el 32% del tráfico

3. **Distribución geográfica significativa**:
   - El código de país desconocido ("??") representa más del 50% del tráfico automatizado
   - Distribución sospechosamente uniforme entre países legítimos (12-13% cada uno)

## Implicaciones para la Seguridad

- El sistema híbrido propuesto permite detección efectiva tanto de bots tradicionales como de aquellos que utilizan tácticas evasivas avanzadas
- Las características temporales (intervalos entre solicitudes) y de comportamiento (patrones de navegación) demostraron ser los indicadores más potentes
- Las técnicas de evasión identificadas sugieren la necesidad de enfoques adaptativos que evolucionen con las estrategias de los atacantes

## Presentación de Resultados

Para facilitar la visualización de los resultados, se ha creado un dashboard interactivo HTML 
(disponible en `/dashboard/dashboard(3).html`) que muestra las principales métricas, distribuciones 
y hallazgos del análisis. Este dashboard funciona localmente sin necesidad de servidor y 
proporciona una interfaz completa para explorar los diferentes perfiles de bots detectados.


## Limitaciones y Trabajo Futuro

### Limitaciones
- Posible sobreajuste debido al alto rendimiento del modelo (AUC cercano a 1.0)
- Generación de etiquetas sintéticas que podrían no representar perfectamente comportamientos del mundo real
- Calibración de probabilidades que podría mejorarse para reflejar confianza real

### Trabajo Futuro
- Implementación de validación con datos de producción etiquetados manualmente
- Desarrollo de mecanismos de aprendizaje continuo y adaptación a nuevas técnicas de evasión
- Integración con sistemas de mitigación (CAPTCHA, rate limiting, bloqueo selectivo)
- Refinamiento de umbrales de clasificación para optimizar el balance entre seguridad y experiencia de usuario

## Estructura del Repositorio

- `notebooks/bot_detection_analysis.ipynb`: Notebook principal con todo el análisis
- `data/`: Contiene el dataset utilizado
- `images/`: Visualizaciones generadas durante el análisis
- `dashboard/dashboard(3).html`: Dashboard interactivo que visualiza los resultados clave del análisis
- `react-app/`: Contiene el prototipo de aplicación desarrollado en CodeSandbox con React y JavaScript para el punto 6 (aplicación de GenAI en la detección y bloqueo de intrusiones)
- `requirements.txt`: Dependencias necesarias para reproducir el análisis

## Cómo Utilizar

1. Clonar el repositorio
2. Instalar dependencias: `pip install -r requirements.txt`
3. Abrir el notebook: `jupyter notebook notebooks/PRUEBA_CIBERSEGURIDAD_MELI.ipynb`
4. Para visualizar el prototipo de aplicación GenAI:
   - Visitar directamente la versión online: [https://lz2lj7.csb.app/](https://lz2lj7.csb.app/)
   - O abrir el archivo HTML en `react-app/` en un navegador web

## Autor

Andres Felipe Montoya Morales  
Científico de Datos  
WhatsApp: 3116225298
