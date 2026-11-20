# Análisis de algoritmos

**Código:** 0192534  
**Estudiante:** Jhorman Esneider Ascanio Tarazona  

---

## 1. Intersecciones en una red de tuberías

### ¿Cuál es la complejidad de comprobar la intersección entre dos segmentos y cuándo se obtiene $O(n)$?

Comprobar si dos segmentos se cortan requiere una cantidad constante de operaciones aritméticas, así que cada comprobación individual es $O(1)$. Cuando se compara una tubería nueva contra las $n$ que ya existen en la red se hacen $n$ comprobaciones y el costo total es $O(n)$. Si lo que se quiere es revisar todos los pares posibles, hacen falta dos ciclos anidados.

### ¿Por qué el segundo ciclo debe comenzar en $i + 1$ al comparar todas las parejas de tuberías?

Las tuberías se enumeran de $1$ a $n$. Para cada tubería $i$, el ciclo interno va desde $j = i + 1$ hasta $n$. De esta manera nunca se compara una tubería consigo misma y cada par se analiza una sola vez. Por ejemplo, al llegar a la tubería 2 el par $(1, 2)$ ya se había comparado cuando $i$ valía 1.

### Construya una tabla de iteraciones para $n = 5$ y $n = 8$

| $i$ | $j$ ($n = 5$) | Comparaciones ($n = 5$) | $j$ ($n = 8$) | Comparaciones ($n = 8$) |
| :---: | :---: | :---: | :---: | :---: |
| 1 | de 2 a 5 | 4 | de 2 a 8 | 7 |
| 2 | de 3 a 5 | 3 | de 3 a 8 | 6 |
| 3 | de 4 a 5 | 2 | de 4 a 8 | 5 |
| 4 | solo 5 | 1 | de 5 a 8 | 4 |
| 5 | — | 0 | de 6 a 8 | 3 |
| 6 | — | — | de 7 a 8 | 2 |
| 7 | — | — | solo 8 | 1 |
| 8 | — | — | — | 0 |
| **Total** | | **10** | | **28** |

### Obtenga la expresión exacta del número de llamadas a intersección. Desarrolle la sumatoria y determine la complejidad temporal

Cada par de tuberías genera exactamente una llamada a `interseccion(a, b, c, d)`. El total es:

$$C(n) = \sum_{i=1}^{n} \sum_{j=i+1}^{n} 1 = \sum_{i=1}^{n} (n - i) = (n - 1) + (n - 2) + \dots + 1 + 0$$

Si esta suma se escribe también en orden inverso y se suman ambas, cada columna da $n - 1$ y hay $n$ columnas, por lo que $2C(n) = n(n - 1)$ y entonces:

$$C(n) = \frac{n^2 - n}{2}$$

Como el término dominante es $n^2$, la complejidad temporal es $\Theta(n^2)$.

### ¿Cuántas comprobaciones se hacen con 100, 1.000 y 40.000 tuberías?

| Cantidad de tuberías | Pares comprobados |
| :---: | :---: |
| 100 | $(100 \times 99) / 2 = 4.950$ |
| 1.000 | $(1.000 \times 999) / 2 = 499.500$ |
| 40.000 | $(40.000 \times 39.999) / 2 = 799.980.000$ |

---

## 2. Orientación del robot

### Calcule el determinante de cada uno de los cuatro casos. ¿Hacia qué lado debe girar el robot?

Tomando $AB$ como la dirección en la que avanza el robot, el signo del determinante dice de qué lado de esa dirección queda el punto $C$. Con el eje vertical creciendo hacia arriba, un valor positivo significa que $C$ está a la izquierda, un valor negativo que está a la derecha y cero que los tres puntos son colineales.

$$O(A, B, C) = (x_B - x_A)(y_C - y_A) - (y_B - y_A)(x_C - x_A)$$

