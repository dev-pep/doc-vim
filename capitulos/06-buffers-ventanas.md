# Buffers y ventanas

Cuando abrimos *Vim*, podemos observar que el archivo a editar ocupa todo el espacio del editor. Es decir, tenemos *una sola ventana* en pantalla. Pero podemos dividir ese espacio en varias ventanas.

Por otro lado, en *Vim* podemos tener uno o más archivos abiertos. Dichos archivos, tanto si están guardados en disco como si tienen cambios pendientes de guardar, o ni siquiera existen en disco todavía, forman los llamados *buffers*. Podemos entender los *buffers* como el texto en memoria correspondiente al estado actual de cada archivo abierto. Veámoslo con más detalle.

## Buffers

Cada ventana muestra (está asociada a) uno de estos *buffers*. Dos ventanas distintas pueden mostrar un *buffer* distinto cada una, o mostrar ambas el mismo *buffer*. En este último caso, veremos cómo los cambios hechos en una de ellas se reflejan inmediatamente en la otra.

Como vimos anteriormente, con el comando `:e` podíamos abrir un archivo concreto de disco en la ventana actual. En este caso, si el archivo ya está abierto en *Vim* (en otra ventana), es decir, si se corresponde con uno de los *buffers* ya existentes, esa ventana se asociará (mostrará) a ese *buffer* concreto. De lo contrario, un nuevo *buffer* será creado y pasará a estar asociado a la ventana.

En el caso de usar `:e` especificando un nombre de archivo que no existe en el disco, el contenido del *buffer* permanece únicamente en la memoria interna (RAM) hasta que se guarda a disco (con `:w`).

Los *buffers* son *activos* si están asociados a alguna de las ventanas existentes. De lo contrario, son *buffers* ocultos, o simples referencias a archivos en disco.

Para ver la *lista de buffers* que *Vim* guarda en memoria, usaremos el comando `:ls` (o `:files`, o `:buffers`, que son comandos sinónimos). Esto nos mostrará dicha lista, con el número de cada uno, y algunas indicaciones: la letra \'a\' indica *buffer* activo. Si está oculto (no lo muestra ninguna ventana actualmente), tendrá la letra \'h\' (*hidden*) en su lugar. Si no existe letra, significa que no está en memoria, es decir, es una simple referencia a un archivo en disco. Por otro lado, el símbolo \'%\' indica que es el *buffer* de la ventana actual. Se le podría denominar el «*buffer* actual». El símbolo \'\#\' indica *buffer* alternativo, que normalmente coincide con el anterior *buffer* actual (el que tenía el símbolo \'%\' antes que el actual). El símbolo \'+\' indica que existen cambios en el *buffer* sin guardar en disco.

El concepto de *buffer* alternativo es útil para alternar el contenido de una ventana continuamente entre dos *buffers*. Es rápido pasar al *buffer* alternativo con

`:e #`

En ese caso, el alternativo pasa a ser el *buffer* actual, y viceversa.

Para que el contenido de la ventana actual muestre otro *buffer* distinto al que está mostrando, a parte de abrir un archivo distinto, nos podemos mover por la lista de *buffers* con estos comandos:

- `:buffer <N>` cambia al *buffer* número \<N\>.
- `:bn` cambia al siguiente *buffer* de la lista, de forma cíclica.
- `:bN` cambia al anterior, de forma cíclica.
- `:bf` cambia al primer *buffer* de la lista.
- `:bl` cambia al último.

A parte de la lista de *buffers*, existe otra lista, subconjunto de esta, que incluye los que podríamos denominar *buffers* argumento. Se trata de una lista con los *buffers* correspondientes a los archivos que se especificaron en la línea de comandos al invocar *Vim*. Por ejemplo, si invocamos el editor así:

`vim file1.txt file2.txt`

y posteriormente abrimos (por ejemplo con `:e`) los archivos 'file3.txt' y 'file4.txt', tendremos una lista de *buffers* con cuatro elementos ('file1.txt', 'file2.txt', 'file3.txt' y 'file4.txt'), mientras que tendremos una lista de *buffers* argumento con dos elementos ('file1.txt' y 'file2.txt'). Para que una ventana pase a mostrar el contenido de un *buffer* argumento, tenemos los siguientes comandos:

- `:argument <N>` cambia al *buffer* argumento con número \<N\>.
- `:n` cambia al siguiente *buffer* argumento de la lista.
- `:N` cambia al anterior *buffer* argumento.
- `:first` cambia al primer *buffer* argumento de la lista.
- `:last` cambia al último de la lista.

Hay que tener en cuenta que cada una de las dos listas tiene su propio apuntador independiente, y que la lista de *buffers* argumento no es circular.

Los comandos que avanzan o retroceden en las listas pueden utilizarse con argumento numérico para saltar varias posiciones de golpe (`:4n` o `:8bn`).

Se puede eliminar un *buffer* utilizando el comando `:bd` y a continuación el número de *buffer* que deseamos eliminar. Todas las ventanas asociadas a ese *buffer*, si las hay, se cierran.

## Ventanas

