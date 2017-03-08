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

¡PRECAUCIÓN! ¡PRECAUCIÓN! ¡PRECAUCIÓN! Sé extremadamente cuidadoso antes de ejecutar este código, asegurándote que no haya un bucle de retroalimentación que te haga doler los oídos (como puede ocurrir cuando el sonido de tus parlantes es captado por tu micrófono). Conecta audífonos, o baja mucho el volumen de tus parlantes antes de conectar el adc al dac (como en este código).

Estrictamente hablando, no necesitas el UGen Gain entre el adc y el dac, pero deberías siempre evitar la conexión directa entre adc y dac. La razón de esto es que adc y dac son los únicos UGens persistentes en ChucK, y una vez conectados permanecen conectados hasta que sean explícitamente desconectados (haciendo unChucKing con =<). En el listado 6.1, sin embargo, el UGen Gain que llamaste g desaparecerá después de que el código haya finalizado su ejecución, rompiendo así la conexión de entrada con salida. Observa que no tuviste que declarar una variable de tipo adc y darle un nombre, como sí tuviste que hacerlo con el UGen Gain, y que no tuviste que ponerle un nombre a tu dac. Tanto adc como dac son únicos en esta manera. Todas las otras unidades generadoras requieren que tú hagas una variable y la nombres si la quieres usar.

AHora sabes cómo escuchar un micrófono conectado o incluido en tu computador, ¿pero qué pasa si quieres usar un micrófono para detectar si hay un sonido fuerte cerca, para poder hacer cosas interesantes basadas en esto, pero no quieres escuchar directamente el sonido de tu micrófono? ChucK tiene otro UGen especial llamado blackhole (hoyo negro), que actúa como un dac que no está conectado a ningún dispositivo de sonido. Existen muchas razones para que tú quieras conectar sonido a una salida que no emite sonido, pero la principal es que si quieres hacer procesamiento de señal de algún tipo (como detección de altura), o para inspeccionar una señal (como el detector de sonidos fuertes que mencionamos anteriormente), sin conectar ese camino directamente a tu salida de sonido. El UGen blackhole sirve para succionar samples desde cualquier UGen que está conectado a el. Estos UGens no computan ningún nuevo sonido de otra forma, porque ChucK es astuto para lograr eficiencia y no calcula ningún sample que no esté conectado a algún otro lugar. El blackhole sirve para usar estos samples, aunque nunca los escuches.

El listado 6.2 es un ejemplo de uso de blackhole para hacer algo útil, específicamente para mantener en observación la entrada de adc e imprimir un mensaje si las cosas se ponen muy fuertes en volumen (1). El g.last() del if condicional (2) retorna el último sample que ha pasado a través del Gain Ugen, Puedes cambiar el valor de comparación (0.9) por cualquier valor que quieras. Por ejemplo, cambiarlo 0.001 hace que el programa imprima incluso para entradas de bajo volumen (quizás en todo momento) y cambiarlo a 10.0 significa que nunca imprimirá nada. Este tipo de detector de picos de audio es útil para muchas cosas en la vida real, como control automático de ganancia o instalaciones de arte que responden a sonido.

Listado 6.2 Un detector de picos de audio en ChucK

```chuck
//(1) blackhole succiona samples desde el adc a través de Gain
adc => Gain g => blackhole;

while(true)
{
  //si es lo suficientemente fuerte
  //(2) .last() obtiene el último sample de cualquier UGen
  if (g.last(0.9) > 0.9)
  {
    //imprime el valor
    <<< "¡¡FUERTE!!", g.last() >>>;
  }  
  //haz esto para cada sample
  samp :: now;
}
```

NOTA Este código se aprovecha de tu habilidad  de manipular tiempo en ChucK de una forma completamente flexible, en este caso avanzando el tiempo un sample a la vez, para que puedas mirar valores individuales a medida que llegan desde el ADC.

## 6.2 El oscilador de ancho de pulso: un clásico de la música electrónica

¿Recuerdas cuando escribiste tu primer programa de ChucK para hacer sonido ("Hola onda sinusoidal")? Usaste un UGen IsnOsc, conectándolo al dac para hacer tu primera nota (sonido puro con altura). También hablamos sobre y usamos unos cuantos de los otros UGens tipo oscilador: TriOsc, SawOsc y SqrOsc. ChucK tiene más UGens tipo osciladores, y vamos a usar unos de esos ahora para grandes sonidos y música de tipo electrónica bailable.

