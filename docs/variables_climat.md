# Variables Climáticas Utilizadas

Definición técnica de cada variable en el stack de datos.

## Variables Primarias (de fuentes CR2MET/CMIP6)

### Temperatura

| Variable | Código | Descripción | Fuente | Resolución |
|----------|--------|-------------|--------|-----------|
| Temp. mínima diaria | tasmin | Temperatura mínima [°C] | CR2MET, CMIP6 | Diaria |
| Temp. máxima diaria | tasmax | Temperatura máxima [°C] | CR2MET, CMIP6 | Diaria |
| Temp. media diaria | tas | (tasmin + tasmax) / 2 | Derivada | Diaria |

### Precipitación

| Variable | Código | Descripción | Fuente | Resolución |
|----------|--------|-------------|--------|-----------|
| Precipitación diaria | pr | Precipitación acumulada [mm/día] | CR2MET, CMIP6 | Diaria |
| Precipitación estacional | pr_seasonal | Suma de pr en trimestre [mm] | Derivada | Trimestral |
| Días secos consecutivos | cwd | Máximo consecutivo sin lluvia | Derivada | Estacional |

### Humedad

| Variable | Código | Descripción | Fuente | Resolución |
|----------|--------|-------------|--------|-----------|
| Humedad relativa | hurs | Humedad relativa [%] | CR2MET | Diaria |

### Radiación

| Variable | Código | Descripción | Fuente | Resolución |
|----------|--------|-------------|--------|-----------|
| Radiación solar | rsds | Radiación solar incidente [W/m²] | CMIP6 (algunos modelos) | Diaria |

---

## Variables Derivadas (Índices de Extremos)

### Índices de Temperatura Extrema

**Percentiles de Temperatura:**

- **T95p**: Percentil 95 de temperatura máxima diaria
  - Definición: Valor de tasmax por encima del cual cae el 5% de días históricos
  - Unidad: °C
  - Interpretación: Eventos de calor extremo
  
- **T5p**: Percentil 5 de temperatura mínima diaria
  - Definición: Valor de tasmin por debajo del cual cae el 5% de días históricos
  - Unidad: °C
  - Interpretación: Eventos de frío extremo

**Amplitud Térmica Extrema:**

- **TAR**: tasmax - tasmin (amplitud diaria)
  - Unidad: °C
  - Interpretación: Continentalidad (amplitud alta = continental, baja = oceánico)

### Índices de Precipitación Extrema

**Percentiles de Precipitación:**

- **R95p**: Percentil 95 de precipitación diaria (solo días con lluvia)
  - Definición: Intensidad de eventos lluviosos extremos
  - Unidad: mm/día
  - Interpretación: Eventos de lluvia intensa

**Duración de Sequías:**

- **CWD**: Consecutive Wet Days
  - Definición: Máximo número consecutivo de días con lluvia < 1 mm
  - Unidad: días
  - Interpretación: Duración de períodos secos (agrícola)

---

## Índices Complejos de Sequía/Humedad

### SPEI (Standardized Precipitation-Evapotranspiration Index)

$$SPEI = \frac{P_i - E_{i} - \bar{(P - E)}}{\sigma_{P-E}}$$

Donde:
- $P_i$: Precipitación en mes i
- $E_i$: Evapotranspiración potencial
- Escalas: SPEI-1 (1 mes), SPEI-3 (3 meses), SPEI-12 (1 año)
- Rango: [-2.5, +2.5]
- Interpretación:
  - SPEI < -1.5: Sequía moderada
  - SPEI < -2: Sequía severa
  - SPEI > +1.5: Período húmedo

### SPI (Standardized Precipitation Index)

$$SPI = \frac{P_i - \bar{P}}{\sigma_P}$$

Donde:
- $P_i$: Precipitación acumulada en período i
- Escalas: SPI-1, SPI-3, SPI-6, SPI-12
- Rango: [-2.5, +2.5]
- Interpretación: Igual que SPEI pero solo considera precipitación

---

## Distribución Temporal en Stack

### Estructura del Dataset Multivariado

Para cada pixel (lat, lon) y día:

```
X[pixel, :] = [tasmin, tasmax, tas, pr, hurs, rsds,
               T95p_monthly, T5p_monthly, TAR_daily,
               R95p_monthly, CWD_monthly,
               SPEI_1, SPEI_3, SPEI_12,
               SPI_1, SPI_3, SPI_6, SPI_12]

Dimensiones:
  - N_pixels = 1,500
  - N_features = 20
  - N_timesteps = 12,775 días (1980-2014)
```

### Frecuencia de Variables

| Grupo | Variables | Frecuencia | Notas |
|-------|-----------|-----------|-------|
| Básicas | tasmin, tasmax, pr, hurs, rsds | Diaria | Fuente: CMIP6/CR2MET |
| Extremos | T95p, T5p, R95p, CWD | Mensual | Calculados sobre días previos |
| Complejos | SPEI, SPI | Mensual | Agregación temporal |

---

## Normalización

Todas las variables normalizadas Z-score independientemente:

$$X_{norm}[i,j] = \frac{X[i,j] - \mu_j}{\sigma_j}$$

Donde:
- $i$: índice de pixel
- $j$: índice de variable
- $\mu_j, \sigma_j$: Media y std de variable j en período histórico (1980-2014)

**Razón:** Evitar que variables con rangos grandes (ej. pr) dominen sobre otras (ej. T)

---

## Glosario de Abreviaciones

| Código | Término Completo | Categoría |
|--------|-----------------|----------|
| tasmin | Temperature minimum | Temperatura |
| tasmax | Temperature maximum | Temperatura |
| tas | Temperature average | Temperatura |
| pr | Precipitation | Precipitación |
| hurs | Humidity relative | Humedad |
| rsds | Radiation solar down shortwave | Radiación |
| T95p | Temperature 95th percentile | Extremos-T |
| T5p | Temperature 5th percentile | Extremos-T |
| TAR | Temperature Amplitude Range | Extremos-T |
| R95p | Rainfall 95th percentile | Extremos-P |
| CWD | Consecutive Wet Days | Extremos-P |
| SPEI | Standardized Precip-Evapotransp Index | Índice complejo |
| SPI | Standardized Precipitation Index | Índice complejo |

---

**Ver también:** [docs/](README.md)
