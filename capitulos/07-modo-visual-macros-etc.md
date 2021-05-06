# Modo Visual, macros y otros

## Modo Visual

Ya hemos visto los modos *Normal*, *Insertar* y *Comando*. Nos queda un último modo importante: el modo *Visual*.

La base de dicho modo es la de seleccionar texto, de forma análoga a como se hace en las aplicaciones de interfaz gráfica. En el caso de *Vim*, el texto marcado queda visiblemente resaltado. La posición inicial de la marca de texto viene definida por la posición del cursor en el momento de entrar en modo *Visual*. Tras entrar en ese modo, podemos mover el cursor utilizando las mismas acciones de movimiento que en modo *Normal*. Este movimiento del cursor es el que marca la posición final del texto a seleccionar. Podemos ver la zona seleccionada resaltada en tiempo real. Podemos mover el cursor a una posición anterior o posterior a la posición inicial.

Existen tres formas de entrar en modo *Visual*, al que se accede desde el modo *Normal*:

- Modo *Visual* (`v`) - el texto resaltado será el comprendido entre la posición inicial y la final, ambas incluidas, siguiendo el flujo normal del texto.

- Modo de *Línea Visual* (`V`) - en este modo se seleccionan líneas enteras, desde la que contiene la posición inicial hasta la que contiene la posición final del cursor, ambas líneas incluidas.

- Modo de *Bloque Visual* (`C-v`) - en este caso se resaltará el texto incluido en el rectángulo formado por las dos posiciones del cursor (inicial y final, ambas incluidas). Dichas posiciones indicarán dos esquinas opuestas de un rectángulo de selección.

Mientras podamos ver el texto resaltado, permaneceremos en modo *Visual*. Hay dos formas básicas de salir de ese modo: presionar `<Esc>` para cancelar la operación, o presionar una tecla de acción que actúe sobre el texto resaltado.

Por ejemplo, estando en modo *Visual*, con el texto resaltado, podemos realizar cualquiera de las acciones de *yank*, *change* o *delete*, para posteriormente hacer un *put* del texto pertinente. Pero también se pueden realizar otras operaciones con el texto resaltado, como por ejemplo cambiar el estado de mayúscula o minúscula del texto seleccionado mediante `~`. Una vez realizada la operación correspondiente, el editor vuelve automáticamente al modo *Normal*.

## Objetos de texto

*Vim* ofrece la posibilidad de definir, de forma sencilla, unas entidades de fácil identificación llamadas «objetos de texto». Se trata de fragmentos de texto con un significado especial según los caracteres que los delimitan. Así, podemos hablar de palabras, frases, párrafos o áreas de texto delimitadas por:

