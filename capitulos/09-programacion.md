# Programación

*Vim* tiene numerosas opciones y utilidades para la edición de código fuente. Sin ser un entorno de desarrollo integrado (*IDE*) en sí mismo, agrupa las funcionalidades más útiles para el desarrollo de programas.

## Numeración y resaltado

Cuando editamos código fuente, resulta muy útil tener visualmente presente la información acerca de la línea y columna en la que se halla el cursor. Para ello disponemos de la posibilidad de activar los números de línea mediante

`:set number`

Existe también la modalidad de numeración relativa, la cual, en lugar de mostrar el número de cada una de las líneas en pantalla, muestra la distancia a la línea actual, y solo en esta, el número de línea. Se activa mediante

`:ser relativenumber`

La utilidad de este modo de numeración reside en el hecho de que en *Vim* se utiliza con mucha frecuencia el argumento numérico para definir rangos de líneas, y esta modalidad permite ver fácilmente la cantidad de líneas que componen los rangos que necesitamos definir.

Si `relativenumber` está activado y `number` no lo está, la línea actual mostrará también esa distancia. En este caso, la distancia consigo misma es 0.

Por otro lado, como vimos con anterioridad, también podemos ver el número de columna (y *byte*) en la línea de estado, utilizando

`:set ruler`

La información mostrada por esta opción es configurable. Para más información, puede consultarse la ayuda de *Vim*:

`:h 'ruler'`

En cuanto al resaltado sintáctico, es una utilidad que permite aumentar la legibilidad del código de forma dramática. Este mecanismo asigna un color distinto a los diferentes elementos del código (literales, palabras clave, comentarios, etc.).

Para activar el resaltado podemos entrar:

`:syntax on`

y para desactivarlo

`:syntax off`

Por otro lado, *Vim* utiliza un esquema sintáctico u otro dependiendo del contenido de la opción `syntax`.

Sin embargo, por lo general no suele ser necesario cambiar estas opciones ni activar el resaltado sintáctico, ya que *Vim* lo hace automáticamente según el tipo de archivo. El editor reconoce los lenguajes de programación más utilizados. Algunos valores posibles de la opción `syntax` son `basic`, `c`, `html`, `make`, `php4`, `python`, `sh`, `sql` o `xml`, entre muchos otros.

## Márgenes, tabulación e indentación

La indentación se refiere a la sangría izquierda de cada línea de texto, algo muy utilizado en programación para facilitar la legibilidad del código. La indentación se consigue añadiendo espacio en blanco (espacios y tabuladores) al principio de la línea.

En el otro lado, el margen derecho se define mediante la inserción de caracteres de fin de línea.

### Margen y ajuste de texto

Cuando vimos la diferencia entre líneas de pantalla y líneas de archivo, explicamos lo que sucedía cuando *Vim* encontraba una línea de archivo con una longitud superior a la anchura de pantalla: la mostraba utilizando varias líneas de pantalla.

Existe otro modo de visualizar estas líneas que consigue que cada línea de archivo se corresponda exactamente con una línea en pantalla. Esta modalidad evita que las líneas de archivo sean fragmentadas. Esto se consigue desactivando la opción `wrap`, activada por defecto:

`:set nowrap`

De este modo, las líneas de archivo con una longitud mayor a la anchura de pantalla se muestran parcialmente, aunque sigue siendo posible desplazarse a cualquier punto de estas, ya que *Vim* desplaza el texto horizontalmente de forma adecuada cuando es necesario para hacer posible esa visualización.

De todas formas, no es muy aconsejable en programación tener líneas tan largas, ya que afectan negativamente a la legibilidad del código.

Para tener un margen a la derecha del texto, evitando así la creación de líneas demasiado largas, debemos insertar finales de línea adecuadamente. Existe un modo de hacerlo de forma automática, definiendo la anchura máxima de texto con la opción `textwidth`, a la que debemos asignar el número máximo de columnas que vamos a permitir:

:set textwidth=50

establece una anchura máxima de 50 columnas de pantalla. A medida que escribamos, *Vim* insertará un fin de línea cada vez que tecleemos un carácter no blanco más allá del umbral definido. El fin de línea se inserta entre palabras para que no quede ninguna palabra partida, si es posible. Los caracteres blancos justo antes y después del fin de línea creado son eliminados.

