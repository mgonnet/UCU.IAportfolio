---
title: "Vertebral Column Dataset"
layout: post
---
# Vertebral Column Dataset

Se analiza un dataset conteniendo seis atributos biomecanicos sobre la forma y orientacion de la pelvis y la espina lumbar. Se intenta clasificar en tres clases, normal `(NO)`, disk hernia `(DH)` o spondilolysthesis `(SL)`. 

## Análisis de los datos

Se cuentan con 310 casos, repartidos en 150 para SL, 100 para NO y 60 para DH. No existen datos faltantes para ningún predictor. Las magnitudes de todos los campos son del mismo órden y no existen outliers, por lo que en primera instancia no se considera necesario realizar una normalización. Existe un único outlier, que presenta valores desproporcionados para el atributo `degree_spondylolisthesis`. Tiene un valor de 418,54, cuando el promedio una vez retirado es 25,07. Ya que el problema es de clasificación y no de detección de anomaly, se decide retirar.

Sabiendo que se trata de un problema de clasificación en tres clases, se utiliza el algoritmo de Kmeans de clustering, configurado con K = 3, para analizar que tan separables pueden ser las clases. A continuación se muestran la distribución de las clases dentro de cada cluster.


|      |SL    |DH   |NO   |
|------|------|-----|-----|
|Cl_0  |85,4% |2,2% |12,4%|
|Cl_1  |8,1%  |36,2%|55,6%|
|Cl_2  |100%  |0%   |0%   |

Se nota que el clustering pudo separar con facilidad los casos de `SL` del resto, sin embargo hay grandes problemas para separar los `DH` de `NO`.

## Procesamiento

Se utiliza el algoritmo Knn para realizar la clasificación. Durante la investigación se realizaron pruebas también con el algoritmo Naive Bayes, pero se obtuvieron siempre resultados inferiores.

Previendo que haya problemas para diferenciar los casos `DH` de los casos `NO` se utilza el operador MetaCost. Obtener un valor de `No` siendo en realidad cualquiera de los otros dos, es muy malo como diagnóstico, ya que estaría perdiendo la oportunidad de realizar un tratamiento sobre la persona, lo que tiene grandes impactos sobre su salud. El operador MetaCost es recomendado en la documentación de Rapidminer precisamente para aquellos casos en que se quiera mejorar el recall de una clase especifica, indicando la relación de costos en una matriz.

![](../_images\columna_metacost.png)

Aquí se muestra cómo en el operador Metacost se asigna un costo 6 veces mayor a predecir como `NO` aquellos casos que en realidad eran `DH` o `SL`. (DH corresponde a la Class 1, SL a la Class 2 y NO a la Class 3).

De todas formas se realiza un Knn normal, sin MetaCost, a fin de realizar comparaciones. En ambos casos el entrenamiento del modelo se realiza dentro de un operador Cross Validation. Luego de varias pruebas, el Knn se configura con K = 6.

![](../_images\columna_proceso.png)
![](../_images\columna_proceso_knn.png)
![](../_images\columna_proceso_metacost.png)

## Análisis de resultados

![](../_images\columna_resultado_knn.png)
![](../_images\columna_resultado_metacost.png)

### Knn
Se observa que como se anticipó, el Knn solo tuvo gran éxito a la hora de predecir que casos son `SL`, obteniendo un recall de 95,3%; pero que tuvo grandes problemas para diferenciar casos de `DH` y en cierta medida `SL` de casos `NO`. En total 28 casos fueron dados de alta estando en realidad enfermos.

### MetaCost
Por su parte, el modelo que combina MetaCost con Knn, obtiene muy buenos valores de recall para `DH` y `SL`. Significativamente solo un caso predicho como `NO` terminó siendo en realidad un `DH` y ninguno un `SL`. Esto es sumamente positivo a la hora de realizar diagnósticos, ya que se corre un muy bajo riesgo de dar de alta un paciente con enfermedad. Una forma alternativa de observar esto es que la precisión de la clase `NO` es 97,44%

La contracara de esto es que hay un bajo recall para la clase `NO`, de 38%. Esto es causado principalmente por una baja precisión de para la clase `DH`. De los casos clasificados como `DH`, un 45,5% en realidad no tenía ninguna de las dos enfermedades. 

Por su parte para la clase `SL` se obtuvieron buenos valores de recall y precision, 97,99% y 92,41% respectivamente.

### Conclusión

* Se concluye que el modelo con MetaCost y Knn es muy confiable para identificar la enfermedad spondilolysthesis, con una precisión de 92,41%.
* También es útil para descartar casos de no enfermedad, con una precisión de 97,44%
* Sin embargo, el médico deberá analizar cuidadosamente aquellos casos clasificados como disk hernia, ya que en realidad solo un 51,79% realmente la tienen.