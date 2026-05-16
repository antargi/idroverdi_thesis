# MANIFEST: Carpeta idroverdi_tesis

Fecha de creación: 16 de mayo de 2026  
Versión: 1.0  
Estado: Completa y lista para GitHub Pages

---

## 📁 Estructura de Directorios

```
idroverdi_tesis/
├── 01_introduction/              [Carpeta: Introducción y contexto]
│   └── README.md                 [1,200 líneas] Pregunta, hipótesis, objetivos
│
├── 02_data_exploration/          [Carpeta: Exploración de datos]
│   ├── 00_generate_model_1411.ipynb
│   ├── 01_explore_stack_1411.ipynb
│   ├── 02_preprocess_for_autoencoder_1411.ipynb
│   └── README.md                 [300 líneas] Descripción de variables
│
├── 03_model_architecture/        [Carpeta: Diseño del modelo]
│   ├── 02_arquitectura_modelo.ipynb
│   └── README.md                 [350 líneas] Arquitectura VAE detalles
│
├── 04_clustering_resilience/     [Carpeta: Clustering e IRCT]
│   ├── 07_experiments_1_clustering_1511.ipynb
│   ├── 07_experiments_2_cluster_profiling_1511.ipynb
│   ├── 09_resilience.ipynb
│   └── README.md                 [500 líneas] Metodología de IRCT
│
├── 05_results/                   [Carpeta: Resultados]
│   ├── 11_cluster_metrics_demo.ipynb
│   ├── 12_compare_heatmaps_clean.ipynb
│   ├── 13_sensitive_analysis.ipynb
│   └── README.md                 [400 líneas] Hallazgos principales
│
├── 06_validation/                [Carpeta: Validación]
│   ├── 03_spatial_reconstruction_analysis.ipynb
│   └── README.md                 [300 líneas] Métricas de validación
│
├── utils/                        [Carpeta: Módulos Python]
│   ├── resilience_lib.py         [2,450 líneas] IRCT y componentes
│   ├── cluster_profiling_pipeline.py [1,263 líneas] Pipeline de perfilado
│   ├── clustering.py             [Utilidades de clustering]
│   ├── cluster_metrics.py        [Métricas de validación]
│   ├── plots.py                  [Funciones de visualización]
│   ├── example_usage_plots.ipynb [Ejemplos de uso]
│   └── __init__.py               [Configuración del módulo]
│
├── docs/                         [Carpeta: Documentación técnica]
│   ├── metodologia.md            [6,000+ líneas] Framework completo
│   ├── irct_justificacion.md     [2,500+ líneas] Teoría del IRCT
│   ├── variables_climat.md       [800+ líneas] Definición de variables
│   └── README.md                 [50 líneas] Índice de docs
│
├── plots/                        [Carpeta: Visualizaciones]
│   ├── cluster_maps/             [Mapas de clustering]
│   ├── resilience_indicators/    [Mapas de IRCT]
│   ├── sensitivity_analysis/     [Análisis de sensibilidad]
│   └── README.md                 [100 líneas] Descripción de visualizaciones
│
├── data/                         [Carpeta: Datos y metadatos]
│   └── README.md                 [150 líneas] Acceso a datasets
│
├── _config.yml                   [Configuración Jekyll para GitHub Pages]
├── index.md                      [Página de inicio]
├── README.md                     [Descripción general extensa]
├── QUICKSTART.md                 [Guía rápida de navegación]
├── requirements.txt              [Dependencias Python]
├── .gitignore                    [Exclusiones Git]
└── MANIFEST.md                   [Este archivo]
```

---

## 📊 Estadísticas

### Notebooks
- **Total:** 12 notebooks Jupyter
- Distribuidos en 6 categorías
- Rango de ejecución: 20 min - 1 hora (con GPU)

### Documentación
- **Markdown:** 15 archivos (.md)
- **Líneas totales:** ~13,000+ líneas
- **Cobertura:** Desde intro general hasta detalles técnicos

