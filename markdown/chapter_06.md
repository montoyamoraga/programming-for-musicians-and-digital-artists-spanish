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

Los inventores originales del modelo de cuerda pulsada realmente excitaron (pulsaron) su cuerda con ruido en vez de un impulso. Esto corresponde a un ataque realmente energético, pero tiene un sonido brillante y atractivo. Afortunadamente, ChucK posee un UGen Noise (ruido), pero que emite constantemente ruido, a diferencia de Impulse, que emite un pulso cada vez que se define el valor .next. Por esto, necesitas un interruptor para controlar la salida del UGen Noise para excitar la línea de retraso cuando pulsas la cuerda, luego debes abrir el interruptor después de un periodo de tiempo muy corto. Para hacer esto, debes encender el ruido (definir su ganancia como 1.0) por un número de samples igual al largo de la línea de retraso, luego apagarlo (definir la ganancia del ruido como 0.0), tña como se muestra en el siguiente listado.

Listado 6.8 Modelo físico mejorado de cuerda pulsada, excitada con ruido

```chuck
//modelo mejorado de cuerda pulsada Karplus-Strong
Noise pluck => Delay str => dac;
//conectar la cuerda a sí misma
str => str;
//recorrido ida y vuelta del retraso, 100 Hz a tasa de 44.1k
441.0 :: samp => strl.delay;
//definir la ganancia de ida y vuelta menor a 1.0
0.98 => gain;
//tocar la cuerda durante el periodo de tiempo correcto
1.0 => pluck.gain;
441.0 :: samp => now;
//apagar el generador de ruido
0.0 => pluck.gain;
//dejar que la cuerda suene un rato
5.0 :: second => now;
```

Tienes que hacer una cosa adicional para copiar lo que Karplus y Strong estaban haciendo, lo que también hará que tu modelo simple de cuerda pulsada suene aún mejor. Esto es añadir un filtro en el bucle de la cuerda (línea de retraso) para modelar el hecho de que las pérdidas experimentadas por las ondas viajando en ambas direcciones por las cuerdas son dependientes de la frecuencia, donde por cada viaje a lo largo de la cuerda, las frecuencias más altas experimentan mayores pérdidas que las frecuencias graves.

### 6.5.3 Modelamiento de decaimiento dependiente de frecuencia con un filtro

Para modelar el decaimiento dependiente de frecuencia, solo necesitas modificar la líea donde conectas la cuerda a sí misma por medio de la adición de un filtro pasabajos, que reduce la ganancia de las frecuencias altas en mayor magnitud que la de las frecuencias bajas.

```chuck
str => OneZero filter => str;
```

Con la adición de este filtro, tu cuerda sonará instantáneamente mejor más realísticamente. OneZero es un UGen tipo filtro, muy simple y sobre el que hablaremos más muy pronto.

### 6.5.4 Modelamiento de retraso fraccional (afinación) y adición de un ADSR para la pulsación de la cuerda

También puedes añadir una cosa: la línea Delay necesita soportar samples fraccionales de retraso, para que puedas afinar tu cuerda a frecuencias arbitrarias. Recuerda, en el capítulo 1 aprendiste que necesitas números de punto flotante para expresar algunas alturas porque los números enteros no son suficientes. PAra el modelo de cuerda, el retraso fraccional es especialmente importante para las frecuencias altas, porque la línea de retraso se vuelve muy corta y la diferencia entre 44 samples y 45 samples a 44.1 kHz de tasa de muestreo es de 980 Hz versus 1002.27 Hz. Si necesitas exactamente 995 Hz, entonces necesitarás tener una lína de retraso de 44.3216 samples. Afortunadamente, ChucK posee retrasos interpolantes, llamados DelayL (por interpolación linear entre samples) y delayA (por allpass, pasatodo, un tipo extraño de filtro que puede producir samples fraccionales de retraso). Entonces todo lo que tienes que hacer es reemplazar Delay por DelayL o DelayA para permitir retraso fraccional y con eso afinación arbitraria. Estos UGens de retraso aceptan un número de punto flotante, con lo que puedes definirlos de cualquier longitud, como este:

```chuck
44.3216 :: samp => str.delay;
```