Veamos ahora cómo podemos abrir más de una ventana en el área del editor para poder visualizar más de un archivo a la vez.

Al abrir *Vim*, el espacio completo del editor es ocupado por una única ventana. Incluso si hemos abierto el editor con varios argumentos (varios archivos), solo uno de ellos es visible inicialmente en esta ventana. Se muestra únicamente el contenido de un solo *buffer*. El resto de *buffers*, si los hay, serán simples referencias a los correspondientes archivos en disco.

Para la gestión de ventanas se utilizan secuencias de teclado en modo *Normal* que se inician con `C-w` y requieren una segunda pulsación. Cuando esta corresponde a una letra minúscula, y solo en ese caso, se puede pulsar esa segunda tecla junto con la tecla Control, para ganar velocidad. Por ejemplo, `C-w j` equivale a `C-w C-j`. Pero `C-w J` no equivale a `C-w C-J`.

### Creación de ventanas

Para subdividir la ventana actual horizontalmente en dos (una encima de otra), de tal modo que las dos muestren el mismo *buffer*, podemos utilizar el siguiente comando:

`:split`

o pulsar `C-w s`.

Cuando hablamos de *ventana actual* nos referimos a la ventana donde está el cursor.

Si lo que queremos es subdividir la ventana verticalmente lo haremos así:

`:vsplit`

o mediante `C-w v`.

Si al hacer la subdivisión especificamos el nombre de un archivo, la nueva ventana mostrará dicho archivo. Si este se corresponde con uno de los *buffers* existentes, la nueva ventana se asociará a ese *buffer*. De lo contrario, se creará un nuevo *buffer* para ese archivo, asociado a la nueva ventana. Todo esto se aplica tanto a subdivisiones horizontales como verticales:

`:split \<archivo\>`

`:vsplit \<archivo\>`

Si lo que queremos es que la nueva ventana muestre un archivo nuevo (vacío y sin guardar en disco), lo haremos entrando

`:new`

o mediante `C-w n`.

Lo anterior realiza una subdivisión horizontal. Si la queremos vertical:

`:vnew`

Cualquiera de las acciones que crean nuevas ventanas acepta argumento numérico, el cual indica el tamaño (anchura o altura, según el tipo de subdivisión) de la ventana nueva. En el caso de subdividir en modo *Comando*, se puede incluir ese argumento tras los dos puntos, justo antes del comando en sí.

Una de las utilidades de tener varios archivo abiertos simultáneamente, es que se puede copiar texto de uno a otro de forma sencilla utilizando los comandos *yank*, *delete* y *put*, y los registros de *Vim*.

### Cierre de ventanas

Podemos cerrar la ventana actual mediante `:q` o con la acción `C-w q`. En caso de que dicha ventana sea la última referencia a un *buffer* con cambios pendientes de guardar, deberemos confirmar explícitamente con `:q!` que asumimos la pérdida de dichos cambios. Cuando cerramos la última referencia a un *buffer* activo, el *buffer* deja de estar en memoria (pasa a ser una referencia al archivo en disco).

Pero esto no es así cuando la opción `hidden` está activa. En ese caso, los *buffers* no desaparecen de memoria cuando se cierra la última referencia a ellos, sino que quedan ocultos, permaneciendo en memoria RAM. Por lo tanto, en ese caso, siempre será posible cerrar ventanas sin especificar '!', exceptuando, en todo caso, la última ventana existente, ya que al cerrar *Vim* se pierden todos los cambios.

Podemos utilizar también el comando `:hide` para cerrar la ventana en lugar de `:q`. Este comando funciona del mismo modo que `:q`, con la diferencia de que en el caso de que estemos cerrando la última referencia a un *buffer* concreto, ocultará ese *buffer* sin descargarlo de memoria, *independientemente del estado de la opción `hidden`. No se puede utilizar este comando si solo existe una ventana abierta en *Vim*.

También se puede cerrar la ventana actual mediante `C-w c`. Esta acción funciona exactamente igual que `C-w q`, con la diferencia de que no se puede cerrar *Vim* así. Es decir, si solo existe una ventana abierta en el editor, no la cerrará.

La acción `C-w o` cierra todas las ventanas en pantalla excepto la actual.

Para cerrar todas las ventanas del editor, es decir, para cerrar *Vim* en una sola acción, independientemente del número de ventanas existente, usaremos `:qa` (o `:qa!` si existen cambios sin guardar en algún *buffer*).

Para guardar los cambios de todos los *buffers* *que tengan cambios no guardados*, lo haremos con `:wa`.

El comando `:wqa` (o su equivalente `:xa`) combina `:wa` y `:qa`.

### Cambio de ventana

Existen varias acciones que establecen la ventana actual. Suelen aceptar argumento numérico, cuyo efecto es repetir la acción un determinado número de veces.

De forma análoga a las acciones `h`, `j`, `k`, `l` de movimiento del cursor, tenemos las acciones `C-w h`, `C-w j`, `C-w k` y `C-w l` para establecer como nueva ventana actual a la ventana inmediatamente a la izquierda, debajo, encima o a la derecha de la actual, respectivamente (si procede).

