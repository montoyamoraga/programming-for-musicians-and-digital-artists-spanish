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

//(2)define el valor almacenado en el elemento cero
57 => a[0];
57 => a[1];
64 => a[2];
64 => a[3];
66 => a[4];
66 => a[5];
64 => a[6];
//(3)define el valor almacenado en el último elemento

<<< a[0], a[1], a[2], a[3], a[4], a[5], a[6] >>>
```

Ahora tienes

HEREIAM
page 63
page 64
page 65
page 66
page 67
page 68
page 69

## 3.2 Grabar y modificar datos en arreglos

## 3.3

## 3.4

### 3.4.1

### 3.4.2

## 3.5

## 3.6 Resumen