- paréntesis (...)
- llaves {...}
- corchetes [...]
- comillas dobles "..."
- comillas simples '...'
- acentos agudos \`...\`
- etiquetas *html* (\<p\>...\</p\>)
- corchetes angulares \<...\>

Cada uno de estos objetos de texto tiene una variedad normal y una variedad interior. La variedad normal viene caracterizada por la letra 'a', y la interior, por la letra 'i', como veremos a continuación.

### Palabra, frase y párrafo

Una palabra (*word*) es una sucesión de caracteres no blancos delimitados por espacios o signos de puntuación. La palabra empieza en su primer carácter. La variedad interior termina en su último carácter, mientras que la variedad normal incluye también el espacio blanco que exista justo después de la palabra (*trailing space*).

La secuencia característica de la palabra normal es `aw` (*a word*), mientras que la de la palabra interior es `iw` (*inner word*).

Una frase (*sentence*) puede contener espacios y signos de puntuación. La separación entre frases queda marcada por uno de los caracteres '.', '?' o '!', seguido de un carácter blanco como mínimo (espacio, tabulador o fin de línea). La frase empieza en el primer carácter no blanco de la misma. La variante interior termina en el signo de puntuación final (incluido), mientras que la variante normal incluye también todo el espacio blanco tras dicho signo de puntuación.

La secuencia característica de la frase normal es `as` (*a sentence*), mientras que la de la frase interior es `is` (*inner sentence*).

Un párrafo (*paragraph*) es un fragmento de texto seguido de dos caracteres de fin de línea consecutivos. El párrafo empieza en el primer carácter no blanco del mismo. La variante interior termina en el carácter de fin de línea de su última línea con texto, incluido, mientras que la variante normal incluye también todo el espacio blanco tras dicho carácter.

La secuencia característica del párrafo normal es `ap` (*a paragraph*), mientras que la del párrafo interior es `ip` (*inner paragraph*).

### Objetos delimitados

Los bloques de texto encerrados entre un par de caracteres delimitadores también tienen su forma normal e interior. La forma interior se refiere al texto delimitado, sin incluir ese par de delimitadores. La variante normal, por supuesto, incluye dichos caracteres.

Las secuencias características de estos objetos son, en sus variantes normales, `a(`, `a{`, `a[`, `a"`, `a'`, ``a` ``, `at` y `a>`, correspondientes a los objetos delimitados respectivamente por paréntesis, llaves, corchetes, comillas dobles, comillas simples, acentos agudos, etiquetas *html* y corchetes angulares. Las secuencias correspondientes a sus variantes interiores son, respectivamente, `i(`, `i{`, `i[`, `i"`, `i'`, ``i` ``, `it` y `i>`.

### Uso de los objetos de texto

Al entrar en modo *Visual*, podemos utilizar la secuencia característica de un tipo de objeto de texto concreto tras situar el cursor sobre uno de los caracteres pertenecientes a dicho objeto. Con esto, se recalculará el fragmento de texto resaltado. Dado que seguiremos en modo *Visual*, posteriormente podremos seguir modificando la selección, ya sea moviendo el cursor, o aplicando otras secuencias características de objetos de texto. Aplicar la misma secuencia repetidamente equivale a hacerlo con argumento numérico.

En caso de que antes de ejecutar la secuencia del objeto de texto estemos en modo *Línea Visual* o *Bloque Visual*, el modo cambiará automáticamente a *Visual*. También conviene tener en cuenta que en el caso de objetos de texto anidados, se hará la selección del bloque más interior posible, aunque repetir la acción repetidamente (o hacerlo con argumento numérico) irá seleccionando bloques más y más exteriores (si los hay).

En general, si aplicamos alguna de estas secuencias sobre un carácter que no pertenezca a un objeto de texto como el especificado, la secuencia no hará nada.

En cuanto al uso de objetos de texto en modo *Normal*, debe hacerse en conjunción con una acción que actúe sobre el texto seleccionado, como puede ser un borrado, o un *yank*, entre otros. Con el cursor sobre algún carácter perteneciente al objeto que nos interesa, teclearemos primero la acción concreta e, inmediatamente después, la secuencia correspondiente al objeto de texto, con o sin argumento numérico (que también puede indicarse antes de la acción).

## Macros

Es frecuente repetir numerosas veces las mismas secuencias de operaciones, de forma rutinaria. En estos casos resultan muy útiles las macros. Se trata de la definición de macroinstrucciones que agrupan un número más o menos grande de instrucciones o secuencias de operaciones que pueden ser reproducidas posteriormente como una sola operación.

El proceso de grabación de una macro es muy simple. Asumimos que empezamos la grabación en modo *Normal*. Hay que tener en cuenta que la macro *se almacenará en el registro de texto que indiquemos*. Se deben realizar los siguientes pasos:

- Pulsar `q` y a continuación la letra correspondiente al registro al que deseamos asociar esa macro.
- Realizar todas las operaciones que deseamos registrar.
- Pulsar nuevamente `q`.

Para invocar posteriormente esa macro, simplemente debemos pulsar, desde modo *Normal*, `@` y a continuación la letra correspondiente al registro donde hayamos almacenado la macro.

Para repetir la última macro invocada, podemos utilizar `@@`.

Veamos un ejemplo: Si queremos crear una macro que simplemente inserte tres asteriscos ('\*\*\*') justo antes de la posición del cursor, y asociar esa acción al registro 'k', teclearemos:

`qki***<Esc>q`

Veámoslo desglosado:

- `qk` inicia la grabación de la macro en el registro 'k'.
- `i` entra en modo insertar.
- `***` inserta los tres asteriscos.
- `<Esc>` sale de modo insertar.
- `q` termina la grabación y guarda la macro (en el registro 'k').

Ahora ya estaríamos en posición de ejecutar la macro mediante `@k`, y posteriormente se podría invocar repetidamente con `@@`. Por supuesto, también podríamos insertar el contenido del registro 'k' en el texto mediante una acción *put*, es decir, podríamos insertar la secuencia grabada como texto.

Del mismo modo, existe otra forma de definir una macro, y es escribiendo las acciones en el texto que estamos editando, o modificando una macro insertada en el texto, y posteriormente copiando ese fragmento resultante hacia uno de los registros nuevamente.

Este sistema presenta una peculiaridad: hay que tener en cuenta que a la hora de ejecutar esa macro, cada uno de los caracteres de ese texto en el registro será interpretado como una sola acción (pulsación) de teclado. Por lo tanto, ¿cómo insertamos por escrito pulsaciones de teclas como `<Esc>`, `<Intro>` o `C-w`?

La solución es la orden `C-v` *al introducir texto*. Esta acción es válida en los modos *Insertar*, *Reemplazar* y *Comando*, así como en la introducción de patrones de búsqueda de texto (mediante `/` o `?`). Estando en alguno de estos modos o estados, cuando deseemos añadir una de estas teclas especiales, presionaremos `C-v`, y a continuación, la tecla concreta que deseamos insertar.

Es importante recordar que `C-v` en modo *Normal* realiza una función totalmente distinta (entra en modo de *Bloque Visual*).

Recordemos que podemos usar el comando `:reg` para comprobar el contenido de los registros, y por lo tanto, examinar las macros grabadas.

## Marcas

Con los marcas (*marks*) es posible colocar referencias a un punto concreto (línea, columna) de un archivo. Para crear una marca dentro del archivo actual, hay que colocar el cursor allí donde deseamos esa marca, y presionar `m`, seguido de la letra que queremos asociar a esa marca. Posteriormente, podemos desplazarnos a cada una de las marcas definidas, mediante las comillas simples (`'`) o el acento agudo (`` ` ``) seguido de la letra correspondiente. La diferencia entre estas dos formas de desplazamiento es que la primera dejará el cursor en el primer carácter de la línea donde está la marca, mientras que con el acento agudo, nos desplazaremos a la posición concreta, tanto a la línea como a la columna que define la marca.

Por ejemplo, si tras colocar el cursor en la posición que deseamos guardar tecleamos `ma`, definiremos la marca 'a' para el archivo actual. En cualquier otro momento que estemos editando ese archivo podremos teclear `` `a`` para colocar el cursor en esa posición concreta, o `'a` para desplazar el cursor a la columna 1 de esa línea.

Sin embargo, si tras definir la marca pasamos a editar un *buffer* distinto que no disponga de marca 'a', teclear `'a` no producirá desplazamiento del cursor si ese otro archivo no dispone de esa marca concreta. Cada archivo tiene su propio conjunto de marcas. Los desplazamientos a una marca de este tipo son *locales*, es decir, se refieren únicamente al archivo actual. Sin embargo, existe una forma de definir marcas *globales*.

Si asociamos una marca a una letra mayúscula, esa marca quedará asociada a una ubicación que será accesible globalmente, desde cualquier archivo que estemos editando. Mientras cada archivo tiene sus marcas locales, solo puede existir un conjunto de marcas globales en *Vim*. Es decir, puede haber varias marcas 'a', una por cada archivo. Pero si un archivo tiene una marca 'A', ningún otro la puede tener al mismo tiempo. En cuanto definamos una marca 'A' en otro archivo, dejará de existir en el primero.

Si nos desplazamos a una marca global que está definida en un archivo distinto del archivo actual, *Vim* activará el *buffer* pertinente; y si ese archivo no está en memoria, lo abrirá, creando un *buffer* para él.

Hay que tener en cuenta que los desplazamientos a una marca (con `'` o `` ` ``) pueden combinarse con los comandos de borrado, cambio o *yank*, del mismo modo que cualquier otro comando de movimiento del cursor. Por ejemplo, ``d`a`` eliminará el texto existente desde la posición actual del cursor (incluida) hasta la marca 'a' (no incluida). De forma similar, `d'a` eliminará desde la línea actual entera, incluida, hasta la línea donde está la marca, también incluida.

Es posible visualizar las marcas actuales definidas con el comando `:marks`. Ello nos mostrará las marcas del archivo actual, así como las marcas globales, a parte de otras marcas especiales, como la '.' que indica el lugar del último cambio realizado en el *buffer* actual.

Podemos ver una o varias marcas específicas:

`:marks aB`

nos mostrará las marcas 'a' y 'B'.

Es posible eliminar marcas mediante el comando `:delm` seguido de las marcas a borrar. Por ejemplo:

`:delm a`

elimina la marca 'a',

`:delm acB`

elimina las marcas 'a', 'c' y 'B', mientras que

`:delm a-d`

elimina las marcas 'a', 'b', 'c' y 'd'. Es posible eliminar todas las marcas locales del archivo actual mediante

`:delm!`
