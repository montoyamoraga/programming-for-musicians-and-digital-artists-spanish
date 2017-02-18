# Bibliotecas: herramientas incluidas en ChucK

Este capitulo cubre:
* Funciones de bilbiotecas (métodos)
* La biblioteca standard de ChucK, conversión de unidades
* Conversión de tipo con la biblioteca standard
* La biblioteca matemática de ChucK
* Generación de números aleatorios con la biblioteca de matemáticas y función sinusoidal
* Uso de funciones de bibliotecas para hacer música más expresiva

Ahora que tienes a ChucK a tus pies y has aprendido los fundamentos de su funcionamiento, y has creado tu primer conjunto de composiciones, es hora de aprender cómo producir música y sonido de una forma aún más expresiva. También estarás aprendiendo cómo hacer que la programación sea más simple, además de que tus programas sean de más fácil lectura. En este capítulo introduciremos a las bibliotecas, que son conjuntos de herramientas incluidas en ChucK que te permitirán alcanzar estas metas.

En casi todos los lenguajes de programación existen algunos conjuntos generales de funciones útiles que son empaquetadas en el formato llamado biblioteca. Estas bibliotecas se encargan de funciones básicas que los programadores tienen que usan en algún momento. Puedes pensar en estas funciones como herramientas, y estas herramientas son almacenadas en cajas de herramientas (cada biblioteca) de acuerdo a su tipo. Las herramientas individuales tomarán el nombre de métodos o funciones, sobre lo que aprenderás más en el capítulo 4.

Revisaremos dos bibliotecas, o colecciones de funciones: la biblioteca ChucK Standard (llamada Std) y la biblioteca Chuck Math. Aprendiendo las funciones de la biblioteca Standard que convierten números y unidades, serás capaz de controlar altura y volumen mucho más simplemente. También aprenderás formas de convertir entre distintos tipos de datos, de strings a números y viceversa, por ejemplo. Con la biblioteca Math, aprenderás a generar y usar números aleatorios y cómo redondead números de punto flotante a enteros. También aprenderás algunos procedimientos matemáticos útiles, como calcular explícitamente valores de una función sinusoidal usando la función incluida sin() de la biblioteca Math, para controlar parámetros de síntesisde maneras musicales interesantes. Específicamente, en la sección 2.3 usarás la función sin() para suavizar el paneo de una señal hacia hacia izquierda y derecha en el campo sonoro stereo.

## 2.1 La biblioteca Standard: herramientas para altura, volumen y más

Para empezar, vamos a tratar de enfocarnos en los métodos de la biblioteca Standard que te permiten convertir unidades (como cambiar de grados Fahrenheit a centígrados, o los números que representan las notas en un teclado musical a las frecuencias para nuestros osciladores), y mostraremos cómo te pueden ayudar en tus composiciones.

¿Recuerdas del capítulo 1 que para tocar nuestra canción "Twinkle" tuviste que escribir muchos números para las alturas? Algunas de ellas, como 220 y 330, eran fáciles. ¿Pero cómo calculamos las otras? Otras, como 415.5305 Hz, podrían haberte llevado a preguntar "¿de dónde vino eso?". Bueno, una de las funciones más útiles de la biblioteca Standard te permite calcular la frecuencia de cualquier nota musical. Además, la biblioteca Standard tiene muchas otras funciones útiles, como una que te permite calcular lo relacionado a tu percepción de volumen, y otras funciones para convertir entre distintos tipos de datos. Armado de un par de funciones de la biblioteca Standard, serás capaz de tocar nuesra canción "Twinkle" mucho más fácilmente, y tu código será mucho más amigable y de fáci llectura. ¡Exploremos entonces algunas de las funciones de la biblioteca Standard!

La Interfaz Digital de Instrumentos Musicales (MIDI, por la sigla en inglés)

