# Arreglos: ordenar y acceder a los datos de tu composición

Este capitulo cubre:
* Declaración e inicialización de arreglos
* Recuperación y modificación de datos en arreglos
* Almacenamiento de diferentes tipos de datos en arreglos
* Uso de arreglos para controlar parámetros musicales en una canción

Ahora que has sido introducido al poder de las utiliidades de las bibliotecas Standard y Math, vas a aprender una de las cosas más importantes para hacer que el código de tus melodías y composiciones sea mucho más fácil y expresivo y te permitirá crear canciones de mucho mayor duración evitando escritura excesiva. ESte capítulo cubre arreglos, que son mecanismos superpoderosos que te permiten hacer colecciones de datos que puedes usar para variadas cosas. Los arreglos son fundamentales al funcionamiento de casi todos los programas de computadores. Son la forman en que los correos electrónicos, imágenes, archivos de sonido/música y todo lo demás son almacenados y accedidos dentro de computadores, teléfonos inteligentes, tablets y en la web.

Un arreglo es una coleccón de datos grabados en la memoria. Para los compositores-programadores, un uso de los arreglos es almacenar listas de parámetros musicales que quieres usar para controlar tus canciones a lo largo del tiempo, como alturas musicales (o números de notas MIDI), volúmenes, tiempos y/o duraciones. Otro uso para los arreglos es almacenar listas de strings como letras de canciones. Los arreglos pueden contener cualquier tipo de dato, incluso puedes hacer areglos de generadores de unidad como SinOsc o Noise, cosa que harás en el siguiente capítulo.

Hasta el momento has generado melodías usando bucles para aumetnar o disminuir frecuencias o alturas, o has hechos programas largos que especifican cada nota que deseas, línea a línea. ¿No sería genial si pudieras hacer al comienzo del programa una lista de todas las notas que quieres tocar? ¿Y si pudieras de alguna forma almacenar tus parámetros de composición en la memoria, haciendo más fácil la escritura de canciones completas? Eso es exactamente lo que los arreglos te permiten hacer.

## 3.1 Declaración y almacenamiento de datos en arreglos

Aquí empezarás a aprender sobre cómo tocar notas de nuestra canción "Twinkle" mediante el almacenamiento de ellas en un solo arreglo. En este caso, grabarás tu melodía como una lista de números de notas MIDI. Para empezar, visualicemos conceptualmente qué es un arreglo.

En la figura 3.1 puedes ver siete celdas que contienen enteros. Estas celdas son siete trozos de memoria adjuntados a tu arreglo. Dentro de las celdas tenemos números enteros; en este caso ellos representan las notas MIDI de nuestra melodía "Twinkle". Bajo cada celda, puedes ver números que corresponden al índice de cada celda. Observa que los índices empiezan en 0. Todos los arreglos empiezan con el índice 0. El índice 3 del arreglo te dará el dato en la celda, en este caso, el número 64 de nota MIDI, la cuarta nota de Twinkle.

Ahora vemaos cómo representar este concepto con código. Representas un arreglo con paréntesis cuadrados [], con el número dentro de los paréntesis cuadrados siendo el número de elementos en el arreglo. En este caso hay sietes notas MIDI, así que declaras un arreglo a como int a[7] (1) en el siguiente listado. Observa que el arreglo es de tipo entero (puedes hacer arreglos de cualquier tipo, como verás más adelante). A continuación, tienes que almacenar tu melodía en el arreglo. Esto lo haces poniendo cada entero en su ubicación correspondiente en el arreglo, uno a la vez, como se ve en las líneas entre (2) y (3).

Listado 3.1 Declarando y llenando de forma larga un arreglo de enteros

```chuck
//delaración de arreglo (método 1)
//(1) declara un arreglo de un largo específico (7)
int a[7];

//(2) define el valor almacenado en el elemento cero
57 => a[0];
57 => a[1];
64 => a[2];
64 => a[3];
66 => a[4];
66 => a[5];
64 => a[6];
//(3) define el valor almacenado en el último elemento

<<< a[0], a[1], a[2], a[3], a[4], a[5], a[6] >>>
```

Ahora tienes tus notas MIDI en el arreglo, pero esto tomó muchas líneas de código. ¿Existe un atajo? ¡Por supuesto que sí! Como se muestra en el listado 3.2, puedes grabar todas tus notas MIDI en una sola línea de código. Observa que ahora usamos un nuevo operador @=>, que es un operador especial de ChucK usado para almacenar todos los datos en tu arreglo a, de una vez. @=> es llamado el operador at-ChucK o el operador de asignación explícita. Puedes usar este operador para copiar otros objetos en ChucK, pero por lejos su uso más común es para copiar listas de elementos a arreglos.

Listado 3.2 Declarar e inicializar un arreglo de una vez

```chuck
[57, 57, 64, 64, 66, 66, 64] @=> int a[];

<<< a[0], a[1], a[2], a[3], a[4], a[5], a[6] >>>
```

La otra parte de esta nueva manera de declarar e inicializar un arrelgo es que ahora la declaración de tu arreglo a no necesita el [7] preasignado como número de elementos. Esta es la parte que el operador @=> te permite hacer. El arreglo a apunta al comienzo de tu lista de notas MIDI, pero el tamaño no necesita ser determinado durante la declaración. ChucK se encarga de esto por ti, haciendo fácil añadir y restar notas para hacer que la melodía sea más larga o corta, tecleando números extra en la lista, sin tener que contar cuántos notas hay cada vez.

## 3.2 Grabar y modificar datos en arreglos

AHora sabes un par de maneras de poner datos en arreglos: un elemento a la vez o copiando la lista a través de @=>, el operador at-ChucK. ¿Pero cómo accedes a estos datos? El listado 3.3 muestra cómo hacerlo. Aquí puedes leer un elemento del arreglo en la lína (1) y luego imprimir el resultado en (2). ¿Pero qué imprimie esto? Piénsalo. ¿Dijiste 57? ¡Recuerda que el índice empieza en 0! La respuesta correcta es 64 (el tercer item de la lista).

EMPEZAR EN CERO Podrías estar preguntándote por qué este libro empezó con el capítulo 0 en vez del capítulo 1. Esto es porque somos programadores, y queríamos que empezaras a pensar dedse el principio en cosas como los arreglos de los índices empezando en cero en vez de uno. Dato interesante: muchos edificios de ciencias de la computación en universidades nombran el piso base como piso cero.

¿Qué pasa si quieres cambiar la melodía desde dentro de tu programa? No es un problema; un arreglo contiene variables, así que puedes cambiar lo que está en el arreglo en cualquier momento, definiendo un nuevo entero en la ubicación a[2] del arreglo (o cualquier otra ubicación) (3).

 HEREIAM
page 64
page 65
page 66
page 67
page 68
page 69

## 3.3

## 3.4

### 3.4.1

### 3.4.2

## 3.5

## 3.6 Resumen
