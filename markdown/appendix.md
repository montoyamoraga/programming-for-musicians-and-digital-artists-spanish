# Apéndice A: instalando ChucK y miniAudicle

Este apéndice te enseña cómo descargar e instalar miniAudicle, el entorno de desarrollo integrado (IDE, por integrated development environment) de ChucK, y otros componentes de ChucK en tu computador. Para empezar, navega al sitio de ChucK en [http://chuck.stanford.edu/](http://chuck.stanford.edu/) y haz click en "download Chuck", como se muestra en la figura A.1. Esto te lleva a  la p'agina "ChucK : Release ", donde puedes encontrar enlaces de descarga para cada plataforma que soporta ChucK. Para usuarios de Mac, la siguiente sección cubre el proceso desde aquí. Los usuarios de Window pueden saltar a la siguiente sección, "Instalación en Windows", y los usuarios de Linux pueden saltar a la última sección, "Instalación en Ubuntu Linux".

## Instalación en Mac OS X

Desde la página de descarga principal "ChucK : release", como se muestra en la figura A.2., haz click en el link de "Installer" bajo el subtítulo MacOS X, y se empezará a descargar un archivo llamado chuck-W.X.Y.Z.pkg (donde W, X, Y y Z son números de versión que variarán a medida que nuevas versiones de ChucK sean liberadas). En Mac OS X, una sola aplicación instaladora se encarga de instalar tanto miniAudicle (el editor y la aplicación de desarrollo gráfica de ChucK) como el programa de línea de comandos chuck.

Abre este archivo tras descargarlo, y el diálogo de instalación standard de Mac OS X, como se muestra en la figura. Una vez que hagas click en Install, te será pedida tu contraseña; ingrésala y luego haz click en Install Software. Después de eso, la instalación tomará unos pocos minutos en finalizar. Si todo sale bien, verás la siguiente pantalla mostrada en la figura A.4, que señala el éxito.

miniAudice ahora está disponible en tu directorio de aplicaciones. ¡Ahora puedes volver al capítulo 1 y empezar a usar miniAudicle!

## Instalación en Windows

Para instalar ChucK y miniAudicle en Windows, desde la página de descargas principal ChucK : release, haz click en el enlace "Executable" bajo la sección de descargas para Windows, como se muestra en la figura A.5.

Al hacer click en este enlace se descargará un archivo llamado chuck-W.X.Y.Z.msi (donde W, X, Y y Z corresponden a los números de versión actual). Abre este archivo cuando se complete la descarga, y verás un diálogo de configuración standard, como se muestra en la figura A.6.

Haz click a través de las múltiples pantallas de este diálogo, asegurándote de aceptar la licencia de ChucK. Tras hacer click en el botón "Install", Windows te preguntará si quieres permitir cambios en tu computador, haz click en "Yes". El instalador tomará unos pocos minutos en continuar; tras finalizar, verás la pantalla mostrada en la figura A.7. Ahora puedes volver al capítulo 1 y usar miniAudicle para aprender a programar en ChucK, como se muestra en la figura A.8.

## Instalación en Ubuntu Linux

La instalación de ChucK y miniAudicle en Linux requiere un poco más de trabajo, pero con unos pocos comandos de temrinal puedes encaminarte hacia la felicidad con ChucK. Dada la variedad de sistemas Linux disponibles, hemos enfocado estas instrucciones en Ubuntu Linux, una opción popular para tanto novatos como expertos. Para otros sistemas Linux, el procedimiento de instalación es conceptualmente similar, aunque los comandos exactos variarán.

Para empezar, abre la aplicación Terminal. En Linux, ChucK y miniAudicle son compilados desde código fuente (source code), y un número de programas adicionales y bibliotecas deben ser instalados para permitir esto. En la ventana Terminal, ingresa el siguiente comando (en una sola línea) y luego presiona la tecla Return (Enter):


```shell
$ sudo apt-get install make gcc g++ bison flex libasound2-dev libsndfile1-dev liqt4-dev libqscintilla2-dev libpulse-dev
```

(El caracter $ indica la terminal y no debería ser escrito). Si se pide la contraseña, escríbela y tecla Return de nuevo. Este comando automáticamente instarlá make, gcc, g++, bison y flex, que son herramientas para construir software a partir de código fuente, mientras que el resto son bibliotecas de apoyo usadas por ChucK y miniAudicle. Este comando puede tomar un poco de tiempo en completarse, porque descarga e instala individualmente cada componente y sus dependencias.

Una vez que el comando finaliza, visita la página de ChucK ([http://chuck.stanford.edu/](http://chuck.stanford.edu/)), haz click en el enlace "Download", y luego haz click en "Source" bajo la categoría Linux. Esto enlaza a un archivo tar-zip (con una extensión .tgz), que es un archivo que contiene el código fuente. Deberías grabar el archivo fuente en una ubicación de tu disco duro, como el directorio de descargas. Mientras se está descargando, haz click en el enlace miniAudicle bajo la sección Linux del sitio web. Esto te llevará a la página web de miniAudicle, donde se puede encontrar un enlace al código fuente de miniAudicle. Haz click en este enlace, y también empezará la descarga de un archivo .tgz.

Cuando finalicen ambas descargas, vuelve a Terminal. Ingresa el comando

```shell
$ cd Downloads
```

donde Downloads es el directorio donde descargaste los archivos .tgz. Esto hace que tu sesión de terminal navegue a ese directorio. Ahora desempaqueta los archivos con estos comandos:

```shell
$ tar xzf chuck-W.X.Y.Z.tgz
$ tar xzf miniAudicle-A.B.C.tgz
```

Cuando ingreses los comandos, asegúrate que W.X.Y.Z. y A.B.C. calcen con las versiones reales de ChucK y miniAudicle que descargaste. Ahora navega al directorio fuente (source, src) de Chuck, recién desempaquetado, e ingresa el comando para empezar el procedimiento de construcción (build):

```shell
$ cd chuck-W.X.Y.Z/src
$ make linux-pulse
```
Esto tomará un poco de tiempo, porque todo los archivos de código fuente son compilados en un solo programa. Cuando el comando make termina, el último paso es instalar el recién compilado programa:

```shell
$ sudo make install
```

Necesitarás ingresar tu contraseña. Puedes probar que esto funciona ingresando el comando:

```shell
$ chuck --version
```

Esto imprimirá lo siguiente:

```shell
chuck version: W.X.Y.Z (chimera)
  linux (pulse): 64 bit
  http://chuck.cs.princeton.edu/
  http://chuck.stanford.edu/
```

Compilar miniAudicle, el editor gráfico de ChucK, es un procedimiento similar, navega al directorio fuente de miniAudicle, compila el programa y luego instálalo.

```shell
$ cd ../../miniAudicle-A.B.C/src
$ make linux-pulse
muchas cosas son impresas...
$ sudo make install
```

Si todo salió bien, puedes ahora iniciar miniAudicle tecleando lo siguiente en la línea de comandos:

```shell
$ miniAudicle
```

El editor de miniAudicle y las ventanas asociadas deberían aparecer en tu pantalla. ¡Ahora estás listo para trabajar con ChucK!

> Chuck en Raspberry Pi

> Correr ChucK en una Raspberry Pi es también muy sencillo. Si tu Raspberry Pi está corriendo Ubuntu Linux o una variante relacionada, puedes seguir las instrucciones previas para Linux. Alternativamente, te recomendamos instalar Satellite CCRMA en tu Raspberry Pi. Satellite CCRMA es una versión completamente personalizada de Linux que incluye un número de aplicaciones para música por computador y computación física, incluyendo ChucK. Para más información, visita [http://ccrma.stanford.edu/~eberdahl/satellite/](http://ccrma.stanford.edu/~eberdahl/satellite/).

# Apéndice B

page 241
page 242
page 243
page 244
page 245
page 246
page 247
page 248

# Apéndice C

page 249
page 250
page 251
page 252
page 253
page 254
page 255
page 256
page 257
page 258
page 259
page 260
page 261
page 262
page 263
page 264
page 265
page 266
page 267
page 268
page 269
page 270

# Apéndice D

page 271
page 272
page 273
page 274
page 275

# Apéndice E

page 276
page 277
page 278
page 279
page 280
page 281

# Apéndice F

page 282
page 283
page 284
page 285
page 286

# Apéndice G

page 287
page 288
page 289
page 290
page 291
page 292
page 293
page 294
page 295
page 296
page 297

# Apéndice H

page 298
page 299
page 300
page 301