El UGen PulseOsc genera un pulso cuadrado (como SqrOsc), per puedes también controlar la fracción de cada periodo que es alta versus baja (esto es llamado el ancho del pulso, o ciclo de trabajo). Puedes definir o variar el ciclo de trabajo de PulseOsc en cualquier punto entre 0.0 y 1.0, para crear distintos espectros de sonido (un ciclo de trabajo pequeño arroja un espectro muy brillante, 0.5 es menos brillante). La figura 6.1 muestra la salida de PulseOsc para dos distintos anchos: 0.1 (alto solo el 10% del tiempo) y 0.5 (igualmente alto y bajo el 50% del tiempo, como una onda cuadrada).

La configuración 0.5, también llamada ciclo de trabajo 50%, genera la misma forma de onda que un SqrOsc. Estos osciladores con anchos de pulso variables con comúnmente usados en música electrónica bailable, usualmente cambiendo el ancho del pulso al ritmo de la música. Hacer un barrido dinámico del ancho de pulso puede crear también efectos de sonido de ciencia ficción.

El código en el listado 6.3 genera una línea de bajo techno bailable, usando un PulseOsc conectado al dac como fuente sonora. En el bucle infinito principal, defines el ancho del pulso aleatoriamente entre 0.01 (realmente puntiagudo, por lo tanto sonido muy brillante) y 0.5 (cuadrado, sonido más meloso). También usas Math.random2(0, 1) para lanzar una moned y determinar una entre dos alturas para tu onda pulso. La frecuencia más grave de 84 Hz es cercana a la nota musical E2 (la cuerda más grave en una guitarra) y la frecuencia de 100 es cercana a la nota G2 sobre esa. Para obtener un ritmo, alterna el encendido y el apagado del oscilador cada décima de segundo, enciende durante 60 milisegundos (0.06 segundos) y apaga por 40 milisegundos (0.04 segundos).

Listado 6.3 Línea de bajo de techno y ciencia ficción usando el UGen PulseOsc

```chuck
//PulseOsc para techno en bajo, por programador de ChucK, 2014
//conecta un nuevo PulseOsc al dac
PulseOsc p => dac;

//bucle infinito de techno tipo ciencia ficción
while (true)
{
  //define un ancho de pulso aleatorio
  Math.random2f(0.01, 0.5) => p.width;
  //selecciona una altura aleatoria
  //entre dos posibilidades
  if (Math.random2(0, 1))
  {
    84.0 => p.freq;
  }
  else
  {
    100.0 => p.freq;
  }

  //enciende el oscilador y transcurre un poco de tiempo
  1 => p.gain;
  0.06 :: second => now;

  //apaga el oscilador y transcurre un poco de tiempo
  0 => p.gain;
  0.04 :: second => now;

}
```

## 6.3 Unidades generadores tipo Envelope (envolvente, función lenta y suave)

Hasta el momento en tus programas, para separar notas repetidas o para hacer sonidos con ritmo, has encendido y apagado tus osciladores cambiando la ganancia en algún lugar de tu cadena de sonido. Lo has configurado a cero para silencio y a no-cero para hacer sonido, cambiando entre ambos para tocar notas individuales. No obstante, la mayoría de los sonidos y las notas musicales no funcionan de esa manera en la naturaleza: cuando soplas en un clarinete o trompeta, o empiezas a arquear un violín, o empiezas a cantar, las notas no se encienden instantáneamente. Cuando dejas de soplar, arquear o cantar, no escuchas un click porque el sonido se ha detenido instantáneamente. En la mayoría de los instrumentos, tú puedes escogar entre comenzar una nota suavemente y aumentar el volumen o empezar con mayor volumen e ir decayendo. Debe existir una mejor manera en ChucK para encender y apagar tus notas de forma más gradual, y de hecho la hay.

Los Ugens Envelope (envolvente) incluidos en ChucK generan rampas hacia arriba y abajo para controlar volumen y otros parámetros que quieas cambiar lentamente. El UGen Envelope hace una rampa de 0.0 a 1.0 en respuesta un mensaje .keyOn (encender nota), en un tiempo definido por el método .time (tiempo). El Ugen Envelope hace una rampa de vuelta a cero en respuesta a un mensaje .keyOff (apagar nota).

