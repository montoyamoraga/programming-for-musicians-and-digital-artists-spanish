# Manipulación de sonido y archivos de sonido

Este capítulo cubre:
* Almacenamiento de sonido en computadores
* Carga y reproducción de archivos de sonido en ChucK usando SndBuf
* Trabajo con archivos stereo
* Modulo, un nuevo y útil operador aritmético
* Construcción de tu propia máquina de ritmos usando samples

Hasta el momento solo has usado osciladores y ruido para hacer tus sonidos y composiciones. Pero hay mucho más en la composición y producción de música que estos sonidos. Existen un sinnúmero de tupos de sonidos en el mundo y en la música. En este capítulo, te mostraremos cómo usar archivos de sonido en ChucK. Los archivos de sonido, como .wav y .aif, u otros archivos que tengas en tu computador o has visto en internet, son a menudo llamados samples, por razones que verás pronto. Los samples son una manera fácil y efectiva de construir elementos sónicos y musicales con mucha variedad y realismo. Usando samples, puedes acceder una gran colección de sonidos existentes que otra gente ha grabado y sintetizado a lo largo de los años.

En los sigiuentes capítulos aprenderas a sintetizar tus propios sonidos desde cero. Pero en este capítulo, usarás samples para llegar al punto que puedes crear tu propia máquina de ritmos con muchas nuevas habilidades de programación. Primero, le daremos un vistazo rápido a cómo el sonido es transformado de formas de onda en el aire a números digitales para almacenamiento y transmisión por computador, llamado sampling. Luego profundizaremos en cómo usar samples en ChucK. También revisaremos nuevas maneras de usar arreglos e introduciremos un nuevo operador matemático llamado modulo.

## 4.1 Sampling: convertir sonido en números

Primero hablemos un poco de sonido. Definimos sonido como el viaje de compresiones fluctuando a través del aire, desde una fuente y hasta nuestros oídos. En los sistemas de computadores, el sonido es almacenado y reproducido en varios formatos digitales como números. Aunque no necesitas saber los detalles de todo esto para usar archivos de sonido en ChucK, es importante que sepas algunas cosas cuando cargas y reproduces archivos de sonido y cuando empieces a sintetizar sonidos en el futuro.

Para obtener sonido en un formato que tu computador y ChucK pueden entender, el sonido debe ser transformado en un flujo de números, que recibe el nombre de señal digital. El proceso de convertir una forma de onda sonora en una señal digital es llamado conversión análogo a digital. El dispositivo que hace esto se llamada conversor análogo-digital (ADC, por analog-to-digital converter), que es esencialmente lo opuesto al conversor digital-análogo (DAC) que has estado usando todo este tiempo para lograr que las señales digitales de ChucK sean emitidas por tus parlantes o audífonos. El ADC funciona por medio de la inspección electrónica de la forma de onda entrante (esto es llamado sampling o muestreo) a intervalos regulares, llamado el periodo de sampling o muestreo (que equivale a 1/ tasa de sampleo o muestreo) y el redondeo al entero más cercano disponible para representar y almacenar esa amplitud. La figura 4.1 muestra este proceso, haciendo zoom en en periodo de "eee" de nuestro sonido "see" anterior. Hacemos aún más zoom al inicio de la forma de onda y mostramos unos pocos números enteros individuales que representan los primeros valores de samples.

En el capítulo anterior, cuando hablamos de arreglos, dijimos que los arreglos pueden almacenar lo que sea, y todo en tu computador está almacenado en un arreglo. El proceso mostrado en la figura 4.1 está haciendo exactamente eso, transformando los valores de la forma de onda en una secuencia de números y almacenándolos en un arreglo. Aquí los índices (0, 1, 2, ...) representan el tiempo en aumento, una cuenta por cada periodo de sampleo (1 / tasa de sampleo). Los niveles correspondientes a cada punto en el tiempo (sample) son enteros en muchos de los formatos de sonido como .wav y .aiff, pero ChucK los convierte en números de punto flotante en el rango +1.0 a -1.0.

Todas las formas de onda que hasta visto, incluyendo la de la figura 4.1, requieren tanto números positivos como negativos porque la línea de la mitad es el cero. Esto es generalmente cierto para audio, que es el viaje a través del aire de compresiones (presiones de aire mayores a la normal) y rarefacciones (menores a la normal), así que necesitas tanto números positivos para presiones de aire mayores que la presión ambiental del aire y negativos para presiones más bajas.