También puedes usar un ADSR para permitir que tu ruido pulse la cuerda, lo que significa que no tienes que explícitamente prenderlo y apagarlo. Una vez que has configurado los parámetros attack, decay, sustain y release, puedes usar el método de ADSR .keyOn para lograr la pulsación de la cuerda. Todo esto se muestra en la figura 6.5 y en el siguiente listado.

Listaod 6.9 Modelo aún mejor de cuerda pulsada, con ruido con envolente y filtro pasabajo

```chuck
//Noise a través de ADSR y a través de una línea de retraso con interpolación
Noise nois => ADSR pluck => DelayA str => dac;

//conectar la cuerda a sí misma
//retroalimentación a través de un filtro pasabajos
str => OneZero lowPass => str;

//definir los parámetros del ADSR del Noise
//parámetros para pulsación corta y luego mantención en cero
pluck.set(0.002 :: second, 0.002 :: second, 0.0, 0.01 :: second);

//tocar notas aleatorias para siempre
while(true)
{
  //ahora puedes definir el retraso a un número de punto flotante arbitrario
  Math.random2f(110.0, 440.0) :: samp => str.delay;
  //pulsa enviando keyOn al ADSR, permite el paso de Noise a la cuerda
  1 => pluck.keyOn;
  0.3 :: second => now;
}
```

## 6.6 Introducción a UGens de filtro: ganancia dependiente de frecuencia

¿Qué fue ese UGen OneZero mágico que acabas de usar para proveer de ganancia dependiente en frecuencia a tu cuerda en bucle? El UGen OneZero usa matemáticas simples que suman su entrada de sample actual al sample de entrada más reciente y divide por 2, calculando así un promedio entre ambos samples. Otra manera de llamar a este filtro es de Promedio en Movimiento (Moving Average).

```chuck
(thisInput + lastInput) / 2 => output;
```

thisInput, por esta entrada y lastInput, por la entrada más reciente, output por salida.

Promediar tiende a suavizar las señales en bruto, reduciendo frecuencias altas y enfatizando y suavizando frecuencias bajas. Esto es precisamente lo que ocurre en instrumento de cuerda real a medida que las ondas viajan a lo largo de la cuerda, ida y vuelta. La respuesta en frecuencia del UGen OneZero illustrada en la figura 6.6, muestra una ganancia de 1.0 para la frecuencia más baja (0.0 Hz),una menor ganancia para frecuencias mayores, y una ganancia de 0 para la frecuencia correspondiente a la mitad de la tasa de sampleo. Esta ganancia cero en una frecuencia es el "one zero" (un cero) del nombre del filtro.

Otro tipo de filtro UGen que nos gusta usar es ResonZ, mostrado en el listado 6.10, que crea una resonancia (ganancia mayor en una frecuencia seleccionable) en cualquier señal que pasa a través de él. ResonZ responde a .freq, que define la frecuencia de resonancia y .Q que representa el factor de calidad (quality factor) y determina la cantidad de énfasis en la frecuencia resonante. Si defines .Q con un valor muy alto, ResonZ puede oscilar como una frecuencia sinusoidal; si defines Q a 100 o parecido (2) y alimentas el filtro con una excitación Impulse (1), obtienes sonido ping o pop con altura correspondiente a la frecuencia de resonancia cada vez que gatillas el impulso (3).

Listado 6.10 Un impulso simple a través de un filtro resonante genera música por computador interesante

```chuck
//¡¡música por computador!! Impulso a través de un filtro resonante
//(1) el impulso excita al filtro resonante
Impulse imp => ResonZ filt => dac;

//define la calidad (Q) a un número alto, para que genere una altura
//(2) Q (calidad) es la cantidad de resonancia
100.0 => filt.Q;

while(1) {
  //elige una frecuencia aleatoria
  Math.random2f(500.0, 2500.0) => filt.freq;

  //gatilla el impulso y espera un poco
  //(3) haz que el impulso genere 100.0 de salida (solo durante el siguiente sample)
  100.0 => imp.next;
  0.1 :: second => now;
}
```

Para moldear sonidos y efectos especiales, existen filtros UGens de pasaaltos (HPF, por high-pass filter), pasabajos (LPF, por low-pass filter), pasabanda (BPF, por band-pass filter) y rechazabanda (BRF, por band-reject filter). Todos ellos son controlados usando los métodos .freq y .Q. El rango de frecuencia que puede pasar a través de estos filtros es llamado la pasabanda, y las frecuencias que reciben una ganancia disminuida son llamadadas la rechazabanda. La frontera entre la pasabanda y la rechazabanda es llamada la frecuencia de corte, que es definida por el método .freq. Una vez más, Q es de "calidad" (por quality) y determine el tamaño del énfasis en la frecuencia de corte y el rolloff (la pendiente de la ganancia hacia la rechazabanda).