### 6.3.1 Haciendo un sonido de clarinete usando SqrOsc y Envelope

En el capítulo 1 hablamos sobre cómo una onda cuadrada puede sonar parecida a un clarinete y cómo la física de las cuerdas frotadas generan una onda parecida una diente de sierra. Ahora, aplicando un Envelope a SqrOsc, puedes construir un clarinete muy simple que empieza y finaliza las notas de forma apropiada (gradualmente).

El listado 6.4 muestra un ejemplo simple del uso de UGens Envelope y SqrOsc para hacer un sonido de clarinete. Observa que conectas el oscilador a través de Envelope al dac (1). Después de definir una frecuencia inicial (2), usas un bucle while (3) para tocar notas individuales por medio del accionamiento del Envelope (4), esperando un poco para dejar que suba suavemente, y luego apagando el Envelope (5) (nuevamente esperando un poco antes de hacer una rampa descendente). Usamos un bucle para tocar lo que se llama una "serie armónica", aumentando la altura en 55 Hz cada vez (6).

```chuck
//síntesis simple de un clarinete
//Envelope aplicado a SqrOsc
//(1) La onda cuadrada se asemeja a un clarinete
SqrOsc clar => Envelope env => dac;
//(2) define la altura inical (nota A)
55.0 => clar.freq;
//toca una serie armónica
//(3) itera sobre tres octavas de altura
while (clar.freq() < 441.0)
{
  //gatilla la envolvente
  //(4) Envelope.keyOn inicia la nota
  1 => env.keyOn;
  //espera un poco
  0.2 :: second => now;
  //haz que la envolvente haga una rampa descendente
  //(5) Envelope.keyOff finaliza la nota
  1 => env.keyOff;
  //espera un poco
  0.2 :: second => now;
  //siguiente nota de la serie de sobretonos
  //(6) aumenta la altura, subiendo por la serie armónica
  clar.freq() + 55.0 => clar.freq;
}
```

El lado izquierdo de la figura 6.2 muestra la forma de onda generada por una nota única del código del listado 6.4. Observa que la nota empieza y termina de forma gradual, en vez de prenderse y apagarse (como se muestra en comparación en el lado derecho de la figura 6.2).

Existen otros métodos que puedes usar con Envelope, como causar que se mueva a un valor objetivo arbitrario en respuesta al mensaje .target (objetivo), o puedes definir la salida de forma inmediata con el método .value (valor). Por ejemplo, 0.5 => env.target causa que el valor de la envolvente haga una rampa a 0.5 (sin importar el valor actual) y permanece ahí una vez que alcanza el valor 0.5. Invocar 0.1 => env.value causa que inmediatamente se emita ese valor, para siempre, hasta que se envíe un mensaje keyOn, keyOff, target o un nuevo value.

### 6.3.2 Haciendo un sonido de violín con SawOsc y el UGen de envolvente ADSR

Cambiando a un nuevo modelo de instrumento, si quieres hacer un sonido parecido a un violín, puedes intercambiar el oscilador de onda cuadrada por uno de diente de sierra en el ejemplo anterior. Pero ahora hagamos cosas más interesantes para hacerlo sonar aún más como un violín tocado con un arco. Los violinistas tienden a usar gestos específicos para atacar las notas, lo que se refleja a menudo en sus movimientos físicos mientras tocan. Existe un generador de envolvente más avanzado y útil en ChucK, el ADSR (attack decay sustain release, por ataque decaimiento sostenimiento liberación). La figura 6.3 muestra una función típica generada por un UGen ADSR, en este caso con attack, decay,  y release configurados en 0.1 segundos y el nivel de sustain en 0.5. Puedes definirlos todos usando los métodos/funciones .attackTime, .decayTime, .sustainLevel y .releaseTime, o puedes hacerlo todo una vez usando el método .set así:

```chuck
myEnv.set(0.1 :: second, 0.1 :: second, 0.5, 0.1 :: second);
```

Observa en la figura 6.3 que tanto decay como release duran la mitad de lo que dura attack, incluso cuando sus tiempos fueron definidos en la misma duración. Eso ocurre poque solo tienen que llegar a la mitad, de 1.0 a 0.5 de nivel de sustain para decay y de de 0.5 a 0.0 para la fase de relase.