| N.º de caso | Reemplazo de valores | Valor y dirección del giro |
| :---: | :---: | :---: |
| 1 | $(5 - 4)(7 - 2) - (3 - 2)(3 - 4)$ | $6 \rightarrow \text{izquierda}$ |
| 2 | $(6 - 2)(2 - 7) - (5 - 7)(8 - 2)$ | $-8 \rightarrow \text{derecha}$ |
| 3 | $(3 - 1)(6 - 7) - (3 - 7)(6 - 1)$ | $18 \rightarrow \text{izquierda}$ |
| 4 | $(5 - 0)(1 - 4) - (4 - 4)(3 - 0)$ | $-15 \rightarrow \text{derecha}$ |

---

## 3. Cómo detectar una pared en la ruta

### ¿Cómo se define la función `interseccion(a, b, c, d)` para detectar si una ruta atraviesa una pared o toca una esquina?

Sea $AB$ la ruta que se planea recorrer y $CD$ la pared. Se calculan cuatro orientaciones: $o_1 = O(A,B,C)$, $o_2 = O(A,B,D)$, $o_3 = O(C,D,A)$ y $o_4 = O(C,D,B)$. Hay un cruce propio cuando $o_1$ y $o_2$ tienen signos contrarios y, al mismo tiempo, $o_3$ y $o_4$ también los tienen.

Cuando alguna orientación es cero hay que verificar además que el punto realmente esté sobre el segmento y no solo sobre su prolongación, es decir, que quede dentro del rango de ambas coordenadas. Por ejemplo, si $o_1 = 0$, el punto $C$ está en $AB$ cuando:

$$\min(x_A, x_B) \le x_C \le \max(x_A, x_B) \quad \text{y} \quad \min(y_A, y_B) \le y_C \le \max(y_A, y_B)$$

Con esta revisión se detectan tanto los toques en una esquina como las superposiciones.

Así, la función de cuatro parámetros retorna verdadero si ocurre el cruce propio o si se presenta alguno de los cuatro casos de contacto. En la implementación de la sección 6 se calculan cuatro determinantes por cada par de segmentos, de modo que el número total de orientaciones evaluadas es $4C(n)$.

---

## 4. Cruce de las rutas de los autos

### Para cada pareja de rutas, dibuje los segmentos, calcule las cuatro orientaciones y determine si existe intersección, contacto o superposición

En los gráficos el origen está en la esquina inferior izquierda de la cuadrícula y cada cuadro equivale a una unidad. El segmento $AB$ se dibujó con línea continua y el segmento $CD$ con línea discontinua.

* **Caso 1.** $A(1, 1)$, $B(7, 7)$, $C(2, 6)$, $D(8, 1)$.  
  Orientaciones: `24`, `-42`, `-35` y `31`. En las dos parejas el signo cambia, por lo tanto las rutas se cruzan.  
  *(Insertar imagen del Caso 1)*

* **Caso 2.** $A(1, 7)$, $B(3, 3)$, $C(5, 5)$, $D(7, 7)$.  
  Orientaciones: `12`, `24`, `12` y `0`. Los puntos $C$ y $D$ quedan del mismo lado de $AB$, así que las rutas no se tocan.  
  *(Insertar imagen del Caso 2)*

* **Caso 3.** $A(0, 2)$, $B(6, 2)$, $C(3, 2)$, $D(9, 2)$.  
  Orientaciones: `0`, `0`, `0` y `0`. Todas valen cero porque los segmentos son colineales, y ambos comparten el tramo que va de $(3, 2)$ a $(6, 2)$.  
  *(Insertar imagen del Caso 3)*

---

## 5. Triangulación del terreno irregular

### Identifique los vértices convexos y cóncavos del terreno mediante el determinante de orientación

El borde del terreno se recorre en sentido antihorario con los vértices $A(0,0)$, $B(8,0)$, $C(8,6)$, $D(4,3)$ y $E(0,6)$. Para clasificar cada vértice se calcula la orientación formada por el vértice anterior, el vértice mismo y el siguiente.

| Vértice | Puntos evaluados | Valor | Tipo de vértice |
| :---: | :---: | :---: | :---: |
| A | EAB | 48 | Convexo |
| B | ABC | 48 | Convexo |
| C | BCD | 24 | Convexo |
| D | CDE | -24 | Cóncavo |
| E | DEA | 24 | Convexo |

### ¿Cuántos triángulos y diagonales internas requiere la triangulación del terreno?

