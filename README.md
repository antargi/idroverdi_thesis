# Tesis: Resiliencia Climática Territorial - Valle de Hidrógeno

Este repositorio contiene el código, datos y documentación de una **tesis de magister en Sistemas Complejos** que estudia la resiliencia climática territorial del Valle de Aconcagua (Chile) mediante técnicas de deep learning y análisis energético.

## 📋 Contenido

```
idroverdi_tesis/
├── 01_introduction/        → Problema, hipótesis, objetivos
├── 02_data_exploration/    → Exploración del stack climático
├── 03_model_architecture/  → Descripción técnica del AE/VAE
├── 04_clustering_resilience/ → Cálculo del IRCT, clustering
├── 05_results/             → Resultados y visualizaciones
├── 06_validation/          → Validación y benchmarking
├── utils/                  → Módulos Python reutilizables
├── plots/                  → Figuras estáticas
├── docs/                   → Documentación técnica completa
└── data/                   → Metadata y acceso a datos
```

## 🚀 Para Empezar

### Acceso rápido a secciones:
1. **Lectura rápida** (5 min): [index.md](index.md)
2. **Contexto** (15 min): [01_introduction/](01_introduction/)
3. **Metodología completa** (1 hora): [docs/](docs/)
4. **Notebooks interactivos**: Navega por cada sección

### Ambiente Python

```bash
# Clonar y acceder
git clone <repo-url>
cd idroverdi_tesis

# Crear ambiente (conda)
conda create -n idroverdi python=3.10
conda activate idroverdi
pip install -r requirements.txt

# O si prefieres jupyter
jupyter notebook
```

## 📚 Estructura de Lectura Recomendada

**Para no-técnicos:**
- `01_introduction/` → `05_results/README.md` → `docs/resultados_principales.md`

**Para técnicos:**
- `02_data_exploration/` → `03_model_architecture/` → `04_clustering_resilience/` → `06_validation/`

**Para reproducir:**
- Ejecutar notebooks en orden: `02_*.ipynb` → `03_*.ipynb` → `04_*.ipynb` → `05_*.ipynb` → `06_*.ipynb`

## 🔍 Conceptos Clave

### Resiliencia Climática Territorial (RCT)
Capacidad de una región de mantener estructura y función frente a cambios climáticos, medida a través del **Índice de Resiliencia Climático-Territorial (IRCT)**:

$$IRCT = 0.30 \cdot S_D + 0.25 \cdot S_C + 0.20 \cdot S_E + 0.20 \cdot S_{EL} + 0.05 \cdot H_T$$

Donde:
- $S_D$: Persistencia (distancia latente)
- $S_C$: Identidad de régimen
- $S_E$: Cohesión interna
- $S_{EL}$: Elasticidad
- $H_T$: Horizontes temporales

### Metodología

1. **Datos**: CR2MET (1980-2014) + CMIP6 (2040-2069)
2. **Modelo**: Autoencoder/VAE no supervisado
3. **Representación**: Espacio latente de baja dimensión
4. **Clustering**: K-means para identificar zonas
5. **Resiliencia**: IRCT pixel-wise en escenarios futuros

## 📊 Datos

| Variable | Fuente | Período | Resolución |
|----------|--------|---------|------------|
| Temp., Precip. | CR2MET | 1980-2014 | 5 km |
| CMIP6 | ESGF | 1950-2100 | Regridded a 5 km |
| DEM | SRTM | Static | 30 m |
| Producción H₂ | Calliope | 2040-2069 | 5 km |

## 🎯 Objetivos

- [x] Construcción base de datos climáticos multivariados
- [x] Entrenamiento AE/VAE en datos históricos
- [x] Clustering espacial en espacio latente
- [x] Cálculo del IRCT para escenarios futuros
- [x] Validación de coherencia espacial
- [ ] *Análisis de política energética (futuro)*

## 📈 Resultados

**Hallazgos principales:**
- Identificación de 5 zonas con resiliencia diferenciada
- Relación inversa: elevación ↑ = resiliencia ↑
- Producción de H₂ moderadamente resiliente bajo SSP2-4.5
- Vulnerabilidad crítica en SSP5-8.5 en zonas bajas

Ver [05_results/](05_results/README.md) para gráficos y detalles.

## 🛠️ Dependencias Principales

```
numpy, pandas, xarray, scipy      # Data science
torch, pytorch-lightning          # Deep learning
scikit-learn                      # ML
geopandas, rasterio, rioxarray    # Geoespacial
matplotlib, plotly, folium        # Viz
xclim, dask                       # Climate
```

## 📖 Citar esta Tesis

```bibtex
@thesis{diaz2026resilience,
  title={Caracterización de resiliencia climática territorial en un valle 
         de hidrógeno usando datos climáticos, autoencoders y clusterización},
  author={Díaz, Idrover},
  year={2026},
  school={Universidad, Programa Magister Sistemas Complejos}
}
```

## 📧 Contacto

Idrover Díaz - aninotna@gmail.com

---

**Última actualización:** Mayo 2026  
**Status:** Tesis en desarrollo  
**GitHub Pages:** [Ver en línea](https://)
