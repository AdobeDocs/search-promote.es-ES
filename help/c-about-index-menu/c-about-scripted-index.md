---
description: Con Índice de secuencias de comandos puede escribir, actualizar y mantener opciones de indexación incrementales sin necesidad de iniciar sesión. El robot de búsqueda lee instrucciones de un archivo de texto alojado en el servidor.
seo-description: Con Índice de secuencias de comandos puede escribir, actualizar y mantener opciones de indexación incrementales sin necesidad de iniciar sesión. El robot de búsqueda lee instrucciones de un archivo de texto alojado en el servidor.
seo-title: Acerca del índice de secuencias de comandos
solution: Target
subtopic: Scripted Index
title: Acerca del índice de secuencias de comandos
topic: Index,Site search and merchandising
uuid: 51e726ad-414b-4cbd-8a68-fefc3cf9b565
translation-type: tm+mt
source-git-commit: f21a3f7fe0aeaab517a5ca36da43594873b3e69a

---


# Acerca del índice de secuencias de comandos{#about-scripted-index}

Con Índice de secuencias de comandos puede escribir, actualizar y mantener opciones de indexación incrementales sin necesidad de iniciar sesión. El robot de búsqueda lee instrucciones de un archivo de texto alojado en el servidor.

## Uso del índice de secuencias de comandos {#concept_34F58D551BC04BFB8ADC294B9DA9199D}

## Acerca de la configuración de la indexación incremental mediante secuencias de comandos {#section_161D254065E143F3A39F3FC09C400090}

Para utilizar índice con secuencias de comandos, utilice la página Configuración de índice incremental con secuencias de comandos para especificar la URL de un archivo de secuencia de comandos (un archivo de texto sin formato) que se encuentra en el servidor. Por ejemplo, `https://www.mysite.com/indexlist.txt`. A medida que el sitio cambia, puede agregar bloques de comandos al archivo de texto de forma manual o automática (con una secuencia de comandos desencadenada por la llegada de información desde una fuente de noticias, un ticker de bolsa u otro archivo modificado).

Cuando comienza el índice incremental con secuencias de comandos, el robot de búsqueda lee el archivo de texto y ejecuta los nuevos comandos que se encuentran en ese archivo. De forma predeterminada, el robot de búsqueda procesa sólo los nuevos comandos, que están determinados por la fecha del archivo. A menos que compruebe **[!UICONTROL Clear Date]** en el momento de configurar el índice de secuencias de comandos, el robot de búsqueda &quot;recuerda&quot; el especificador de fecha del bloque procesado más recientemente.

## Acerca del archivo de secuencias de comandos {#section_B312E40539F44C6583B4F9637D428E19}

El archivo de secuencias de comandos que se especifica en la URL es un archivo de texto sin formato que se encuentra en el servidor. Puede utilizar retornos de carro, fuentes de línea o ambos para la secuencia final de línea. Una línea en blanco contiene cero o más caracteres de espacio en blanco seguidos de una secuencia final de línea. Todos los comandos no distinguen entre mayúsculas y minúsculas.

El archivo de texto está organizado en bloques que describen la información que utiliza el robot de búsqueda cuando realiza un índice incremental con secuencias de comandos.

Los bloques se ordenan por fecha, con los bloques más antiguos en la parte superior del archivo de texto y los bloques más recientes en la parte inferior. Cada bloque comienza con una sola línea date-command y un comando date-specifier, y termina con un separador de línea en blanco como en el siguiente ejemplo de bloque (entre varios comandos):

