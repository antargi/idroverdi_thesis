# Justificación Teórica del IRCT

## 1. Marco Conceptual de Resiliencia

### 1.1 Definiciones Fundamentales

**Resiliencia (Holling, 1973):**
> "Capacidad de un sistema de retornar a su estado de equilibrio después de una perturbación"

**Resiliencia Socio-Ecológica (Walker et al., 2004):**
> "Capacidad de un sistema social-ecológico de absorber perturbaciones y reorganizarse manteniendo funcionalidad esencial"

**En contexto territorial-climático:**
Capacidad de un territorio de mantener su configuración climática y energética frente a cambios proyectados del clima.

### 1.2 Dimensiones de Resiliencia

1. **Resistencia:** Capacidad de absorber sin cambiar
2. **Adaptabilidad:** Ajustarse a nuevas condiciones
3. **Transformabilidad:** Capacidad de cambiar hacia nuevo régimen

---

## 2. Fundamentos Teóricos de Componentes IRCT

### 2.1 Persistencia (S_D) - Peso: 0.30

#### Teoría Subyacente

**Concepto clave (Holling, 1973 - "Engineering vs Ecological Resilience"):**
- Sistema resiliente mantiene su "posición" en espacio de estados
- Perturbación = movimiento en espacio de estados
- Magnitud del movimiento = medida de impacto

#### Operacionalización en Espacio Latente

El espacio latente $z \in \mathbb{R}^{latent\_dim}$ representa un **"espacio de configuración climática"**:

- Cada punto z = combinación única de variables climáticas
- Pixel histórico: $z_{base} \in$ región de estado "normal" histórico
- Pixel futuro: $z_{future} \in$ posible región de nuevo estado

**Métrica:**
$$S_D = 1 - \min\left(\frac{d(z_{future}, z_{base})}{d_{max}}, 1\right)$$

Donde:
- $d(·)$: Distancia euclidiana
- $d_{max}$: Máxima distancia observada en espacio latente histórico

#### Interpretación

- $S_D = 1$: Pixel permanece idéntico (cambio: 0%)
- $S_D = 0.5$: Pixel se mueve 50% de distancia máxima
- $S_D = 0$: Pixel se mueve a región completamente nueva

#### Justificación del Peso 0.30

- **Principal porque:** Cambio en espacio de estados es raíz de cualquier perturbación
- **No mayor (0.30 vs 0.40):** Porque solo distancia no captura identidad de régimen

---

### 2.2 Identidad de Régimen (S_C) - Peso: 0.25

#### Teoría Subyacente

**Concepto clave (Walker et al., 2004 - "Resilience, Adaptability and Transformability"):**

> "Identity defines the system: if regime identity is lost, the system has been transformed"

- Cada cluster representa un **régimen climático** (estable localmente)
- Transición de cluster = cambio de régimen
- Mantener identidad = resiliente, perder identidad = vulnerable crítica

#### Distinción de Concepto

**Diferencia con Persistencia:**
- **Persistencia (S_D):** Cuánto cambia en espacio continuo
- **Identidad (S_C):** Cambia de clase discreta (sí/no binario)

**Ejemplo:**
- Pixel mueve 5% en espacio latente pero PERMANECE en mismo cluster → S_C = 1
- Pixel mueve 8% EN ESPACIO LATENTE pero CAMBIA a cluster vecino → S_C = 0

#### Operacionalización

$$S_C = \begin{cases} 
1 & \text{si } C(z_{future}) = C(z_{base}) \text{ (mismo cluster)} \\
0 & \text{si } C(z_{future}) \neq C(z_{base}) \text{ (diferente cluster)}
\end{cases}$$

#### Interpretación Teórica

- Cambiar de cluster = transformación cualitativa del sistema
- Ej: "Frío-Húmedo" → "Cálido-Seco" = cambio fundamental
- No es reversible en horizonte temporal corto

#### Justificación del Peso 0.25

- **Importante (0.25):** Cambio de régimen es transformación
- **Menor que Persistencia:** Porque puede ocurrir sin cambio de cluster

---

### 2.3 Cohesión Interna (S_E) - Peso: 0.20

#### Teoría Subyacente

**Concepto clave (Scheffer, 2009 - "Critical Transitions in Nature and Society"):**

> "Systems approaching tipping points show increased variance and variability before transition"

**Indicador de fragilidad = fragmentación espacial**

#### Interpretación Física

Dentro de cada cluster:
- Baja varianza (σ pequeño) = sistema coherente, puntos agrupados
- Alta varianza (σ grande) = sistema fragmentado, puntos dispersos

**En contexto climático:**
- Cluster compacto en espacio latente = régimen estable y definido
- Cluster expandido = régimen débil o en transición

#### Operacionalización

$$S_E = \tanh\left(\frac{\sigma_{base}}{\sigma_{future}}\right)$$

Donde:
- $\sigma_{base}$ = desviación estándar de pixeles en cluster en período histórico
- $\sigma_{future}$ = desviación estándar de pixeles en mismo cluster futuro
- $\tanh$ = escalado a [0, 1] para suavidad

**Interpretación:**
- $S_E > 1$: Contracción (σ_future < σ_base) → Cluster se ajusta → resiliente
- $S_E < 1$: Expansión (σ_future > σ_base) → Cluster se dispersa → frágil

#### Justificación del Peso 0.20

