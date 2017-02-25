# Unidades generadoras: objetos de ChucK para síntesis y procesamiento de sonido

Este capítulo cubre:
* Más osciladores de ChucK
* Generadores de envolvente
* Síntesis de frecuencia modulada
* Más sobre sonido y acústica, para motivar e inspirar
* Introducción a la síntesis de modelamiento físico

Bueno, aquí es donde se vuelve realmente bueno. Habiendo cubieto mucho sobre cómo funciona ChucK, podemos empezar a profundizar en los detalles de la síntesis y el procesamiento en ChucK. En el corazón del poder de ChucK para síntesis de audio y procesamiento está el concepto de UGens. Ya has usado UGens, empezando con nuestro ejemplo "Hola onda sinusoidal", donde hiciste ChucKing de un UGen SinOsc al dac para escuchar tu primer sonido. Continuaste con el uso de UGens en casi todos los programas que has escrito hasta ahora. Lo bueno es que no tienes que preocuparte sobre cómo cada UGen produce o procesa sonido; el diseñador de ese UGen se encargó de esa parte. Tú solo necesitas saber lo que pueden hacer y cómo se usan.

En este capítulo, introduciremos algunos de los UGens incluidos en ChucK que puedes usar para hacer que tu producción y procesamiento de música y sonida sea muy fácil. Usarás un nuevo tipo de oscilador (PulseOsc) para hacer sonidos techno de ciencia ficción. Hasta ahora has usado la ganancia para encender y apagar tus sonidos y notas musicales de forma abrupta, pero en este capítulo aprenderás y usarás generadores de envolvente, que generan funciones suaves en el tiempo. Con envolventes, serás capaz de encender y apgar tus notas de forma suave, como un DJ haciendo transiciones en su tornamesa con controles deslizantes de volumen, o un violinista iniciando y deteniendo el arqueado de un violín, el que cuando no es abrupto, presenta rampas suaves al principio y al final. Introduciremos la síntesis de frecuencia modulada (FM), así como también la síntesis y procesamiento de sonido por modelamiento físico, donde en vez de sintetizar una forma de onda de salida deseada, modelas y calculas la física de las ondas al vibrar y propagarse (viajar) dentro de instrumentos y habitaciones. En este capítulo construirás modelos físicos simples de un instrumento de cuerda pulsada y de los patrones de reverberación de una habitación. Definiremos el escenario para el siguiente capítulo donde veremos muchos de los instrumentos UGen incluidos en ChucK de modelamiento físico. ¡Empecemos!

> Historia de la síntesis: unidades generadoras

> La idea de UGen viene de hace mucho tiempo atrás, de los tiempos de los sintetizadores análogos donde osciladores, fuentes de ruido y otros generadores de señal eran conectados a través de filtros y otros procesadores en mezcladores y eventualmente a ampificadores y parlantes. En 1967, cuando muchos pioneros de la síntesis estaban trabajando con síntesis análoga, Max Mathews (considerado por lmuchos el Padre de la Música por Computador) en Bell Laboratories ideó (e implementó) la idea de sintetizar sonido y música digitalmente conectando UGens para formar instrumentos y usando otros programas para leer partituras para controlar todo esto y hacer música. La mayoría de los lenguajes de música por computador (incluyendo ChucK) desde entonces se han alineado con esta idea básica.

## 6.1 UGens especiales de ChucK: adc, dac y blackhole

Has estado usando el UGen dac a lo largo de este libro, permitiéndote conectar otros UGens al mundo exterior de tus parlantes o audífonos a través de tu hardware de sonido. ChucK tiene dos UGens especiales para conectarte con el mundo exterior a través de tu hardware de audio. La razón que sean especiales es que estos tres UGens (adc, dac y blackhole) siempre están ahí, lo que significa que no tienes que declararlos como otros UGens como SinOsc, SndBuf o Impulse. Recuerda que cuando usaste estos otros tipos de unidades generadoras, tuviste que declararlos de esta forma:

```chuck
SinOsc s => dac;
```

o

```chuck
Impulse imp => ResonZ filter => dac;
```

Pero no tuviste que declarar un nombre de variable dac, porque dac está siempre ahí, y solo hay uno de elllos. Así que simplemente lo llamas dac.

El otro UGen especial para el mundo exterior es llamado adc (por analog-to-digital converter, conversor análogo digital), y te permite obtener sonido en tu computador desde un micrófono interno, una entrada de micórono o señal de línea, u otras entradas a través de una tarjeta de sonido externa conectada. Para probar esto, puedes hacer una conexión entre la salida y la entrada de tu computador. Puedes usar el código del siguiente listado.

Listado 6.1 Conexión de la entrada de audio a la salida usando adc y dac

```chuck
//conecta la entrada de audio a la salida de audio a través de un UGen Gain
adc => Gain g => dac;

//deja que transcurran 10 segundos
10.0 :: second => now;
```

¡PRECAUCIÓN! ¡PRECAUCIÓN! ¡PRECAUCIÓN! Sé extremadamente

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