La figura 6.7 muestra la respuesta en frecuencia (ganancia versus frecuencia) de una unidad generadora LPF con un Q igual a 1, 10 y 100. Las regiones de pasabanda, rechazabanda y rollof están etiquetadas.

Fig 6.7 Respuesta en frecuencia de un filtro pasabajos LPF para Q=1, Q=10 y Q=100

El siguiente listado muestra el uso de un UGen LPF (filtro pasabajos resonante, pasa todas las frecuencias bajo la frecuencia de corte definida por el método .freq) para filtrar ruido.

Listado 6.11 Prueba de un filtro pasabajos resonante LPF con ruido como entrada

```chuck
//pasar ruido a través de un filtro pasabajos
Noise nz => LPF lp => dac;

//definir frecuencia y Q
500.0 => lp.freq;
100.0 => lp.Q;
0.2 => lp.gain;

second => now;
```

> Ejercicio

> Cambia LPF por HPF en el listado 6.11 En el caso del LPF deberías escuchar frecuencias bajas hasta la frecuencia de resonancia. En el caso del HPF deberías escuchar frecuencias altas desde la frecuencia de resonancia. Prueba con BPF y BRF. ¿Qué escuchas? Cambia los valores de .freq y .Q y pon atención en cómo el sonido cambia.

## 6.7 Más sobre retrasos: acústica de espacios y reverberación

Cuando se produce un sonido, las ondas se propagan (viajan) desde la fuente del sonido. Los sonidos hechos en habitaciones viajan y rebotan contra las paredes, piso, techo y objetos en la habitación (ver figura 6.8). Podrías saber que un solo reflejo de un sonido contra un límite, como una pared o edificio, es llamado un eco si el tiempo de retraso es lo suficientemente largo como para que escuches el sonido reflejado por sí mismo (mayor a 50ms aproximadamente).

En una habitación de un tamaño razonable, los reflejos son más cortos en tiempo que los ecos y se suman en los oídos del sujeto que escucha (o un micrófono), para crear reverberación. Como las ondas de sonido se demoran un tiempo en viajar, estos viajes alrededor de la habitación, rebotando contra las paredes y otros obstáculos, toman diferentes tiempos en eventualmente llegar a tus oídos (donde todas los reflejos son sumados). ¿Te acuerdas cuando estábamos hablando de la velocidad del sonid, longitudes de onda, y todo eso? Ahora que ya conoces los UGens Delay, puedes poner todo esto en práctica para hacer un modelo simple de la acústica de una habitación. Serás capaz de alimentar una señal (como un micrófono entrando por el adc) a través de la acústica de esta habitación y resultará en el sonido de estar ahí.

Asumamos que quieres modelar una pieza que mide 40 por 50 pies, con un techo de 30 pies de alto (figura 6.8). Yo sé que es un techo alto, pero el truco de las dimensiones 3x4x5 es muy conocido por los diseñadores de parlantes, salas de conciertos y otros artefactos acústicos. Si asumes que las paredes de tu pieza son paralelas y muy reflectivas, entonces el camino entre cada par de paredes paralelas se parece mucho a la cuerda que desarrollamos en la sección 6.5.1 donde las ondas viajan ida y vuelta, siendo reflejadas y absorbidas un poco en cada viaje. Como tienes tres caminos primarios de reflejo sonoro, entre los dos pares de paredes y del techo al suelo, puedes usar tres líneas de retraso para modelar la acústica en modo grueso de la habitación con forma de caja. El tiempo total de un viaje (ida y vuelta) entre el par de paredes espaciadas por 50 pies es de 100 ms (de forma aproximada, es 1 ms por pie, y como es un viaje ida y vuelta, 2 x 50 pies), el retraso del otro par de paredes es de 80 ms, y el retraso techo/suelo es de 60 ms.