Entonces, cada número que representa un valor en el tiempo de una forma de onda recibe el nombre de sample y - quizás confusamente - una vez que todo el sonido es transformado en un torrente de números, esta colección de valores, indexados en el tiempo, también recibe el nombre de sample. Cuando esa colección de valores es escrito para su almacenamiento en un disco, o como enlace en una página web, o quemado a un CD/DVD, es llamado un archivo de sonido. Otro término que deberías concoer es cuando un archivo de sonido es cargado a tu computador o sintetizador, donde a veces recibe el nombre de wavetable (tabla de onda.

Para reproducir audio, necesitas un DAC; que convierte los números de vuelta a samples de forma de onda continua, usualmente voltajes, en tus parlates o audífonos. Ya has usado bastante el objeto dac en ChucK para conectar tus osciladores y generadores de ruido a tus parlantes. ChucK, comibnado con el hardware de audio de tu computador, se encarga de convertir los archivos de sonido de vuelta a los voltajes necesarios para alimetnar tus audífonos o parlantes.

Sampling, tamaño de palabra, y tasa de sampleo

Los ingenieros necesitan considerar muchas aritas cuando crean y trabajan con archivo de uadio y especialmente cuando diseñan hardware para ADCs y DACs. Estos incluyen cuánto almacenamiento de computador debe ser reservado para cada valor de sample individual (llamado tamaño de palabra, típicamente 16 bits o 24 bits por sample) para música y 8 bits para sonidos de habla. Además, deben elegir cómo es sampleada la forma de onda (típicamente 44100 samples por segundo). Esto tiene implicaciones en la calidad y espacio en memoria/disco. Por ejemplo, tasas de sampleo más altas y más bits por sampe son generalmente mejores, hasta cierto punto. La calidad de sonido de un CD, a 2 bytes por sample, 2 canales y 44100 valores por segundo, consume cerca de 10 megabytes por minuto.

Para más información sobre sampling, formatos de archivos de sonido, entre otros, ve la referencia al final de este capítulo.

Ahora sabes un poco más sobre cómo el sonido llega a tu computador. Profundicemos más ahora y reproduzcamos archivos de sonido usando el generador unitario incluido en ChucK llamado SndBuf.

## 4.2 SndBuf: carga y reproducción de archivos de sonido en ChucK

SndBuf, abreviación de sound buffer (buffer de sonido), es el UGen incluido en Chuck que permite cargar archivos de audio. Para hacerlo, debes empezar con un archivo de sonido o quizás una carpeta llena de archivos de sonido. Hemos provisto una serie de archivos de sonido que puedes usar para empezar a componer y que estarás usando a lo largo de este libro, para que todos los ejemplos funcionen sin que tengas que modificar algo. También es posible que agregues tus propios archivos de sonido y que hagas funcionar todo como tú desees. Primero configurarás una estructura de archivos en tu copmutador para que ChucK pueda encontrar los archivos de sonido. Luego aprenderás muchas maneras de cargar, reproducir y manipular archivos de sonido usando SndBuf.

### 4.2.1 Organización de tus archivos de sonido

Cuando empieces a usar archivos de sonido en ChucK, primero tenemos que ayudarle a configurar la estructura de archivos de computador para que puedas seguir los ejemplos de este libro, así como adoptar buenos hábitos de organización de tu código, sonidos y canciones.

Como se muestra en la figura 4.2, recomendamos primero crear un directorio (carpeta) llamado chuck en tu computador donde grabar todos tus archivos. En este directorio también tienes que crear otro directorio llamado audio para almacenar tus samples. En la versión de ChucK instalada en tu computador, encontrarás un directorio llamado audio. Copia y pega este directorio a tu nuevo directorio chuck para que puedsas usar esos archivos de sonido mientras lees este capítulo y el resto del libro.

Ahora que tienes configurada una estructura de archivos en tu computador que hace fácil usar archivos de sonido, puedes usar ChucK para probarla, usando samples pregrabados del toque de un tambor ubicado en el directorio audio. El listado 4.1 muestra cómo lograr justo eso, usando SndBuf, que es una nuevo UGen generador de onisod que te permite cargar y reproducir samples. Aunque lo conectes al dac tal como lo habías hecho con otros UGen hasta el moento, cuando se comparar con nuestro oscilador (SinOsc, TriOsc, entre otros) y generadores de ruido, existen un númbero de cosas sobre SndBuf que necesitas conocer para usarlo.

Primero, cuando

Listado 4.1

HEREIAM
page 74
page 75
page 76
page 77
page 78
page 79
page 80
page 81
page 82
page 83
page 84
page 85
page 86
page 87
page 88
page 89
page 90
page 91

### 4.2.2 Looping (repetición automática) de tus samples

### 4.2.3 Reproducción de tus samples en reversa

### 4.2.4 Manejo de múltiples samples a la vez

## 4.3 Archivos de sonido stereo y reproducción

## 4.4

## 4.5

## 4.6

## 4.7