Para hacer tu sintetizador de violín simple, puedes combinar un generador de envolvente ADSR con un SawOsc, de esta manera:

```chuck
SawOsc viol => ADSR env => dac;
```

No obstante, un sonido de violín es más que una onda de diente de sierra. Los violines son famosos por su vibrato, así que podrías querer hacerlo también. Este es un momento perfecto para hablar sobre una característica de los UGens Oscillator, y es que puedes hacer ChucKing a elllos de una señal para modelar cosas como frecuencia y fase. Estas son muy buenas noticias, porque puedes usar un SinOsc para generar vibrato en tu sintetizador de violín, parecido a lo que se muestra en la figura 6.4.

```chuck
SinOsc vibrato => SawOsc viol => ADSR env => dac;
```

Esto no funcionará aún, porque primero necesitas decirle a tu SawOsc que interpete la onda sinusoidal ocmo una modulación en frecuencia. PAra hacerlo usa el método .sync() (1), como se muestra en el listado 6.5. También tienes que definir la frecuencia de tu vibrato a algo razonable, como 6 Gz, usando el método .freq() (2). Puedes definir todos los parámetros de envolvente de una vez (3), definir una escala en un arreglo (4), y luego tocar esa escala usando un bucle for (5), definiendo la altura del violín (6) y tocando cada not usando el ADSR. Finalmente, puedes aumentar el vibrato y tocar la última nota durante un tiempo mayor (7).

Listado 6.5 Violín simple usando SawOsc, Envelope y SinOsc para vibrato

```chuck
//violín simple basado en SawOsc con envolvente ADSR y vibrato
SinOsc vibrato => SawOsc viol => ADSR env => dac;
//(1) indicar al SawOsc que interprete la entrada de vibrato como modulación en frecuencia
2 => viol.sync;
//define la frecuencia de vibrato
6.0 => vibrato.freq;
//(3) configura los parámetros de ADSR
env.set(0.1 :: second, 0.1 :: second, 0.5, 0.1 :: second);
//define una escala D mayor (en números de notas MIDI)
//(4) construye un arreglo de notas de escala
[62, 64, 66, 67, 69, 71, 73, 74] @=> int scale[];
//recorre nuestra escala una nota a la vez
//(5) toca la escala usando un bloque for
for (0 => int i; i < scale.cap(); i++)
{
  //(6) define la frecuencia de cada nota a partir del número de nota
  Std.mtof(scale[i]) => viol.freq;
  //gatilla la nota y espera un poco
  1 => env.keyOn;
  0.3 :: second => now;
  //apaga la nota y espera un poco
  1 => env.keyOff;
  0.1 :: second => now;
}

//repite la última nota con mucho vibrato
1 => env.keyOn;
//usa más vibrato para la última nota
10.0 => vibrato.gain;
1.0 :: second => now;
0 => env.keyOff;
0.2 :: second => now;
```

Existen otros tipos de osciladores, incluyendo una familia completa de generadores de tabla GenX, como exponenciales, polinomiales y de segmentos de línea y UGens que automáticamente suman ondas sinusoidales armónicas. Todas estas funcionan como tablas lookup (entra un valor, sale un valor), peor también pueden ser usadas como osciladores si se recorren con Phasor, un UGen especial. Todos ellos, y más, son cubiertos en mayor profundidad en el apéndice C.

## 6.4 Síntesis de frecuencia modulada

Si defines la frecuencia del oscilador de vibrato de la figura 6.4 a un valor mucho más alto en el rango audible, escucharás algo bastante extraño. Esto es exactamente lo que le ocurrió al compositor John Chowning en el Stanford Center for Computer Research in Music and Acoustics (CCRMA) cuando escribió 100 en vez de 10 para la frecuencia de un oscilador de vibrato. Estaba usando osciladores de onda sinusoidal en ambos, pero lo que escuchó sonaba mucho más interesante que estas ondas. Hizo consultas y descubrió que lo que estaba escuchando era un espectro complejo creado por modulación de frecuencia (FM, por frequency modulation).