El código en el listado 6.2 crea un reverb (el procesamiento de señal que modela la reverberación, es usualmente llamado un reverberador, o reverb) conectando el adc a través de tres líneas de retraso, en paralelo (2), al dac(1). Luego conectas cada línea de retraso a sí misma (3), y defines sus ganancias a algo razonable para retrasos y dimensiones de habitación típicas. Luego debes definir el tiempo de retraso para cada retraso. Las duraciones/tiempos de retraso que usan mucha memoria (más de unas pocas docenas de milisegundos) requieren que le digas al UGen Delay cuánto es lo que esperas que duren, para que pueda alocar memoria. Esto es hecho usando el método .max. Puedes definir .max y .delay en la misma línea (5). Como estás conectando el adc con el dac a través de delays, sé muy cuidadoso con la retroalimentación (usa audífonos).

Listado 6.12 Reverb simple usando tres UGens Delay

```chuck
//sonido directo
//(1) señal directa desde adc hacia el dac (a través de Gain)
adc => Gain input => dac;
1.0 => input.gain;

//líneas de retraso para modelar paredes + techo
//(2) adc a dac a través de tres líneas de retraso en paralelo
input => Delay d1 => dac;
input => Delay d2 => dac;
input => Delay d3 => dac;

//conecta las líneas de retraso a sí mismas
//(3) cierra cada bucle de retraso (conecta la salida a la entrada)
d1 => d1;
d2 => d2;
d3 => d3;

//define la retroalimentación/pérdida en todas las líneas de retraso
0.6 => d1.gain => d2.gain => d3.gain;

//aloca memoria y define las duraciones de retraso
0.06 :: second => d1.max => d1.delay;
0.08 :: second => d2.max => d2.delay;
0.10 :: second => d3.max => d3.delay;

//¡disfruta la habitación que construiste!
while (1) {
  1.0 :: second => now;
}
```

Los resultados son muy mágicos, pero quizás te das cuenta de un sonido incómodo a una altura baja. Esto es porque 60, 80 y 1000 ms comparten algunos factores en común, y estos tienden a acumularse y causar resonancias. Esto es fácil de arreglar cambiando las duraciones de retraso a números primos relativos entre sí (sin factores en común), como 61, 83 y 97 ms.

> Tarea

> Cambia los números a valores distintos y observa los efectos. Cambia las ganancias de retraso de 0.6 a otro número (pero nunca mayor a 1.0, porque esto causa que el sonido se acumule infinitamente). Notarás que para números más pequeños, el reverb suena por menos tiempo, y que para números más grandes, por un tiempo mayor. Esto recibe el nombre de tiempo de reverberancia, ¡y tienes el control como programador! Este reverb todavía suena brillante y resonante, pero puedes arreglar esto poniendo fitltros en los bucles de retroalimentación tal como lo hiciste con la cuerda pulsada. Prueba poniendo un filtro pasabajos simple OneZero en el bucle entre cada retraso donde se conecta a sí mismo, de esta forma: d1 => OneZero lp1 => d1;.

Bueno, ahora pensarás que diseñar y construir reverberadores que suenan muy bien podría ser difícil. Lo es, pero una vez más tienes suerte, porque ChucK tiene incluidos UGens reverberadores: PRCRev (nombrado en honor a Perry R. Cook, quien lo escribió para ser el reverb más eficiente computacionalmente de sonido decente), JCRev (nombrado en honor a John Chowning, famoso por su trabajo con FM), y NREV (N de nuevo, fue nuevo en los años 1980s). Estos son realmente fáciles de usar, como se muestra en el siguiente listado.

Listado 6.13 Usando el UGen NRev reverberador incluido en ChucK

```chuck
//crea un nuevo reverb y conéctalo
//una vez más, ten cuidado con la retroalimentación
//usa un volumen bajo o audífonos
adc => Nrev rev => dac;

//define la mezcla entre señal original y con reverb
0.05 => rev.mix;

//escucha y disfruta el espacio
while(1) {
  1.0 :: second => now;
}
```

Te podrás dar cuenta que este reverb suena muy bien en comparación al nuestro con tres retrasos. Esto ocurre porque tiene muchas líneas de retraso y filtros, interconectados en formas que le dan propiedades consideradas deseables para espacios acústicos. Fue diseñado por gente muy inteligente que saben mucho sobre simulación de reverb, pero tú lo puedes usar sin preocuparte de su funcionamiento interno. Si realmente te interesa este tipo de cosas, podrías implementar un reverberador a tu gusto usando ChucK. Esa es la belleza de saber programar y tener un lenguaje poderoso y expresivo.

