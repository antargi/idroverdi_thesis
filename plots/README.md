# Visualizaciones Espaciales

Carpeta de figuras estáticas y mapas generados por análisis.

## Estructura

```
plots/
├── cluster_maps/              # Mapas de clustering espacial
│   ├── clusters_k3.png
│   ├── clusters_k5.png
│   └── clusters_k10.png
│
├── resilience_indicators/     # Mapas de IRCT por escenario
│   ├── irct_ssp245.png
│   ├── irct_ssp585.png
│   └── irct_comparison.png
│
└── sensitivity_analysis/      # Análisis de robustez
    ├── weight_sensitivity.png
    ├── k_sensitivity.png
    └── temporal_stability.png
```

## Descripción

### cluster_maps/
Visualización espacial de clusters en espacio latente:
- Cada color = cluster diferente
- Coherencia espacial demostrada
- Validación visual de segregación

### resilience_indicators/
Mapas IRCT pixel-wise para escenarios futuros:
- Colormaps: Rojo (vulnerable) → Verde (resiliente)
- Comparación SSP2-4.5 vs SSP5-8.5
- Localización de zonas críticas

### sensitivity_analysis/
Robustez del modelo ante variaciones:
- Perturbación de pesos componentes IRCT
- Variación de k (número de clusters)
- Estabilidad temporal

---

**Ir a:** [README.md](../README.md)
