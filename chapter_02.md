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

Ahora, usemos un bucle for para tocar muchas notas usando la función Std.mtof. El código del listado 2.1 muestra un programa que toca la escala cromática completa (cada nota del teclado musical, tanto blanca como negra) usando un oscilador de onda triangular (TriOsc). Lo logras por medio de la creación de un bucle for que va de 0 a 127 (cubriendo cada nota MIDI), almacenando este valor en la variable i. Observa que la variable i se usa como entrada (también conocido como argumento de la función) a Std.mtof() (1). Entonces la primera vez que se recorre el bucle for, la nota MIDI 0 es convertida a 8.175799 Hz, que corresponde a una nota C que es muy grave como para ser escuchada, cinco octavas bajo el C central (también conocido como C-1).

Listado 2.1 Tocando una escala crómatica usando Std.mtof

```chuck
//cadena de sonido
TriOsc t => dac;
0.4 => t.gain;

//bucle
for (0 => int i; i < 127; i++) {
  //(1) Usar Std.mtof para convertir de número de nota a frecuencia
  Std.mtof(i) => float Hz;
  //imprimir el resultado
  <<< i, Hz >>>
  //(2) Definir en Hz la frecuencia del oscilador
  Hz => t.freq;
  //Hacer avanzar el tiempo
  200 :: ms => now;
}
```

En la segunda iteración, la nota MIDI 1 es convertida a 8.661597 Hz. Cada vez que se ejecuta el bucle, el programa imprime el valor de la nota MIDI correspondiente y el valor convertido a frecuencia en Hz. El valor de frecuencia es ChucKed a t.freq (2) en cada iteración para que escuches la escala. En la tercera iteración, la nota MIDI 2 se convierte en 9.177024 Hz. No serás capaz de escuchar muchas de las primeras notas graves, porque son muy bajas en frecuencia como para ser posibles de reproducir con tus parlantes. Incluso cuando empieces a escuchar o sentir el sonido, tus oídos no serán capaces de percibir una altura en las frecuencias muy graves.

PRUEBA ESTO Corre el código del listado 2.1 varias veces, y trata de determinar la frecuencia más grave que puedes escuchar. Prueba con parlantes y con audífonos. ¿En qué frecuencia realmente empiezas a escuchar alturas en vez de solo sentir el sonido? El oído humano es más sensible a ciertos rangos de frecuencias que otros. ¿Cuál es la frecuencia que escuchas a mayor volumen?

PRUEBA ESTO Reescribe el bucle for para tocar la escala cromática, usando un bucle tipo while. ¡Recuerda incrementar el contador dentro del bucle!

Como puedes imaginar, en adelante el método Std.mtof() va a jugar un gran rol en la creación de melodías. Es también posible ir en la otra dirección y usar el método Std.ftom() para convertir frecuencias en notas MIDI. Esto puede resultar útil si tú tienes alguna frecuencia particular y quieres saber la nota musical más cercana a ella, or si estás utilizando un sintetizador externo de software o hardware que acepta notas MIDI. Aprenderás sobre cómo enviar notas MIDI a sintetizadores y otros dispositivos conectados a tu computador en el capítulo 11.

NOTA PARA MÚSICOS Std.mtof() puede también aceptar valores de punto flotante para ayudarte con escalas microtonales y alturas, con lo que Std.mtof(60.5) resulta en un cuarto de tono entre C y C#.

### 2.1.2 Convirtiendo entre tipos de datos: float a int

El siguiente método de la bilbioteca Standard te permitirá convertir números de punto flotante en enteros. Cuando te empezaste a dar cuenta que necesitabas números de punto flotante para expresar de forma precisa frecuencias y ganancias, es posible que hayas escrito alguna línea que arrojara un error porque estabas tratando de almacenar una variable tipo float en otra tipo int. El ejemplo más simple de esto es

```chuck
220.0 => int myFreq;
```

