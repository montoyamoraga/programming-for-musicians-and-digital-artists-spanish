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

### 2.1.1 Derivando frecuencias musicales de números de notas MIDI

La Interfaz Digital de Instrumentos Musicales (MIDI, por la sigla en inglés)

> La Interfaz Digital de Instrumentos Musicales (MIDI) fue adoptada en los años 1980s por casi todos los fabricantes de sintetizadores de teclados y electrónicos, órganos y pianos digitales y luego por los fabricantes de copmputadores, entre otros. MIDI le permitió a los instrumentos musicales electrónicos, controladores, computadores, programas de computador de notación musical y más, comunicar entre ellos cosas musicales, usando el mismo conjunto de códigos y números preestablecidos. Por ejemplo, un computador puede ordenarle a un sintetizador que toque una nota mediante el envío del mensaje NoteOn o alterar al altura mandando un mensaje PitchBend.

HEREIAM
page 48
page 49
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