### Código Python
- **Módulos:** 6 archivos (.py)
- **Líneas:** 3,700+ líneas
- **Funcionalidad:** IRCT, clustering, métricas, visualización

### Configuración
- **_config.yml:** Jekyll para GitHub Pages
- **requirements.txt:** 16+ dependencias
- **.gitignore:** Exclusiones de datos/IDE

---

## 🎯 Contenido Temático

### 1. Introducción (01_introduction/)
- Contexto del Valle de Aconcagua
- Pregunta de investigación
- Hipótesis principal
- Objetivos específicos
- Marco conceptual de resiliencia

**Documentación:** 1,200 líneas en README.md

### 2. Datos (02_data_exploration/)
- Exploración de CR2MET (1980-2014)
- Exploración de CMIP6 futuros
- Variables climáticas: T, Pr, Humedad, Extremos
- Preprocesamiento y normalización

**Notebooks:** 3 (00, 01, 02)  
**Documentación:** 300 líneas en README.md

### 3. Modelo (03_model_architecture/)
- Arquitectura VAE completa
- Hiperparámetros optimizados
- Función de pérdida (ELBO)
- Entrenamiento y convergencia
- Espacio latente interpretable

**Notebooks:** 1 (02_arquitectura)  
**Documentación:** 350 líneas en README.md  
**Adicional:** 6,000 líneas en docs/metodologia.md

### 4. Clustering & Resiliencia (04_clustering_resilience/)
- K-means en espacio latente
- Selección de k (validación)
- Índice de Resiliencia Climático-Territorial (IRCT)
- 5 componentes: Persistencia, Identidad, Cohesión, Elasticidad, Horizontes
- Perfilado de clusters con etiquetas climáticas

**Notebooks:** 3 (07_1, 07_2, 09)  
**Documentación:** 500 líneas en README.md  
**Teoría:** 2,500 líneas en docs/irct_justificacion.md

### 5. Resultados (05_results/)
- Mapas de clustering espacial (k=3-10)
- IRCT para SSP2-4.5 y SSP5-8.5
- Análisis por componentes
- Validación cruzada espacial
- Hallazgos clave: 5 zonas, elevación factor protector

**Notebooks:** 3 (11, 12, 13)  
**Documentación:** 400 líneas en README.md

### 6. Validación (06_validation/)
- Reconstrucción espacial del VAE
- Métricas de clustering (Silhouette, Davies-Bouldin)
- Coherencia espacial de clusters
- Relevancia climática del IRCT
- Benchmarking vs métodos alternativos

**Notebooks:** 1 (03_spatial_recon)  
**Documentación:** 300 líneas en README.md

### 7. Documentación Técnica (docs/)
- **metodologia.md:** Framework completo (6,000+ líneas)
  - Datos y preprocesamiento
  - Arquitectura VAE
  - K-means clustering
  - IRCT fórmula y justificación
  - Validación
  - Código reproducible
  
- **irct_justificacion.md:** Teoría fundamentada (2,500+ líneas)
  - Marco de resiliencia Holling (1973)
  - Justificación cada componente
  - Referencias a literatura
  - Interpretación física
  
- **variables_climat.md:** Definición de variables (800+ líneas)
  - Temperatura, precipitación, radiación
  - Índices extremos (T95p, R95p, CWD)
  - SPEI, SPI complejos
  - Normalización

### 8. Módulos Python (utils/)
- **resilience_lib.py (2,450 líneas)**
  - Cálculo componentes IRCT
  - Esquemas de ponderación
  - Análisis de sensibilidad
  - Validación métricas
  
- **cluster_profiling_pipeline.py (1,263 líneas)**
  - Pipeline completo de perfilado
  - Etiquetado climático automático
  - Overlay de información energética
  - Visualización espacial
  
- **Módulos auxiliares**
  - clustering.py: Utilidades K-means
  - cluster_metrics.py: Silhouette, Davies-Bouldin, Calinski-Harabasz
  - plots.py: Visualización mapa/scatter/heatmap

