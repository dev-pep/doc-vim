# Cortar, copiar y pegar

Aunque en *Vim* las acciones de «corta y pega» no se llaman de este modo, funcionan de forma muy parecida, con lo que utilizaremos esta nomenclatura como referencia. En *Vim*, estas acciones reciben el nombre de *delete* (borrar, equivalente a cortar), *yank* (equivalente a copiar) y *put* (equivalente a pegar).

## Cortar (change, delete)

### Más acerca de change (c)

Ya hemos visto cómo mover el cursor por el documento. También hemos visto que combinando el comando `c` (*change*) con un movimiento del cursor, el texto comprendido entre la posición inicial del cursor y la posición final tras el movimiento quedaba eliminado del documento, y entrábamos en modo *Insertar*.

Por ejemplo, la secuencia `2w` mueve el cursor hasta el principio de la palabra siguiente a la siguiente. Si tecleamos `c2w`, lo que sucede es que todo ese tramo de texto correspondiente al movimiento descrito desaparece, justo antes de entrar en modo *Insertar*. Como hemos visto anteriormente, la secuencia `c2w` equivale a `2cw`.

Todo esto se aplica a cualquier tipo de desplazamiento.

Recordemos que `C` equivale a `c$` (cambio hasta fin de línea).

### Borrar texto con delete (d)

Si en lugar de usar el comando `c` usamos `d` (*delete*), sucede algo muy similar, con la única diferencia de que no entraremos en modo *Insertar*. Simplemente se borrará el texto.

Si tecleamos `d2w` (o `2dw`) veremos también que ese fragmento de texto es borrado. Pero esta vez seguiremos en modo *Normal*.

Nuevamente, el comando `d` funciona con cualquier acción de desplazamiento del cursor.

De forma análoga a la secuencia de cambio `cc`, si tecleamos `dd` se elimina la línea donde se halla el cursor. Con argumento numérico (que puede ir antes o después de la primera `d`) se eliminarán por completo el número de líneas especificadas, desde la actual hacia abajo. La línea actual se refiere a la línea donde se encuentra el cursor.

La orden `D` equivale a `d$` (borrar hasta fin de línea).

Otra forma de eliminar texto es mediante `x`. En este caso, se elimina únicamente el carácter bajo el cursor (y sucesivos si añadimos argumento numérico). En cuanto a `X`, elimina el carácter anterior al cursor (o más de uno con argumento numérico).

### Destino del texto borrado

El texto borrado (tanto con `c` como con `d`) no se pierde para siempre. De hecho, se guarda una copia en unos registros internos de los que hablaremos a continuación.

## Copiar (yank)

Existe una forma de colocar un fragmento de texto en los registros mencionados sin borrar el texto. Para ello, usaremos el comando `y` (*yank*). Este comando se usa exactamente del mismo modo que `d` o `c`, con los desplazamientos habituales, con sus argumentos numéricos, etc. Aunque ni borra el texto, ni cambia de modo, ni mueve el cursor. Solo copia el fragmento de texto que deseemos en los registros mencionados.

Para copiar una línea completa (o más), se usa `yy`, que equivale en este caso a `Y`.

## Pegar (put)

El texto borrado (con `d` o `c`, incluso con otras órdenes como `x` o `s`) o copiado (con `y`) puede recuperarse utilizando el comando `p` (*put*). Este comando inserta, después de la posición del cursor, una copia del último texto eliminado. En el caso de que este texto esté formado por líneas completas *sin fragmentar* (borradas con
`dd` o `cc`, o copiadas con `yy`), se insertará el texto debajo de la línea actual.

Si queremos que el texto se inserte *antes* del cursor (o *encima* de la línea actual), usaremos el comando `P`.

## Los registros de texto

Existen diez registros numerados (del 0 al 9) y un registro sin numerar (el registro «sin nombre») que almacenan los valores de los últimos borrados o *yanks* de la forma que veremos a continuación.

