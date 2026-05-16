# 2. Exploración de Datos

## Overview

Esta sección documenta el exploración exploratorio del stack de datos climáticos, incluyendo:

- **Fuentes de datos**: CR2MET (histórico), CMIP6 (futuro)
- **Variables climáticas**: Temperatura, precipitación, humedad, indices extremos
- **Estadísticas espaciales**: Mapas de media, varianza, tendencias
- **Análisis temporal**: Ciclos anuales, cambios interdecadales

---

## Notebooks en esta Sección

| # | Nombre | Descripción |
|---|--------|-------------|
| 00 | [00_generate_model_1411.ipynb](00_generate_model_1411.ipynb) | Generación y validación del stack de datos base |
| 01 | [01_explore_stack_1411.ipynb](01_explore_stack_1411.ipynb) | Exploración visual de variables por región |
| 02 | [02_preprocess_for_autoencoder_1411.ipynb](02_preprocess_for_autoencoder_1411.ipynb) | Preparación de datos para autoencoder |

---

## Variables Principales

### Temperatura
- **tasmin**: Temperatura mínima diaria [°C]
- **tasmax**: Temperatura máxima diaria [°C]
- **tas**: Temperatura media [°C]
- **Derivadas**: Amplitud térmica (tasmax - tasmin)

### Precipitación
- **pr**: Precipitación acumulada diaria [mm]
- **Derivadas**: Acumulada estacional, días secos consecutivos

### Extremos Climáticos
- **T95p**: Percentil 95 de temperatura (eventos cálidos)
- **T5p**: Percentil 5 de temperatura (eventos fríos)
- **R95p**: Percentil 95 de precipitación (eventos húmedos)
- **CWD**: Consecutive Wet Days (sequía agrícola)

### Índices Complejos
- **SPEI**: Standardized Precipitation-Evapotranspiration Index
- **SPI**: Standardized Precipitation Index

---

## Características del Dataset

**Espacial:**
- Resolución: 5 km (regridded desde CMIP6 ~100 km)
- Dominio: Valle de Aconcagua (lat: -33.27 a -32.26, lon: -71.89 a -70.00)
- Total de pixels: ~1,500 grid points

**Temporal:**
- Historical: 1980-2014 (35 años, 12,775 días)
- Future: 2040-2069 (30 años, 10,960 días)
- Frecuencia: Diaria

**Modelos CMIP6 incluidos:**
- ACCESS-ESM1-5
- CNRM-CM6-1
- IPSL-CM6A-LR
- MRI-ESM2-0
- Otros ~10 modelos

---

## Hallazgos Clave (Exploración)

1. **Gradientes espaciales:**
   - Temperaturas: Decrecen con elevación (~6.5°C/km)
   - Precipitación: Máxima en cordillera, mínima en valle

2. **Variabilidad temporal:**
   - Ciclo anual bien definido (amplit. 20-25°C)
   - ENSO modulación visible en precipitación

3. **Cambios futuro-presente:**
   - Incremento T: 2-4°C (SSP2-4.5), 3-6°C (SSP5-8.5)
   - Cambio Pr: -5 a -15% (reducción generalizada)
   - Aumento eventos extremos

---

## Preprocesamiento Realizado

1. **Regridding**: CMIP6 (native) → 5 km (xESMF)
2. **Alineación calendárica**: 360-day → 365-day
3. **Normalización**: Z-score por variable
4. **Stacking**: Datos 3D (lat, lon, time) → 2D (space, multivariate_features)

---

## Archivos de Salida

```
data/
├── clima_stack_1980-2014.nc    # Dataset histórico multivariado
├── cmip6_regridded/
│   ├── {MODEL}_1950-2100.nc
│   └── ...
└── processed/
    ├── X_historical_normalized.npy
    ├── X_ssp245_normalized.npy
    └── coords_df.parquet
```

---

## Próximas Secciones

→ [03_model_architecture/](../03_model_architecture/) - Diseño del autoencoder
