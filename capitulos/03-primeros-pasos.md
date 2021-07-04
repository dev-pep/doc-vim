# Primeros pasos

## Modos

Al entrar en *Vim*, el editor está en el llamado modo *Normal*. En este modo es donde realizaremos la mayor parte de acciones a través de diferentes pulsaciones con el teclado.

Algunas acciones de teclado se componen de una secuencia de pulsaciones consecutivas. Una secuencia sin terminar se puede cancelar en todo momento pulsando la tecla `Esc`.

Estas acciones son (utilizando como ejemplo la tecla `v`) del tipo `C-v` (`Control` + `v`), `V` (`Mayúsculas` + `v`) o `v` (tecla `v`). Las acciones compuestas por dos pulsaciones consecutivas en las que la primera incluye la tecla `Control` y la segunda es una simple tecla (minúscula) suelen tener el mismo efecto dejando la tecla `Control`
presionada en la segunda pulsación (p. e. `C-w` `j` equivale a `C-w` `C-j`). En las acciones con tecla `Control`, el estado de la tecla `Mayúsculas` es irrelevante (`C-r` equivale a `C-R`).

La mayor parte de acciones admiten un argumento numérico, que consiste en teclear las cifras del número deseado justo antes de las teclas de acción.

Algunas de las acciones disponibles cambian a modo *Insertar* (o *Reemplazar*), en el que se puede introducir texto directamente. Para salir de estos modos se utiliza la tecla `Esc`.

Mediante `:` (dos puntos) podremos acceder al modo *Comando*, en el cual podremos teclear un comando seguido de `Intro` para realizar una acción concreta.

Existe otro modo, llamado *Visual*, que se verá más adelante en el libro.

## Entrar, salir y guardar

Para ejecutar *Vim*, debemos introducir lo siguiente desde la línea de comandos:

`vim`

o

`vim <archivo>`

donde '\<archivo\>' es un nombre de archivo opcional, que se abrirá automáticamente si existe. Si no se especifica, *Vim* se inicia con un archivo vacío sin nombre. Si se especifica un nombre pero el archivo no existe, se iniciará con un archivo vacío con ese nombre, y no será creado en disco hasta que se guarde explícitamente.

Podemos observar el símbolo `~` en aquellas líneas donde no hay texto (en el archivo vacío se ve claramente).

Por otro lado, cuando la línea es demasiado grande para encajar dentro de la anchura de la pantalla (o ventana), la línea del archivo (llamada línea física) ocupa varias líneas de pantalla. Sin embargo, si no existen suficientes líneas de pantalla para mostrar la línea física completa, *Vim* no la muestra; solo aparecen símbolos ***@*** en el margen izquierdo, en las líneas de pantalla donde debería aparecer la línea física. En cambio, si la opción `display` tiene el valor `truncate`, entonces se mostrará esa línea hasta donde pueda, exceptuando la última línea de pantalla, que mostrará solo ***@@@***. Para activar esa opción, teclearemos

`:set display=truncate`

seguido de `Intro`, dado que estamos en modo comando.

Para salir de *Vim*, utilizaremos el comando

`:q`

Sin embargo, si existen cambios sin guardar, no nos permitirá salir. En ese caso, tendremos dos opciones. Una es guardar lo cambios mediante

`:w`

y salir posteriormente tecleando

`:q`

Estas dos acciones se pueden combinar en una sola:

`:wq`

La otra forma es salir descartando los cambios mediante

`:q!`

Si queremos guardar el archivo actual con otro nombre, teclearemos

`:w <archivo>`

donde \<archivo\> es el nuevo nombre, que puede ser un simple nombre, o incluir una ruta (relativa o absoluta). Hay que tener en cuenta que el directorio actual es el directorio desde el que ejecutamos *Vim*, a no ser que lo cambiemos posteriormente (lo cual podemos hacer con el comando `:cd`).

