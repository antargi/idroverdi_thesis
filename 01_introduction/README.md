# 1. Introducción

## Contexto y Motivación

El Valle de Aconcagua en Chile enfrenta cambios climáticos que impactan su capacidad productiva energética. Este estudio desarrolla un **framework computacional** para caracterizar la resiliencia climática territorial integrando:

1. **Datos climáticos multivariados** (CR2MET + CMIP6)
2. **Modelado energético** (Calliope - producción de hidrógeno verde)
3. **Deep learning no supervisado** (autoencoders para reducción dimensional)
4. **Análisis territorial** (clustering espacial + métricas de resiliencia)

---

## Pregunta de Investigación

**¿Cómo caracterizar zonas con distinta resiliencia climática en el Valle de Aconcagua usando variables climáticas históricas y proyectadas (CMIP6), autoencoders no supervisados y clusterización?**

### Hipótesis Principal

Un autoencoder (AE/VAE) no supervisado entrenado en datos climáticos históricos permite identificar agrupamientos espaciales que reflejan diferentes niveles de **resiliencia climática territorial** cuando se aplica a proyecciones futuras.

---

## Objetivos

### Objetivo General
Desarrollar un modelo computacional integrado que combine datos climáticos, modelado energético y machine learning para identificar y caracterizar zonas de resiliencia climática en el Valle de Aconcagua.

### Objetivos Específicos

1. **Construcción base de datos climáticos multivariados**
   - Integrar CR2MET (1980-2014) con CMIP6 (1950-2100)
   - Seleccionar variables relevantes para resiliencia (temperatura, precipitación, extremos)
   - Preprocesamiento y normalización

2. **Desarrollo del modelo de representación**
   - Entrenar autoencoders (AE/VAE) en espacio latente
   - Validar reconstrucción espacial
   - Optimizar arquitectura y dimensionalidad latente

3. **Clustering territorial**
   - Aplicar K-means en espacio latente (k=3 a 10)
   - Validar coherencia espacial de clusters
   - Perfilar clusters con variables climáticas

4. **Cálculo de Índice de Resiliencia Climático-Territorial (IRCT)**
   - Definir componentes: persistencia, identidad, cohesión, elasticidad
   - Calcular IRCT pixel-wise para escenarios futuros
   - Análisis de sensibilidad de pesos

5. **Validación y análisis de resultados**
   - Evaluación de coherencia espacial
   - Estabilidad temporal
   - Relevancia climática y energética

---

## Marco Conceptual

### Resiliencia en Sistemas Complejos

**Definición**: Capacidad de un sistema de mantener estructura, función e identidad frente a perturbaciones (Holling, 1973; Walker et al., 2004).

**Dimensiones de resiliencia territorial:**
1. **Resistencia**: Capacidad de absorber cambios
2. **Adaptabilidad**: Ajuste a nuevas condiciones
3. **Transformabilidad**: Cambio hacia nuevo régimen

### Espacio Latente como Proxy de Complejidad Climática

El autoencoder captura patrones climáticos complejos en baja dimensionalidad, permitiendo:
- Identificar regímenes climáticos similares espacialmente
- Cuantificar cambios entre períodos históricos y futuros
- Medir "distancia climática" entre zonas

---

## Novedad Metodológica

Esta tesis integra tres campos que típicamente se estudian por separado:

| Campo | Aporte | Novedad |
|-------|--------|---------|
| Climate science | Datos CR2MET + CMIP6 | Análisis integrado histórico-futuro |
| Machine Learning | AE/VAE no supervisado | Representación latente de complejidad climática |
| Energía | Calliope modeling | Coupling resiliencia-hidrógeno verde |
| Geography | Análisis territorial | Clustering en espacio latente para resiliencia |

---

## Estructura de la Tesis

### Secciones Principales

1. **Exploración de datos** (Notebooks 00-02)
   - Stack de variables: temperatura, precipitación, indices extremos
   - Estadísticas espaciales y temporales

2. **Modelado** (Notebooks 03)
   - Arquitectura AE/VAE
   - Entrenamiento en histórico
   - Proyección a futuros

3. **Clustering y caracterización** (Notebooks 07-09)
   - Clustering en espacio latente
   - Perfilado de clusters
   - Cálculo de IRCT

4. **Validación** (Notebooks 11-13)
   - Métricas de clustering
   - Análisis de sensibilidad
   - Benchmarking

---

## Datos de Entrada

| Dataset | Variables | Período | Resolución | Uso |
|---------|-----------|---------|------------|-----|
| CR2MET | T, Precip., Humedad | 1980-2014 | 5 km | Entrenamiento |
| CMIP6 | T, Precip. (regridded) | 1950-2100 | 5 km | Proyección futura |
| SRTM DEM | Elevación | Static | 30 m | Análisis topográfico |
| Calliope | H₂ producción | 2040-2069 | 5 km | Contexto energético |

---

## Métrica Principal: IRCT

El **Índice de Resiliencia Climático-Territorial** integra:

$$IRCT_{pixel,escenario} = 0.30 \cdot S_D + 0.25 \cdot S_C + 0.20 \cdot S_E + 0.20 \cdot S_{EL} + 0.05 \cdot H_T$$

**Componentes:**

| Componente | Símbolo | Fórmula |
|-----------|---------|---------|
| **Persistencia** | $S_D$ | $1 - \frac{\|\|z_{future} - z_{base}\|\|}{\|\|z_{base}\|\|}$ |
| **Identidad** | $S_C$ | $P(\text{cluster}_{\text{future}} = \text{cluster}_{\text{base}})$ |
| **Cohesión** | $S_E$ | $\frac{\sigma_{base}}{\sigma_{future}}$ (↑ resiliente) |
| **Elasticidad** | $S_{EL}$ | Velocidad de re-estabilización |
| **Horizontes** | $H_T$ | Factor de largo plazo |

**Interpretación:**
- IRCT ∈ [0,1]: 1 = máxima resiliencia, 0 = vulnerabilidad crítica
- Spatial map: Identifica zonas resilientes vs vulnerables

---

## Contribuciones Esperadas

1. **Teórica**: Framework integrado para resiliencia climático-territorial
2. **Metodológica**: Uso de autoencoders para reducción dimensional climática
3. **Aplicada**: Identificación de zonas de riesgo energético en Chile
4. **Reproducible**: Pipeline completo documentado y automatizado

---

## Próximas Secciones

→ [02_data_exploration/](../02_data_exploration/) - Exploración del stack climático
