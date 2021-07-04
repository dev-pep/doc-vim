# Referencia rápida

En esta sección vamos a mostrar un breve sumario de las funcionalidades que hemos visto hasta ahora. Haremos un rápido repaso de órdenes, comandos y opciones de *Vim*.

Abreviaturas utilizadas:

| Abreviatura | Significado                                                 |
| :---------- | :---------------------------------------------------------- |
| \<mov>      | Movimiento del cursor, objeto de texto o selección de texto |
| \<acc>      | Acción de teclado                                           |
| \<N>        | Número                                                      |
| \<a>, \<A>  | Letra minúscula, mayúscula                                  |
| \<R>        | Carácter que identifica un registro                         |
| \<C>        | Cualquier carácter                                          |

## Órdenes de teclado

### Modo Normal

Movimiento del cursor:

| Orden                | Acción                                                         |
| :------------------- | :------------------------------------------------------------- |
| `h`, `j`, `k`, `l`   | Izquierda, abajo, arriba, derecha                              |
| `<N>|`               | Número de columna                                              |
| `0`, `^`             | Principio, primer blanco de línea                              |
| `$`                  | Fin de línea                                                   |
| `w`, `W`             | Principio siguiente palabra (sin signos, con signos)           |
| `e`, `E`             | Fin de palabra actual o siguiente (sin signos, con signos)     |
| `b`, `B`             | Principio de palabra actual o anterior(sin signos, con signos) |
| `+`, `-`             | Primer no blanco de línea siguiente, anterior                  |
| `C-Fin`              | Último carácter de línea indicada (última por defecto)         |
| `G`                  | Primer no blanco de línea indicada (última por defecto)        |
| `C-Inicio`<br>`gg`   | Primer no blanco de línea indicada (primera por defecto)       |
| `(`, `)`             | Principio, final de frase                                      |
| `{`, `}`             | Principio, final de párrafo                                    |
| `gk`, `gj`           | Línea de pantalla arriba, abajo                                |
| `g0`, `g^`           | Principio, primer blanco de línea de pantalla                  |
| `gm`                 | Centro (horizontal) de pantalla                                |
| `g$`                 | Último carácter de línea de pantalla                           |
| `C-f`, `C-b`         | Avanza, retrocede 1 pantalla                                   |
| `C-d`, `C-u`         | Avanza, retrocede 1/2 pantalla                                 |
| `C-e`, `C-y`         | Desplaza pantalla arriba, abajo                                |
| `H`, `M`, `L`        | Arriba, centro, abajo (pantalla)                               |
| `f<C>`, `F<C>`       | Búsqueda adelante, atrás                                       |
| `t<C>`, `T<C>`       | Búsqueda *'til* adelante, atrás                                |
| `;`, `,`             | Repetir búsqueda igual, al revés                               |
| `n`, `N`             | Repetir búsqueda de patrón igual, al revés                     |
| `[[`, `]]`           | Retrocede, avanza por primeras líneas de secciones             |
| `[]`, `][`           | Retrocede, avanza por últimas líneas de secciones              |
| `[z`, `zj`           | Retrocede, avanza por primeras líneas de pliegues              |
| `zk`, `]z`           | Retrocede, avanza por últimas líneas de pliegues               |
| `<N>%`               | Desplazar al porcentaje indicado                               |
| `%`                  | Desplazar al carácter delimitador correspondiente              |
| `` `<a>``, `` `<A>`` | Ir a una marca local, global                                   |
| `'<a>`, `'<A>`       | Ir a principio de línea que contenga marca local, global       |

Cambio a modo *Insertar*:

| Orden                | Acción                                                   |
| :------------------- | :------------------------------------------------------- |
| `i`, `I`             | Insertar, insertar en primer blanco de línea             |
| `a`, `A`             | *Append*, *append* final de línea                        |
| `c <mov>`            | *Change* fragmento indicado                              |
| `cc`<br>`S`          | *Change* línea actual                                    |
| `C`                  | Equivale a `c$`                                          |
| `o`, `O`             | Nueva línea después, antes                               |
| `s`                  | Cambiar 1 carácter                                       |
| `R`                  | Modo *Reemplazar*                                        |

Varios:

