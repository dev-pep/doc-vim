# Mapeo y configuración

## Configuración de Vim

### Opciones

Existen numerosos mecanismos para configurar el comportamiento de *Vim*. Uno de ellos es mediante el comando `:set`.

Como hemos visto ya, existen numerosas opciones cuyo estado es de tipo booleano, es decir, están activadas o desactivadas. Pongamos por ejemplo la opción `ic` (`ignorecase`) para ignorar mayúsculas y minúsculas en las búsquedas. Para *activar* esa opción entraremos

`:set ic`

o

`:set ignorecase`

Al igual que `ignorecase`, las opciones tienen su versión abreviada, que puede utilizarse indistintamente con la palabra completa.

Para *desactivar* una opción concreta, bastará añadir el prefijo 'no' a esa opción. En nuestro ejemplo anterior, podemos desactivar `ignorecase` entrando

`:set noignorecase`

o

`:set noic`

Algunas opciones tienen más de un estado posible. Es el caso, por ejemplo, de la opción `display`, que puede tener valores como `truncate`, `lastline` o nada (cadena vacía). En este caso, para cambiar su valor hay que asignárselo mediante el carácter '=' de este modo:

`:set display=truncate`

o

`:set display=`

para asignarle la cadena vacía.

Por otro lado, si no estamos seguros del estado que tiene de una opción concreta, podemos verlo añadiendo el signo de interrogación al final. En nuestro caso, para `ignorecase` entraremos

`:set ignorecase?`

lo cual nos mostrará el estado actual: 'ignorecase' o 'noignorecase'.

Esto funciona para todas las opciones, tengan estado booleano (activo o inactivo) o no (múltiples valores). En el caso de que no sea así, también podemos consultar su estado sin necesidad de incluir el signo de interrogación:

`:set display`

Si queremos ver los valores de todas las opciones cuyo valor difiere de su valor por defecto en *Vim*, entraremos simplemente

`:set`

Si lo que queremos es ver el estado de *todas* las opciones de *Vim*, entraremos

`:set all`

Podemos dar el valor por defecto a una opción utilizando '&':

`:set ignoracase&`

En el caso de las opciones booleanas, podemos cambiar su estado usando el signo de exclamación:

`:set ignorecase!`

activará la opción si estaba desactivada, o la desactivará en caso contrario.

Para obtener una información más detallada sobre el funcionamiento de las diversas opciones, se puede usar el comando `:h` (o `:help`) con la opción deseada (obreviada o no), entre comillas simples:

`:h 'ignorecase'`

En cambio, para ver la ayuda sobre un comando concreto, se debe especificar prefijando los dos puntos (':'), en su forma normal o abreviada:

`:h :set`

o

`:h :se`

### El archivo .vimrc

Cuando cerramos *Vim*, todos los ajustes realizados con `:set` se pierden. Al volver a abrir el editor, se deben realizar de nuevo. Para evitarlo, disponemos del archivo *.vimrc*, donde podemos establecer los ajustes iniciales al abrir el programa, cuando no deseemos mantener los valores por defecto. En algunos sistemas operativos que no sean de tipo *Unix*, este archivo de configuración puede llamarse *\_vimrc*.

Cada vez que ejecutamos *Vim*, los comandos que incluyamos en este archivo serán ejecutados al inicio.

Existe un archivo *.vimrc* que afecta a todos los usuarios del sistema. A parte, cada usuario puede tener su propio *.vimrc* con sus propias preferencias. El *.vimrc* del sistema se ejecuta primero, y después el de usuario, de tal forma que si hay ajustes contradictorios, el último ejecutado (el de usuario) tendrá preferencia. Para saber dónde busca estos archivos de configuración nuestro *Vim*, podemos ejecutar `:version`. Entre la información que proporciona este comando, veremos qué archivos de configuración carga, y en qué orden.

Como veremos, la salida de este comando utiliza las variables *\$VIM* (directorio de *Vim*) y *\$HOME* (directorio del usuario) para especificar la ruta de estos archivos de configuración. Si queremos saber el valor de esas variables podemos hacerlo mediante

`:echo $VIM`

o

`:echo $HOME`

En cuanto al formato del archivo de configuración, las líneas que empiezan con dobles comillas son ignoradas, pues es la forma de definir comentarios. En cuanto a los comandos, no es necesario escribir los dos puntos iniciales. Un ejemplo de archivo *.vimrc* podría ser:

```
" Configuraciones varias
set display=truncate
set ignorecase
```

### Información viminfo

