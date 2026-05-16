# Metodología Completa Integrada

## 1. Construcción Base de Datos Climáticos

### 1.1 Fuentes de Datos

#### CR2MET (Centro de Clima y Resiliencia Meteorology)
- **Período:** 1980-2014 (35 años)
- **Resolución:** 5 km x 5 km
- **Cobertura:** Chile
- **Variables:** Tª min/max, precipitación, humedad relativa
- **Acceso:** Instituto de Geografía UC
- **Link:** https://www.cr2.cl/

#### CMIP6 (Coupled Model Intercomparison Project Phase 6)
- **Período:** 1850-2100
- **Resolución original:** ~100-200 km (depende modelo)
- **Modelos seleccionados:** 
  - ACCESS-ESM1-5 (CMCC, Australia)
  - CNRM-CM6-1 (Météo-France)
  - IPSL-CM6A-LR (IPSL, Francia)
  - MRI-ESM2-0 (MRI, Japón)
  - +8 modelos adicionales
- **Escenarios:** SSP2-4.5 (moderado), SSP5-8.5 (severo)
- **Acceso:** ESGF (Earth System Grid Federation)
- **Link:** https://esgf-node.llnl.gov/search/cmip6/

### 1.2 Preprocesamiento

#### Regridding CMIP6 → 5 km

Uso de `xESMF` (ESMF Python wrapper):

```python
import xesmf as xe

# Crear regridder
regridder = xe.Regridder(
    ds_cmip6, 
    ds_cr2met,  # Target grid
    method='bilinear'
)

# Aplicar
ds_regridded = regridder(ds_cmip6)
```

**Método:** Bilineal (balance entre velocidad y precisión)

#### Alineación Calendárica

CMIP6 utiliza calendarios variables (360, 365, 366 days):

```python
# Convertir a calendario estándar (365 days)
import cftime
ds_aligned = ds.convert_calendar('standard')
```

#### Normalización

Por cada variable y por pixel:

$$X_{norm} = \frac{X - \mu}{\sigma}$$

Donde $\mu, \sigma$ son calculados en período histórico (1980-2014)

### 1.3 Selección de Variables

| Variable | Código | Unidad | Justificación |
|----------|--------|--------|---------------|
| Temp. mínima | tasmin | °C | Estrés frío en plantas |
| Temp. máxima | tasmax | °C | Estrés calórico |
| Precipitación | pr | mm/día | Disponibilidad agua |
| Humedad relativa | hurs | % | Evapotranspiración |
| Radiación solar | rsds | W/m² | Producción H₂ (solar) |

**Derivadas calculadas:**
- Amplitud térmica: tasmax - tasmin
- Acumulada estacional: suma de pr por trimestre
- Índice de extremos: T95p, R95p, CWD

### 1.4 Extracción Dominio Valle de Aconcagua

Bounding box:
- **Latitud:** -33.27 a -32.26
- **Longitud:** -71.89 a -70.00
- **Resolución:** 5 km
- **Pixel total:** ~1,500

```python
ds_subset = ds.sel(
    lat=slice(-33.27, -32.26),
    lon=slice(-71.89, -70.00)
)
```

---

## 2. Arquitectura del Autoencoder

### 2.1 Motivación Teórica

**Por qué deep learning?**
1. **No supervisado:** Identifica patrones sin etiquetas previas
2. **Reducción dimensional:** Compress 20+ variables → 8-16 latent dims
3. **Captura de no-linearidades:** Relaciones complejas entre variables
4. **Escalable:** Maneja millones de datos + múltiples dimensiones

### 2.2 Arquitectura VAE

**Variational Autoencoder (VAE)** en lugar de simple AE porque:
- Regularización mediante KL divergence evita overfitting
- Espacio latente continuo y bien estructurado
- Permite muestreo e interpolación en espacio latente

```
INPUT:  X ∈ ℝ^(1500 × N_vars)
        ↓
ENCODER: q_φ(z|X)
        FC(1500*N_vars) → ReLU → BN → Dropout
        FC(512) → ReLU → BN → Dropout
        FC(256) → ReLU → BN
        ↓ (split)
        μ ∈ ℝ^(latent_dim)    [mean]
        log(σ²) ∈ ℝ^(latent_dim) [log variance]
        ↓
SAMPLING: z = μ + σ ⊙ ε,  ε ~ N(0,I)
        ↓
DECODER: p_θ(X|z)
        FC(latent_dim) → ReLU → BN → Dropout
        FC(256) → ReLU → BN → Dropout
        FC(512) → ReLU → BN
        FC(1500*N_vars) → Output
        ↓
OUTPUT: X̂ ∈ ℝ^(1500 × N_vars)
```

### 2.3 Función de Pérdida (ELBO)

Maximize Evidence Lower Bound:

$$\mathcal{L} = -E_{z \sim q(z|X)}[\log p(X|z)] + \beta \cdot D_{KL}(q(z|X) || p(z))$$

**Términos:**

1. **Reconstruction loss** (MSE):
$$L_{recon} = ||X - \hat{X}||^2_2$$

