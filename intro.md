# Palabras previas

"¡Feliz ChucKing!"

¿Qué significa esta corta frase, dicha por Ge Wang, uno de los autores de este excelente libro? Mi interpretación es que significa explorar y componer sonidos de manera divertida con ChucK, el lenguaje de programación que es la base de este libro. Pero, "ChucKing" es más que eso - es una manera de abordar el aprender a programar con un enfoque en las artes; es al mismo tiempo animado y profundo.

Como muchos otros, a mí me importan las artes visuales y la música por sobre todas las cosas. También estoy interesado en los computadores, pero primordialmente como herramientas para hacer imágenes y ruido. Sin embargo, como todas las veces que fallé en aprender cómo programar computadores cuando tenía entre 10 y 25 años, estuve forzado a aprender código a través de texto - imprimiendo "Hello World" en una pantalla o escribiendo código para calcular números. Una aclaración, me gusta escribir y creo que las matemáticas tienen un valor incalculable para lo que amo hacer, pero las palabras y los números nunca son el foco de atención. Siempre son un medio para llegar a un fin.

¿Qué pasaría si pudiera aprender a programar ayudado por lo que más me mporta? ¿Aprender a programar haciendo imágenes y ruidos? Antes de que los computadores se convirtieran en las extraordinarias máquinas multimedia de la actualidad, la mayor parte de las personas usaban computadors para trabajar exclusivamente con texto. Los estudiantes que estaban más interesados en imagen y sonido no eran capaces de aprender a programar a través de la persecución de sus pasiones. Afortunadamente, esto ha cambiado y ahora los computadores (desde los teléfonos móbiles hasta los supercomputadores) pueden generar imágenes, sintetizar sonido y mucho más. Desafortunadamente, la mayor parte de las clases para aprender a programar siguen siendo de la misma forma que hace 40 años - aprender a programar fuerza a todos, artistas visuales y músicos por igual, a adaptarse a las rígidas restricciones de usar caracteres alfanuméricos como entrada y salida.

Me esforcé en aprender código de la manera tradicional. Durante la última década, he enseñado a programar a través de una nueva plataforma de programación que coinventé con Ben Fry. En MIT en el año 2001, empezamos a desarrollar un entorno y lenguaje de programación llamado Processing. Processing fue creado para que aprender a programar por primera vez y alienta a "bosquejar" con código. Lo más importante de Processing es que puedes aprender todos los fundamentos de programar, pero a través del trabajo con medios visuales dinámicos - por ejemplo, con dibujo, color y animación. En el momento en que empezamos a trabajar en Processing, no sabíamos que Ge Wang, en ese entonces un estudiante de posgrado en Princeton, estaba haciendo lo mismo en el dominio de la música. A través de su lenguaje de programación sobre la marcha, ChucK, se aprende a programar a través de la creación de onsido.

Uno de los primeros programas que verás en este libro va al grano:
```
SinOsc s => dac;
440 => s.freq;
1 :: second => now;
```

Este programa crea un tono puro por un segundo: es el equivalente en software de tocar una tecla de piano por primera vez. Esta es una extraordinaria manera de aprender a programar; invita a preguntas emocionantes. ¿Qué es este extraño símbolo =>? ¿A qué se refiere el número 440? Las respuestas a estas preguntas abren un nuevo mundo; una nueva manera de pensar en cómo crear sonido y música y de forma simultánea aprender los fundamentos de la programación. Desde este programa, se abre un nuevo mundo de posibilidades.

Me emocionó leer Programación para músicos y artistas digitales. Con una multitud de ejemplos bien explicados en el fascinante lenguaje ChucK, los lectores aprenden de manera activa y comprometida. No puedo imaginarme un grupo de gente más capacitada y hábil para escribir cómo aprender a programar a través de la creación musical. Ajay Kapur, Perry Cook, Spencer Salazar y Ge crearon ChucK y desarrollaron la manera en que es enseñado. Después de una década de experiencia con ChucK en la sala de clases y una gran experiencia previa a eso, este libro deja la vara imposiblemente alta. Espero que disfrutes aprender a programar de forma "Chuckian" y "¡Feliz ChucKing!".

CASEY REAS

PROFESOR ASOCIADO

DEPARTAMENTO DE ARTES MEDIALES Y DISEÑO EN UCLA

COCREADOR DEL LENGUAJE DE PROGRAMACIÓN PROCESSING

# Prefacio

¡Bienvenido a ChucK!

Tendemos a realizar nuestro mejor esfuerzo cuando seguimos nuestros reales intereses. ChucK fue creado porque amamos de manera genuina tanto la música como la programación - y para cada cualquier persona que quiera hacer música con computadores (o que quiera aprender a hacerlo). Como el creador y el diseñador principal de Chuck, estoy convencido firmemente que programar es (o debería ser) una tarea creativa en sí misma. Debería ser entretenida, expresiva y gratificante. Y crear música a través de la programación, es doblemente asombroso.

Empecé a trabajar en ChucK en el año 2002, habiendo empezado el programa de doctorado en Ciencias de la Computación de Princeton University un año atrás. Si la música rock fue mi drogra de entrada a hacer música (y mi alma mater de pregrado Duke University mi iniciación a la programación), entonces Princeton fue donde empecé a combinar ambos elementos. Aunque no lo podía articular en ese momento, estaba atraído por la elegancia de ciertas características de los lenguajes de programación, y aspiraba a crear cosas, cosas programables con software, que empoderaran a a otros para hacer música, pero de manera estéticamente elegante y entretenida. Quería rockear, y ayudar a otra gente a rockear - con el computador.

Perry R. Cook era mi tutor (una de las mejores cosas que me han pasado a nivel personal y profesional); su trabajo en diseño de interacción física y su Synthesis ToolKit (STK) fueron una gran inspiración. También su personalidad divertida, fantástica e imaginativa ayudó a definir el tono y alentó la libertad en la exploración. Todavía recuerdo cuando le mostré a Perry mis ideas iniciales de un "nuevo lenguaje de programación para música" cuya cohesión se basaba en el operador ChucK (que se ve así =>, lo verás mucho en este libro), y a Perry diciendo, "Bueno, eso parece una locura. ¡Hazlo!"

HEREIAM page xviii

# Agradecimiento