## 6.8 Efectos de audio basados en retraso

De tu experiencia hasta el momento con la cuerda pulsada, y con los modelos de ecos y reverberación usando retrasos, puedes ver que los UGens de línea de retraso son muy buenos en modelar muchos asuntos físicos interesantes. Todas estas líneas de retraso tienen un largo fijo una vez que defines el retraso inicial. No obstante, pueden pasar cosas interesantes cuando los retrasos varían en el tiempo. El efecto Doppler de cambio de altura ocurre cuando un auto, tren o avión se mueve hacia o se aleja de ti porque el tiempo de retraso entre la fuente y tú está cambiando. Entonces una línea de retraso que cambia longitud causa que la altura de lo que está pasando a través de ella cambia, hacia arriba si el retraso disminuye y hacia abajo si el retraso aumenta. Puedes usar esto para hacer un efecto de coro, que usa líneas de retraso que cambian longitud lentamente hacia arriba y abajo para crear copias retrasadas de cualquier señal de entrada, con altura levemente cambiante. La altura sube cuando la línea de retraso está acortándose y baja cuando la línea de retraso está aumentando. Todo esto ocurre cíclicamente, arriba y abajo. ChucK tiene un UGen Chorus, que puedes usar así:

```chuck
adc => Chorus chor => dac;
```

Hay parámetros de Chorus con los que puedes jugar, como .modFreq (tasa a la que la altura cambia arriba y abajo, por defecto es 0.25 Hz) y .modDepth (profundidad de modulación, por defecto es 0.5) y .mix (la misma función de los UGens de reverberación).

> Ejercicio

> Agrega un UGen Chorus a uno de los ejemplos de este capítulo. Pruébalo en la cuerda pulsada, violín, clarinete, Wurley, entre otros.


Si quisieras constantemente acortar una línea de retraso para aumentar la altura de modo constante, eventualmente se te acabaría el retraso y llegaría a 0.0. Pero si hicieras un banco de líneas de retraso y hicieras crossfade
(gradualmente aumentar el volumen de una línea mientras disminuyes el de otra) entre ellas cuando una está volviéndose muy corta, entonces podrías hacer un efecto para cambiar alturas (pitch shifter). En el siguiente listado mostramos código que demuestra cómo funciona esto.

Listado 6.14
```chuck
//conecta la entrada de micrófono al pitch shifter
adc => PitShift p => dac;
//haz que la mezcla sea solo la señal con efecto (no se escucha la señal original)
1.0 => p.mix;

//modificación de altura para siempre
while (1) {
  //elige un cambio aleatorio de +/- 1 octava
  Math.random2f(0.5, 2.0) => p.shift;
  0.2 :: second => now;
}
```

Introduciremos otro efecto muy útil llamado Dyno, para procesamiento dinámico. Dyno tiene muchas características, incluyendo:

* Limitación - Hace que la señal no supere cierto nivel de volumen.
* Compresión - Hace que los sonidos fuertes sean más suaves y que los sonidos suaves sean más fuertes, produciendo un rango dinámico menor entre lo más suave y lo más fuerte.
* Compuerta de ruido - no permite que pasen los sonidos muy suaves, como ruido ambiental o de fondo, pero se abre sobre cierto umbral y deja que los sonidos más fuerte pasen.
* Ducking - modifica el nivel de la señal, pero lo hace basado en el volumen de otra señal externa.

Aquellos de ustedes que saben un poco sobre estos efectos encontrarán que las configuraciones son familiares, y deberías revisarlas todas en la referencia de unidades generadoras de ChucK (apéndice C). Incluso si no sabes nada sobre compresión, compuertas de ruido y ducking, una cosa para la que Dyno es perfecto es para proteger tus oídos y parlantes. El compresor y el limitador por defecto son muy buenos en hacer eso, porque si el sonido es muy fuerte, Dyno lo mantiene a raya. Muchos programadores de ChucK que conocemos siempre ponen un Dyno antes del dac en casi cada programa que escriben, especialmente los experimentales que piensan que podrían salirse de control, como en el caso de la retroalimentación. Usar Dyno es, por supuesto, tan simple como:

```chuck
adc => Dyno safety => dac;
```