En el caso de que esta opción esté desactivada (valor 0), podemos utilizar la opción `wrapmargin`. Funciona de forma similar, aunque define la anchura mínima que tendrá el margen derecho del archivo en la pantalla actual. Este margen se mantiene constante, con lo que la anchura máxima irá cambiando dinámicamente cuando la anchura de pantalla se vea afectada de algún modo (cambiando la anchura de la ventana, añadiendo o quitando columnas de información, como la numeración de líneas, etc.). De todas formas, estos cambios dinámicos solo afectan al texto que se va escribiendo, no al que está ya escrito.

Existe un mecanismo automático en *Vim* que resulta muy útil a la hora de redactar comentarios. Si hemos definido una anchura o margen derecho con `textwidth` o `wrapmargin` y escribimos un comentario en el código, precedido por sus correspondientes caracteres de comentario, como los '//' usados en *C*, o el '#' usado en *Python*, al insertarse automáticamente un salto de línea, *Vim* iniciará la siguiente línea con la indentación adecuada y la secuencia de caracteres iniciales pertinente, de tal modo que solo deberemos preocuparnos por la redacción del comentario. También funciona, por ejemplo, con comentarios multilínea de lenguaje *C* encerrados entre '/*...*/', aunque en este caso, si el comentario tiene algún tipo de indentación, será necesario tener activado un mecanismo de indentación automática como los que veremos más adelante para que quede visualmente correcto.

### Tabulación

El mecanismo de tabulación consiste en dividir la anchura de la pantalla en una serie de puntos (*tab stops*) o columnas específicas para alinear elementos de texto verticalmente.

Las explicaciones sobre la inserción y borrado de tabuladores con las teclas `Tab` y retroceso deben entenderse siempre realizadas en modo *Insertar* (o *Reemplazar*).

Al presionar la tecla `Tab`, se insertará un carácter tabulador en el texto, cuya función es desplazar el cursor al siguiente *tab stop*. Hay que recalcar que aunque esto haga avanzar el cursor varias posiciones, el carácter tabulador insertado es *un solo carácter*.

Para definir la posición de estos puntos de tabulación, disponemos de la opción `tabstop`, cuyo valor por defecto es 8. De esta manera, podemos saber qué columnas forman los *tab stops*: si partimos de la primera columna de la pantalla (la 1) y le vamos sumando el valor de `tabstop`, nos aparecen puntos de tabulación en las columnas 9, 17, 25, 33, etc. Al insertar un carácter tabulador al escribir, aparecerá un espacio en blanco más o menos ancho que llevará el cursor hasta el siguiente *tab stop*.

Si cambiamos el valor de esta opción mientras visualizamos un archivo que contenga ya tabuladores, veremos que se producen cambios visibles en estos espacios, ya que los puntos de tabulación cambiarán sus posiciones.

Existe otra opción interesante, `expandtab`, desactivada por defecto. Si la activamos, al pulsar `Tab` no se insertará ningún carácter tabulador, sino una secuencia de espacios que adelantarán el cursor hasta el siguiente punto de tabulación.

Modificar esta opción no afecta a los caracteres tabulador ni a los espacios presentes ya en el archivo con anterioridad, sino únicamente a los tabuladores que se insertarán posteriormente.

Cuando `expandtab` no está activado, `Tab` inserta un tabulador; si a continuación pulsamos la tecla de retroceso, ese tabulador será eliminado. En cambio, con `expandtab` activado, `Tab` insertará una secuencia de espacios; si seguidamente pulsamos la tecla de retroceso, únicamente el último espacio quedará borrado. Este comportamiento extraño es el que intenta corregir la opción `softtabstop`.

Por defecto, `softtabstop` tiene valor 0, es decir, está desactivada. Si le damos un valor positivo, creará unos puntos de tabulación lógicos, que podemos llamar *soft tab stops*, y pueden tener un tamaño distinto a los *tab stops* reales. Estos nuevos puntos serán los que se tendrán en cuenta al pulsar la tecla `Tab` o la tecla de retroceso. Para que el cursor quede en el *soft tab stop* correspondiente, *Vim* reorganizará cada vez el espacio en blanco, utilizando caracteres de tabulación (donde pueda) y espacios. De este modo, al pulsar retroceso y `Tab`, tendremos la sensación de que el tamaño de los puntos de tabulación es el definido en `softtabstop`. Sin embargo, si movemos el cursor por el espacio en blanco, observaremos que los espacios en blanco que va dejando `<Tab>` no se corresponden exactamente con un carácter tabulador.

