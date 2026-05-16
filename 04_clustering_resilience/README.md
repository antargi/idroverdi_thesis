# 4. Clustering y Resiliencia

## Objetivo

Identificar **zonas territoriales homogéneas en resiliencia climática** mediante clustering en espacio latente y cálculo del **Índice de Resiliencia Climático-Territorial (IRCT)**.

---

## Metodología: Clustering en Espacio Latente

### Step 1: Proyección al Espacio Latente

```python
# Datos históricos
z_base = encoder(X_historical)        # Shape: (n_pixels, latent_dim)

# Datos futuros (CMIP6)
z_future = encoder(X_cmip6_scenario)  # Igual shape
```

### Step 2: K-means Clustering

Aplicar K-means al espacio latente histórico:

```python
from sklearn.cluster import KMeans

kmeans = KMeans(n_clusters=k, random_state=42)
clusters_base = kmeans.fit_predict(z_base)

# Validar con métricas
silhouette_score = silhouette_score(z_base, clusters_base)
davies_bouldin = davies_bouldin_score(z_base, clusters_base)
```

**Selección de k**: Testear k ∈ [3, 4, 5, 10] con validación cruzada

### Step 3: Asignación Futura

Para cada pixel en escenario futuro:
```python
clusters_future = kmeans.predict(z_future)
```

---

## Notebooks en esta Sección