## 6.9 Ejemplo: diversión con los UGens Filter y Delay

Para usar todo lo que has aprendido en este capítulo, terminaremos con un ejemplo que expande nuestro ejemplo de UGen ResonZ del listado 6.10, y nuestro reverberador de tres UGens Delay del listado 6.12. En el listado 6.15, usas los mismos UGen Impulso excitados por filtro ResonZ para resultar en ruidos cortos con sensación de altura (1). Luego haces un arreglo de tres UGens Delay (2) (sí, puedes hacer arreglos de lo que sea), y conéctalos entre tu entrada y los canales izquierdo, central y derecho del dac (3). Haz el resto de las conexiones de línea de retraso, define los tiempos de retraso y ganancias en un bucle for (4). Haz los retrasos largos, del orden de un segundo o más, para crear un efecto de eco multicanal stereo. Si revisas la matemática en (5), puedes deducir que las líneas de retraso son de 0.8, 1.1 y 1.4 segundos. Luego declara un arreglo de números de notas MIDI (6) que usarás para definir las alturas del filtro ResonZ.

Después de configurar todo, entra a un bucle while infinito, que define alturas aleatorias entre las permitidas en la tabla (7) y gatilal el UGen Impulse para hacer sonido (8). Observa que existe solo un objeto que produce sonido en todo este programa (el Impulse), pero se pueden lograr resultados musicales de interesantes polifonía (multisonido) y ritmos, debido a las líneas de retraso y sus duraciones.

Listado 6.15 Diversión musical con un filtro resonante y tres línas de retraso


```chuck
//¡diversión con UGens! Por el tipo UG, octubre 14, 2020
//filtro resonante excitado por impulso alimenta
//tres líneas de retraso, retroalimentadas consigo mismas
//(1) camino directo del impulso a través del filtro resonante
Impulse imp => ResonZ rez => Gain input => dac;
100.0 => rez.Q;
100 => rez.gain;
1.0 => input.gain;

//también podemos hacer arreglos de UGens
//(2) arreglo de tres líneas de retraso
Delay del[3];

//usemos stereo
//(3) retrasos salen por los canales izquierdo, central y derecho
input => del[0] => dac.left;
input => del[1] => dac;
input => del[2] => dac.right;

//(4) configura todas las líneas de retraso
for (0 => int i; i < 3; i++) {
  del[i] => del[i];
  0.6 => del[i].gain;
  //(5) cada línea de retraso es diferente pero están relacionadas
  (0.8 + i*0.3) :: second => del[i].max => del[i].delay
}

//(6)define el arreglo de notas para la canción

[60, 64, 65, 67, 70, 72] @=> int notes[];
notes.cap() - 1 => int numNotes;

//¡que la diversión comience! (y que dure para siempre)
while(1) {
  //(7) toca una nota aleatoria (frecuencia del filtro resonante)
  Std.mtof(notes[Math.random2(0, numNotes)]) => rez.freq;
  //(8) se gatilla el impulso (emite un 1 en el siguiente sample)
  1.0 => imp.next;
  0.4 :: second => now;
}
```

## 6.10 Resumen

En este capítulo elevaste tus habilidades de síntesis y procesamiento de sonido a otro nivel, aprendiendo más sobre las unidades generadoras de ChucK. Los UGens están incluidos en ChucK para hacer más fácil la síntesis y el procesamiento de sonido. Los puntos importantes a recordar incluyen:

* Los UGens Envelope y ADSR generan valores que cambian lentamente para controlar volumen y otras cosas.
* Puedes generar sonido por síntesis de modulación en frecuencia desde cero, usando una onda sinusoidal para modular otra, o usando los UGens FM incluidos en ChucK.
* Modelos físicos, como la cuerda pulsada, pueden ser implementados usando UGens Delay y se pueden refinar usando UGens de filtro, ruido y ADSR.
* Las líneas de retraso pueden simular ecos, reverberación, efecto coro y alteración de altura (pitch shifting).
* ¡ChucK tiene muchos UGens para hacer muchos efectos!

En el siguiente capítulo, aprenderás (más) UGens de STK (Synthesis ToolKit), expandiendo tu conocimiento sobre los UGens de instrumento de ChucK, incluyendo modelos físicos y otros modelos de síntesis y de instrumento flexibles y de buen sonido.
