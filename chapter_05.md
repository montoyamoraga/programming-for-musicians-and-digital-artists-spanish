# Funciones: haciendo tus propias herramientas

Este capítulo cubre:
* Escribir y usar funciones en ChucK
* Definir y nombrar funciones
* Argumentos de función y tipos de retorno
* Funciones que se llaman a sí mismas
* Uso de funciones para composición de sonido y música

Ahora que te has divertido creando una máquina de ritmos usando el UGen SndBuf, es hora de aprender a escribir y usar funciones. Ya has usado un número de funciones, que también has llamado métodos, cuyo rango va entre cambiar .freq y .gain de osciladores, a usar Std.mtof() para convertir números de notas MIDI en frecuencias, a generar números aleatorios usando Math.random2() y Math.random2f(). Todos estos son ejemplos de funciones.

A menudo durante la escritura de programas necesitas hacer el mismo tipo de cosas múltiples vecs y en múltiples lugares. Hasta el momento has reescrito o copiado bloques de código, posiblemente cambiando solo una pequeña parte. Has visto cómo el uso de las funciones de las bibliotecas Standard y Math incluidas en ChucK te permiten hacer mucho trabajo llamándolas con un argumento o dos. ¿Que pasaría si pudiéramos crear y llamar a nuestras propias funciones? ESo es lo que aprenderás a hacer en este capítulo.

Añadir funciones a tus programas te permitirá dividir tareas comunes en unidades individuales. Las funciones te permiten construir pequeños módulos de código que puedes usar una y otra vez. La modularidad ayuda a la reusabilidad y la colaboración con otros programadores y artistas. También hace que tu código sea de más fácil lectura.

En este capítulo aprenderás cómo escribir funciones, definirlas y nombrarlas, pasarles valores, y especificar su retorno. También veremos funciones que pueden llamarse a sí mismas, llamadas funciones recursivas. Estaremos atentos a funciones que hagan música y que mejoren aún más tus composiciones.

## 5.1 Creación y uso de funciones en tus programas

Inicialmente, estarás creando funciones en un solo archivo principal. Tendrás un archivo, grabado como myProgram.ck, por ejemplo, que contiene el programa principal que ejecuta tu código, y desde este código principal llamarás a tus funciones, cajas llamadas Function 1 y Function 2, que son definidas de la misma forma que el archivo myProgram.ck. Como están definidas en el mismo archivo, puedes usarlas con su nombre cuando sean invocadas por tu programa y composición. Una vez que hayas definido y rprobado tus funciones, usualmente las moverás al final del archivo de tu programa, aunque puedes definir tus funciones en cualquier parte.

NOTA Es posible definir funciones y grabarlas en sus propios archivos, y estarás haciendo esto más adelante, pero por ahora mantendremos todo en el mismo archivo.

### 5.1.1 Declaración de funciones

Las funciones son como las variables, así que tienes que declararlas para poder usarlas, tal como lo hiciste con los int, float Y SinOsc, entre otros. Empezaremos discutiendo cómo declarar una función. Existen cuatro partes principales en la declaración de una función, como se muestra en la figura 5.2. Empiezas la línea con la palabra function, o con su abreviación fun. A continuación, declaras el tipo de retorno, o el tipo de datos que la función arroja tras su ejecución. Por ejemplo, esta función retornará un entero cuando termine su ejecución; esto es como Std.ftoi(), que retorna un entero a partir de un float.

NOTA Usar una función también recibe a veces el nombre de llamar o invocar una función.

A continuación, le darás un nombre a la función. Tal como en el caso de las variables, puedes nombrar las funciones como quieras, aunque es útil usar nombres con sentido y que impliquen la utilidad de la función. Para finalizar, haces una lista de los argumentos de entrada, los que son opcionales. Estas son variables que le pasas a la función para su uso durante la ejecución. Pueden haber muchos distintos argumentos de entrada distintos, todos de tipos distintos, basado en tus decisiones de diseño como programador.

### 5.1.2 Tu primera función musical

Observemos ahora una función útil musicalmente, una que retorna el intervalo entre dos números de notas MIDI

```chuck
function int interval(int note1, int note2)
{
  note1 - note2 => int result;
  return result;
}
```

Sí, esto es bastante trivial, porque podrías haber restado los dos números de notas, pero hacerlo en una función cumple con dos propósitos; puedes usarla una y otra vez y puedes nombrarla con algo que te recuerde por qué estás haciendo el cálculo, en este caso, interval.

Como puedes ver en la figura 5.3, puedes pensar en una función como un módulo de código con valores de entradas y un valor de salida. Este tiene dos entradas de variables enteras llamadas note1 y note2. La función usa esos valores y crea un resultado de valor entero que es luego entregado al programa que llamó a la función. Por ejemplo, si pasas (72, 60) como entrada, como los argumentos, la función interval retorna 12 (72-60), el número de pasos en una octava.

Para entender mejor este proceso, empecemos con un ejemplo aún más simple, una función que sube en una octava cualquier número de nota MIDI que se le pasa, como se muestra en el listado 5.1. Primero creas una función llamada addOctave (1) con un arugmento de entrada entero que llamas note y declaras un tipo de retono int. Luego creas un resultado entero llamado result (2), que puedes usar para almacenar tu respuesta final. Sumas 12 (una octava) a tu variable note (3) y almacenas ese nuevo valor en tu variable result. La variable result es retornada al programa principal.

