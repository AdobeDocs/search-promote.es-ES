---
description: Aprenda a personalizar la salida en cualquier formato basado en texto, incluido XML o JSON.
solution: Target
title: Resultados de la búsqueda guiada
topic-legacy: Appendices,Site search and merchandising
uuid: 234fd563-f249-42b0-88ca-c89b44f8df77
exl-id: 5e36b30f-defb-4a28-8516-53cea81d44c2
translation-type: tm+mt
source-git-commit: 7559f5f7437d46e3510d4659772308666425ec96
workflow-type: tm+mt
source-wordcount: '6284'
ht-degree: 2%

---

# Resultados de la búsqueda guiada{#guided-search-output}

Puede personalizar la salida en cualquier formato basado en texto, incluso XML o JSON.

## Uso de la salida Búsqueda guiada {#concept_2A1BA3AD413848A1AC2A3ABC4FFE481F}

El formato de salida es personalizable para admitir las decisiones específicas de facetas, clasificación y otras implementaciones que se adoptan durante el proceso de diseño. Puede adaptar el propio formato para simplificar el desarrollo del front-end del cliente, si es necesario.

El resultado completo está contenido en etiquetas `<result>` y la mayoría de los datos dinámicos se incluyen entre etiquetas `<![CDATA[ ]]>`. Esta organización permite que los resultados contengan HTML y otras entidades no XML.

Cuando se proporcionan vínculos a otras páginas, estos se presentan en forma de una dirección URL relativa. Este resultado también incluye los parámetros de cadena de consulta que se pasan para generar el resultado deseado.

## Explicación de una implementación de Búsqueda guiada {#section_95483980930C4325BAB50A40BD47245A}

Cuando inicie una implementación de Búsqueda guiada, recuerde que [!DNL Adobe Search&Promote] es responsable de la capa comercial. Es decir, la lógica que rodea qué resultados y facetas se muestran a un cliente en un momento dado.

Cuando implemente el front-end de la aplicación web que analiza y muestra los resultados como HTML, restrinja la funcionalidad para que solo se muestre. En otras palabras, cualquier lógica del lado del servidor que utilice para crear la capa de presentación no toma las decisiones sobre qué presentar a un cliente, a menos que sea necesario. Las reglas comerciales no funcionarán como se espera si el script front-end está modificando los resultados de búsqueda.

[!DNL Adobe Search&Promote] mantiene el estado del usuario de las opciones de perfeccionamiento de búsqueda seleccionadas mediante los parámetros de URL. Todos los nodos `<link>` contienen los parámetros relevantes de las selecciones del cliente. Estos parámetros pueden incluir la ruta de exploración, la paginación, la ordenación y las selecciones de facetas. Donde corresponda, se devuelven `<undolink>` nodos para permitir que un cliente &quot;vuelva a estar&quot; fuera de una selección. Las facetas y las rutas de exploración ofrecen estos tipos de vínculos.

## Uso del servidor de búsqueda {#section_8DBEACDECD714E59BDED6315E6041B8D}

Se utiliza una API similar a REST con la que puede interactuar para realizar búsquedas y recibir resultados. Los formatos más comunes utilizados para los resultados son XML o JSON.

El URI base está asociado a una cuenta específica y a un entorno de ensayo o activo. Puede solicitar varios alias para el URI base desde el administrador de cuentas. Por ejemplo, una empresa ficticia llamada Megacorp tiene las dos URL base siguientes asociadas a su cuenta:

* `https://search.megacorp.com `
* `https://stage.megacorp.com`

La antigua URI realiza búsquedas con su índice activo y la segunda URI con su índice escalonado.

Las solicitudes de búsqueda consisten en el URI base y un conjunto de parámetros CGI o pares de clave-valor que indican la búsqueda deseada para la cuenta asociada al URI base.

Se admiten tres formatos de parámetros CGI. De forma predeterminada, su cuenta está configurada para separar parámetros CGI con un punto y coma ( `;`), como en el siguiente ejemplo:

* `https://search.megacorp.com?q=shoes ;page=2`

Si lo prefiere, puede hacer que su administrador de cuentas configure su cuenta para que utilice el símbolo &amp; ( `&`) para separar los parámetros CGI, como en el siguiente ejemplo:

* `https://search.megacorp.com?q=shoes &page=2`

También se admite un tercer formato, denominado formato SEO, en el que se utiliza una barra diagonal ( `/`) en lugar del separador y el signo igual para generar vínculos &quot;limpios&quot;, como en el siguiente ejemplo:

* `https://search.megacorp.com/q/shoes/page/2`

Cada vez que se utiliza el formato SEO para enviar una solicitud, todos los vínculos de salida se devuelven en el mismo formato.

## Parámetros de consulta de búsqueda {#section_7ADA5E130E3040C9BE85D0D68EDD3223}

En la tabla siguiente se describen los parámetros de consulta de búsqueda estándar &quot;predeterminados&quot; que puede utilizar. Las reglas de procesamiento y las reglas comerciales se pueden crear en función de parámetros de consulta definidos por el usuario para implementar una lógica empresarial personalizada que sea relevante para su empresa. Puede trabajar con el equipo de consultoría para obtener documentación sobre esos parámetros.

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Parámetro de consulta de búsqueda </p> </th> 
   <th colname="col2" class="entry"> <p>Ejemplo </p> </th> 
   <th colname="col3" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> q </span> </p> </td> 
   <td colname="col2"> <p> <span class="codeph"> q= cadena  </span> </p> </td> 
   <td colname="col3"> <p> Especifica la cadena de consulta para la búsqueda. Este parámetro se asigna al parámetro de búsqueda back-end <span class="codeph"> sp_q </span> . </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> q# </span> </p> </td> 
   <td colname="col2"> <p> <span class="codeph"> q#= cadena  </span> </p> </td> 
   <td colname="col3"> <p>Los parámetros <span class="codeph"> q </span> y <span class="codeph"> x </span> numerados realizan facetas o búsquedas dentro de un campo determinado. </p> <p>El parámetro <span class="codeph"> q </span> define el término que está buscando en la faceta como indica el parámetro <span class="codeph"> x </span> numerado correspondiente. Por ejemplo, si tiene dos facetas con nombres tamaño y color, podría tener algo así como lo siguiente: </p> <p> <span class="codeph"> q1=small;x1=size;q2=red;x2=color  </span> </p> <p>Este parámetro se asigna a los parámetros de búsqueda de back-end <span class="codeph"> sp_q_specific_# </span> . </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> x# </span> </p> </td> 
   <td colname="col2"> <p> <span class="codeph"> x#= cadena  </span> </p> </td> 
   <td colname="col3"> <p> Los parámetros <span class="codeph"> q </span> y <span class="codeph"> x </span> numerados realizan facetas o búsquedas dentro de un campo determinado. </p> <p>El parámetro <span class="codeph"> q </span> define el término que está buscando en la faceta como indica el parámetro <span class="codeph"> x </span> numerado correspondiente. Por ejemplo, si tiene dos facetas con nombres tamaño y color, podría tener algo así como lo siguiente: </p> <p> <span class="codeph"> q1=small;x1=size;q2=red;x2=color  </span> </p> <p>Este parámetro se asigna a los parámetros de búsqueda back-end <span class="codeph"> sp_x_# </span> . </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> colección </span> </p> </td> 
   <td colname="col2"> <p> <span class="codeph"> collection= string  </span> </p> </td> 
   <td colname="col3"> <p> Especifica la colección que se usará para la búsqueda. Este parámetro se asigna al parámetro de búsqueda back-end <span class="codeph"> sp_k </span> . </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> count  </span> </p> </td> 
   <td colname="col2"> <p> <span class="codeph"> count= number  </span> </p> </td> 
   <td colname="col3"> <p> Especifica el recuento total de resultados que se muestran. El valor predeterminado se define en <span class="uicontrol"> Configuración </span> &gt; <span class="uicontrol"> Búsqueda </span> &gt; <span class="uicontrol"> Búsquedas </span>. Este parámetro se asigna al parámetro de búsqueda back-end <span class="codeph"> sp_c </span> . </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> page </span> </p> </td> 
   <td colname="col2"> <p> <span class="codeph"> page= number  </span> </p> </td> 
   <td colname="col3"> <p> Especifica la página de resultados que se devuelven. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> rank  </span> </p> </td> 
   <td colname="col2"> <p> <span class="codeph"> rank= campo  </span> </p> </td> 
   <td colname="col3"> <p> Especifica el campo de clasificación que se utilizará para la clasificación estática. El campo debe ser un campo de tipo Clasificación con relevancia buena a 0. Este parámetro se asigna al parámetro back-end <span class="codeph"> sp_sr </span> . </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> gs_store  </span> </p> </td> 
   <td colname="col2"> <p> <span class="codeph"> gs_store= cadena  </span> </p> </td> 
   <td colname="col3"> <p> Especifica la tienda que se va a buscar. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> sort  </span> </p> </td> 
   <td colname="col2"> <p> <span class="codeph"> sort= number  </span> </p> </td> 
   <td colname="col3"> <p> Especifica el orden. "0" es el valor predeterminado y se ordena por puntuación de relevancia; "1" se clasifica por fecha; "-1" no se ordena. </p> <p>Los usuarios pueden especificar un nombre de campo para el valor del parámetro <span class="codeph"> sp_s </span>. Por ejemplo, <span class="codeph"> sp_s=title </span> ordena los resultados según los valores contenidos en el campo de título. Cuando se utiliza un nombre de campo para el valor de un parámetro <span class="codeph"> sp_s </span>, los resultados se ordenan por ese campo y luego se subordenan por relevancia. </p> <p>Para habilitar esta función, haga lo siguiente: </p> 
    <ol id="ol_3894F81EA7BF4827A84DE8662111ABEF"> 
     <li id="li_C040C0B88F174A4885E1A8E721FD032A">En el menú del producto, haga clic en <span class="uicontrol"> Configuración </span> &gt; <span class="uicontrol"> Metadatos </span> &gt; <span class="uicontrol"> Definiciones </span>. </li> 
     <li id="li_2E83C3A46D1B4BF991EABAD9D3E52B7D">En la página <span class="wintitle"> Definiciones por etapas </span>, realice una de las siguientes acciones: 
      <ul id="ul_8018FEE10E0A4C96A74F84A897080580"> 
       <li id="li_E9A7CE43E2734F4D9522A1283CA111FB">Haga clic en <span class="uicontrol"> Agregar nuevo campo </span>. </li> 
       <li id="li_9D2434A321924FBD874569CA9AD2EEF7">Haga clic en <span class="uicontrol"> Editar </span> para un nombre de campo determinado. </li> 
      </ul> </li> 
     <li id="li_90D5E3F4AC0A4A6189934A5589F69903">En la lista desplegable <span class="wintitle"> Ordenar </span>, haga clic en <span class="uicontrol"> Ascendente </span> o <span class="uicontrol"> Descendente </span>. <p>Este parámetro se asigna al parámetro de búsqueda back-end <span class="codeph"> sp_s </span> . </p> </li> 
    </ol> </td> 
  </tr> 
 </tbody> 
</table>

## Integración con su sistema {#section_91261B19A44A4E579B3FC9FAB9AD3665}

Las siguientes son recomendaciones para la integración con su sistema.

* Comunicarse con el servidor de búsqueda.

   Puede comunicarse con los servidores web [!DNL Adobe Search&Promote] mediante solicitudes de GET http. Sus servidores generan estas solicitudes o en el lado del cliente haciendo una solicitud de Ajax.
* Guardar el historial de búsqueda.

[!DNL Adobe Search&Promote] no tiene estado donde se pasa todo el estado en la solicitud http.
* Analizando los resultados devueltos.

   Se recomienda utilizar un analizador XML basado en SAX para analizar la respuesta XML. Si está generando una solicitud de Ajax, configure [!DNL Adobe Search&Promote] para devolver respuestas JSON para esas solicitudes con el fin de facilitar el análisis de la respuesta.

## Salida JSON de búsqueda guiada {#reference_EB8182A564DE4374BB84158F2AABEF74}

Tablas que describen el resultado de respuesta de JSON estándar.