El *tamaño real del carácter tabulador* seguirá siendo el que marca `tabstop`, pero el *comportamiento de las teclas `Tab` y retroceso* será como si el tamaño del tabulador fuese el definido en `softtabstop`.

Podemos verlo más fácilmente experimentando con este mecanismo mientras está activa la opción `list`, que muestra los caracteres no imprimibles en pantalla, incluyendo los caracteres tabulador.

Si `softtabstop` tiene el mismo valor que `tabstop`, su efecto será imperceptible, y será como si estuviese desactivado, a no ser que `expandtab` esté a su vez activado. En ese caso, no se insertarán caracteres tabulador sino espacios, pero al borrar espacio en blanco pulsando retroceso, se borrará no solo el último espacio, sino el número de espacios que marca `softtabstop` (y `tabstop`).

Cuando `expandtab` y `softtabstop` están activados, el valor de `tabstop` es irrelevante.

### Indentación

La indentación se refiere al espacio en blanco que precede a cada línea de código. Esta sangría izquierda es indispensable para asegurar la legibilidad del código fuente. Cuanto más espacio hay a la izquierda de una línea, más indentada se dice que está esa línea. Podemos ver la indentación como espacio de tabulación al principio de línea, pero tomando como referencia los puntos de indentación, que pueden tener un tamaño distinto a los puntos de tabulación, y están definidos en la opción `shiftwidth`, cuyo valor por defecto es 8.

Se dice que cada línea tiene un nivel de indentación que se corresponde con la cantidad de puntos de indentación que ocupa su espacio inicial. En modo *Normal*, podemos utilizar `>` para añadir un nivel de indentación a una línea o conjunto de líneas. Añadir un nivel implica aumentar el espacio blanco inicial de la línea en tantas columnas como indique `shiftwidth`. Del mismo modo, el comando `<` reduce un nivel de indentación.

Existen varias formas de utilizar estas acciones. Una forma es entrando en modo *Visual* y seleccionando un fragmento de texto. Al pulsar `>` o `<`, la indentación de todas las líneas seleccionadas será debidamente cambiada. Las líneas afectadas serán todas las comprendidas entre la que contiene la posición inicial del cursor y la que contiene la final (ambas incluidas).

Otra forma es pulsando la acción deseada combinada con con cualquier acción de movimiento del cursor, o un objeto de texto.

Si queremos modificar la indentación de una sola línea, nos colocaremos en ella y pulsaremos `>>` para aumentar un nivel de indentación o `<<` para reducirlo.

Cualquier operación de cambio de indentación, así como la inserción de indentación automática (que veremos a continuación), reorganizan la sangría inicial en tabuladores y espacios según la configuración de las opciones `tabstop` y/o `expandtab`, igual que hacían las operaciones de inserción y borrado de *soft tabs*.

Existe una opción, `smarttab`, que marca el funcionamiento de las teclas `Tab` y retroceso. Cuando la pulsación se produce tras la sangría inicial, dentro del texto posterior, el funcionamiento de estas teclas es el habitual según las opciones de tabulación. Sin embargo, cuando la inserción o borrado se produce al principio de línea o dentro del espacio de indentación de la línea, estas teclas funcionan según los puntos de indentación definidos en `shiftwidth`. Tras cada cambio en la indentación, ese espacio es reorganizado en tabuladores y espacios como sucedía con `softtabstop`.

### Autoindentación

*Vim* puede encargarse de indentar automáticamente las líneas nuevas que se van creando. Existen varios mecanismos que lo consiguen, según nuestra preferencia.

El más simple de los mecanismos de autoindentación se consigue activando la opción `autoindent`. En este caso, se copia siempre la indentación de la línea anterior al crearse una línea nueva, sin más.