Cada vez que ejecutamos una acción de borrado (`d`), cambio (`c`), o *yank* (`y`), se guarda una copia del texto
especificado en el registro sin nombre, que es el que utiliza el comando *put* (`p` o `P`) por defecto para insertar texto.

Si el texto borrado (con `d` o `c`) incluye por lo menos un fin de línea, se guardará, además, otra copia en el primer registro numerado (número 1). Cuando eso sucede, el antiguo contenido del registro 1 pasa al registro 2, cuyo contenido pasa al 3, y así sucesivamente hasta el registro 9, que recibirá el contenido del registro 8, perdiéndose su contenido anterior.

Esto no se aplica al comando *yank* (`y`), que guarda siempre el texto, a parte de en el registro sin nombre, en el registro 0. Un *yank* sucesivo se seguirá guardando en el registro 0, cuyo valor antiguo se pierde. El registro 0, pues, almacena única y exclusivamente el último *yank*.

Por otro lado, mientras el registro 1 guarda el último *delete* o *change*, el registro 2 guarda el penúltimo; el 3 el antepenúltimo; etc. Recordemos que en este caso nos referimos solo a textos que incluyan algún fin de línea. Y dentro de los borrados y cambios incluimos todas las acciones `d`, `D`, `x`, `X`, `c`, `C`, `s` y `S`.

Si queremos insertar en nuestro archivo el texto que hay en el registro sin nombre, simplemente teclearemos `p`. Pero también podemos insertar el texto de un registro numerado (suponiendo que tenga contenido): tecleando `"<n>p` insertaremos el contenido del registro número \<n\>. Para insertar, por ejemplo, el penúltimo texto borrado, teclearemos `"2p`. Si lo queremos insertar 5 veces, teclearemos `5"2p`. Como de costumbre, el texto queda insertado después del cursor (o debajo de la línea actual) si usamos `p`. Con `P` se inserta antes (o encima).

Si tras insertar el contenido de un registro numerado pulsamos `.` (repetir), se insertará el contenido (si lo hay) del siguiente registro numerado.

Por ejemplo, la secuencia `"3p..u...u...` inserta primero el contenido del registro 3 (`"3p`), luego el del 4 (`.`), el del 5 (`.`), que deshace (`u`), luego el 6, 7 y 8 (`...`); deshace el 8 (`u`); finalmente inserta tres veces el 9
(`...`), ya que no se incrementa el número de registro cuando se ha llegado al 9. Por lo tanto, quedará insertado el contenido de los registros 3, 4, 6, 7, 9, 9, 9.

Adicionalmente existen otros 26 registros asociados a cada letra del alfabeto (de la 'a' a la 'z'). Si deseamos copiar un texto (mediante `y`, `d` o `c`) a uno de esos registros, lo haremos tecleando `"<c>` justo antes de la acción *yank*, *delete* o *change*.

Por ejemplo, para copiar desde el cursor hasta fin de línea, más las dos líneas siguientes, en el registro 'k', teclearemos `"ky3$`.

Es importante mencionar que en el caso de que copiemos texto en uno de los registros con nombre, ese texto entrará también en el sistema de registros numerados, independientemente de si contiene un fin de línea o no.

Para insertar en nuestro archivo el texto de un registro con nombre, se hace prefijando `"<c>` al comando *put*, donde \<c\> es la letra que identifica el registro deseado. Por ejemplo, si queremos insertar el contenido del registro 'k', teclearemos `"kp`. Como siempre, podemos utilizar un argumento numérico para insertar ese contenido varias veces. Por ejemplo, para insertar 5 veces el contenido del registro 'k', podemos hacer `5"kp`, o `"k5p`.

Si especificamos la letra del registro en mayúscula, el contenido del registro no será remplazado por el nuevo texto, sino que será añadido al final del mismo.

Existen algunos registros especiales, como el asociado al carácter ':', que guarda el último comando introducido en modo *Comando*, el registro '/' que guarda la última búsqueda realizada, o el registro '+', que veremos más abajo.

