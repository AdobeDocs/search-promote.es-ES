---
description: Un repaso sobre la sintaxis y las reglas de construcción de expresiones regulares.
seo-description: Un repaso sobre la sintaxis y las reglas de construcción de expresiones regulares.
seo-title: Expresiones regulares
solution: Target
title: Expresiones regulares
topic: Appendices,Site search and merchandising
uuid: 369b54f6-372a-41de-bb5d-3ae0bd640199
translation-type: tm+mt
source-git-commit: ef818327e1cdaad79ac47575a8dfba1de3dc5c2e

---


# Regular Expressions{#regular-expressions}

Un repaso sobre la sintaxis y las reglas de construcción de expresiones regulares.

Consulte también [Configuración de un índice incremental de un sitio Web](../c-about-index-menu/c-about-incremental-index.md#task_46A367B0786C4C90BFFA5D3F95FD86C0)escalonado.

**Sintaxis de las expresiones regulares**

<table> 
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <b>Texto</b> </p> </td> 
   <td colname="col2"> </td> 
   <td colname="col3"> <p>Cualquier carácter </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> </td> 
   <td colname="col2"> <p> [caracteres] </p> </td> 
   <td colname="col3"> <p> Clase de caracteres: Uno de los caracteres </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> </td> 
   <td colname="col2"> <p> [^chars] </p> </td> 
   <td colname="col3"> <p>Clase de caracteres: Ninguno de los caracteres </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> </td> 
   <td colname="col2"> <p> text1|text2 </p> </td> 
   <td colname="col3"> <p> Alternativa: text1 o text2 </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <b>Cuantificadores</b> </p> </td> 
   <td colname="col2"> </td> 
   <td colname="col3"> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> </td> 
   <td colname="col2"> <p> ? </p> </td> 
   <td colname="col3"> <p> 0 o 1 del texto anterior </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> </td> 
   <td colname="col2"> <p> * </p> </td> 
   <td colname="col3"> <p> 0 o N del texto anterior (N &gt; 1) </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> </td> 
   <td colname="col2"> <p> + </p> </td> 
   <td colname="col3"> <p>1 o N del texto anterior (N &gt; 1) </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <b>Grupo</b> </p> </td> 
   <td colname="col2"> </td> 
   <td colname="col3"> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> </td> 
   <td colname="col2"> <p> (text) </p> </td> 
   <td colname="col3"> <p> Agrupación de texto, ya sea para establecer los bordes de una alternativa o para volver a hacer referencias donde se utiliza el grupo Nth en el RHS de una regla de reescritura con $N) </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <b>Anclajes</b> </p> </td> 
   <td colname="col2"> </td> 
   <td colname="col3"> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> </td> 
   <td colname="col2"> <p> ^ </p> </td> 
   <td colname="col3"> <p> Inicio de anclaje de línea. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> </td> 
   <td colname="col2"> <p> $ </p> </td> 
   <td colname="col3"> <p> Anclaje final de línea. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <b>Escapar</b> </p> </td> 
   <td colname="col2"> </td> 
   <td colname="col3"> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> </p> </td> 
   <td colname="col2"> <p>\char </p> </td> 
   <td colname="col3"> <p>Escapar del carácter concreto. Por ejemplo, para especificar los caracteres ".[]()" etc. </p> </td> 
  </tr> 
 </tbody> 
</table>

**Reglas sobre expresiones regulares**