El siguiente mecanismo es `smartindent`. Activando esta opción, el estado de `autoindent` es ignorado. En este caso, se hace una gestión algo más activa de la indentación, basándose en la sintaxis del lenguaje *C*, sobre todo en lo referido a los bloques y las llaves delimitadoras, añadiendo y quitando niveles de indentación sobre la marcha.

Existen otros dos mecanismos, más sofisticados y configurables, como son los definidos en las opciones `cindent`, para lenguajes basados en *C*, e `indentexpr`, para configurar completamente la indentación.

Independientemente del mecanismo usado, al añadir y quitar niveles de indentación, *Vim* los reorganiza utilizando caracteres de tabulación y, si es necesario, espacios. Si `expandtab` está activo, solo usará espacios.

La autoindentación afecta solo a las nuevas líneas creadas, con lo que cambiar las opciones de autoindentación no afectará al texto ya escrito.

Al pegar texto de los registros mediante operaciones *put*, se aplicará también este mecanismo (si está activado). Si no deseamos que sea así, podemos desactivar la autoindentación momentáneamente, o activar la opción `paste`, que evita que las líneas pegadas sufran autoindentación.

En modo *Normal* podemos utilizar `=` para recalcular automáticamente la indentación de una línea o un bloque de texto ya escrito. De forma similar a como se utilizaban las acciones `<` o `>`, podemos utilizar `=` en modo *Visual*, o combinándolo con un comando de movimiento del cursor o un objeto de texto. Si queremos recalcular solo la indentación de la línea actual, podemos hacerlo con `==`.

Este recálculo, por defecto, utiliza una sintaxis basada en el lenguaje *C*, a no ser que hayamos definido la opción `indentexpr`, en cuyo caso utilizará ese método.

## Folding

El mecanismo de *folding* permite contraer y expandir varias líneas consecutivas, formando pliegues para ayudar a hacer el código más manejable.

Cuando los bloques de código (bucles, funciones, métodos, etc.) son muy grandes, resulta muy útil poder reducirlos visualmente a una sola línea y desplegarlos cuando sea necesario editarlos.

Este mecanismo se activa mediante la opción `foldenable`, la cual se activa automáticamente cuando realizamos algunas operaciones de *folding*. Se puede activar explícitamente con el comando `:set`, o con `zN`. Mediante `zn` se desactiva.

Existen varios métodos distintos de *folding*. Para seleccionar el que más nos convenga, debemos asignar a la opción `foldmethod` uno de los siguientes valores: `manual`, `syntax`, `indent`, `diff`, `expr` y `marker`. Veamos los tres primeros con más detalle, ya que a pesar de su simplicidad, se ajustarán a nuestras necesidades la mayor parte de las veces.

### Manual

Para especificar manualmente nuestros propios pliegues, configuraremos la opción `foldmethod` en `manual`.

Para crear un pliegue, se debe situar el cursor en la primera línea que lo formará, y teclear `zf` seguido de un comando de movimiento que lleve el cursor a la línea final incluida en el pliegue. En el momento de crearse, los pliegues se contraen visualmente, en una línea que indica el número de líneas que lo componen y el contenido de la primera de ellas.

Por ejemplo, `zf9j` creará un pliegue de 10 líneas, empezando por la actual. El argumento numérico se puede teclear también en primer lugar (`9zfj`). Si pulsamos `zfk`, se crea un pequeño pliegue de dos líneas que empieza en la línea superior a la actual (nos desplazamos 1 línea hacia arriba).

También se puede crear un pliegue mediante `zF` con argumento numérico. Esto crea un pliegue que empieza en la línea actual. El número especificado indica el número total de líneas que lo formarán.

Existe otro modo de crearlos entrando en modo *Visual*, seleccionando las líneas deseadas y tecleando `zf`. Se forma entonces un pliegue con las líneas que estén seleccionadas total o parcialmente.

Especialmente útil en este caso puede resultar la selección de objetos de texto. Se puede formar también un pliegue con un objeto de texto sin, entrar en modo Visual. Así, la secuencia `va{zf` (modo *Visual*, selección de objeto de texto y creación de pliegue) equivaldría simplemente a `zfa{`.

Adicionalmente, podemos crear pliegues desde el modo *Comando* entrando un rango de líneas delante del comando `:fold`:

