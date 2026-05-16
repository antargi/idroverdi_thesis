# ⚡ QUICK START

Guía rápida para entender y navegar la tesis de resiliencia climática territorial.

---

## 🎯 En 5 minutos

1. **Lee:** [index.md](index.md) - Página de inicio
2. **Contexto:** [01_introduction/README.md](01_introduction/README.md) - Pregunta y objetivos
3. **Resultados:** [05_results/README.md](05_results/README.md) - Hallazgos clave

---

## 🗺️ Estructura de la Tesis

```
Introducción → Datos → Modelo → Clustering → Resultados → Validación
     ↓            ↓        ↓          ↓           ↓            ↓
  01_intro     02_data   03_arch    04_clust    05_results  06_valid
  [README]     [NB×3]    [NB×1]     [NB×3]      [NB×3]      [NB×1]
```

**Lectura recomendada:** En orden (02 → 03 → 04 → 05 → 06)

---

## 📚 Navegación por Perfil

### Para Interesados en Clima
→ [01_introduction/](01_introduction/) → [02_data_exploration/](02_data_exploration/) → [05_results/README.md](05_results/README.md)

### Para Ingenieros/Data Scientists
→ [03_model_architecture/](03_model_architecture/) → [04_clustering_resilience/](04_clustering_resilience/) → [06_validation/](06_validation/)

### Para Tomadores de Decisión
→ [05_results/README.md](05_results/README.md) → [docs/metodologia.md](docs/metodologia.md) (sección resumen ejecutivo)

### Para Investigadores
→ [docs/](docs/) (toda la carpeta)

---

## 📖 Contenido por Carpeta

| Carpeta | Contenido | Notebooks | Tiempo |
|---------|-----------|-----------|--------|
| **01_introduction** | Contexto y problema | - | 15 min |
| **02_data_exploration** | Exploración de datos | 3 | 30 min |
| **03_model_architecture** | Diseño del AE/VAE | 1 | 20 min |
| **04_clustering_resilience** | Clustering e IRCT | 3 | 45 min |
| **05_results** | Resultados principales | 3 | 40 min |
| **06_validation** | Validación estadística | 1 | 25 min |
| **docs** | Documentación técnica | - | 1+ hora |
| **utils** | Código reutilizable | 6 módulos | - |

**Total:** ~3 horas explorando, ~1 hora leyendo docs técnicos

---

## 🚀 Para Reproducir Análisis

### Requisitos
```bash
python >= 3.9
pip install -r requirements.txt
```

### Pasos
1. Preparar datos (ver instrucciones en `02_data_exploration/README.md`)
2. Ejecutar notebooks en orden: `02_*.ipynb` → `03_*.ipynb` → ... → `06_*.ipynb`
3. Generar IRCT con `04_clustering_resilience/09_resilience.ipynb`
4. Validar con `06_validation/03_spatial_reconstruction_analysis.ipynb`

---

## 🔍 Conceptos Clave (1 minuto cada uno)

### ¿Qué es Resiliencia Climática Territorial?
Capacidad de una región de mantener su clima y estructura frente a cambios proyectados.

### ¿Qué es el IRCT?
**Índice de Resiliencia Climático-Territorial** = métrica integrada que combina:
- Persistencia (mantener estado)
- Identidad (mantener cluster/régimen)
- Cohesión (estructura interna)
- Elasticidad (velocidad de cambio)
- Horizontes (factor temporal)

### ¿Por qué Autoencoders?
Aprenden representación comprimida (espacio latente) de complejidad climática multivariada. Permite clustering interpretable.

### ¿Qué son los Clusters?
Agrupamientos de pixeles en espacio latente. Cada cluster = régimen climático único (ej: "Frío-Húmedo", "Cálido-Seco")

### ¿Qué significa IRCT alto/bajo?
- **IRCT > 0.70**: Zona resiliente (protegida de cambios climáticos)
- **0.30 < IRCT ≤ 0.70**: Zona moderadamente vulnerable
- **IRCT ≤ 0.30**: Zona crítica (probable transformación de régimen)

---

## 📊 Hallazgos Principales (Resumen)

1. **5 zonas de resiliencia** identificadas en Valle de Aconcagua
2. **Elevación es factor protector**: IRCT aumenta ~3% por cada 100m
3. **SSP2-4.5 moderado**: IRCT promedio 0.58 (resiliente)
4. **SSP5-8.5 severo**: IRCT promedio 0.32 (vulnerable crítica)
5. **Producción H₂ viable** solo en zonas altas bajo SSP5-8.5

---

## 📖 Documentación Técnica

- [Metodología completa](docs/metodologia.md)
- [Justificación IRCT](docs/irct_justificacion.md)
- [Variables climáticas](docs/variables_climat.md)

---

## 🎓 Referencias Principales

1. Holling (1973): "Resilience and Stability of Ecological Systems"
2. Walker et al. (2004): "Resilience, Adaptability and Transformability"
3. Scheffer (2009): "Critical Transitions in Nature and Society"

---

## ❓ Preguntas Frecuentes

**¿Puedo reproducir esto sin GPU?**
Sí, pero será lento. Recomendado GPU (NVIDIA CUDA).

**¿Están disponibles todos los datos?**
Notebooks sí. Datos climáticos grandes disponibles por solicitud (aninotna@gmail.com).

**¿Qué Python version?**
3.9+. Probado en 3.10.

**¿Tiempo de ejecución?**
- Entrenamiento VAE: ~10-20 min (con GPU)
- Clustering + IRCT: ~5 min
- Total: ~1 hora

---

## 📁 Archivos Útiles

| Archivo | Uso |
|---------|-----|
| `README.md` | Descripción general |
| `index.md` | Página de inicio |
| `requirements.txt` | Dependencias |
| `.gitignore` | Exclusiones Git |
| `_config.yml` | Configuración GitHub Pages |

---

## 🔗 Enlaces

- **GitHub:** [justh2_pipeline](https://github.com/aninotna/justh2_pipeline)
- **Email:** aninotna@gmail.com
- **Institución:** Universidad - Programa Magister Sistemas Complejos

---

**Última actualización:** Mayo 2026  
**Status:** Tesis en revisión  
**Prox. hito:** Presentación oral
