# 🌳 Árboles de Decisión: Guía Completa

Este documento explica qué son los árboles de decisión, cómo se construyen y proporciona ejemplos prácticos.

---

## 1. ¿Qué son los Árboles de Decisión?

Un **Árbol de Decisión** es un modelo de **aprendizaje automático supervisado** que se utiliza tanto para tareas de **clasificación** (predecir una categoría, como "Sí/No" o "Spam/No Spam") como de **regresión** (predecir un valor numérico, como el precio de una casa).

Visualmente, se asemeja a un diagrama de flujo o un árbol invertido. Está compuesto por:

* **Nodo Raíz (Root Node):** El punto de partida del árbol, que representa a todo el conjunto de datos.
* **Nodos Internos (Internal Nodes):** Representan una "pregunta" o una prueba sobre un atributo (una característica) del dato.
* **Ramas (Branches):** Representan el resultado de la "pregunta" (la respuesta). Cada rama conecta un nodo con otro.
* **Nodos Hoja (Leaf Nodes):** Son los nodos finales del árbol. Representan la decisión final o la predicción (una etiqueta de clase o un valor).



[Image of a simple decision tree diagram]


La idea principal es **dividir los datos** en subconjuntos cada vez más pequeños y "puros" (homogéneos) basándose en las preguntas de los nodos, hasta que se pueda tomar una decisión clara en un nodo hoja.

---

## 2. ¿Cómo se construyen?

La construcción de un árbol de decisión, generalmente usando algoritmos como **ID3**, **C4.5** o **CART**, sigue un proceso llamado **particionamiento recursivo**.

El objetivo es encontrar, en cada paso, la "mejor" pregunta (el mejor atributo) para dividir los datos. ¿Pero qué significa "mejor"?

Significa elegir el atributo que mejor separe los datos en grupos que sean lo más **homogéneos** posible. Para medir esto, se usan principalmente dos métricas:

1.  **Ganancia de Información (Information Gain) (Usada en ID3, C4.5):**
    * Mide la reducción de la **entropía**.
    * La *entropía* es una medida del desorden o la impureza en un conjunto de datos. Un conjunto con 50% de "Sí" y 50% de "No" tiene entropía máxima (máximo desorden). Un conjunto con 100% de "Sí" tiene entropía cero (totalmente puro).
    * El algoritmo elige el atributo que proporciona la **mayor ganancia de información**, es decir, el que más reduce el desorden.

2.  **Impureza de Gini (Gini Impurity) (Usada en CART):**
    * Mide la probabilidad de clasificar incorrectamente un elemento si se eligiera al azar.
    * Un valor de Gini de 0 significa que el nodo es perfectamente puro (todos los elementos son de la misma clase).
    * El algoritmo elige el atributo que resulta en la **menor impureza de Gini** promedio para los subconjuntos resultantes.

### Proceso de Construcción (Simplificado):

1.  **Empezar en el Nodo Raíz:** Se toma todo el conjunto de datos.
2.  **Encontrar el mejor atributo:** Se calcula la Ganancia de Información (o la Impureza de Gini) para *cada* atributo disponible.
3.  **Dividir (Split):** Se selecciona el atributo con la mejor métrica (mayor ganancia o menor impureza) y se convierte en el nodo de decisión.
4.  **Crear Ramas:** Se crea una rama para cada posible valor de ese atributo.
5.  **Repetir:** Para cada rama, se crea un nuevo subconjunto de datos. Se repiten los pasos 2 al 4 de forma recursiva para cada subconjunto.
6.  **Parar:** El proceso se detiene para una rama cuando:
    * El nodo resultante es **puro** (todos los datos pertenecen a la misma clase).
    * No quedan más atributos para dividir.
    * Se alcanzan condiciones predefinidas (como una profundidad máxima del árbol o un número mínimo de muestras por hoja), esto se hace para evitar el **sobreajuste (overfitting)**.

---

## 3. Ejemplo de Construcción (Clásico)