Si el texto que estamos editando no tiene todavía nombre de archivo, debemos especificar ese nombre de archivo para poderlo guardar en disco. De este modo, el texto (archivo) actual pasa a tener el nombre especificado. En cambio, si estamos editando un archivo que ya tiene nombre, guardarlo con un nuevo nombre creará una copia del mismo, con el nombre especificado, pero no cambiará el nombre del archivo actual (el que estamos editando).

Si intentamos guardar con un nombre de archivo que ya existe, no se ejecutará la acción de guardado. En ese caso, si queremos sobreescribir el archivo, debemos utilizar

`:w! <archivo>`

Por otro lado, para abrir un archivo, utilizaremos el comando

`:e <archivo>`

En este caso, si el archivo actual tiene cambios no guardados, no se abrirá el nuevo, a no ser que le indiquemos que descarte esos cambios:

`:e! <archivo>`

Si el archivo especificado no existe, se abrirá un archivo vacío con ese nombre. Para que dicho archivo sea creado en disco se deberá guardar con `:w`.

Si queremos revertir el archivo actual a la versión guardada en disco, perdiendo los cambios recientes, teclearemos

`:e!`

Si no incluimos el símbolo `!`, solo cargará esa versión guardada cuando no hayamos realizado cambios en relación a esta desde el editor. En este caso, esto solo tendría algún efecto si se hubiese realizado algún cambio en el archivo desde una aplicación externa (que podría ser otra instancia de *Vim*).

Para insertar el contenido de un archivo después de la línea actual:

`:r <archivo>`

## Movimientos del cursor

Podemos mover el cursor una posición usando las teclas `h` (izquierda), `j` (abajo), `k` (arriba) y `l` (derecha). Anteponiendo un argumento numérico, avanzaremos de golpe tantas posiciones como indiquemos. Hay que tener en cuenta que el cursor siempre debe estar encima de un carácter existente, con lo que si se encuentra encima del último carácter de la línea, no podrá avanzar más a la derecha. La única excepción es cuando nos encontramos sobre una línea
vacía (sin ningún carácter), en cuyo caso el cursor estará al principio de la misma.

Al movernos verticalmente cambiando de línea, el número de columna del cursor cambiará cuando pasemos por líneas con menos caracteres que el número de columna actual. En ese caso, *Vim* intentará en todo momento acercarse lo más posible a esa columna, que cambiará de valor solo cuando realicemos un desplazamiento horizontal explícito. Para definir la columna del cursor, podemos teclear:

`<número>|`

Al hablar de líneas, nos referimos a líneas de archivo, que no tienen por qué coincidir con las líneas en pantalla. Cuando la línea del archivo ocupa más que el ancho de pantalla, *Vim* la divide en dos o más líneas de pantalla, dependiendo de su longitud, como ya se ha dicho anteriormente. En ese caso, el número de columna dentro de la línea podría no coincidir con la columna de pantalla en caso de líneas con anchura superior a la anchura de pantalla.

Existen varias órdenes que mueven el cursor de forma rápida. Muchas de estas acciones de movimiento aceptan argumento numérico, que se teclea antes de la acción, y que la repite o amplía el número de veces que especifiquemos:

- `0` - principio de línea.
- `^` - primer carácter no blanco de la línea. Por blancos entendemos espacios y tabuladores. Si son todos blancos, último carácter de la línea.
- `$` - último carácter de la línea.
- `w` - primer carácter de la palabra siguiente. `W` es similar, con la diferencia de que los símbolos de puntuación contiguos a la palabra se consideran también parte de la misma (solo los caracteres de espacio separan palabras).
- `e` - fin de la palabra actual. `E` es similar, con la diferencia de que los símbolos de puntuación contiguos se consideran también parte de la palabra. Si estamos ya en el último carácter de la palabra correspondiente, saltará al último carácter de la palabra siguiente.
- `b` - principio de la palabra actual. `B` es similar, con la diferencia de que los símbolos de puntuación contiguos se consideran también parte de la palabra. Si estamos ya en el primer carácter de la palabra actual, saltará al primer carácter de la palabra anterior.
- `+` - primer carácter no blanco de la línea siguiente (si solo tiene caracteres en blanco, último carácter de esta). `-` hace lo mismo pero en la línea anterior.
- `C-Fin` - se desplaza al último carácter de la última línea del archivo. Con argumento numérico, al último carácter de la línea especificada. Es similar a `G`, que no lo hace al último carácter de la línea, sino al primer carácter no blanco de la misma (si todos son blancos, se mueve al último de ellos).
- `C-Inicio` - mueve el cursor al primer carácter no blanco de la primera línea del archivo (al último si solo hay blancos). Con argumento numérico, el destino es la línea especificada, no la última. Es equivalente a `gg`.
- `%` con argumento numérico se desplaza a la linea cuyo porcentaje dentro del archivo corresponde al argumento.
- `%` sin argumento numérico, funciona distinto. Si estamos sobre un carácter delimitador (llave, paréntesis, corchete,...) desplaza el cursor al otro carácter con el que está emparejado. Si no, avanza al carácter delimitador más próximo, aunque si no existe tal carácter en algún punto tras el cursor, en la línea actual, no hace nada.
- `*` desplaza el cursor a otras apariciones de la palabra sobre la que está dicho cursor (si las hay), a través del mecanismo de búsqueda. Presionar esta tecla repetidamente desplaza el cursor cíclicamente a través de todas las apariciones de la palabra.

Las siguientes acciones actúan sobre las **líneas de pantalla**, que pueden no coincidir con las del archivo:

- `gk` - mueve el cursor una línea de pantalla hacia arriba. Acepta argumento numérico.
- `gj` - mueve una línea de pantalla hacia abajo. Acepta argumento numérico.
- `g0` - mueve al principio de la línea de pantalla actual.
- `g^` - mueve al primer carácter no blanco de la línea de pantalla actual (si son todos blancos, al último de ellos).
- `gm` - mueve al centro (en anchura) de la pantalla.
- `g$` - mueve al último carácter de la línea actual de pantalla.

Las siguientes acciones avanzan/retroceden el cursor en función del tamaño de la pantalla:

- `C-f` - avanza (*forward*) 1 pantalla (o más, con argumento numérico).
- `C-b` - retrocede (*backward*) 1 pantalla (o más, con argumento numérico).
- `C-d` - avanza (*down*) 1/2 pantalla.
- `C-u` - retrocede (*up*) 1/2 pantalla.
- `H`, `M` y `L` mueven el cursor al primer carácter no blanco de la línea que ocupa la parte superior, el centro y la parte inferior de la pantalla, respectivamente.

Otras acciones para moverse por el texto. Aceptan argumento numérico:

- `(` - Mueve el cursor al principio de la frase.
- `)` - Mueve al principio de la frase siguiente.
- `{` - Mueve el cursor al principio del párrafo.
- `}` - Mueve al principio del párrafo siguiente.

*Vim* considera que una *frase* ha terminado después de un signo de puntuación ***?***, ***.*** o ***!*** seguido de algún espacio en blanco (incluyendo espacio, tabulador y/o fin de línea). Del mismo modo, una línea en blanco (sin caracteres de ningún tipo, ni siquiera espacios), es decir, dos o más caracteres de fin de línea consecutivos, marcan el final de un *párrafo*.

Mediante `C-e` y `C-y` desplazamos el texto hacia arriba y hacia abajo, respectivamente, sin cambiar el cursor de posición en el archivo, a no ser que ese desplazamiento fuese suficiente para hacer desaparecer el cursor de pantalla. En ese caso, sí sería movido para evitar esa desaparición.

## Modo Insertar

