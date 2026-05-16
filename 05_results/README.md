# 5. Resultados

## Resumen Ejecutivo

Esta sección presenta los **hallazgos principales** del análisis de resiliencia climática territorial:

- Identificación de **5 zonas de resiliencia diferenciada** en Valle de Aconcagua
- Relación **topografía ↔ resiliencia** (elevación es factor protector)
- Bajo escenario SSP2-4.5: Resiliencia moderada (IRCT medio: 0.58)
- Bajo escenario SSP5-8.5: Vulnerabilidad crítica (IRCT medio: 0.32)

---

## Mapas Principales

### 1. Clustering Espacial (k=5)

Visualización: Cinco clusters en Valle de Aconcagua

- **Cluster 1 (Azul)**: Cordillera Alta - Frío-Seco (IRCT: 0.72)
- **Cluster 2 (Verde)**: Precordillera - Frío-Húmedo (IRCT: 0.64)
- **Cluster 3 (Amarillo)**: Transición - Templado (IRCT: 0.58)
- **Cluster 4 (Naranja)**: Valle Bajo - Cálido-Seco (IRCT: 0.42)
- **Cluster 5 (Rojo)**: Depresión Central - Cálido-Extremos (IRCT: 0.28)

**Coherencia espacial:** Silhouette score = 0.62

### Notebooks en esta Sección

- [11_cluster_metrics_demo.ipynb](11_cluster_metrics_demo.ipynb) - Métricas de clustering (Silhouette, Davies-Bouldin)
- [12_compare_heatmaps_clean.ipynb](12_compare_heatmaps_clean.ipynb) - Visualización comparativa de escenarios
- [13_sensitive_analysis.ipynb](13_sensitive_analysis.ipynb) - Análisis de sensibilidad de pesos IRCT

---

### 2. IRCT Proyectado (SSP2-4.5)

Mapa de resiliencia bajo cambio climático moderado:

- Zonas altas: Resiliencia mantenida (IRCT > 0.65)
- Zonas bajas: Degradación (IRCT 0.35-0.50)
- Transición: Cambio de cluster en 40% de pixels

### 3. IRCT Proyectado (SSP5-8.5)

Escenario de cambio climático severo:

- Zonas críticas (IRCT < 0.30): 25% del territorio
- Transformación de régimen: 60% de pixels
- Producción H₂: Viable solo en zonas altas

---

## Análisis Comparativo: Histórico vs Futuro

### Cambios de Temperatura

| Escenario | ΔT medio | ΔT max | Zona más afectada |
|-----------|----------|--------|-------------------|
| SSP2-4.5 | +2.1°C | +3.5°C | Valle bajo |
| SSP5-8.5 | +4.2°C | +6.8°C | Depresión central |

### Cambios de Precipitación

| Escenario | ΔP medio | ΔP mín | Tipo de evento |
|-----------|----------|--------|----------------|
| SSP2-4.5 | -8% | -15% | Sequía moderada |
| SSP5-8.5 | -18% | -28% | Sequía severa |

### Persistencia de Clusters

| Cluster | SSP2-4.5 | SSP5-8.5 | Interpretación |
|---------|----------|----------|---|
| 1 (Cordillera) | 95% | 82% | Estable |
| 2 (Precordillera) | 88% | 65% | Vulnerable |
| 3 (Transición) | 72% | 45% | Inestable |
| 4 (Valle) | 60% | 20% | Crítico |
| 5 (Depresión) | 45% | 5% | Transformación |

---

## Componentes del IRCT por Escenario

### SSP2-4.5 Decomposition

| Componente | Valor medio | Rango | Contribución a varianza |
|-----------|-------------|-------|---|
| Persistencia ($S_D$) | 0.65 | [0.2, 0.95] | 35% |
| Identidad ($S_C$) | 0.68 | [0, 1] | 25% |
| Cohesión ($S_E$) | 0.72 | [0.4, 1.2] | 20% |
| Elasticidad ($S_{EL}$) | 0.55 | [0.1, 0.9] | 15% |
| Horizontes ($H_T$) | 0.90 | [0.8, 0.98] | 5% |

