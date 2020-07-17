---
layout: page
mathjax: true
title: 1. Nearest Neighbor Algorithms 
published: true
---
## $k-$NN Algorithms and how they may fail

**Pending sources.**

Let $\chi$ be our data set and $d$ an metric over $\chi$. 

$k-$NN type algoritms rely on the **Similarity on Nearby Behavior** principle. An *anomaly score* is computed for every $x \in \chi$, whose value is given by some function $f$ and the set $N_k(x):= ${ $x_i \in \chi: d(x,x_i) \leq d(x,x_{i+1}) \wedge k=1,...,k $} of the $k-$nearest neighbors. The habitual common behavior is said to be strongly related to small distances within points inside big masses or cores and outliers are labeled because of their clear isolation from those cores.


Any $k-$NN algorithm can be described by the way it complete the next three processes: 

- Definition of a neighborhood $N_{k}(x)$ for every point $x \in \chi$.
- Computing of the average behavior inside this neighborhood by means of a certain function $f$ over $N_{k}(x)$.
- Comparisson of the $f$ values.

### Local Outlier Factor (LOF) 

LOF algorithm was desing as an improvement of the basic $k-$NN and $k^{th}-$NN algorithms whose neighborhoods are defined by proximity 
$N_k(x):= ${ $x_i \in \chi: d(x,x_i) \leq d(x,x_{i+1}) \wedge k=1,...,k $}

Rougly speaking, LOF algorithm computes a cost function based on how much distance there is between $x$ and its $k$ nearest neighbors $x_1,...,x_k$. These in turn have their own $k$ nearest neighbors and so their cost distance associated. Finally, a certain function $f$, here called LOF, compares this cost of $x$ with the cost associated to each neighbor one by one. 

Aunque LOF aparenta ser el algoritmo definitivo no se tardó en encontrar situaciones donde su desempeño era pobre. En la figura 2  se tienen las siguientes condiciones: \cite{tang2002enhancing}\\

* $Card(C_1)=91$ and given two consecutive points $x,y \in C_1$, equation $d(x,y)=1$ holds.
* 8 uniformly distributed points over the unit disk assemble $C_2$ set.
* $C_1$ middle point, $C_2$ center and $o_1$ are collinear points.
* $d(C_2,o_1)=1000$ y $d(C_1,o_1)=2$
* Points belonging to $C_2 \cup$ {$ o_1 $} are anomalous.

<center>
<img src="https://user-images.githubusercontent.com/67338552/86149156-33478100-bac1-11ea-90ef-e990eadf6972.png" height="200" width="400">
</center>

Under these conditions,  $k \leqslant 7$ LOF algorithm does not label any point in $C_2$ as anomalous (e.g., $k=4$ means $LOF_4(o_1)=2,055$ but $LOF_4(x_i)=1$ for every $x_i \in C_2$ so the Local Reachability Distance is the same for every point). Otherwise, $k \geq 8$ implies LOF algorithm does not label $o_1$ as anomalous because $LOF_8(o_1)=1,4209$ 
and $LOF(\overline{x_1})=1,0951$, where $\overline{x_1}$ is the middle point of $C_1$ and $x_1$ is the nearest point to $o_1$ in $C_2$ while $LOF(x_1)=6,425$. Therefore the anomaly score given to $o_1$ is pretty small in front of $x_1$ score in such a way it may be confused as a $C_1$ point. 

 Nótese que las escalas de anomalía se incrementan progresivamente a medida que un punto se acerca a los bordes de la recta. Esto ocurre porque la forma de medir, en este caso, mediante la métrica usual de $\mathbb{R}^2$, obliga a los puntos en los extremos a buscar puntos vecinos muy lejanos dado que todos ellos están a un solo lado, (derecha o izquierda dependiendo del extremo en que se encuentre). \\