Si cambias el oscilador viol SawOsc del listado 6.5 por un SinOsc, defines vibrato.freq en 100.0 y defines vibrato.gain en un número mayor, como 1000.0, escucharás algo que se parece mucho a lo que Chowning escuchó en 1967. Lo que estás escuchando es un montón de frecuencias sinusoidales que son creadas cuanod la onda (llamada modulante) modula la frecuencia de la otra (llamada portadora). Una manera de ver esto es que cambiando rápidamente la frecuencia de la portadora, la forma de la onda sinusoidal es distorsionada de diferentes formas. De hecho, FM es parte de una clase de algoritmos de síntesis llamados de formación de ondas (wave shaping).

Ahora cambia la línea dentro del bucle ((6) en el listado 6.5 y repetido aquí) para que los osciladores viol y vibrato sean definidos con la misma frecuencia, y modifica vibrato.gain para obtener más modulación:

```chuck
//define ambas frecuencias según el número de nota
//(6)
Std.mtof(scale[i]) => viol.freq => vibrato.freq;
100.0 => vibrato.gain;
```

Notarás que esto suena como un instrumento de viento metálico tocando una escala. Ahora cambia la línea (6) por estas dos líneas:

```chuck
Math.random2f(300.0, 1000.0) => viol.freq;
Math.random2f(300.0, 1000.0) => vibrato.freq;
```

Ejecútalo un par de veces. Observarás que cada nota tiene un carácer distinto, generalmente tonos inarmónicos tipo campana (pero sin las características de decaimiento que tienen las campanas).

Chowning resolvió que si las frecuencias de portadora (C, por carrier) y modulante (M) no están relacionadas en razones simples de enteros (razón C:M de 1:2, 2:1, 2:3, 1:3, ...) entonces el espectro resultante sería inarmónico. Continuó sintetizando campanas, instrumentos de viento de metal, voices y otros sonidos interesantes, ¡todos usando solo ondas sinusoidales! La añadidura y combinación de más modulantes y portadoras puede arrojar sonidos aún más interesantes.

ChucK tiene un número de UGens FM y presets incluidos para modelar el espectro de varios instrumentos, incluyendo pianos eléctricos (Rhodey y Wurley). El siguiente listado es un programa simple pra probar el piano eléctrico Wurley.

```chuck
//programa de prueba de instrumento de unidad generadora FM
//por persona FM, Marzo 4, 1976

//haz in instrumento FM y conéctalo al dac
//(1) piano eléctrico FM
Wurley instr => dac;

//tócalo para siempre con frecuencia y duración aleatoria
while (true)
{
  Math.random2f(100.0, 300.0) => instr.freq;

  //enciende la nota (gatilla ADSR interno)
  //(2) enciende la nota, espera un momento aleatorio
  1 => instr.noteOn;
  Math.random2f(0.2, 0.5) :: second => now;

  //apaga la nota (rampa descendente del ADSR interno)
  //(3) apaga la nota, espera un momento aleatorio
  1 => instr.noteOff;
  Math.random2f(0.05, 0.1) :: second => now;
}
```

> Ejercicio

> Otros instrumentos STK FM incluyen órgano (BeeThree), FMVoices, campanas de orquesta (TubeBell), flauta (PercFlut), y guitarra distorsionada (HevyMetl). Intercambia el UGen Wurley en (1) por algúno de los otros (TubeBell, PercFlut, Rhodey, entre otros). Como estos UGens son instrumentos completamente autocontenidos, responden a mensajes noteOn (2) y noteOff (3), que a su vez encienden y apagan el generador de envolvente ADSR que controla los niveles internos de portadora y modulante.

## 6.5 Síntesis de cuerda pulsada por modelamiento físico

Probablemente es tiempo de observar que el clarinete y el violín que has construido hasta ahora no tienen un sonido muy realístico, lo que podría estar bien para muchos propósitos, pero puedes tener mejores resultados tomando en cuenta la física involucrada en instrumentos como el clarinete, el violín y el trombón. La síntesis de modelamiento físico (PM, por physical modeling) resuelve las ecuaciones de las ondas dentro y alrededor de objetos sonoros para generar los sonidos de forma automática. Esto difiere mucho de lo que has hecho hasta ahora, donde has sintetizado una forma de onda o ruido de algún tipo. En PM, enfatizas la física del instrumento, con fe en que la forma de onda resultante será acertada.