Consulte también [Salida JSON de búsqueda guiada](../c-appendices/c-guidedsearchoutput.md#reference_EB8182A564DE4374BB84158F2AABEF74).

Puede revisar la respuesta de JSON para lo siguiente:

* [Banners](../c-appendices/c-guidedsearchoutput.md#section_88519CAAD25F4BD49D5E517077745B0E)
* [Ruta de navegación](../c-appendices/c-guidedsearchoutput.md#section_A7DB0F1DA9ED4CBCAE18395122F3E01E)
* [Facetas](../c-appendices/c-guidedsearchoutput.md#section_65932C95931743A1BFAF1DF16D7E6D92)
* [Encabezado y consulta](../c-appendices/c-guidedsearchoutput.md#section_1D57062259CA46E0B4F598FA4EB37065)
* [Paginación](../c-appendices/c-guidedsearchoutput.md#section_504E7AB570BD49AF9839530DC438EE96)
* [Búsquedas recientes](../c-appendices/c-guidedsearchoutput.md#section_525816A0355C48F8970D89B8FC3F1FFF)
* [Resultados](../c-appendices/c-guidedsearchoutput.md#section_41AC56BB0A084BF59379B06C8BEF2157)
* [Buscar formulario](../c-appendices/c-guidedsearchoutput.md#section_434DA13EA295474C99FFE9F14801CD0E)
* [Ordenar](../c-appendices/c-guidedsearchoutput.md#section_558853CD376F4D71BACF211D53085D55)
* [Sugerencias](../c-appendices/c-guidedsearchoutput.md#section_6EC104E1DDD94AC799B65E6E61A2FB3C)
* [Zonas](../c-appendices/c-guidedsearchoutput.md#section_AE53A498B440465EAF2286F2AE87D548)

## Banners {#section_88519CAAD25F4BD49D5E517077745B0E}

Ejemplo:

```xml
<banners> 
 <banner> 
  <area><![CDATA[top-left]]></area> 
  <content><![CDATA[<img src="https://www.megacorp.com/discount.gif"/>]]></content> 
 </banner> 
</banners>
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Etiquetas en titulares </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;banner&gt; </span> </p> </td> 
   <td colname="col2"> <p> Un nodo de banner individual. Puede tener varios nodos de banner. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;area&gt; </span> </p> </td> 
   <td colname="col2"> <p> El área de la página web donde se muestra el banner. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;content&gt; </span> </p> </td> 
   <td colname="col2"> <p> El contenido HTML del área del banner. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Ruta de navegación {#section_A7DB0F1DA9ED4CBCAE18395122F3E01E}

En el siguiente ejemplo, cada vez que el cliente se reduce más a través de las facetas, la selección se agrega a la ruta de exploración. Cada elemento se representa como `<breadcrumb-item>`.

Ejemplo:

```xml
 <breadcrumb> 
  <breadcrumb-item> 
   <link><![CDATA[?q=new+year]]></link> 
   <value><![CDATA[new year]]></value> 
  </breadcrumb-item> 
  <breadcrumb-item> 
   <link><![CDATA[?q=new+year;q1=Articles;x1=content-type]]></link> 
   <value><![CDATA[Articles]]></value> 
  </breadcrumb-item> 
 </breadcrumb> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Etiquetas en la ruta de navegación </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;link&gt; </span> </p> </td> 
   <td colname="col2"> <p> Vínculo relativo a los resultados de búsqueda que muestra la vista deseada. Al hacer clic en un vínculo de ruta de exploración, el cliente llega a una vista en la que se eliminan todas las mejoras posteriores. También hay otras opciones disponibles. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;value&gt; </span> </p> </td> 
   <td colname="col2"> <p> Texto orientado al cliente para el elemento de ruta de exploración. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Facetas {#section_65932C95931743A1BFAF1DF16D7E6D92}

Las facetas son opciones de refinamiento que permiten a los clientes filtrar los resultados. Las facetas suelen utilizarse para la categorización, intervalos de precios, selecciones de color y otro refinamiento de atributos. Los metadatos del índice son lo que impulsa las facetas.

Es habitual ocultar o mostrar facetas de categorización a medida que un cliente baja por la categorización. El nivel más alto de categorización (categoría) se conoce como Nivel 1. Cuando un cliente hace clic en una opción de Nivel 1, aparecen las opciones de refinamiento de Nivel 2 (subcategoría) y desaparecen las opciones de Nivel 1. Cuando un cliente hace clic en una opción de Nivel 2, aparecen las opciones de refinamiento de Nivel 3 (subcategoría) y desaparecen las opciones de Nivel 2. Como se ha indicado anteriormente, estas opciones están ocultas y se muestran: su aplicación web no se ve afectada por ellas.

Cada faceta está contenida dentro de etiquetas `<facet-item>` . En el siguiente ejemplo, muestra una faceta que permite al cliente refinar los resultados de búsqueda por &quot;vacaciones&quot;.

Ejemplo:

```xml
 <facets> 
  <facet-item> 
   <facet-title><![CDATA[Holidays]]></facet-title> 
   <facet-value> 
    <label><![CDATA[New Year]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=New+Year;x1=content-type;x2=holidays]]></link> 
    <count><![CDATA[11]]></count> 
   </facet-value> 
   <facet-value> 
    <label><![CDATA[Christmas]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=Christmas;x1=content-type;x2=holidays]]></link> 
    <count><![CDATA[7]]></count> 
   </facet-value> 
   <facet-value> 
    <label><![CDATA[Chinese New Year]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=Chinese+New+Year;x1=content-type;x2=holidays]]></link> 
    <count><![CDATA[2]]></count> 
   </facet-value> 
   <facet-value> 
    <label><![CDATA[Thanksgiving]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=Thanksgiving;x1=content-type;x2=holidays]]></link> 
    <count><![CDATA[2]]></count> 
   </facet-value> 
   <facet-value> 
    <label><![CDATA[4th of July]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=4th+of+July;x1=content-type;x2=holidays]]></link> 
    <count><![CDATA[1]]></count> 
   </facet-value> 
   <facet-value> 
    <label><![CDATA[Father&#39;s Day]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=Father's+Day;x1=content-type;x2=holidays]]></link> 
    <count><![CDATA[1]]></count> 
   </facet-value> 
   <facet-value> 
    <label><![CDATA[Hanukkah]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=Hanukkah;x1=content-type;x2=holidays]]></link> 
    <count><![CDATA[1]]></count> 
   </facet-value> 
   <facet-value> 
    <label><![CDATA[Mother&#39;s Day]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=Mother's+Day;x1=content-type;x2=holidays]]></link> 
    <count><![CDATA[1]]></count> 
   </facet-value> 
   <facet-value> 
    <label><![CDATA[Valentine&#39;s Day]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=Valentine's+Day;x1=content-type;x2=holidays]]></link> 
    <count><![CDATA[1]]></count> 
   </facet-value> 
  </facet-item> 
  <facet-item> 
   <facet-title><![CDATA[Seasons]]></facet-title> 
   <facet-value> 
    <label><![CDATA[Winter]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=Winter;x1=content-type;x2=seasons]]></link> 
    <count><![CDATA[20]]></count> 
   </facet-value> 
   <facet-value> 
    <label><![CDATA[Summer]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=Summer;x1=content-type;x2=seasons]]></link> 
    <count><![CDATA[7]]></count> 
   </facet-value> 
   <facet-value> 
    <label><![CDATA[Autumn]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=Autumn;x1=content-type;x2=seasons]]></link> 
    <count><![CDATA[4]]></count> 
   </facet-value> 
   <facet-value> 
    <label><![CDATA[Spring]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=Spring;x1=content-type;x2=seasons]]></link> 
    <count><![CDATA[2]]></count> 
   </facet-value> 
  </facet-item> 
 </facets> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Etiquetas en facetas </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;facet-title&gt; </span> </p> </td> 
   <td colname="col2"> <p> Título orientado al cliente para la faceta. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;label&gt; </span> </p> </td> 
   <td colname="col2"> <p> Etiqueta de cara al cliente para la opción faceta. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;link&gt; </span> </p> </td> 
   <td colname="col2"> <p> Vínculo relativo a los resultados que la opción refina. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;count&gt; </span> </p> </td> 
   <td colname="col2"> <p> Número de resultados en ese conjunto de resultados refinado. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;undolink&gt; </span> </p> </td> 
   <td colname="col2"> <p> Cuando se selecciona un valor de faceta, el nodo devuelve un "vínculo de deshacer" que permite que un cliente vuelva a estar fuera de los resultados. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Encabezado y consulta {#section_1D57062259CA46E0B4F598FA4EB37065}

Ejemplo:

```xml
<result> 
 <query> 
  <user-query><![CDATA[new year]]></user-query> 
  <lower-results><![CDATA[1]]></lower-results> 
  <upper-results><![CDATA[16]]></upper-results> 
  <total-results><![CDATA[621]]></total-results> 
 </query> 
```

En conjunto, estas etiquetas presentan un mensaje como el siguiente: &quot;Mostrando los resultados 1-16 de 621 para &#39;año nuevo&#39;&quot;.

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Etiquetas en el encabezado y la consulta </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;user-query&gt; </span> </p> </td> 
   <td colname="col2"> <p> La consulta de palabra clave que se envía con la solicitud. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;lower-results&gt; </span> </p> </td> 
   <td colname="col2"> <p> El número de artículo del primer resultado de esta página. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;upper-results&gt; </span> </p> </td> 
   <td colname="col2"> <p> El número de artículo del último resultado de esta página. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;total-results&gt; </span> </p> </td> 
   <td colname="col2"> <p> Número total de resultados que coinciden con la consulta del usuario. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;custom-field&gt; </span> </p> </td> 
   <td colname="col2"> <p> Campo opcional que se aplica globalmente a los resultados de búsqueda. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Paginación {#section_504E7AB570BD49AF9839530DC438EE96}

Ejemplo:

```xml
<pagination> 
 <total-pages><39></total-pages> 
 <pages> 
   <page position="first"></page> 
   <page position="last">?i=1;page=39;q=new+year;q1=Articles;x1=content-type]]></page> 
   <page position="previous"></page> 
   <page position="next">?i=1;page=2;q=new+year;q1=Articles;x1=content-type]]></page> 
   <page position="1" selected="true">?i=1;q=new+year;q1=Articles;x1=content-type]]></page> 
   <page position="2">?i=1;page=2;q=new+year;q1=Articles;x1=content-type]]></page> 
   <page position="3">?i=1;page=3;q=new+year;q1=Articles;x1=content-type]]></page> 
   <page position="4">?i=1;page=4;q=new+year;q1=Articles;x1=content-type]]></page> 
   <page position="5">?i=1;page=5;q=new+year;q1=Articles;x1=content-type]]></page> 
   <page position="6">?i=1;page=6;q=new+year;q1=Articles;x1=content-type]]></page> 
   <page position="7">?i=1;page=7;q=new+year;q1=Articles;x1=content-type]]></page> 
   <page position="8">?i=1;page=8;q=new+year;q1=Articles;x1=content-type]]></page> 
   <page position="9">?i=1;page=9;q=new+year;q1=Articles;x1=content-type]]></page> 
   <page position="10">?i=1;page=10;q=new+year;q1=Articles;x1=content-type]]></page> 
 </pages> 
