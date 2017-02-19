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


HEREIAM
page 94
page 95
page 96
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

### 5.1.3

## 5.2

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