- [07_experiments_1_clustering_1511.ipynb](https://github.com/antargi/idroverdi_thesis/blob/main/04_clustering_resilience/07_experiments_1_clustering_1511.ipynb) - Clustering con K-means en espacio latente
- [07_experiments_2_cluster_profiling_1511.ipynb](https://github.com/antargi/idroverdi_thesis/blob/main/04_clustering_resilience/07_experiments_2_cluster_profiling_1511.ipynb) - Caracterización de perfiles de clusters
- [09_resilience.ipynb](https://github.com/antargi/idroverdi_thesis/blob/main/04_clustering_resilience/09_resilience.ipynb) - Cálculo del Índice IRCT

---

## Índice de Resiliencia Climático-Territorial (IRCT)

### Definición Conceptual

Métrica integrada que evalúa la capacidad de un pixel territorial de mantener su régimen climático frente a cambios futuros proyectados.

### Fórmula

$$IRCT_{pixel,escenario} = w_D \cdot S_D + w_C \cdot S_C + w_E \cdot S_E + w_{EL} \cdot S_{EL} + w_H \cdot H_T$$

### Componentes

#### 1. **Persistencia** ($S_D$, peso: 0.30)
Capacidad de mantener estructura en espacio latente:

$$S_D = 1 - \frac{||z_{future} - z_{base}||_2}{\max(||z - z_{center}||_2)}$$

- Rango: [0,1]
- 1 = Sin cambio climático
- 0 = Cambio máximo

#### 2. **Identidad de Régimen** ($S_C$, peso: 0.25)
Probabilidad de mantener el cluster asignado:

$$S_C = P(\text{cluster}_{future} = \text{cluster}_{base}) = \begin{cases} 1 & \text{si cluster}_f = \text{cluster}_b \\ 0 & \text{si cluster}_f \neq \text{cluster}_b \end{cases}$$

- Rango: {0, 1}
- 1 = Permanece en mismo cluster
- 0 = Cambio de régimen climático

#### 3. **Cohesión Interna** ($S_E$, peso: 0.20)
Variabilidad interna (dispersión = frágil):

$$S_E = \frac{\sigma_{base}}{\sigma_{future}} = \frac{\text{std}(z_{\text{cluster},base})}{\text{std}(z_{\text{cluster},future})}$$

- Rango: (0, 2]
- >1 = Contracción (mayor cohesión = resiliente)
- <1 = Expansión (fragmentación = frágil)

#### 4. **Elasticidad** ($S_{EL}$, peso: 0.20)
Velocidad de re-estabilización post-shock:

$$S_{EL} = \exp\left(-\frac{\Delta t_{recovery}}{t_{characteristic}}\right)$$

Donde $t_{characteristic}$ es la escala temporal de recuperación (aprox. 10 años para sistemas climáticos)

#### 5. **Horizontes Temporales** ($H_T$, peso: 0.05)
Factor de largo plazo (penaliza cambios persistentes):

$$H_T = 1 - \frac{t_{projection}}{100}$$

donde $t_{projection}$ es años desde presente (2025-2069)

---

## Esquemas de Ponderación

### Esquema Teórico (por defecto)

Basado en literature de resiliencia socio-ecológica:

| Componente | Peso | Justificación |
|-----------|------|---|
| Persistencia | 0.30 | Core de resiliencia (Holling 1973) |
| Identidad | 0.25 | Pertenencia a régimen (Walker et al. 2004) |
| Cohesión | 0.20 | Estructura interna |
| Elasticidad | 0.20 | Velocidad de ajuste |
| Horizontes | 0.05 | Factor de largo plazo |

### Esquema Energético (alternativo)

Prioriza estabilidad para producción H₂:

| Componente | Peso | Justificación |
|-----------|------|---|
| Persistencia | 0.35 | Estabilidad operacional |
| Identidad | 0.30 | Compatibilidad H₂ |
| Elasticidad | 0.25 | Adaptabilidad tech |
| Cohesión | 0.05 | Menor importancia |
| Horizontes | 0.05 | Igual |

---

## Perfilado de Clusters

Para cada cluster k ∈ {1...K}:

1. **Variables climáticas dominantes**:
   - Top 5 variables con mayor varianza intra-cluster
   - Comparación vs media regional

2. **Etiqueta climática**:
   - "Frío-Húmedo" vs "Cálido-Seco" etc.
   - Basado en percentiles de T y Pr

3. **Producción H₂**:
   - Promedio del escenario Calliope
   - Variabilidad

4. **Localización**:
   - Centro geográfico
   - Rangos de elevación

---

## Validación del Clustering

### Métricas Internas

| Métrica | Descripción | Rango | Criterio |
|---------|------------|-------|----------|
| Silhouette Score | Cohesión vs separación | [-1, 1] | >0.5 es bueno |
| Davies-Bouldin | Ratio intra/inter | [0, ∞] | < 1.5 es bueno |
| Calinski-Harabasz | Ratio de varianza | [0, ∞] | > 300 es bueno |

### Validación Espacial

- **Coherencia geográfica**: Clusters no fragmentados
- **Gradientes suave**: Transiciones graduales entre clusters
- **Interpretabilidad**: Etiquetas climáticas sensatas

### Validación Temporal

- Estabilidad de cluster assignments entre años
- IRCT correlaciona con extremos observados

---

## Análisis de Sensibilidad

Test de robustez variando:

1. **k (número de clusters)**: k ∈ {3, 4, 5, 6, 8, 10}
2. **Pesos del IRCT**: Perturbaciones ±10% de cada componente
3. **Escenarios**: SSP2-4.5 vs SSP5-8.5

**Output**: Matriz de sensibilidad (Δ IRCT / Δ input)

---

## Notebooks Principales

| # | Notebook | Contenido |
|---|----------|----------|
| 07 | `07_experiments_clustering.ipynb` | K-means y validación |
| 07b | `07_cluster_profiling.ipynb` | Etiquetado y perfilado |
| 09 | `09_resilience_index.ipynb` | Cálculo de IRCT |
| 13 | `13_sensitivity_analysis.ipynb` | Análisis de robustez |

---

## Archivos de Salida

```
data/
├── clusters_base.npy               # Asignaciones histórico
├── clusters_ssp245.npy             # Asignaciones futuro
├── clusters_ssp585.npy
├── irct_pixel_ssp245.npy           # IRCT por pixel
├── irct_cluster_mean.csv           # IRCT agregado
└── cluster_profiles.json           # Etiquetas + perfilado
```

---

## Próximas Secciones

→ [05_results/](../05_results/) - Visualización y análisis de resultados