Con `C-w w` nos desplazamos a siguiente ventana (si la hay). El concepto de «siguiente ventana» se refiere a la ventana inmediatamente a la derecha o debajo de la actual, dependiendo de cómo se haya producido la subdivisión. Aplicando esta acción continuadamente, el cursor pasará por *todas las ventanas visibles* de la pantalla, de forma cíclica. `C-w W` hace lo mismo en sentido contrario (izquierda o encima).

`C-t` cambia a la ventana superior izquierda de la pantalla, mientras que `C-b` nos lleva a la ventana inferior derecha. Con `C-p` vamos a la ventana previa, es decir la que estaba activa antes que la actual.

Si la ventana actual ha sido creada a base de subdividir (en dos o más) una ventana que inicialmente ocupaba todo el ancho o alto de la pantalla, podemos utilizar estas acciones:

- `C-w r` avanza una posición, hacia la derecha o hacia abajo, dependiendo de si la ventana inicial antes de la subdivisión ocupaba todo el ancho o todo el alto de la pantalla. El avance se realiza de forma cíclica.
- `C-w R` hace lo mismo en sentido contrario, es decir, retrocede hacia la izquierda o hacia arriba, también de forma circular.

Por ejemplo, si una ventana que ocupaba todo el ancho de la pantalla queda subdividida verticalmente (`:vsplit`) en dos o más ventanas, podremos movernos por estas ventanas con estos comandos hacia la derecha e izquierda. Análogamente, si una ventana que ocupaba todo el alto de la pantalla queda subdividida horizontalmente (`:split`) en dos o más ventanas, podremos movernos por estas con estos comandos hacia arriba y abajo.

### Movimiento de ventanas

`C-w x` intercambia el contenido de la ventana actual con el de la siguiente ventana (la de su derecha o abajo, en función del tipo de subdivisión), si existe. En caso de no existir, lo hará con la anterior (la de la izquierda o arriba). Con argumento numérico, el intercambio no es con la inmediatamente siguiente o anterior, sino que saltará el número de posiciones indicadas, si tal ventana existe.

En todo caso, el cursor queda en la ventana que contiene el *buffer* que estábamos editando, es decir, en la nueva posición.

`C-w` con `H`, `J`, `K`, `L` mueve la ventana a la izquierda del todo, abajo del todo, arriba del todo o a la derecha del todo respectivamente, modificando su tamaño para que ocupe todo el ancho (si la mueve arriba o abajo) o todo el alto (si la mueve a la derecha o izquierda) de la pantalla.

`C-w =` ajusta el tamaño de todas las ventanas en pantalla para que tengan el tamaño más uniforme posible.

Existe una serie de acciones que permiten aumentar o disminuir el ancho o alto de la ventana actual en una fila o columna, siempre y cuando sea posible:

`C-w +` aumenta la altura una fila.

`C-w -` disminuye la altura una fila.

`C-w \<` disminuye la anchura una columna.

`C-w \>` aumenta la anchura una columna.

Especificando argumento numérico, la variación de tamaño no será uno, sino lo que indiquemos.

## Pestañas (tabs)

Las pestañas (*tabs*) son disposiciones (*layouts*) de ventanas en pantalla. Puede haber más de una pestaña, cada cual con su disposición de ventanas propia. Cada pestaña ocupa la pantalla entera, y no puede verse más que una pestaña a la vez. Se puede pasar de una a otra con facilidad.

El comando `:tabnew` abre una nueva pestaña que contiene una sola ventana con un archivo nuevo. Si queremos que la pestaña nueva tenga un archivo abierto, debemos especificar el nombre del archivo al abrirla:

`:tabnew \<archivo\>`

Cuando existen múltiples pestañas, podemos ver la lista de las mismas arriba del todo de la pantalla. La pestaña que aparece resaltada es la actual. En esa línea indicadora de pestañas, podemos ver el nombre del archivo activo de cada pestaña. En su caso podremos ver también el signo \'+\' si alguna de las ventanas que contiene tiene cambios sin guardar en disco. También puede verse el número de ventanas que tiene cada pestaña, en caso de contener más de una.

Para cerrar la pestaña actual, teclearemos

`:tabclose`

Si queremos cerrar todas las pestañas excepto la actual:

`:tabonly`

Si el cierre de pestañas que intentamos realizar va a cerrar todas las referencias a un *buffer* con cambios sin guardar, no nos permitirá cerrar esas ventanas, que seguirán abiertas (cerrará solo las que pueda). Para remediarlo, podemos guardar los cambios, forzar la pérdida de cambios (`:tabclose!`), o usar la opción `hidden` como hemos visto anteriormente.

Para cambiar de pestaña usaremos `C-PgUp` (Control con página arriba), que cambiará a la pestaña de la izquierda, o `C-PgDn` (Control con página abajo) para cambiar a la de la derecha.

Si estamos en una pestaña con más de una ventana, mediante `C-w T` movemos la ventana actual a una nueva pestaña. De lo contrario, no hace nada.
