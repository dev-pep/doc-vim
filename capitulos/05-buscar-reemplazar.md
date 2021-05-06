# Buscar y reemplazar

## Búsqueda dentro de la línea actual

Se puede buscar **un carácter** concreto dentro de la línea donde se halla el cursor mediante la orden `f` seguida del carácter que deseamos encontrar.

Por ejemplo, `fa` dejará el cursor sobre el siguiente carácter 'a', empezando a buscar desde la posición actual del cursor (no incluida) hasta el fin de línea. Si no existe dicho carácter, el cursor permanecerá en su posición.

Tras haber encontrado el carácter buscado, podemos repetir la búsqueda mediante `;`. En cambio, si utilizamos la coma (`,`) se repetirá la búsqueda hacia atrás.

También puede realizarse la búsqueda de un carácter utilizando `F`. Esto buscará directamente hacia atrás.

`Fa` dejará el cursor sobre el anterior carácter 'a' desde la posición actual del cursor, no incluida, sin exceder el principio de línea. Si no existe tal carácter, el cursor no se moverá. De forma análoga, `;` repetirá esa búsqueda, también hacia atrás, mientras que la coma (`,`) lo hará en dirección contraria, es decir, hacia adelante.

Existen otros tipos de búsqueda dentro de la misma línea. `t` es similar a `f`, con la diferencia de que el cursor quedará una posición antes del carácter encontrado. La acción `T` es similar a `F`, dejando el cursor una posición después del carácter encontrado.

El efecto del argumento numérico es el de repetir la acción de búsqueda tantas veces como le indiquemos.

## Búsqueda en todo el archivo

Podemos buscar cualquier patrón de texto utilizando la acción

`/<patrón>`

Donde \<patrón\> es el texto a buscar. La acción debe terminarse con un `<Intro>`. Esto buscará el patrón dentro del archivo entero. La búsqueda se efectúa hacia adelante, y se inicia en la posición del cursor. Si no se encuentra ninguna coincidencia antes del fin de archivo, continúa buscando desde el principio.

La acción de la búsqueda mueve el cursor a la posición inicial del patrón encontrado. Si no existe coincidencia en el archivo, el cursor no se mueve.

Una vez encontrada una coincidencia, la acción `n` repite la búsqueda. Mediante `N` se repite hacia atrás.

La búsqueda del tipo

`?<patrón>`

es similar a la anterior, con la diferencia de que se trata de una búsqueda hacia atrás. Si llega al principio del archivo, sigue buscando por el final. En este caso, la acción `n` también repite hacia atrás, mientras que `N` lo hace hacia adelante.

Independientemente de si la última búsqueda se ha realizado mediante `/` o mediante `?`, la acción `/<Intro>` repite la búsqueda hacia adelante, mientras que `?<Intro>` lo hace hacia atrás.

Las búsquedas tienen su propio historial, al que puede accederse con las flechas arriba y abajo cuando en la esquina inferior izquierda aparece únicamente el carácter '/' o '?'.

Si queremos buscar solo palabras completas, debemos incluir los espacios pertinentes (o signos de puntuación deseados) en el patrón.

Dado que estas acciones de búsqueda son, en realidad, comandos de desplazamiento del cursor, pueden combinarse con las acciones `d` (*delete*), `c` (*change*) o `y` (*yank*). Por ejemplo, la acción `d?la` seguida de `<Intro>` borrará el texto existente entre la posición actual del cursor y la anterior coincidencia del texto \'la\'. Se pueden sofisticar estos borrados o copiados utilizando argumento numérico.

## Reemplazar (substitute)

Se puede reemplazar un patrón de texto por otro de forma automática utilizado el comando *substitute* (`s`) del modo *Comando*.

`:s/<viejo>/<nuevo>`

reemplaza el patrón \<viejo\>, en caso de existir, por el \<nuevo\>. En este caso, solo realizará un reemplazo, en la siguiente coincidencia después de la posición del cursor, y antes del fin de línea.

Si queremos que el comando afecte a otras líneas, se puede anteponer un rango de líneas justo delante del comando `s`. En todo caso, efectuará un solo reemplazo en cada línea, el de la primera coincidencia encontrada en cada una.

Si lo que deseamos es que no se limite a un solo reemplazo por línea, debemos añadir `/g` al final del comando:

`:s/<viejo>/<nuevo>/g`

reemplaza todas las coincidencias en la línea actual.

`:10,20s/<viejo>/<nuevo>/g`

reemplaza todas las ocurrencias en el rango comprendido entre las líneas 10 y 20, ambas inclusive.

Si añadimos `c`, *Vim* además pedirá confirmación en cada reemplazo:

`:%s/<viejo>/<nuevo>/gc`

reemplaza en todo el archivo, e irá solicitando confirmación para cada caso.

También se pueden incluir las letras `i`, para que ignore siempre mayúsculas y minúsculas, o `I`, para que no las ignore nunca. Estas indicaciones tienen prioridad sobre el estado de la opción `ignorecase`.

Hemos visto que se puede reemplazar definiendo un rango de líneas. Existe otra forma de definir un subconjunto de líneas del archivo. En lugar de definir las líneas que componen un rango consecutivo, podemos elegir las que contengan un patrón determinado. Para encontrar dichas líneas utilizaremos la forma `:g/\<patrón\>`. Por ejemplo, si introducimos:

`:g/los`

podremos ver en pantalla el subconjunto de líneas que contienen el patrón de texto 'los'.

Esta es la forma de definir este tipo de subconjuntos de líneas. Ahora podemos incorporarlo a nuestras acciones de reemplazo:

`:g/<patrón>/s/<viejo>/<nuevo>/g`

En todas las líneas que contengan el patrón definido por \<patrón\>, sustituiremos todas la ocurrencias del texto definido en \<viejo\> por el texto especificado en \<nuevo\>; si no añadimos la 'g' final, solo reemplazará la primera ocurrencia (en caso de haberla) de cada línea.

Si lo que queremos sustituir es el patrón de selección por el nuevo texto, no hace falta que tecleemos

`:g/<patrón>/s/<patrón>/<nuevo>/g`

sino que es suficiente teclear

`:g/<patrón>/s//<nuevo>/g`

Aunque en este caso no haría falta usar el comando de selección `g`, ya que se podría aplicar el reemplazo en todo el archivo, con el mismo resultado:

`:%s/<patrón>/<nuevo>/g`

## Expresiones regulares

A la hora de encontrar coincidencias, podemos utilizar no solo patrones de texto, sino también expresiones regulares, que es un mecanismo más flexible y potente. Esto es así tanto en búsquedas (`/` o `?`) como en reemplazos (`:s`).

### Caracteres especiales

A la hora de buscar, podemos encontrarnos que algunos caracteres no se comportan como esperamos. Por ejemplo, si buscamos el carácter punto (`.`), la acción `/.` desplazará el cursor hasta el siguiente carácter, sea cual sea. Ello es debido a que el punto tiene un significado especial. Se refiere a cualquier carácter (excepto fin de línea).