Se requiere un cero inicial para todas las fechas ordinales inferiores a la décima cuando se utiliza el estilo HTTP 1.1. Por ejemplo, el 6 de noviembre es el 6 de noviembre, no el 6.

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Comando </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p>date-command </p> </td> 
   <td colname="col2"> <p>La primera línea de cada bloque comienza con uno de los dos comandos de fecha: </p> <p> 
     <ul id="ul_9C1B229B7F1846C490B853FC34989E77"> 
      <li id="li_31FEF1A7163842BDBB0ABE779D07045A"> <span class="codeph"> date </span> <p>Utilice el comando "date" para indicar que el especificador de fecha constará de un día, una fecha, una hora y una zona horaria. </p> </li> 
      <li id="li_0918D5B090014C1A852CB80BB7C2867C"> <span class="codeph">"Segundos"</span> <p>Utilice <span class="codeph"> segundos </span> para indicar que el especificador de fecha constará de una hora en segundos epoch (por ejemplo, 784111777). Cuando utilice <span class="codeph"> segundos </span>, asegúrese de que el número de segundos aumenta entre bloques. </p> </li> 
     </ul> </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>date-specifier </p> </td> 
   <td colname="col2"> <p>El comando <span class="codeph"> date-specifier </span> suele registrar la fecha y hora ordinales (comando date) o la hora en segundos epoch (comando seconds) en que se agregó la información del bloque al archivo. Por ejemplo: </p> <p> <code> date&nbsp;Sun,&nbsp;06&nbsp;Nov&nbsp;1994&nbsp;08:49:37&nbsp;GMT&nbsp;(HTTP&nbsp;1.1&nbsp;style) 
      date&nbsp;Sunday,&nbsp;06-Nov-94&nbsp;08:49:37&nbsp;GMT&nbsp;(HTTP&nbsp;1.0&nbsp;style) 
      date&nbsp;Sun&nbsp;Nov&nbsp;6&nbsp;08:49:37&nbsp;1994&nbsp;(Unix&nbsp;asctime()&nbsp;date&nbsp;style) 
      seconds&nbsp;784111777&nbsp;(Unix&nbsp;epoch-seconds&nbsp;style) </code> </p> <p>Se requiere un cero inicial para todas las fechas ordinales inferiores a la décima cuando se utiliza el estilo HTTP 1.1. Por ejemplo, el 6 de noviembre es el 6 de noviembre, no el 6. </p> <p>El robot de búsqueda "recuerda" el especificador de fecha del bloque procesado más recientemente y sólo indexa la información que considera "más reciente". (El tiempo real no le importa al robot de búsqueda. En cambio, el tiempo en relación con otros tiempos procesados anteriormente es lo que importa). </p> <p>Después de que el robot de búsqueda lea un bloque con un especificador de fecha de 10:00 p.m., por ejemplo, no lee ningún bloque que registre tiempos antes de las 10:00 p.m., independientemente de cuándo se ejecute la operación de índice. En el peor de los casos, podría introducir erróneamente el año "2040" en lugar de "2004" en el especificador de fechas. En tal caso, el robot de búsqueda indexa el bloque 2040 durante la siguiente operación de indexación y luego se niega a leer cualquier otro bloque de información (a menos que uno sea posterior a 2040). Si esto ocurriera, elimine todos los bloques procesados anteriormente del archivo de texto, haga clic en <span class="uicontrol"> Borrar fecha </span>y, a continuación, implemente la acción. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>línea de comentarios </p> </td> 
   <td colname="col2"> <p>Comience las líneas de comentarios con el carácter "#". </p> <p>Cada línea de comentarios debe ser una línea propia; no se pueden escribir comentarios de fin de línea. </p> <p>Una línea de comentarios no se considera una línea en blanco. También puede aparecer en cualquier parte de un bloque, incluso antes de un comando date o seconds, como en el siguiente ejemplo: </p> <p> <code> &nbsp;&nbsp;&nbsp;&nbsp;#Added&nbsp;by&nbsp;Cathy&nbsp;Read&nbsp;after&nbsp;the&nbsp;Y2K&nbsp;seminar 
      &nbsp;&nbsp;&nbsp;&nbsp;date&nbsp;Mon,&nbsp;29&nbsp;Dec&nbsp;1999&nbsp;09:32:20&nbsp;GMT&nbsp; </code> </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>action-command </p> </td> 
   <td colname="col2"> <p>Cada bloque de texto puede contener tantos comandos de acción como desee. Las siguientes opciones de acción-comando corresponden a las de indexación incremental estándar: </p> <p> 
     <ul id="ul_8E1435350A0F416BB8F7826CD3886E74"> 
      <li id="li_22181666628C48A28A6A0BA1F7CA8E77"> 
       <userinput>
         agregar 
       </userinput> <p>Se utiliza con URL. El robot de búsqueda solo indexa las direcciones URL especificadas que han cambiado desde la última operación de indexación. Además, el robot de búsqueda sigue los vínculos contenidos en documentos especificados e indexa sólo los documentos que han cambiado. </p> <p>Puede seguir la dirección URL con 
        <userinput>
          noseguir 
        </userinput> o 
        <userinput>
          noindex 
        </userinput> palabras clave como en el siguiente ejemplo: </p> <p> <code> add&amp;nbsp;https://www.mydomain.com/&amp;nbsp;noindex </code> </p> </li> 
      <li id="li_8E47BF07DB24417083883F5BF40D6B9E"> 
       <userinput>
         update 
       </userinput> <p>Se utiliza con máscara de URL. El robot de búsqueda busca y actualiza todos los documentos que coinciden con la máscara de dirección URL especificada. </p> <p>Puede seguir la dirección URL con 
        <userinput>
          noseguir 
        </userinput> o 
        <userinput>
          noindex 
        </userinput> palabras clave como en el siguiente ejemplo: </p> <p> <code> update&amp;nbsp;https://www.mydomain.com/products/ </code> </p> </li> 
      <li id="li_B3EC8B1670D54F66A1D8411A694EF7E4"> 
       <userinput>
         están 
       </userinput> o 
       <userinput>
         excluir 
       </userinput> <p>Se utiliza con máscara de URL. El robot de búsqueda busca e indexa ("incluir") o ignora ("excluir") documentos según el tipo de máscara especificada. </p> <p>Por ejemplo, </p> <p> <code> include&amp;nbsp;https://www.mydomain.com/products/household/lightbulbs*.html </code> </p> <p>o </p> <p> <code> exclude&amp;nbsp;https://www.mydomain.com/archive/ </code> </p> </li> 
      <li id="li_050B54B735F0475E93806455FA6DC6A5"> 
       <userinput>
         include-date 
       </userinput> o 
       <userinput>
         exclude-date 
       </userinput> <p>Se utiliza con máscara de URL. El robot de búsqueda busca e indexa ("incluir") o ignora ("excluir") documentos basados tanto en la dirección URL como en la fecha de los documentos. Están disponibles los siguientes tipos de máscaras: </p> <p> 
        <ul id="ul_23A15CB492214B86BE84D8E6EA1820AE"> 
         <li id="li_0C7051AC3B5A4C57A3E477F7B6246611"> 
          <userinput>
            include-days NNN 
          </userinput> <p>El robot de búsqueda indexa todos los documentos que coinciden con la máscara URL especificada y que son NNNN días o más antiguos. </p> <p>Puede seguir la máscara de dirección URL con las palabras clave 
           <userinput>
             noseguir 
           </userinput>, 
           <userinput>
             noindex 
           </userinput>y/o 
           <userinput>
             server-date 
           </userinput>. </p> </li> 
         <li id="li_983A10E2ED5D434EA9031F32143F4EF4"> 
          <userinput>
            include-date AAAA-MM-DD 
          </userinput> <p> El robot de búsqueda indexa todos los documentos que coinciden con la máscara de dirección URL especificada y que son tan antiguos o antiguos como la fecha AAAA-MM-DD, donde "AAAA" es el año de 4 dígitos, "MM" es el mes de uno o dos dígitos (1-12) y "DD" es el día de uno o dos dígitos (1-31). </p> <p>Puede seguir la máscara de dirección URL con las palabras clave 
           <userinput>
             noseguir 
           </userinput>, 
           <userinput>
             noindex 
           </userinput>y/o 
           <userinput>
             server-date 
           </userinput>. </p> </li> 
         <li id="li_733CE1B748024CECA7FBE00D7BC7B88A"> 
          <userinput>
            exclude-days NNN 
          </userinput> <p> Deshabilita la indexación de todos los documentos que coinciden con la máscara URL especificada y que son NNN días o más antiguos. </p> <p>Puede seguir la máscara de dirección URL con la palabra clave 
           <userinput>
             server-date 
           </userinput>. </p> </li> 
         <li id="li_90056A0B96CC4DA3854711860A15CE89"> 
          <userinput>
            exclude-date AAAA-MM-DD 
          </userinput> <p>Deshabilita la indexación de todos los documentos que coinciden con la máscara URL especificada y que son anteriores o anteriores a la fecha AAAA-MM-DD. </p> <p>Puede seguir la máscara de dirección URL con la palabra clave 
           <userinput>
             server-date 
           </userinput>. </p> </li> 
        </ul> </p> </li> 
      <li id="li_AA78F22B60FE4535BE73BA87A8992C08"> 
       <userinput>
         delete 
       </userinput> <p>Especifique las direcciones URL. El robot de búsqueda elimina los documentos del índice identificados por la dirección URL. </p> </li> 
      <li id="li_9C63061568AA4D57A4FEBCF6DB9194EC"> 
       <userinput>
         eliminemask 
       </userinput> <p>El robot de búsqueda elimina los documentos del índice que coinciden con la máscara de URL especificada. </p> </li> 
     </ul> </p> </td> 
  </tr> 
 </tbody> 
