# Fundamentos: sonido, ondas y programación en ChucK

Este capítulo cubre:
* Introducción a la acústica, sonido y formas de onda
* Ondas sinusoidales y otros osciladores
* Variables
* Estructuras de control y lógica

Hasta el momento hemos mostrado por qué creemos que ChucK es el mejor lenguaje y el más poderoso para hacer todo tipo de arte y sistemas artísticos. Ahora es el momento de empezar a aprender a programar en ChucK para hacer sonido y música. Primero hablaremos de sonido en general, observando una o dos formas de onda de sonido. Discutiremos las propiedades de los sonidos en términos de volumen, altura y ruido. Aprenderás que objetos llamados osciladores son fundamentales para la física, el sonido y la música, y harás música con los osciladores incluidos en ChucK. Aprenderás:

* Cómo los datos son manejados y manipulados por ChucK mediante el uso de variables.
* Sobre los mecanismos de temporización incluidos en ChucK. La forma en que maneja el tiempo es uno de los aspectos que lo hace diferente de los otros lenguajes de programación, y que lo hace tan bueno para programar música, sonido y arte basado en el tiempo.
* Sobre el control del flujo de tus programas mediante el uso de variables lógicas y pruebas y bucles.

¡Para el final del capítulo habrás escrito tu primera composición con ChucK!

## 1.1 Ondas de sonido y formas de onda

Empezaremos discutiendo sobre la física y la naturaleza del sonido. El sonido consiste de fluctuaciones de alta y baja presión (ondas) de aire que son causadas por uno o más objetos vibrando. Las ondas de sonido luego se propagan a través del aire, quizás rebotando contra las paredes y otras superficies, para finalmente alcanzar tus oídos o un micrófono. La gente que trabaja con sonido generalmente grafica las formas de onda (amplitud de la presión de aire, o voltaje de un micrófono, como una función del tiempo). La figura 1.1 muestra un gráfico de valores de onda como una función del tiempo de un hombre diciendo la palabra "see".

Algunos aspectos son obvios en este gráfico. La consonante ruidosa "sss" en la primera mitad del sonido cambia rápidamente a una distinta estructura para la vocal "eee" en la segunda mitad. Si haces zoom a la zona de transición, más aspectos se hacen obvios, como muestra la figura 1.2.

La "sss" se ve prácticamente igual, todavía ruidosa con muchas ondulaciones en la forma de onda, pero el sonido "eee" rápidamente entra a lo que se llama oscilación periódica, donde una forma básica se repite una y otra vez. Podrán haber pequeñas desviaciones debido a ruido, temblores en la voz en sí misma, entre otros factores, pero la oscilación generalmente se repite una y otra vez. Esta es una característica de los sonidos que tienen altura. La altura es tu percepción de la frecuencia, de grave a alta. Es lo que cantas o silbas de una melodía de una canción, y es lo que los músicos anotan y discuten en términos de nombres de notas y números. Las teclas de un piano son orientadas por altura de izquierda (grave) a derecha (alto).

Casi todo lo que te da una sensación de altura posee una oscilación periódica, y cualquier objeto que oscile periódicamente producirá un sonido de una cierta altura (dentro de un rango de frecuencias). La región marcada como T en la figura 1.2 es el periodo de la oscilación (usualmente medido en segundos), y 1/T es la frecuencia de la oscilación en ciclos por segundo. Así que en el caso del sonido "eee", si el periodo T mide 6.6 milisegundos (un milisegundo es una milésima de un segundo), entonces la frecuencia de oscilación será 1/0.0066 = 150 ciclos por segundo, o 150 Hz (la unidad de frecuencia nombrada en honor al físico Heinrich Hertz).

Si repites ese único periodo del sonido "eee" una y otra vez, repitiéndolo en un punto conveniente como en alguna cima principal, podrás sintetizar un sonido "eee" de la duración que quieras. Si reproduces ese sonido un poco más rápido, la altura subirá. Tócalo a una velocidad menor y obtendrás una altura menor. El sonido sintético será aburrido, porque al seleccionar un solo periodo y repetirlo una y otra vez de forma exacta, habrás eliminado todo el ruido, pequeñas desviaciones en altura, y otros aspectos que hacen que el original suene natural. Esta es una razón que explica por qué los sonidos de voz sintetizada suenan, ah, sintéticos.