</pagination> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Etiquetas en la paginación </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;total-pages&gt; </span> </p> </td> 
   <td colname="col2"> <p> Número total de páginas de resultados, según el número de resultados dividido por el número de resultados por página. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;page position="first"&gt; </span> </p> </td> 
   <td colname="col2"> <p> Contiene un vínculo relativo a la primera página del conjunto de resultados, a menos que el cliente ya esté viendo la página 1. En tal caso, está en blanco. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;page position="last"&gt; </span> </p> </td> 
   <td colname="col2"> <p> Contiene un vínculo relativo a la última página del conjunto de resultados, a menos que el cliente esté viendo la última página. En tal caso, está en blanco. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;page position="previous"&gt; </span> </p> </td> 
   <td colname="col2"> <p> Contiene un vínculo relativo a la página anterior del conjunto de resultados, a menos que el cliente esté viendo la página 1; en tal caso, está en blanco. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;page position="next"&gt; </span> </p> </td> 
   <td colname="col2"> <p> Contiene un vínculo relativo a la última página del conjunto de resultados, a menos que el cliente esté viendo la última página. En tal caso, está en blanco. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;page position="x"&gt;</span> </p> </td> 
   <td colname="col2"> <p> Contiene un vínculo relativo a un número de página en particular. Se muestran diez números de página contiguos. En la página 1, serían las páginas 1-10. Al final del conjunto de resultados (en este caso, 39), serían las páginas 30-39. Por ejemplo, en el centro del conjunto de resultados, la página 15, serían las páginas 11-20. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> selected="true"&gt;  </span> </p> </td> 
   <td colname="col2"> <p> Se aplica como atributo a la página seleccionada actualmente. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Búsquedas recientes {#section_525816A0355C48F8970D89B8FC3F1FFF}

Búsquedas recientes es una función basada en cookies que solo funciona si reenvía la información de las cookies a los servidores.

Ejemplo:

```xml
<recent-searches> 
 <recent-search> 
  <search-term><![CDATA[shoes]]></search-term> 
  <link><![CDATA[?q=shoes]]></link> 
 </recent-search> 
</recent-searches> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Etiquetas en búsquedas recientes </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;recent-search&gt; </span> </p> </td> 
   <td colname="col2"> <p> Un nodo de búsqueda reciente individual. Puede tener varios nodos de búsqueda reciente. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;search-term&gt; </span> </p> </td> 
   <td colname="col2"> <p> Término que el cliente buscó anteriormente. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;link&gt; </span> </p> </td> 
   <td colname="col2"> <p> Vínculos a la búsqueda anterior. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Resultados {#section_41AC56BB0A084BF59379B06C8BEF2157}

El conjunto de resultados es un área personalizable de la respuesta JSON. Cada índice es único en los mecanismos de asignación de nombres de campo de los metadatos. Hay campos comunes que se devuelven para cada resultado, como título, descripción y dirección URL. Sin embargo, los metadatos definidos para una página del índice pueden estar disponibles para su uso en cada nodo de resultado. La categorización, los precios, los colores y las miniaturas son solo algunas de las opciones que puede aplicar a un resultado para producir resultados de búsqueda más convincentes.

El formato Resultados se personaliza según los metadatos específicos de la implementación. Aquí puede encontrar todos los datos por resultado para mostrarlos en los resultados, incluidas las URL de imágenes en miniatura.

Además, es posible configurar varias zonas de resultados dentro de la página, como &quot;Resultados destacados&quot; o secciones de resultados &quot;Productos&quot; y &quot;Contenido&quot; independientes. En estos casos, se proporcionan varias zonas de resultados dentro del HTML, aunque las facetas solo están asociadas con el conjunto de resultados principal.

Ejemplo:

```xml
 <results> 
  <result> 
    <index><![CDATA[1]]></index> 
    <result-title><![CDATA[New Year's Eve Slumber Party]]></result-title> 
    <url><![CDATA[https://mysite.com/parties/new-years-eve-slumber-party-705199/]]></url> 
    <meta-description><![CDATA[Fun New Year's celebration ideas for your kids]]></meta-description> 
    <category><![CDATA[parties]]></category> 
    <content-type><![CDATA[Articles]]></content-type> 
    <small-thumbnail-img><![CDATA[https://mysite.com/assets/cms/parties/new-years-eve-

slumber-party-parties-photo-80-FF1200SLEEPA18.jpg]]></small-thumbnail-img> 
    <large-thumbnail-img><![CDATA[https://mysite.com/assets/cms/parties/new-years-eve- 
slumber-party-parties-photo-160-FF1200SLEEPA18.jpg]]></large-thumbnail-img> 
    <byline><![CDATA[Nancy Mades]]></byline> 
    <blurb><![CDATA[Fun New Year's celebration ideas for your kids]]></blurb> 
  </result>   
  <result> 
    <index><![CDATA[2]]></index> 
    <result-title><![CDATA[10 Holiday Traditions to Start This Year]]></result-title> 
    <url><![CDATA[https://mysite.com/parties/10-holiday-traditions-to-start-this-year-704781/]]></url> 
    <meta-description><![CDATA[Reader ideas to make Thanksgiving, Christmas, and New Year's even more magical]]></meta-description> 
    <category><![CDATA[parties]]></category> 
    <content-type><![CDATA[Articles]]></content-type> 
    <small-thumbnail-img><![CDATA[https://mysite.com/assets/cms/parties/10-holiday- 
traditions-to-start-this-year-parties-photo-80-FF1107HOLIA01.jpg]]></small-thumbnail-img> 
    <large-thumbnail-img><![CDATA[https://mysite.com/assets/cms/parties/10-holiday- 
traditions-to-start-this-year-parties-photo-160-FF1107HOLIA01.jpg]]></large-thumbnail-img> 
    <byline><![CDATA[Julie Taylor]]></byline> 
    <blurb><![CDATA[Reader ideas to make Thanksgiving, Christmas, and New Year's even more magical]]></blurb> 
  </result>   
  <result> 
    <index><![CDATA[3]]></index> 
    <result-title><![CDATA[A Perfect New Year's Eve]]></result-title> 
    <url><![CDATA[https://mysite.com/parties/a-perfect-new-years-eve-705258/]]></url> 
    <meta-description><![CDATA[You can turn New Year's into a celebration for the whole family.]]></meta-description> 
    <category><![CDATA[parties]]></category> 
    <content-type><![CDATA[Articles]]></content-type> 
    <byline><![CDATA[Teri Keough]]></byline> 
    <blurb><![CDATA[You can turn New Year's into a celebration for the whole family.]]></blurb> 
  </result>   
  <result> 
    <index><![CDATA[4]]></index> 
    <result-title><![CDATA[New Year's Fun and Games]]></result-title> 
    <url><![CDATA[https://mysite.com/parties/new-years-fun-and-games-705220/]]></url> 
    <meta-description><![CDATA[Craft, game and food ideas for a New Year's celebration with kids.]]></meta-description> 
    <category><![CDATA[parties]]></category> 
    <content-type><![CDATA[Articles]]></content-type> 
    <byline><![CDATA[Charlotte Meryman]]></byline> 
    <blurb><![CDATA[Craft, game and food ideas for a New Year's celebration with kids.]]></blurb> 
  </result>   
  <result> 
    <index><![CDATA[5]]></index> 
    <result-title><![CDATA[11 Great Ways to Start the New Year]]></result-title> 
    <url><![CDATA[https://mysite.com/parties/11-great-ways-to-start-the-new-year-705552/]]></url> 
    <meta-description><![CDATA[11 New Family Traditions to Start This Year from My Magazine]]></meta-description> 
    <category><![CDATA[parties]]></category> 
    <content-type><![CDATA[Articles]]></content-type> 
    <byline><![CDATA[Emily Block]]></byline> 
    <blurb><![CDATA[11 New Family Traditions to Start This Year from My Magazine]]></blurb> 
  </result>   
  <result> 
    <index><![CDATA[6]]></index> 
    <result-title><![CDATA[Celebrating Chinese New Year]]></result-title> 
    <url><![CDATA[https://mysite.com/parties/celebrating-chinese-new-year-705260/]]></url> 
    <meta-description><![CDATA[Crafts, food, and games to help you celebrate Chinese New Year.]]></meta-description> 
    <category><![CDATA[parties]]></category> 
    <content-type><![CDATA[Articles]]></content-type> 
    <blurb><![CDATA[Crafts, food, and games to help you celebrate Chinese New Year.]]></blurb> 
  </result>   
  <result> 
    <index><![CDATA[7]]></index> 
    <result-title><![CDATA[New Year's Eve, Family Style]]></result-title> 
    <url><![CDATA[https://mysite.com/holidays/new-years-eve-family-style-701283/]]></url> 
    <meta-description><![CDATA[Start a family New Year's Eve tradition by having an evening of kid-focused fun at home]]></meta-description> 
    <category><![CDATA[holidays]]></category> 
    <content-type><![CDATA[Articles]]></content-type> 
    <blurb><![CDATA[Start a family New Year's Eve tradition by having an evening of kid-focused fun at home]]></blurb> 
  </result>   
  <result> 
    <index><![CDATA[8]]></index> 
    <result-title><![CDATA[Chinese New Year Activities]]></result-title> 
    <url><![CDATA[https://mysite.com/crafts/chinese-new-year-activities-710345/]]></url> 
    <meta-description><![CDATA[Activities for celebrating Chinese New Year.]]></meta-description> 
    <category><![CDATA[crafts]]></category> 
    <content-type><![CDATA[Articles]]></content-type> 
    <blurb><![CDATA[Activities for celebrating Chinese New Year.]]></blurb> 
  </result>   
  <result> 
    <index><![CDATA[9]]></index> 
    <result-title><![CDATA[More Organized in the New Year]]></result-title> 
    <url><![CDATA[https://mysite.com/holidays/more-organized-in-the-new-year-701284/]]></url> 
    <meta-description><![CDATA[Tips for getting your household more organized--and getting the kids to help.]]></meta-description> 
    <category><![CDATA[holidays]]></category> 
    <content-type><![CDATA[Articles]]></content-type> 
    <blurb><![CDATA[Tips for getting your household more organized--and getting your kids to help out.]]></blurb> 
  </result>   
  <result> 
    <index><![CDATA[10]]></index> 
    <result-title><![CDATA[Checklists: Year-End Safety Checklist]]></result-title> 
    <url><![CDATA[https://mysite.com/holidays/checklists-year-end-safety-checklist-701352/]]></url> 
    <meta-description><![CDATA[Make sure that your home is safe with our year-end safety checklist!]]></meta-description> 
    <category><![CDATA[holidays]]></category> 
    <content-type><![CDATA[Articles]]></content-type> 
    <blurb><![CDATA[Make sure that your home is safe with our year-end safety checklist!]]></blurb> 
  </result>   
 </results> 
</customer-result> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Etiquetas en los resultados </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;index&gt; </span> </p> </td> 
   <td colname="col2"> <p> Número de serie del resultado dentro de este conjunto de resultados. En este ejemplo, donde se muestran diez resultados por página, en la página 2 de los resultados, el primer elemento tendría un índice de 11. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;result-title&gt; </span> </p> </td> 
   <td colname="col2"> <p> El título de cara al cliente para esta página. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;url&gt; </span> </p> </td> 
   <td colname="col2"> <p> La dirección URL de esta página. Se utiliza para crear un hipervínculo que permite al cliente hacer clic en los resultados. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Buscar formulario {#section_434DA13EA295474C99FFE9F14801CD0E}

Ejemplo:

```xml
<search-form> 
 <include-tnt-mbox>1 </included-tnt-mbox> 
 <autocomplete> 
  <css><![CDATA[<!--link rel="stylesheet" type="te 
        xt/css"href="//content.atomz.com/sp000000a8/publish/autoc 
        omplete_styles.css?sp_css_cache_ver=2" /-->]]> 
  </css> 
  <form-content><![CDATA[<div id="autocomplete"></div>]]> 
  </form-content> 
  <js><![CDATA[<script type="text/javascript" 
   src="//content.atomz.com/sp100491de/publish/autoc 
   omplete_data.js?sp_js_cache_ver=3"></script>]]> 
  </js> 
 </autcomplete> 
 <hidden-parameters> 
  <parameter> 
   <name><![CDATA[store]]></name> 
   <value><![CDATA[mens]]></value> 
  </parameter> 
 </hidden-parameters> 
</search-form>
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Etiquetas en el formulario de búsqueda </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;include-tnt-mbox&gt; </span> </p> </td> 
   <td colname="col2"> <p> Opcional. Cuando está presente en el JSON, un valor de 1 indica que su cuenta está vinculada a <span class="keyword"> Test&amp;Target </span> y que tiene al menos una regla comercial que se encuentra en una prueba A:B. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;autocomplete&gt; </span> </p> </td> 
   <td colname="col2"> <p> Opcional. Al utilizar la función de autocompletar, este nodo está presente para indicar que CSS y JavaScript están presentes en la página junto con el contenido que se encuentra en el formulario. Estos campos normalmente no cambian a menos que alguien haya cambiado una configuración de autocompletado. En estos casos, el campo xxx_cache_ver se incrementa para forzar una invalidación del contenido almacenado en caché en el explorador del cliente. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;css&gt; </span> </p> </td> 
   <td colname="col2"> <p> CSS asociada al autocompletado. Se recomienda colocar esta etiqueta en una posición alta en la página para mejorar el procesamiento de la página. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;form-content&gt; </span> </p> </td> 
   <td colname="col2"> <p> Contenido que se requiere dentro de la búsqueda: desde la utilidad autocompletar hasta el control correcto. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;js&gt; </span> </p> </td> 
   <td colname="col2"> <p> JavaScript personalizado necesario para el autocompletado. Se recomienda colocar esta etiqueta en un nivel inferior en la página para mejorar el procesamiento de la página. El JavaScript de la interfaz de usuario de YUI también es necesario para el autocompletado. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;hidden-parameters&gt; </span> </p> </td> 
   <td colname="col2"> <p> Contiene todos los parámetros ocultos (nombre y valor) que se incluyen en el formulario de búsqueda. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Ordenar {#section_558853CD376F4D71BACF211D53085D55}

En el siguiente ejemplo se muestran los datos de un menú de ordenación de tres opciones. El menú permite al cliente ordenar por relevancia, título o clasificación. El elemento seleccionado actualmente incluye un atributo &quot;selected=true&quot;. &quot;. Ofrecer siempre una opción de relevancia para permitir que un cliente vuelva a los resultados de búsqueda predeterminados que se mostraron originalmente.

Ejemplo:

```xml
 <sort> 
  <sort-item selected="true"> 
   <label><![CDATA[Relevance]]></label> 
   <value><![CDATA[relevance]]></value> 
   <link><![CDATA[]]></link> 
  </sort-item> 
  <sort-item> 
   <label><![CDATA[Title]]></label> 
   <value><![CDATA[title]]></value> 
   <link><![CDATA[?q=new+year;q1=Articles;sort=title;x1=content-type]]></link>     
  </sort-item> 
  <sort-item> 
   <label><![CDATA[Rating]]></label> 
   <value><![CDATA[user-rating]]></value> 
   <link><![CDATA[?q=new+year;q1=Articles;sort=user-rating;x1=content-type]]></link>     
  </sort-item> 
 </sort>
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Etiquetas en el menú Ordenar </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;label&gt; </span> </p> </td> 
   <td colname="col2"> <p> Texto orientado al cliente para la opción . </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;value&gt; </span> </p> </td> 
   <td colname="col2"> <p> Representa el valor del parámetro de cadena de consulta "sort" para esta opción. Esta etiqueta no es necesaria si se utiliza el valor <span class="codeph"> &lt;link&gt; </span> . </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;link&gt; </span> </p> </td> 
   <td colname="col2"> <p> Para las opciones no seleccionadas, el parámetro <span class="codeph"> &lt;link&gt; </span> contiene el vínculo relativo que devuelve el mismo conjunto de resultados, ordenado por el nuevo parámetro de ordenación. Este campo está en blanco para la opción de ordenación seleccionada actualmente. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Sugerencias {#section_6EC104E1DDD94AC799B65E6E61A2FB3C}

Se devuelven sugerencias cuando solo hay unos pocos resultados o no hay ningún resultado. Este nodo contiene términos que no arrojan consultas correctas y que pueden mostrarse en una página &quot;Sin resultados&quot;. El vínculo también se devuelve para que un cliente pueda ir a la nueva consulta.

Ejemplo:

```xml
 <suggestions> 
  <suggestion-item> 
   <link><![CDATA[?q=video]]></link> 
   <word><![CDATA[video]]> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Etiquetas en sugerencias </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;link&gt; </span> </p> </td> 
   <td colname="col2"> <p>Vínculo relativo que se utiliza para crear un hipervínculo y buscar resultados para el término de sugerencia. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;word&gt; </span> </p> </td> 
   <td colname="col2"> <p>El término sugerido. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Zonas {#section_AE53A498B440465EAF2286F2AE87D548}

Ejemplo:

```xml
<zones> 
 <zone> 
  <name><![CDATA[best-sellers]]></name> 
  <display><![CDATA[1]]></display> 
 </zone> 
</zones> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Etiquetas en zonas </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;zone&gt; </span> </p> </td> 
   <td colname="col2"> <p> Un nodo de zona individual. Puede tener varios nodos de zona. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;name&gt; </span> </p> </td> 
   <td colname="col2"> <p> Nombre de la zona. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;display&gt; </span> </p> </td> 
   <td colname="col2"> <p> 1 o 0 para indicar si la zona se muestra o no. El contenido real de la zona puede ser un área estática de la página web o de los resultados de búsqueda, como los mejores vendedores o productos relacionados. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Salida XML de búsqueda guiada {#reference_D93E859A277643068B10AE7A61C973EA}

Tablas que describen la salida de respuesta XML estándar.

Puede revisar la respuesta XML para lo siguiente:

* [Banners](../c-appendices/c-guidedsearchoutput.md#section_6A19EC26DD3B494194AAA788151B78B5)
* [Ruta de navegación](../c-appendices/c-guidedsearchoutput.md#section_E48A71B0EBDB4EDDA7587009AD865488)
* [Facetas](../c-appendices/c-guidedsearchoutput.md#section_5CEB1F966C004FFEA3CF675638966E25)
* [Encabezado y consulta](../c-appendices/c-guidedsearchoutput.md#section_802835E19BCB48239C6770A1B72DFFF8)
* [Paginación](../c-appendices/c-guidedsearchoutput.md#section_72DB86DDE1284B1EA295CFFBC16A3150)
* [Búsquedas recientes](../c-appendices/c-guidedsearchoutput.md#section_BCA2DDD17F264CF6BA11634E1A514E28)
* [Resultados](../c-appendices/c-guidedsearchoutput.md#section_EC496F5CA2634158891455E2F6DF6833)
* [Buscar formulario](../c-appendices/c-guidedsearchoutput.md#section_F92D8C3D37174A10A4E26CAFF3F3DF89)
* [Ordenar](../c-appendices/c-guidedsearchoutput.md#section_32DC50A103BF491BA3665A5CADCCAADE)
* [Sugerencias](../c-appendices/c-guidedsearchoutput.md#section_D81BCE46F0AF443695DF9C4BA084B716)
* [Zonas](../c-appendices/c-guidedsearchoutput.md#section_15D8AA585F3246799968BA88EE2C9FC2)

## Banners {#section_6A19EC26DD3B494194AAA788151B78B5}

Ejemplo:

```xml
<banners> 
 <banner> 
  <area><![CDATA[top-left]]></area> 
  <content><![CDATA[<img src="https://www.megacorp.com/discount.gif"/>]]></content> 
 </banner> 
</banners>
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Etiquetas en titulares </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;banner&gt; </span> </p> </td> 
   <td colname="col2"> <p> Un nodo de banner individual. Puede tener varios nodos de banner. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;area&gt; </span> </p> </td> 
   <td colname="col2"> <p> El área de la página web donde se muestra el banner. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;content&gt; </span> </p> </td> 
   <td colname="col2"> <p> El contenido HTML del área del banner. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Ruta de navegación {#section_E48A71B0EBDB4EDDA7587009AD865488}

En el siguiente ejemplo, cada vez que el cliente se reduce más a través de las facetas, la selección se agrega a la ruta de exploración. Cada elemento se representa como `<breadcrumb-item>`.

Ejemplo:

```xml
 <breadcrumb> 
  <breadcrumb-item> 
   <link><![CDATA[?q=new+year]]></link> 
   <value><![CDATA[new year]]></value> 
  </breadcrumb-item> 
  <breadcrumb-item> 
   <link><![CDATA[?q=new+year;q1=Articles;x1=content-type]]></link> 
   <value><![CDATA[Articles]]></value> 
  </breadcrumb-item> 
 </breadcrumb> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Etiquetas en la ruta de navegación </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;link&gt; </span> </p> </td> 
   <td colname="col2"> <p> Vínculo relativo a los resultados de búsqueda que muestra la vista deseada. Al hacer clic en un vínculo de ruta de exploración, el cliente llega a una vista en la que se eliminan todas las mejoras posteriores. También hay otras opciones disponibles. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;value&gt; </span> </p> </td> 
   <td colname="col2"> <p> Texto orientado al cliente para el elemento de ruta de exploración. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Facetas {#section_5CEB1F966C004FFEA3CF675638966E25}

Las facetas son opciones de refinamiento que permiten a los clientes filtrar los resultados. Las facetas suelen utilizarse para la categorización, intervalos de precios, selecciones de color y otro refinamiento de atributos. Los metadatos del índice son lo que impulsa las facetas.

Es habitual ocultar o mostrar facetas de categorización a medida que un cliente baja por la categorización. El nivel más alto de categorización (categoría) se conoce como Nivel 1. Cuando un cliente hace clic en una opción de Nivel 1, aparecen las opciones de refinamiento de Nivel 2 (subcategoría) y desaparecen las opciones de Nivel 1. Cuando un cliente hace clic en una opción de Nivel 2, aparecen las opciones de refinamiento de Nivel 3 (subcategoría) y desaparecen las opciones de Nivel 2. Como se ha indicado anteriormente, estas opciones están ocultas y se muestran: su aplicación web no se ve afectada por ellas.

Cada faceta está contenida dentro de etiquetas `<facet-item>` . En el siguiente ejemplo, muestra una faceta que permite al cliente refinar los resultados de búsqueda por &quot;vacaciones&quot;.

Ejemplo:

```xml
 <facets> 
  <facet-item> 
   <facet-title><![CDATA[Holidays]]></facet-title> 
   <facet-value> 
    <label><![CDATA[New Year]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=New+Year;x1=content-type;x2=holidays]]></link> 
    <count><![CDATA[11]]></count> 
   </facet-value> 
   <facet-value> 
    <label><![CDATA[Christmas]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=Christmas;x1=content-type;x2=holidays]]></link> 
    <count><![CDATA[7]]></count> 
   </facet-value> 
   <facet-value> 
    <label><![CDATA[Chinese New Year]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=Chinese+New+Year;x1=content-type;x2=holidays]]></link> 
    <count><![CDATA[2]]></count> 
   </facet-value> 
   <facet-value> 
    <label><![CDATA[Thanksgiving]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=Thanksgiving;x1=content-type;x2=holidays]]></link> 
    <count><![CDATA[2]]></count> 
   </facet-value> 
   <facet-value> 
    <label><![CDATA[4th of July]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=4th+of+July;x1=content-type;x2=holidays]]></link> 
    <count><![CDATA[1]]></count> 
   </facet-value> 
   <facet-value> 
    <label><![CDATA[Father&#39;s Day]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=Father's+Day;x1=content-type;x2=holidays]]></link> 
    <count><![CDATA[1]]></count> 
   </facet-value> 
   <facet-value> 
    <label><![CDATA[Hanukkah]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=Hanukkah;x1=content-type;x2=holidays]]></link> 
    <count><![CDATA[1]]></count> 
   </facet-value> 
   <facet-value> 
    <label><![CDATA[Mother&#39;s Day]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=Mother's+Day;x1=content-type;x2=holidays]]></link> 
    <count><![CDATA[1]]></count> 
   </facet-value> 
   <facet-value> 
    <label><![CDATA[Valentine&#39;s Day]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=Valentine's+Day;x1=content-type;x2=holidays]]></link> 
    <count><![CDATA[1]]></count> 
   </facet-value> 
  </facet-item> 
  <facet-item> 
   <facet-title><![CDATA[Seasons]]></facet-title> 
   <facet-value> 
    <label><![CDATA[Winter]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=Winter;x1=content-type;x2=seasons]]></link> 
    <count><![CDATA[20]]></count> 
   </facet-value> 
   <facet-value> 
    <label><![CDATA[Summer]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=Summer;x1=content-type;x2=seasons]]></link> 
    <count><![CDATA[7]]></count> 
   </facet-value> 
   <facet-value> 
    <label><![CDATA[Autumn]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=Autumn;x1=content-type;x2=seasons]]></link> 
    <count><![CDATA[4]]></count> 
   </facet-value> 
   <facet-value> 
    <label><![CDATA[Spring]]></label> 
    <link><![CDATA[?q=new+year;q1=Articles;q2=Spring;x1=content-type;x2=seasons]]></link> 
    <count><![CDATA[2]]></count> 
   </facet-value> 
  </facet-item>  
 </facets> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Etiquetas en facetas </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;facet-title&gt; </span> </p> </td> 
   <td colname="col2"> <p> Título orientado al cliente para la faceta. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;label&gt; </span> </p> </td> 
   <td colname="col2"> <p> Etiqueta de cara al cliente para la opción faceta. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;link&gt; </span> </p> </td> 
   <td colname="col2"> <p> Vínculo relativo a los resultados que la opción refina. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;count&gt; </span> </p> </td> 
   <td colname="col2"> <p> Número de resultados en ese conjunto de resultados refinado. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;undolink&gt; </span> </p> </td> 
   <td colname="col2"> <p> Cuando se selecciona un valor de faceta, el nodo devuelve un "vínculo de deshacer" que permite que un cliente vuelva a estar fuera de los resultados. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Encabezado y consulta {#section_802835E19BCB48239C6770A1B72DFFF8}

Ejemplo:

```xml
<?xml version="1.0" encoding="utf-8" standalone="yes" ?> 
<result> 
 <query> 
  <user-query><![CDATA[new year]]></user-query> 
  <lower-results><![CDATA[1]]></lower-results> 
  <upper-results><![CDATA[16]]></upper-results> 
  <total-results><![CDATA[621]]></total-results> 
 </query> 
```

En conjunto, estas etiquetas presentan un mensaje como el siguiente: &quot;Mostrando los resultados 1-16 de 621 para &#39;año nuevo&#39;&quot;.

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Etiquetas en encabezado y consulta </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;user-query&gt; </span> </p> </td> 
   <td colname="col2"> <p> La consulta de palabra clave que se envía con la solicitud. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;lower-results&gt; </span> </p> </td> 
   <td colname="col2"> <p> El número de artículo del primer resultado de esta página. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;upper-results&gt; </span> </p> </td> 
   <td colname="col2"> <p> El número de artículo del último resultado de esta página. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;total-results&gt; </span> </p> </td> 
   <td colname="col2"> <p> Número total de resultados que coinciden con la consulta del usuario. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;custom-field&gt; </span> </p> </td> 
   <td colname="col2"> <p> Campo opcional que se aplica globalmente a los resultados de búsqueda. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Paginación {#section_72DB86DDE1284B1EA295CFFBC16A3150}

Ejemplo:

```xml
<pagination> 
 <total-pages><39></total-pages> 
 <pages> 
   <page position="first"></page> 
   <page position="last">?i=1;page=39;q=new+year;q1=Articles;x1=content-type]]></page> 
   <page position="previous"></page> 
   <page position="next">?i=1;page=2;q=new+year;q1=Articles;x1=content-type]]></page> 
   <page position="1" selected="true">?i=1;q=new+year;q1=Articles;x1=content-type]]></page> 
   <page position="2">?i=1;page=2;q=new+year;q1=Articles;x1=content-type]]></page> 
   <page position="3">?i=1;page=3;q=new+year;q1=Articles;x1=content-type]]></page> 
   <page position="4">?i=1;page=4;q=new+year;q1=Articles;x1=content-type]]></page> 
   <page position="5">?i=1;page=5;q=new+year;q1=Articles;x1=content-type]]></page> 
   <page position="6">?i=1;page=6;q=new+year;q1=Articles;x1=content-type]]></page> 
   <page position="7">?i=1;page=7;q=new+year;q1=Articles;x1=content-type]]></page> 
   <page position="8">?i=1;page=8;q=new+year;q1=Articles;x1=content-type]]></page> 
   <page position="9">?i=1;page=9;q=new+year;q1=Articles;x1=content-type]]></page> 
   <page position="10">?i=1;page=10;q=new+year;q1=Articles;x1=content-type]]></page> 
 </pages> 
</pagination> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Etiquetas en la paginación </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;total-pages&gt; </span> </p> </td> 
   <td colname="col2"> <p> Número total de páginas de resultados, según el número de resultados dividido por el número de resultados por página. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;page position="first"&gt; </span> </p> </td> 
   <td colname="col2"> <p> Contiene un vínculo relativo a la primera página del conjunto de resultados, a menos que el cliente ya esté viendo la página 1. En tal caso, está en blanco. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;page position="last"&gt; </span> </p> </td> 
   <td colname="col2"> <p> Contiene un vínculo relativo a la última página del conjunto de resultados, a menos que el cliente esté viendo la última página. En tal caso, está en blanco. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;page position="previous"&gt; </span> </p> </td> 
   <td colname="col2"> <p> Contiene un vínculo relativo a la página anterior del conjunto de resultados, a menos que el cliente esté viendo la página 1; en tal caso, está en blanco. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;page position="next"&gt; </span> </p> </td> 
   <td colname="col2"> <p> Contiene un vínculo relativo a la última página del conjunto de resultados, a menos que el cliente esté viendo la última página. En tal caso, está en blanco. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;page position="x"&gt;</span> </p> </td> 
   <td colname="col2"> <p> Contiene un vínculo relativo a un número de página en particular. Se muestran diez números de página contiguos. En la página 1, serían las páginas 1-10. Al final del conjunto de resultados (en este caso, 39), serían las páginas 30-39. Por ejemplo, en el centro del conjunto de resultados, la página 15, serían las páginas 11-20. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> selected="true"&gt;  </span> </p> </td> 
   <td colname="col2"> <p> Se aplica como atributo a la página seleccionada actualmente. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Búsquedas recientes {#section_BCA2DDD17F264CF6BA11634E1A514E28}

Búsquedas recientes es una función basada en cookies que solo funciona si reenvía la información de las cookies a los servidores.

Ejemplo:

```xml
<recent-searches> 
 <recent-search> 
  <search-term><![CDATA[shoes]]></search-term> 
  <link><![CDATA[?q=shoes]]></link> 
 </recent-search> 
</recent-searches> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Etiquetas en búsquedas recientes </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;recent-search&gt; </span> </p> </td> 
   <td colname="col2"> <p> Un nodo de búsqueda reciente individual. Puede tener varios nodos de búsqueda reciente. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;search-term&gt; </span> </p> </td> 
   <td colname="col2"> <p> Término que el cliente buscó anteriormente. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;link&gt; </span> </p> </td> 
   <td colname="col2"> <p> Vínculos a la búsqueda anterior. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Resultados {#section_EC496F5CA2634158891455E2F6DF6833}

El conjunto Results es un área personalizable de la respuesta XML. Cada índice es único en los mecanismos de asignación de nombres de campo de los metadatos. Hay campos comunes que se devuelven para cada resultado, como título, descripción y dirección URL. Sin embargo, los metadatos definidos para una página del índice pueden estar disponibles para su uso en cada nodo de resultado. La categorización, los precios, los colores y las miniaturas son solo algunas de las opciones que puede aplicar a un resultado para producir resultados de búsqueda más convincentes.

El formato Resultados se personaliza según los metadatos específicos de la implementación. Aquí puede encontrar todos los datos por resultado para mostrarlos en los resultados, incluidas las URL de imágenes en miniatura.

Además, es posible configurar varias zonas de resultados dentro de la página, como &quot;Resultados destacados&quot; o secciones de resultados &quot;Productos&quot; y &quot;Contenido&quot; independientes. En estos casos, se proporcionan varias zonas de resultados dentro del HTML, aunque las facetas solo están asociadas con el conjunto de resultados principal.

Ejemplo:

```xml
 <results> 
  <result> 
    <index><![CDATA[1]]></index> 
    <result-title><![CDATA[New Year's Eve Slumber Party]]></result-title> 
    <url><![CDATA[https://mysite.com/parties/new-years-eve-slumber-party-705199/]]></url> 
    <meta-description><![CDATA[Fun New Year's celebration ideas for your kids]]></meta-description> 
    <category><![CDATA[parties]]></category> 
    <content-type><![CDATA[Articles]]></content-type> 
    <small-thumbnail-img><![CDATA[https://mysite.com/assets/cms/parties/new-years-eve-

slumber-party-parties-photo-80-FF1200SLEEPA18.jpg]]></small-thumbnail-img> 
    <large-thumbnail-img><![CDATA[https://mysite.com/assets/cms/parties/new-years-eve- 
slumber-party-parties-photo-160-FF1200SLEEPA18.jpg]]></large-thumbnail-img> 
    <byline><![CDATA[Nancy Mades]]></byline> 
    <blurb><![CDATA[Fun New Year's celebration ideas for your kids]]></blurb> 
  </result>   
  <result> 
    <index><![CDATA[2]]></index> 
    <result-title><![CDATA[10 Holiday Traditions to Start This Year]]></result-title> 
    <url><![CDATA[https://mysite.com/parties/10-holiday-traditions-to-start-this-year-704781/]]></url> 
    <meta-description><![CDATA[Reader ideas to make Thanksgiving, Christmas, and New Year's even more magical]]></meta-description> 
    <category><![CDATA[parties]]></category> 
    <content-type><![CDATA[Articles]]></content-type> 
    <small-thumbnail-img><![CDATA[https://mysite.com/assets/cms/parties/10-holiday- 
traditions-to-start-this-year-parties-photo-80-FF1107HOLIA01.jpg]]></small-thumbnail-img> 
    <large-thumbnail-img><![CDATA[https://mysite.com/assets/cms/parties/10-holiday- 
traditions-to-start-this-year-parties-photo-160-FF1107HOLIA01.jpg]]></large-thumbnail-img> 
    <byline><![CDATA[Julie Taylor]]></byline> 
    <blurb><![CDATA[Reader ideas to make Thanksgiving, Christmas, and New Year's even more magical]]></blurb> 
  </result>   
  <result> 
    <index><![CDATA[3]]></index> 
    <result-title><![CDATA[A Perfect New Year's Eve]]></result-title> 
    <url><![CDATA[https://mysite.com/parties/a-perfect-new-years-eve-705258/]]></url> 
    <meta-description><![CDATA[You can turn New Year's into a celebration for the whole family.]]></meta-description> 
    <category><![CDATA[parties]]></category> 
    <content-type><![CDATA[Articles]]></content-type> 
    <byline><![CDATA[Teri Keough]]></byline> 
    <blurb><![CDATA[You can turn New Year's into a celebration for the whole family.]]></blurb> 
  </result>   
  <result> 
    <index><![CDATA[4]]></index> 
    <result-title><![CDATA[New Year's Fun and Games]]></result-title> 
    <url><![CDATA[https://mysite.com/parties/new-years-fun-and-games-705220/]]></url> 
    <meta-description><![CDATA[Craft, game and food ideas for a New Year's celebration with kids.]]></meta-description> 
    <category><![CDATA[parties]]></category> 
    <content-type><![CDATA[Articles]]></content-type> 
    <byline><![CDATA[Charlotte Meryman]]></byline> 
    <blurb><![CDATA[Craft, game and food ideas for a New Year's celebration with kids.]]></blurb> 
  </result>   
  <result> 
    <index><![CDATA[5]]></index> 
    <result-title><![CDATA[11 Great Ways to Start the New Year]]></result-title> 
    <url><![CDATA[https://mysite.com/parties/11-great-ways-to-start-the-new-year-705552/]]></url> 
    <meta-description><![CDATA[11 New Family Traditions to Start This Year from My Magazine]]></meta-description> 
    <category><![CDATA[parties]]></category> 
    <content-type><![CDATA[Articles]]></content-type> 
    <byline><![CDATA[Emily Block]]></byline> 
    <blurb><![CDATA[11 New Family Traditions to Start This Year from My Magazine]]></blurb> 
  </result>   
  <result> 
    <index><![CDATA[6]]></index> 
    <result-title><![CDATA[Celebrating Chinese New Year]]></result-title> 
    <url><![CDATA[https://mysite.com/parties/celebrating-chinese-new-year-705260/]]></url> 
    <meta-description><![CDATA[Crafts, food, and games to help you celebrate Chinese New Year.]]></meta-description> 
    <category><![CDATA[parties]]></category> 
    <content-type><![CDATA[Articles]]></content-type> 
    <blurb><![CDATA[Crafts, food, and games to help you celebrate Chinese New Year.]]></blurb> 
  </result>   
  <result> 
    <index><![CDATA[7]]></index> 
    <result-title><![CDATA[New Year's Eve, Family Style]]></result-title> 
    <url><![CDATA[https://mysite.com/holidays/new-years-eve-family-style-701283/]]></url> 
    <meta-description><![CDATA[Start a family New Year's Eve tradition by having an evening of kid-focused fun at home]]></meta-description> 
    <category><![CDATA[holidays]]></category> 
    <content-type><![CDATA[Articles]]></content-type> 
    <blurb><![CDATA[Start a family New Year's Eve tradition by having an evening of kid-focused fun at home]]></blurb> 
  </result>   
  <result> 
    <index><![CDATA[8]]></index> 
    <result-title><![CDATA[Chinese New Year Activities]]></result-title> 
    <url><![CDATA[https://mysite.com/crafts/chinese-new-year-activities-710345/]]></url> 
    <meta-description><![CDATA[Activities for celebrating Chinese New Year.]]></meta-description> 
    <category><![CDATA[crafts]]></category> 
    <content-type><![CDATA[Articles]]></content-type> 
    <blurb><![CDATA[Activities for celebrating Chinese New Year.]]></blurb> 
  </result>   
  <result> 
    <index><![CDATA[9]]></index> 
    <result-title><![CDATA[More Organized in the New Year]]></result-title> 
    <url><![CDATA[https://mysite.com/holidays/more-organized-in-the-new-year-701284/]]></url> 
    <meta-description><![CDATA[Tips for getting your household more organized--and getting the kids to help.]]></meta-description> 
    <category><![CDATA[holidays]]></category> 
    <content-type><![CDATA[Articles]]></content-type> 
    <blurb><![CDATA[Tips for getting your household more organized--and getting your kids to help out.]]></blurb> 
  </result>   
  <result> 
    <index><![CDATA[10]]></index> 
    <result-title><![CDATA[Checklists: Year-End Safety Checklist]]></result-title> 
    <url><![CDATA[https://mysite.com/holidays/checklists-year-end-safety-checklist-701352/]]></url> 
    <meta-description><![CDATA[Make sure that your home is safe with our year-end safety checklist!]]></meta-description> 
    <category><![CDATA[holidays]]></category> 
    <content-type><![CDATA[Articles]]></content-type> 
    <blurb><![CDATA[Make sure that your home is safe with our year-end safety checklist!]]></blurb> 
  </result>   
 </results> 
</customer-result> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Etiquetas en los resultados </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;index&gt; </span> </p> </td> 
   <td colname="col2"> <p> Número de serie del resultado dentro de este conjunto de resultados. En este ejemplo, donde se muestran diez resultados por página, en la página 2 de los resultados, el primer elemento tendría un índice de 11. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;result-title&gt; </span> </p> </td> 
   <td colname="col2"> <p> El título de cara al cliente para esta página. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;url&gt; </span> </p> </td> 
   <td colname="col2"> <p> La dirección URL de esta página. Se utiliza para crear un hipervínculo que permite al cliente hacer clic en los resultados. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Buscar formulario {#section_F92D8C3D37174A10A4E26CAFF3F3DF89}

Ejemplo:

```xml
<search-form> 
 <include-tnt-mbox>1 </included-tnt-mbox> 
 <autocomplete> 
  <css><![CDATA[<!--link rel="stylesheet" type="te 
        xt/css"href="//content.atomz.com/sp000000a8/publish/autoc 
        omplete_styles.css?sp_css_cache_ver=2" /-->]]> 
  </css> 
  <form-content><![CDATA[<div id="autocomplete"></div>]]> 
  </form-content> 
  <js><![CDATA[<script type="text/javascript" 
   src="//content.atomz.com/sp100491de/publish/autoc 
   omplete_data.js?sp_js_cache_ver=3"></script>]]> 
  </js> 
 </autcomplete> 
 <hidden-parameters> 
  <parameter> 
   <name><![CDATA[store]]></name> 
   <value><![CDATA[mens]]></value> 
  </parameter> 
 </hidden-parameters> 
</search-form>
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Etiquetas en el formulario de búsqueda </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;include-tnt-mbox&gt; </span> </p> </td> 
   <td colname="col2"> <p> Opcional. Cuando está presente en el XML, un valor de 1 indica que su cuenta está vinculada a <span class="keyword"> Test&amp;Target </span> y que tiene al menos una regla comercial que se encuentra en una prueba A:B. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;autocomplete&gt; </span> </p> </td> 
   <td colname="col2"> <p> Opcional. Al utilizar la función de autocompletar, este nodo está presente para indicar que CSS y JavaScript están presentes en la página junto con el contenido que se encuentra en el formulario. Estos campos normalmente no cambian a menos que alguien haya cambiado una configuración de autocompletado. En estos casos, el campo xxx_cache_ver se incrementa para forzar una invalidación del contenido almacenado en caché en el explorador del cliente. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;css&gt; </span> </p> </td> 
   <td colname="col2"> <p> CSS asociada al autocompletado. Se recomienda colocar esta etiqueta en una posición alta en la página para mejorar el procesamiento de la página. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;form-content&gt; </span> </p> </td> 
   <td colname="col2"> <p> Contenido que se requiere dentro de la búsqueda: desde la utilidad autocompletar hasta el control correcto. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;js&gt; </span> </p> </td> 
   <td colname="col2"> <p> JavaScript personalizado necesario para el autocompletado. Se recomienda colocar esta etiqueta en un nivel inferior en la página para mejorar el procesamiento de la página. El JavaScript de la interfaz de usuario de YUI también es necesario para el autocompletado. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;hidden-parameters&gt; </span> </p> </td> 
   <td colname="col2"> <p> Contiene todos los parámetros ocultos (nombre y valor) que se incluyen en el formulario de búsqueda. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Ordenar {#section_32DC50A103BF491BA3665A5CADCCAADE}

En el siguiente ejemplo se muestran los datos de un menú de ordenación de tres opciones. El menú permite al cliente ordenar por relevancia, título o clasificación. El elemento seleccionado actualmente incluye un atributo &quot;selected=true&quot;. &quot;. Ofrecer siempre una opción de relevancia para permitir que un cliente vuelva a los resultados de búsqueda predeterminados que se mostraron originalmente.

Ejemplo:

```xml
 <sort> 
  <sort-item selected="true"> 
   <label><![CDATA[Relevance]]></label> 
   <value><![CDATA[relevance]]></value> 
   <link><![CDATA[]]></link> 
  </sort-item> 
  <sort-item> 
   <label><![CDATA[Title]]></label> 
   <value><![CDATA[title]]></value> 
   <link><![CDATA[?q=new+year;q1=Articles;sort=title;x1=content-type]]></link>     
  </sort-item> 
  <sort-item> 
   <label><![CDATA[Rating]]></label> 
   <value><![CDATA[user-rating]]></value> 
   <link><![CDATA[?q=new+year;q1=Articles;sort=user-rating;x1=content-type]]></link>     
  </sort-item> 
 </sort>
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Etiquetas en el menú Ordenar </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;label&gt; </span> </p> </td> 
   <td colname="col2"> <p> Texto orientado al cliente para la opción . </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;value&gt; </span> </p> </td> 
   <td colname="col2"> <p> Representa el valor del parámetro de cadena de consulta "sort" para esta opción. Esta etiqueta no es necesaria si se utiliza el valor <span class="codeph"> &lt;link&gt; </span> . </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;link&gt; </span> </p> </td> 
   <td colname="col2"> <p> Para las opciones no seleccionadas, el parámetro <span class="codeph"> &lt;link&gt; </span> contiene el vínculo relativo que devuelve el mismo conjunto de resultados, ordenado por el nuevo parámetro de ordenación. Este campo está en blanco para la opción de ordenación seleccionada actualmente. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Sugerencias {#section_D81BCE46F0AF443695DF9C4BA084B716}

Se devuelven sugerencias cuando solo hay unos pocos resultados o no hay ningún resultado. Este nodo contiene términos que no arrojan consultas correctas y que pueden mostrarse en una página &quot;Sin resultados&quot;. El vínculo también se devuelve para que un cliente pueda ir a la nueva consulta.

Ejemplo:

```xml
 <suggestions> 
  <suggestion-item> 
   <link><![CDATA[?q=video]]></link> 
   <word><![CDATA[video]]> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Etiquetas en sugerencias </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;link&gt; </span> </p> </td> 
   <td colname="col2"> <p>Vínculo relativo que se utiliza para crear un hipervínculo y buscar resultados para el término de sugerencia. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;word&gt; </span> </p> </td> 
   <td colname="col2"> <p>El término sugerido. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Zonas {#section_15D8AA585F3246799968BA88EE2C9FC2}

Ejemplo:

```xml
<zones> 
 <zone> 
  <name><![CDATA[best-sellers]]></name> 
  <display><![CDATA[1]]></display> 
 </zone> 
</zones> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Etiquetas en zonas </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;zone&gt; </span> </p> </td> 
   <td colname="col2"> <p> Un nodo de zona individual. Puede tener varios nodos de zona. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;name&gt; </span> </p> </td> 
   <td colname="col2"> <p> Nombre de la zona. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> <span class="codeph"> &lt;display&gt; </span> </p> </td> 
   <td colname="col2"> <p> 1 o 0 para indicar si la zona se muestra o no. El contenido real de la zona puede ser un área estática de la página web o de los resultados de búsqueda, como los mejores vendedores o productos relacionados. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Salida XML de búsqueda guiada para Adobe Experience Manager {#reference_DBE13C606C3A4BB185DE53F88D0D3048}

Tablas que describen la salida de respuesta XML estándar para AEM (Adobe Experience Manager).

Consulte también . [Salida XML de búsqueda guiada](../c-appendices/c-guidedsearchoutput.md#reference_D93E859A277643068B10AE7A61C973EA)

Puede revisar la respuesta XML para lo siguiente:

* [Banners](../c-appendices/c-guidedsearchoutput.md#section_B16EDC5533FA4494AC9983AA7357CBE3)
* [Rutas de navegación](../c-appendices/c-guidedsearchoutput.md#section_49EA7043FBE44315A79A4E738BE30114)
* [Campos personalizados](../c-appendices/c-guidedsearchoutput.md#section_38DD31AFE5DD4263A63644AFF484E0F4)
* [Facetas](../c-appendices/c-guidedsearchoutput.md#section_BE98990E3DD748A1BD4E0CA322314B79)
* [Encabezado](../c-appendices/c-guidedsearchoutput.md#section_5305B1DC5774439485CA0665DB683F9C)
* [Menús y ordenación](../c-appendices/c-guidedsearchoutput.md#section_A34CBB645DBF4C70A12A5B7E81211295)
* [Paginación](../c-appendices/c-guidedsearchoutput.md#section_E52F81C6A6EB4B8F996157B657EC540F)
* [Consulta](../c-appendices/c-guidedsearchoutput.md#section_3DAA1013F09742869B80F6A361816E6C)
* [Búsquedas recientes](../c-appendices/c-guidedsearchoutput.md#section_17F942F6EC07456DABED7A483AC08446)
* [Resultados](../c-appendices/c-guidedsearchoutput.md#section_155A80B8C4F641678DD9C8F257108412)
* [Buscar formulario](../c-appendices/c-guidedsearchoutput.md#section_9E4B99D4FEDC49629F6C7E866F3A7493)
* [Sugerencias](../c-appendices/c-guidedsearchoutput.md#section_2899FACB9AD84F60B3687C1B4EF09E15)
* [Plantilla](../c-appendices/c-guidedsearchoutput.md#section_1E2BB2F274B04F5491A4CCCC38F507BD)
* [Zonas](../c-appendices/c-guidedsearchoutput.md#section_26C4A947E7B1474A8E37D86D9579B93E)

## Banners {#section_B16EDC5533FA4494AC9983AA7357CBE3}

La búsqueda/comercialización del sitio puede administrar los banners de un cliente, conectándolos en varias partes de una página web.

Ejemplo de titular:

El siguiente es un ejemplo de un banner que se coloca en el área de las páginas llamada &quot;top&quot;.

```xml
   <banners> 
       <banner> 
           <area><![CDATA[top]]></area> 
           <content><![CDATA[<div style="color:#70A100">We have custom shipping</div>]]></content> 
       </banner> 
    </banners> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Nodo </p> </th> 
   <th colname="col2" class="entry"> <p>Nodo principal </p> </th> 
   <th colname="col3" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p>banners </p> </td> 
   <td colname="col2"> <p>resultados del cliente </p> </td> 
   <td colname="col3"> <p>Contiene nodos de banner 0-n que indican cada área de banner y el contenido que está conectado a esa área. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>banner </p> </td> 
   <td colname="col2"> <p>banners </p> </td> 
   <td colname="col3"> <p>Un nodo de banner individual. Puede tener varios nodos de banner. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>area </p> </td> 
   <td colname="col2"> <p>banner </p> </td> 
   <td colname="col3"> <p>El área de la página web donde se muestra el banner. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>content </p> </td> 
   <td colname="col2"> <p>banner </p> </td> 
   <td colname="col3"> <p>El contenido del banner. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Rutas de navegación {#section_49EA7043FBE44315A79A4E738BE30114}

Se admiten varias rutas de exploración. Puede definir las rutas de exploración y su comportamiento correspondiente en **[!UICONTROL Design]** > **[!UICONTROL Navigation]** > **[!UICONTROL Breadcrumbs]**. Además, debe asignar un nombre único a cada ruta de exploración que defina. El nodo XML de rutas de exploración se repite en todas las rutas de exploración definidas. Se recomienda que solo muestre una ruta en los resultados de búsqueda.

En el siguiente ejemplo, cada vez que el cliente se reduce más a través de las facetas, la selección se agrega a la ruta de exploración. Cada elemento se representa como `<breadcrumb-item>`.

Ejemplo de nodo de ruta de exploración:

```xml
    <breadcrumbs> 
  <breadcrumb> 
            <name><![CDATA[default]]></name> 
     <breadcrumb-item> 
   <link><![CDATA[?i=1;q=mens;sp_cs=UTF-8;view=xml]]></link> 
   <value><![CDATA[mens]]></value> 
                <label><![CDATA[]]></label> 
      </breadcrumb-item> 
     <breadcrumb-item> 
   <link><![CDATA[?i=1;q=mens;q1=Channel;sp_cs=UTF-8;view=xml;x1=brand]]></link> 
   <value><![CDATA[Channel]]></value> 
                <label><![CDATA[brand]]></label> 
      </breadcrumb-item> 
   </breadcrumb> 
    </breadcrumbs> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Nodo </p> </th> 
   <th colname="col2" class="entry"> <p>Nodo principal </p> </th> 
   <th colname="col3" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p>rutas de exploración </p> </td> 
   <td colname="col2"> <p>resultados del cliente </p> </td> 
   <td colname="col3"> <p> Contiene nodos de ruta de exploración 0-n que definen cada ruta de exploración. La mayoría de los clientes solo tienen una ruta de exploración. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>ruta </p> </td> 
   <td colname="col2"> <p>rutas de exploración </p> </td> 
   <td colname="col3"> <p> Contiene los nodos secundarios que definen la definición de una ruta de exploración. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>name </p> </td> 
   <td colname="col2"> <p>ruta </p> </td> 
   <td colname="col3"> <p> Nombre de la ruta de exploración. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>elemento de ruta de exploración </p> </td> 
   <td colname="col2"> <p>Elemento individual dentro de la ruta de exploración. Cada elemento denota un paso en la pista a medida que el usuario reduce el conjunto de resultados. </p> </td> 
   <td colname="col3"> <p> </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>link </p> </td> 
   <td colname="col2"> <p>elemento de ruta de exploración </p> </td> 
   <td colname="col3"> <p> Vínculo relativo a los resultados de búsqueda que muestra la vista deseada. Al hacer clic en un vínculo de ruta de exploración, el cliente llega a una vista en la que se eliminan todas las mejoras posteriores. También hay otras opciones disponibles, como soltar y quitar. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>value </p> </td> 
   <td colname="col2"> <p>elemento de ruta de exploración </p> </td> 
   <td colname="col3"> <p> Texto orientado al cliente para el elemento de ruta de exploración. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>label </p> </td> 
   <td colname="col2"> <p>elemento de ruta de exploración </p> </td> 
   <td colname="col3"> <p> La etiqueta genera una etiqueta para un valor de ruta de exploración que detalla qué faceta se seleccionó para generar ese elemento de ruta de exploración. Solo se utiliza en el contexto de un bloque de ruta guiada. Para el paso del término de consulta, esto está en blanco. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Campos personalizados {#section_38DD31AFE5DD4263A63644AFF484E0F4}

Los campos personalizados son una colección diversa de variables con un contexto global. Normalmente se utiliza para pasar variables con fines de SEO que se establecen en los metadatos de la página de resultados de búsqueda.

Ejemplo de nodo de campos personalizados:

```xml
    <custom-fields> 
        <custom-field name="seo-search-title"><![CDATA[Geometrixx Search Results]]></custom-field> 
        <custom-field name="seo-search-keywords"><![CDATA[]]></custom-field> 
    </custom-fields> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Nodo </p> </th> 
   <th colname="col2" class="entry"> <p>Nodo principal </p> </th> 
   <th colname="col3" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p>campos personalizados </p> </td> 
   <td colname="col2"> <p>resultados del cliente </p> </td> 
   <td colname="col3"> <p> Puede contener nodos secundarios 0-n que definen campos personalizados. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>custom-field </p> </td> 
   <td colname="col2"> <p>campos personalizados </p> </td> 
   <td colname="col3"> <p> Opcional. Contiene un valor para un campo personalizado determinado indicado por el atributo name. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Facetas {#section_BE98990E3DD748A1BD4E0CA322314B79}

Las facetas son opciones de refinamiento que permiten a los clientes filtrar los resultados. Las facetas suelen utilizarse para la categorización, intervalos de precios, selecciones de color y otro refinamiento de atributos. Las facetas se crean sobre los metadatos del índice.

Es habitual ocultar o mostrar facetas de categorización a medida que un cliente baja por la categorización. El nivel más alto de categorización (categoría) se conoce como Nivel 1. Cuando un cliente hace clic en una opción de Nivel 1, aparecen las opciones de refinamiento de Nivel 2 (subcategoría) y desaparecen las opciones de Nivel 1. Cuando un cliente hace clic en una opción de Nivel 2, aparecen las opciones de refinamiento de Nivel 3 (subcategoría) y desaparecen las opciones de Nivel 2. Como se ha señalado anteriormente, estas opciones están ocultas y se muestran; la aplicación web no los afecta.

Cada faceta está contenida dentro de etiquetas `<facet-item>` . En el siguiente ejemplo, muestra una faceta que permite al cliente refinar los resultados de búsqueda por &quot;vacaciones&quot;.

Ejemplo de bloque de facetas:

```xml
<facets>          
     <facet> 
         <facet-title><![CDATA[Department]]></facet-title> 
                <behavior><![CDATA[sticky]]></behavior> 
                <selected>1</selected> 
                <undo-link><![CDATA[?i=1;lang=enus;q=*;q1=Armora+Jeans;sp_staged=1;view=xml;x1=brand]]></undo-link> 
      <facet-value> 
          <selected><![CDATA[true]]></selected> 
              <label><![CDATA[Mens]]></label> 
       <link><![CDATA[?i=1;lang=enus;q=*;q1=Armora+Jeans;q2=Mens;sp_staged=1;view=xml;x1=brand;x2=leveli]]></link> 
       <count><![CDATA[3]]></count> 
                        <undolink><![CDATA[?i=1;lang=enus;q=*;q1=Armora+Jeans;sp_staged=1;view=xml;x1=brand]]></undolink> 
      </facet-value> 
      </facet> 
     <facet> 
         <facet-title><![CDATA[Sub-Category]]></facet-title> 
                <behavior><![CDATA[sticky]]></behavior> 
                <selected>0</selected> 
      <facet-value>           
              <label><![CDATA[Apparel]]></label> 
       <link><![CDATA[?i=1;lang=enus;q=*;q1=Mens;q2=Armora+Jeans;q3=Apparel;sp_staged=1;view=xml;x1=leveli;x2=brand;x3=levelii]]></link> 
       <count><![CDATA[3]]></count>                         
      </facet-value>   
      </facet>         
     <facet> 
         <facet-title><![CDATA[Brand]]></facet-title> 
                <behavior><![CDATA[multi-select]]></behavior> 
                <selected>1</selected> 
                <undo-link><![CDATA[?i=1;lang=enus;q=*;q1=Mens;sp_staged=1;view=xml;x1=leveli]]></undo-link> 
      <facet-value>        
              <label><![CDATA[Amoura]]></label> 
       <link><![CDATA[?i=1;lang=enus;q=*;q1=Mens;q2=Armora+Jeans|Amoura;sp_staged=1;view=xml;x1=leveli;x2=brand]]></link> 
       <count><![CDATA[9]]></count>                         
      </facet-value>   
      <facet-value>         
              <label><![CDATA[Armora]]></label> 
       <link><![CDATA[?i=1;lang=enus;q=*;q1=Mens;q2=Armora+Jeans|Armora;sp_staged=1;view=xml;x1=leveli;x2=brand]]></link> 
       <count><![CDATA[12]]></count>                        
      </facet-value>   
      <facet-value> 
          <selected><![CDATA[true]]></selected> 
              <label><![CDATA[Armora Jeans]]></label> 
       <link><![CDATA[?i=1;lang=enus;q=*;q1=Mens;q2=Armora+Jeans|Armora+Jeans;sp_staged=1;view=xml;x1=leveli;x2=brand]]></link> 
 
       <count><![CDATA[3]]></count> 
                        <undolink><![CDATA[?i=1;lang=enus;q=*;q1=Mens;sp_staged=1;view=xml;x1=leveli]]></undolink> 
      </facet-value>   
      <facet-value>           
              <label><![CDATA[Art of Grooming]]></label> 
       <link><![CDATA[?i=1;lang=enus;q=*;q1=Mens;q2=Armora+Jeans|Art+of+Grooming;sp_staged=1;view=xml;x1=leveli;x2=brand]]></link> 
       <count><![CDATA[4]]></count>                         
      </facet-value>   
      <facet-value>          
              <label><![CDATA[Bear Co.]]></label> 
       <link><![CDATA[?i=1;lang=enus;q=*;q1=Mens;q2=Armora+Jeans|Bear+Co.;sp_staged=1;view=xml;x1=leveli;x2=brand]]></link> 
       <count><![CDATA[1]]></count> 
      </facet-value> 
      <facet-value>      
              <label><![CDATA[Citizens]]></label> 
       <link><![CDATA[?i=1;lang=enus;q=*;q1=Mens;q2=Armora+Jeans|Citizens;sp_staged=1;view=xml;x1=leveli;x2=brand]]></link> 
       <count><![CDATA[4]]></count> 
      </facet-value> 
      <facet-value> 
              <label><![CDATA[D&amp;B]]></label> 
       <link><![CDATA[?i=1;lang=enus;q=*;q1=Mens;q2=Armora+Jeans|D%26B;sp_staged=1;view=xml;x1=leveli;x2=brand]]></link> 
       <count><![CDATA[17]]></count> 
      </facet-value> 
      <facet-value> 
              <label><![CDATA[David Yuri]]></label> 
       <link><![CDATA[?i=1;lang=enus;q=*;q1=Mens;q2=Armora+Jeans|David+Yuri;sp_staged=1;view=xml;x1=leveli;x2=brand]]></link> 
       <count><![CDATA[2]]></count>    
      </facet-value>   
      </facet> 
    </facets> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Nodo </p> </th> 
   <th colname="col2" class="entry"> <p>Nodo principal </p> </th> 
   <th colname="col3" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p>facetas </p> </td> 
   <td colname="col2"> <p>resultados del cliente </p> </td> 
   <td colname="col3"> <p>El nodo de facetas de contenedor que tiene 0-n nodos secundarios que representan cada faceta. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>faceta </p> </td> 
   <td colname="col2"> <p>facetas </p> </td> 
   <td colname="col3"> <p> Una instancia de faceta única. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>facet-title </p> </td> 
   <td colname="col2"> <p>faceta </p> </td> 
   <td colname="col3"> <p>Título orientado al cliente para la faceta. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>comportamiento </p> </td> 
   <td colname="col2"> <p>faceta </p> </td> 
   <td colname="col3"> <p>El comportamiento de la faceta. Por ejemplo, normal, adhesivo o selección múltiple. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>seleccionados </p> </td> 
   <td colname="col2"> <p>faceta </p> </td> 
   <td colname="col3"> <p>1 si la faceta tiene un valor seleccionado; en caso contrario, 0. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>deshacer enlace </p> </td> 
   <td colname="col2"> <p>faceta </p> </td> 
   <td colname="col3"> <p> Solo está presente cuando se selecciona la faceta. Deshacer vínculo revierte toda la faceta. Por ejemplo, cuando se trata de una faceta de selección múltiple, anula la selección de todas las opciones seleccionadas para la faceta. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>facet-value </p> </td> 
   <td colname="col2"> <p>faceta </p> </td> 
   <td colname="col3"> <p>Contiene todos los elementos de faceta individuales que pertenecen a la faceta. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>seleccionados </p> </td> 
   <td colname="col2"> <p>facet-value </p> </td> 
   <td colname="col3"> <p>Si se selecciona el elemento actual con la faceta, este nodo está presente y se establece en "true". </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>label </p> </td> 
   <td colname="col2"> <p>facet-value </p> </td> 
   <td colname="col3"> <p>Etiqueta de cara al cliente para la opción faceta. De forma predeterminada, esto ya debería estar garantizado por el escape HTML. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>vínculo </p> </td> 
   <td colname="col2"> <p>facet-value </p> </td> 
   <td colname="col3"> <p> Vínculo relativo a los resultados que la opción perfecciona aún más. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>count </p> </td> 
   <td colname="col2"> <p>facet-value </p> </td> 
   <td colname="col3"> <p>Número de resultados en ese conjunto de resultados refinado. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>deshacer enlace </p> </td> 
   <td colname="col2"> <p>facet-value </p> </td> 
   <td colname="col3"> <p>Cuando se selecciona un valor de faceta, el nodo devuelve un "vínculo de deshacer" que permite al cliente volver a seleccionar esa selección de facetas individual. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Encabezado {#section_5305B1DC5774439485CA0665DB683F9C}

Ejemplo:

```xml
xml version="1.0" encoding="utf-8" standalone="yes" 
```

## Menús y ordenación {#section_A34CBB645DBF4C70A12A5B7E81211295}

Se admiten los menús para ordenar los resultados y cambiar el número de resultados que se van a devolver por página. También es compatible con un menú de navegación que resulta útil para utilizar &quot;buscar como navegación&quot;. Una cuenta puede definir varios menús del mismo tipo y utilizar cualquiera de los menús para su presentación.

Nodo de menús de ejemplo:

En el siguiente ejemplo se muestran los datos de un menú de ordenación y un menú de navegación de tres opciones. El menú de ordenación permite al cliente ordenar por relevancia, título o clasificación. El elemento seleccionado actualmente incluye un atributo &quot;selected=true&quot;. &quot;. Ofrecer siempre una opción de relevancia para permitir que un cliente vuelva a los resultados de búsqueda predeterminados que se mostraron originalmente.

```xml
<menus> 
        <menu> 
           <name><![CDATA[sort]]></name>         
             <item selected="true"> 
          <label><![CDATA[Relevance]]></label> 
          <value><![CDATA[relevance]]></value> 
          <link><![CDATA[ ]]></link> 
             </item> 
             <item> 
          <label><![CDATA[Lowest Price]]></label> 
          <value><![CDATA[Price]]></value> 
          <link><![CDATA[?i=1;q=mens;sort=Price;sp_cs=UTF-8;sp_staged=1;view=xml]]></link>     
             </item> 
             <item> 
          <label><![CDATA[Highest Price]]></label> 
          <value><![CDATA[Price_r]]></value> 
          <link><![CDATA[?i=1;q=mens;sort=Price_r;sp_cs=UTF-8;sp_staged=1;view=xml]]></link>     
             </item> 
             <item> 
          <label><![CDATA[Brand]]></label> 
          <value><![CDATA[brand]]></value> 
          <link><![CDATA[?i=1;q=mens;sort=brand;sp_cs=UTF-8;sp_staged=1;view=xml]]></link>     
             </item> 
        </menu> 
        <menu> 
            <name><![CDATA[ss_head_nav]]></name>   
                    <item> 
                        <label><![CDATA[WOMEN'S]]></label> 
          <value><![CDATA[?q1=Womens;sp_sfvl_field=levelii|leveli|brand|leveliii;x=0;x1=leveli;y=0;view=nav;top=1]]></value> 
          <link><![CDATA[?q1=Womens;sp_sfvl_field=levelii|leveli|brand|leveliii;x=0;x1=leveli;y=0;view=nav;top=1;i=1;m_ss_head_nav=WOMEN'S]]></link> 
                    </item> 
                    <item> 
                        <label><![CDATA[MEN'S]]></label> 
          <value><![CDATA[/q1/Mens/x1/leveli/view/nav/top/1/]]></value> 
          <link><![CDATA[/q1/Mens/x1/leveli/view/nav/top/1/]]></link> 
                    </item> 
                    <item> 
                        <label><![CDATA[JEWELRY & ACCESSORIES]]></label> 
          <value><![CDATA[?q1=Jewelry+%26+Accessories&sp_sfvl_field=levelii|leveli|brand|leveliii&x1=leveli&view=nav&top=1]]></value> 
          <link><![CDATA[?q1=Jewelry+%26+Accessories&sp_sfvl_field=levelii|leveli|brand|leveliii&x1=leveli&view=nav&top=1;i=1;m_ss_head_nav=JEWELRY+%26+ACCESSORIES]]></link> 
                    </item> 
                    <item> 
                        <label><![CDATA[BEAUTY & FRAGRANCE]]></label> 
          <value><![CDATA[?q1=Beauty+%26+Fragrance;sp_sfvl_field=levelii|leveli|brand|leveliii;x1=leveli;view=nav;top=1]]></value> 
          <link><![CDATA[?q1=Beauty+%26+Fragrance;sp_sfvl_field=levelii|leveli|brand|leveliii;x1=leveli;view=nav;top=1;i=1;m_ss_head_nav=BEAUTY+%26+FRAGRANCE]]></link> 
                    </item> 
                    <item> 
                        <label><![CDATA[GIFTS & HOME]]></label> 
          <value><![CDATA[?q1=Gifts+%26+Home;sp_sfvl_field=levelii|leveli|brand|leveliii;x1=leveli;view=nav;top=1]]></value> 
          <link><![CDATA[?q1=Gifts+%26+Home;sp_sfvl_field=levelii|leveli|brand|leveliii;x1=leveli;view=nav;top=1;i=1;m_ss_head_nav=GIFTS+%26+HOME]]></link> 
                    </item> 
                    <item> 
                        <label><![CDATA[CHILDREN & TOYS]]></label> 
          <value><![CDATA[?q1=Children+%26+Toys;sp_sfvl_field=levelii|leveli|brand|leveliii;x1=leveli;view=nav;top=1]]></value> 
          <link><![CDATA[?q1=Children+%26+Toys;sp_sfvl_field=levelii|leveli|brand|leveliii;x1=leveli;view=nav;top=1;i=1;m_ss_head_nav=CHILDREN+%26+TOYS]]></link> 
                    </item> 
                    <item> 
                        <label><![CDATA[ELECTRONICS]]></label> 
          <value><![CDATA[?q1=Electronics+%26+Toys;sp_sfvl_field=levelii|leveli|brand|leveliii;x1=leveli;view=nav;top=1]]></value> 
          <link><![CDATA[?q1=Electronics+%26+Toys;sp_sfvl_field=levelii|leveli|brand|leveliii;x1=leveli;view=nav;top=1;i=1;m_ss_head_nav=ELECTRONICS]]></link> 
                    </item> 
        </menu> 
    </menus> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Nodo </p> </th> 
   <th colname="col2" class="entry"> <p>Nodo principal </p> </th> 
   <th colname="col3" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p>menús </p> </td> 
   <td colname="col2"> <p>resultados del cliente </p> </td> 
   <td colname="col3"> <p>Contiene nodos secundarios 0-n que definen cada menú. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>Todas las aplicaciones </p> </td> 
   <td colname="col2"> <p>menús </p> </td> 
   <td colname="col3"> <p>Instancia única de un menú (corresponde a un menú definido en <span class="uicontrol"> Diseño </span> &gt; <span class="uicontrol"> Navegación </span> &gt; <span class="uicontrol"> Menús </span>). </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>name </p> </td> 
   <td colname="col2"> <p>Todas las aplicaciones </p> </td> 
   <td colname="col3"> <p>Nombre del menú. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>elemento </p> </td> 
   <td colname="col2"> <p>Todas las aplicaciones </p> </td> 
   <td colname="col3"> <p>Define cada elemento del menú. El atributo opcional seleccionado se establece en true si el elemento de menú dado está seleccionado actualmente. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>label </p> </td> 
   <td colname="col2"> <p>elemento </p> </td> 
   <td colname="col3"> <p>Texto orientado al cliente para el elemento de menú. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>value </p> </td> 
   <td colname="col2"> <p>elemento </p> </td> 
   <td colname="col3"> <p>Representa el valor del elemento de menú (el valor del parámetro de consulta con el que está establecido el menú) . Esta etiqueta no es necesaria si se utiliza el valor &lt;link&gt; . </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>vínculo </p> </td> 
   <td colname="col2"> <p>elemento </p> </td> 
   <td colname="col3"> <p>Para las opciones no seleccionadas, el parámetro &lt;link&gt; contiene el vínculo relativo que devuelve el mismo conjunto de resultados, pero con la opción de menú aplicada. Este campo está en blanco para la opción de ordenación seleccionada actualmente. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Paginación {#section_E52F81C6A6EB4B8F996157B657EC540F}

Los conjuntos de resultados se dividen en varias páginas. Normalmente, los clientes muestran entre 10 y 20 resultados en una sola página. Los resultados posteriores se muestran en la página siguiente. El XML de paginación le permite crear un conjunto de vínculos de navegación para que sus clientes puedan navegar, página por página, a través de los conjuntos de resultados. Hay cuatro vínculos de navegación disponibles: primero, último, siguiente y anterior. Cada tipo de vínculo permite a los clientes desplazarse rápidamente por las páginas para revisar y perfeccionar lo que buscan fácilmente.

El siguiente ejemplo muestra la paginación de una búsqueda que se encuentra en la primera página con la paginación configurada para mostrar vínculos a cinco páginas.

Ejemplo de paginación:

```xml
    <pagination> 
        <total-pages><![CDATA[112]]></total-pages> 
        <pages> 
     <page position="first"><![CDATA[]]></page> 
     <page position="last"><![CDATA[?i=1;page=112;q=*;sp_cs=UTF-8;sp_staged=1;view=xml]]></page> 
     <page position="next"><![CDATA[?i=1;page=2;q=*;sp_cs=UTF-8;sp_staged=1;view=xml]]></page> 
            <page position="1" selected="true"><![CDATA[?i=1;q=*;sp_cs=UTF-8;sp_staged=1;view=xml]]></page> 
            <page position="2"><![CDATA[?i=1;page=2;q=*;sp_cs=UTF-8;sp_staged=1;view=xml]]></page> 
            <page position="3"><![CDATA[?i=1;page=3;q=*;sp_cs=UTF-8;sp_staged=1;view=xml]]></page> 
            <page position="4"><![CDATA[?i=1;page=4;q=*;sp_cs=UTF-8;sp_staged=1;view=xml]]></page> 
            <page position="5"><![CDATA[?i=1;page=5;q=*;sp_cs=UTF-8;sp_staged=1;view=xml]]></page> 
        </pages> 
    </pagination> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Nodo </p> </th> 
   <th colname="col2" class="entry"> <p>Nodo principal </p> </th> 
   <th colname="col3" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p>paginación </p> </td> 
   <td colname="col2"> <p>resultados del cliente </p> </td> 
   <td colname="col3"> <p> Número total de páginas de resultados, según el número de resultados dividido por el número de resultados por página. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>páginas totales </p> </td> 
   <td colname="col2"> <p>paginación </p> </td> 
   <td colname="col3"> <p>La cantidad total de páginas por las que se distribuyen los resultados de búsqueda. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>páginas </p> </td> 
   <td colname="col2"> <p>paginación </p> </td> 
   <td colname="col3"> <p>Contiene nodos de página 0-n que definen cada página de la paginación. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>página </p> </td> 
   <td colname="col2"> <p>páginas </p> </td> 
   <td colname="col3"> <p>Existen cuatro nodos de página especiales: primero, último, anterior y siguiente. Estas cuatro páginas son opcionales y aparecen en el conjunto de resultados solo si tienen sentido. Por ejemplo, si está en la página 1, no hay ningún vínculo "anterior". Todas las demás páginas indican una posición. El número de páginas que se enumeran depende del "número de vínculos a páginas" configurado en la interfaz de usuario de paginación. El atributo "seleccionado" indica la página en la que se encuentra el cliente. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Consulta {#section_3DAA1013F09742869B80F6A361816E6C}

Ejemplo de nodo de consulta:

```xml
    <query> 
        <user-query><![CDATA[mens]]></user-query> 
 <lower-results><![CDATA[1]]></lower-results> 
 <upper-results><![CDATA[12]]></upper-results> 
 <total-results><![CDATA[265]]></total-results> 
    </query> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Nodo </p> </th> 
   <th colname="col2" class="entry"> <p>Nodo principal </p> </th> 
   <th colname="col3" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p>query </p> </td> 
   <td colname="col2"> <p>resultados del cliente </p> </td> 
   <td colname="col3"> <p> Nodo global que proporciona información general de la consulta. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>user-query </p> </td> 
   <td colname="col2"> <p>query </p> </td> 
   <td colname="col3"> <p> La palabra clave que se buscó. Si <span class="uicontrol"> ¿Quiso decir </span> automáticamente buscó un término sugerido debido a que el término original no arrojó ningún resultado, se refleja en la nueva palabra clave que se buscó (vea el nodo de sugerencias para obtener la palabra clave original). </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>resultados inferiores </p> </td> 
   <td colname="col2"> <p>query </p> </td> 
   <td colname="col3"> <p> El número de artículo del primer resultado de esta página. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>resultados superiores </p> </td> 
   <td colname="col2"> <p>query </p> </td> 
   <td colname="col3"> <p> El número de artículo del último resultado de esta página. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>total-results </p> </td> 
   <td colname="col2"> <p>query </p> </td> 
   <td colname="col3"> <p> Número total de resultados que coinciden con la consulta del usuario. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Búsquedas recientes {#section_17F942F6EC07456DABED7A483AC08446}

Búsquedas recientes es una función basada en cookies que solo funciona si retransmite la información de las cookies a los servidores de búsqueda y comercialización del sitio.

Ejemplo de búsquedas recientes:

```xml
    <recent-searches> 
        <clear-link><![?q=womens&gscr=clear]]></clear-link> 
        <recent-search> 
            <link><![?q=mens]]></link> 
            <label><![CDATA[mens]]></label> 
        <recent-search> 
    </recent-searches> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Nodo </p> </th> 
   <th colname="col2" class="entry"> <p>Nodo principal </p> </th> 
   <th colname="col3" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p>recent-searches </p> </td> 
   <td colname="col2"> <p>resultados del cliente </p> </td> 
   <td colname="col3"> <p>El nodo solo está presente si la búsqueda tiene búsquedas recientes. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>clear-link </p> </td> 
   <td colname="col2"> <p>recent-searches </p> </td> 
   <td colname="col3"> <p>Ruta relativa que borra todas las búsquedas recientes del cliente. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>recent-search </p> </td> 
   <td colname="col2"> <p>recent-searches </p> </td> 
   <td colname="col3"> <p>Define en búsquedas recientes. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>vínculo </p> </td> 
   <td colname="col2"> <p>recent-search </p> </td> 
   <td colname="col3"> <p>La ruta para crear un vínculo que realice una búsqueda que el usuario haya realizado recientemente. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>label </p> </td> 
   <td colname="col2"> <p>recent-search </p> </td> 
   <td colname="col3"> <p>Etiqueta de visualización de cara al cliente para la búsqueda reciente. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Resultados {#section_155A80B8C4F641678DD9C8F257108412}

El conjunto Results es un área personalizable de la respuesta XML. Cada índice es único en los mecanismos de asignación de nombres de campo de los metadatos. Hay campos comunes que se devuelven para cada resultado, como título, descripción y dirección URL. Sin embargo, los metadatos definidos para una página del índice pueden estar disponibles para su uso en cada nodo de resultado. La categorización, los precios, los colores y las miniaturas son solo algunas de las opciones que puede aplicar a un resultado para producir resultados de búsqueda más convincentes.

El formato de resultados se personaliza en función de los metadatos específicos de la implementación. Aquí puede encontrar todos los datos por resultado para mostrarlos en los resultados, incluidas las URL de imágenes en miniatura.

Además, es posible configurar varias zonas de resultados dentro de la página, como &quot;Resultados destacados&quot; o secciones de resultados &quot;Productos&quot; y &quot;Contenido&quot; independientes. En estos casos, se proporcionan varias zonas de resultados dentro del HTML, aunque las facetas solo están asociadas con el conjunto de resultados principal.

Nodo de resultados de ejemplo:

```xml
    <results> 
        <result-set> 
            <name><![CDATA[default]]></name> 
         <result> 
                    <field name="index"><![CDATA[1]]></field> 
                    <field name="sku"><![CDATA[200190]]></field> 
                    <field name="pagename"><![CDATA[Relaxed Paint Splattered]]></field> 
 
                    <field name="img_sm_url"><![CDATA[https://geometrixx.com/images/08_geometrixx_icon_men.jpg]]></field> 
      <field name="brand"><![CDATA[Armora Jeans]]></field> 
      <field name="price"><![CDATA[195]]></field> 
      <field name="foundIn"><![CDATA[Mens,  
            Apparel,  
          Denim]]></field> 
         </result>   
         <result> 
                    <field name="index"><![CDATA[2]]></field> 
                    <field name="sku"><![CDATA[200195]]></field> 
                    <field name="pagename"><![CDATA[Tumbled Jeans]]></field> 
 
                    <field name="img_sm_url"><![CDATA[https://geometrixx.com/images/08_geometrixx_icon_men.jpg]]></field> 
      <field name="brand"><![CDATA[Armora Jeans]]></field> 
      <field name="price"><![CDATA[235]]></field> 
      <field name="foundIn"><![CDATA[Mens,  
            Apparel,  
          Denim]]></field> 
         </result>    
         <result> 
                    <field name="index"><![CDATA[3]]></field> 
                    <field name="sku"><![CDATA[200196]]></field> 
                    <field name="pagename"><![CDATA[Montana Relaxed]]></field> 
 
                    <field name="img_sm_url"><![CDATA[https://geometrixx.com/images/08_geometrixx_icon_men.jpg]]></field> 
      <field name="brand"><![CDATA[Armora Jeans]]></field> 
      <field name="price"><![CDATA[220]]></field> 
      <field name="foundIn"><![CDATA[Mens,  
            Apparel,  
          Denim]]></field> 
         </result>         
        </result-set>   
    </results> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Nodo </p> </th> 
   <th colname="col2" class="entry"> <p>Nodo principal </p> </th> 
   <th colname="col3" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p>resultados </p> </td> 
   <td colname="col2"> <p>resultados del cliente </p> </td> 
   <td colname="col3"> <p>El nodo contenedor para conjuntos de resultados 0-n. Cero conjuntos de resultados significa que está en una página de aterrizaje especial sin resultados. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>conjunto de resultados </p> </td> 
   <td colname="col2"> <p>resultados </p> </td> 
   <td colname="col3"> <p>Una búsqueda entrante puede activar varias búsquedas. Cada conjunto de resultados contiene los resultados de una búsqueda con nombre específica que se realizó. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>name </p> </td> 
   <td colname="col2"> <p>conjunto de resultados </p> </td> 
   <td colname="col3"> <p>Nombre de la búsqueda a la que pertenece el conjunto de resultados. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>resultado </p> </td> 
   <td colname="col2"> <p>conjunto de resultados </p> </td> 
   <td colname="col3"> <p>Contiene todos los campos asociados a un resultado individual para el conjunto de resultados. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>field </p> </td> 
   <td colname="col2"> <p>resultado </p> </td> 
   <td colname="col3"> <p>El atributo name define el nombre del campo dentro del índice que se muestra. El valor es el valor real de ese campo. Algunos resultados pueden tener campos que faltan y que no son relevantes para ese resultado individual. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Buscar formulario {#section_9E4B99D4FEDC49629F6C7E866F3A7493}

El formulario de búsqueda se incluye en el conjunto de resultados para que los clientes puedan crear su formulario de búsqueda de forma dinámica. Este paso es opcional. La mayoría de los clientes tienen un formulario de búsqueda fijo. Sin embargo, permite a los clientes determinar si el formulario de búsqueda necesita un mbox de Test&amp;Target, en función de que tenga al menos una regla comercial que realice una prueba A:B. Del mismo modo, permite a los clientes recoger automáticamente la CSS y JavaScript de autocompletado más recientes.

Ejemplo de formulario de búsqueda XML:

```xml
    <search-form> 
        <include-tnt-mbox>1</include-tnt-mbox> 
        <autocomplete> 
            <enabled>1</enabled> 
            <css><![CDATA[<link rel="stylesheet" type="text/css" href="https://content.t1.atomz.com/sp10043554/stage/autocomplete_styles.css?sp_js_param=2" /> 
]]></css> 
 
            <form-content><![CDATA[<div id="autocomplete"></div> 
<input type="hidden" name="sp_staged" id="sp_staged" value="1" /> 
]]></form-content> 
            <javascript><![CDATA[<script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/yui/2.6.0/build/utilities/utilities.js"></script> 
<script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/yui/2.6.0/build/datasource/datasource-min.js"></script> 
<script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/yui/2.6.0/build/autocomplete/autocomplete-min.js"></script> 
<script type="text/javascript" src="https://content.t1.atomz.com/sp10043554/stage/autocomplete_data.js?sp_js_param=3"></script>]]></javascript> 
        </autocomplete> 
    </search-form> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Nodo </p> </th> 
   <th colname="col2" class="entry"> <p>Nodo principal </p> </th> 
   <th colname="col3" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p>search-form </p> </td> 
   <td colname="col2"> <p>resultados del cliente </p> </td> 
   <td colname="col3"> <p>Contiene datos para conducir el formulario de búsqueda. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>include-tnt-mbox </p> </td> 
   <td colname="col2"> <p> search-form </p> </td> 
   <td colname="col3"> <p>Técnicamente, solo necesita un mbox en el formulario de búsqueda cuando tiene al menos una regla comercial haciendo una prueba A:B de Test&amp;Target. Este nodo indica si necesita un mbox o no le permite reducir el número de visitas en los servidores de Test&amp;Target. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>autocompletar </p> </td> 
   <td colname="col2"> <p>search-form </p> </td> 
   <td colname="col3"> <p>Nodo secundario de casas relacionado con el autocompletado. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>enabled </p> </td> 
   <td colname="col2"> <p>autocompletar </p> </td> 
   <td colname="col3"> <p>Se establece en 1 cuando la cuenta de búsqueda utiliza el llenado automático. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>css </p> </td> 
   <td colname="col2"> <p> autocompletar </p> </td> 
   <td colname="col3"> <p> CSS para autocompletar. Coloque este nodo lo más arriba posible en la página. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p> form-content </p> </td> 
   <td colname="col2"> <p>autocompletar </p> </td> 
   <td colname="col3"> <p>Contenido que se inserta en el formulario de búsqueda. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>javascript </p> </td> 
   <td colname="col2"> <p>autocompletar </p> </td> 
   <td colname="col3"> <p>JavaScript para autocompletar. Coloque este nodo lo más bajo posible en la página. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Sugerencias {#section_2899FACB9AD84F60B3687C1B4EF09E15}

Los clientes pueden configurar la funcionalidad **[!UICONTROL Did You Mean]** de tres maneras: haga sugerencias debido a que no hay resultados, busque automáticamente la primera sugerencia cuando no haya resultados o realice sugerencias debido a que los resultados son bajos (donde las sugerencias tienen un recuento de resultados más alto). Todas las sugerencias arrojan resultados.

Este nodo de sugerencias contiene los términos que generan consultas correctas. El vínculo también se devuelve para que un cliente pueda ir a la nueva consulta.

Ejemplo de salida para hacer sugerencias debido a 0 resultados:

```xml
    <suggestions> 
        <auto-searched>0</auto-searched> 
        <suggestions-low-results>0</suggestions-low-results> 
 <suggestion-item> 
     <link><![CDATA[?i=1;q=arcade;sp_cs=UTF-8;view=xml]]></link> 
     <word><![CDATA[arcade]]></word> 
 </suggestion-item>    
    </suggestions>
```

Ejemplo de salida para buscar automáticamente con una sugerencia:

```xml
    <suggestions> 
        <auto-searched>1</auto-searched> 
        <orig-query><![CDATA[arcace]]></orig-query> 
        <suggestions-low-results>0</suggestions-low-results>         
    </suggestions> 
```

Ejemplo de salida para hacer sugerencias debido a resultados bajos:

```xml
   <suggestions> 
        <auto-searched>0</auto-searched> 
        <suggestions-low-results>1</suggestions-low-results> 
 <suggestion-item> 
     <link><![CDATA[?i=1;q=coffee;sp_cs=UTF-8;view=xml]]></link> 
     <word><![CDATA[coffee]]></word> 
 </suggestion-item>  
    </suggestions> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Nodo </p> </th> 
   <th colname="col2" class="entry"> <p>Nodo principal </p> </th> 
   <th colname="col3" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p>sugerencia </p> </td> 
   <td colname="col2"> <p>resultados del cliente </p> </td> 
   <td colname="col3"> <p> Contiene nodos secundarios que definen sugerencias, si existen. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>búsqueda automática </p> </td> 
   <td colname="col2"> <p>sugerencias </p> </td> 
   <td colname="col3"> <p> Si está presente, indica si la búsqueda o comercialización del sitio se ha buscado automáticamente con un nuevo término debido a que no se han obtenido resultados. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>orig-query </p> </td> 
   <td colname="col2"> <p>sugerencias </p> </td> 
   <td colname="col3"> <p> Cuando la búsqueda/comercialización del sitio busca automáticamente la primera sugerencia, la consulta del usuario en el nodo de consulta muestra la palabra clave con la que se ha buscado. Este nodo muestra el término de consulta original. La combinación de ambos permite a los clientes crear estructuras como "Buscando una arcada en lugar de un espacio". </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>recommendations-low-results </p> </td> 
   <td colname="col2"> <p>sugerencias </p> </td> 
   <td colname="col3"> <p>Si está presente, indica si la búsqueda/comercialización del sitio está haciendo sugerencias debido a que el término de búsqueda actual arroja bajos resultados y una sugerencia que arroja resultados considerablemente más altos. Los dos umbrales se pueden configurar en <span class="uicontrol"> ¿Quiso decir </span>? </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>sugerencia-elemento </p> </td> 
   <td colname="col2"> <p>sugerencias </p> </td> 
   <td colname="col3"> <p>Contiene nodos 0-n que indican las distintas sugerencias. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>vínculo </p> </td> 
   <td colname="col2"> <p>sugerencia-elemento </p> </td> 
   <td colname="col3"> <p>Contiene la ruta para crear un vínculo al término sugerido. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>word </p> </td> 
   <td colname="col2"> <p>sugerencia-elemento </p> </td> 
   <td colname="col3"> <p> Contiene la palabra sugerida. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Plantilla {#section_1E2BB2F274B04F5491A4CCCC38F507BD}

Se admite la capacidad de cambiar una experiencia de búsqueda de clientes según los resultados. Parte de esto implica cambiar entre plantillas diferentes con un diseño diferente de los resultados de búsqueda. Por ejemplo, puede tener una plantilla con una vista de cuadrícula de los productos para cuando tenga muchos productos. O puede tener una plantilla &quot;destacado&quot; al mostrar un solo resultado que tenga más detalles. También puede tener una plantilla &quot;sin resultados&quot; cuando una búsqueda no arroja ningún resultado. El nodo plantilla indica qué plantilla se utiliza para mostrar los resultados de la búsqueda.

Plantilla de ejemplo:

```xml
<template><![CDATA[grid]]></template>
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Nodo </p> </th> 
   <th colname="col2" class="entry"> <p>Nodo principal </p> </th> 
   <th colname="col3" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p>plantilla </p> </td> 
   <td colname="col2"> <p>resultados del cliente </p> </td> 
   <td colname="col3"> <p>Indica el nombre de la plantilla que se usa para mostrar los resultados de la búsqueda. </p> </td> 
  </tr> 
 </tbody> 
</table>

## Zonas {#section_26C4A947E7B1474A8E37D86D9579B93E}

Las zonas son secciones de las páginas que pueden activarse o desactivarse mediante reglas comerciales. Una zona puede contener cualquier contenido, incluidas, entre otras, facetas, búsquedas, rutas de exploración y contenido estático. Las zonas de la página web de los clientes deben asignarse a las mismas áreas que la búsqueda o comercialización del sitio.

Ejemplo de nodos de zona:

```xml
    <zones> 
        <zone> 
            <name><![CDATA[brand-facet]]></name> 
            <display>1</display> 
        </zone> 
    </zones> 
```

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Nodo </p> </th> 
   <th colname="col2" class="entry"> <p>Nodo principal </p> </th> 
   <th colname="col3" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p>zonas </p> </td> 
   <td colname="col2"> <p>resultados del cliente </p> </td> 
   <td colname="col3"> <p>Contiene zonas 0-n. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>zone </p> </td> 
   <td colname="col2"> <p>zonas </p> </td> 
   <td colname="col3"> <p> Un nodo de zona individual. Puede tener varios nodos de zona. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>name </p> </td> 
   <td colname="col2"> <p>zone </p> </td> 
   <td colname="col3"> <p>Nombre de la zona. </p> </td> 
  </tr> 
  <tr> 
   <td colname="col1"> <p>visualización </p> </td> 
   <td colname="col2"> <p>1 o 0, indicando si la zona correspondiente al nombre de la zona se muestra u oculta. </p> </td> 
   <td colname="col3"> <p> </p> </td> 
  </tr> 
 </tbody> 
</table>

## Ejemplos {#reference_64B7D8D228AF4B8D90EDF4DE507B0F84}

Ejemplo de salida para una búsqueda * en un sitio web ficticio llamado Geometrixx y ejemplo de plantilla de presentación que se utiliza para producir el ejemplo de salida.

* [Salida de ejemplo](../c-appendices/c-guidedsearchoutput.md#section_515C000A18B847D59097D0A9CCC02636)
* [Ejemplo de plantilla de presentación](../c-appendices/c-guidedsearchoutput.md#section_AD42571DFB88491AA7F0FDF0929EBE97)

## Ejemplo de salida {#section_515C000A18B847D59097D0A9CCC02636}

Ejemplo de salida para una búsqueda * en un sitio web ficticio llamado Geometrixx.

```xml
<?xml version="1.0" encoding="utf-8" standalone="yes" ?> 
<customer-results> 
    <query> 
        <user-query><![CDATA[*]]></user-query> 
 <lower-results><![CDATA[1]]></lower-results> 
 <upper-results><![CDATA[12]]></upper-results> 
 <total-results><![CDATA[1337]]></total-results> 
    </query> 
 
    <custom-fields> 
 
        <custom-field name="seo-search-title"><![CDATA[Geometrixx Search Results]]></custom-field> 
        <custom-field name="seo-search-keywords"><![CDATA[]]></custom-field> 
    </custom-fields> 
 
    <menus> 
 
        <menu> 
           <name>sort</name>

             <item selected="true"> 
 
          <label><![CDATA[Relevance]]></label> 
          <value><![CDATA[relevance]]></value> 
          <link><![CDATA[ ]]></link> 
             </item>

             <item> 
          <label><![CDATA[Lowest Price]]></label> 
          <value><![CDATA[Price]]></value> 
          <link><![CDATA[?i=1;q=*;sort=Price;sp_cs=UTF-8;sp_staged=1;view=xml]]></link>     
             </item>

             <item> 
          <label><![CDATA[Highest Price]]></label> 
          <value><![CDATA[Price_r]]></value> 
          <link><![CDATA[?i=1;q=*;sort=Price_r;sp_cs=UTF-8;sp_staged=1;view=xml]]></link>     
             </item>

             <item> 
          <label><![CDATA[Brand]]></label> 
          <value><![CDATA[brand]]></value> 
          <link><![CDATA[?i=1;q=*;sort=brand;sp_cs=UTF-8;sp_staged=1;view=xml]]></link>     
             </item>

        </menu> 
        <menu> 
            <name><![CDATA[ss_head_nav]]></name>

                    <label><![CDATA[WOMEN'S]]></label> 
      <value><![CDATA[?q1=Womens;sp_sfvl_field=levelii|leveli|brand|leveliii;x=0;x1=leveli;y=0;view=nav;top=1]]></value> 
      <link><![CDATA[?q1=Womens;sp_sfvl_field=levelii|leveli|brand|leveliii;x=0;x1=leveli;y=0;view=nav;top=1;i=1;m_ss_head_nav=WOMEN'S]]></link>

                    <label><![CDATA[MEN'S]]></label> 
      <value><![CDATA[/q1/Mens/x1/leveli/view/nav/top/1/]]></value> 
      <link><![CDATA[/q1/Mens/x1/leveli/view/nav/top/1/]]></link>

                    <label><![CDATA[JEWELRY & ACCESSORIES]]></label> 
      <value><![CDATA[?q1=Jewelry+%26+Accessories&sp_sfvl_field=levelii|leveli|brand|leveliii&x1=leveli&view=nav&top=1]]></value> 
      <link><![CDATA[?q1=Jewelry+%26+Accessories&sp_sfvl_field=levelii|leveli|brand|leveliii&x1=leveli&view=nav&top=1;i=1;m_ss_head_nav=JEWELRY+%26+ACCESSORIES]]></link>

                    <label><![CDATA[BEAUTY & FRAGRANCE]]></label> 
      <value><![CDATA[?q1=Beauty+%26+Fragrance;sp_sfvl_field=levelii|leveli|brand|leveliii;x1=leveli;view=nav;top=1]]></value> 
      <link><![CDATA[?q1=Beauty+%26+Fragrance;sp_sfvl_field=levelii|leveli|brand|leveliii;x1=leveli;view=nav;top=1;i=1;m_ss_head_nav=BEAUTY+%26+FRAGRANCE]]></link>

                    <label><![CDATA[GIFTS & HOME]]></label> 
      <value><![CDATA[?q1=Gifts+%26+Home;sp_sfvl_field=levelii|leveli|brand|leveliii;x1=leveli;view=nav;top=1]]></value> 
      <link><![CDATA[?q1=Gifts+%26+Home;sp_sfvl_field=levelii|leveli|brand|leveliii;x1=leveli;view=nav;top=1;i=1;m_ss_head_nav=GIFTS+%26+HOME]]></link>

                    <label><![CDATA[CHILDREN & TOYS]]></label> 
      <value><![CDATA[?q1=Children+%26+Toys;sp_sfvl_field=levelii|leveli|brand|leveliii;x1=leveli;view=nav;top=1]]></value> 
      <link><![CDATA[?q1=Children+%26+Toys;sp_sfvl_field=levelii|leveli|brand|leveliii;x1=leveli;view=nav;top=1;i=1;m_ss_head_nav=CHILDREN+%26+TOYS]]></link>

                    <label><![CDATA[ELECTRONICS]]></label> 
      <value><![CDATA[?q1=Electronics+%26+Toys;sp_sfvl_field=levelii|leveli|brand|leveliii;x1=leveli;view=nav;top=1]]></value> 
      <link><![CDATA[?q1=Electronics+%26+Toys;sp_sfvl_field=levelii|leveli|brand|leveliii;x1=leveli;view=nav;top=1;i=1;m_ss_head_nav=ELECTRONICS]]></link>

        </menu> 
    </menus> 
 
    <breadcrumbs> 
  <breadcrumb> 
            <name><![CDATA[default]]></name> 
       
  <breadcrumb-item> 
    <link><![CDATA[?i=1;q=*;sp_cs=UTF-8;sp_staged=1;view=xml]]></link> 
    <value><![CDATA[*]]></value> 
                        <label><![CDATA[]]></label> 
   </breadcrumb-item> 
          
   </breadcrumb> 
 
    </breadcrumbs> 
 
    <suggestions> 
        <auto-searched>0</auto-searched> 
         
        <suggestions-low-results>0</suggestions-low-results> 
         
    </suggestions> 
 
    <pagination> 
        <total-pages><![CDATA[112]]></total-pages> 
 
        <pages> 
     <page position="first"><![CDATA[]]></page> 
     <page position="last"><![CDATA[?i=1;page=112;q=*;sp_cs=UTF-8;sp_staged=1;view=xml]]></page> 
      
     <page position="next"><![CDATA[?i=1;page=2;q=*;sp_cs=UTF-8;sp_staged=1;view=xml]]></page>

                <page position="1" selected="true"><![CDATA[?i=1;q=*;sp_cs=UTF-8;sp_staged=1;view=xml]]></page>

                <page position="2"><![CDATA[?i=1;page=2;q=*;sp_cs=UTF-8;sp_staged=1;view=xml]]></page>

                <page position="3"><![CDATA[?i=1;page=3;q=*;sp_cs=UTF-8;sp_staged=1;view=xml]]></page>

                <page position="4"><![CDATA[?i=1;page=4;q=*;sp_cs=UTF-8;sp_staged=1;view=xml]]></page>

                <page position="5"><![CDATA[?i=1;page=5;q=*;sp_cs=UTF-8;sp_staged=1;view=xml]]></page>

        </pages> 
    </pagination> 
 
    <facets>  
         
     <facet-item> 
         <facet-title><![CDATA[Department]]></facet-title> 
                <selected>0</selected>

      <facet-value> 
           
              <label><![CDATA[Womens]]></label> 
 
       <link><![CDATA[?i=1;q=*;q1=Womens;sp_cs=UTF-8;sp_staged=1;view=xml;x1=leveli]]></link> 
       <count><![CDATA[219]]></count> 
                         
      </facet-value> 
   
      <facet-value> 
           
              <label><![CDATA[Mens]]></label> 
       <link><![CDATA[?i=1;q=*;q1=Mens;sp_cs=UTF-8;sp_staged=1;view=xml;x1=leveli]]></link> 
       <count><![CDATA[202]]></count> 
                         
      </facet-value> 
   
      <facet-value>

              <label><![CDATA[Beauty &amp; Fragrance]]></label> 
       <link><![CDATA[?i=1;q=*;q1=Beauty+%26+Fragrance;sp_cs=UTF-8;sp_staged=1;view=xml;x1=leveli]]></link> 
       <count><![CDATA[169]]></count> 
                         
      </facet-value> 
   
      <facet-value> 
           
              <label><![CDATA[Children &amp; Toys]]></label> 
       <link><![CDATA[?i=1;q=*;q1=Children+%26+Toys;sp_cs=UTF-8;sp_staged=1;view=xml;x1=leveli]]></link> 
       <count><![CDATA[209]]></count> 
                         
      </facet-value>

      <facet-value> 
           
              <label><![CDATA[Electronics &amp; Toys]]></label> 
       <link><![CDATA[?i=1;q=*;q1=Electronics+%26+Toys;sp_cs=UTF-8;sp_staged=1;view=xml;x1=leveli]]></link> 
       <count><![CDATA[200]]></count> 
                         
      </facet-value> 
   
      <facet-value> 
           
              <label><![CDATA[Gifts &amp; Home]]></label> 
       <link><![CDATA[?i=1;q=*;q1=Gifts+%26+Home;sp_cs=UTF-8;sp_staged=1;view=xml;x1=leveli]]></link> 
       <count><![CDATA[156]]></count>

      </facet-value> 
   
      <facet-value> 
           
              <label><![CDATA[Jewelry &amp; Accessories]]></label> 
       <link><![CDATA[?i=1;q=*;q1=Jewelry+%26+Accessories;sp_cs=UTF-8;sp_staged=1;view=xml;x1=leveli]]></link> 
       <count><![CDATA[182]]></count> 
                         
      </facet-value> 
   
      </facet-item> 
  
    </facets> 
 
    <results> 
        <result-set> 
            <name><![CDATA[default]]></name> 
               
         <result> 
                    <field name="index"><![CDATA[1]]></field> 
      <field name="brand"><![CDATA[Citizens]]></field> 
      <field name="price"><![CDATA[149]]></field> 
      <field name="foundIn"><![CDATA[Womens,  
            Apparel,  
          Denim]]></field> 
         </result>   
        
         <result> 
 
                    <field name="index"><![CDATA[2]]></field> 
      <field name="brand"><![CDATA[One For All]]></field> 
      <field name="price"><![CDATA[145]]></field> 
      <field name="foundIn"><![CDATA[Womens,  
            Apparel,  
          Denim]]></field> 
         </result>   
        
         <result> 
                    <field name="index"><![CDATA[3]]></field> 
      <field name="brand"><![CDATA[Citizens]]></field> 
      <field name="price"><![CDATA[208]]></field> 
 
      <field name="foundIn"><![CDATA[Womens,  
            Apparel,  
          Denim]]></field> 
         </result>   
        
         <result> 
                    <field name="index"><![CDATA[4]]></field> 
      <field name="brand"><![CDATA[Vera Watson]]></field> 
      <field name="price"><![CDATA[850]]></field> 
      <field name="foundIn"><![CDATA[Womens,  
            Dresses,  
          Day]]></field> 
         </result>   
        
         <result> 
                    <field name="index"><![CDATA[5]]></field> 
 
      <field name="brand"><![CDATA[Ray Laredo]]></field> 
      <field name="price"><![CDATA[195]]></field> 
      <field name="foundIn"><![CDATA[Children &amp; Toys,  
            Apparel,  
          Boys Toddler (2T-4T)]]></field> 
         </result>   
        
         <result> 
                    <field name="index"><![CDATA[6]]></field> 
      <field name="brand"><![CDATA[Ray Laredo]]></field> 
      <field name="price"><![CDATA[80]]></field> 
      <field name="foundIn"><![CDATA[Children &amp; Toys,  
            Apparel,  
          Boys Toddler (2T-4T)]]></field> 
 
         </result>   
        
         <result> 
                    <field name="index"><![CDATA[7]]></field> 
      <field name="brand"><![CDATA[Petrol]]></field> 
      <field name="price"><![CDATA[85]]></field> 
      <field name="foundIn"><![CDATA[Children &amp; Toys,  
            Apparel,  
          Boys Toddler (2T-4T)]]></field> 
         </result>   
        
         <result> 
                    <field name="index"><![CDATA[8]]></field> 
      <field name="brand"><![CDATA[Woolberry]]></field> 
 
      <field name="price"><![CDATA[280]]></field> 
      <field name="foundIn"><![CDATA[Children &amp; Toys,  
            Apparel,  
          Boys Toddler (2T-4T)]]></field> 
         </result>   
        
         <result> 
                    <field name="index"><![CDATA[9]]></field> 
      <field name="brand"><![CDATA[Petrol]]></field> 
      <field name="price"><![CDATA[149]]></field> 
      <field name="foundIn"><![CDATA[Children &amp; Toys,  
            Apparel,  
          Boys Toddler (2T-4T)]]></field> 
         </result>   
        
         <result> 
 
                    <field name="index"><![CDATA[10]]></field> 
      <field name="brand"><![CDATA[Ray Laredo]]></field> 
      <field name="price"><![CDATA[55]]></field> 
      <field name="foundIn"><![CDATA[Children &amp; Toys,  
            Apparel,  
          Boys Toddler (2T-4T)]]></field> 
         </result>   
        
         <result> 
                    <field name="index"><![CDATA[11]]></field> 
      <field name="brand"><![CDATA[Petrol]]></field> 
      <field name="price"><![CDATA[45]]></field> 
 
      <field name="foundIn"><![CDATA[Children &amp; Toys,  
            Apparel,  
          Boys Toddler (2T-4T)]]></field> 
         </result>   
        
         <result> 
                    <field name="index"><![CDATA[12]]></field> 
      <field name="brand"><![CDATA[Ray Laredo]]></field> 
      <field name="price"><![CDATA[47]]></field> 
      <field name="foundIn"><![CDATA[Children &amp; Toys,  
            Apparel,  
          Boys Toddler (2T-4T)]]></field> 
         </result>   
      
        </result-set>   
    </results>

    <banners> 
         
            <banner> 
                <area><![CDATA[top]]></area> 
                <content><![CDATA[<div style="color:#70A100">We have custom shipping</div>]]></content> 
            </banner>

    </banners> 
 
    <zones> 
        <zone> 
 
            <name><![CDATA[brand-facet]]></name> 
            <display>1</display> 
        </zone> 
    </zones> 
 
    <search-form> 
        <include-tnt-mbox>1</include-tnt-mbox> 
        <autocomplete> 
 
            <enabled>1</enabled> 
            <css><![CDATA[<link rel="stylesheet" type="text/css" href="https://content.t1.atomz.com/sp10043554/stage/autocomplete_styles.css?sp_js_param=2" /> 
]]></css> 
            <form-content><![CDATA[<div id="autocomplete"></div> 
<input type="hidden" name="sp_staged" id="sp_staged" value="1" /> 
]]></form-content> 
            <javascript><![CDATA[<script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/yui/2.6.0/build/utilities/utilities.js"></script> 
<script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/yui/2.6.0/build/datasource/datasource-min.js"></script> 
<script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/yui/2.6.0/build/autocomplete/autocomplete-min.js"></script> 
<script type="text/javascript" src="https://content.t1.atomz.com/sp10043554/stage/autocomplete_data.js?sp_js_param=3"></script>]]></javascript> 
        </autocomplete> 
    </search-form> 
 
</customer-results> 
```

## Ejemplo de plantilla de presentación {#section_AD42571DFB88491AA7F0FDF0929EBE97}

A continuación se muestra un ejemplo de plantilla de presentación que se utiliza para producir el ejemplo de salida anterior.

```xml
<?xml version="1.0" encoding="utf-8" standalone="yes" ?> 
<customer-results> 
    <query> 
        <user-query><![CDATA[<guided-query-param gsname="q" />]]></user-query> 
 <lower-results><![CDATA[<guided-results-lower>]]></lower-results> 
 <upper-results><![CDATA[<guided-results-upper>]]></upper-results> 
 <total-results><![CDATA[<guided-results-total>]]></total-results> 
    </query> 
 
    <custom-fields> 
        <custom-field name="seo-search-title"><![CDATA[Geometrixx Search Results]]></custom-field> 
        <custom-field name="seo-search-keywords"><![CDATA[<guided-general-field gsname="default" field="seo_search_keywords"/>]]></custom-field> 
    </custom-fields> 
 
    <menus> 
 
        <menu> 
           <name>sort</name> 
     <guided-menu gsname="sort"> 
         <guided-if-menu-item-selected> 
             <item selected="true"> 
          <label><![CDATA[<guided-menu-item-label />]]></label> 
          <value><![CDATA[<guided-menu-item-value />]]></value> 
          <link><![CDATA[ ]]></link> 
             </item> 
        <guided-else-menu-item-selected> 
             <item> 
          <label><![CDATA[<guided-menu-item-label />]]></label> 
          <value><![CDATA[<guided-menu-item-value />]]></value> 
          <link><![CDATA[<guided-menu-item-path />]]></link>     
             </item> 
        </guided-if-menu-item-selected> 
    </guided-menu> 
        </menu> 
        <menu> 
            <name><![CDATA[ss_head_nav]]></name> 
            <guided-menu gsname="ss_head_nav"> 
                <guided-if-menu-item-selected> 
                    <item selected="true"> 
                    <label><![CDATA[<guided-menu-item-label />]]></label> 
      <value><![CDATA[<guided-menu-item-value />]]></value> 
      <link><![CDATA[<guided-menu-item-path />]]></link> 
                <guided-else-menu-item-selected> 
                    <label><![CDATA[<guided-menu-item-label />]]></label> 
      <value><![CDATA[<guided-menu-item-value />]]></value> 
      <link><![CDATA[<guided-menu-item-path />]]></link> 
                </guided-if-menu-item-selected> 
            </guided-menu>  
        </menu> 
    </menus> 
 
    <breadcrumbs> 
  <breadcrumb> 
            <name><![CDATA[default]]></name> 
      <guided-breadcrumb gsname="default"> 
  <breadcrumb-item> 
    <link><![CDATA[<guided-breadcrumb-path gsname="goto">]]></link> 
    <value><![CDATA[<guided-breadcrumb-value />]]></value> 
                        <label><![CDATA[<guided-breadcrumb-label>]]></label> 
   </breadcrumb-item> 
         </guided-breadcrumb> 
   </breadcrumb> 
    </breadcrumbs> 
 
    <suggestions> 
        <auto-searched><guided-if-suggestion-autosearch>1<guided-else-suggestion-autosearch>0</guided-if-suggestion-autosearch></auto-searched> 
        <guided-if-suggestion-autosearch><orig-query><![CDATA[<guided-suggestion-original-query/>]]></orig-query></guided-if-suggestion-autosearch> 
        <suggestions-low-results><guided-if-suggestion-low-results>1<guided-else-suggestion-low-results>0</guided-if-suggestion-low-results></suggestions-low-results> 
        <guided-suggestions> 
     <suggestion-item> 
         <link><![CDATA[<guided-suggestion-path />]]></link> 
  <word><![CDATA[<guided-suggestion />]]></word> 
     </suggestion-item> 
 </guided-suggestions> 
    </suggestions> 
 
    <pagination> 
        <total-pages><![CDATA[<guided-page-total />]]></total-pages> 
        <pages> 
     <page position="first"><![CDATA[<guided-page-path gsname="first" />]]></page> 
     <page position="last"><![CDATA[<guided-page-path gsname="last" />]]></page> 
     <guided-if-page-prev><page position="prev"><![CDATA[<guided-page-path gsname="prev" />]]></page></guided-if-page-prev> 
     <guided-if-page-next><page position="next"><![CDATA[<guided-page-path gsname="next" />]]></page></guided-if-page-next> 
     <guided-if-page-viewall><page position="viewall"><![CDATA[<guided-page-path gsname="viewall" />]]></page></guided-if-page-viewall> 
     <guided-if-page-viewpages><page position="viewall"><![CDATA[<guided-page-path gsname="viewpages" />]]></page></guided-if-page-viewpages> 
 
     <guided-pages> 
                <guided-if-page-selected><page position="<guided-page-number />" selected="true"><![CDATA[<guided-page-path />]]></page> 
  <guided-else-page-selected><page position="<guided-page-number />"><![CDATA[<guided-page-path />]]></page> 
  </guided-if-page-selected> 
     </guided-pages> 
        </pages> 
    </pagination> 
 
    <facets>  
        <guided-facet gsname="leveli"> 
     <facet-item> 
         <facet-title><![CDATA[Department]]></facet-title> 
                <selected><guided-if-facet-selected>1<guided-else-facet-selected>0</guided-if-facet-selected></selected> 
                <guided-if-facet-selected><undo-link><![CDATA[<guided-facet-undo-path gsname="leveli">]]></undo-link></guided-if-facet-selected> 
  <guided-facet-values> 
      <facet-value> 
          <guided-if-facet-value-selected><selected><![CDATA[true]]></selected></guided-if-facet-value-selected> 
              <label><![CDATA[<guided-facet-value>]]></label> 
       <link><![CDATA[<guided-facet-value-path />]]></link> 
       <count><![CDATA[<guided-facet-count>]]></count> 
                        <guided-if-facet-value-selected><undolink><![CDATA[<guided-facet-value-undo-path />]]></undolink></guided-if-facet-value-selected> 
      </facet-value> 
  </guided-facet-values> 
      </facet-item> 
 </guided-facet> 
    </facets> 
 
    <results> 
        <result-set> 
            <name><![CDATA[default]]></name> 
            <guided-results gsname="default">   
         <result> 
                    <field name="index"><![CDATA[<guided-result-index />]]></field> 
      <field name="brand"><![CDATA[<guided-result-field gsname="brand" />]]></field> 
      <field name="price"><![CDATA[<guided-result-field gsname="price" />]]></field> 
      <field name="foundIn"><![CDATA[<guided-if-result-field gsname="leveli"><!--tmpl_var name='leveli'-->, </guided-if-result-field> 
            <guided-if-result-field gsname="levelii"><!--tmpl_var name='levelii'-->, </guided-if-result-field> 
          <guided-if-result-field gsname="leveliii"><!--tmpl_var name='leveliii'--></guided-if-result-field>]]></field> 
         </result>   
     </guided-results> 
        </result-set>   
    </results> 
 
    <guided-if-recent-searches> 
    <recent-searches> 
        <clear-link><guided-recent-searches-clear-path/></clear-link> 
        <guided-recent-searches> 
            <recent-search> 
                <link><guided-recent-searches-path></link> 
                <label><guided-recent-searches-value></label> 
            <recent-search> 
        </guided-recent-searches> 
    </recent-searches> 
    </guided-if-recent-searches> 
 
    <banners> 
        <guided-if-banner-set gsname="top"> 
            <banner> 
                <area><![CDATA[top]]></area> 
                <content><![CDATA[<guided-banner gsname="top">]]></content> 
            </banner> 
        </guided-if-banner-set> 
        <guided-if-banner-set gsname="bottom"> 
            <banner> 
                <area><![CDATA[bottom]]></area> 
                <content><![CDATA[<guided-banner gsname="bottom">]]></content> 
            </banner> 
        </guided-if-banner-set> 
    </banners> 
 
    <zones> 
        <zone> 
            <name><![CDATA[brand-facet]]></name> 
            <display><guided-if-zone gsname="brand-facet">1<guided-else-zone>0</guided-if-zone></display> 
        </zone> 
    </zones> 
 
    <search-form> 
        <include-tnt-mbox><guided-if-tnt-business-rules>1<guided-else-tnt-business-rules>0</guided-if-tnt-business-rules></include-tnt-mbox> 
        <autocomplete> 
            <enabled><guided-if-autocomplete>1<guided-else-autocomplete>0</guided-if-autocomplete></enabled> 
            <css><![CDATA[<guided-ac-css/>]]></css> 
            <form-content><![CDATA[<guided-ac-form-content/>]]></form-content> 
            <javascript><![CDATA[<guided-ac-javascript/>]]></javascript> 
        </autocomplete> 
    </search-form> 
 
</customer-results> 
```
