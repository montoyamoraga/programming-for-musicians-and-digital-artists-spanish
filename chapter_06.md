# Unidades generadoras: objetos de ChucK para síntesis y procesamiento de sonido

Este capítulo cubre:
* Más osciladores de ChucK
* Generadores de envolvente
* Síntesis de frecuencia modulada
* Más sobre sonido y acústica, para motivar e inspirar
* Introducción a la síntesis de modelamiento físico

Bueno, aquí es donde se pone realmente muy bueno. Habiendo cubieto mucho sobre cómo funciona ChucK, podemos empezar a profundizar en los detalles de la síntesis y el procesamiento en ChucK. En el corazón del poder de ChucK para síntesis de audio y procesamiento está el concepto de UGens. Ya has usado UGens, empezando con nuestro ejemplo "Hola onda sinusoidal", donde hiciste ChucKing de un UGen SinOsc al dac para escuchar tu primer sonido. Continuaste con el uso de UGens en casi todos los programas que has escrito hasta ahora. Lo bueno es que no tienes que preocuparte sobre cómo cada UGen produce o procesa sonido; el diseñador de ese UGen se encargó de esa parte. Tú solo necesitas saber lo que pueden hacer y cómo se usan.

HEREIAM
page 118
page 118
page 119
page 120
page 121
page 122
page 123
page 124
page 125
page 126
page 127
page 128
page 129
page 130
page 131
page 132
page 133
page 134
page 135
page 136
page 137
page 138

## 6.1 UGens especiales de ChucK: adc, dac y blackhole

## 6.2 El oscilador de ancho de pulso: un clásico de la música electrónica

## 6.3 Unidades generadores tipo Envelope (envolvente, función lenta y suave)

### 6.3.1 Construyendo un sonido de clarinete usando SqrOsc y Envelope

### 6.3.2 Construyendo un sonido de violín con SawOsc y el ADSR UGen Envelope

## 6.4 Síntesis de frecuencia modulada

## 6.5 Síntesis de cuerda pulsada por modelamiento físico

### 6.5.1 La cuerda pulsada más simple

### 6.5.2 Excitando con ruido la cuerda pulsada

### 6.5.3 Modelamiento de decaimiento dependiente de frecuencia con un filtro

### 6.5.4 Modelamiento de delay fraccional (afinación) y agregando un ADSR para la pulsación

## 6.6 Introducción a Ugens de filtro: ganancia dependiente de frecuencia

## 6.7 Más sobre delays: acústica de espacios y reverberación

## 6.8 Efectos de audio basados en delay

## 6.9 Ejemplo: diversión con los UGens Filter y Delay

## 6.10 Resumen
