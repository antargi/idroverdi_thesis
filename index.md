# Caracterización de Resiliencia Climática Territorial en un Valle de Hidrógeno

## 🎯 Pregunta de Investigación

¿Cómo caracterizar zonas con distinta resiliencia climática en el Valle de Aconcagua usando variables climáticas históricas y proyectadas (CMIP6), autoencoders no supervisados y clusterización?

---

## 📚 Estructura de la Tesis

Esta página documenta un estudio integrado que combina:

- **Datos climáticos**: CR2MET históricos + CMIP6 futuros (SSP2-4.5, SSP5-8.5)
- **Modelado energético**: Calliope para simulación de producción de hidrógeno
- **Deep Learning**: Autoencoders (AE/VAE) para representación latente
- **Clustering espacial**: K-means en espacio latente para identificar zonas de resiliencia

---

## 🗂️ Navegación

1. **[Introducción](01_introduction/)** - Contexto, problema, hipótesis
2. **[Exploración de Datos](02_data_exploration/)** - Stack de variables climáticas
3. **[Arquitectura del Modelo](03_model_architecture/)** - Detalles del AE/VAE
4. **[Clustering & Resiliencia](04_clustering_resilience/)** - Índice de Resiliencia Climático-Territorial (IRCT)
5. **[Resultados](05_results/)** - Mapas, métricas, análisis sensible
6. **[Validación](06_validation/)** - Reconstrucción espacial y benchmarking
7. **[Documentación Técnica](docs/)** - Metodología, ecuaciones, referencias

---

## 🔑 Conceptos Principales

### Índice de Resiliencia Climático-Territorial (IRCT)

Métrica integrada que evalúa la capacidad territorial de mantener su estado frente a cambios climáticos futuros:

- **Persistencia**: Distancia en espacio latente (0.30)
- **Identidad de régimen**: Probabilidad de mantener cluster (0.25)
- **Cohesión interna**: Variabilidad interna del sistema (0.20)
- **Elasticidad**: Rapidez de ajuste (0.20)
- **Horizontes temporales**: Factor de resiliencia de largo plazo (0.05)

### Enfoque Metodológico

1. **Entrenamiento**: AE/VAE con datos históricos (1980-2014)
2. **Proyección**: Aplicar modelo a CMIP6 futuros (2040-2069)
3. **Clustering**: K-means en espacio latente para 3-10 clusters
4. **Scoring**: Calcular IRCT pixel-wise mediante cambios en representación latente
5. **Validación**: Coherencia espacial, estabilidad temporal, relevancia climática

---

## 📊 Resultados Principales

- Identificación de **3-5 zonas de resiliencia** en Valle de Aconcagua
- Relación entre **topografía-clima-resiliencia**
- Impacto de **producción de hidrógeno** en vulnerabilidad territorial
- Análisis de **sensibilidad de pesos** del IRCT

---

## 🛠️ Tecnologías

**Python stack:**
- `numpy`, `pandas`, `xarray` - Manipulación de datos multidimensionales
- `pytorch` - Autoencoders
- `scikit-learn` - Clustering, métricas
- `geopandas`, `rasterio` - Análisis espacial
- `matplotlib`, `plotly` - Visualización
- `xclim` - Procesamiento clima

---

## 📖 Cómo Navegar

Cada sección contiene:
- **Notebooks interactivos**: Código reproducible
- **READMEs**: Explicación de contenidos
- **Figuras**: Visualizaciones estáticas
- **Documentación**: Teoría y métodos

Para reproducir análisis, ver instrucciones en [Introducción](01_introduction/).

---

**Tesis de Magister en Sistemas Complejos**  
**Estudiante:** Idrover Díaz  
**2025-2026**