Mucha de la historia de la música electrónica, y la historia de la música en general, y realmente de gran parte de la física, se centra en la noción de oscilación. Usaremos muchos sabores de osciladores incluidos en ChucK en las siguientes secciones, pero por ahora observaremos una de las formas fundamentales de oscilación en la naturaleza y el sonido: la onda sinusoidal. Pronto usarás el oscilador de onda sinusoidal de ChucK para escribir tu primer programa que emite una nota musical. ¿Entonces, qué es una onda siusoidal? Una onda sinusoidal cae y decae de forma suave y periódica, como se muesrta (por un par de periodos) en la figura 1.3.

El círculo de la izquierda muestra una forma en que se pueden graficar, explicar y generar las ondas sinusoidales. Si rotas el círculo en dirección contraria a las agujas del reloj a una tasa constante, digamos N rotaciones por segundo, entonces la altura del punto que empieza en el lado izquierdo del círculo (el lado izquierdo del gráfico) traaz una onda sinusoidal mostrada hacia la derecha, según el tiempo avanza de izquierda a derecha. Una manera de entender esto es imaginarse el punto en el borde del círculo como un lápiz que si tomas un pedazo de papel de izquierda a derecha mientras el círculo avanza, dibujará la onda sinusoidal. Si N fueran cinco ciclos por segundo, entonces el círculo y la onda sinusoidal completarán cinco ciclos en un segundo.

Puedes haber escuchado sobre funciones sinusoidales si has estudiado trigonometría, pero como hicimos notar antes, las ondas sinusoidales pueden ser encontradas en muchos lugares de la naturaleza y de las matemáticas. La electricidad en las tomas de corriente alterna tienen un patrón sinusoidal, porque los generadores en las plantas de generación eléctrica rotan en círculos. Pero no solo rotando objetos se pueden generar ondas sinusoidales. Un péndulo describe una onda sinusoidal de desplazamiento mientras oscila hacia adelante y atrás. Un circuito eléctrico que contiene un inductor y un capacitor oscila de forma sinusoidal. Acústicamente, la resonancia única del aire dentro de una botella oscila de forma sinusoidal. Así que como puedes ver, las ondas sinusoidales están presentes en todas partes y son una característica importante del mundo real.

Otras formas de onda simples que son comunes en la naturaleza y en la electrónica incluyen las ondas triangulares, diente de sierra y cuadradas. La figura 1.4 muestra dos ciclos cada una de onda sinusoidal, triangular, diente de sierra y cuadrada.

Las ondas sinusoidales están en el corazón del análisis de sonido y también de la síntesis. Ondas periódicas más complejas pueden ser construidas a partir de ondas sinusoidales. Puedes construir cualquier sonido posible sumanod ondas sinusoidales de distintas frecuencias, fase (retraso) y amplitudes (volumen). Las ondas sinusoidales son los ladrillos fundamentales de construcción de lo que llamamos visión espectral del sonido, donde en vez de revisar los pliegues individuales de la onda, revisas los componentes sinusoidales que conforman esta forma de onda.La figura 1.5 muestra que una onda diente de sierra puede ser construida sumando ondas sinusoidales en múltiplos enteros (1, 2, 3, ...) de alguna frecuencia fundamental. Estos componentes son llamadas armónicos. Las ondas triangular y cuadrada pueden ser construidas con frecuencias armónicas impares (1, 3, 5 )de ondas sinusoidales a distintas amplitudes.

La onda diente de sierra también tiene una analogía física en el mundo de los instrumentos musicales, porque cuando una cuerda arqueada oscila, el arco alternadamente se apega a la cuerda, arrastrándola consigo, y luego se desliza, dejando que la cuerda rápidamente vuelva a su posición original. Esto crea un movimiento tipo diente de sierra, tal como lo observó hace mucho Herman von Helmholtz (1821 - 1894). Helmholtz nos brindó algunas de las teorías acústicas y experimentos más tempranos de la descripción espectral de instrumentos y sonidos. Una onda diente de sierra puede ser un buen punto de partida en la síntesis de un violín, y lo realizaremos pronto.

Físicamente, un clarinete tocando su nota más grave oscila muy parecidamente a una onda cuadrada, así que puedes construir una síntesis muy simple que suene como un clarinete usando una onda cuadrada y un generador de envolvente (más información dentro de poco). ¡Pero primero, programemos!

## 1.2 Tus primeros programas de ChucK