Si queremos que un carácter con significado especial pierda ese significado, hay que incluir el carácter de *escape* (la barra invertida `\`) inmediatamente antes:

`/\.`

desplaza el cursor hasta el siguiente punto.

Otros caracteres con significado especial son `^` (principio de línea), `$` (final de línea) y `*`, que veremos en la siguiente sección.

En cuanto a la barra invertida en sí, si deseamos buscar ese carácter, hay que escribirlo doble (`\\`).

### Repeticiones (quantifiers)

Para buscar una secuencia de texto que puede estar repetida varias veces, usaremos los llamados cuantificadores (*quantifiers*). Estos se aplican al elemento o grupo de elementos inmediatamente a su izquierda.

En primer lugar tenemos el carácter especial asterisco (`*`), que nos marca la **búsqueda codiciosa (*greedy*)** de 0 o más caracteres. Codiciosa significa que intentará hacer coincidir tantos caracteres como le sea posible. Más adelante veremos algún ejemplo de coincidencias codiciosas y no codiciosas.

`/Hola*\.`

busca la secuencia 'Hol', seguida de cero o más caracteres \'a\' tras los cuales hay un punto. Así, son posibles coincidencias 'Hol.', 'Hola.', o 'Holaaaa.', pero no lo son 'hola.' ni 'Holas.'.

La secuencia `\+` corresponde a la *búsqueda codiciosa de 1 o más caracteres*. Veamos un ejemplo:

`/Hola\+\.`

En este caso, son posibles coincidencias 'Hola.', o 'Holaaaa.', pero no lo son 'Hol.', 'hola.' ni 'Holas.'.

`\?` indica *0 o 1 coincidencias*, es decir, un elemento opcional:

`/Hola\?\.`

solo tiene dos coincidencias: 'Hol.' y 'Hola'.

Puede buscarse un número concreto de repeticiones mediante `\{<N>}`, donde \<N\> es el número de repeticiones deseadas:

`/Hola\{10}\.`

tiene una única coincidencia posible: 'Holaaaaaaaaaa.'.

En lugar de un número, podemos buscar un *número de repeticiones dentro de un rango*, usando `\{<min>,<max>}`, donde \<min\> y \<max\> son respectivamente los números mínimo y máximo de repeticiones deseadas, ambos incluidos. *Vim* intentará hacer coincidir la mayor parte de caracteres posible (búsqueda codiciosa).

`/Hola\{3,5}\.`

tiene 3 coincidencias posibles: 'Holaaa.', 'Holaaaa.', y 'Holaaaaa.'.

Si deseamos realizar una búsqueda *no codiciosa de repeticiones dentro de un rango*, utilizaremos `\{-<min>,<max>}`. En este caso la coincidencia se hará utilizando el menor número de caracteres posible.

En la definición de rangos, se puede omitir el número mínimo, el máximo, o ambos, teniendo en cuenta que el valor por defecto del mínimo es cero y el del máximo es infinito. Si omitimos ambos números, no es necesario incluir la coma. Así, la secuencia para la *coincidencia no codiciosa de 0 o más caracteres* sería `\{-}`.

### Selectores de caracteres

Los corchetes no tienen significado especial, a no ser que delimiten un conjunto de caracteres, dado que en ese caso, ese conjunto de caracteres entre corchetes (`[]`) coincidirá con un solo carácter del texto que sea igual a alguno de los caracteres especificados. Por ejemplo:

`/H[aeu]llo!`

tiene 3 posibles coincidencias: 'Hallo!', 'Hello!' y 'Hullo!'.

Los caracteres dentro de los corchetes no tienen significado especial, con lo que un punto o un asterisco coinciden específicamente con un punto o asterisco del texto. La excepción sería la barra invertida, pues hay que indicarla doblada.

Hay otra excepción: si el primero de los caracteres entre corchetes es el circunflejo (`^`), el significado del conjunto es «todos los caracteres, excepto los aquí indicados». Por ejemplo, para referirnos a todos los caracteres excepto el circunflejo, lo expresaremos como `[^^]`.

También se pueden definir rangos de caracteres. Por ejemplo `[a-j]` se referiría a las letras 'a' hasta la 'j' minúsculas, mientras que `[a-zA-Z0-9]` encontraría una coincidencia con cualquier letra del abecedario, mayúscula o minúscula, así como cualquier cifra numérica. `[^0-8]` se refiere a todos los caracteres, excepto los números del 0 al 8.

Si debemos indicar el carácter guión en sí (`-`), habrá que colocarlo al principio, al final, o precedido por una barra invertida.

### Secuencias especiales

En general, los caracteres sin significado especial pueden expresarse precedidos por el carácter de escape sin que ello tenga efecto alguno en la búsqueda. Sin embargo, algunas secuencias con escape tienen un significado especial en sí mismas:

- `\a` coincide con cualquier carácter alfabético. Equivale a `[a-zA-Z]`.

- `\A` coincide con cualquier carácter no alfabético. Equivale a `[^a-zA-Z]`.

- `\l` coincide con cualquier carácter alfabético en minúscula. Equivale a `[a-z]`.

- `\L` coincide con cualquier carácter que no sea alfabético y en minúscula. Equivale a `[^a-z]`.

- `\u` coincide con cualquier carácter alfabético en mayúscula. Equivale a `[A-Z]`.

- `\U` coincide con cualquier carácter que no sea alfabético y en mayúscula. Equivale a `[^A-Z]`.

- `\d` coincide con cualquier carácter numérico. Equivale a `[0-9]`.

- `\D` coincide con cualquier carácter no numérico. Equivale a `[^0-9]`.

- `\x` coincide con cualquier carácter numérico hexadecimal. Equivale a `[0-9a-fA-F]`.

- `\X` coincide con cualquier carácter no numérico hexadecimal. Equivale a `[^0-9a-fA-F]`.

- `\w` coincide con cualquier carácter alfanumérico o guión bajo. Equivale a `[a-zA-Z0-9_]`.

- `\W` coincide con cualquier carácter no alfanumérico ni guión bajo. Equivale a `[^a-zA-Z0-9_]`.

- `\s` coincide con cualquier carácter blanco (espacio o tabulador).

- `\S` coincide con cualquier carácter no blanco.

- `\t` es utilizado *en el patrón de reemplazo* para insertar un tabulador.

- `\r` es utilizado *en el patrón de reemplazo* para insertar un fin de línea.

- `\<` indica principio de palabra.

- `\>` indica final de palabra.

- Para aceptar una coincidencia entre 2 o más patrones alternativos se usa `\|` entre cada alternativa. Por ejemplo: `/alto\|medio\|bajo` coincidirá con cualquiera de los patrones 'alto', 'medio' o 'bajo'.

Veamos ahora un ejemplo de búsqueda codiciosa en contraposición a una búsqueda no codiciosa. Supongamos que tenemos un texto tal que así:

***Panqueque=leche+huevos+harina+sal+mantequilla.***

Una búsqueda codiciosa:

`/.*[=+]`

coincidirá con 'Panqueque=leche+huevos+harina+sal+', es decir, el mayor conjunto posible de caracteres terminado en '+' o '=' (en este caso, '+'). En cambio la misma búsqueda, pero en su variante no codiciosa:

`/.{-}[=+]`

coincidirá únicamente con 'Panqueque=', es decir, el menor conjunto posible de caracteres terminado en '+' o '=' (ahora '='). En este caso, podríamos repetir la búsqueda, que daría una nueva coincidencia: 'leche+'. En total encontraríamos hasta 5 coincidencias, mientras que en el caso de la búsqueda codiciosa solo se produciría una coincidencia.

### Grupos

Para agrupar partes de una expresión regular, se deben utilizar paréntesis precedidos por el carácter de escape, es decir, `\(` y `\)`. Los fragmentos agrupados así, pueden ser referenciados posteriormente mediante el número de orden que ocupa el grupo dentro de la expresión, precedido por el carácter de escape: `\1`, `\2`, etc. Se pueden utilizar estas referencias tanto en la misma expresión regular (con posterioridad a su aparición) como en el patrón de reemplazo.

Un cuantificador justo después de un paréntesis cerrado se aplicará al conjunto entero entre paréntesis.

### Modificadores

En cualquier punto de la expresión se puede incluir un modificador, que afectará al modo en que los caracteres subsiguientes serán interpretados.

En el caso del modificador `\v`, a partir de su aparición, los cuantificadores deben introducirse sin carácter de escape, así como los caracteres de agrupación (paréntesis), principio y final de palabra (`<`, `>`) o patrones alternativos (`|`). Si utilizamos el carácter de escape, estamos indicando una referencia a dicho carácter:

`/\vhand(y|ful)`

coincidiría tanto con 'handy' como con 'handful'.

Mediante el modificador `\V`, los caracteres `^`, `*`, `.` y `$` pierden su significado especial, pasando a representar el carácter en sí. Si utilizamos este modificador y necesitamos utilizar el significado especial de alguno de estos caracteres, podemos utilizarlos precedidos del carácter de escape.

`/\V^.*$`

coincide con el patrón exacto '^.*$'.

El modificador `\c` indica que la búsqueda debe hacerse sin tener en cuenta mayúsculas y minúsculas. Mediante `\C` ordenamos que se tengan en cuenta las mayúsculas y minúsculas.

*En el patrón de reemplazo*, dado que en ocasiones reemplazaremos por referencias a grupos (`\1`, `\2`, etc.), es posible que nos interese cambiar la capitalización de estas referencias. Para ello, disponemos de `\u` para cambiar el siguiente carácter a mayúsculas. Con `\U` cambiamos los subsiguientes caracteres a mayúsculas. De forma similar, podemos cambiar a minúscula el siguiente (`\l`) o subsiguientes (`\L`) caracteres. En caso de activar el cambio de los subsiguientes caracteres (con `\U` o `\L`), podemos desactivar los cambios con `\e` o `\E`.

Veamos un ejemplo:

`:%s/\v(\a+)/\u\1/g`

pone en mayúscula la primera letra de cada palabra del archivo.