En esta sección, estarás construyendo un modelo de cuerda cada vez más realístico, empezando con el modelo de absoluta simpleza (una línea de retraso (delay line) excitada por un impulso, para modelar el sonido de la uñeta viajando hacia abajo y de vuelta a lo largo de la cuerda), luego añadiendo una mejor excitación (ruido), luego mejorando la línea de retraso para permitir una mejor afinación, y finalmente añadiendo aún más control sobre la excitación por uñeta. Empezamos con uno de los primeros modelos computacionales históricos de cuerda.

### 6.5.1 La cuerda pulsada más simple

Uno de los modelos físicos más tempranos y básicos para síntesis de sonido es de cuerda pulsada. La versión más simple de esto involucra una línea de retraso (delay line, un UGen que retrasa la señal entre la entrada y la salida, con lo que cualquier cosa en la entrada sale sin modificaciones, pero después en el tiempo), retroalimentado a sí mismo, y excitado con un impulso como entrada. Por supuesto, ChucK tiene todos estos elementos incluidos como UGens. Este código muestra el UGen Impulse alimentado a una línea de retraso y luego al dac; luego la línea de retraso es retroalimentada a sí misma para formar un bucle:

```chuck
//Impulse es alimentado a una línea de retraso
Impulse imp => Delay str => dac;
//haz un bucle con la línea de retraso
str => str;
```

El Ugen Impulse genera una salida de solo un sample cada vez que defines su método .next a cualquier valor distinto de cero. Esto es, la línea 1.0 => imp.next; (1) en el listado 6.7 causa que imp emita un 1.0 en el siguiente sample y luego 0.0 para siempre y hasta que uses de nuevo el método .next.

¿Por qué un impulso alimentado a una línea de retraso y luego retroalimentado a sí mismo es un modelo físico? Porque esta cadena de sonido es una simulación física válida de una cuerda pulsada, donde las ondas viajeras se mueven arriba y abajo a lo largo de la cuerda, con pequeñas pérdidas en cada viaje.

> Historia de la síntesis: modelamiento físico

> El algoritmo de Cuerda Pulsada (llamado también el algoritmo Karplus-Strong) fue descubierto en 1982 por los científicos de la computación de Stanford Kevin Karplus y Alan Strong. Ese mismo año, Julius Smith y David Jaffe (un ingeniero eléctrico y un compositor, trabajando en conjunto en Stanford CCRMA) explicaron el modelo científicamente y lo mejoraron. Smith nombró esta estructura como un "filtro de guía de onda", porque la cuerda (retraso) guía a la onda (impulso) adelante y atrás a lo largo de la cuerda. La síntesis de modelamiento  físco es a veces llamada síntesis de guía de onda.

Si defines la ganancia de viaje ida y vuelta por la cuerda a algo menor a 1.0 (para representar las pequeñas pérdidas) y defines el largo de la cuerda (tiempo de retraso) a un valor razonable para el tiempo de viaje ida y vuelta (el periodo de oscilación de la cuerda), entonces has construido un modelo de cuerda primitivo, como se muestra en el siguiente listado.

Listado 6.7 Modelo físico simple de cuerda pulsada
```chuck
//modelo super simple de cuerda pulsada Karplus-Strong
Impulse imp => Delay str => dac;
//conecta str a sí mismo
str => str
//retraso de cuerda ida y vuelta, 100 Hz a tasa de sampleo 44.1k
441.0 :: samp => str.delay;
//definir como menor que 1.0 la ganancia de viaje ida y vuelta
0.98 => str.gain;
//"toca" la cuerda
//(1) le indica al impulso que emita un 1.0 (solo durante el siguiente sample)
1.0 => imp.next;
//deja que la cuerda "resuene"
5.0 :: second => now;
```

Esto hace un sonido que es vagamente parecido a una cuerda, en el sentido de que empieza súbitamente, decae de forma más lenta y tiene una altura. Pero puedes hacerlo mejor, excitando la cuerda con algo más interesante que un impulso.

### 6.5.2 Excitando con ruido la cuerda pulsada



HEREIAM
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

### 6.5.3 Modelamiento de decaimiento dependiente de frecuencia con un filtro

### 6.5.4 Modelamiento de delay fraccional (afinación) y agregando un ADSR para la pulsación

## 6.6 Introducción a Ugens de filtro: ganancia dependiente de frecuencia

## 6.7 Más sobre delays: acústica de espacios y reverberación

## 6.8 Efectos de audio basados en delay

## 6.9 Ejemplo: diversión con los UGens Filter y Delay

## 6.10 Resumen