En el modo *Insertar* se utiliza el teclado para escribir texto, y no para definir acciones. Estando en modo *Normal*, podemos pulsar `i` para pasar a modo *Insertar*. Entonces podemos teclear lo que deseemos, incluyendo finales de línea con `Intro`. También podemos borrar texto con las teclas de suprimir o retroceso. Una vez hayamos
terminado, regresaremos a modo *Normal* pulsando `Esc`.

El nuevo texto introducido quedará *antes* de la posición donde estaba el cursor al entrar en ese modo (si es que no hemos borrado el texto correspondiente a esa posición).

Podemos observar que al pulsar `Esc`, el cursor retrocede una posición, ya que así quedará sobre el último carácter
insertado. Si entramos a modo *Insertar* y salimos de él sin teclear nada, veremos que también retrocede.

Existen varias formas de entrar en modo *Insertar*, que se diferencian por la acción previa al cambio de modo:

- `i` - inserta (*insert*) texto antes del cursor. `I` se desplaza al primer carácter no blanco de la línea actual antes de cambiar de modo. Si todos los caracteres son blancos, se desplaza al último de ellos.
- `a` - añade (*append*) texto después del cursor. `A` se desplaza al último carácter de la línea antes de cambiar de modo (útil para añadir texto a final de línea).
- `c` - cambia (*change*) el texto definido por la tecla que se pulsa justo después de la `c` mediante una acción de desplazamiento del cursor como las que ya hemos visto. La secuencia borra todos los caracteres desde la posición del cursor (incluida) hasta el lugar definido (`0`, `^`, `$`, `w`, `e`, `b`, etc.). A continuación cambia a modo *Insertar*.

> Hay que añadir que aquí `w` equivale a `e` (y `W` a `E`), sin incluir nunca espacio blanco posterior a la palabra.
>
> En el caso de `c` (es decir, la secuencia `c` `c`), lo que se elimina es el texto completo de la línea actual, pero no la línea en sí (queda la línea vacía), aunque si incluimos argumento numérico, eliminará todas las líneas especificadas, y solo dejará una línea vacía. En todos los casos vistos, se puede incluir indistintamente argumento numérico justo antes del comando *change*, o justo antes de la acción de desplazamiento. Algunas acciones de desplazamiento, como `C-f`, `C-b`, `C-u` o `C-d` no tienen efecto dentro del comando *change*.

- `o` - inserta una línea justo después de la línea actual y cambia a modo *Insertar*. Con `O` se añade esa línea antes de la línea actual.
- `s` - elimina el carácter sobre el que está el cursor antes de entrar en modo *Insertar*.

Existe un modo similar a *Insertar*, con la diferencia de que al ir escribiendo, el nuevo texto va sobrescribiendo el texto antiguo. Se trata del modo *Reemplazar*, y se activa mediante `R`.

Por lo general, si aplicamos un argumento numérico a las acciones que realizan una eliminación (borrado) de texto antes de cambiar de modo (como hace la acción *change*), se repetirá esa modificación previa e inmediatamente entrará al modo *Insertar*; una vez terminada la edición de texto y salgamos de ese modo, no se repetirá ese texto insertado. El resto de acciones, las que entran en modo *Insertar* sin borrar nada, repiten la acción previa al cambio de modo (si la hay) y también el texto insertado en ese modo (en el caso del modo *Reemplazar*, sobrescribiendo el texto antiguo).

Existen algunas equivalencias a nivel de secuencias de teclas: `C` equivale a `c` `$`, `S` equivale a `c` `c`, y `c` `Espacio` equivale a `s`.

## Deshacer, rehacer y repetir

`u` deshace (*undo*) el último cambio realizado, retrocediendo una posición (o más si especificamos argumento numérico) en el historial de acciones.

En cambio, `U` realiza una nueva acción de tal modo que los últimos cambios realizados en la última línea modificada quedan revertidos. De este modo, en lugar de retroceder una posición en el historial de acciones, se añadirá una nueva acción en dicho historial que dejará la última línea modificada como estaba antes de empezar a editarla.

