# 3. Arquitectura del Modelo

## Concepto General

El modelo utiliza un **autoencoder variacional (VAE)** para aprender una representación latente comprimida de la complejidad climática. Este espacio latente de baja dimensión permite:

1. **Captura de patrones climáticos**: Regímenes similares → puntos cercanos en espacio latente
2. **Cuantificación de cambios**: Distancia latente = magnitud de cambio climático
3. **Clustering intuitivo**: K-means en espacio latente identifica "zonas de régimen"

---

## Diseño del Autoencoder

### Arquitectura (Variational Autoencoder - VAE)

```
INPUT (lat, lon, time) 
  ↓ [Stack multivariable]
  
ENCODER: 
  FC(1500*N_features) → ReLU → FC(512) → ReLU → FC(256)
    → μ(latent_dim) + σ(latent_dim)
  
LATENT SPACE (z):
  z ~ N(μ, σ²) [reparameterization trick]
  
DECODER:
  FC(latent_dim) → ReLU → FC(256) → ReLU → FC(512) 
    → ReLU → FC(1500*N_features) → Output
  
OUTPUT: Reconstrucción (lat, lon, time)
```

### Hyperparámetros

| Parámetro | Valor | Justificación |
|-----------|-------|---------------|
| Latent dimensions | 8-16 | Balance compresión-fidelidad |
| Encoder layers | 3-4 | Capturar patrones complejos |
| Batch size | 64-128 | Memoria + estabilidad |
| Learning rate | 1e-3 a 1e-4 | Adam optimizer |
| Epochs | 100-200 | Early stopping en validación |
| KL weight (β) | 0.1-1.0 | Regularización VAE |

### Pérdidas

$$\mathcal{L} = \underbrace{||X - \hat{X}||^2}_{\text{Reconstruction}} + \underbrace{\beta \cdot D_{KL}(q_\phi(z|x) || p(z))}_{\text{KL divergence}}$$

---

## Componentes del Modelo

### 1. Encoder
Mapea datos climáticos multidimensionales → distribución latente:

```python
def encoder(X):
    h1 = ReLU(FC(X, 512))      # Reducción inicial
    h2 = ReLU(FC(h1, 256))     # Compresión intermedia
    μ = FC(h2, latent_dim)      # Media de Z
    log_σ = FC(h2, latent_dim)  # Log varianza
    return μ, log_σ
```

**Características:**
- Dropout (0.2) para regularización
- Batch normalization para estabilidad
- Inicialización He para ReLU

### 2. Reparameterization Trick (VAE)
Permite backpropagation a través de variables estocásticas:

$$z = μ + \sigma \odot \epsilon, \quad \epsilon \sim N(0, I)$$

### 3. Decoder
Reconstruye datos desde código latente:

```python
def decoder(z):
    h1 = ReLU(FC(z, 256))        # Expansión inicial
    h2 = ReLU(FC(h1, 512))       # Compresión intermedia
    X_recon = FC(h2, input_dim)  # Reconstrucción
    return X_recon
```

---

## Espacio Latente

### Interpretabilidad
Cada dimensión latente puede interpretarse como un "factor climático":

- **Eje 1**: Gradiente temperatura (frío ↔ cálido)
- **Eje 2**: Variabilidad (estable ↔ extremos)
- **Eje 3**: Continentalidad (oceánico ↔ continental)
- **Ejes 4-8**: Interacciones complejas

### Proyección Futuro
1. Procesar datos CMIP6 futuros con encoder entrenado
2. Obtener z_future = encoder(X_CMIP6)
3. Comparar con z_base = encoder(X_CR2MET)

---

## Entrenamiento

### Protocolo

1. **Preparación**: Normalización Z-score por variable
2. **Validación split**: 80% train, 20% validation
3. **Training loop**:
   ```python
   for epoch in epochs:
       for batch in train_loader:
           z_μ, z_σ = encoder(batch)
           z = reparameterize(z_μ, z_σ)
           x_recon = decoder(z)
           loss = reconstruction_loss + β * kl_loss
           optimizer.step()
   ```
4. **Early stopping**: Si val_loss no mejora 20 epochs
5. **Model selection**: Checkpoint del mejor modelo

### Convergencia

- **Reconstruction loss**: Converge a ~0.1-0.2 (normalizado)
- **KL loss**: Converge a ~0.05-0.15 (depende β)
- **Epoch típico**: 100-150

---

## Validación de Arquitectura

### Test de Reconstrucción Espacial

Métricas de fidelidad:

- **RMSE**: Error cuadrático medio
- **Correlation**: Correlación espacial pixel-pixel
- **MSE by variable**: Errores por variable

**Criterio**: RMSE < 0.3 (normalizado)

### Test de Separabilidad Latente

Visualización:
1. t-SNE en espacio latente
2. Verificar clusters espaciales visibles
3. Validar coherencia geográfica

---

## Variantes Probadas

| Variante | Latent_dim | Layers | Notas |
|----------|------------|--------|-------|
| AE_simple | 8 | 2 | Baseline |
| VAE_base | 8 | 3 | + KL regularization |
| VAE_deep | 16 | 4 | Mayor complejidad |
| AE_conv | 8 | CNN | Convolucional 2D-time |

**Mejor rendimiento:** VAE_deep con latent_dim=16

---

## Archivos de Modelo

```
data/autoencoder_trained/
├── model_vae_best.pt          # Pesos del modelo
├── config.json                 # Hyperparámetros
├── training_history.csv        # Loss por epoch
└── latent_projection.npy       # Espacio latente histórico
```

---

## Próximas Secciones

→ [04_clustering_resilience/](../04_clustering_resilience/) - Clustering y IRCT