`:25,60 fold`

crea un pliegue entre las líneas 25 y 60, ambas inclusive.

### Sintaxis o indentación

El método de *folding* `syntax` crea los pliegues a partir de los elementos sintácticos del lenguaje. En el caso de los lenguajes basados en *C*, por ejemplo (*C++*, *C#*, *Java*, *Javascript*, *PHP*, etc.), se definirían a partir de las llaves que forman bloques (funciones, métodos, bucles, etc.).

Por otro lado, el método `indent` se basa exclusivamente en la indentación que tiene el código. Este método resulta útil para lenguajes como *Python*.

No se pueden utilizar los comandos de creación y eliminación de pliegues en estos modos, ya que estos se crean automáticamente.

Al cambiar a un modo automático, se crean los pliegues pertinentes de forma automática, perdiéndose los que estuviesen creados con anterioridad. Si tras esto establecemos modo `manual`, los pliegues creados automáticamente no se perderán, aunque se podrán eliminar los que deseemos, o añadir otros nuevos manualmente.

### Expansión y contracción

Es importante apuntar que los pliegues pueden (y suelen) estar anidados.

Para abrir (expandir) un pliegue cerrado (contraído), hay que colocarse en la línea contraída y presionar `zo`. Para cerrar el pliegue, simplemente hay que colocarse en alguna de las líneas que lo componen y presionar `zc`. Si la línea en la que estamos pertenece a más de un pliegue (anidado), se cerrará el más interior. Además, si hay otros pliegues anidados en cualquier otro punto del pliegue que estamos contrayendo, *Vim* recordará el estado (abierto o cerrado) de todos ellos, que será recuperado cuando volvamos a abrir este pliegue en el futuro.

La secuencia `za` abre o cierra el pliegue según su estado (alterna entre `zo` y `zc`).

En el caso de un pliegue anidado, mediante `zC` podemos contraerlo recursivamente, es decir, se cierra como haría `zc`, pero además contraerá también todos los pliegues exteriores que lo contengan.

De forma similar, `zO` expande el pliegue donde está el cursor de forma recursiva, es decir, abriendo también todos los pliegues que contenga.

En cuanto a `zA` abre o cierra el pliegue recursivamente según su estado (alterna entre `zO` y `zC`).

Existe una opción llamada `foldlevel` que sirve para configurar las contracciones y expansiones de pliegues de forma consistente en todo el archivo. En el mismo momento en que le asignamos un valor numérico, todos los pliegues de nivel superior a ese número se contraen, mientras que los de nivel igual o inferior a ese número se expanden.

Para *Vim*, los pliegues más exteriores tienen nivel 1. Un pliegue definido dentro de un pliegue de nivel 1 tiene nivel 2. Otro pliegue dentro de este último tendrá nivel 3. Y así sucesivamente.

Por lo tanto, cuando hacemos

`:set foldlevel=0`

todos los pliegues del archivo se contraen. Si le asignamos el número 3, los pliegues de nivel 1 hasta 3 se abrirán, mientras los de nivel 4 y superior se cerrarán.

La orden `zM` establece el valor de esta opción en 0, es decir, contrae todos los pliegues. Por el contrario, `zR` establece el valor en el nivel del pliegue más interior de todo el archivo, es decir, expande todos los pliegues. La acción `zm` reduce `foldlevel` en 1 (aumenta la contracción), mientras que `zr` aumenta la opción en 1 (reduce la contracción). Mediante `zm` y `zr` no podemos establecer un nivel de *folding* inferior a 0 ni superior al del pliegue más interior del archivo.

Podemos usar también el modo *Comando* para abrir y cerrar pliegues, utilizando los comandos `:foldopen` y `:foldclose` precedidos de un rango de líneas:

`:25 foldopen`

abre el pliegue en la línea 25, mientras que

`:13,30 foldclose`

contraería todos los pliegues que tengan alguna línea entre la 13 y la 30.

Estos dos comandos no son recursivos, con lo que no cierran más de un pliegue a la vez aunque se trate de un pliegue anidado. En ese caso, `:foldopen` abriría el pliegue contraído más exterior posible, mientras que `:foldclose` cerraría el más interior posible.

### Movimiento por pliegues y secciones

Existen algunas secuencias para moverse rápidamente entre pliegues. Con `zj`, el cursor se desplazará hasta el inicio (primera línea) del siguiente pliegue tras el actual (independientemente de su nivel de anidamiento). Si tal pliegue no existe, el cursor no se mueve. Con `zk`, el cursor se desplaza hasta el final (última línea) del pliegue anterior al actual, también sin importar su nivel de anidamiento, y solo en caso de existir tal pliegue.

Mediante `[z` desplazamos el cursor a la primera línea del pliegue en el que nos encontremos. Si ya estamos en esa línea, se desplaza a la primera línea del pliegue inmediatamente exterior que lo contenga, si existe. Y así sucesivamente. De forma análoga, `]z` se desplaza a la última línea del pliegue en el que esté el cursor.

Adicionalmente, existe una forma de desplazarse por secciones. Aunque no está relacionado con el *folding*, en algunos lenguajes de programación se definen secciones por defecto que pueden ser útiles. Por ejemplo, las funciones en *C* (bloques de nivel más exterior) forman secciones. De este modo podemos utilizar las siguientes órdenes:

- `[[` para ir al principio de la sección actual. Si ya está en él, al principio de la sección anterior.
- `]]` para ir al principio de la siguiente sección. Si no existe, al final de la sección actual.
- `[]` para ir al final de la sección anterior, si existe. De lo contrario, al principio de la sección actual.
- `][` para ir al final de la sección actual. Si ya está en ese punto, al final de la siguiente sección.

Tanto las acciones de movimiento por pliegues como por secciones pueden utilizarse conjuntamente con cualquier acción que utilice movimientos del cursor, como las de borrado o *yank*, entre otras. Y, lógicamente, pueden usarse también con argumento numérico, como siempre.

### Eliminación

Para eliminar pliegues (no su texto), es imprescindible estar en el modo de *folding* `manual`. Para eliminar un pliegue hay que colocar el cursor en una de las líneas que lo componen y teclear `zd`. En caso de que el pliegue esté anidado dentro de otro, se borrará únicamente el más interior. Eliminar un pliegue con `zD` funciona del mismo modo, con la diferencia de que si el pliegue a borrar contiene otros pliegues en su interior, serán eliminados también (eliminación recursiva).

Mediante `zE` se eliminan todos los pliegues definidos en el archivo actual.

## Vistas

Los pliegues definidos no se guardan en el archivo, con lo que si salimos de *Vim* se pierde toda la información de *folding*.

Para evitarlo, podemos guardar esta información en lo que se llaman vistas. Ello permite guardar y recuperar la configuración de plegado, junto con otros elementos de configuración, como la visualización de números de línea, o la configuración de indentación. Para guardar la vista actual del archivo que estamos editando, se usa el comando `:mkview` (sin argumentos). Para recuperar esa vista posteriormente, se utiliza el comando `:loadview`. Hay que tener en cuenta que si modificamos la vista e intentamos salir de *Vim* sin guardarla, el programa no nos dará ningún aviso al respecto y perderemos esos cambios.

Para evitar perder involuntariamente los cambios realizados en las vistas, existe un modo de automatizar su guardado y recuperación, añadiendo estas dos líneas en el archivo de configuración *.vimrc*:

```
autocmd BufWinLeave *.* mkview
autocmd BufWinEnter *.* silent loadview
```

Si no deseamos que *Vim* guarde y cargue automáticamente las vistas para todos los archivos, sino solamente para un tipo específico, o incluso para un archivo concreto, se debe cambiar el patrón '*.*' adecuadamente. Si deseamos que se aplique a varios tipos o a varios archivos, deberemos incluir estas dos líneas para cada tipo o para cada archivo concreto, cambiando los patrones de nombre de archivo adecuadamente.

Es importante señalar que para que se pueda usar el guardado y recuperación de vistas debe existir un directorio *.vim* dentro del directorio de usuario, ya que es la ubicación donde *Vim* las guarda.

## Etiquetas

Las etiquetas (*tags*) son una herramienta que puede llegar a ser muy útil. Es un mecanismo que permite crear un índice de las definiciones de los diferentes elementos de un proyecto: funciones, métodos, macros, etc. Para ello es necesario contar con una herramienta externa llamada *Ctags*, disponible en todos los sistemas operativos en sus distintas variantes. Esta utilidad genera un archivo, normalmente llamado *tags*, que guarda el índice de etiquetas.

Una de las variantes más conocidas es *Exuberant Ctags*, que reconoce una gran cantidad de lenguajes. Es la que utilizaremos para nuestros ejemplos.

Normalmente almacenaremos cada uno de nuestros proyectos en su correspondiente directorio, organizado estructuradamente en subdirectorios que contendrán los archivos de código fuente. De este modo, resultará sencillo generar (y regenerar) el índice de etiquetas.

Desde la línea de comandos de nuestro sistema operativo, estableciendo el directorio de trabajo en el directorio raíz del proyecto, podemos ejecutar la herramienta *Ctags* de una forma parecida a esta:

`ctags -R .`

Este comando creará el archivo *tags* en el directorio raíz del proyecto. En su examen de archivos de código, pasará por todos los subdirectorios de ese directorio raíz ('.', directorio actual), gracias al parámetro '-R' que indica modo recursivo.

En el caso que queramos que solo un archivo concreto o grupo de ellos sea incluido en el índice, podemos indicarlo de una forma como esta:

`ctags *.c`

En este caso, solo los archivos con extensión '.c' que residan en el directorio actual serán utilizados.

Otra característica útil de *Ctags* es la posibilidad de añadir entradas al índice sin borrar el contenido anterior. Esto se consigue, normalmente, mediante el parámetro '-a'. Así, si después de ejecutar el anterior comando entramos

`ctags -a include/*.h`

se añadirán al anterior índice los archivos de cabecera del subdirectorio 'include'.

Todos estos comandos pueden, en realidad, ejecutarse desde *Vim*, de la forma que ya conocemos:

`:!ctags -R .`

Para un uso correcto, debemos tener en cuenta el directorio de trabajo al ejecutar estos comandos externos. Para saber el directorio de trabajo actual de *Vim*, es suficiente con entrar

`:pwd`

Si deseamos cambiar dicho directorio, podemos usar el comando :cd` desde el mismo *Vim*.

Una vez hayamos creado un archivo *tags* en el directorio de trabajo, podremos acceder a la funcionalidad de etiquetas.

### Uso de las etiquetas

Si estamos editando un archivo y queremos examinar la definición de una función llamada 'fun', por ejemplo, pero no sabemos en qué archivo se halla definida, podemos teclear simplemente

`:tag fun`

y *Vim* nos llevará al punto exacto donde se encuentra esa definición, abriendo el archivo adecuado si es necesario. Hay que tener en cuenta que si ello representa cerrar el archivo actual y existen cambios sin guardar en este, deberemos guardarlo antes o especificar el forzado de esta acción con '!' para asumir la pérdida de esos cambios.

Una vez examinada la información que deseábamos ver acerca de esa función, podemos volver al punto donde estábamos antes de ejecutar el comando `:tag` introduciendo

`:pop`

Una vez más, si eso implica perder los cambios, habrá que guardar o forzar.

Es posible utilizar el mecanismo de etiquetas desde modo *Normal*. Para ello hay que localizar en el texto del archivo actual el nombre de la etiqueta que deseamos inspeccionar, colocar el cursor sobre algún carácter de ese nombre, y pulsar `C-]`. Para regresar nuevamente al punto de edición anterior, se debe pulsar `C-t`.

## Autocompletar

*Vim* incorpora un mecanismo de autocompletado, que puede ser útil, por ejemplo, cuando no recordamos cómo se escribe exactamente una función pero sabemos cómo empieza, o cuando no recordamos exactamente la ruta o nombre de un archivo que queremos incluir en el código.

El mecanismo se utiliza desde el modo *Insertar* (o *Reemplazar*) y nos lleva a un modo especial, llamado modo *Completar* (*Completion mode*), en el que las sugerencias aparecen en una lista que va cambiando dinámicamente según vamos escribiendo.

En lugar de escribir, podemos movernos por los elementos de la lista de sugerencias mediante `C-n` (siguiente elemento) y `C-p` (elemento anterior). Cuando vemos el elemento que nos interesa insertar, nos desplazamos hasta él y pulsamos `Intro`, de tal modo que la palabra será insertada y saldremos de modo *Completar* (regresando a modo *Insertar*). Si queremos salir de ese modo sin aceptar ninguna sugerencia, debemos pulsar `C-e`. Si lo hacemos con `Esc`, aceptaremos la sugerencia y saldremos, pero pasando a modo *Normal* en lugar de regresar a modo *Insertar*.

Para entrar en el modo *Completar*, debemos estar en modo *Insertar* y teclear el principio de la palabra que deseamos completar. Seguidamente pulsaremos `C-x`. En este momento, *Vim* esperará una segunda pulsación que indique la fuente donde buscar las sugerencias. De las distintas posibilidades, podemos destacar las siguientes:

- `C-n` busca palabras del archivo actual.
- `C-i` busca en el archivo actual y en los archivos que incluye. Por defecto, buscará archivos incluidos mediante '#include', que es la forma utilizada en lenguaje *C*. Para definir otro tipo de inclusiones, se debe modificar la opción `include`.
- `C-l` busca líneas completas. Las fuentes por defecto son la lista entera de *buffers* (activos, ocultos y en disco), etiquetas (*tags*) si están definidas, y archivos incluidos (como en el caso de `C-i`). Si queremos cambiar estas fuentes, de debe modificar la opción `complete`.
- `C-]` busca en las etiquetas (*tags*) si está definido el archivo *tags*.
- `C-f` completa nombres de archivo existentes en disco. Funciona con rutas relativas (al directorio de trabajo) y absolutas.

Existe otra forma de entrar en modo *Completar* sin pulsar `C-x`. Se puede conseguir directamente mediante `C-n` o `C-p`, indistintamente. En este caso buscará coincidencias en las fuentes definidas en la opción `complete`, al igual que en el caso de `C-x C-l`, con la diferencia de que en este caso completaremos una palabra y no una línea entera. Como se ha dicho, la opción `complete` indica, por defecto, las siguientes fuentes: la lista entera de *buffers*, las etiquetas y los archivos incluidos en el archivo actual.

Por último, un apunte útil: es frecuente utilizar el mapeo de atajos de teclado para insertar pequeños fragmentos de código (*snippets*) que se utilicen con frecuencia. Como siempre, deberemos asegurarnos de mapear, si es posible, secuencias no utilizadas por defecto por *Vim* o asegurarnos de que no vamos a necesitar ese atajo predefinido.

Si deseamos mecanismos de autocompletar más sofisticados y adaptados a un lenguaje concreto, podemos instalar una extensión (*plugin*) en *Vim* que nos permita acceder a esa mejora.

## Compilación y ejecución

Para probar y ejecutar el código fuente que estamos editando puede ser necesario invocar un compilador para obtener un archivo ejecutable. Es el caso de los lenguajes compilados, como *C*.

Si estamos editando un programa en un lenguaje compilado, para generar ese archivo ejecutable será suficiente una llamada al compilador. Suponiendo un programa en lenguaje *C* llamado 'myprogram.c' que esté en el directorio de trabajo actual, se podría usar un comando en *Vim* similar a este:

`:!gcc -o myprogram myprogram.c`

Con objeto de simplificar este proceso, se puede mapear este comando de compilación, o crear una macro para no tener que teclearlo cada vez que deseemos compilar.

En el caso de un proyecto grande, podría ser necesario crear un *makefile* e invocar la utilidad *Make*. En este caso, *Vim* nos permite hacerlo usando simplemente el comando `:make`. Para funcionar correctamente, es necesario que el *makefile* esté dentro del directorio actual.

Para comprobar el funcionamiento de un programa, tanto si se trata de un archivo ejecutable como si es un *script* de código en un lenguaje interpretado, se puede invocar el mismo desde el modo *Comando* con el signo de exclamación. Nuevamente, podemos incluir esa invocación, sobre todo si es larga y lleva argumentos, en un atajo o macro.

Si deseamos depurar (*debug*) el código, necesitaremos una herramienta externa para hacerlo, o instalar una extensión (*plugin*) en *Vim* que nos permita disponer de esa funcionalidad.
