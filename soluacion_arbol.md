# Árbol de Decisión basado en Ganancia de Información (Algoritmo ID3)

## 1. Conjunto de Datos Inicial ($S$)

El objetivo es predecir si una persona **Compra Ordenador** (Sí/No). El conjunto de datos total ($N=14$) tiene la siguiente distribución:

| Clase | Conteo | Proporción ($p$) |
| :--- | :--- | :--- |
| **Sí** | 9 | $9/14 \approx 0.643$ |
| **No** | 5 | $5/14 \approx 0.357$ |

## 2. Entropía Inicial del Conjunto Raíz ($H(S)$)

La Entropía mide la impureza del conjunto inicial.

$$H(S) = - \sum_{i=1}^{c} p_i \log_2(p_i)$$

$$H(S) = - \left( \frac{9}{14} \log_2 \left(\frac{9}{14}\right) \right) - \left( \frac{5}{14} \log_2 \left(\frac{5}{14}\right) \right)$$

$$H(S) \approx 0.940 \text{ bits}$$

## 3. Ganancia de Información por Atributo ($Gain$)

Se calcula la ganancia de información para cada atributo utilizando la fórmula:

$$Gain(S, A) = H(S) - \sum_{v \in Values(A)} \frac{|S_v|}{|S|} H(S_v)$$




| Atributo ($A$) | Entropía Ponderada  | Ganancia  |
| **Edad** | $0.6940$ | $0.940 - 0.6940 = **0.246**$ |
| **Estudiante** | $0.7885$ | $0.940 - 0.7885 = **0.152**$ |
| **Calificación Crediticia** | $0.8920$ | $0.940 - 0.8920 = **0.048**$ |
| **Ingresos** | $0.9108$ | $0.940 - 0.9108 = **0.029**$ |






---

## 4. Primer Nivel del Árbol (Nodo Raíz)

El atributo con la **mayor Ganancia de Información** es **Edad** ($0.246$). Por lo tanto, **Edad** es el nodo raíz.

### A. Rama: Edad = $31-40$

| Conteo | Sí | No | Entropía | Decisión |
| :--- | :--- | :--- | :--- | :--- |
| 4 | 4 | 0 | $0$ | **Sí** (Nodo Puro) |

### B. Rama: Edad = $\leq 30$ (Subconjunto $S_{\leq 30}$)

| Conteo | Sí | No | Entropía |
| :--- | :--- | :--- | :--- |
| 5 | 2 | 3 | $0.971$ |

**Siguiente división para $S_{\leq 30}$:** (La mayor ganancia se da por **Estudiante** o **Ingresos**, $0.571$)

| Sub-Atributo | $Gain(S_{\leq 30}, A)$ |
| :--- | :--- |
| **Estudiante** | **0.571** |

* **Estudiante = Sí** (2 Sí, 0 No) $\implies$ **Sí**
* **Estudiante = No** (0 Sí, 3 No) $\implies$ **No**

### C. Rama: Edad = $> 40$ (Subconjunto $S_{> 40}$)

| Conteo | Sí | No | Entropía |
| :--- | :--- | :--- | :--- |
| 5 | 3 | 2 | $0.971$ |

**Siguiente división para $S_{> 40}$:**

| Sub-Atributo | $Gain(S_{> 40}, A)$ |
| :--- | :--- |
| **Calificación Crediticia** | **0.171** |

* Se continúa dividiendo por **Calificación Crediticia** para esta rama.

---

## 5. Estructura Final del Árbol (Parcial)

La estructura del árbol de decisión, priorizando el atributo con la mayor reducción de entropía en cada paso, es:

1.  **¿Edad?**
    * **SI Edad = $31-40$** $\rightarrow$ **COMPRA ORDENADOR: SÍ**
    * **SI Edad = $\leq 30$** $\rightarrow$ **¿Estudiante?**
        * SI Estudiante = Sí $\rightarrow$ **COMPRA ORDENADOR: SÍ**
        * SI Estudiante = No $\rightarrow$ **COMPRA ORDENADOR: NO**
    * **SI Edad = $> 40$** $\rightarrow$ **¿Calificación Crediticia?**
        * Continúa la división...