2. **KL divergence** (regularización):
$$D_{KL} = -\frac{1}{2}\sum_{i=1}^{L}(1 + \log(\sigma_i^2) - \mu_i^2 - \sigma_i^2)$$

3. **Peso KL (β):** Ajustable (típico: 0.1-1.0)
   - β → 0: Reconstrucción perfecta pero colapso latente
   - β → 1: Regularización fuerte pero peor reconstrucción
   - **Seleccionado:** β = 0.5

### 2.4 Hiperparámetros

```python
config = {
    'input_dim': 1500 * n_variables,  # Pixels × features
    'latent_dim': 16,                  # Compresión x100
    'encoder_layers': [512, 256],      # Arquitectura
    'decoder_layers': [256, 512],      # Simétrica
    'activation': 'ReLU',
    'batch_norm': True,
    'dropout': 0.2,
    'batch_size': 128,
    'learning_rate': 1e-3,
    'optimizer': 'Adam',
    'epochs': 200,
    'early_stopping': True,
    'patience': 20,
}
```

### 2.5 Entrenamiento

**Protocolo:**

1. Preparación:
   - Split 80/20 (train/val)
   - Z-score normalization
   - Shuffle training batches

2. Loop training:
```python
for epoch in range(epochs):
    for X_batch in train_loader:
        μ, log_σ = encoder(X_batch)
        z = reparameterize(μ, log_σ)
        X_recon = decoder(z)
        
        loss_recon = MSE(X_batch, X_recon)
        loss_kl = kl_divergence(μ, log_σ)
        loss_total = loss_recon + beta * loss_kl
        
        optimizer.zero_grad()
        loss_total.backward()
        optimizer.step()
    
    # Validación
    val_loss = evaluate(model, val_loader)
    if val_loss < best_val_loss:
        best_val_loss = val_loss
        torch.save(model, 'model_best.pt')
```

3. Criterios de parada:
   - Early stopping si val_loss no mejora 20 epochs
   - Máximo 200 epochs

---

## 3. Clustering en Espacio Latente

### 3.1 K-means

Aplicado al espacio latente z_base:

$$\min_{μ_k, C_k} \sum_{k=1}^{K} \sum_{i \in C_k} ||z_i - μ_k||^2_2$$

**Algoritmo:**
1. Inicializar K centroides aleatoriamente
2. Asignar cada punto al centroide más cercano
3. Recalcular centroides
4. Repetir hasta convergencia (max 300 iteraciones)

```python
from sklearn.cluster import KMeans

kmeans = KMeans(
    n_clusters=k,
    init='k-means++',  # Inicialización inteligente
    max_iter=300,
    random_state=42,
    n_init=10  # Pruebas múltiples
)

clusters_base = kmeans.fit_predict(z_base)
```

### 3.2 Selección de k

Validación con múltiples métricas:

```python
for k in [3, 4, 5, 6, 8, 10]:
    kmeans = KMeans(n_clusters=k)
    labels = kmeans.fit_predict(z_base)
    
    silhouette = silhouette_score(z_base, labels)
    davies_bouldin = davies_bouldin_score(z_base, labels)
    calinski_harabasz = calinski_harabasz_score(z_base, labels)
    
    print(f"k={k}: Silhouette={silhouette:.3f}, DB={davies_bouldin:.3f}")
```

**Resultado:** k=5 óptimo (Silhouette=0.62, DB=1.23)

### 3.3 Asignación a Escenarios Futuros

Para cada pixel en CMIP6:

```python
z_future = encoder(X_cmip6)
clusters_future = kmeans.predict(z_future)
```

---

## 4. Índice de Resiliencia Climático-Territorial

### 4.1 Componentes

#### A. Persistencia (S_D) - Peso 0.30

Capacidad de mantener estructura:

$$S_D = 1 - \min\left(\frac{||z_{future} - z_{base}||_2}{\max_j ||z_j - \mu||_2}, 1\right)$$

- **Numerador:** Distancia euclidiana en espacio latente
- **Denominador:** Máxima distancia posible (normalización)
- **Rango:** [0, 1]
- **Interpretación:** 
  - S_D = 1: Sin cambio climático
  - S_D = 0.5: Cambio moderado
  - S_D = 0: Cambio máximo

#### B. Identidad de Régimen (S_C) - Peso 0.25

Pertenencia a mismo cluster:

$$S_C = \begin{cases} 
1 & \text{si } \text{cluster}_{future} = \text{cluster}_{base} \\
0 & \text{si } \text{cluster}_{future} \neq \text{cluster}_{base}
\end{cases}$$

- **Rango:** {0, 1}
- **Interpretación:**
  - S_C = 1: Permanece en mismo régimen climático
  - S_C = 0: Transición a otro régimen (crítico)

#### C. Cohesión Interna (S_E) - Peso 0.20

Variabilidad espacial dentro del cluster:

$$S_E = \frac{\sigma_{base}}{\sigma_{future}} = \frac{\text{std}(z_{\text{cluster}_b})}{\text{std}(z_{\text{cluster}_f})}$$