| Orden                | Acción                                                   |
| :------------------- | :------------------------------------------------------- |
| `Esc`                | Cancelar acciones y secuencias a medias                  |
| `d <mov>`            | *Delete* fragmento indicado                              |
| `dd`                 | *Delete* línea actual                                    |
| `D`                  | Equivale a `d$`                                          |
| `x`, `X`             | Eliminar carácter debajo, antes del cursor               |
| `y <mov>`            | *Yank* fragmento indicado                                |
| `yy`<br>`Y`          | *Yank* línea actual                                      |
| `p`, `P`             | *Put* después, antes                                     |
| `"<R><acc>`          | Acción con el registro especificado                      |
| `.`                  | Repetir                                                  |
| `r`                  | Reemplazar un carácter                                   |
| `u`, `U`, `C-r`      | Deshacer, deshacer línea, rehacer                        |
| `~`                  | Cambiar mayúscula/minúscula                              |
| `J`, `gJ`            | Juntar líneas con, sin espacio de separación             |
| `C-g`                | Información de estado                                    |
| `v`                  | Modo *Visual*                                            |
| `V`                  | Modo *Línea Visual*                                      |
| `C-v`                | Modo *Bloque Visual*                                     |
| `q<R>`               | Grabar macro                                             |
| `q`                  | Fin grabación macro                                      |
| `@<R>`               | Ejecutar macro                                           |
| `@@`                 | Ejecutar última macro                                    |
| `m<a>`, `m<A>`       | Definir marca local, global                              |
| `C-i`                | Equivale a `Tab`                                         |
| `C-[`                | Equivale a `Esc`                                         |
| `C-m`                | Equivale a `Intro`                                       |
| `C-h`                | Equivale a retroceso                                     |
| `><mov>`, `<<mov>`   | Aumentar, disminuir indentación de las líneas indicadas  |
| `<<`, `>>`           | Aumentar, disminuir indentación de la línea actual       |
| `=<mov>`             | Recalcular indentación de las líneas indicadas           |
| `==`                 | Recalcular indentación de la línea actual                |

Objetos de texto:

| Orden                | Selección                                                |
| :------------------- | :------------------------------------------------------- |
| `aw`, `iw`           | *A word*, *inner word*                                   |
| `as`, `is`           | *A sentence*, *inner sentence*                           |
| `ap`, `ip`           | *A paragraph*, *inner paragraph*                         |
| `a(`, `i(`           | *A (...) block*, *inner (...) block*                     |
| `a{`, `i{`           | *A {...} block*, *inner {...} block*                     |
| `a[`, `i[`           | *A [...] block*, *inner [...] block*                     |
| `a'`, `i'`           | *A '...' block*, *inner '...' block*                     |
| `a"`, `i"`           | *A "..." block*, *inner "..." block*                     |
| ``a` ``, ``i` ``     | *A \`...\` block*, *inner \`...\` block*                 |
| `at`, `it`           | *A HTML block*, *inner HTML block*                       |
| `a>`, `i>`           | *A \<...\> block*, *inner \<...\> block*                 |

Gestión de ventanas y pestañas:

| Orden                | Acción                                                   |
| :------------------- | :------------------------------------------------------- |
| `C-w s`              | *Split*                                                  |
| `C-w v`              | *Split* vertical                                         |
| `C-w n`              | Nueva ventana                                            |
| `C-w q`              | Cerrar ventana                                           |
| `C-w c`              | Cerrar ventana (no *Vim*)                                |
| `C-w o`              | Cerrar todas menos actual                                |
| `C-w h`, `C-w j`, `C-w k`, `C-w l` | Ir a ventana izquierda, inferior, superior, derecha |
| `C-w`, `C-W`         | Ir a ventana siguiente, anterior                         |
| `C-t`, `C-b`         | Ir a ventana superior izquierda, inferior derecha        |
| `C-p`                | Ir a ventana previa                                      |
| `C-w r`, `C-w R`     | Ir cíclicamente a siguiente, anterior ventana            |
| `C-w x`              | Intercambiar ventana siguiente                           |
| `C-w H`, `C-w J`, `C-w K`, `C-w L` | Ir a ventana más a la izquierda, abajo, arriba, derecha |
| `C-w =`              | Reajustar tamaño ventanas                                |
| `C-w +`, `C-w -`     | Aumentar, disminuir altura                               |
| `C-w <`, `C-w >`     | Aumentar, disminuir anchura                              |
| `C-PgUp`, `C-PgDn`   | Pestaña anterior, siguiente                              |
| `C-w T`              | Ventana a nueva pestaña                                  |

*Folding*:

| Orden                | Acción                                                   |
| :------------------- | :------------------------------------------------------- |
| `zN`, `zn`           | Activar, desactivar *folding*                            |
| `zf<mov>`            | Crear pliegue                                            |
| `zF`                 | Crear pliegue                                            |
| `zo`                 | Abrir pliegue                                            |
| `zc`                 | Cerrar pliegue                                           |
| `za`                 | Conmutar pliegue                                         |
| `zO`                 | Abrir pliegue recursivamente                             |
| `zC`                 | Cerrar pliegue recursivamente                            |
| `zA`                 | Conmutar pliegue recursivamente                          |
| `zM`, `zR`           | Contraer, expandir todos los pliegues                    |
| `zm`, `zr`           | Aumentar, disminuir un nivel de contracción              |
| `zd`                 | Eliminar un pliegue                                      |
| `zD`                 | Eliminar un pliegue recursivamente                       |
| `zE`                 | Eliminar todos los pliegues                              |

Autocompletar:

| Orden                | Fuente                                                   |
| :------------------- | :------------------------------------------------------- |
| `C-x C-n`            | Archivo actual                                           |
| `C-x C-i`            | Archivo actual e incluidos                               |
| `C-x C-l`            | Líneas completas según `complete`                        |
| `C-x C-]`            | Etiquetas                                                |
| `C-x C-f`            | Nombres de archivo                                       |
| `C-n`<br>`C-p`       | Archivo actual según `complete`                          |

### Otros modos

`Esc` vuelve a modo Normal desde cualquier otro modo.

`C-r R` inserta el contenido de un registro en modo *Insertar*.

`C-v` seguido de cualquier combinación de teclas, inserta esa combinación en modo *Insertar*.

`C-n`, `C-p` siguente, anterior en modo *Completar*.

`C-e` cancela modo *Completar* sin elección.

## Comandos

Relación de comandos con sus correspondientes abreviaturas o equivalencias:

| Comando              | Abreviatura (o equivalencia)                             |
| :------------------- | :------------------------------------------------------- |
| `:=`                 |                                                          |
| `:cd`                |                                                          |
| `:change`            | `:c`                                                     |
| `:delete`            | `:d`                                                     |
| `:delmarks`          | `:delm`                                                  |
| `:echo`              | `:ec`                                                    |
| `:edit`              | `:e`                                                     |
| `:fold`              | `:fo`                                                    |
| `:foldclose`         | `:foldc`                                                 |
| `:foldopen`          | `:foldo`                                                 |
| `:global`            | `:g`                                                     |
| `:help`              | `:h`                                                     |
| `:let`               |                                                          |
| `:loadview`          | `:lo`                                                    |
| `:make`              | `:mak`                                                   |
| `:marks`             |                                                          |
| `:mkview`            | `:mkvie`                                                 |
| `:move`              | `:m`                                                     |
| `:pop`               | `:po`                                                    |
| `:pwd`               | `:pw`                                                    |
| `:qa`                | `:q` todos                                               |
| `:quit`              | `:q`                                                     |
| `:read`              | `:r`                                                     |
| `:registers`         | `:reg`                                                   |
| `:set`               | `:se`                                                    |
| `:substitute`        | `:s`                                                     |
| `:syntax`            | `:sy`                                                    |
| `:tag`               | `:ta`                                                    |
| `:version`           | `:ve`                                                    |
| `:wa`                | `:w` todos (con cambios)                                 |
| `:wq`                | `:w` `:q`                                                |
| `:wqa`<br>`:xa`      | `:wa` `:qa`                                              |
| `:write`             | `:w`                                                     |
| `:yank`              | `:y`                                                     |

Mapeo de teclas:

| Comando              | Abreviatura                                              |
| :------------------- | :------------------------------------------------------- |
| `:map`               |                                                          |
| `:map!`              |                                                          |
| `:nmap`              | `:nm`                                                    |
| `:imap`              | `:im`                                                    |
| `:vmap`              | `:vm`                                                    |
| `:smap`              |                                                          |
| `:xmap`              | `:xm`                                                    |
| `:cmap`              | `:cm`                                                    |
| `:omap`              | `:om`                                                    |
| `:unmap`             | `:unm`                                                   |
| `:unmap!`            | `:unm!`                                                  |
| `:nunmap`            | `:nun`                                                   |
| `:iunmap`            | `:iu`                                                    |
| `:vunmap`            | `:vu`                                                    |
| `:sunmap`            | `:sunm`                                                  |
| `:xunmap`            | `:xu`                                                    |
| `:cunmap`            | `:cu`                                                    |
| `:ounmap`            | `:ou`                                                    |
| `:mapclear`          | `:mapc`                                                  |
| `:mapclear!`         | `:mapc!`                                                 |
| `:nmapclear`         | `:nmapc`                                                 |
| `:imapclear`         | `:imapc`                                                 |
| `:vmapclear`         | `:vmapc`                                                 |
| `:smapclear`         | `:smapc`                                                 |
| `:xmapclear`         | `:xmapc`                                                 |
| `:cmapclear`         | `:cmapc`                                                 |
| `:omapclear`         | `:omapc`                                                 |
| `:noremap`           | `:no`                                                    |
| `:noremap!`          | `:no!`                                                   |
| `:nnoremap`          | `:nn`                                                    |
| `:inoremap`          | `:ino`                                                   |
| `:vnoremap`          | `:vn`                                                    |
| `:snoremap`          | `:snor`                                                  |
| `:xnoremap`          | `:xn`                                                    |
| `:cnoremap`          | `:cno`                                                   |
| `:onoremap`          | `:ono`                                                   |

Ventanas, pestañas y *buffers*:

| Comando              | Abreviatura                                              |
| :------------------- | :------------------------------------------------------- |
| `:ls`                |                                                          |
| `:files`             |                                                          |
| `:buffers`           |                                                          |
| `:buffer`            | `:b`                                                     |
| `:bnext`             | `:bn`                                                    |
| `:bNext`             | `:bN`                                                    |
| `:bfirst`            | `:bf`                                                    |
| `:blast`             | `:bl`                                                    |
| `:argument`          | `:argu`                                                  |
| `:next`              | `:n`                                                     |
| `:Next`              | `:N`                                                     |
| `:first`             | `:fir`                                                   |
| `:last`              | `:la`                                                    |
| `:hide`              | `:hid`                                                   |
| `:tabnew`            |                                                          |
| `:tabclose`          | `:tabc`                                                  |
| `:tabonly`           | `:tabo`                                                  |
| `:split`             | `:sp`                                                    |
| `:vsplit`            | `:vs`                                                    |
| `:new`               |                                                          |
| `:vnew`              | `:vne`                                                   |

## Opciones

Relación de opciones con sus correspondientes abreviaturas:

| Opción               | Abreviatura                                              |
| :------------------- | :------------------------------------------------------- |
| `autoindent`         | `ai`                                                     |
| `cindent`            | `cin`                                                    |
| `complete`           | `cpt`                                                    |
| `display`            | `dy`                                                     |
| `expandtab`          | `et`                                                     |
| `foldenable`         | `fen`                                                    |
| `foldlevel`          | `fdl`                                                    |
| `foldmethod`         | `fdm`                                                    |
| `hidden`             | `hid`                                                    |
| `hlsearch`           | `hls`                                                    |
| `ignorecase`         | `ic`                                                     |
| `include`            | `inc`                                                    |
| `incsearch`          | `is`                                                     |
| `indentexpr`         | `inde`                                                   |
| `list`               |                                                          |
| `number`             | `nu`                                                     |
| `paste`              |                                                          |
| `relativenumber`     | `rnu`                                                    |
| `ruler`              | `ru`                                                     |
| `shiftwidth`         | `sw`                                                     |
| `smartindent`        | `si`                                                     |
| `smarttab`           | `sta`                                                    |
| `softtabstop`        | `sts`                                                    |
| `tabstop`            | `ts`                                                     |
| `textwidth`          | `tw`                                                     |
| `wrap`               |                                                          |
| `wrapmargin`         | `wm`                                                     |