que arroja un error que es impreso en la ventana Console Monitor:

```chuck
line(1): cannto resolve operator "=>" on types 'float' and 'int'...
```

Esto es ChucK protegiéndote de una forma, al decirte que podrías estar perdiendo en la conversión de número flotante a entero. Un ejemplo de esto sería

```chuck
220.5 => int myInt;
```

donde la parte .5 sería desechada sin que te dieras cuenta. Es por esto que ChucK se niega a hacerlo e imprime un error. ChucK como lenguaje es de tipado fuerte, lo que significa que los tipos de datos son mantenidos con relativa pureza, y convertir de uno a otro puede requerir un poco de trabajo de tu parte. Interesantemente, convertir un int a un float no requiere trabajo, porque no se pierdan datos en el proceso. Entonces

```chuck
220 => float myFloatFreq;
```

sí funciona, porque 220 es cambiado a 220.0, no se pierden datos, y de hecho se gana precisión, para uso posterior.

Ir en dirección contraria, de float a int, requiere lo que se llaama una conversión o casteo de tipo, lo que significa tomar datos de un tipo y castearlos en otro de la mejor forma posible en otro tipo de datos. La más común que necesitarás como programador de ChucK es ir de float a int, lo que se logra usando la función Std.ftoi (float to integer, de punto flotante a entero) de la siguiente forma:

```chuck
220.5 => Std.ftoi => int myFreq; (o también Std.ftoi(220.5) => int myFreq;)
```

Nota que en este caso, la parte fraccional es truncada (desechada), resultando en 220. Incluso si especificaras

```chuck
Std.ftoi(220.999999999) => int myFreq;
```

también obtendrías 220, porque toda parte fraccional es desechada.

### 2.1.3 Obteniendo un entero a partir de un número expresado como texto

Otro ejemplo de conversión de tipo de datos podría ser si tuvieras una variable string con valor "128.7", y lo quisieras usar como float para definir una frecuencia u otra cosa. Muchos programs aceptan entradas del usuario mediante la escritura de la información en el computador, y ChucK también puede leer datos de tu computador, algunos de los que podrían contener texto. Afortunadamente, existen también métodos de la biblioteca Std para convertir strings (conocidos también como ASCII, de donde proviene la a de la función atoi) a y en enteros y números de punto flotante. Para hacerlo usa Std.atoi("128.7"); como se muestra en la tabla 2.1

Tabla 2.1 Funciones de la biblioteca Standard de ChucK para convertir datos de tipo string, float e int

| Método           | Resultado | Descripción                       |
| :--------------  | :-------- | :-------------------------------- |
| Std.atoi(string) | int       | Convierte ASCII (string) en int   |
| Std.atof(string) | float     | Convierte ASCII (string) en float |
| Std.itoa(int)    | string    | Convierte int en ASCII (string)   |
| Std.ftoa(float)  | string    | Convierte float en ASCII (string) |

Existen más funciones útiles de la biblioteca Std, cuyo listado y descripción completos se encuentra en el apéndice B, incluyendo los de calcular el valor absoluto de números (la función valor absoluta retorna números negativos como positivos, y no altera los números positivos) y retorna el signo, negativo o positivo, de los números

Standard no es la única biblioteca disponible en ChucK, así que dejémosla por ahora y continuemos con otra biblioteca muy útil, llamada Math

## 2.2 La biblioteca Math de ChucK

¿Recuerdas cuán entretenido era cuando usabas Math.random() para definir las frecuencias y duraciones de notas en un bucle infinito? En las siguientes subsecciones, profundizaremos en otra biblioteca, conocida como Math, que tiene un gran número de funciones útiles que te permitirán hacer que tus programas sean mucho más poderosos y expresivos.

### 2.2.1 Funciones random de la biblitoeca Math



HEREIAM
page 53
page 54
page 55
page 56
page 57
page 58
page 59
page 60

## 2.3

## 2.4

## 2.5
