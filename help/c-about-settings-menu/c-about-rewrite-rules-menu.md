---
description: Utilice el menú Reglas de reescritura para establecer las reglas de rastreo y búsqueda de URL y título.
solution: Target
subtopic: Rewrite Rules
title: Acerca del menú Reescribir reglas
topic-legacy: Settings,Site search and merchandising
uuid: 77ee84dd-fdba-4d34-ae8e-2fe786599800
exl-id: cff17ead-6997-4ff6-a995-7ca020b06d50
translation-type: tm+mt
source-git-commit: aa095add9eb656913792b3f14001dda66cdd7d67
workflow-type: tm+mt
source-wordcount: '10178'
ht-degree: 0%

---

# Acerca del menú Reescribir reglas {#about-the-rewrite-rules-menu}

Utilice el menú Reglas de reescritura para establecer las reglas de rastreo y búsqueda de URL y título.

## Acerca de las reglas de URL del almacén de listas de rastreo {#concept_B71CF4C8030A4A74A22C3BFE4DE3B865}

Las reglas de rastreo de URL especifican cómo se reescriben las direcciones URL que encuentra dentro del contenido web. Puede especificar un número ilimitado de reglas y condiciones, y puede manipular cualquier parte de las direcciones URL encontradas.

<!-- 

c_about_crawl_list_store_url_rules.xml

 -->

Las reglas de rastreo son más útiles para reescribir partes dinámicas de una URL, como un identificador de sesión que es único para cada cliente que visita el sitio web. También puede utilizar reglas de reescritura para ocultar partes de una URL, como parámetros de consulta, del robot de búsqueda. De forma predeterminada, no se especifican reglas y no se reescribe la dirección URL.

A medida que se rastrea un sitio web, las direcciones URL de contenido incrustado se almacenan en una lista temporal de páginas web adicionales para rastrear. Antes de añadir una dirección URL a esta lista, se le aplican las reglas de reescritura de la tienda. Normalmente, las reglas de reescritura del almacén se utilizan para eliminar un ID de sesión de una dirección URL o para aplicar un ID de sesión específico para el rastreo. Cuando el robot de búsqueda recupera una dirección URL de la lista, las reglas de reescritura Recuperar se utilizan para volver a manipular partes de esa dirección URL. Normalmente, las reglas de recuperación se utilizan para insertar datos con distinción de tiempo en la dirección URL. Es esta URL final la que se utiliza para recuperar la página de su sitio web.