Imaginemos que queremos decidir si **"Jugar al Tenis" (Sí/No)** basándonos en el clima.

**Datos:**

| Cielo | Temperatura | Humedad | Viento | ¿Jugar? |
| :--- | :--- | :--- | :--- | :--- |
| Soleado| Caluroso | Alta | Débil | No |
| Soleado| Caluroso | Alta | Fuerte | No |
| Nublado| Caluroso | Alta | Débil | Sí |
| Lluvia | Templado | Alta | Débil | Sí |
| Lluvia | Frío | Normal | Débil | Sí |
| Lluvia | Frío | Normal | Fuerte | No |
| Nublado| Frío | Normal | Fuerte | Sí |
| Soleado| Templado | Alta | Débil | No |
| Soleado| Frío | Normal | Débil | Sí |
| Lluvia | Templado | Normal | Débil | Sí |
| Soleado| Templado | Normal | Fuerte | Sí |
| Nublado| Templado | Alta | Fuerte | Sí |
| Nublado| Caluroso | Normal | Débil | Sí |
| Lluvia | Templado | Alta | Fuerte | No |

**Construcción (Conceptual):**

1.  **Nodo Raíz (14 muestras):** 9 "Sí" y 5 "No". Es impuro.
2.  **Calcular Ganancia de Información:** El algoritmo calcula la ganancia para "Cielo", "Temperatura", "Humedad" y "Viento".
3.  **Mejor Atributo:** El algoritmo determina que **"Cielo"** es el atributo que mejor divide los datos.
    * *Rama "Soleado" (5 muestras):* 2 "Sí", 3 "No" -> (Impuro)
    * *Rama "Nublado" (4 muestras):* 4 "Sí", 0 "No" -> **(Puro -> Hoja: Jugar = Sí)**
    * *Rama "Lluvia" (5 muestras):* 3 "Sí", 2 "No" -> (Impuro)
4.  **Recursión (Rama "Soleado"):**
    * De las 5 muestras de "Soleado", el algoritmo ahora evalúa "Temperatura", "Humedad" y "Viento".
    * Descubre que **"Humedad"** es el mejor divisor.
    * *Rama "Humedad = Alta" (3 muestras):* 0 "Sí", 3 "No" -> **(Puro -> Hoja: Jugar = No)**
    * *Rama "Humedad = Normal" (2 muestras):* 2 "Sí", 0 "No" -> **(Puro -> Hoja: Jugar = Sí)**
5.  **Recursión (Rama "Lluvia"):**
    * De las 5 muestras de "Lluvia", el algoritmo evalúa los atributos restantes.
    * Descubre que **"Viento"** es el mejor divisor.
    * *Rama "Viento = Fuerte" (2 muestras):* 0 "Sí", 2 "No" -> **(Puro -> Hoja: Jugar = No)**
    * *Rama "Viento = Débil" (3 muestras):* 3 "Sí", 0 "No" -> **(Puro -> Hoja: Jugar = Sí)**

**Árbol Final Resultante:**

* **¿Cielo?**
    * `Soleado`:
        * **¿Humedad?**
            * `Alta`: **No Jugar**
            * `Normal`: **Sí Jugar**
    * `Nublado`: **Sí Jugar**
    * `Lluvia`:
        * **¿Viento?**
            * `Fuerte`: **No Jugar**
            * `Débil`: **Sí Jugar**

---

## 4. Ejercicio de Construcción y Solución

Construye un árbol de decisión para predecir si un cliente **"Comprará" (Sí/No)** un producto, basándote en los siguientes datos. Utiliza el criterio de **Ganancia de Información (ID3)**.

**Conjunto de Datos:**

| ID | Edad | Ingreso | Estudiante | ¿Comprará? |
| :- | :--- | :--- | :--- | :--- |
| 1 | Joven | Alto | No | No |
| 2 | Joven | Alto | Sí | Sí |
| 3 | Adulto | Alto | No | Sí |
| 4 | Joven | Bajo | No | Sí |
| 5 | Adulto | Bajo | Sí | Sí |
| 6 | Adulto | Bajo | No | Sí |
| 7 | Joven | Bajo | Sí | Sí |

