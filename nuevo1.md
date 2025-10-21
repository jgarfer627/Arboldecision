A continuación, se presenta la construcción del **árbol de decisión completo** utilizando la Ganancia de Información (ID3), asegurando que cada nodo hoja sea puro.

El proceso implica calcular la Entropía y la Ganancia de Información en cada nodo para seleccionar el mejor atributo de división.

---

## 1. Nodo Raíz: Edad

La **Edad** fue seleccionada como nodo raíz por tener la máxima Ganancia de Información ($0.246$). El conjunto inicial $S$ tiene una distribución de (9 Sí, 5 No), con $H(S) \approx 0.940$ bits.

| Rama | Conteo (Total) | Distribución (Sí, No) | Entropía | Estado |
| :--- | :--- | :--- | :--- | :--- |
| **$31-40$** | 4 | (4, 0) | $\mathbf{0.000}$ | **Hoja: SÍ** (Puro) |
| **$\leq 30$** | 5 | (2, 3) | $0.971$ | Continúa al Sub-Árbol A |
| **$> 40$** | 5 | (3, 2) | $0.971$ | Continúa al Sub-Árbol B |

---

## 2. Sub-Árbol A: Edad $\leq 30$

El subconjunto $S_{\leq 30}$ (2 Sí, 3 No) tiene $H(S_{\leq 30}) \approx 0.971$. La máxima ganancia se logra con **Estudiante** (Ganancia $0.571$).

| Rama | Conteo (Total) | Distribución (Sí, No) | Entropía | Estado |
| :--- | :--- | :--- | :--- | :--- |
| **Estudiante = Sí** | 2 | (2, 0) | $\mathbf{0.000}$ | **Hoja: SÍ** (Puro) |
| **Estudiante = No** | 3 | (0, 3) | $\mathbf{0.000}$ | **Hoja: NO** (Puro) |

---

## 3. Sub-Árbol B: Edad $> 40$

El subconjunto $S_{> 40}$ (3 Sí, 2 No) tiene $H(S_{> 40}) \approx 0.971$. La máxima ganancia se logra con **Calificación Crediticia** (Ganancia $0.171$).

| Rama | Conteo (Total) | Distribución (Sí, No) | Entropía | Próximo Paso |
| :--- | :--- | :--- | :--- | :--- |
| **Cal. Cred. = Excelente** | 2 | (1, 1) | $1.000$ | Continúa al Nivel B.1 |
| **Cal. Cred. = Justa** | 3 | (2, 1) | $0.918$ | Continúa al Nivel B.2 |

### Nivel B.1: Edad $> 40$ Y Cal. Cred. = Excelente

El subconjunto (1 Sí, 1 No) tiene $H \approx 1.000$. **Ingresos** y **Estudiante** tienen la máxima Ganancia ($1.000$). Elegimos **Ingresos**.

| Rama | Conteo | Distribución (Sí, No) | Entropía | Estado |
| :--- | :--- | :--- | :--- | :--- |
| **Ingresos = Bajo** | 1 | (0, 1) | $\mathbf{0.000}$ | **Hoja: NO** (Puro) |
| **Ingresos = Medio** | 1 | (1, 0) | $\mathbf{0.000}$ | **Hoja: SÍ** (Puro) |

### Nivel B.2: Edad $> 40$ Y Cal. Cred. = Justa

El subconjunto (2 Sí, 1 No) tiene $H \approx 0.918$. **Ingresos** y **Estudiante** tienen la máxima Ganancia ($0.918$). Elegimos **Ingresos**.

| Rama | Conteo | Distribución (Sí, No) | Entropía | Próximo Paso |
| :--- | :--- | :--- | :--- | :--- |
| **Ingresos = Bajo** | 1 | (1, 0) | $\mathbf{0.000}$ | **Hoja: SÍ** (Puro) |
| **Ingresos = Medio** | 2 | (1, 1) | $1.000$ | Continúa al Nivel B.2.1 |

#### Nivel B.2.1: Edad $> 40$, Cal. Cred. = Justa Y Ingresos = Medio

El subconjunto (1 Sí, 1 No) tiene $H = 1.000$. El único atributo restante es **Estudiante**.

| Rama | Conteo | Distribución (Sí, No) | Entropía | Estado |
| :--- | :--- | :--- | :--- | :--- |
| **Estudiante = No** | 2 | (1, 1) | $1.000$ | **Hoja: [SÍ/NO]** (No se puede purificar más) |
| **Estudiante = Sí** | 0 | (0, 0) | $-$ | Detiene |

*En esta última rama, el conjunto de datos se reduce a (1 Sí, 1 No), ambos con **Estudiante = No**. Como se han agotado todos los atributos, el nodo **no es puro** y la clasificación final en este punto es ambigua (se podría asignar la clase mayoritaria o detener el crecimiento del árbol).*

---

## 4. Árbol de Decisión Completo (Estructura Final)

1.  **¿Edad?**
    * **SI Edad = $31-40$** $\rightarrow$ **SÍ**
    * **SI Edad = $\leq 30$** $\rightarrow$ **¿Estudiante?**
        * SI Estudiante = Sí $\rightarrow$ **SÍ**
        * SI Estudiante = No $\rightarrow$ **NO**
    * **SI Edad = $> 40$** $\rightarrow$ **¿Calificación Crediticia?**
        * SI Cal. Cred. = Excelente $\rightarrow$ **¿Ingresos?**
            * SI Ingresos = Bajo $\rightarrow$ **NO**
            * SI Ingresos = Medio $\rightarrow$ **SÍ**
        * SI Cal. Cred. = Justa $\rightarrow$ **¿Ingresos?**
            * SI Ingresos = Bajo $\rightarrow$ **SÍ**
            * SI Ingresos = Medio $\rightarrow$ **[SÍ/NO, 50% de probabilidad]** *(El conjunto no es separable con los atributos restantes)*