El historial de acciones no tiene límite. Se guardan todas las acciones desde que se inició *Vim*.

En el caso de haber retrocedido en el historial mediante `u`, podemos avanzar nuevamente una acción hacia adelante con `C-r` (rehacer). Con argumento numérico podemos deshacer o rehacer más de una acción de una vez.

Mediante `.` repetimos la última acción realizada. Si incluimos argumento numérico, se repetirá tantas veces como le indiquemos. Esta acción es la última que hemos realizado, independientemente de la posición actual del historial de acciones. Es decir, si realizamos las acciones A, B y C, y deshacemos (con `u`) las acciones C y B, la última acción realizada sigue siendo la C, no la A. La acción repetir puede ser extremadamente útil para tareas repetitivas.

Hay que tener en cuenta (tanto para deshacer/rehacer como para repetir) que las acciones que cambian a modo *Insertar* cuentan como una única acción tanto los cambios previos al cambio de modo que realizan como todos los cambios hechos dentro de ese modo.

## Algunas acciones simples

Para remplazar un solo carácter, no hace falta salir de modo *Normal*. Simplemente colocaremos el cursor sobre el carácter a sustituir, presionaremos `r` y a continuación el nuevo carácter.

La orden `~` cambia el estado de mayúsculas del carácter actual (si está en mayúsculas lo deja en minúsculas y viceversa) y avanza una posición (siempre que no estemos ya en el fin de línea). Mantenerlo pulsado resulta útil para cambiar fragmentos de línea, o líneas completas. También podemos usar argumento numérico para cambiar varios caracteres a la vez.

`J` une (*join*) la línea actual con la siguiente, eliminando los espacios iniciales de la segunda, y añadiendo un espacio de separación entre ambas en el caso de que no lo hubiera al final de la primera. Si no queremos que inserte ese espacio, usaremos `g` `J`. Para unir varias líneas a la vez, podemos utilizar argumento numérico.

Se puede ejecutar un comando del sistema operativo mediante

`:!<comando>`

Para insertar la salida de un comando del sistema después de la línea actual:

`:r !<comando>`

Resulta útil el mecanismo de autocompletado que podemos usar en modo *Comando* mediante la tecla `Tab`, el cual tiene en cuenta el contexto de lo que estamos tecleando.

## Información de estado

`C-g` proporciona información del estado actual. En principio, la información mostrada será: nombre del archivo (incluyendo la ruta si no está en el directorio actual), indicación (si procede) de archivo modificado, línea actual, número de líneas total, porcentaje de la línea actual dentro del número total de líneas, y número de columna actual.

Si la opción `ruler` esté activa, `C-g` no mostrará la línea actual ni la columna actual, dado que esa información se está visualizando ya en pantalla.

En cuanto al número de columna mostrado, en el caso de que existan tabuladores o caracteres *multibyte* a la izquierda del cursor, se verán dos números, de la forma B-C, donde B indica el número de *byte* donde está el cursor dentro de la línea de archivo actual (número de *bytes* a la izquierda del cursor, más uno) mientras que C es el número de columna sobre la que está el cursor dentro también de la línea de archivo actual (número de columnas a la izquierda del cursor, más una). Un carácter tabulador ocupa un solo *byte*, mientras que suele ocupar visualmente
varias posiciones (columnas). De forma inversa, un carácter *multibyte*, como una letra acentuada o un carácter especial, ocupa 2 o más *bytes*, mientras que solo ocupa una posición visualmente (columna). El número de *bytes* que ocupa un carácter viene determinado, por defecto, por el formato *Unicode UTF-8*. Cuando el cursor está sobre una línea vacía, sin más caracteres que el fin de línea, se puede ver 0-1, es decir, el cursor no está sobre ningún *byte* (porque no los hay), pero sin embargo sí podemos verlo en la columna 1 de pantalla.