Al salir de *Vim*, no solo se pierde la configuración de opciones. Existen otros tipos de información que se pierden, como el historial de comandos, el historial de búsquedas, el contenido de los registros, las marcas de texto de los diferentes archivos, o la lista de *buffers*.

Para evitar que se pierdan todos estos elementos al cerrar el programa, *Vim* utiliza un archivo que llamaremos 'viminfo', donde almacena estas y otras informaciones útiles. En general no tiene mucha utilidad saber dónde está almacenado este archivo ni su nombre exacto, ya que es *Vim* quien se encarga de su mantenimiento.

Para configurar qué información deseamos que se mantenga entre una sesión y otra, está la opción `viminfo`, que guarda una cadena de caracteres que define exactamente esta configuración. Para una información detallada de esta opción, se puede utilizar

`:h 'viminfo'`

En general, la configuración por defecto de esta opción nos será útil la mayor parte de las veces, de tal modo que no perderemos el contenido de los registros (incluyendo las macros definidas), el historial de comandos, el historial de búsquedas, o las marcas de texto.

## Mapeo de teclas

*Vim* permite asociar secuencias de acciones a pulsaciones específicas del teclado, de forma similar a como hacían las macros, pero asociando estas acciones a una tecla o secuencia de teclas y no a un registro. Para ello proporciona el mecanismo de mapeo (*mapping*), que consiste precisamente en mapear una serie de acciones a un atajo de teclado concreto.

Aunque los mapeos se pueden definir sobre la marcha dentro de una sesión de *Vim*, la verdadera utilidad de este mecanismo es la configuración de atajos de teclado en el archivo *.vimrc*, ya que en el primer caso, el atajo se pierde al cerrar el programa.

Hay que tener en cuenta que los atajos de teclado se deben definir para un modo o conjunto de modos concreto. La forma más simple de definir un mapeo es mediante un comando de mapeo seguido de una secuencia de una o más teclas a mapear, y finalmente la secuencia de acciones que se ejecutarán cuando se invoque ese atajo de teclado.

A la hora de crear un atajo, *Vim* distingue entre los modos *Normal*, *Insertar*, *Visual*, *Selección*, *Comando* y *Pendiente*. El modo *Selección* tiene poco interés, y es una variante del modo *Visual*. El modo *Pendiente* se refiere al momento concreto en que dentro del modo *Normal* hemos empezado a pulsar una secuencia de acciones y *Vim* está a la espera del resto de la secuencia. Por ejemplo, después de pulsar ***y*** para una acción *yank*, y antes de definir la acción de movimiento.

*Vim* asocia una letra prefijo a cada uno de estos modos: 'n' (*Normal*), 'i' (*Insertar* y *Reemplazar*), 'v' (*Visual* y *Selección*), 's' (*Selección*), 'x' (*Visual*), 'c' (*Comando*) y 'o' (*Pendiente*, *operator-pending*). Cabe añadir que el prefijo 'c' se refiere también a las búsquedas (con `/` o `?`).

Los comandos de mapeo son los siguientes: `:map`, `:map!`, `:nmap`, `:imap`, `:vmap`, `:smap`, `:xmap`, `:cmap` y `:omap`. En el caso de `:map`, el atajo se define para los modos *Normal*, *Visual*, *Selección* y *Pendiente*. En cambio, `:map!` se refiere a los modos *Insertar* y *Comando* (y afines, como *Reemplazar* y búsqueda).

Tras el comando hay que especificar la tecla que queremos mapear. Si queremos mapear una secuencia de dos o más teclas, hay que escribirla seguida, sin espacios.

Por último se debe definir la secuencia de acciones que se llevarán a cabo cuando se pulse el atajo que estamos creando. Veamos un ejemplo:

`:imap aa Pepe`

define un atajo de teclado de tal modo que cuando pulsemos la secuencia `aa` dentro de modo *Insertar*, se insertará 'Pepe' en el texto en lugar de 'aa'. Recordemos que si definimos el mapeo en *.vimrc* no es necesario incluir los dos puntos antes del comando.

Es importante recalcar aquí que a la hora de mapear una secuencia de teclado, podemos perder funcionalidades de *Vim* que estén definidas previamente si no vamos con cuidado. Si entramos

`:nmap u dd`

perderemos la funcionalidad original de `u` en modo *Normal*, que es la acción de deshacer la última acción, y en cambio, borrará la línea actual.

Para ver la lista completa de acciones de teclado definidas por defecto en *Vim*, se puede introducir

`:h index.txt`