* Un carácter normal (no uno de los caracteres especiales descritos a continuación) es una expresión regular de un carácter que se ajusta a sí misma.
* Una barra invertida (\) seguida de cualquier carácter especial es una expresión regular de un carácter que coincide con el carácter especial en sí. Los caracteres especiales incluyen lo siguiente:

   * `.` (punto), `*` (asterisco), `?` (signo de interrogación), `+` (signo más), `[` (corchete izquierdo), `|` (barra vertical) y `\` (barra invertida) son siempre caracteres especiales, excepto cuando aparecen entre corchetes.
   * `^` (circunflejo o acento circunflejo) es especial al principio de una expresión regular, o cuando inmediatamente sigue a la izquierda de un par de corchetes.
   * `$` (signo de dólar) es especial al final de una expresión regular.
   * `.` (punto) es una expresión regular de un carácter que coincide con cualquier carácter, incluidos los caracteres de conjunto de códigos complementarios, a excepción de la nueva línea.
   * Una cadena de caracteres no vacía entre corchetes `[ ]` (corchetes izquierdo y derecho) es una expresión regular de un carácter que coincide con un carácter, incluidos los caracteres de conjunto de códigos complementarios, en esa cadena.

      Sin embargo, si el primer carácter de la cadena es un `^` (circunflejo), la expresión regular de un carácter coincide con cualquier carácter, incluidos los caracteres de conjunto de códigos complementarios, con excepción de la nueva línea y los caracteres restantes de la cadena.

      El `^` tiene este significado especial solo si se produce primero en la cadena. Puede utilizar `-` (signo menos) para indicar un rango de caracteres consecutivos, incluidos los caracteres de conjunto de códigos complementarios. Por ejemplo, [0-9] equivale a [0123456789].

      Los caracteres que especifican el rango deben pertenecer al mismo conjunto de códigos. Cuando los caracteres proceden de distintos conjuntos de códigos, se hace coincidir uno de los caracteres que especifican el rango. El `-` pierde este significado especial si se produce primero (después de un `^`primer, si es que se produce alguno) o último en la cadena. La `]` (corchete derecho) no termina tal cadena cuando es el primer carácter dentro de ella, después de una inicial `^`, si existe. Por ejemplo, []a-f] coincide con un `]` (corchete derecho) o con una de las letras ASCII de a a a f inclusive. Los cuatro caracteres enumerados como caracteres especiales arriba se representan dentro de una cadena de caracteres.

**Reglas para construir expresiones regulares desde expresiones regulares de un carácter**

Puede utilizar las siguientes reglas para construir expresiones regulares a partir de expresiones regulares de un carácter:

* Una expresión regular de un carácter es una expresión regular que coincide con cualquier expresión regular de un carácter.
* Una expresión regular de un carácter seguida de un `*` (asterisco) es una expresión regular que coincide con cero o más incidencias de la expresión regular de un carácter, que puede ser un carácter de conjunto de códigos complementario. Si hay alguna opción, se elige la cadena más larga a la izquierda que permite una coincidencia.
* Una expresión regular de un carácter seguida de un `?` (signo de interrogación) es una expresión regular que coincide con cero o con una aparición de la expresión regular de un carácter, que puede ser un carácter de conjunto de códigos complementario. Si hay alguna opción, se elige la cadena más larga a la izquierda que permite una coincidencia.
* Una expresión regular de un carácter seguida de un signo `+` (signo más) es una expresión regular que coincide con una o más incidencias de la expresión regular de un carácter, que puede ser un carácter de conjunto de códigos complementario. Si hay alguna opción, se elige la cadena más larga a la izquierda que permite una coincidencia.
* Una expresión regular de un carácter seguida de `{m}`, `{m,}`o `{m,n}` es una expresión regular que coincide con un rango de incidencias de la expresión regular de un carácter. Los valores de m y n deben ser enteros no negativos inferiores a 256; `{m}` coincide exactamente con m ocurrencias; `{m,}` coincide al menos con m incidencias; `{m,n}` coincide con cualquier número de incidencias entre m y n inclusive. Siempre que existe una opción, la expresión regular coincide con tantas incidencias como sea posible.
* La concatenación de expresiones regulares es una expresión regular que coincide con la concatenación de las cadenas coincidentes con cada componente de la expresión regular.
* Una expresión regular delimitada entre las secuencias de caracteres ( y ) es una expresión regular que coincide con cualquier expresión regular sin adornar que coincida.
* Una expresión regular seguida de una `|` (barra vertical) seguida de una expresión regular es una expresión regular que coincide con la primera expresión regular (antes de la barra vertical) o con la segunda expresión regular (después de la barra vertical).

También puede restringir una expresión regular para que coincida únicamente con un segmento inicial o final de una línea, o ambos.

* Un `^` (circunflejo) al principio de una expresión regular restringe esa expresión regular para que coincida con un segmento inicial de una línea.
* Un `$` (signo de dólar) al final de toda una expresión regular restringe esa expresión regular para que coincida con un segmento final de una línea.
* La expresión ^regular$ de construcción restringe la expresión regular para que coincida con toda la línea.

Hay algunos nombres de clase de caracteres predefinidos que puede utilizar en lugar de expresiones regulares complejas entre corchetes. Por ejemplo, un dígito puede representarse con la expresión regular de un carácter [0-9] o con la expresión regular de un carácter de la clase de caracteres [[:digit:]].

Las clases de caracteres predefinidas y sus significados son los siguientes:

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Clase de caracteres </p> </th> 
   <th colname="col2" class="entry"> <p>Significado </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> [[:alnum:]] </p> </td> 
   <td colname="col2"> <p> Un carácter alfabético o un dígito. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> [[:alpha:]] </p> </td> 
   <td colname="col2"> <p>Carácter alfabético. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> [[:en blanco:]] </p> </td> 
   <td colname="col2"> <p>Un espacio o una ficha. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> [[:cntrl:]] </p> </td> 
   <td colname="col2"> <p> Un código de control; carácter no imprimible. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> [[:digit:]] </p> </td> 
   <td colname="col2"> <p>Un dígito. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> [[:graph:]] </p> </td> 
   <td colname="col2"> <p> Cualquier carácter de impresión excepto el espacio. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> [[:lower:]] </p> </td> 
   <td colname="col2"> <p>Carácter alfabético en minúscula. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> [[:print:]] </p> </td> 
   <td colname="col2"> <p> Cualquier carácter de impresión, incluido el espacio. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> [[:punct:]] </p> </td> 
   <td colname="col2"> <p> Puntuación. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> [[:espacio:]] </p> </td> 
   <td colname="col2"> <p> Espacio en blanco, como un espacio, una ficha o un final de línea. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> [[:top:]] </p> </td> 
   <td colname="col2"> <p> Carácter alfabético en mayúsculas. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> [[:xdigit:]] </p> </td> 
   <td colname="col2"> <p> Un dígito hexadecimal, mayúscula o minúscula. </p> </td> 
  </tr> 
 </tbody> 
</table>

Dos nombres de clase de caracteres especiales coinciden con el espacio nulo al principio y al final de una palabra. En otras palabras, no coinciden con un carácter real. Una palabra se considera cualquier secuencia de caracteres alfabéticos, dígitos o caracteres de subrayado (_).

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Clase de caracteres </p> </th> 
   <th colname="col2" class="entry"> <p>Significado </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> [[:&lt;:]] </p> </td> 
   <td colname="col2"> <p> inicio de una palabra </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> [[:&gt;:]] </p> </td> 
   <td colname="col2"> <p>final de una palabra </p> </td> 
  </tr> 
 </tbody> 
</table>
