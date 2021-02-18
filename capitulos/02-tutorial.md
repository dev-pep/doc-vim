# Tutorial completo

Las órdenes de este tutorial se ejecutan en el *modo Normal*. En caso de duda, la tecla `Esc` lleva a ese modo. `Esc` también limpia cualquier secuencia de teclas que esté a medio teclear.

Las combinaciones de teclas tienen el siguiente formato:

- `C-v` se refiere a las teclas `Control` + `v`.
- `V` es la tecla `V` (mayúscula), es decir, `Mayúsculas` + `v`.
- `v` es simplemente la tecla `v`.

## Lección 1

### Lección 1.1 - Movimientos del cursor

Teclas para desplazar el cursor por el texto:

- `h` (izquierda).
- `j` (abajo).
- `k` (arriba).
- `l` (derecha).

Aunque las flechas del teclado también funcionan, la ventaja de *Vim* es que proporciona mucha agilidad cuando se trabaja sin mirar el teclado. Aunque no es imprescindible, saber mecanografía acelera mucho el trabajo con *Vim*.

### Lección 1.2 - Entrando y saliendo de Vim

Mediante `:` se entra en *modo Comando*. Entonces se puede teclear un comando, seguido de `Intro`. Algunos comandos:

- `:w` - guarda el archivo en disco.
- `:q` - cierra el archivo (si es el único abierto, cierra *Vim*).
- `:wq` - guarda y cierra.
- `:q!` - cierra sin guardar los cambios.

Para entrar en *Vim* desde línea de comandos, se debe teclear `vim <nombre>`, donde \<nombre\> es el nombre del archivo que deseamos abrir. Si no se especifica nombre, *Vim* se ejecuta sin abrir ningún archivo.

### Lección 1.3 - Edición de texto

Con la orden `x` se borra el carácter que esté sobre el cursor.

### Lección 1.4 - Edición de texto - inserción

Con la orden `i` entramos en *modo Insertar*. Entonces podemos insertar texto normalmente. Para desplazar el cursor en este modo, sí son necesarias las flechas del teclado, aunque los puristas prefieren no utilizar este tipo de desplazamientos, y no utilizar el modo *Insertar* para desplazarse.

Para volver a modo *Normal*, se pulsa `Esc`.

## Lección 2

### Lección 2.1 - Mandatos para borrar

Mediante `dw` (*delete word*) se borra hasta el final de la palabra actual, incluyendo el espacio en blanco posterior (aunque solo dentro de la línea actual).

### Lección 2.2 - Más mandatos para borrar

`d$` se usa para borrar hasta fin de línea, incluyendo el carácter bajo el cursor.

### Lección 2.3 - Sobre mandatos y objetos

La orden `d` (*delete*) tiene cualquiera de estos dos formatos:

- `[número] d <objeto>`
- `d [número] <objeto>`

El número (opcional) puede tener uno o más dígitos. El objeto puede ser, por ejemplo:

- `w` - una palabra, incluyendo el espacio blanco posterior.
- `e` - una palabra, sin incluir el espacio blanco posterior.
- `$` - fin de línea.

### Lección 2.4 - Una excepción al \'mandato-objeto\'

Mediante `dd` borramos la línea sobre la que se halla el cursor. El formato puede tener cualquiera de las dos formas:

- `[número] dd`
- `d [número] d`

### Lección 2.5 - El mandato deshacer

`u` retrocede un paso en el historial de deshacer. `U` añade al historial un paso que revierte el estado de una línea de texto a la situación anterior a su edición. La acción de `U` se puede deshacer a su vez con `u`.

Para rehacer (avanzar un paso en el historial de deshacer) se usa `C-r`.

## Lección 3

### Lección 3.1 - El mandato put (colocar)

Con `p` (*put*) se añade después del cursor el contenido de la última acción de borrado. Si se ha borrado una línea mediante `dd`, se crea una línea debajo del cursor.

### Lección 3.2 - El mandato replace (reemplazar)

Mediante `r` (*replace*) seguido del carácter deseado, se sustituye el carácter bajo el cursor por el carácter indicado.

### Lección 3.3 - El mandato change (cambiar)

La orden `cw` (*change word*) borra la palabra actual desde la posición del cursor (incluida) hasta el final (sin incluir el espacio blanco posterior), y entra en modo *Insertar*. Útil para cambiar palabras (total o parcialmente).

### Lección 3.4 - Más cambios usando c

`c` se puede usar con los mismos objetos que `d` (*delete*):

- `[número] c <objeto>`
- `c [número] <objeto>`

Si especificamos un número superior a 1, indicamos el número de objetos consecutivos a borrar antes de entrar en modo *Insertar*.

## Lección 4

### Lección 4.1 - Situación en el fichero y su estado

`C-g` muestra información del estado del archivo: su nombre, si está modificado (sin guardar), línea actual, líneas totales (porcentaje de la línea actual) y número de columna.

`G` desplaza a la última línea. Si se antecede un número, desplaza a esa línea.

### Lección 4.2 - El mandato search (buscar)