De este modo podemos ver qué secuencias están libres o, si decidimos cambiar algún atajo que no vayamos a utilizar, podemos saber exactamente qué secuencias predefinidas estamos cambiando.

Es posible comprobar el uso por defecto de una pulsación concreta. Por ejemplo, si queremos ver el uso de `C-v` dentro del modo *Insertar*, teclearemos

`:h i\_CTRL-V`

Aquí podemos usar el prefijo 'i' (modo *Insertar*), 'c' (modo *Comando* o búsqueda de patrones), 'v' (modo *Visual*) o sin prefijo ni guión bajo (modo *Normal*).

Es frecuente definir atajos de teclado que empiecen por coma (',') o punto y coma (';') para evitar interferir con los atajos por defecto.

Para ver los mapeos definidos en un modo concreto, usaremos el comando pertinente sin argumentos. Por ejemplo:

`:nmap`

nos mostrará los atajos de teclado definidos para el modo *Normal*. Del mismo modo, el resto de comandos de mapeo muestra los atajos definidos en su modo o modos relacionados. En este caso, por ejemplo, `:map!` mostrará la lista de mapeos dentro de los modos *Insertar* y *Comando* (y afines).

El primer campo de la lista mostrada especifica la letra o letras relacionadas con los modos para los que está definido cada mapeo. Si ese primer campo es un signo de exclamación, se trata de un atajo definido en los modos del comando `:map!`. Si es un espacio en blanco, se refiere a los modos de `:*map`.

Los elementos de la lista que contienen un asterisco ('\*') son mapeos definidos como no recursivos, lo cual se verá un poco más adelante.

Para ver una lista de atajos concretos, podemos especificar un prefijo:

`:nmap g`

mostrará los atajos en modo *Normal* que mapeen secuencias que empiecen por 'g'.

### Eliminación de mapeos

Para eliminar un atajo definido previamente, se utilizan los comandos `:unmap`, `:unmap!`, `:nunmap`, `:iunmap`, `:vunmap`, `:sunmap`, `:xunmap`, `:cunmap` y `:ounmap`, que se corresponden con los respectivos comandos de mapeo. En este caso, el comando define el modo o modos para los que se debe eliminar el atajo. Estos comandos necesitan un argumento con la secuencia precisa a eliminar.

Supongamos que creamos un atajo de este modo:

`:map! p pepe`

Esto define el mapeo `p` para los modos *Insertar* y *Comando*. Si posteriormente eliminamos el mapa de este modo:

`:cunmap p`

el mapeo quedará definido únicamente para el modo *Insertar*. Así, estos dos comandos equivalen a uno solo:

`:imap p pepe`

Para eliminar *todos* los mapeos definidos en un modo o modos concretos, existen los comandos `:mapclear`, `:mapclear!`, `:nmapclear`, `:imapclear`, `:vmapclear`, `:smapclear`, `:xmapclear`, `:cmapclear` y `:omapclear`, relacionados con los respectivos comandos de mapeo.

### Mapeo recursivo

Si la acción a ejecutar definida en un atajo contiene alguna secuencia que a su vez está definida en otro mapeo, será previamente sustituida. Veamos un ejemplo:

`:imap a jkn`

`:imap n rrr`

En este caso, al pulsar `a` dentro de modo normal, se insertará el texto 'jkrrr', dado que la 'n' del mapeo a aplicar está mapeada a su vez a 'rrr'.

Al mecanismo que aplica un mapeo dentro de la secuencia a mapear se le denomina *mapeo anidado o recursivo*. Debe irse con cuidado al definir este tipo de mapeos para no crear recursiones infinitas, como en este caso:

`:imap b abc`

Se trata de una recursión infinita, ya que siempre habrá una 'b' que deberá sustituirse. En este caso, cuando pulsemos `b` en modo *Insertar*, se producirá un bucle de mapeos infinitos, que deberemos cortar con `C-c`, generando un resultado impredecible. Veamos un caso menos evidente de recursión infinita:

`:imap a jkn`

`:imap n rst`

`:imap t abc`

En este caso, al aplicar las distintas sustituciones, se acaba formando un bucle infinito: 'a' se mapea a 'jkn', que quedaría sustituido por 'jkrst', que a su vez se sustituiría por 'jkrsabc', donde vuelve a haber una 'a'.

Existe una excepción a este mecanismo: cuando la secuencia del atajo es un prefijo de la secuencia resultante, no se aplica la sustitución. En este ejemplo:

`:imap ab abcd`

no se produce sustitución porque 'ab' es prefijo de 'abcd', pero en este caso:

