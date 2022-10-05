---
permalink: /
layout: home
title: Bienvenido
---

Este es un portfolio donde me propongo aplicar y desarrollar los conocimientos adquiridos en la materia Introducción a los Métodos de Aprendizaje Automático.

En formato de blog, se desarrollarán aquí casos de estudios destacados del curso y se analizarán aspectos teóricos, que por su dificultad o aplicabilidad sean de particular interés.

Asimismo, se realizará una serie de estudios sobre el tráfico público y privado de Montevideo basado en los datos abiertos públicados por la Intendencia de Montevideo. Los mismos están públicados [aquí][abiertosIM]. La IM publica gran cantidad de datos, sin embargo los más relevantes para el caso de estudio que me planteo son los siguientes:

* Conteo de vehículos: Es un data set en el que se registra el conteo o volumen de tráfico que pasa a través de puntos de fijos de control. El conteo se realiza en intervalos de 5 minutos, publicandose la cantidad de vehículos que pasaron en esos ultimos 5 minutos.
* Velocidad promedio: Es un data set similar anterior, sobre los mismos puntos de control, que realiza la medición de la velocidad promedio los vehículos en los últimos 5 minutos
* Viajes STM: En este data set se publican todos los viajes registrados por el Sistema de Transporte Metropolitano. En este data set cada fila representa un "boleto cortado". Asimismo, permite el seguimiento de viajes con etapas, ya que un único id de viaje unifica varias transbordos, estando cada etapa diferenciada por un ordinal.

Estos data set se publican de manera continua y mensual por parte de la Intendencia, existiendo solo una pequeña demora. Estando a principios de octubre al momento de escribir este post, ya he podido acceder a los datos de agosto. Los data set son muy completos, no encontrándose en ellos faltantes ni campos con valores extraños a su tipo. Conjuntamente con ellos se publica la metadata que ayuda a entender la información que esta consignada. Son muy grandes, con aproximadamente 1,7 millones de filas los primeros dos y 22 millones el de STM. 

Tal como están los data set, no tiene mucho sentido utilizarlos directamente en RapidMiner tal cómo realizamos con los casos de estudio en clase. Sin embargo a partir de ellos se puede derivar nueva información a partir de un estudio detallado. A continuación listaré algunas de las posibles:

* Principales líneas de omnibus y paradas, según la cantidad de pasajeros
* Tiempo de espera en una parada para una línea de omnibus en concreto
* Cantidad de personas esperando en una parada
* Prinicipales paradas donde se realizan transbordos
* Tiempo que un omnibus particular demora en realizar su recorrido y la posible brecha con el horario pautado

Toda esta información derivada, en conjunto con información de contexto, como por ejemplo registros de precipitaciones, de temperaturas y de horario, pueden ser utilizadas para realizar predicciones. 

[abiertosIM]: https://catalogodatos.gub.uy/organization/intendencia-montevideo