Tiene la forma `/<texto>` y se utiliza para buscar el texto especificado. El texto se visualiza en la línea inferior mientras lo tecleamos, como en el modo *Comando*.

Para repetir la misma búsqueda posteriormente, se usa la orden `n`, que buscará hacia abajo la siguiente ocurrencia del texto. Para buscar hacia arriba, se usa la orden `N`.

Si en lugar de buscar con `/` lo hacemos mediante `?<texto>`, la búsqueda se realizará hacia arriba inicialmente, al igual que hará `n`; con `N` buscaremos hacia abajo.

### Lección 4.3 - Búsqueda para comprobar paréntesis

Con el cursor sobre un paréntesis, llave o corchete (`(`, `)`, `{`, `}`, `[`, `]`), la orden `%` desplazará el cursor al paréntesis, llave o corchete correspondiente.

### Lección 4.4 - Una forma de cambiar errores

Mediante `:s/<viejo>/<nuevo>/g` sustituimos la siguiente ocurrencia del texto \<viejo\> por \<nuevo\>.

Si queremos que la sustitución se produzca solo en la línea actual, el formato es `:s/<viejo>/<nuevo>` (sin la \'g\', que denota «global»).

Si queremos restringir la sustitución a un fragmento del fichero entre las líneas \<L1\> y \<L2\>, el formato es
`:<L1>,<L2>s/<viejo>/<nuevo>/g`

Para cambiar todas las ocurrencias del texto en todo el fichero a la vez, se usa `:%s/<viejo>/<nuevo>/g`

En este último caso, si queremos que nos solicite confirmación para cada ocurrencia, debemos teclear `:%s/<viejo>/<nuevo>/gc`

## Lección 5

### Lección 5.1 - Cómo ejecutar un comando externo

`:!<comando>` ejecuta un comando externo y muestra su resultado en pantalla.

### Lección 5.2 - Más sobre guardar ficheros

Si queremos guardar el fichero con un nombre concreto, teclearemos:

`:w <nombre>`

### Lección 5.3 - Un mandato de escritura selectivo

Para guardar en un archivo únicamente el fragmento comprendido entre las líneas \<L1\> y \<L2\> (ambas inclusive), teclearemos:

`:<L1>,<L2> w <nombre>`

### Lección 5.4 - Recuperando y mezclando ficheros

Para insertar el contenido de un fichero (debajo de la línea del cursor), teclearemos `:r <nombre>`

## Lección 6

### Lección 6.1 - El mandato open (abrir)

Con la orden `o` (*open*) se crea (abre) una nueva línea en blanco debajo de la línea donde está el cursor; inmediatamente se desplaza el cursor a esa línea y entra en modo *Insertar*.

Utilizando `O` sucede lo mismo, pero justo encima de la línea actual.

### Lección 6.2 - El mandato append (añadir)

Mediante la orden `a` (*append*) entramos en modo *Insertar* con el cursor **después** del carácter actual. Cuando usamos `i`, el cursor queda antes de ese carácter, con lo que si queremos añadir texto al final de la línea, la orden `a` es más útil.

Para añadir texto al final de línea con `i`, tenemos dos opciones:

- Colocarse al final de la línea, pulsar `i`, volver a teclear el último carácter de la línea, teclear el texto a añadir, volver al modo *Normal* con `Esc`, desplazarse una posición a la derecha y pulsar `x` para borrar el carácter que ha quedado flotando.
- Colocarse al final de la línea, pulsar `i`, pulsar la flecha derecha, teclear el texto a añadir y volver al modo *Normal* con `Esc`. Pero el uso de las flechas va contra la filosofía de *Vim*.

### Lección 6.3 - Otra versión de replace (reemplazar)

Con `R` entramos en *modo Reemplazar*, en el cual, en lugar de insertarse, el texto tecleado va reemplazando al texto antiguo.

### Lección 6.4 - Fijar opciones

Se pueden activar y desactivar opciones de *Vim* mediante el comando `set`. El formato es `:set <opción>`

Algunas opciones:

- `ic` o `ignorecase` ignora las mayúsculas/minúsculas en las búsquedas.
- `hls` o `hlsearch` remarca todo el texto coincidente.
- `is` o `incsearch` va remarcando la primera ocurrencia del texto mientras lo escribimos, de forma incremental.

Para desactivar una opción, añadiremos el prefijo \'no\' en la misma (por ejemplo, `:set nois`). Entrando simplemente `:set` obtendremos una lista de las opciones con sus respectivos valores.

Se puede incluir más de una opción en el mismo comando `:set`. Por ejemplo, podríamos introducir

`:set ic nois hls`

para activar `ic` y `hls`, al tiempo que desactivamos `is`.

## Lección 7

El sistema de ayuda en línea de *Vim* permite acceder a la ayuda de varias formas:

- Pulsando `F1`.
- Mediante el comando `:help`.

Si queremos ayuda sobre un tema o comando concreto, podemos usar el comando `:help <tema>`.

La ayuda se abre en una ventana de texto nueva. Mediante `:q` cerraremos esa ventana y volveremos a nuestro archivo.