Es tiempo de empezar a aprender a programar. En este punto, deberías haber leído las intrucciones del apéndice A sobre cómo instalar miniAudicle y la funcionalidad básica del entorno de programación. Una vez que ha sido instalado, abre miniAudicle y te guiaremos para escribir tus primeros programas. La figura 1.6 muestra al miniAudicle listo para ejecutar código en ChucK.

Existen tres ventanas principales en el miniAudicle. La ventana prinicpal que ves en la parte superior de la figura 1.6 se llama inicialmente Untitled (sin título), y es la ventana en la que escribes tus programas. Puedes cambiar el nombre de la ventana, lo que haremos dentro de poco cuando escribamos y grabemos nuestro primer programa. La ventana de la esquina inferior derecha es el monitor de la máquina virtual (VM, por Virtual Machine), que muestra el estado del servidor de la VM mientras tu código en ChucK es ejecutado. Una vez iniciada, la VM está siempre ahí corriendo, esperando cosas que hacer.

Cuando haces click en Start Virtual Machine (iniciar la máquina virtual), crea una ventana llamada Console Monitor (monitor de consola), mostrada en la esquina inferior izquierda de la figura 1.6. Aquí es donde recibirás mensajes del computador para ayudarte con tu programa. Si hay algún error en el código, aquí es donde el computador te dirá que existen problemas y específicamente dónde (en cuáles líneas) ocurren. También puedes imprimir mensajes en el Monitor Console a ti mismo desde tu código en ejecución para ayudarte a corregir errores en tu programa o para mantenerte informado sobre lo que está pasando.

### 1.2.1 Tu primer programa: "Hello World"

En casi todos los lenguajes, el primer programa que escribes es "Hello World" (Hola Mundo), así que empezaremos mostrándote cómo lograr esto en ChucK. Dirígite a la ventana llamada Untitled, grábala con el nombre helloworld.ck usando el menú File (Archivo), y luego escribe esta línea:

```
<<< "Hello World" >>>;
```

No te olvides de incluir el punto y coma, que es importante para señalar el término de cada línea de código en ChucK.


Luego, asegúrate de que tu VM esté corriendo (verás el tiempo transcurrido: el valor aumenta constantemente), y luego haz click en Add Shred (Añadir Shred) en la ventana principal, que ahora se llama helloworld.ck. De forma instantánea verás que "Hello World" aparece en el Console Monitor. ¡Felicidades, has ejecutado correctamente tu primer programa en ChucK! Haciendo click en el botón Add Shred le dice ChucK que ejecute el código "Hello World" que escribiste en la ventana principal. Harás click en este botón cada vez que quieras ejecutar un programa en ChucK.

Aunque siempre estarás escribiendo y ejecutando código desde la ventana principal, desde ahora solo mostraremos el código como texto, con etiquetas y comentarios. No mostraremos la ventana principal completa con los botones de control, pero mostraremos partes de la figura 1.6 en el futuro para señalar algunos de los controles. Todas las ventanas pueden ser movidas en tu escritorio según prefieras, tal como muchos de los programas con los que estás acostumbrados a trabajar con en tu computador.

Tal como fue discutido anteriormente, es útil para ti como programador imprimir al Console Monitor para efectos de detectar y corregir errores y como compositor para saber en qué parte de la canción va la ejecución. Desde este primer ejemplo sabes que poner ítemes dentro de <<< >>> te permite imprimirlos. Poneros entre comillas dentro de los triples <<< >>> los imprime directamente. Te daremos técnicas más avanzadas pra imprimir otras cosas a lo largo de este capítulo.

### 1.2.2 Tu primer programa sonoro: "Hello Sine!"

HEREIAM

### 1.2.3 Ahora hagamos música

### 1.2.4 Probando nuevas formas de onda

## 1.3 Tipos de datos y variables

## 1.4 Tiempo en ChucK: se trata de now

## 1.5 Lógica y estructuras de control para tus composiciones

## 1.6 Uso de múltiples osciladores en tu música

## 1.7 Un ejemplo final: "Twinkle" con osciladores, variables, lógica y estructuras de control

## 1.8 Resumen

page 17
page 18
page 19
page 20
page 21
page 22
page 23
page 24
page 25
page 26
page 27
page 28
page 29
page 30
page 31
page 32
page 33
page 34
page 35
page 36
page 37
page 38
page 39
page 40
page 41
page 42
page 43
page 44
page 45
page 46