Consulte [Acerca de las reglas de recuperación de listas arrastradas](../c-about-settings-menu/c-about-rewrite-rules-menu.md#concept_EC8E2E48B99A458D8567B526C9827CBA).

Normalmente, las reglas de URL de almacenamiento se utilizan exclusivamente. Recuperar reglas de URL solo es necesario si las direcciones URL contienen datos dinámicos, como un ID de sesión, y si esos datos dinámicos cambian con el tiempo para seguir siendo válidos. En este caso, utilice las reglas de URL de la tienda para obtener el estado más reciente de los datos de las direcciones URL encontradas. A continuación, utilice Recuperar reglas de URL para agregar esos datos a cada URL cuando el robot de búsqueda intente recuperar la página.

Cada regla se especifica con una regla de reescritura (RewriteRule) directiva y una o más condiciones de reescritura opcionales (RewriteCond). El orden de las reglas es importante. El conjunto de reglas pasa de un bucle a otra regla. Cuando una regla coincide, recorre en bucle las condiciones de reescritura correspondientes. Se especifica una regla de URL de rastreo de la siguiente manera:

```
RewriteCond TestString CondPattern [Flags] 
RewriteRule Pattern Substitution [Flags]
```

Cuando se encuentra una URL incrustada, el robot de búsqueda intenta hacer coincidir la URL con el Patrón de cada regla de rastreo. Si el patrón coincide, el motor de reescritura busca las directivas de RewriteCond correspondientes. Si no hay condiciones, la URL se sustituye por un nuevo valor construido a partir de la cadena de sustitución y continúa con la siguiente regla en el conjunto de reglas. Si existen condiciones, se procesan en el orden en que aparecen en la lista. El motor de reescritura intenta hacer coincidir un patrón de condición (CondPattern) con una cadena de prueba (TestString). Si las dos coinciden, la siguiente condición se procesa hasta que no haya más condiciones disponibles. Si todas las condiciones coinciden, la dirección URL se sustituye por la sustitución especificada en la regla. Si no se cumple la condición, falla el conjunto completo de condiciones y la regla correspondiente.

## Acerca de las directivas RewriteRule {#section_162122340BB34F12BB9A36DC9349092B}

Una directiva RewriteRule tiene la siguiente forma:

```
           
<i>RewriteRule Pattern Substitution [Flags]</i> 
        
```

`Pattern` puede ser una expresión regular POSIX, que se aplica a la dirección URL actual. La &quot;dirección URL actual&quot; puede diferir de la dirección URL solicitada original, ya que es posible que las reglas anteriores ya hayan coincidido y modificado la dirección URL.

Consulte [Expresiones regulares](../c-appendices/r-regular-expressions.md#reference_B5BA7D61D82E4109A01D2A2D964E3A6A).

No se puede usar el carácter &quot;no&quot; (&#39;!&#39;) para prefijar el patrón. El carácter &quot;not&quot; permite negar un patrón, es decir, que sea verdadero solo si la dirección URL actual NO coincide con este patrón. El carácter &quot;not&quot; se puede usar cuando es mejor que coincida con un patrón negativo o como regla predeterminada final.

>[!NOTE]
>
>No se pueden usar tanto el carácter &quot;no&quot; como los comodines agrupados en un patrón. Además, no se puede utilizar un patrón denegado cuando la cadena de sustitución contiene $N.

Puede utilizar paréntesis para crear una referencia secundaria en el patrón, al que se puede hacer referencia mediante Substitution y CondPattern.

**** SustituciónLa URL se reemplaza por la cadena de sustitución que contiene lo siguiente:

Texto sin formato: Texto que se pasa sin cambiar.

Las referencias de fondo proporcionan acceso a las partes agrupadas (entre paréntesis) del Patrón o CondPattern. Los siguientes son los dos tipos de referencias invertidas:

* **RewriteRule** BackReferencesEstas referencias se corresponden con las del patrón de regla de reescritura correspondiente y toman el formato $N (0)  &lt;> Por ejemplo, `RewriteRule ^https:// ([^/]*) (.*)$ https://${tolower: $1} $2.`

* **RewriteCond** BackReferencesEstas referencias coincidentes en el último CondPattern coincidente de RewriteCond y toman la forma %N (0  &lt;>

Variables: Son variables con la forma %{NAME_OF_VARIABLE} donde NAME_OF_VARIABLE es una cadena para el nombre de una variable definida. Consulte el indicador `*[E]*` para obtener más información sobre la configuración de variables de entorno.

Funciones: Estas son funciones del formulario ${NAME_OF_FUNCTION:key}, donde NAME_OF_FUNCTION es el siguiente:

* hace que todos los caracteres de *key* estén en minúsculas.
* toupper escribe todos los caracteres en *key* mayúscula.
* escape URL-codifica todos los caracteres de *key*.
* Los caracteres &quot;a&quot;...z&#39;, &#39;A&#39;..&#39;Z&#39;, &#39;0&#39;...9&#39;, &#39;*&#39;, &#39;-&#39;, &#39;.&#39;, &#39;/&#39;, &#39;@&#39; y &#39;_&#39; no se han modificado; los espacios se traducen a &#39;+&#39; y todos los demás caracteres se transforman en su equivalente con codificación URL %xx.
* unescape transforma &#39;+&#39; de nuevo en espacio y todos los caracteres con codificación URL de %xx de nuevo en caracteres únicos.

>[!NOTE]
>
>Hay una cadena de sustitución especial: `'-'` significa &quot;NO substitution&quot;. La cadena `'-'` se utiliza a menudo con el indicador C (cadena), lo que permite hacer coincidir una dirección URL con varios patrones antes de que se produzca una sustitución.

**Indicadores**

(opcional) Incluya los indicadores entre corchetes `[]`. Los indicadores múltiples están separados por coma.

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Indicador </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> 'last|L' </p> </td> 
   <td colname="col2"> <p>Última regla. </p> <p>Detiene el proceso de reescritura y no aplica ninguna regla de reescritura adicional. Utilice este indicador para evitar que se siga procesando la dirección URL actual. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'next|N' </p> </td> 
   <td colname="col2"> <p>La siguiente ronda. </p> <p> Vuelve a ejecutar el proceso de reescritura (comenzando de nuevo con la primera regla de reescritura) utilizando la URL de la última regla de reescritura (no la URL original). ¡Ten cuidado de no crear un bucle muerto! </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'chain|C' </p> </td> 
   <td colname="col2"> <p>Encadenado con la regla siguiente. </p> <p>Encadena la regla actual con la siguiente regla (que también se puede encadenar con la regla siguiente, etc.). Si una regla coincide, el proceso de sustitución continúa como de costumbre. Si la regla no coincide, se omiten todas las reglas encadenadas subsiguientes. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'nocase|NC' </p> </td> 
   <td colname="col2"> <p>No hay caso. </p> <p> Hace que el Patrón no distinga mayúsculas de minúsculas (es decir, no hay diferencia entre "A-Z" y "a-z") cuando el patrón coincide con la dirección URL actual. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'skip|S=num' </p> </td> 
   <td colname="col2"> <p>Omita la regla o las reglas siguientes. </p> <p> Si la regla actual coincide, este indicador obliga al motor de reescritura a omitir las siguientes reglas de núm del conjunto de reglas. Utilice este indicador para realizar construcciones pseudo if-then-else. La última regla de la cláusula then se convierte en un skip=N donde N es el número de reglas de la cláusula else. </p> <p> <p>Nota:  ¡Esta bandera no es la misma que la bandera 'chain|C'!) </p> </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'env|E=VAR:VAL' </p> </td> 
   <td colname="col2"> <p>Establece la variable ambiental. </p> <p>Crea una variable de entorno "VAR" configurada en el valor VAL, donde VAL puede contener referencias atrás de expresiones regulares, $N y %N, que se expanden. Puede utilizar este indicador más de una vez para configurar varias variables. Posteriormente se puede anular la referencia a las variables en un patrón RewriteCond siguiente a través de %{VAR}. </p> <p>Utilice este indicador para eliminar y recordar información de direcciones URL. </p> </td> 
  </tr> 
 </tbody> 
</table>

>[!NOTE]
>
>Las reglas de reescritura de almacén y las reglas de reescritura de recuperación comparten valores de variables. Debido a este comportamiento, puede establecer una variable en un valor sessionid que distinga el tiempo cuando se encuentre y almacene una URL incrustada. Cuando se recupera la siguiente dirección URL de la lista de almacenamiento temporal, se puede añadir el valor más reciente de sessionid antes de recuperar la página.

**Ejemplo de una regla de reescritura con una función**

Supongamos que tiene un servidor que distingue entre mayúsculas y minúsculas y que gestiona las cadenas `"www.mydomain.com"` y `"www.MyDomain.com"` de forma diferente. Para que el servidor funcione correctamente, asegúrese de que el dominio sea siempre `"www.mydomain.com"` aunque algunos documentos contengan vínculos que hagan referencia a `"www.MyDomain.com."` Para ello, puede utilizar la siguiente regla:

```
RewriteRule  ^https:// 
<b>([^/]*)</b> 
<i>(.*)</i>$  https://${tolower:$1}$2
```

Esta regla de reescritura utiliza la función `tolower` para reescribir la parte de dominio de una URL y asegurarse de que siempre esté en minúsculas, como en el siguiente ejemplo:

1. El patrón `(^https://([^/]*)(.*)$)` contiene una referencia `([^/]*)` que coincide con todos los caracteres entre `https://` y el primer `/` de la dirección URL. El patrón también contiene una segunda referencia `(.*)` que coincide con todos los caracteres restantes de la dirección URL.

1. La sustitución `(https://${tolower:$1}$2)` indica al motor de búsqueda que vuelva a escribir la dirección URL utilizando la función `tolower` en la primera referencia `(https:// ${tolower:$1}$2)` sin modificar el resto de la dirección URL `(https://${tolower:$1} $2)`.

Por lo tanto, una dirección URL del formulario `https://www.MyDomain.com/INTRO/index.Html` se reescribe como `https://www.mydomain.com/INTRO/index.Html`.

## Acerca de las directivas RewriteCond {#section_CD5A19B2D3204F73B645411931FC34A1}

La directiva RewriteCond define una condición de regla. Cuando RewriteCond precede a una regla de reescritura, la regla solo se utiliza si su patrón coincide con el título actual y se aplican las condiciones adicionales. Las condiciones de reescritura toman la siguiente forma:

```
           
<i>RewriteCond TestString CondPattern [Flags]</i> 
        
```

`TestString` es una cadena que puede contener las siguientes construcciones:

Texto sin formato: Texto que se pasa sin cambiar.

Las referencias de fondo proporcionan acceso a las partes agrupadas (entre paréntesis) del Patrón o CondPattern. Los siguientes son los dos tipos de referencias invertidas:

* **RewriteRule** BackReferencesEstas referencias se corresponden con las del patrón de regla de reescritura correspondiente y toman el formato $N (0)  &lt;> Por ejemplo, `RewriteRule ^https:// ([^/]*) (.*)$ https://${tolower: $1} $2`.

* **RewriteCond** BackReferencesEstas referencias coincidentes en el último CondPattern coincidente de RewriteCond y toman la forma %N (0&lt;>

Variables: Son variables con la forma %{NAME_OF_VARIABLE} donde NAME_OF_VARIABLE puede ser una cadena para el nombre de una variable definida. Consulte el indicador RewriteRule *`[E]`* para obtener más información sobre la configuración de variables.

Funciones: Estas son funciones del formulario ${NAME_OF_FUNCTION:key}, donde NAME_OF_FUNCTION es el siguiente:

* hace que todos los caracteres de *key* estén en minúsculas.
* toupper escribe todos los caracteres en *key* mayúscula.
* escape URL-codifica todos los caracteres en clave. Los caracteres &quot;a&quot;...z&#39;, &#39;A&#39;..&#39;Z&#39;, &#39;0&#39;...9&#39;, &#39;*&#39;, &#39;-&#39;, &#39;.&#39;, &#39;/&#39;, &#39;@&#39; y &#39;_&#39; se dejan sin cambios, los espacios se traducen en &#39;+&#39; y todos los demás caracteres se transforman en su equivalente con codificación de URL `%xx`.
* unescape transforma &#39;+&#39; en espacio y todos los caracteres de codificación de URL `%xx` vuelven a codificar en caracteres únicos.

**** CondPatternis es una expresión regular extendida estándar con algunas adiciones. A la cadena de patrón se le puede añadir el prefijo `!` (signo de exclamación) para especificar un patrón no coincidente. En lugar de cadenas de expresión regular reales, puede utilizar una de las siguientes variantes especiales:

>[!NOTE]
>
>También puede prefijar todas estas pruebas con un signo de exclamación (&#39;!&#39;) para negar su significado.

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Cadena CondPattern </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> '&lt;CondPattern' </p> </td> 
   <td colname="col2"> <p>Lexicamente menos. </p> <p> Trata el CondPattern como una cadena sin formato y lo compara lexicamente con TestString. True si TestString es lexicamente menor que CondPattern. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> '&gt;CondPattern' </p> </td> 
   <td colname="col2"> <p>Bueno Lexicamente. </p> <p> Trata el CondPattern como una cadena sin formato y lo compara lexicamente con TestString. True si TestString es lexicamente bueno que CondPattern. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> '=CondPattern' </p> </td> 
   <td colname="col2"> <p>Lexicamente igual. </p> <p> Trata el CondPattern como una cadena sin formato y lo compara lexicamente con TestString. True si TestString es lexicamente igual a CondPattern, es decir, las dos cadenas son exactamente iguales (carácter por carácter). Si CondPattern es solo "" (dos comillas), se compara TestString con la cadena vacía. </p> </td> 
  </tr> 
 </tbody> 
</table>

**Indicadores**
 (opcional) Incluir indicadores entre corchetes  `[]`. Los indicadores múltiples están separados por coma.

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Indicador </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> 'nocase|NC' </p> </td> 
   <td colname="col2"> <p> No hay caso. </p> <p>Este indicador hace que la prueba no distinga entre mayúsculas y minúsculas, es decir, no hay diferencia entre "A-Z" y "a-z" tanto en la cadena de prueba expandida como en CondPattern. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'ornext|OR' </p> </td> 
   <td colname="col2"> <p>O condición siguiente. </p> <p>Utilice este indicador para combinar las condiciones de regla con una condición OR local en lugar de la condición Y implícita. Sin este indicador, tendría que escribir el cond/regla varias veces. </p> </td> 
  </tr> 
 </tbody> 
</table>

**Ejemplo**

Algunas páginas web asignan una variable CGI &quot;sessionid&quot; la primera vez que un visitante llega a un sitio. Esta variable se utiliza para identificar al visitante y, a medida que el visitante navega por el sitio, se pasa la variable . Dado que el robot de búsqueda se parece a un visitante del sitio, se le asigna un número &quot;sessionid&quot;. El robot de búsqueda mantiene este único valor &quot;sessionid&quot;, incluso si una segunda página del sitio intenta asignar un nuevo valor. Para garantizar esto, necesita dos reglas de reescritura.

La primera regla se utiliza para identificar y almacenar la variable sessionid :

```
RewriteCond  %{sessionid}  !.+ 
RewriteRule  ^.+sessionid= 
<b>([^&#]+)</b>.*$  -   
<i>[E=sessionid:$1]</i>
```

La regla de reescritura utiliza un indicador E `([E=sessionid:$1])` para asignar el valor actual del parámetro CGI de sessionid a la variable `sessionid`. El `$1` hace referencia a la primera referencia secundaria, que se encuentra entre el primer conjunto de paréntesis del patrón `([^&#]+)` de RewriteRule.

La expresión regular `^&#]+` coincide con la parte de una dirección URL entre la palabra `sessionid` y el siguiente carácter `**&**or**#**`. Dado que esta regla de reescritura solo se utiliza para crear el valor inicial de la variable sessionid, no se reescribe. Tenga en cuenta que el campo Sustitución de la regla está establecido en `-` para indicar que no es necesario reescribir.

RewriteCond examina la variable `sessionid` ( `%{sessionid}`). Si ni siquiera tiene un solo carácter (!.+), entonces la regla de reescritura coincide.

Con esta regla, la dirección URL se lee como `https://www.domain.com/home/?sessionid=1234&function=start` y asigna el valor `1234` a la variable `sessionid`.

La segunda regla se usa para reescribir todas las direcciones URL que coincidan con el siguiente patrón de regla de reescritura:

```
RewriteRule   
<b>^(.+)</b>sessionid=[^&#]+ 
<i>(.*)$</i>  $1sessionid=%{sessionid}$2
```

El patrón de regla de reescritura contiene dos referencias atrás: `(.+)` y `(.*)`. La primera referencia inversa coincide con todos los caracteres anteriores a `sessionid`. La segunda referencia backreference coincide con todos los caracteres después de la `&` o `#` terminadora.

El patrón de sustitución reescribe la dirección URL utilizando la primera referencia inversa, seguida de la cadena &quot;sessionid=&quot;, seguida del valor de la variable de ID de sesión definida por la primera regla `%{sessionid}`, seguida de la segunda referencia inversa. `($1sessionid=%{sessionid} $2)`

Observe que esta regla de reescritura no contiene un objeto RewriteCond. Como tal, provoca una reescritura para todas las direcciones URL que coinciden con la regla de reescritura *Pattern*. Por lo tanto, si el valor de la variable sessionid ( `%{sessionid}`) es `1234`, una dirección URL del formulario `https://www.domain.com/products/?sessionid=5678&function=buy` se reescribe como `https://www.domain.com/products/?sessionid=1234&function=buy`

## Reconocimiento {#section_B17088EF38244496BC1DDD4ECF75EB5B}

El software del motor de reescritura fue originalmente desarrollado por el Grupo Apache para su uso en el proyecto del servidor HTTP Apache (https://www.apache.org/).

## Adición de una regla de URL de almacenamiento de lista de rastreo {#task_22DD40DF95584B12BE8E6ECFBF579BCD}

Puede agregar reglas de URL del almacén de listas de rastreo para especificar cómo se reescriben las direcciones URL que se encuentran en el contenido web. Puede especificar un número ilimitado de reglas y condiciones, y puede manipular cualquier parte de las direcciones URL encontradas.

<!-- 

t_adding_a_crawl_list_store_url_rule.xml

 -->

**Para agregar reglas de URL del almacén de listas de rastreo:**

1. En el menú del producto, haga clic en **[!UICONTROL Settings]** > **[!UICONTROL Rewrite Rules]** > **[!UICONTROL Crawl List Store URL Rules]**.
1. En el campo [!DNL Crawl List Store URL Rules], introduzca las reglas que desee.

   Se permiten líneas en blanco y líneas de comentarios que empiecen por un carácter &#39;#&#39; (hash).
1. (Opcional) En la página [!DNL Crawl List Store URL Rules], en el campo [!DNL Test Crawl List Store URL Rules], introduzca una URL de prueba cuyas reglas de rastreo desee probar y, a continuación, haga clic en **[!UICONTROL Test]**.
1. Haga clic **[!UICONTROL Save Changes]**.
1. (Opcional) Reconstruya el índice del sitio provisional si desea obtener una vista previa de los resultados.

   Consulte [Configuración de un índice incremental de un sitio web provisional](../c-about-index-menu/c-about-incremental-index.md#task_46A367B0786C4C90BFFA5D3F95FD86C0).
1. (Opcional) En la página [!DNL Crawl List Store URL Rules], realice una de las acciones siguientes:

   * Haga clic en **[!UICONTROL History]** para revertir cualquier cambio que haya realizado.

      Consulte [Uso de la opción Historial](../t-using-the-history-option.md#task_70DD3F87A67242BBBD2CB27156F43002).

   * Haga clic **[!UICONTROL Live]**.

      Consulte [Visualización de la configuración de lanzamiento](../c-about-staging.md#task_401A0EBDB5DB4D4CA933CBA7BECDC10F).

   * Haga clic **[!UICONTROL Push Live]**.

      Consulte [Inserción de la configuración del escenario en directo](../c-about-staging.md#task_44306783B4C0408AAA58B471DAF2D9A4).

## Acerca de la lista de rastreo Recuperar reglas de URL {#concept_EC8E2E48B99A458D8567B526C9827CBA}

Las reglas de rastreo de URL especifican cómo se reescriben las direcciones URL que se encuentran en el contenido web. Puede especificar un número ilimitado de reglas y condiciones, y puede manipular cualquier parte de las direcciones URL encontradas.

<!-- 

c_about_crawl_list_retrieve_url_rules.xml

 -->

Antes de que los efectos de las reglas sean visibles para los clientes, asegúrese de reconstruir el índice del sitio.

Las reglas de rastreo son más útiles para reescribir partes dinámicas de una URL, como un identificador de sesión que es único para cada cliente que visita el sitio web. También puede utilizar reglas de reescritura para ocultar partes de una URL, como parámetros de consulta, del robot de búsqueda. De forma predeterminada, no se especifican reglas y no se reescribe la dirección URL.

A medida que se rastrea un sitio web, las direcciones URL de contenido incrustado se almacenan en una lista temporal de páginas web adicionales para rastrear. Cuando el robot de búsqueda recupera una dirección URL de la lista, se utiliza Recuperar reglas de reescritura para manipular partes de esa dirección URL. Normalmente, las reglas de recuperación se utilizan para insertar datos que diferencian el tiempo en una dirección URL. Es esta URL final la que se utiliza para recuperar la página de su sitio web.

Recuperar reglas de reescritura solo es necesario si las direcciones URL contienen datos dinámicos, como un ID de sesión, y si esos datos dinámicos cambian con el tiempo para seguir siendo válidos. En este caso, utilice las reglas de reescritura de la tienda para obtener el estado más reciente de los datos de las direcciones URL encontradas. A continuación, utilice Recuperar reglas de reescritura para agregar esos datos a cada URL cuando los robots de búsqueda recuperen la página.

Cada regla se especifica con una regla de reescritura (RewriteRule) directiva y una o más condiciones de reescritura opcionales (RewriteCond). El orden de las reglas es importante. El conjunto de reglas pasa de un bucle a otra regla. Cuando una regla coincide, recorre en bucle las condiciones de reescritura correspondientes. Se especifica una regla de URL de rastreo de la siguiente manera:

```
RewriteCond TestString CondPattern [Flags] 
RewriteRule Pattern Substitution [Flags]
```

Cuando se encuentra una URL incrustada, el robot de búsqueda intenta hacer coincidir la URL con el Patrón de cada regla de rastreo. Si el patrón coincide, el motor de reescritura busca las directivas de RewriteCond correspondientes. Si no hay condiciones, la URL se sustituye por un nuevo valor construido a partir de la cadena de sustitución y continúa con la siguiente regla en el conjunto de reglas. Si existen condiciones, se procesan en el orden en que aparecen en la lista. El motor de reescritura intenta hacer coincidir un patrón de condición (CondPattern) con una cadena de prueba (TestString). Si las dos coinciden, la siguiente condición se procesa hasta que no haya más condiciones disponibles. Si todas las condiciones coinciden, la dirección URL se sustituye por la sustitución especificada en la regla. Si no se cumple la condición, falla el conjunto completo de condiciones y la regla correspondiente.

## Acerca de las directivas RewriteRule {#section_32B24B29627946398AFBC5F869A610CB}

Una directiva RewriteRule tiene la siguiente forma:

```
           
<i>RewriteRule Pattern Substitution [Flags]</i> 
        
```

`Pattern` puede ser una expresión regular POSIX, que se aplica a la dirección URL actual. La &quot;dirección URL actual&quot; puede diferir de la dirección URL solicitada original, ya que es posible que las reglas anteriores ya hayan coincidido y modificado la dirección URL.

Consulte [Expresiones regulares](../c-appendices/r-regular-expressions.md#reference_B5BA7D61D82E4109A01D2A2D964E3A6A).

No se puede usar el carácter &quot;no&quot; (&#39;!&#39;) para prefijar el patrón. El carácter &quot;not&quot; permite negar un patrón, es decir, que sea verdadero solo si la dirección URL actual NO coincide con este patrón. El carácter &quot;not&quot; se puede usar cuando es mejor que coincida con un patrón negativo o como regla predeterminada final.

>[!NOTE]
>
>No se pueden usar tanto el carácter &quot;no&quot; como los comodines agrupados en un patrón. Además, no se puede utilizar un patrón denegado cuando la cadena de sustitución contiene $N.

Puede utilizar paréntesis para crear una referencia secundaria en el patrón, al que se puede hacer referencia mediante Substitution y CondPattern.

**** SustituciónLa URL se reemplaza por la cadena de sustitución que contiene lo siguiente:

Texto sin formato: Texto que se pasa sin cambiar.

Las referencias de fondo proporcionan acceso a las partes agrupadas (entre paréntesis) del Patrón o CondPattern. Los siguientes son los dos tipos de referencias invertidas:

* **RewriteRule** BackReferencesEstas referencias se corresponden con las del patrón de regla de reescritura correspondiente y toman el formato $N (0)  &lt;> Por ejemplo, `RewriteRule ^https:// ([^/]*) (.*)$ https://${tolower: $1} $2.`

* ** Referencias de RewriteCond** Estas referencias de coincidencia coinciden con las anteriores en el último CondPattern de RewriteCond coincidente y toman la forma %N (0 &lt;= N &lt;= 9).

Variables: Son variables con la forma %{NAME_OF_VARIABLE} donde NAME_OF_VARIABLE es una cadena para el nombre de una variable definida. Consulte el indicador *[E]* para obtener más información sobre la configuración de variables de entorno.

Funciones: Estas son funciones del formulario ${NAME_OF_FUNCTION:key}, donde NAME_OF_FUNCTION es el siguiente:

* hace que todos los caracteres de *key* estén en minúsculas.
* toupper escribe todos los caracteres en *key* mayúscula.
* escape URL-codifica todos los caracteres de *key*.
* Los caracteres &quot;a&quot;...z&#39;, &#39;A&#39;..&#39;Z&#39;, &#39;0&#39;...9&#39;, &#39;*&#39;, &#39;-&#39;, &#39;.&#39;, &#39;/&#39;, &#39;@&#39; y &#39;_&#39; no se han modificado; los espacios se traducen a &#39;+&#39; y todos los demás caracteres se transforman en su equivalente con codificación URL %xx.
* unescape transforma &#39;+&#39; de nuevo en espacio y todos los caracteres con codificación URL de %xx de nuevo en caracteres únicos.

>[!NOTE]
>
>Hay una cadena de sustitución especial: &#39;-&#39; que significa &quot;NO substitution&quot;. La cadena &quot;-&quot; se utiliza a menudo con el indicador C (cadena), lo que permite hacer coincidir una dirección URL con varios patrones antes de que se produzca una sustitución.

**Indicadores**

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Indicador </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> 'last|L' </p> </td> 
   <td colname="col2"> <p>Última regla. </p> <p>Detiene el proceso de reescritura y no aplica ninguna regla de reescritura adicional. Utilice este indicador para evitar que se siga procesando la dirección URL actual. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'next|N' </p> </td> 
   <td colname="col2"> <p>La siguiente ronda. </p> <p> Vuelve a ejecutar el proceso de reescritura (comenzando de nuevo con la primera regla de reescritura) utilizando la URL de la última regla de reescritura (no la URL original). Tenga cuidado de no crear un bucle muerto. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'chain|C' </p> </td> 
   <td colname="col2"> <p>Encadenado con la regla siguiente. </p> <p>Encadena la regla actual con la siguiente regla (que también se puede encadenar con la regla siguiente, etc.). Si una regla coincide, el proceso de sustitución continúa como de costumbre. Si la regla no coincide, se omiten todas las reglas encadenadas subsiguientes. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'nocase|NC' </p> </td> 
   <td colname="col2"> <p>No hay caso. </p> <p> Hace que el Patrón no distinga mayúsculas de minúsculas (es decir, no hay diferencia entre "A-Z" y "a-z") cuando el patrón coincide con la dirección URL actual. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'skip|S=num' </p> </td> 
   <td colname="col2"> <p>Omita la regla o las reglas siguientes. </p> <p> Si la regla actual coincide, este indicador obliga al motor de reescritura a omitir las siguientes reglas de núm del conjunto de reglas. Utilice este indicador para realizar construcciones pseudo if-then-else. La última regla de la cláusula then se convierte en un skip=N donde N es el número de reglas de la cláusula else. </p> <p> <p>Nota:  ¡Esta bandera no es la misma que la bandera 'chain|C'!) </p> </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'env|E=VAR:VAL' </p> </td> 
   <td colname="col2"> <p>Establece la variable ambiental. </p> <p>Crea una variable de entorno "VAR" configurada en el valor VAL, donde VAL puede contener referencias atrás de expresiones regulares, $N y %N, que se expanden. Puede utilizar este indicador más de una vez para configurar varias variables. Posteriormente se puede anular la referencia a las variables en un patrón RewriteCond siguiente a través de %{VAR}. </p> <p>Utilice este indicador para eliminar y recordar información de direcciones URL. </p> </td> 
  </tr> 
 </tbody> 
</table>

>[!NOTE]
>
>Las reglas de reescritura de almacén y las reglas de reescritura de recuperación comparten valores de variables. Debido a este comportamiento, puede establecer una variable en un valor sessionid que distinga el tiempo cuando se encuentre y almacene una URL incrustada. Cuando se recupera la siguiente dirección URL de la lista de almacenamiento temporal, se puede añadir el valor más reciente de sessionid antes de recuperar la página.

**Ejemplo de una regla de reescritura con una función**

Supongamos que tiene un servidor que distingue entre mayúsculas y minúsculas y que gestiona las cadenas &quot;www.mydomain.com&quot; y &quot;www.MyDomain.com&quot; de forma diferente. Para que su servidor funcione correctamente, asegúrese de que el dominio sea siempre &quot;www.mydomain.com&quot; aunque algunos documentos contengan vínculos que hagan referencia a &quot;www.MyDomain.com&quot;. Para ello, puede utilizar la siguiente regla:

```
RewriteRule  ^https:// 
<b>([^/]*)</b> 
<i>(.*)</i>$  https://${tolower:$1}$2
```

Esta regla de reescritura utiliza la función `tolower` para reescribir la parte de dominio de una URL y asegurarse de que siempre esté en minúsculas, como en el siguiente ejemplo:

1. El patrón `(^https://([^/]*)(.*)$)` contiene una referencia secundaria ** `([^/]*)`** que coincide con todos los caracteres entre `https://` y el primer `/` en la dirección URL. El patrón también contiene una segunda referencia `(.*)` que coincide con todos los caracteres restantes de la dirección URL.
1. La sustitución `(https://${tolower:$1}$2)` indica al motor de búsqueda que vuelva a escribir la dirección URL utilizando la función `tolower` en la primera referencia `(https:// ${tolower:$1}$2)` sin modificar el resto de la dirección URL `(https://${tolower:$1} $2)`.

Por lo tanto, una dirección URL del formulario `https://www.MyDomain.com/INTRO/index.Html` se reescribe como `https://www.mydomain.com/INTRO/index.Html`.

## Acerca de las directivas RewriteCond {#section_ADD642A24B68452CB98294A0BD687EC3}

La directiva RewriteCond define una condición de regla. Cuando RewriteCond precede a una regla de reescritura, la regla solo se utiliza si su patrón coincide con el título actual y se aplican las condiciones adicionales. Las condiciones de reescritura toman la siguiente forma:

```
           
<i>RewriteCond TestString CondPattern [Flags]</i> 
        
```

`TestString` es una cadena que puede contener las siguientes construcciones:

Texto sin formato: Texto que se pasa sin cambiar.

Las referencias de fondo proporcionan acceso a las partes agrupadas (entre paréntesis) del Patrón o CondPattern. Los siguientes son los dos tipos de referencias invertidas:

* **RewriteRule** BackReferencesEstas referencias se corresponden con las del patrón de regla de reescritura correspondiente y toman el formato $N (0)  &lt;> Por ejemplo, `RewriteRule ^https:// ([^/]*) (.*)$ https://${tolower: $1} $2`.

* **RewriteCond** BackReferencesEstas referencias coincidentes en el último CondPattern coincidente de RewriteCond y toman la forma %N (0&lt;>

Variables: Son variables con la forma %{NAME_OF_VARIABLE} donde NAME_OF_VARIABLE puede ser una cadena para el nombre de una variable definida. Consulte el indicador RewriteRule *`[E]`* para obtener más información sobre la configuración de variables.

Funciones: Estas son funciones del formulario ${NAME_OF_FUNCTION:key}, donde NAME_OF_FUNCTION es el siguiente:

* hace que todos los caracteres de *key* estén en minúsculas.
* toupper escribe todos los caracteres en *key* mayúscula.
* escape URL-codifica todos los caracteres en clave. Los caracteres &quot;a&quot;...z&#39;, &#39;A&#39;..&#39;Z&#39;, &#39;0&#39;...9&#39;, &#39;*&#39;, &#39;-&#39;, &#39;.&#39;, &#39;/&#39;, &#39;@&#39; y &#39;_&#39; se dejan sin cambios, los espacios se traducen en &#39;+&#39; y todos los demás caracteres se transforman en su equivalente con codificación URL de %xx.
* unescape transforma &#39;+&#39; de nuevo en espacio y todos los caracteres de codificación de URL de %xx vuelven a codificarse en caracteres únicos.

**** CondPatternis es una expresión regular extendida estándar con algunas adiciones. La cadena de patrón puede tener el prefijo &quot;!&quot; carácter (signo de exclamación) para especificar un patrón no coincidente. En lugar de cadenas de expresión regular reales, puede utilizar una de las siguientes variantes especiales:

>[!NOTE]
>
>También puede prefijar todas estas pruebas con un signo de exclamación (&#39;!&#39;) para negar su significado.

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Cadena CondPattern </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> '&lt;CondPattern' </p> </td> 
   <td colname="col2"> <p>Lexicamente menos. </p> <p> Trata el CondPattern como una cadena sin formato y lo compara lexicamente con TestString. True si TestString es lexicamente menor que CondPattern. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> '&gt;CondPattern' </p> </td> 
   <td colname="col2"> <p>Bueno Lexicamente. </p> <p> Trata el CondPattern como una cadena sin formato y lo compara lexicamente con TestString. True si TestString es lexicamente bueno que CondPattern. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> '=CondPattern' </p> </td> 
   <td colname="col2"> <p>Lexicamente igual. </p> <p> Trata el CondPattern como una cadena sin formato y lo compara lexicamente con TestString. True si TestString es lexicamente igual a CondPattern, es decir, las dos cadenas son exactamente iguales (carácter por carácter). Si CondPattern es solo "" (dos comillas), se compara TestString con la cadena vacía. </p> </td> 
  </tr> 
 </tbody> 
</table>

**Indicadores**
 (opcional) Incluir indicadores entre corchetes  `[]`. Los indicadores múltiples están separados por coma.

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Indicador </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> 'nocase|NC' </p> </td> 
   <td colname="col2"> <p> No hay caso. </p> <p>Este indicador hace que la prueba no distinga entre mayúsculas y minúsculas, es decir, no hay diferencia entre "A-Z" y "a-z" tanto en la cadena de prueba expandida como en CondPattern. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'ornext|OR' </p> </td> 
   <td colname="col2"> <p>O condición siguiente. </p> <p>Utilice este indicador para combinar las condiciones de regla con una condición OR local en lugar de la condición Y implícita. Sin este indicador, tendría que escribir el cond/regla varias veces. </p> </td> 
  </tr> 
 </tbody> 
</table>

**Ejemplo**

Algunas páginas web asignan una variable CGI &quot;sessionid&quot; la primera vez que un visitante llega a un sitio. Esta variable se utiliza para identificar al visitante y, a medida que el visitante navega por el sitio, se pasa la variable . Dado que el robot de búsqueda se parece a un visitante del sitio, se le asigna un número &quot;sessionid&quot;. El robot de búsqueda mantiene este único valor &quot;sessionid&quot;, incluso si una segunda página del sitio intenta asignar un nuevo valor. Para garantizar esto, necesita dos reglas de reescritura.

La primera regla se utiliza para identificar y almacenar la variable sessionid :

```
RewriteCond  %{sessionid}  !.+ 
RewriteRule  ^.+sessionid= 
<b>([^&#]+)</b>.*$  -   
<i>[E=sessionid:$1]</i>
```

La regla de reescritura utiliza un indicador E `([E=sessionid:$1])` para asignar el valor actual del parámetro CGI de sessionid a la variable `sessionid`. El `$1` hace referencia a la primera referencia secundaria, que se encuentra entre el primer conjunto de paréntesis del patrón `([^&#]+)` de RewriteRule.

La expresión regular `^&#]+` coincide con la parte de una dirección URL entre la palabra `sessionid` y el siguiente**&amp;**o**#**carácter. Dado que esta regla de reescritura solo se utiliza para crear el valor inicial de la variable sessionid, no se reescribe. Tenga en cuenta que el campo Sustitución de la regla está establecido en `-` para indicar que no es necesario reescribir.

RewriteCond examina la variable `sessionid` ( `%{sessionid}`). Si ni siquiera tiene un solo carácter (!.+), entonces la regla de reescritura coincide.

Con esta regla, la dirección URL se lee como `https://www.domain.com/home/?sessionid=1234&function=start` y asigna el valor `1234` a la variable `sessionid`.

La segunda regla se usa para reescribir todas las direcciones URL que coincidan con el siguiente patrón de regla de reescritura:

```
RewriteRule   
<b>^(.+)</b>sessionid=[^&#]+ 
<i>(.*)$</i>  $1sessionid=%{sessionid}$2
```

El patrón de regla de reescritura contiene dos referencias atrás: `(.+)` y `(.*)`. La primera referencia inversa coincide con todos los caracteres anteriores a `sessionid`. La segunda referencia backreference coincide con todos los caracteres después de la `&` o `#` terminadora.

El patrón de sustitución reescribe la dirección URL utilizando la primera referencia inversa, seguida de la cadena &quot;sessionid=&quot;, seguida del valor de la variable de ID de sesión definida por la primera regla `%{sessionid}`, seguida de la segunda referencia inversa. `($1sessionid=%{sessionid} $2)`

Observe que esta regla de reescritura no contiene un objeto RewriteCond. Como tal, provoca una reescritura para todas las direcciones URL que coinciden con la regla de reescritura *Pattern*. Por lo tanto, si el valor de la variable sessionid ( `%{sessionid}`) es `1234`, una dirección URL del formulario `https://www.domain.com/products/?sessionid=5678&function=buy` se reescribe como `https://www.domain.com/products/?sessionid=1234&function=buy`

## Reconocimiento {#section_EC3A1DAEB5A54C93A265CB119DF91E9F}

El software del motor de reescritura fue originalmente desarrollado por el Grupo Apache para su uso en el proyecto del servidor HTTP Apache (https://www.apache.org/).

## Agregar lista de rastreo para recuperar reglas de URL {#task_94A28ED7DC404BFF9767DBB5ADEE6B7A}

Puede agregar reglas de recuperación de URL de lista de rastreo para especificar cómo se reescriben las direcciones URL encontradas dentro del contenido web. Recuperar reglas de reescritura solo es necesario si las direcciones URL contienen datos dinámicos, como un ID de sesión, y si esos datos dinámicos cambian con el tiempo para seguir siendo válidos.

<!-- 

t_adding_crawl_list_retrieve_url_rules.xml

 -->

**Para agregar la lista de rastreo, recupere las reglas de URL:**

1. En el menú del producto, haga clic en **[!UICONTROL Settings]** > **[!UICONTROL Rewrite Rules]** > **[!UICONTROL Crawl List Retrieve URL Rules]**.
1. En el campo [!DNL Crawl List Retrieve URL Rules], introduzca las reglas que desee.

   Se permiten líneas en blanco y líneas de comentarios que empiecen por un carácter &#39;#&#39; (hash).
1. (Opcional) En la página [!DNL Crawl List Retrieve URL Rules], en el campo [!DNL Test Crawl List Retrieve URL Rules], introduzca una URL de prueba cuyas reglas de rastreo desee probar y, a continuación, haga clic en **[!UICONTROL Test]**.
1. Haga clic **[!UICONTROL Save Changes]**.
1. (Opcional) Reconstruya el índice del sitio provisional si desea obtener una vista previa de los resultados.

   Consulte [Configuración de un índice incremental de un sitio web provisional](../c-about-index-menu/c-about-incremental-index.md#task_46A367B0786C4C90BFFA5D3F95FD86C0).
1. (Opcional) En la página [!DNL Crawl List Retrieve URL Rules], realice una de las acciones siguientes:

   * Haga clic en **[!UICONTROL History]** para revertir cualquier cambio que haya realizado.

      Consulte [Uso de la opción Historial](../t-using-the-history-option.md#task_70DD3F87A67242BBBD2CB27156F43002).

   * Haga clic **[!UICONTROL Live]**.

      Consulte [Visualización de la configuración de lanzamiento](../c-about-staging.md#task_401A0EBDB5DB4D4CA933CBA7BECDC10F).

   * Haga clic **[!UICONTROL Push Live]**.

      Consulte [Inserción de la configuración del escenario en directo](../c-about-staging.md#task_44306783B4C0408AAA58B471DAF2D9A4).

## Acerca de las reglas de título de rastreo {#concept_BD3A576987DA4D93A998B0B9CDDC3C79}

Las reglas de título de rastreo especifican cómo se reescriben los títulos encontrados dentro del contenido web antes de almacenarlos en el índice de búsqueda.

<!-- 

c_about_crawl_title_rules.xml

 -->

Por ejemplo, puede utilizar una regla de reescritura para eliminar una parte de un título, como un nombre de organización. A medida que se rastrea un sitio web, los títulos encontrados se almacenan en un búfer temporal. Sin embargo, antes de añadir un título a este búfer, se le aplican las reglas de título. De forma predeterminada, la búsqueda/comercialización del sitio no tiene reglas de título de rastreo y no realiza modificaciones de título.

Antes de que los efectos de las reglas sean visibles para los clientes, reconstruya el índice del sitio.

Puede revertir rápidamente cualquier cambio que realice en las reglas de título de rastreo mediante la función Historial.

Las reglas pueden constar de dos elementos principales: la regla de reescritura y la regla de reescritura opcional. Puede especificar un número ilimitado de reglas y condiciones. El orden de estas reglas es importante porque el conjunto de reglas pasa de una regla a otra. Cuando una regla coincide, recorre en bucle cualquier condición de reescritura correspondiente (opcional). Las reglas de rastreo de URL se especifican de la siguiente manera:

```
RewriteCond  
<i>TestString CondPattern [Flags]</i> 
RewriteRule  
<i>Pattern Substitution [Flags]</i> 
 
RewriteCond  
<i>TestString CondPattern [Flags]</i> 
RewriteRule  
<i>Pattern Substitution [Flags]</i>
```

Cuando se encuentra un título, el robot de búsqueda intenta hacer coincidir el título con el Patrón de cada regla de rastreo. Si el patrón coincide, el motor de reescritura busca las directivas de RewriteCond correspondientes. Si no hay condiciones, la URL se sustituye por un nuevo valor construido a partir de la cadena de sustitución y continúa con la siguiente regla en el conjunto de reglas. Si existen condiciones, se procesan en el orden en que aparecen en la lista. El motor de reescritura intenta hacer coincidir un patrón de condición (CondPattern) con una cadena de prueba (TestString). Si las dos coinciden, la siguiente condición se procesa hasta que no haya más condiciones disponibles. Si todas las condiciones coinciden, la dirección URL se sustituye por la sustitución especificada en la regla. Si no se cumple la condición, falla el conjunto completo de condiciones y la regla correspondiente.

Introduzca las reglas de URL de rastreo en el cuadro de texto y, a continuación, haga clic en Guardar cambios. Se permiten líneas en blanco y líneas de comentarios que empiecen por un carácter &#39;#&#39; (hash). Para probar las reglas de búsqueda, puede introducir una URL de prueba en el cuadro de texto &quot;Probar reglas de reescritura&quot; y luego hacer clic en Probar.

## Directiva RewriteRule {#section_669445C505754E838E14029D6583D9B6}

Cada directiva RewriteRule define una regla de reescritura. Las reglas se aplican en el orden en que aparecen en la lista. Una regla de reescritura adopta la siguiente forma:

```
RewriteRule Pattern Substitution [Flags]
```

**** El patrón puede ser una expresión regular POSIX, que se aplica al título actual. El &quot;título actual&quot; difiere del título original, ya que las reglas anteriores ya lo han hecho y lo han modificado.

Consulte [Expresiones regulares](../c-appendices/r-regular-expressions.md#reference_B5BA7D61D82E4109A01D2A2D964E3A6A).

Puede utilizar el carácter &quot;no&quot; (&#39;!&#39;) para prefijar el patrón. El carácter &quot;not&quot; permite negar un patrón, es decir, que sea verdadero solo si el título actual NO coincide con el patrón. El carácter &quot;not&quot; se puede usar cuando es mejor que coincida con un patrón negativo o como regla predeterminada final. Nota: No se pueden usar tanto el carácter &quot;no&quot; como los comodines agrupados en un patrón. Además, no se puede utilizar un patrón denegado cuando la cadena de sustitución contiene `$N`.

Puede utilizar paréntesis para crear una referencia secundaria, a la que se puede hacer referencia mediante Substitution y CondPattern.

Sustitución : el título se sustituye por la cadena de sustitución. La cadena puede contener lo siguiente:

Texto sin formato : texto que se pasa sin cambiar.

Las referencias de fondo proporcionan acceso a las partes agrupadas (entre paréntesis) del Patrón o CondPattern. Los siguientes son dos tipos de referencias invertidas:

* Referencias de retorno de regla de reescritura : coinciden con las referencias anteriores en el patrón de regla de reescritura correspondiente y toman el formato $N (0 &lt;= N &lt;= 9).

   Por ejemplo, `RewriteRule ^My[[:blank:]] (.*)$ ${toupper: $1}`

* Rereferencias de RewriteCond : coinciden con referencias anteriores en el último CondPattern coincidente de RewriteCond y toman la forma %N (0 &lt;= N &lt;= 9).

Variables Son variables del formulario %{NAME_OF_VARIABLE} donde NAME_OF_VARIABLE puede ser una cadena para el nombre de una variable definida. Consulte el indicador `[E]` para obtener más información sobre la configuración de variables de entorno.

Funciones Estas son funciones del formulario ${NAME_OF_FUNCTION: clave} donde NAME_OF_FUNCTION es:

* hace que todos los caracteres de *key* estén en minúsculas.
* toupper escribe todos los caracteres en *key* mayúscula.

>[!NOTE]
>
>Hay una cadena de sustitución especial: &#39;-&#39; que significa &quot;NO substitution&quot;. La cadena &quot;-&quot; suele ser útil con el indicador C (cadena), lo que permite hacer coincidir un título con varios patrones antes de que se produzca una sustitución.

**Indicadores**  (opcional)

Los indicadores se incluyen entre corchetes `[]`y varios indicadores están separados por coma:

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Indicador </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> 'last|L' </p> </td> 
   <td colname="col2"> <p>Última regla. </p> <p> Detiene el proceso de reescritura y no aplica ninguna regla de reescritura adicional. Utilice este indicador para evitar que se siga procesando el título actual. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'next|N' </p> </td> 
   <td colname="col2"> <p>La siguiente ronda. </p> <p> Vuelve a ejecutar el proceso de reescritura (comenzando de nuevo con la primera regla de reescritura) utilizando el título de la última regla de reescritura, no el título original. Tenga cuidado de no crear un bucle muerto. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'chain|C' </p> </td> 
   <td colname="col2"> <p>Encadenado con la regla siguiente. </p> <p>Encadena la regla actual con la siguiente regla (que también se puede encadenar con la regla siguiente, etc.). Si una regla coincide, el proceso de sustitución continúa como de costumbre. Si la regla no coincide, se omiten todas las reglas encadenadas subsiguientes. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'nocase|NC' </p> </td> 
   <td colname="col2"> <p>No hay caso. </p> <p>Hace que el Patrón no distinga mayúsculas de minúsculas (es decir, no hay diferencia entre "A-Z" y "a-z") cuando el patrón se compara con el título actual. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'skip|S=num' </p> </td> 
   <td colname="col2"> <p>Omita la regla o las reglas siguientes. </p> <p> Si la regla actual coincide, este indicador obliga al motor de reescritura a omitir las siguientes reglas de núm del conjunto de reglas. Utilice esto para hacer construcciones pseudo if-then-else. La última regla de la cláusula then se convierte en un skip=N donde N es el número de reglas de la cláusula else. (Nota: ¡Esto no es lo mismo que la bandera 'chain|C'!) </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'env|E=VAR:VAL' </p> </td> 
   <td colname="col2"> <p>Establezca la variable de entorno. </p> <p> Crea una variable de entorno "VAR" configurada en el valor VAL, donde VAL puede contener referencias atrás de expresiones regulares, $N y %N, que se expande. Puede utilizar este indicador más de una vez para configurar varias variables. Se puede hacer referencia a las variables más adelante en el siguiente patrón RewriteCond mediante %{VAR}. Utilice este indicador para eliminar y recordar información de títulos. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Directiva RewriteCond (opcional) {#section_D664B71DE3884E0790531804C49A3222}

La directiva RewriteCond define una condición de regla. Cuando RewriteCond precede a una regla de reescritura, la regla solo se utiliza si su patrón coincide con el título actual y se aplican las condiciones adicionales.

Las directivas de condiciones de reescritura adoptan la siguiente forma:

```
RewriteCond TestString CondPattern [Flags] 
```

**** TestString es una cadena que puede contener las siguientes construcciones:

Texto sin formato : texto que se pasa sin cambiar.

Las referencias de fondo proporcionan acceso a las partes agrupadas (entre paréntesis) del Patrón o CondPattern. Existen dos tipos de referencias secundarias:

* Referencias de retorno de regla de reescritura : coinciden con las referencias anteriores en el patrón de regla de reescritura correspondiente y toman el formato $N (0 &lt;= N &lt;= 9).

   Por ejemplo, `RewriteRule ^My[[:blank:]] (.*)$ ${toupper: $1}`

* Rereferencias de RewriteCond : coinciden con referencias anteriores en el último CondPattern coincidente de RewriteCond y toman la forma %N (0 &lt;= N &lt;= 9).

Variables Son variables del formulario %{NAME_OF_VARIABLE} donde NAME_OF_VARIABLE puede ser una cadena para el nombre de una variable definida. Consulte el indicador `[E]` para obtener más información sobre la configuración de variables de entorno.

Funciones Estas son funciones del formulario ${NAME_OF_FUNCTION:key} donde NAME_OF_FUNCTION es:

* hace que todos los caracteres de *key* estén en minúsculas.
* toupper escribe todos los caracteres en *key* mayúscula.
* escape URL-codifica todos los caracteres en clave.
* Los caracteres &quot;a&quot;...z&#39;, &#39;A&#39;..&#39;Z&#39;, &#39;0&#39;...9&#39;, &#39;*&#39;, &#39;-&#39;, &#39;.&#39;, &#39;/&#39;, &#39;@&#39; y &#39;_&#39; se dejan sin cambios, los espacios se traducen en &#39;+&#39; y todos los demás caracteres se transforman en su equivalente con codificación URL de %xx.
* unescape transforma &#39;+&#39; de nuevo en espacio y todos los caracteres con codificación URL de %xx de nuevo en caracteres únicos.

**** CondPatternis es una expresión regular extendida estándar con algunas adiciones. La cadena de patrón puede tener el prefijo &quot;!&quot; carácter (signo de exclamación) para especificar un patrón no coincidente. En lugar de cadenas de expresión regular reales, puede utilizar una de las siguientes variantes especiales.

>[!NOTE]
>
>Puede anteponer todas estas pruebas con un signo de exclamación (&#39;!&#39;) para negar su significado.

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Cadena CondPattern </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> '&lt;CondPattern' </p> </td> 
   <td colname="col2"> <p>Es lexicamente menor. </p> <p>Trata el <i>CondPattern</i> como una cadena sin formato y lo compara lexicamente con <i>TestString</i>. True si <i>TestString</i> es lexicamente menor que <i>CondPattern</i>. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> '&gt;CondPattern' </p> </td> 
   <td colname="col2"> <p>Es lexicamente bueno. </p> <p>Trata el <i>CondPattern</i> como una cadena sin formato y lo compara lexicamente con <i>TestString</i>. True si <i>TestString</i> es lexicamente bueno que <i>CondPattern</i>. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> '=CondPattern' </p> </td> 
   <td colname="col2"> <p> Es lexicamente igual. </p> <p>Trata el <i>CondPattern</i> como una cadena sin formato y lo compara lexicamente con <i>TestString</i>. True si <i>TestString</i> es lexicamente igual a <i>CondPattern</i>, es decir, las dos cadenas son exactamente iguales (carácter por carácter). Si <i>CondPattern</i> es solo "" (dos comillas), se compara <i>TestString</i> con la cadena vacía. </p> </td> 
  </tr> 
 </tbody> 
</table>

**Indicadores**  (opcional)

Los indicadores se incluyen entre corchetes `[]`y varios indicadores están separados por coma:

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Indicador </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> 'nocase|NC' </p> </td> 
   <td colname="col2"> <p>No hay caso. </p> <p> Hace que la prueba no sea sensible. Es decir, no hay diferencia entre "A-Z" y "a-z" tanto en el <i>TestString</i> expandido como en el <i>CondPattern.</i> </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'ornext|OR' </p> </td> 
   <td colname="col2"> <p> O condición siguiente. </p> <p>Utilice este indicador para combinar las condiciones de regla con una condición OR local en lugar de la condición Y implícita. Sin este indicador, tendría que escribir el cond/regla varias veces. </p> </td> 
  </tr> 
 </tbody> 
</table>

**Ejemplo**

Supongamos que tiene un sitio Web corporativo con un formato de título estándar: &quot;Mi empresa&quot; seguida de un guión y, a continuación, una descripción específica de la página (&quot;Mi empresa - bienvenida&quot; o &quot;Mi empresa - noticias&quot;, por ejemplo). Desea eliminar &quot;Mi empresa -&quot; del título y convertir todo el título en letras mayúsculas cuando indexa el sitio.

La siguiente regla de reescritura utiliza la función toupper para reescribir solo la parte descriptiva de un título en mayúsculas:

```
RewriteRule  ^My[[:blank:]]Company[[:blank:]]-[[:blank:]] 
<b>(.*)</b>$  ${toupper: 
<b>$1</b>}
```

Patrón de la regla

`(^My[[:blank:]]Company[[:blank:]]-[[:blank:]] (.*))`

contiene una referencia `(.*)` que coincide con el contenido del título que sigue a &quot;Mi empresa-&quot;. Recuerde que rodear una parte de un patrón con paréntesis ( ) crea una referencia secundaria a la que la sustitución puede hacer referencia. En este ejemplo, la sustitución

`(${toupper:**$1**})`

reescribe esa referencia de retorno (`**$1**`) utilizando la función toupper .

Por lo tanto, un título del formulario &quot;Mi empresa - bienvenida&quot; se reescribe como &quot;BIENVENIDO&quot;.

**Reconocimiento**

El software del motor de reescritura fue originalmente desarrollado por el Grupo Apache para su uso en el proyecto del servidor HTTP Apache (https://www.apache.org/).

## Adición de reglas de título de rastreo {#task_272BB4C603BA4C9ABDBEEB398798B101}

Puede agregar reglas de título de rastreo para especificar cómo se reescriben los títulos encontrados dentro del contenido web antes de que se almacenen en el índice de búsqueda.

<!-- 

t_adding_crawl_title_rules.xml

 -->

**Para agregar reglas de título de rastreo:**

1. En el menú del producto, haga clic en **[!UICONTROL Settings]** > **[!UICONTROL Rewrite Rules]** > **[!UICONTROL Crawl Title Rules]**.
1. En el campo [!DNL Crawl Title Rules], introduzca las reglas que desee.

   Se permiten líneas en blanco y líneas de comentarios que empiecen por un carácter &#39;#&#39; (hash).
1. (Opcional) En la página [!DNL Crawl Title Rules], en el campo [!DNL Test Crawl Title Rules], introduzca una URL de prueba cuyas reglas de búsqueda desee probar y, a continuación, haga clic en **[!UICONTROL Test]**.
1. Haga clic **[!UICONTROL Save Changes]**.
1. (Opcional) Reconstruya el índice del sitio provisional si desea obtener una vista previa de los resultados.

   Consulte [Configuración de un índice incremental de un sitio web provisional](../c-about-index-menu/c-about-incremental-index.md#task_46A367B0786C4C90BFFA5D3F95FD86C0).
1. (Opcional) En la página [!DNL Crawl Title Rules], realice una de las acciones siguientes:

   * Haga clic en **[!UICONTROL History]** para revertir cualquier cambio que haya realizado.

      Consulte [Uso de la opción Historial](../t-using-the-history-option.md#task_70DD3F87A67242BBBD2CB27156F43002).

   * Haga clic **[!UICONTROL Live]**.

      Consulte [Visualización de la configuración de lanzamiento](../c-about-staging.md#task_401A0EBDB5DB4D4CA933CBA7BECDC10F).

   * Haga clic **[!UICONTROL Push Live]**.

      Consulte [Inserción de la configuración del escenario en directo](../c-about-staging.md#task_44306783B4C0408AAA58B471DAF2D9A4).

## Acerca de las reglas de URL de búsqueda {#concept_017EC95E68844B6C8CC9F874F0EC8D3C}

Las reglas de búsqueda de URL especifican cómo se deben mostrar las direcciones URL en los resultados de búsqueda de sitios web. Las reglas funcionan con direcciones URL completas. Se puede manipular cualquier parte de la dirección URL, incluidos los argumentos de consulta en los que la información del ID de sesión se suele conservar.

<!-- 

c_about_search_url_rules.xml

 -->

Normalmente, las reglas de URL de búsqueda se utilizan para insertar un ID de sesión en una dirección URL. Sin embargo, también puede usar reglas de URL de búsqueda para modificar el nombre de dominio que se muestra con los resultados. De forma predeterminada, no se especifican reglas y no se realiza ninguna modificación de la dirección URL.

Las reglas de URL de búsqueda pueden constar de dos elementos principales: la regla de reescritura y la regla de reescritura opcional. Cuando se incluye una URL como parte de un resultado de búsqueda, las reglas se utilizan para manipularla. Puede especificar un número ilimitado de reglas y condiciones de URL de búsqueda. El orden de estas reglas es importante porque el conjunto de reglas pasa de una regla a otra. Cuando una regla coincide, el software rastrea cualquier condición de reescritura correspondiente (opcional). Las reglas de rastreo de URL se especifican de la siguiente manera:

```
RewriteCond  
<i>TestString CondPattern [Flags]</i> 
RewriteRule  
<i>Pattern Substitution [Flags]</i> 
 
RewriteCond  
<i>TestString CondPattern [Flags]</i> 
RewriteRule  
<i>Pattern Substitution [Flags]</i>
```

Al procesar una dirección URL, la búsqueda/comercialización del sitio intenta hacerla coincidir con el patrón de cada regla de búsqueda. Si la coincidencia falla, el motor de reescritura detiene inmediatamente el procesamiento de la regla y continúa con la siguiente regla del conjunto. Si el patrón coincide, el motor de reescritura busca las instrucciones de RewriteCond correspondientes. Si no existen condiciones, la dirección URL se sustituye por un nuevo valor. Este valor se construye a partir de la cadena de sustitución y continúa con la siguiente regla del conjunto de reglas. Si existen condiciones, el orden en que aparecen en la lista es cómo se procesan. El motor de reescritura intenta hacer coincidir un patrón de condición (CondPattern) con una cadena de prueba (TestString). Si las dos coinciden, la siguiente condición se procesa hasta que no haya más condiciones disponibles. Si todas las condiciones coinciden, la dirección URL se sustituye por la sustitución especificada en la regla. Si no se cumple la condición, falla el conjunto completo de condiciones y la regla correspondiente.

## Acerca de la directiva RewriteRule {#section_A3473B5B90B74DA1970213113A90591E}

Una regla de reescritura adopta la siguiente forma:

```
RewriteRule  
<i>Pattern Substitution [Flags]</i>
```

**** El patrón puede ser una expresión regular POSIX, que se aplica a la dirección URL actual. La &quot;dirección URL actual&quot; puede diferir de la dirección URL original, ya que es posible que las reglas anteriores ya hayan coincidido y lo hayan modificado.

Consulte [Expresiones regulares](../c-appendices/r-regular-expressions.md#reference_B5BA7D61D82E4109A01D2A2D964E3A6A).

Puede utilizar el carácter &quot;no&quot; (&#39;!&#39;) para prefijar el patrón. El carácter &quot;not&quot; permite negar un patrón. En otras palabras, solo es verdadero si la dirección URL actual NO coincide con el patrón. Puede utilizar el carácter &quot;no&quot; cuando sea mejor que coincida con un patrón negativo o como regla predeterminada final. Tenga en cuenta que no se pueden usar tanto el carácter &quot;no&quot; como los comodines agrupados en un patrón. Además, no se puede utilizar un patrón denegado cuando la cadena de sustitución contiene $N.

Puede utilizar paréntesis para crear una referencia secundaria, a la que se puede hacer referencia mediante Substitution y CondPattern.

**** SustituciónLa URL se sustituye completamente por la cadena de sustitución, que puede contener lo siguiente:

Texto sin formato : texto que se pasa sin cambiar.

ReferenciasProporciona acceso a las partes agrupadas (entre paréntesis) del Patrón o CondPattern. Existen dos tipos de referencias secundarias:

Reescribir referencias de regla : coinciden con referencias anteriores en el patrón de regla de reescritura correspondiente y toman el formato $N (0 &lt;= N &lt;= 9). Por ejemplo, `RewriteRule ^My[[:blank:]] (.*)$ ${toupper: $1}`

Referencias de referencias de rewriteCond: coinciden con referencias de atrás en el último CondPattern coincidente con RewriteCond y toman la forma %N (0 &lt;= N &lt;= 9).

Funciones: Estas son funciones del formulario ${NAME_OF_FUNCTION:key} donde NAME_OF_FUNCTION es:

* hace que todos los caracteres de *key* estén en minúsculas.
* toupper escribe todos los caracteres en *key* mayúscula.
* escape URL-codifica todos los caracteres de *key*.
* Los caracteres &quot;a&quot;...z&#39;, &#39;A&#39;..&#39;Z&#39;, &#39;0&#39;...9&#39;, &#39;*&#39;, &#39;-&#39;, &#39;.&#39;, &#39;/&#39;, &#39;@&#39; y &#39;_&#39; no se han modificado; los espacios se traducen en &#39;+&#39;; todos los demás caracteres se transforman en su equivalente con codificación URL %xx.
* unescape transforma &#39;+&#39; de nuevo en espacio y todos los caracteres con codificación URL de %xx de nuevo en caracteres únicos.

>[!NOTE]
>
>Hay una cadena de sustitución especial: &#39;-&#39; que significa &quot;NO substitution&quot;. La cadena &quot;-&quot; suele ser útil junto con el indicador C (cadena). Permite hacer coincidir una dirección URL con varios patrones antes de que se produzca una sustitución.

**Indicadores**  (opcional)

Los indicadores se incluyen entre corchetes `[]`y varios indicadores están separados por coma:

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Indicador </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> 'last|L' </p> </td> 
   <td colname="col2"> <p>Última regla. </p> <p> Detenga el proceso de reescritura y no aplique ninguna regla de reescritura adicional. Utilice este indicador para evitar que se siga procesando la dirección URL actual. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'next|N' </p> </td> 
   <td colname="col2"> <p> La siguiente ronda. </p> <p>Vuelva a ejecutar el proceso de reescritura (empezando de nuevo con la primera regla de reescritura) utilizando la URL de la última regla de reescritura (no la URL original). ¡Ten cuidado de no crear un bucle muerto! </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'chain|C' </p> </td> 
   <td colname="col2"> <p> Encadenado con la regla siguiente. </p> <p>Este indicador encadena la regla actual a la regla siguiente, que también se puede encadenar a la regla siguiente, etc. Si una regla coincide, el proceso de sustitución continúa como de costumbre. Si la regla no coincide, se omiten todas las reglas encadenadas subsiguientes. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'nocase|NC' </p> </td> 
   <td colname="col2"> <p>No hay caso. </p> <p>Este indicador distingue entre mayúsculas y minúsculas en Pattern. En otras palabras, no hay diferencia entre "A-Z" y "a-z" cuando el patrón se compara con la dirección URL actual. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'skip|S=num' </p> </td> 
   <td colname="col2"> <p>Omita la regla o las reglas siguientes. </p> <p> Si la regla actual coincide, este indicador obliga al motor de reescritura a omitir las siguientes reglas de núm del conjunto de reglas. Utilice esto para hacer construcciones pseudo if-then-else. La última regla de la cláusula then se convierte en un skip=N donde N es el número de reglas de la cláusula else. (Nota: ¡Esto no es lo mismo que la bandera 'chain|C'!) </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'env|E=VAR:VAL' </p> </td> 
   <td colname="col2"> <p>Establezca la variable ambiental. </p> <p> Este indicador crea una variable de entorno "VAR" configurada en el valor VAL. VAL puede contener referencias anteriores de expresiones regulares, $N y %N, que están expandidas. Puede utilizar este indicador más de una vez para configurar varias variables. Posteriormente se puede anular la referencia a las variables en un patrón RewriteCond siguiente a través de %{VAR}. Utilice este indicador para eliminar y recordar información de direcciones URL. </p> </td> 
  </tr> 
 </tbody> 
</table>

Tenga en cuenta que las reglas de reescritura de almacenamiento y las reglas de reescritura de recuperación comparten valores de variables. Debido a esto, puede establecer una variable en un valor sessionid que distinga el tiempo cuando se encuentre y almacene una URL incrustada. Cuando se recupera la siguiente dirección URL de la lista de almacenamiento temporal, se puede añadir el valor más reciente de sessionid antes de recuperar la página.

**Ejemplo**

Supongamos que tiene un servidor que distingue entre mayúsculas y minúsculas. Gestiona las cadenas &quot;www.mydomain.com&quot; y &quot;www.MyDomain.com&quot; de forma diferente. Para que su servidor funcione correctamente, debe asegurarse de que el dominio sea siempre &quot;www.mydomain.com&quot; aunque algunos documentos contengan vínculos que hagan referencia a &quot;www.MyDomain.com&quot;. Para ello, puede utilizar la siguiente regla:

```
RewriteRule  ^https:// 
<b>([^/]*)</b> 
<i>(.*)</i>$  https://${tolower:$1}$2 
```

Esta regla de reescritura utiliza la función &quot;tolower&quot; para reescribir la parte de dominio de una URL para asegurarse de que siempre esté en minúsculas:

1. El patrón `(^https://([^/]*)(.*)$)` contiene una referencia **`([^/]*)`** que coincide con todos los caracteres entre &quot;https://&quot; y el primer &quot;/&quot; en la dirección URL. El patrón también contiene una segunda referencia **(.*)** que coincide con todos los caracteres restantes de la dirección URL.

1. La sustitución `(https://${tolower:$1}$2)` indica al motor de búsqueda que vuelva a escribir la dirección URL utilizando la función **tolower** en la primera referencia `(https://**${tolower:$1**}$2)` sin modificar el resto de la dirección URL `(https://${tolower:$1}*$2*)`.

Por lo tanto, una dirección URL del formulario `https://www.MyDomain.com/INTRO/index.Html` se reescribe como `https://www.mydomain.com/INTRO/index.Html`

**Directiva RewriteCond**  (opcional)

La directiva RewriteCond define una condición de regla. Cuando RewriteCond precede a una regla de reescritura, la regla solo se utiliza si su patrón coincide con el título actual y se aplican las condiciones adicionales.

Las directivas de condiciones de reescritura adoptan la siguiente forma:

```
RewriteCond  
<i>TestString CondPattern [Flags]</i>
```

** TestString es una cadena que puede contener las siguientes construcciones:

Texto sin formato: Texto que se pasa sin cambiar.

Las referencias de fondo proporcionan acceso a las partes agrupadas (entre paréntesis) del Patrón o CondPattern. Existen dos tipos de referencias secundarias:

* Referencias de retorno de regla de reescritura : coinciden con las referencias anteriores en el patrón de regla de reescritura correspondiente y toman el formato $N (0 &lt;= N &lt;= 9).

   Por ejemplo, `RewriteRule ^My[[:blank:]] (.*)$ ${toupper: $1}`

* Rereferencias de RewriteCond : coinciden con referencias anteriores en el último CondPattern coincidente de RewriteCond y toman la forma %N (0 &lt;= N &lt;= 9).

Variables Son variables del formulario %{NAME_OF_VARIABLE} donde NAME_OF_VARIABLE puede ser una cadena para el nombre de una variable definida. Consulte el indicador RewriteRule *`[E]`* para obtener más información sobre la configuración de variables.

>[!NOTE]
>
>Las reglas de reescritura generalmente utilizan variables. Todos los parámetros CGI de la dirección URL actual se convierten automáticamente en variables. Por ejemplo, la dirección URL de búsqueda `"https://search.atomz.com/search/?sp_a=sp00000000&sp_q="Product"&session=1234&id=5678"` proporciona automáticamente cuatro variables, a las que se puede hacer referencia en las reglas de reescritura. En este ejemplo, una variable se llama &quot;session&quot; y su valor es &quot;1234&quot;, mientras que otra variable se denomina &quot;id&quot; y su valor es &quot;5678&quot;. (Las otras dos variables son `sp_a` y `sp_q`). Debe pasar todas las variables necesarias como campos ocultos desde el formulario de búsqueda de la página web. En este ejemplo, debe pasar los valores &quot;sesión&quot; e &quot;id&quot;, que identifican al usuario del sitio web que realiza la búsqueda. Para pasar un campo oculto en el formulario de búsqueda, utilice una etiqueta como `<input type=hidden name="session" value="1234">`.

Funciones Estas son funciones del formulario ${NAME_OF_FUNCTION:key} donde NAME_OF_FUNCTION es:

* hace que todos los caracteres de *key* estén en minúsculas.
* toupper escribe todos los caracteres en *key* mayúscula.
* escape URL-codifica todos los caracteres de *key*. Los caracteres &quot;a&quot;...z&#39;, &#39;A&#39;..&#39;Z&#39;, &#39;0&#39;...9&#39;, &#39;*&#39;, &#39;-&#39;, &#39;.&#39;, &#39;/&#39;, &#39;@&#39; y &#39;_&#39; se dejan sin cambios, los espacios se traducen en &#39;+&#39; y todos los demás caracteres se transforman en su equivalente con codificación URL de %xx.
* unescape transforma &#39;+&#39; de nuevo en espacio y todos los caracteres de codificación de URL de %xx vuelven a codificarse en caracteres únicos.

**** CondPatternis es una expresión regular extendida estándar con algunas adiciones. La cadena de patrón puede tener el prefijo &quot;!&quot; carácter (signo de exclamación) para especificar un patrón no coincidente. En lugar de cadenas de expresión regular reales, puede utilizar una de las siguientes variantes especiales.

Puede anteponer todas estas pruebas utilizando un signo de exclamación (&#39;!&#39;) para negar su significado.

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Cadena CondPattern </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> '&lt;CondPattern' </p> </td> 
   <td colname="col2"> <p>Es lexicamente menor. </p> <p> Trata el <i>CondPattern</i> como una cadena sin formato y lo compara lexicamente con <i>TestString</i>. True si <i>TestString</i> es lexicamente menor que <i>CondPattern</i>. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> '&gt;CondPattern' </p> </td> 
   <td colname="col2"> <p>Es lexicamente bueno. </p> <p> Trata el <i>CondPattern</i> como una cadena sin formato y lo compara lexicamente con <i>TestString</i>. True si <i>TestString</i> es lexicamente bueno que <i>CondPattern</i>. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> '=CondPattern' </p> </td> 
   <td colname="col2"> <p>Es lexicamente igual. </p> <p> Trata el <i>CondPattern</i> como una cadena sin formato y lo compara lexicamente con <i>TestString</i>. True si <i>TestString</i> es lexicamente igual a <i>CondPattern</i>. Es decir, las dos cadenas son exactamente iguales (carácter por carácter). Si <i>CondPattern</i> es solo "" (dos comillas), se compara <i>TestString</i> con la cadena vacía. </p> </td> 
  </tr> 
 </tbody> 
</table>

**Indicadores**  (opcional)

Los indicadores se incluyen entre corchetes `[]`y varios indicadores están separados por coma:

&#39;nocase|NC&#39; (sin caso): Esto distingue entre mayúsculas y minúsculas en la prueba. En otras palabras, no hay diferencia entre &quot;A-Z&quot; y &quot;a-z&quot; tanto en el *TestString* expandido como en el *CondPattern*.

&#39;ornext|OR&#39; (o condición siguiente): Utilice esto para combinar las condiciones de regla con una condición OR local en lugar de la Y implícita. Sin este indicador, tendría que escribir el cond/regla varias veces.

**Ejemplo**

Algunas páginas web asignan una variable CGI &quot;sessionid&quot; la primera vez que un cliente llega a un sitio. Esta variable se utiliza para identificar al cliente y, a medida que el cliente navega por el sitio, se pasa la variable . Dado que el robot de búsqueda se parece a un cliente del sitio, se le asigna un número &quot;sessionid&quot;. El robot de búsqueda mantiene este único valor &quot;sessionid&quot;, incluso si una segunda página del sitio intenta asignar un nuevo valor. Para garantizar esto, necesita la siguiente regla de reescritura:

```
RewriteCond  %{sessionid}  .+ 
RewriteRule  ^ 
<b>(.+)</b>sessionid=[^&#]+ 
<i>(.*)</i>$   
<b>$1</b>sessionid=%{sessionid} 
<i>$2</i>
```

El patrón de regla de reescritura contiene dos referencias atrás: (.+) y (.*). La primera referencia inversa coincide con todos los caracteres anteriores a &quot;sessionid=&quot;. La segunda referencia inversa coincide con todos los caracteres después de que el sessionid termine con &#39;&amp;&#39; o &#39;#&#39;.

El patrón de sustitución reescribe la dirección URL utilizando la primera referencia, seguida de la cadena &quot;sessionid=&quot;, seguida del valor de la variable de ID de sesión, que se pasó como un parámetro CGI en la dirección URL, seguido de la segunda referencia. `($1sessionid=%{sessionid}$2)`.

El **RewriteCond** examina la variable sessionid `(%{sessionid})`. Si contiene al menos un carácter (.+), entonces la regla de reescritura coincide.

Por lo tanto, si la consulta de búsqueda es `"https://search.atomz.com/search/?sp_a=sp99999999&sp_q=word&sessionid=5678"`, todas las direcciones URL de los resultados de búsqueda se reescribirán de modo que el valor &quot;sessionid&quot; sea &quot;5678&quot; en lugar del valor &quot;sessionid&quot; que el robot de búsqueda encontró cuando rastreó el sitio y guardó los vínculos.

**Reconocimiento**

El software del motor de reescritura fue originalmente desarrollado por el Grupo Apache para su uso en el proyecto del servidor HTTP Apache (https://www.apache.org/).

## Adición de reglas de URL de búsqueda {#task_50C77D1B53804AEEB20896F74265BD6F}

Puede agregar reglas de URL de búsqueda para especificar cómo se muestran las direcciones URL en los resultados de búsqueda del sitio web. Las reglas funcionan con direcciones URL completas. Puede manipular cualquier parte de la dirección URL, incluidos los argumentos de consulta en los que la información del ID de sesión se conserva a menudo.

<!-- 

t_adding_search_url_rules.xml

 -->

**Para agregar reglas de URL de búsqueda:**

1. En el menú del producto, haga clic en **[!UICONTROL Settings]** > **[!UICONTROL Rewrite Rules]** > **[!UICONTROL Search URL Rules]**.
1. En el campo [!DNL Search URL Rules], introduzca las reglas que desee.

   Se permiten líneas en blanco y líneas de comentarios que empiecen por un carácter &#39;#&#39; (hash).

1. (Opcional) En la página [!DNL Search URL Rules], en el campo [!DNL Test Search URL Rules], introduzca una URL de prueba cuyas reglas de rastreo desee probar y, a continuación, haga clic en **[!UICONTROL Test]**.
1. Haga clic **[!UICONTROL Save Changes]**.
1. (Opcional) Reconstruya el índice del sitio provisional si desea obtener una vista previa de los resultados.

   Consulte [Configuración de un índice incremental de un sitio web provisional](../c-about-index-menu/c-about-incremental-index.md#task_46A367B0786C4C90BFFA5D3F95FD86C0).
1. (Opcional) En la página [!DNL Search URL Rules], realice una de las acciones siguientes:

   * Haga clic en **[!UICONTROL History]** para revertir cualquier cambio que haya realizado.

      Consulte [Uso de la opción Historial](../t-using-the-history-option.md#task_70DD3F87A67242BBBD2CB27156F43002).

   * Haga clic **[!UICONTROL Live]**.

      Consulte [Visualización de la configuración de lanzamiento](../c-about-staging.md#task_401A0EBDB5DB4D4CA933CBA7BECDC10F).

   * Haga clic **[!UICONTROL Push Live]**.

      Consulte [Inserción de la configuración del escenario en directo](../c-about-staging.md#task_44306783B4C0408AAA58B471DAF2D9A4).

## Acerca de las reglas de título de búsqueda {#concept_C72D20F8DFF64EDE809AF4B72797E858}

Las reglas de título de búsqueda especifican cómo se muestran los títulos en los resultados de búsqueda del sitio web. Se puede manipular cualquier parte del título.

<!-- 

c_about_search_title_rules.xml

 -->

Se puede usar una regla de reescritura para eliminar una parte de un título, como un nombre de organización, por ejemplo. De forma predeterminada, la búsqueda/comercialización del sitio no tiene reglas de título y no realiza modificaciones de título.

Las reglas de título pueden constar de dos elementos principales: RewriteRule y RewriteCond opcionales. Se puede especificar un número ilimitado de reglas y condiciones. El orden de estas reglas es importante, ya que el conjunto de reglas pasa de una regla a otra. Cuando una regla coincide, el software rastrea cualquier condición de reescritura correspondiente (opcional). Las reglas de título de búsqueda se especifican de la siguiente manera:

```
RewriteCond  
<i>TestString CondPattern [Flags]</i> 
RewriteRule  
<i>Pattern Substitution [Flags]</i> 
 
RewriteCond  
<i>TestString CondPattern [Flags]</i> 
RewriteRule  
<i>Pattern Substitution [Flags]</i>
```

Cuando se encuentra un título, la búsqueda/comercialización del sitio intenta hacerlo coincidir con el Patrón de cada regla de rastreo. Si el patrón coincide, el motor de reescritura busca las directivas de RewriteCond correspondientes. Si no hay condiciones, el título se sustituye por un nuevo valor construido a partir de la cadena de sustitución y continúa con la siguiente regla del conjunto de reglas. Si existen condiciones, se procesan en el orden en que aparecen en la lista. El motor de reescritura intenta hacer coincidir un patrón de condición (CondPattern) con una cadena de prueba (TestString). Si las dos coinciden, la siguiente condición se procesa hasta que no haya más condiciones disponibles. Si todas las condiciones coinciden, la dirección URL se sustituye por la sustitución especificada en la regla. Si no se cumple la condición, falla el conjunto completo de condiciones y la regla correspondiente.

## Directiva RewriteRule {#section_3BF2B0FF89F74A26AE79D68FA3184B9B}

Cada directiva RewriteRule define una regla de reescritura. Las reglas se aplican en el orden en que aparecen en la lista. Una regla de reescritura adopta la siguiente forma:

```
RewriteRule Pattern Substitution [Flags]
```

**** PatternExpresión regular POSIX, que se aplica al título actual. El &quot;título actual&quot; puede diferir del título original, ya que es posible que las reglas anteriores ya lo hayan hecho y lo hayan modificado.

Consulte [Expresiones regulares](../c-appendices/r-regular-expressions.md#reference_B5BA7D61D82E4109A01D2A2D964E3A6A).

Puede utilizar el carácter &quot;not&quot; (&#39;!&#39;) para prefijar el patrón. El carácter &quot;not&quot; permite negar un patrón. Es decir, que sea verdadero solo si el título actual NO coincide con el patrón. El carácter &quot;not&quot; se puede usar cuando es mejor que coincida con un patrón negativo o como regla predeterminada final. Nota: No se pueden usar tanto el carácter &quot;no&quot; como los comodines agrupados en un patrón. Además, no se puede utilizar un patrón denegado cuando la cadena de sustitución contiene $N.

Puede utilizar paréntesis para crear una referencia secundaria, a la que se puede hacer referencia mediante Substitution y CondPattern.

**** SustituciónEl título se sustituye completamente por la cadena de sustitución, que puede contener lo siguiente:

Texto sin formato : texto que se pasa sin cambiar.

* Referencias de fondo : Proporcione acceso a las partes agrupadas (entre paréntesis) del Patrón o CondPattern. Los siguientes son dos tipos de referencias invertidas:

* Referencias de retorno de regla de reescritura : coinciden con las referencias anteriores en el patrón de regla de reescritura correspondiente y toman el formato $N (0 &lt;= N &lt;= 9).

   Por ejemplo, `RewriteRule ^My[[:blank:]] (.*)$ ${toupper: $1}`

* ** Referencias de RewriteCond** Estas referencias de coincidencia coinciden con las anteriores en el último CondPattern de RewriteCond coincidente y toman la forma %N (0 &lt;= N &lt;= 9).

**** VariablesSon variables con la forma %{NAME_OF_VARIABLE} donde NAME_OF_VARIABLE puede ser una cadena para el nombre de una variable definida. Consulte el indicador [E] para obtener más información sobre la configuración de variables de entorno. Las variables también se pueden definir en el formulario de búsqueda que generó los resultados de la búsqueda.

**** FuncionesSon funciones del formulario ${NAME_OF_FUNCTION: clave} donde NAME_OF_FUNCTION es:

* hace que todos los caracteres de *key* estén en minúsculas.
* toupper escribe todos los caracteres en *key* mayúscula.

Hay una cadena de sustitución especial: &#39;-&#39; que significa &quot;NO substitution&quot;. La cadena &quot;-&quot; suele ser útil junto con el indicador C (cadena), lo que permite hacer coincidir un título con varios patrones antes de que se produzca una sustitución.

**Indicadores**  (opcional)

Los indicadores se incluyen entre corchetes `[]` y varios indicadores están separados por coma:

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Indicador </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> 'last|L' </p> </td> 
   <td colname="col2"> <p> Última regla. </p> <p> Detenga el proceso de reescritura y no aplique ninguna regla de reescritura adicional. Utilice este indicador para evitar que se siga procesando el título actual. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'next|N' </p> </td> 
   <td colname="col2"> <p>La siguiente ronda. </p> <p> Vuelva a ejecutar el proceso de reescritura (comenzando de nuevo con la primera regla de reescritura) utilizando el título de la última regla de reescritura (¡no el título original!). Tenga cuidado de no crear un bucle muerto. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'chain|C' </p> </td> 
   <td colname="col2"> <p>Encadenado con la regla siguiente. </p> <p> Este indicador encadena la regla actual a la siguiente regla (que también se puede encadenar a la regla siguiente, etc.). Si una regla coincide, el proceso de sustitución continúa como de costumbre. Si la regla no coincide, se omiten todas las reglas encadenadas subsiguientes. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'nocase|NC' </p> </td> 
   <td colname="col2"> <p> No hay caso. </p> <p> Este indicador distingue entre mayúsculas y minúsculas en Pattern. Es decir, no hay diferencia entre "A-Z" y "a-z" cuando el patrón se compara con el título actual. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'skip|S=num' </p> </td> 
   <td colname="col2"> <p> Omita la regla o las reglas siguientes. </p> <p> Si la regla actual coincide, este indicador obliga al motor de reescritura a omitir las siguientes reglas de núm del conjunto de reglas. Utilice esto para hacer construcciones pseudo if-then-else. La última regla de la cláusula then se convierte en un skip=N donde N es el número de reglas de la cláusula else. (Esto no es lo mismo que la bandera 'chain|C'!) </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'env|E=VAR:VAL' </p> </td> 
   <td colname="col2"> <p>Establezca la variable de entorno. </p> <p> Este indicador crea una variable de entorno "VAR" configurada en el valor VAL, donde VAL puede contener referencias atrás de expresiones regulares, $N y %N, que se expandirán. Puede utilizar este indicador más de una vez para configurar varias variables. Se puede hacer referencia a las variables más adelante en el siguiente patrón RewriteCond mediante %{VAR}. Utilice este indicador para eliminar y recordar información de títulos. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Directiva RewriteCond (opcional) {#section_9D72B2AB454849A7B681BC39C506AAA3}

La directiva RewriteCond define una condición de regla. Cuando RewriteCond precede a una regla de reescritura, la regla solo se utiliza si su patrón coincide con el título actual y se aplican las condiciones adicionales.

Las directivas de condiciones de reescritura adoptan la siguiente forma:

```
RewriteCond TestString CondPattern [Flags]
```

**** TestString es una cadena que puede contener las siguientes construcciones:

Texto sin formato : texto que se pasa sin cambiar.

Las referencias de fondo proporcionan acceso a las partes agrupadas (entre paréntesis) del Patrón o CondPattern. Existen dos tipos de referencias secundarias:

* Referencias de retorno de regla de reescritura : coinciden con las referencias anteriores en el patrón de regla de reescritura correspondiente y toman el formato $N (0 &lt;= N &lt;= 9).

   Por ejemplo, `RewriteRule ^My[[:blank:]] (.*)$ ${toupper: $1}`

* Rereferencias de RewriteCond : coinciden con referencias anteriores en el último CondPattern coincidente de RewriteCond y toman la forma %N (0 &lt;= N &lt;= 9).

**** VariablesSon variables con la forma %{NAME_OF_VARIABLE} donde NAME_OF_VARIABLE puede ser una cadena para el nombre de una variable definida. Consulte el indicador `[E]` para obtener más información sobre la configuración de variables de entorno. Las variables también se pueden definir en el formulario de búsqueda que generó los resultados de la búsqueda.

**** FuncionesSon funciones del formulario ${NAME_OF_FUNCTION:key} donde NAME_OF_FUNCTION es:

* hace que todos los caracteres de *key* estén en minúsculas.
* toupper escribe todos los caracteres en *key* mayúscula.
* escape URL-codifica todos los caracteres de *key*.
* Los caracteres &quot;a&quot;...z&#39;, &#39;A&#39;..&#39;Z&#39;, &#39;0&#39;...9&#39;, &#39;*&#39;, &#39;-&#39;, &#39;.&#39;, &#39;/&#39;, &#39;@&#39; y &#39;_&#39; se dejan sin cambios, los espacios se traducen en &#39;+&#39; y todos los demás caracteres se transforman en su equivalente con codificación URL de %xx.
* unescape transforma &#39;+&#39; de nuevo en espacio y todos los caracteres con codificación URL de %xx de nuevo en caracteres únicos.

Hay una cadena de sustitución especial: &#39;-&#39; que significa &quot;NO substitution&quot;. La cadena &quot;-&quot; suele ser útil junto con el indicador C (cadena), lo que permite hacer coincidir una dirección URL con varios patrones antes de que se produzca una sustitución.

**** CondPattern Expresión regular extendida estándar con algunas adiciones. La cadena de patrón puede tener el prefijo &quot;!&quot; carácter (signo de exclamación) para especificar un patrón no coincidente. En lugar de cadenas de expresión regular reales, puede utilizar una de las siguientes variantes especiales.

Todas estas pruebas también pueden ir acompañadas de un signo de exclamación (&#39;!&#39;) para negar su significado.

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Cadena CondPattern </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> '&lt;CondPattern' </p> </td> 
   <td colname="col2"> <p>Es lexicamente menor. </p> <p> Trata el <i>CondPattern</i> como una cadena sin formato y lo compara lexicamente con <i>TestString</i>. True si <i>TestString</i> es lexicamente menor que <i>CondPattern</i>. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> '&gt;CondPattern' </p> </td> 
   <td colname="col2"> <p> Es lexicamente bueno. </p> <p> Trata el <i>CondPattern</i> como una cadena sin formato y lo compara lexicamente con <i>TestString</i>. True si <i>TestString</i> es lexicamente bueno que <i>CondPattern</i>. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> '=CondPattern' </p> </td> 
   <td colname="col2"> <p>Es lexicamente igual. </p> <p> Trata el <i>CondPattern</i> como una cadena sin formato y lo compara lexicamente con <i>TestString</i>. True si <i>TestString</i> es lexicamente igual a <i>CondPattern</i>. Es decir, las dos cadenas son exactamente iguales (carácter por carácter). Si <i>CondPattern</i> es solo "" (dos comillas), se compara <i>TestString</i> con la cadena vacía. </p> </td> 
  </tr> 
 </tbody> 
</table>

**Indicadores**  (opcional)

Los indicadores se incluyen entre corchetes`[]` y varios indicadores están separados por coma:

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Indicador </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> 'nocase|NC' (sin caso) </p> </td> 
   <td colname="col2"> <p>Hace que la prueba no sea sensible. Es decir, no hay diferencia entre "A-Z" y "a-z" tanto en el <i>TestString</i> expandido como en el <i>CondPattern</i>. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> 'ornext|OR' (o condición siguiente) </p> </td> 
   <td colname="col2"> <p> Utilice esto para combinar las condiciones de regla con una condición OR local en lugar de la Y implícita. Sin este indicador, tendría que escribir el cond/regla varias veces. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Ejemplo {#section_E7454FFE169E459AABD9D033651939CB}

Supongamos que tiene un sitio Web corporativo con un formato de título estándar: &quot;Mi empresa&quot; seguida de un guión y, a continuación, una descripción específica de la página (&quot;Mi empresa - bienvenida&quot; o &quot;Mi empresa - noticias&quot;, por ejemplo). Desea eliminar &quot;Mi empresa -&quot; del título y convertir todo el título en letras mayúsculas cuando indexa el sitio.

La siguiente regla de reescritura utiliza la función toupper para reescribir solo la parte descriptiva de un título en mayúsculas:

```
RewriteRule  ^My[[:blank:]]Company[[:blank:]]-[[:blank:]] 
<b>(.*)</b>$  ${toupper: 
<b>$1</b>} 
```

El patrón de la regla `(^My[[:blank:]]Company[[:blank:]]-[[:blank:]] (.*))` contiene una referencia **`(.*)`** que coincide con el contenido del título que sigue a &quot;Mi empresa-&quot;. Recuerde que rodear una parte de un patrón con paréntesis ( ) crea una referencia secundaria a la que la sustitución puede hacer referencia. En este ejemplo, la sustitución

`(${toupper:**$1**})`

reescribe esa referencia de retorno (**$1**) utilizando la función de toupper.

Por lo tanto, un título del formulario &quot;Mi empresa - bienvenida&quot; se reescribe como &quot;BIENVENIDO&quot;.

**Reconocimiento**

El software del motor de reescritura fue originalmente desarrollado por el Grupo Apache para su uso en el proyecto del servidor HTTP Apache (https://www.apache.org/).

## Adición de reglas de título de búsqueda {#task_155CECB74BE3444384EDBBD04F41515E}

Puede agregar reglas de título de búsqueda para especificar cómo se muestran los títulos en los resultados de búsqueda del sitio web. Puede manipular cualquier parte del título.

<!-- 

t_adding_search_title_rules.xml

 -->

**Para agregar reglas de título de búsqueda:**

1. En el menú del producto, haga clic en **[!UICONTROL Settings]** > **[!UICONTROL Rewrite Rules]** > **[!UICONTROL Search Title Rules]**.
1. En el campo [!DNL Search Title Rules], introduzca las reglas que desee.

   Se permiten líneas en blanco y líneas de comentarios que empiecen por un carácter &#39;#&#39; (hash).
1. (Opcional) En la página [!DNL Search Title Rules], en el campo [!DNL Test Search Title Rules], introduzca un título de prueba y haga clic en **[!UICONTROL Test]**.
1. Haga clic **[!UICONTROL Save Changes]**.
1. (Opcional) Reconstruya el índice del sitio provisional si desea obtener una vista previa de los resultados.

   Consulte [Configuración de un índice incremental de un sitio web provisional](../c-about-index-menu/c-about-incremental-index.md#task_46A367B0786C4C90BFFA5D3F95FD86C0).
1. (Opcional) En la página [!DNL Search Title Rules], realice una de las acciones siguientes:

   * Haga clic en **[!UICONTROL History]** para revertir cualquier cambio que haya realizado.

      Consulte [Uso de la opción Historial](../t-using-the-history-option.md#task_70DD3F87A67242BBBD2CB27156F43002).

   * Haga clic **[!UICONTROL Live]**.

      Consulte [Visualización de la configuración de lanzamiento](../c-about-staging.md#task_401A0EBDB5DB4D4CA933CBA7BECDC10F).

   * Haga clic **[!UICONTROL Push Live]**.

      Consulte [Inserción de la configuración del escenario en directo](../c-about-staging.md#task_44306783B4C0408AAA58B471DAF2D9A4).