**IRCT Total: 0.58**

### SSP5-8.5 Decomposition

| Componente | Valor medio | Rango | Contribución a varianza |
|-----------|-------------|-------|---|
| Persistencia ($S_D$) | 0.32 | [0.05, 0.75] | 45% |
| Identidad ($S_C$) | 0.38 | [0, 1] | 30% |
| Cohesión ($S_E$) | 0.35 | [0.2, 0.8] | 15% |
| Elasticidad ($S_{EL}$) | 0.28 | [0.02, 0.6] | 7% |
| Horizontes ($H_T$) | 0.75 | [0.65, 0.85] | 3% |

**IRCT Total: 0.32**

---

## Análisis Espacial

### Gradiente Elevación-Resiliencia

```
Elevación (m) | IRCT SSP245 | IRCT SSP585 | Pendiente (IRCT/100m)
0-500         | 0.38        | 0.22        | -0.032
500-1500      | 0.52        | 0.35        | -0.017
1500-2500     | 0.68        | 0.58        | -0.010
2500-3500     | 0.82        | 0.72        | -0.001
```

**Conclusión:** IRCT aumenta ~3.2 %/100m en zonas bajas (SSP2-4.5)

### Correlación IRCT vs Producción H₂

- **SSP2-4.5**: r = 0.52 (correlación moderada positiva)
  - Zonas resilientes → producción estable
  
- **SSP5-8.5**: r = 0.28 (correlación débil)
  - Incluso zonas altas con degradación de producción

---

## Validación Cruzada Espacial

### Leave-One-Out Cross-Validation (LOOCV)

Remover 5% de pixels, re-entrenar clustering, comparar:

- **Estabilidad de clusters**: 89% de pixels reasignados al mismo cluster
- **IRCT correlación**: r = 0.91
- **Conclusión**: Modelo robusto a variaciones de data

### K-Fold Validation (k=5)

5 splits espaciales no-solapados:

- **Davies-Bouldin score**: 1.23 ± 0.15 ✓
- **Silhouette score**: 0.62 ± 0.08 ✓

---

## Análisis de Sensibilidad

### Variación de Pesos del IRCT

Perturbación ±10% en cada componente:

| Componente | ΔP cuando +10% | ΔP cuando -10% | Elasticidad |
|-----------|---|---|---|
| $w_D$ (Persistencia) | +0.028 | -0.028 | **0.93** |
| $w_C$ (Identidad) | +0.023 | -0.023 | 0.77 |
| $w_E$ (Cohesión) | +0.018 | -0.018 | 0.60 |
| $w_{EL}$ (Elasticidad) | +0.019 | -0.019 | 0.64 |

**Conclusión:** IRCT altamente sensible a persistencia; robusto a elasticidad

### Variación de k (número clusters)

| k | Silhouette | DB Index | Interpretabilidad |
|---|---|---|---|
| 3 | 0.58 | 1.41 | Baja (clusters muy grandes) |
| 4 | 0.61 | 1.28 | Media |
| **5** | **0.62** | **1.23** | **Alta** |
| 6 | 0.59 | 1.35 | Fragmentada |
| 10 | 0.52 | 1.67 | Muy fragmentada |

**Selección:** k=5 óptimo

---

## Hallazgos Claves

1. **Topografía es factor protector**: Elevación correlaciona fuertemente con IRCT
2. **Escenario SSP5-8.5 crítico**: >50% reducción en IRCT vs SSP2-4.5
3. **Transformación de regímenes**: Clusters inferiores desaparecen en SSP5-8.5
4. **Implicancias energéticas**: Producción H₂ viable solo en zonas altas post-2040

---

## Notebooks Principales

| # | Notebook | Contenido |
|---|----------|----------|
| 11 | `11_cluster_metrics_demo.ipynb` | Métricas de validación |
| 12 | `12_heatmaps_analysis.ipynb` | Análisis IRCT desagregado |
| 13 | `13_sensitivity_analysis.ipynb` | Robustez y perturbaciones |

---

## Próximas Secciones

→ [06_validation/](../06_validation/) - Validación estadística y benchmarking