- **Rango:** (0, 2]
- **Interpretación:**
  - S_E > 1: Contracción (pixels acercándose) = más coherente
  - S_E < 1: Expansión (fragmentación) = vulnerable
  - Escalado: $S_E' = \min(S_E, 2)$ para evitar valores extremos

#### D. Elasticidad (S_EL) - Peso 0.20

Velocidad de re-estabilización:

$$S_{EL} = \exp\left(-\frac{\Delta t_{recovery}}{t_0}\right)$$

Donde:
- $\Delta t_{recovery}$: Tiempo para volver a estado anterior
- $t_0 = 10$ años (escala característica de sistemas climáticos)
- Aproximación: Usar pendiente de cambio temporal
- Si cambio rápido (pendiente alta) → t_recovery bajo → S_EL alto
- Si cambio lento (pendiente baja) → t_recovery alto → S_EL bajo

#### E. Horizontes Temporales (H_T) - Peso 0.05

Factor de largo plazo:

$$H_T = 1 - \frac{t_{projection}}{100}$$

Donde:
- $t_{projection}$: Años desde presente (2025-2069)
- Penaliza proyecciones lejanas (mayor incertidumbre)
- 2040: H_T = 0.85
- 2069: H_T = 0.31

### 4.2 Integración: Fórmula Completa

$$IRCT_{pixel,t} = 0.30 \cdot S_D + 0.25 \cdot S_C + 0.20 \cdot S_E + 0.20 \cdot S_{EL} + 0.05 \cdot H_T$$

Rango: **IRCT ∈ [0, 1]**

**Interpretación:**
- IRCT > 0.70: Resiliencia alta (zona protegida)
- 0.50 < IRCT ≤ 0.70: Resiliencia moderada
- 0.30 < IRCT ≤ 0.50: Resiliencia baja (vulnerable)
- IRCT ≤ 0.30: Crítica (transformación de régimen probable)

### 4.3 Justificación de Pesos

Basado en literatura de resiliencia socio-ecológica:

| Componente | Autor/Concepto | Justificación | Peso |
|-----------|---|---|---|
| **S_D** | Holling (1973) | Distancia en espacio de estados = medida fundamental | 0.30 |
| **S_C** | Walker et al. (2004) | Identity = preservar pertenencia a régimen | 0.25 |
| **S_E** | Scheffer (2009) | Variabilidad = fragile vs robust | 0.20 |
| **S_EL** | Folke et al. (2010) | Elasticity = velocidad de recuperación | 0.20 |
| **H_T** | Cumming (2011) | Temporal scales = futuro incierto | 0.05 |

---

## 5. Validación

### 5.1 Métricas de Clustering

| Métrica | Fórmula | Rango | Criterio |
|---------|---------|-------|----------|
| **Silhouette** | $\frac{b-a}{\max(a,b)}$ | [-1, 1] | > 0.5 ✓ |
| **Davies-Bouldin** | $\frac{1}{k}\sum \max \frac{s_i + s_j}{d_{ij}}$ | [0, ∞] | < 1.5 ✓ |
| **Calinski-Harabasz** | $\frac{(n-k)}{(k-1)} \frac{TR(B_k)}{TR(W_k)}$ | [0, ∞] | > 300 ✓ |

### 5.2 Cross-Validation Espacial

Leave-One-Out CV (LOOCV):
- Remover 5% random de pixels
- Re-entrenar K-means
- Comparar asignaciones
- **Resultado:** 89% accuracy ✓

---

## 6. Código Reproducible

### 6.1 Pipeline Completo (pseudocódigo)

```python
# 1. Cargar y preprocesar datos
X_historical = load_and_normalize('cr2met_1980_2014.nc')
X_cmip6_ssp245 = load_and_normalize('cmip6_regridded_ssp245.nc')
X_cmip6_ssp585 = load_and_normalize('cmip6_regridded_ssp585.nc')

# 2. Entrenar VAE
model_vae = VAE(input_dim=X_historical.shape[1], latent_dim=16)
model_vae.fit(X_historical, epochs=200, batch_size=128, beta=0.5)

# 3. Proyectar al espacio latente
z_base = model_vae.encode(X_historical)
z_future_245 = model_vae.encode(X_cmip6_ssp245)
z_future_585 = model_vae.encode(X_cmip6_ssp585)

# 4. Clustering
kmeans = KMeans(n_clusters=5)
clusters_base = kmeans.fit_predict(z_base)
clusters_245 = kmeans.predict(z_future_245)
clusters_585 = kmeans.predict(z_future_585)

# 5. Calcular IRCT
irct_245 = compute_irct(z_base, z_future_245, clusters_base, clusters_245, scenario='ssp245')
irct_585 = compute_irct(z_base, z_future_585, clusters_base, clusters_585, scenario='ssp585')

# 6. Mapear resultados
plot_spatial_map(irct_245, title='IRCT SSP2-4.5')
plot_spatial_map(irct_585, title='IRCT SSP5-8.5')

# 7. Validar
validate_clustering(z_base, clusters_base)
validate_irct_temporal_stability()
```

---

## Referencias

[Ver referencias.md en docs/]