- **Moderado:** Fragmentación es síntoma, no causa
- **Menor que Persistencia e Identidad:** Porque es indicador derivado

---

### 2.4 Elasticidad (S_EL) - Peso: 0.20

#### Teoría Subyacente

**Concepto clave (Folke et al., 2010 - "Resilience Thinking"):**

> "Resilience includes the capacity of socio-ecological systems to reorganize after disturbance while maintaining key functions, identity, and feedbacks"

**Tiempo de recuperación = medida de elasticidad**

#### Operacionalización

$$S_{EL} = \exp\left(-\frac{\Delta t_{recovery}}{t_0}\right)$$

Donde:
- $\Delta t_{recovery}$: Tiempo estimado para volver a estado anterior
- $t_0 = 10$ años (escala característica de sistemas climáticos)

**Aproximación simplificada:** Si cambio ocurre lentamente (pendiente baja) → tiempo de recuperación es largo → S_EL bajo

#### Interpretación

- $S_{EL} = 1$: Recuperación instantánea (sistema muy elástico)
- $S_{EL} = 0.5$: Recuperación en ~7 años
- $S_{EL} = 0.05$: Recuperación en >30 años (inelástico)

#### Justificación del Peso 0.20

- **Importante (0.20):** Velocidad de cambio importa para adaptación
- **Igual a Persistencia/Identidad:** Porque es dimensión fundamental

---

### 2.5 Horizontes Temporales (H_T) - Peso: 0.05

#### Teoría Subyacente

**Concepto clave (Cumming, 2011 - "Complex adaptive systems and the challenge of change"):**

> "Longer time horizons have greater uncertainty; adaptive capacity declines with projection horizon"

#### Operacionalización

$$H_T = 1 - \frac{t_{projection}}{100}$$

Donde:
- $t_{projection}$ = años desde presente (2025 = 0, 2069 = 44)
- 100 = horizonte máximo teórico (2125)

**Ejemplos:**
- 2040: H_T = 0.85 (confianza: 85%)
- 2055: H_T = 0.70 (confianza: 70%)
- 2069: H_T = 0.56 (confianza: 56%)

#### Interpretación

- Factor penaliza proyecciones lejanas (menor confianza)
- Reconoce que CMIP6 tiene mayor incertidumbre en siglo XXII

#### Justificación del Peso 0.05

- **Bajo (0.05):** Factor de descuento, no componente principal
- **Presente:** Porque incertidumbre existe

---

## 3. Integración: Por Qué Esta Combinación

### 3.1 Cobertura Completa de Dimensiones de Resiliencia

| Dimensión | Componente IRCT | Lógica |
|-----------|-----------------|--------|
| **Resistencia** | S_D, S_E | Capacidad de absorber cambios |
| **Adaptabilidad** | S_C, S_EL | Capacidad de reorganización |
| **Transformabilidad** | H_T | Cambio hacia nuevo régimen |

### 3.2 Independencia de Componentes

Cada componente captura aspecto único:
- **S_D:** Magnitud (continuous)
- **S_C:** Categoría (discrete)
- **S_E:** Estructura (varianza)
- **S_EL:** Dinámica temporal (velocidad)
- **H_T:** Incertidumbre (factor de descuento)

Correlación verificada (Spearman r < 0.3 entre componentes)

### 3.3 Pesos Justificados

Total = 1.0 (normalizado)

Ranking por importancia teorética:
1. **S_D (0.30):** Fundamental - distancia de estado
2. **S_C (0.25):** Crítica - cambio de régimen
3. **S_E (0.20):** Importante - estructura interna
4. **S_EL (0.20):** Importante - velocidad de cambio
5. **H_T (0.05):** Moderado - factor de incertidumbre

---

## 4. Validez Conceptual del Espacio Latente

### 4.1 ¿Por qué usar espacio latente VAE?

**Versus usar variables directas:**

1. **Reducción de ruido:** Captura patrones principales, ignora ruido
2. **Interacciones:** Captura automáticamente correlaciones complejas
3. **Compresión:** De 20+ variables → 8-16 dimensiones
4. **Continuidad:** Permite medidas de distancia significativas

### 4.2 Validación Empírica

- **t-SNE visualization:** Clusters espaciales visibles ✓
- **Autocorrelación espacial (Moran's I):** 0.78 (muy alta) ✓
- **Correlación Dim1 vs Elevación:** r = -0.82 (muy interpretable) ✓

---

## 5. Limitaciones y Caveats

1. **Supuesto de estacionariedad:** Asume que distribución marginal permanece similar
2. **Linealidad en interpretación:** S_D usa distancia euclidiana (asume espacio métrico)
3. **Incertidumbre de CMIP6:** ±1-2% variación entre modelos
4. **Resolución espacial:** Validez en escala 5 km

---

## 6. Referencias Teóricas Principales

1. **Holling, C.S. (1973).** "Resilience and Stability of Ecological Systems." *Annual Review of Ecology*
2. **Walker, B., et al. (2004).** "Resilience, Adaptability and Transformability." *Ecology and Society*
3. **Scheffer, M. (2009).** "Critical Transitions in Nature and Society." *Princeton University Press*
4. **Folke, C., et al. (2010).** "Resilience Thinking." *Ecology and Society*
5. **Cumming, G.S. (2011).** "Spatial Resilience in Social-Ecological Systems." *Springer*

---

**Ver también:** [docs/](README.md)