> La Interfaz Digital de Instrumentos Musicales (MIDI) fue adoptada en los años 1980s por casi todos los fabricantes de sintetizadores de teclados y electrónicos, órganos y pianos digitales y luego por los fabricantes de copmputadores, entre otros. MIDI le permitió a los instrumentos musicales electrónicos, controladores, computadores, programas de computador de notación musical y más, comunicar entre ellos cosas musicales, usando el mismo conjunto de códigos y números preestablecidos. Por ejemplo, un computador puede ordenarle a un sintetizador que toque una nota mediante el envío del mensaje NoteOn o alterar al altura enviando un mensaje PitchBend.

### 2.1.1 Derivando frecuencias musicales de números de notas MIDI

El primer método que vas a aprender es el conversor de nota MIDI a frecuencia, conocido en ChucK como Std.mtof() (mtof, por MIDI to frequency, de MIDI a frecuencia). Así como con los nombres de las variables, verás que las funciones de la biblioteca casi siempre tienen nombres con sentido, aunque puedan parecer crípticos al principio.

Para convertir de números de notas MIDI a a frecuencias, primero tienes que saber cómo funcionan los números de notas MIDI. Los números de notas MIDI son enteros que corresponden a las teclas individuales de un un teclado musical, como se muestra en la figura 2.1. Las notas MIDI (teclas del teclado) son designadas por números enteros en el rango entre 0 y 127 (lo que corresponde al standard MIDi para la mayor parte de los parámetros MIDI). Los números de notas MIDI son dispuestos con tal que el C central (también llamado C) sea cercano a 60, cerca de la mitad del rango de notas MIDI.

El teclado musical standard tipo pianos

> Este tipo de teclado de teclas blancas y negras es denominado frecuentemente como teclado de piano o teclado musical. Esto es útil para distinguirlo de otros teclados, como el teclado QWERTY del computador en el que escribes.

> Algunos fabricantes decidieron que el C central debería ser C3, lo que corresponde a la nota MIDI #48, así que podrías encontrar diferencias en octavas entre distintos dispositivos y software computacional. Nosotros vamos a seguir con el C central correcto según la teoría musical, esto es, C central = C4 = 60.

Los números de notas MIDI combinados con la función Std.mtof(), hacen fácil crear melodías, tocar escalas y hacer otras tareas musicaesl. Para probarla, escribe y corre este código:

```chuck
<<< Std.mtof(57) >>>;
```

Deberías ver lo siguiente impreso en la ventana Console Monitor

```chuck
220.000000 :(float)
```

¡Corresponde la frecuencia de tu primra nota "Twinkle"!

FUNCIONES: ARGUMENTOS Y VALORES DE RETORNO

El número en los paréntesis (57 en este caso) es llamado el argumento de la función. La ejecuión de una función recibe el nombre de invocar o llamar a la función. El valor que resulta de usar la función es llamado el valor de retorno.

Ahora prueba escribiendo

```chuck
<<< Std.mtof(60), Std.mtof(62), Std.mtof(64), Std.mtof(65), Std.mtof(67)>>>;
```

lo que debería arrojar lo siguiente en la ventana Console:

```chuck
261.625565 293.664768 329.627557 349.228231 391.995436
```

Estas son las frecuencias de las primeras cinco notas de la escala de C: C, D, E, F, G. Revisando el teclado de la figura 2.1, puedes ver que si cuentas cada tecla (tanto blanca como negra), empezando en la 60 correspondiente a C4, las teclas blancas de la escala de C están asociadas a los números 60, 62, 64, 65 y 67.

Nota sobre programar en ChucK

> Tal como en el resto de la programación de computadores, existen múltiples maneras de lograr el mismo resultado. Las funciones de ChucK pueden ser usadas de dos maneras distintas: llamándolas con paréntesis y un argumento, como

> ```chuck
<<< Std.mtof(64) >>>;
```

> o haciendo ChucKing del argumento a la función:

> ```chuck
<<< 64 => Std.mtof >>>;
```

> Ambos hacen lo mismo y arrojarán el mismo resultado (valor de retorno).

Ahora,


HEREIAM
page 50
page 51
page 52
page 53
page 54
page 55
page 56
page 57
page 58
page 59
page 60

## 2.2

## 2.3

## 2.4

## 2.5
