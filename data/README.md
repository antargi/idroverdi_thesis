# Documentación - Datos

## Acceso a Datos Completos

Esta carpeta contiene **metadatos** y referencias a los datos procesados.

### Estructura de datos principal

```
/home/aninotna/magister/tesis/justh2_pipeline/data/

├── cr2met/                    # CR2MET histórico (1980-2014)
│   └── clima.zarr/
│
├── cmip6/                     # CMIP6 regridded y procesados
│   ├── regridded/
│   ├── bias_params/
│   └── corrected/
│
├── autoencoder_trained/       # Modelos entrenados
│   └── model_vae_best.pt
│
├── autoencoder_results/       # Salidas principales
│   ├── clusters_base.npy
│   ├── clusters_ssp245.npy
│   ├── irct_pixel_ssp245.npy
│   └── cluster_profiles.json
│
└── h2v_production_spatial/    # Producción H₂ Calliope
    └── h2_production_*.nc
```

### Acceso a datos

Para reproducir análisis:

```python
import numpy as np
import xarray as xr
from pathlib import Path

# Ruta base
DATA_PATH = Path('/home/aninotna/magister/tesis/justh2_pipeline/data')

# Cargar datos históricos
ds_historical = xr.open_dataset(DATA_PATH / 'cr2met/clima.zarr')

# Cargar clustering
clusters = np.load(DATA_PATH / 'autoencoder_results/clusters_ssp245.npy')

# Cargar IRCT
irct = np.load(DATA_PATH / 'autoencoder_results/irct_pixel_ssp245.npy')
```

### Volumen aproximado

| Dataset | Tamaño | Formato |
|---------|--------|---------|
| CR2MET histórico | 4.2 GB | zarr |
| CMIP6 regridded | ~50 GB | netCDF |
| Modelos AE | 45 MB | PyTorch |
| Resultados IRCT | 250 MB | npy + json |

### Contacto para Acceso

Para solicitar datos completos (no incluidos en repo por tamaño):
- aninotna@gmail.com
- Especificar: qué dataset, período, variables

---

**Ir a:** [README.md](../README.md)