</table>

Consulte también [Acerca de las máscaras](../c-about-settings-menu/c-about-crawling-menu.md#concept_8039DFC53FF3410AA494D602F71BA164)URL.

## Ejemplo de archivo de secuencia de comandos {#section_9F580F20E7214751B157A28B392BD64E}

En el siguiente ejemplo de archivo de secuencia de comandos, el robot de búsqueda procesa los bloques siempre que los especificadores de fecha posterior a la fecha del especificador de fecha del bloque procesado más recientemente. Si ese es el caso, se producen las siguientes operaciones de indexación:

* Elimina `y2k-problems.html` del índice.
* Agrega `no-y2k-problems.html` al índice de búsqueda y no sigue ninguno de los vínculos de `no-y2k-problems.html`.

* Durante el rastreo, excluya las direcciones URL que coinciden `housewares.htm` y `lightfixtures.htm`l del índice de búsqueda.

* Incluya todos los demás directorios y documentos en `www.mydomain.com`.
* Actualice todos los documentos de los directorios `products` y `information` , rastreando e indexando todos los vínculos subsidiarios que hayan cambiado desde la última operación de indexación.

* Durante el rastreo, excluya las direcciones URL de la sección `archive` del sitio web si tienen fecha el 1 de enero de 1999 o antes.
* Excluya las direcciones URL que coincidan `housewares.html` y `lightfixtures.html` del índice de búsqueda.

* Indique los archivos del `help` directorio, pero no rastree ni indexe ningún vínculo de esos archivos.
* Rastrear e indexar cualquier otro archivo encontrado para `www.mydomain.com`.

```
# Start of file. 
# Added by John Smith 
date Sat, 01 Jan 2004 16:05:53 PST 
exclude https://www.mydomain.com/housewares.html 
exclude https://www.mydomain.com/lightfixtures.html 
include https://www.mydomain.com/ 
delete https://www.mydomain.com/y2k-problems.html 
add https://www.mydomain.com/no-y2k-problems.html nofollow 
 
date Sun, 02 Jan 2004 20:19:08 PST 
# Added by the wire service updater 
exclude-date 1999-01-01 https://www.mydomain.com/archive server-date 
exclude https://www.mydomain.com/housewares.html 
exclude https://www.mydomain.com/lightfixtures.html 
include https://www.mydomain.com/help/ nofollow 
include https://www.mydomain.com/ 
# no add files, just update existing files 
# update all files in the "products" directory 
update https://www.mydomain.com/products/ 
# update all files in the "information" directory 
update regexp ^https://www\.mydomain\.com/information/.*$ 
# End of file.
```

## Configuración de un índice incremental con secuencias de comandos {#task_05AE040FE75E40FFAA5E10B6B6D4D255}

Puede especificar una secuencia de comandos que haya creado y que escriba, actualice y mantenga un índice incremental sin necesidad de iniciar sesión. El robot de búsqueda lee instrucciones del archivo de texto alojado en el servidor para realizar el índice incremental.

**Para configurar un índice incremental con secuencias de comandos**

1. En el menú de producto, haga clic en **[!UICONTROL Index]** > **[!UICONTROL Scripted Index]** > **[!UICONTROL Configuration]**.
1. En la **[!UICONTROL Scripted Incremental Index Configuration]** página, en la **[!UICONTROL Script File URL]**, introduzca la dirección URL del archivo de texto que se encuentra en el servidor.

   Consulte [Acerca del índice](../c-about-index-menu/c-about-scripted-index.md#concept_34F58D551BC04BFB8ADC294B9DA9199D)con secuencias de comandos.
1. (Opcional) Compruebe **[!UICONTROL Clear Date]** si no desea que el robot de búsqueda &quot;recuerde&quot; el especificador de fecha del bloque procesado más recientemente.

   De forma predeterminada, el robot de búsqueda procesa sólo los bloques nuevos de comandos que se encuentran en el archivo de texto, que se determina por la fecha del archivo. Si no desea el valor predeterminado, marque **[!UICONTROL Clear Date]**.
1. Haga clic **[!UICONTROL Save Changes]**.
1. (Opcional) Realice una de las siguientes acciones:

   * Haga clic en **[!UICONTROL History]** para revertir cualquier cambio que haya realizado.

      Consulte [Uso de la opción](../t-using-the-history-option.md#task_70DD3F87A67242BBBD2CB27156F43002)Historial.

   * Haga clic **[!UICONTROL Live]**.

      Consulte [Visualización de la configuración](../c-about-staging.md#task_401A0EBDB5DB4D4CA933CBA7BECDC10F)de lanzamiento.

   * Haga clic **[!UICONTROL Push Live]**.

      Consulte [Inserción de la configuración del escenario en directo](../c-about-staging.md#task_44306783B4C0408AAA58B471DAF2D9A4).

## Configuración del programa de índice incremental con secuencias de comandos para un sitio web activo {#task_B3A87AC4AC784507859C23B9062BA11C}

Puede programar la indexación incremental mediante secuencias de comandos para que se produzca a intervalos regulares durante todo el día.

La hora base que seleccione es local según la zona horaria configurada en Configuración de cuenta.

Consulte [Configuración de la cuenta](../c-about-settings-menu/c-about-account-options-menu.md#task_80A38D0C8E4F453395BD67B81E4B45D9).

Los servidores Web suelen programarse para que no funcionen por mantenimiento a mitad de la noche. Si el servidor está inactivo durante un tiempo de índice programado, el proceso de indexación fallará. Asegúrese de seleccionar una hora del día cuando el servidor web esté disponible.

La programación de índice solo se aplica al índice activo; no se pueden programar índices incrementales escalonados.

**Definición de la programación de índice incremental con secuencias de comandos para un sitio web activo**

1. En el menú de producto, haga clic en **[!UICONTROL Index]** > **[!UICONTROL Scripted Index]** > **[!UICONTROL Live Schedule]**.
1. En la lista desplegable de la **[!UICONTROL Scripted Incremental Index Schedule]** **[!UICONTROL Read the Scripted Incrementally Indexing File]** página, seleccione la frecuencia con la que desea que se ejecute el archivo de texto de índice incremental con secuencias de comandos, en horas o minutos.
1. En la lista **[!UICONTROL Base Time]** desplegable, seleccione la hora de inicio en la que desea volver a generar un nuevo índice incremental con secuencias de comandos.
1. Haga clic **[!UICONTROL Save Changes]**.

## Ejecución de un índice incremental con secuencias de comandos de un sitio web activo o en un sitio web en etapas {#task_6E6FC76EE1E84A5FADB3B67AD7B1DACB}

Puede utilizar el Índice incremental con secuencias de comandos para indexar &quot;partes&quot; del sitio web en vivo o en etapas, como una colección de páginas que se cambian con frecuencia, sin necesidad de iniciar sesión.

Para utilizar esta función, asegúrese de que ha configurado un archivo de texto de índice incremental con secuencias de comandos.

Consulte [Configuración de un índice](../c-about-index-menu/c-about-scripted-index.md#task_05AE040FE75E40FFAA5E10B6B6D4D255)incremental con secuencias de comandos.

**Ejecutar un índice incremental con secuencias de comandos de un sitio web activo o en un sitio web en etapas**

1. En el menú del producto, realice una de las siguientes acciones:

   * Haga clic en **[!UICONTROL Index]** > **[!UICONTROL Scripted Index]** > **[!UICONTROL Live Index]**.
   * Haga clic en **[!UICONTROL Index]** > **[!UICONTROL Scripted Index]** > **[!UICONTROL Staged Index]**.

1. Haga clic **[!UICONTROL Scripted Index Now]**.
1. (Opcional) Si se producen errores de indexación, haga clic en **[!UICONTROL View Errors]** para ver el registro asociado.

## Visualización del registro de índice incremental con secuencias de comandos de un sitio Web activo o en un sitio Web en etapas {#task_CBFCE9B9A87B4DF7A2A35A6E83DE93D7}

Cuando se completa un índice completo de secuencia de comandos activo o un índice escalonado de secuencia de comandos completa, puede ver su registro asociado para solucionar cualquier error que se haya producido.

No puede exportar registros ni guardarlos. Sin embargo, el registro permanece disponible para su visualización hasta que se produzca el nuevo índice.

**Para ver el registro de índice incremental de un sitio Web activo o en etapas**

1. En el menú del producto, realice una de las siguientes acciones:

   * Haga clic en **[!UICONTROL Index]** > **[!UICONTROL Scripted Index]** > **[!UICONTROL Live Log]**.

   * Haga clic en **[!UICONTROL Index]** > **[!UICONTROL Scripted Index]** > **[!UICONTROL Staged Log]**.

1. En la página de registro, en la parte superior o inferior, realice una de las siguientes acciones:

   * Utilice las opciones de navegación **[!UICONTROL First]****[!UICONTROL Prev]**, **[!UICONTROL Next]**, **[!UICONTROL Last]** o **[!UICONTROL Go to line]** para desplazarse por el registro.

   * Utilice las opciones de visualización **[!UICONTROL Errors only]**, **[!UICONTROL Wrap line]** o **[!UICONTROL Show]** para perfeccionar lo que ve.