`:imap ab abcdab`

no se realizaría la sustitución en el primer 'ab' de la secuencia resultante, pero sí en el segundo, produciéndose, ahora sí, un bucle infinito.

Para evitar que se produzca recursión de ningún tipo, existen las versiones de los comandos de mapeo que no realizan recursión: `:noremap`, `:noremap!`, `:nnoremap`, `:inoremap`, `:vnoremap`, `:snoremap`, `:xnoremap`, `:cnoremap` y `:onoremap`. En este ejemplo:

`:inoremap b abc`

al pulsar `b` en modo *Insertar* no se producirá ningún tipo de sustitución en la secuencia resultante y simplemente se insertará el texto 'abc'.

### Notación de teclas

A la hora de describir los mapeos, hay que hacer referencia con frecuencia a ciertas combinaciones de teclas como `<Esc>`, `<Intro>` u otras del tipo `C-w`. ¿Cómo podemos escribir estas teclas en la definición?

Una solución a este problema la vimos cuando definíamos macros por escrito. Al escribir la macro, pulsábamos `C-v` seguido de la tecla que deseábamos insertar. Esta solución sigue siendo útil en la definición de mapeos, tanto para la secuencia de teclas a mapear, como en la secuencia que definamos para esas teclas.

Sin embargo, en la definición de atajos, podemos utilizar también la versión imprimible (*printable*) de las teclas, de tal modo que aumenta su legibilidad. Esta es la forma aconsejable de hacerlo.

A continuación, podemos ver una relación de algunos códigos de teclas especiales en formato imprimible:

- Tabulador: *\<Tab\>*
- Intro: *\<CR\>*, *\<Enter\>* o *\<Return\>*
- Escape: *\<Esc\>*
- Espacio: *\<Space\>*
- Flecha arriba: *\<Up\>*
- Flecha abajo: *\<Down\>*
- Flecha izquierda: *\<Left\>*
- Flecha derecha: *\<Right\>*
- Teclas de función: *\<F1\>* a *\<F12\>*
- Insertar: *\<Insert\>*
- Suprimir: *\<Del\>*
- Retroceso: *\<BS\>*
- Inicio: *\<Home\>*
- Fin: *\<End\>*
- Página arriba: *\<PageUp\>*
- Página abajo: *\<PageDown\>*
- Carácter '|': *\<bar\>* o *\\|*

Como se puede comprobar en la tabla, el carácter '|' debe indicarse como una secuencia de escape (*\\|*) o *\<bar\>*. Ello se debe a que, como ya vimos, se trata de un carácter con un significado especial en modo *Comando* (es el carácter separador de comandos).

En cuanto a las teclas que se presionan juntamente con la tecla Control, la forma de escribirlas es *\<C-h\>*. En este caso, tanto la 'c' como la tecla acompañante se pueden escribir en mayúscula o minúscula indistintamente.

Veamos un ejemplo con esta notación:

`:inoremap \<C-b\> a\<C-b\>c`

En este caso, cada vez que tecleemos `C-b` en modo *Insertar*, se insertarán tres caracteres en el texto: 'a', '\<C-b\>' (se verá en pantalla como *\^B*) y 'c'.

Hay que tener en cuenta también que en *Vim* existe la siguente equivalencia de teclas: `C-i` equivale al tabulador, `C-[` equivale a `<Esc>`, `C-m` equivale a `<Intro>` y `C-h` equivale a la tecla de retroceso. Pueden utilizarse indistintamente cualquiera de las dos formas.

## Definición de macros en .vimrc

En el apartado sobre macros vimos dos formas de definirlas: mediante ***@*** en modo *Normal*, o almacenando directamente el texto de los mismos en los registros correspondientes. Existe otra forma que consiste en definir el contenido de un registro con el comando `:let`.

A este comando hay que pasarle por parámetro la letra correspondiente al registro que deseamos cambiar, precedida por '@'; seguidamente un signo '=' y el texto que debe almacenarse en el registro, entre comillas simples. Por ejemplo:

`:let \@s='eaPepe'`

define el contenido del registro 's', que en este caso es una macro que quedará, lógicamente, asociada al registro 's'. La macro inserta el texto 'Pepe' al final de la palabra actual, terminando en modo *Insertar*.

Cualquier tecla especial que queramos definir dentro del texto de la macro deberá ser insertado en su versión no imprimible (mediante `C-v`).

La verdadera utilidad de este comando reside en la definición de macros (y cualquier otro contenido de los registros) en el archivo *.vimrc*.