---

### Solución del Ejercicio

#### Paso 1: Nodo Raíz

* **Datos Totales (7 muestras):** 6 "Sí" y 1 "No".
* Necesitamos calcular la Ganancia de Información (Gain) para "Edad", "Ingreso" y "Estudiante" para ver cuál es el mejor divisor.

* `Gain(Edad)`:
    * Joven (4 muestras): 3 "Sí", 1 "No" (Impuro)
    * Adulto (3 muestras): 3 "Sí", 0 "No" (Puro)
    * El cálculo de entropía muestra una ganancia (Gain) de **0.129**

* `Gain(Ingreso)`:
    * Alto (3 muestras): 2 "Sí", 1 "No" (Impuro)
    * Bajo (4 muestras): 4 "Sí", 0 "No" (Puro)
    * El cálculo de entropía muestra una ganancia (Gain) de **0.199**

* `Gain(Estudiante)`:
    * Sí (3 muestras): 3 "Sí", 0 "No" (Puro)
    * No (4 muestras): 3 "Sí", 1 "No" (Impuro)
    * El cálculo de entropía muestra una ganancia (Gain) de **0.129**

**Decisión Nivel 1:**
El atributo **"Ingreso"** tiene la Ganancia de Información más alta (0.199). Por lo tanto, es nuestro Nodo Raíz.

* **¿Ingreso?**
    * `Bajo`: (4 muestras: 4 "Sí", 0 "No") -> **Hoja: Comprará = Sí** (Este nodo es puro, terminamos esta rama).
    * `Alto`: (3 muestras: 2 "Sí", 1 "No") -> (Este nodo es impuro, debemos seguir dividiendo).

#### Paso 2: Sub-árbol (para Ingreso = Alto)

* **Datos restantes (muestras 1, 2, 3):** 2 "Sí" y 1 "No".
* Atributos restantes: "Edad" y "Estudiante".

* `Gain(Edad)` (para este subconjunto):
    * Joven (Muestras 1, 2): 1 "Sí", 1 "No" (Impuro)
    * Adulto (Muestra 3): 1 "Sí", 0 "No" (Puro)
    * Ganancia (Gain): **0.251**

* `Gain(Estudiante)` (para este subconjunto):
    * Sí (Muestra 2): 1 "Sí", 0 "No" (Puro)
    * No (Muestras 1, 3): 1 "Sí", 1 "No" (Impuro)
    * Ganancia (Gain): **0.251**

**Decisión Nivel 2:**
Hay un **empate**. Tanto "Edad" como "Estudiante" tienen la misma ganancia. En un empate, el algoritmo ID3 puede elegir arbitrariamente. Elegiremos **"Edad"**.

* **¿Ingreso?**
    * `Bajo`: **Sí**
    * `Alto`:
        * **¿Edad?**
            * `Adulto`: (Muestra 3: 1 "Sí", 0 "No") -> **Hoja: Comprará = Sí** (Puro).
            * `Joven`: (Muestras 1, 2: 1 "Sí", 1 "No") -> (Impuro, seguir dividiendo).

#### Paso 3: Sub-árbol (para Ingreso = Alto Y Edad = Joven)

* **Datos restantes (muestras 1, 2):** 1 "Sí" y 1 "No".
* Atributo restante: "Estudiante".

* `Gain(Estudiante)`:
    * No (Muestra 1): 0 "Sí", 1 "No" (Puro)
    * Sí (Muestra 2): 1 "Sí", 0 "No" (Puro)
    * Dividir por "Estudiante" crea dos nodos hoja puros.

**Decisión Nivel 3:**
Dividimos por **"Estudiante"**.

* `No`: **Hoja: Comprará = No**
* `Sí`: **Hoja: Comprará = Sí**

### Árbol de Decisión Final (Solución)