El comando `:reg` nos muestra el contenido de todos los registros, tanto los especiales como los numerados, los registros con nombre o el registro sin nombre (asociado al carácter dobles comillas, ***"***). Si deseamos ver el contenido de un registro o registros concretos podemos especificarlo también. Por ejemplo:

`:reg a`

nos mostrará el contenido del registro 'a', mientras que mediante

`:reg 12c`

podremos ver el contenido de los registros 1,2 y c.

Existe una forma de insertar texto de uno de los registros estando en modo *Insertar*, sin necesidad de salir de ese modo. Esto se consigue simplemente pulsando `C-r` y a continuación el carácter que define el registro deseado. Esto nos evitará tener que regresar momentáneamente a modo *Normal* cada vez que deseemos insertar el contenido de un
registro.

### Acceder al portapapeles (clipboard)

Para utilizar el portapapeles del sistema (el *clipboard*) y poder transferir texto entre otras aplicaciones y *Vim*, existe un registro especial llamado *quoteplus*, asociado al signo '+'. Este registro es una referencia al portapapeles. Tecleando por ejemplo `"+yy` copiamos la línea actual al *clipboard* (además de al registro sin nombre, naturalmente). Después podríamos ya *pegar* ese texto en cualquier otra aplicación. Del mismo modo, cualquier texto *copiado* o *cortado* al portapapeles desde cualquier otra aplicación, podría ser insertado en nuestro archivo en *Vim* tecleando `"+p`.

Es importante observar que no todas las versiones de *Vim* están compiladas con acceso al portapapeles. Para saber si es nuestro caso, ejecutaremos

`vim --version`

desde la línea de comandos, y buscaremos las entradas 'clipboard' y 'xterm_clipboard'. Si aparece un signo '+' delante de cualquiera de estos módulos, tenemos acceso al portapapeles. De lo contrario, no disponemos de esta funcionalidad.

En caso de no tener acceso, una solución suele ser instalar *gVim*, que es la versión de *Vim* con interfaz gráfica. No significa que debamos usar esta nueva aplicación; simplemente, instalarla añadirá la funcionalidad que buscamos a nuestro *Vim* de modo texto.

## Manipulación de texto en modo Comando

En modo comando existen herramientas muy útiles para borrar, copiar y mover texto.

En esta sección nos resultará útil la opción `number`, que nos muestra los números de línea. Para activarla:

`:set number`

Podemos desplazarnos rápidamente a una línea concreta introduciendo el número de línea:

`:45`

mueve el cursor a la línea 45, concretamente al primer carácter no blanco de esa línea. Si la línea solo tiene caracteres blancos, se desplaza al último de ellos.

En modo comando también podemos utilizar los comandos *delete* (`d`), *change* (`c`) y *yank* (`y`).

Para borrar una línea concreta:

`:45d`

elimina la línea 45 y el cursor se queda sobre la nueva línea 45.

En lugar de una simple línea, podemos definir un *rango de líneas*. Dos números separados por una coma definen el rango comprendido entre las dos líneas especificadas, ambas incluidas.

`:3,15d`

elimina las líneas 3 a 15, ambas incluidas. También añade el texto eliminado a los registros correspondientes (sin nombre y numerados).

`:3,15y`

copia las líneas 3 a 15 en el registro sin nombre (y en el registro 0).

`:3,15c`

esperará que introduzcamos una o varias líneas. Una vez terminado, pulsaremos `Esc`. Las líneas introducidas sustituirán a las líneas 3 a 15. Si la última línea tecleada no termina en fin de línea (`Intro`), será descartada. En este caso, el texto eliminado no se copia a ningún registro.

A la hora de definir números de línea, existen algunos caracteres especiales:

- `.` se refiere a la línea actual.
- `$` es la última línea del archivo.
- `%` es un rango equivalente a `1,$` es decir, todas las líneas del archivo.
- Se puede sumar o restar cualquier número a una posición específica. Por ejemplo, `.+5` se refiere a 5 líneas más allá de la línea actual. `$-2` es la antepenúltima línea del archivo. Si no se especifica la posición de referencia, se entiende la línea actual:

    `+3` o `-5` equivalen respectivamente a `.+3` y `.-5`. Si no se especifica número, se entiende 1, es decir, `+` equivale a `.+1` (línea siguiente a la actual) y `$-` es la penúltima línea. Se pueden repetir los signos: `.+++`, `$--`, o incluso `20----` (línea 16).

Veamos algunos ejemplos:

`:-5`

Desplaza el cursor 5 líneas hacia arriba.

`:%d`

borra todas las líneas del archivo.

`:d`

borra la línea actual.

`:c`

cambia la línea actual. Debemos introducir el nuevo texto (terminado siempre en `Intro`), y finalmente `Esc`.

`:y`

copia (*yank*) la línea actual.

`:.,\$d`

elimina todas las líneas desde la actual hasta la última del archivo.

`:.,.+20d`

borra la línea actual y las siguientes 20.

`:,++d`

borra la línea actual y las dos siguientes.

El comando `m` se utiliza para mover texto:

`:3,15m67`

mueve las líneas 3 a 15 *a continuación* de la línea 67. Si queremos mover al principio del archivo (antes de la línea 1), debemos indicar línea 0:

`:3,15m0`

Para mover un fragmento de texto al final de todo:

`:20,.m$`

mueve las líneas 20 hasta la actual al final del archivo.

`:25,$m.-1`

mueve las líneas de la 25 hasta la última a continuación de la línea que está antes de la línea actual, es decir, justo antes de la línea actual.

`:m$`

mueve la línea actual al final del archivo.

El texto movido no es copiado a ningún registro.

## Otras utilidades en modo Comando

En modo *Comando* podemos aprovechar el historial de comandos para reutilizar los que hemos tecleado con anterioridad. A este mecanismo se accede mediante las flechas arriba y abajo cuando estamos en dicho modo y no hay nada tecleado a la derecha de los dos puntos (`:`).

`:=`

muestra el número total de líneas del archivo.

`:.=`

muestra el número de línea actual.

Para guardar un fragmento del archivo a disco:

`:.,$w`

guardaría desde la línea actual hasta la última, siempre y cuando el archivo que estamos editando ya tenga nombre. Sin embargo, la operación *no se ejecutará*, ya que eso implicaría la pérdida de texto completo que está en disco (en este caso, se perderían las líneas anteriores a la actual). Para que un guardado parcial se ejecute, necesitamos incluir el signo de exclamación:

`:.,$w!`

En cambio, si le damos un nombre de archivo distinto al nombre actual, nos permitirá realizar el guardado parcial sin el símbolo `!`, a no ser que el nuevo nombre ya exista, en cuyo caso, sí deberemos incluirlo:

`:5,25w <archivo>`

guarda las líneas 5 a 25 (incluidas) en el archivo con el nombre especificado. Como siempre, el nombre puede ser un simple nombre de fichero, o incluir una ruta relativa o absoluta. Un guardado parcial no cambia el nombre del archivo actual, ni siquiera aunque este no tenga nombre.

Si el nombre empieza por los caracteres `>>`, el texto se añadirá al final del archivo en disco.

En el modo *Comando* se pueden combinar dos o más comandos en una misma línea utilizando el carácter `|` entre ellos:

`: <comando1> | <comando2> | ...`

En este caso conviene apuntar que así como el primero de los comandos de la línea necesita incluir los dos puntos iniciales (`:`), en los sucesivos comandos no es necesario.

Podemos insertar un archivo completo a continuación de la línea actual mediante el comando `r`:

`:r <archivo>`

incluye el contenido íntegro del archivo especificado bajo la línea actual.

`:$r <archivo>`

añade el contenido de ese archivo al final. Si queremos que lo añada al principio:

`:0r <archivo>`

Para insertarlo en una línea específica:

`:130r <archivo>`

Lo inserta tras la línea 130.
