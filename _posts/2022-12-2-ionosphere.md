---
title: "Ionosfera"
layout: post
---

En la investigación ionosférica, se deben clasificar las lecturas de radar como utilizables para el analisis o no. Es una tarea demandante que requiere intervención humana. Se ha consultado un paper que analiza el uso de MLFN (multilayer feedforward networks) para realizar la tarea de clasificación. En su caso, obtuvieron una accuracy de 100% para el set de entrenamiento y de 98% para el set de prueba. Aquí se utilizarán los algoritmos Knn, Random Forest y Regresión Logística, para comparar los resultados obtenidos.

## Anális de los datos

Los datos son obtenidos por un radar ubicado en Goose Bay, Labrador. Consiste de 16 antenas de alta frecuencia y otra de frecuencia entre 8 a 20 MHz. El radar emite un patrón de multipulso a la atmósfera. Entre cada pulso, un receptor es encendido, que realiza las mediciones. La medición es un numero complejo, es decir, está compuesta por un coeficiente real y un coeficiente ireal. Por tanto, cada dato está compuesto de 17 pares de predictores (uno por cada antenta). Además, el dataset cuenta con un atributo label, que es el que clasifica la medición como buena o no, los valores puede ser `g` (good) o `b` (bad). Es por tanto un problema de clasificación binaria. Un 61,4% corresponde a `g` y 35,9% corresponde a `b`

El dataset ya está normalizado al rango [-1, 1], no hay faltantes y se cuenta con label para todos los datos.

## Proceso

<img src="https://mgonnet.github.io/IAportfolio/assets/imgs/ionosfera_proceso.png" >

Se utiliza un cross validation, con 50 folds y strattified sampling para evaluar la performance de cada algoritmo. 

Random Forest fue configurado con 100 árboles y una profundidad máxima de 30. La regresión logística fue configurada con sus parámetros por defecto y el Knn con n = 3.

<img src="https://mgonnet.github.io/IAportfolio/assets/imgs/ionosfera_regresionLogisticaParametros.png" >

## Análisis de resultados

<img src="https://mgonnet.github.io/IAportfolio/assets/imgs/ionosfera_performance_knn.png" >
<img src="https://mgonnet.github.io/IAportfolio/assets/imgs/ionosfera_performance_logisticregression.png" >
<img src="https://mgonnet.github.io/IAportfolio/assets/imgs/ionosfera_performance_randomForest.png" >

Se observa que el algoritmo que obtiene la mejor accuracy es Random Forest, con 93,21%. Esto se debe en gran medida a que obtiene el mejor valor de recall para la clase `b` (bad). El recall es de 88,89%, 77,78% y 61,11% para Random Forest, Regresión Logística y Knn respectivamente.

Knn, si bien tiene una mala accuracy en general, presenta un buen recall para la clase `g`, de 98,22%. El mal rendimiento esta causado por el bajo recall para `b` de 61,11%.
En el paper no se menciona si existe alguna preferencia en particular sobre si es preferible un mejor recall para `g` o `b`. Si no afectara tener falsos `g`, Knn sería una buena alternativa a considerar. Sin embargo se estima como más probable que se prefiera descartar aquellos datos que sean `b`.

La regresión logística obtiene resultados similares para el recall de `g`, pero también malos resultados para el recall de de `b`. 

Por tanto el mejor modelo para este caso es el obtenido con Random Forest. Este obtiene una alta accuracy, pero aun existe el riesgo de tomar como buena una señal que en realidad era mala (falso positivo). El modelo planteado en el paper es por tanto mejor que los tres aquí considerados.

Sin embargo, bajo ciertos supuestos el modelo Random Forest puede aún resultar útil. Supongase que el proceso de tomar las mediciones no es muy costoso, que el mayor costo está en realizar los estudios en base a las mediciones. Por tanto, los investigadores querrán asegurarse de que las mediciones con las que trabajan son buenas, aun si tienen que realizar un mayor número de mediciones para contar con las suficientes. En este caso, puede utilizarse la técnica de Lift Charts, proveniente del marketing, para obtener una seleccionar aquellas mediciones sobre las que se tiene una gran confianza de que son buenas.

<img src="https://mgonnet.github.io/IAportfolio/assets/imgs/ionosfera_liftchart.png" >

En este lift chart, dividido en deciles, se observa que en los primeros 4 deciles, se capturan 140 mediciones buenas, y se tiene un único falso positivo. Si los investigadores utilizan este método para seleccionar sus mediciones, estarían obteniendo 99,2% señales buenas. Mientras que, si el dataset trabajado es representativo de la realidad, eligiendo muestras al azar hubieran obtenido solo 61,4% señales buenas.

Se concluye entonces que el modelo Random Forest, utilizando una selección con Lift Charts, podría ser mejor que el modelo presentado en el paper (99,2% vs 98%), si se da la condición de que es suficientemente barato tomar suficientes mediciones como para poder descartar mediciones potencialmente buenas.

## Notas adicionales

Se intentó realizar un sampleo para entrenar el modelo con un 50 - 50 de mediciones buenas y malas y no se obtuvieron mayores diferencias.


*Acceso al paper estudiado* 
https://www.jhuapl.edu/Content/techdigest/pdf/V10-N03/10-03-Sigillito_Class.pdf

*Dataset*
https://archive-beta.ics.uci.edu/dataset/52/ionosphere