Cuando el programa empieza, siempre parte en el programa principal y no corre la función hasta sque es llamada. El programa empieza donde addOctave() es llamada (5). Observa que addOctave() tiene un par de paréntesis, y aquí puedes ingresar cualquier entrada que quieras (pero debe ser un entero). En este caso empiezas pasando el número 60, y la función es ejecutada usando 60 como argumento y corriendo la función. Al final de la función, el resultado es retornado, enviando 72 a answer (6).

Listado 5.2 Definición y prueba de una función que suma una octava a cualquier número de nota MIDI

```chuck
//Una función simple de ejemplo

//declaramos nuestra función aquí
//(1) Declaración de la función
fun int addOctave(int note)
{
  //(2) Resultado a retornar
  int result;
  //(3) Calcula el valor a retornar
  note + 12 => result;
  //(4) Lo retorna
  return result
}

//Programa principal de prueba de addOctave, llama e imprime el resultado
//(5) Usa la función
addOctave(60) => int answer;

//(6) Revisa el resultado
<<< answer >>>;
```

answer imprime esto en la consola:

```chuck
72 :(int)
```

Llamar a addOctage con un argumento de 72 retorna 84, 90 retorna 102, y así. Definir una función como addOctave te permite usarla una y otra vez y como tiene un nombre lleno de significado, puedes saber qué estas haciendo cada vez que la usas o la vez en el código.

> Dos maneras de llamar funciones en ChucK

> Las funciones con un argumento pueden ser llamadas de dos maneras:

> ```chuck
addOctave(60);
```

> o

>  ```chuck
60 => addOctave;
```

> Estas dos maneras de invocar una función funcionan exactamente de la misma manera.

Hagamos una prueba simple sonora/musical de la función addOctave y probemos también dos maneras de llamar funciones, añadiendo estas líneas al listado 5.1. En el listado 5.2 tocas un tono sinusoidal (1) en el C central (nota MIDI 60, asignada a una variable entera llamada note (2)). Primero usa la forma con paréntesis de llamar a Std.mtof (3) y luego usa la forma "hacer ChucKing" de la función addOctave y la función Std.mtof (4). Como puedes ver, las dos funcionan bien.

Listado 5.2 Prueba con sonido de la función addOctave

```chuck
//Usemos la función addOctave para hacer música
//(1) Oscilador para poder escuchar la función addOctave
SinOsc s => dac;
//(2) Nota inicial
60 => int myNote;

//(3) Toca la nota inicial
Std.mtof(myNote) => s.freq;
second => now;

//(4) Toca una octava más arriba
myNote => addOctave => Std.mtof => s.freq;
second => now;
```

Ahora volvamos atrás y probemos la  función interval. Pasa dos argumentos de entrada a la función, note1 y note2, como se muestra en el siguiente listado. La función es ejecutada dps veces, con dos pares de números distintos, que luego calculan result, dos veces, retornándolo al programa principal para ser impresos, resultando esto en la consola:

```chuck
12 -7
```

Listado 5.3 Definición y prueba de una función de intervalo MIDI

```chuck
//Definición de la función
fun int interval(int note1, int note2)
{
  note2 - note1 => int result;
  return result;
}

//programa principal, prueba e imprime
interval (60, 72) => int int1;
interval (67, 60) => int int2;

<<< int1, int2 >>>
```

### 5.1.3 Variable globales versus locales

En la versión modificada del programa mostrada en el listado 5.4, es importante entender que las variables globales pueden ser usadas en cualquier lugar, incluyendo dentro de las funciones o estructuras con un par de llaves {}; las variables locales de la función, en este caso, result, no pueden ser llamadas fuera del ámbito (scope).

Cada variable tiene un ámbito basado en dónde fue definida, llamado localidad. En cada función, existe un par de llaves {}. Esto define el área del ámbito local. Entonces en los programas 5.4 y 5.5, la función interval posee las variables de ámbito local note1, note2 y result.  El programa principal, como se muestra en el listado 5.4, posee las variables de ámbito global int1, int2, glob y howdy.

El programa en el siguiente listado dará un error, "line 16: undefined variable result" (línea 16: resultado de variable indefinida). Pero si decides borrar la última línea, o comentarla al insertar // al principio, el programa correrá sin problemas.

Listado 5.4 Ámbito de variables locales versus globales

```chuck
//define algunas variables globales
"HODY" => string howdy;
100.0 => float glob;
int int1, int2;

//Definición de la función
fun int interval(int note1, int note2)
{
  int result;
  note2 - note1 => result;
  <<< howdy, glob >>>;
  return result;
}

//Programa principal, prueba e imprime
interval(60, 72) => int1;
interval(67, 60) => int2;

<<< int1, int2 >>>;

<<< result >>>; //Esta línea causará el error
```

Hasta el momento has hecho y usado muchas funciones simples que operan en enteros interpretados como números de notas MIDI. Ahora harás y usarás algunas nuevas funciones tipo float para ganancia y frecuencia.

## 5.2 Algunas funciones para calcular ganancia y frecuencia

HEREIAM
page 97
page 98
page 99
page 100
page 101
page 102
page 103
page 104
page 105
page 106
page 107
page 108
page 109
page 110
page 111
page 112
page 113
page 114

### 5.2.1

### 5.2.2

### 5.2.3

## 5.3

### 5.3.1

### 5.3.2

### 5.3.3

## 5.4

### 5.4.1

### 5.4.2

### 5.4.3

## 5.5

## 5.6 Resumen