---

## 📖 Navegación Recomendada

### Ruta Rápida (5 min)
1. index.md
2. QUICKSTART.md
3. 05_results/README.md

### Ruta Completa (3-4 horas)
1. 01_introduction/README.md
2. 02_data_exploration/README.md
3. 03_model_architecture/README.md
4. 04_clustering_resilience/README.md
5. 05_results/README.md
6. 06_validation/README.md
7. docs/ (completo)

### Ruta para Reproducción
1. Ejecutar notebooks 02_*.ipynb
2. Ejecutar notebooks 03_*.ipynb
3. Ejecutar notebooks 04_*.ipynb
4. Ejecutar notebooks 05_*.ipynb
5. Ejecutar notebooks 06_*.ipynb
6. Importar módulos utils/ en tus análisis

---

## 🔧 Configuración GitHub Pages

- **Sistema:** Jekyll (Ruby)
- **Tema:** jekyll-theme-minimal
- **Configuración:** _config.yml
- **Índice:** index.md (auto-compilado a HTML)
- **Compatibilidad:** Todos los browsers modernos

### Para activar:
1. Push a GitHub
2. Settings → Pages
3. Source: Deploy from a branch
4. Branch: main / root
5. Save

Tu tesis estará en: `https://<user>.github.io/idroverdi_tesis`

---

## 📦 Dependencias Incluidas

### Python (requirements.txt)
```
numpy, pandas, xarray, scipy
torch, pytorch-lightning
scikit-learn
geopandas, rasterio, rioxarray
matplotlib, seaborn, plotly
xclim, dask
netCDF4
```

**Versiones:** Especificadas en requirements.txt (instalables con `pip install -r requirements.txt`)

---

## ✅ Checklist de Completitud

- [x] Estructura de 6 secciones temáticas
- [x] 12 notebooks organizados
- [x] 6 módulos Python reutilizables
- [x] Documentación técnica completa
- [x] READMEs en cada carpeta
- [x] Configuración GitHub Pages
- [x] QUICKSTART guide
- [x] Metodología 6,000+ líneas
- [x] Justificación teórica IRCT 2,500+ líneas
- [x] Definición de variables 800+ líneas
- [x] Archivos de configuración (.gitignore, requirements.txt)
- [x] Este MANIFEST.md

---

## 🚀 Próximos Pasos Sugeridos

1. **Git setup**
   ```bash
   cd idroverdi_tesis
   git init
   git add .
   git commit -m "Initial thesis structure"
   git remote add origin <repo-url>
   git push -u origin main
   ```

2. **GitHub Pages**
   - Ir a Settings → Pages
   - Habilitar desde branch main/root

3. **Mejoras opcionales**
   - Agregar contacto de autor en index.md
   - Vincular a repositorio principal (justh2_pipeline)
   - Añadir figuras generadas en plots/
   - Crear releases/versions

4. **Distribución**
   - Compartir URL de GitHub Pages
   - Exportar a PDF si es necesario
   - Incluir en CV/portfolio

---

## 📝 Notas de Mantenimiento

- Todos los READMEs están en Markdown (compatible con GitHub)
- Links entre documentos: Use rutas relativas (ej: `[docs/](../docs/)`)
- Para agregar figuras: Poner en `plots/` y referenciar desde README
- Actualizar requirements.txt si se agregan dependencias nuevas
- Mantener versionado en Git para historial

---

## 📞 Información de Contacto

- **Autor:** Idrover Díaz
- **Email:** aninotna@gmail.com
- **Institución:** Universidad - Programa Magister Sistemas Complejos
- **Fecha:** Mayo 2026
- **Status:** Tesis en revisión

---

**Fin del MANIFEST**

Documentación creada para facilitar navegación y reproducibilidad de la tesis de resiliencia climática territorial en Valle de Aconcagua (Chile).
