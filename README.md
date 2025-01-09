---
title: Teoría del árbol con particionamiento recursivo binario
author: Javier Carela
date: 10/2024
output:
  prettydoc::html_pretty:
    theme: tactile
    highlight: github
    toc: yes
    toc_depth: 3
---

# Introducción

Antes de empezar con el algoritmo, se introducirán algunos conceptos

- **Modelo de regresión**: Un modelo de regresión proporciona una función que describe la relación entre una o más variables independientes y una variable de respuesta.

- [**Particionamiento recursivo**](https://en.wikipedia.org/wiki/Recursive_partitioning): Proceso de partición, que divide la población , y a su vez, divide las diferentes subpoblaciones bajo el mismo criterio hasta que alcanza un criterio de detención previamente especificado.


- [**Distrubución de probabilidad condicionada**](https://en.wikipedia.org/wiki/Conditional_probability_distribution): Se define como $D(Y|X)$ la probabilidad de la distribución de $Y$, dadas los valores particulares de $X$.

- [**Función indicatriz**](https://es.wikipedia.org/wiki/Funci%C3%B3n_indicatriz): Función definida sobre un conjunto $X$  que indica la pertenencia o no en un subconjunto $A$ de $X$. Sea $I:X \rightarrow \{1,0\}$. 

 \begin{equation}
 I(x) =
   \left\{\begin{array}{lr}
       1 , & x \in A \\
       0, & x \notin A 
    \end{array}\right.
 \end{equation}
 
# Marco
Sea $Y$ la variable respuesta y $X$ con $m$ covariables, de forma que $X = (x_{1}, \ldots	 , x_{m})$  con su espacio $\chi = \chi_{1} \times \dots	 \times \chi_{m}$.
Suponemos que hay una distribución condicional $D(Y|X)$ de la respuesta $Y$ dadas las covariables $X$ 
$$D(Y|X) = D(Y|x_{1}, \ldots	 , x_{m})= D(Y|f(x_{1}, \ldots	 , x_{m}))$$ 
Siendo $f$ una función de covariables.
Aplicamos la restricción disjunta $B_{1},...B_{k}$ del espacio $\chi=\bigcup_{k=1}^{r} B_k$.
Ahora se ajusta una muestra de aprendizaje $\mathcal{L}_{n}$ con $n$ aleatoria, independiente e identicamente distribuida, con posiblemente algún $X_{ij}$ faltante.
$$ \mathcal{L}_{n}:=\{(Y_{i},x_{1i}, \ldots , x_{mi}); i = 1, \ldots , n \}$$
Una vez definido estos subcojuntos, aplicaremos un algoritmo para la partición recursiva binaria, asignando valores no negativos sobre un vector $w = (w_{1}, \ldots , w_{n})$ denominado **vector de pesos**.
Cada nodo del árbol es representado por un vector de pesos que tiene elementos distintos de cero cuando las observaciones pertenecen al nodo, y 0 en caso contrario.
Notese que la longitud de todos los vectores $w$ en cada nodo será la misma.

# Algoritmo
1. Para los pesos $w$ se prueba la hipótesis nula global de **independencia de variables** entre cualquiera de las  $m$ covariables  e $Y$.
Se detiene si **NO rechaza** la hipótesis. En caso contrario, selecciona la j-ésima covariable $X_{j}^{*}$ con la asociación más fuerte con *Y*.



2. Escoge $A^{*} \subset \chi_{j^{*}}$ para particionar $\chi_{j^{*}}$ en dos subconjuntos disjuntos $A^{*}$ y $\chi_{j^{*}}/A^{*}$.
El vector de pesos $w_{izq}$ y $w_{der}$ determina los dos subgrupos con 
$$w_{izq}=w_{i}I(X_{ij} \in A^{*} ) \\
w_{der}=w_{i}I(X_{ij} \notin A^{*} ) \\
\forall i=1, \ldots,n
$$
Donde $I( · )$ es la función indicatriz.


3. Se repite recursivamente el paso 1. y el paso 2. con las modificaciones respectivas en $w_{izq}$ y $w_{der}$

Es importante señalar que nos detenemos cuando la hipótesis nula global de independencia entre la respuesta y cualquiera de las covariables no puede rechazarse. El algorítmo induce una partición $B_{1},...B_{k}$ del espacio de covariables $\chi$ donde cada $B \in B_{1},...B_{k}$ está asociada con un vector de pesos. 

# Referencias
- [**Hothorn, T., Hornik, K., & Zeileis, A. (2006)**](https://www.zeileis.org/papers/Hothorn+Hornik+Zeileis-2006.pdf). Unbiased Recursive Partitioning: A Conditional Inference Framework. Journal of Computational and Graphical Statistics, 15(3), 651–674. https://doi.org/10.1198/106186006X133933 