En un polígono simple de $n$ vértices la triangulación genera $n - 2$ triángulos y usa $n - 3$ diagonales internas. Con $n = 5$ resultan **3 triángulos** y **2 diagonales**.

### Dibuje una triangulación válida del terreno

**Resolución:** Al trazar las diagonales $BD$ y $AD$, el terreno queda dividido en los triángulos $BCD$, $ABD$ y $ADE$.

*(Insertar imagen de la triangulación)*

### Identifique una primera oreja que pueda recortarse. ¿Qué condiciones deben verificarse antes?

$BCD$ puede tomarse como la primera oreja. El giro en $C$ es positivo ($+24$), la diagonal $BD$ queda dentro del terreno y ni $A$ ni $E$ están dentro del triángulo $BCD$ ni sobre su borde. Entonces se elimina $C$ y queda el cuadrilátero $ABDE$, al que se le añade la diagonal $AD$ para terminar la triangulación.

> **Verificación del área:** el área de cada triángulo es $\frac{\vert{}\text{determinante}\vert{}}{2} = \frac{24}{2} = 12$, y la suma de los tres da $36$ unidades cuadradas.

---

## 6. Implementación en Python

### Implemente en Python una función de cuatro parámetros para comprobar si dos segmentos se intersectan y aplíquela al conteo de cruces

```python
def giro(p, q, r):
    return ((q[0] - p[0]) * (r[1] - p[1])
            - (q[1] - p[1]) * (r[0] - p[0]))

def dentro_del_rango(p, q, x):
    return (min(p[0], q[0]) <= x[0] <= max(p[0], q[0])
            and min(p[1], q[1]) <= x[1] <= max(p[1], q[1]))

def se_cruzan(a, b, c, d):
    g1, g2 = giro(a, b, c), giro(a, b, d)
    g3, g4 = giro(c, d, a), giro(c, d, b)
    if g1 * g2 < 0 and g3 * g4 < 0:
        return True
    return ((g1 == 0 and dentro_del_rango(a, b, c))
            or (g2 == 0 and dentro_del_rango(a, b, d))
            or (g3 == 0 and dentro_del_rango(c, d, a))
            or (g4 == 0 and dentro_del_rango(c, d, b)))

def contar_intersecciones(tuberias):
    cruces = 0
    for i in range(len(tuberias)):
        for j in range(i + 1, len(tuberias)):
            if se_cruzan(*tuberias[i], *tuberias[j]):
                cruces += 1
    return cruces
```

### Implemente una función que reciba $n$ y calcule el número de triángulos y diagonales internas

```python
def datos_triangulacion(n):
    if n < 3:
        raise ValueError("Un polígono necesita al menos tres vértices")
    return n - 2, n - 3  # (triángulos, diagonales internas)
```

Por ejemplo, `datos_triangulacion(5)` retorna `(3, 2)`. Para las tres parejas de la sección 4, `se_cruzan` retorna `True`, `False` y `True`, en ese orden.

---

## 7. Comparación de algoritmos de triangulación

### Calcule $T_A(n)$ y $T_B(n)$ para 10, 100 y 1.000 vértices. Compare sus complejidades, calcule la razón $T_A(n)/T_B(n)$ y determine qué algoritmo conviene utilizar

El ejercicio plantea que el algoritmo A tiene un costo $T_A(n) = n^3$ y el algoritmo B un costo $T_B(n) = n^2$. Para ver cuánto más rápido crece uno que el otro se divide la cantidad de operaciones de A entre la de B.

| $n$ (vértices) | Operaciones de A: $n^3$ | Operaciones de B: $n^2$ | Cociente $A/B$ |
| :---: | :---: | :---: | :---: |
| 10 | 1.000 | 100 | 10 |
| 100 | 1.000.000 | 10.000 | 100 |
| 1.000 | 1.000.000.000 | 1.000.000 | 1.000 |

La razón se reduce a:

$$\frac{n^3}{n^2} = n$$

Es decir, con 100 vértices A hace 100 veces más operaciones que B, y con 1.000 vértices hace 1.000 veces más. De acuerdo con estos modelos conviene el **algoritmo B**, cuya complejidad es $O(n^2)$, mientras que A es $O(n^3)$.