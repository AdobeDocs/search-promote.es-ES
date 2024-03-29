---
description: Puede utilizar metaetiquetas de SEO (Optimización del motor de búsqueda) para ayudar a adaptar ciertos elementos de las páginas y mejorar así la ubicación del motor de búsqueda.
solution: Target
subtopic: SEO
title: Acerca de SEO
topic-legacy: Settings,Site search and merchandising
uuid: 5c5d64f5-fe79-4489-85c6-399d1437f2c4
exl-id: 0045455b-cb86-4820-82fa-753475ab6ca6
translation-type: tm+mt
source-git-commit: 7559f5f7437d46e3510d4659772308666425ec96
workflow-type: tm+mt
source-wordcount: '1201'
ht-degree: 1%

---

# Acerca de SEO{#about-seo}

Puede utilizar metaetiquetas de SEO (Optimización del motor de búsqueda) para ayudar a adaptar ciertos elementos de las páginas y mejorar así la ubicación del motor de búsqueda.

## Uso de SEO {#concept_C58BFCE720824A2B9B5F5E613CFD4C0B}

Puede definir cada elemento de este tipo como un texto inicial seguido de una lista de palabras. La lista de palabras puede incluir lo siguiente:

* Frases de búsqueda populares en el sitio durante un período de tiempo seleccionado (por ejemplo, &quot;último mes&quot;), sujetas a las exclusiones personalizadas.
* Conjuntos de valores de uno o más campos de la búsqueda de la que provino la página.
* Palabras y frases estáticas que defina en una lista

Los elementos de página más comunes que las personas utilizan para SEO son el título de la página y las metaetiquetas &quot;palabras clave&quot; y &quot;descripción&quot;. Puede definir la configuración de estos tres elementos de página. Además, puede definir estas tres configuraciones para cada uno de los tres tipos de páginas: Páginas de resultados de búsqueda, Páginas de exploración y Páginas de detalles de elementos.

Al guardar o previsualizar la configuración de SEO, se generan pequeños segmentos de plantillas de búsqueda (transporte) que puede incluir en las demás plantillas de búsqueda con la etiqueta de plantilla `<search-include>`. A modo de ejemplo, puede incluir el campo Título de las páginas de resultados de búsqueda en otra plantilla con lo siguiente:

```
<search-include file="seo/seo_search_title.tpl" />
```

Los nueve valores posibles para el atributo `file` de la etiqueta `<search-include>` para los campos SEO son los siguientes:

* `seo/seo_search_title.tpl`
* `seo/seo_search_description.tpl`
* `seo/seo_search_keywords.tpl`
* `seo/seo_browse_title.tpl`
* `seo/seo_browse_description.tpl`
* `seo/seo_browse_keywords.tpl`
* `seo/seo_item_title.tpl`
* `seo/seo_item_description.tpl`
* `seo/seo_item_keywords.tpl`

Para utilizar campos SEO en una plantilla de presentación, debe realizar varios pasos.

En primer lugar, utilice `<search-include>` como se describe anteriormente para incluir los campos deseados en una plantilla de búsqueda XML, dentro de un elemento `<general>` .

A continuación, rodee cada etiqueta `<search-include>` con etiquetas `<general-field>` y `</general-field>` , dando los nombres de campo en el atributo `name` como en el siguiente ejemplo:

```
<general> 
    <general-field name="seo_search_title"><search-include file="seo/seo_search_title.tpl" /></general-field> 
    <general-field name="seo_search_description"><search-include file="seo/seo_search_description.tpl" /></general-field> 
    <general-field name="seo_search_keywords"><search-include file="seo/seo_search_keywords.tpl" /></general-field> 
</general>
```

A continuación, en la plantilla de presentación, puede utilizar etiquetas `<guided-general-field>` para insertar los campos con nombre dondequiera que necesite, como en el siguiente ejemplo:

```
<title><guided-general-field gsname="default" field="seo_search_title"></title> 
<meta name="description" content="<guided-general-field gsname="default" field="seo_search_description">"/> 
<meta name="keywords" content="<guided-general-field gsname="default" field="seo_search_keywords">"/>
```

Observe el uso del atributo `gsname` para especificar, en este caso, la búsqueda &quot;predeterminada&quot;.

En total, puede definir nueve campos SEO diferentes que puede utilizar en sus páginas web. Incluyen lo siguiente:

* Páginas de resultados de búsqueda: título, descripción y palabras clave
* Examinar páginas: título, descripción y palabras clave
* Páginas de detalles del elemento: título, descripción y palabras clave

Los nueve campos se definen con una configuración similar. De hecho, los nombres de los nueve campos, como el campo &quot;título&quot; en &quot;Páginas de resultados de búsqueda&quot;, solo son sugerencias sobre cómo utilizarlos. Puede utilizar cualquiera de los campos en cualquier contexto en las plantillas de presentación y las plantillas de búsqueda.

Consulte también [Acerca de las plantillas](../c-about-design-menu/c-about-templates.md#concept_06EB481B14864E18A8AE2BCD1D6EF0B5).

Consulte también [Presentation template tags](../c-appendices/c-templates.md#reference_F1BBF616BCEC4AD7B2548ECD3CA74C64).

Consulte también [Buscar etiquetas de plantilla](../c-appendices/c-templates.md#reference_F7AA3FF602314E42842BBC740D2CA1A4).

Consulte también [Configuración de una lista de palabras de resultados de búsqueda](../c-about-settings-menu/c-about-seo.md#task_A459A3734EC04042BA52C81184251DD4).

## Configuración de una lista de palabras de resultados de búsqueda {#task_A459A3734EC04042BA52C81184251DD4}

Puede configurar la lista de palabras y frases de resultados de búsqueda que se incluyen en títulos de páginas, descripciones y palabras clave.

**Para configurar una lista de palabras de resultados de búsqueda**

1. En el menú del producto, haga clic en **[!UICONTROL Settings]** > **[!UICONTROL SEO]** > **[!UICONTROL Search Result Pages]**.
1. En la página [!DNL SEO Browse Pages Word List], en los grupos [!DNL Title], [!DNL Description] y [!DNL Keywords] respectivos, configure las opciones que desee.

   <table> 
    <thead> 
      <tr> 
      <th colname="col1" class="entry"> <p>Opción </p> </th> 
      <th colname="col2" class="entry"> <p>Descripción </p> </th> 
      </tr> 
    </thead>
    <tbody> 
      <tr> 
      <td colname="col1"> <p>Título | Descripción | Palabras clave iniciadas con </p> </td> 
      <td colname="col2"> <p>Texto que desea mostrar delante de la lista de palabras. </p> <p>Por ejemplo, es posible que desee que la lista Palabras clave comience por la palabra "Marcas". </p> </td> 
      </tr> 
      <tr> 
      <td colname="col1"> <p>Incluir búsquedas populares durante </p> </td> 
      <td colname="col2"> <p>Período de tiempo para el cual se incluyen las búsquedas. </p> </td> 
      </tr> 
      <tr> 
      <td colname="col1"> <p>Recuento máximo de búsqueda </p> </td> 
      <td colname="col2"> <p>El número máximo de búsquedas que se incluyen. </p> </td> 
      </tr> 
      <tr> 
      <td colname="col1"> <p>Incluir valores de campo </p> </td> 
      <td colname="col2"> <p>Los valores de campo que se incluyen en la lista de palabras de resultados. </p> </td> 
      </tr> 
      <tr> 
      <td colname="col1"> <p>Agregar estas palabras y frases </p> </td> 
      <td colname="col2"> <p>Enumere las palabras que desee agregar al título de la página de resultados de la búsqueda, al título de la página de exploración o al título de la página de detalles del elemento. </p> <p>Haga clic en <b>Editar</b> para agregar palabras a la lista. </p> <p>Puede agregar una palabra o frase por línea. Cuando haya terminado, haga clic en <b>Guardar cambios</b> y cierre la página para volver a la página principal de SEO. </p> </td> 
      </tr> 
      <tr> 
      <td colname="col1"> <p>Eliminar estas palabras y frases (excepto los valores de campo incluidos) </p> </td> 
      <td colname="col2"> <p>Enumere las palabras que desea eliminar del título de la página de resultados de búsqueda, del título de la página de exploración o del título de la página de detalles del elemento. </p> <p>Haga clic en <b>Editar</b> para agregar palabras a la lista de eliminación. </p> <p>Puede agregar una palabra o frase por línea. Cuando haya terminado, haga clic en <b>Guardar cambios</b> y cierre la página para volver a la página principal de SEO. </p> </td> 
      </tr> 
    </tbody> 
    </table>

1. (Opcional) Haga clic en **Vista previa de salida de muestra** para obtener una vista previa de los valores resultantes para los campos SEO que haya establecido.

   Consulte [Vista previa de las metaetiquetas de SEO que configuró](../c-about-settings-menu/c-about-seo.md#task_3F97949E193C4F92A104AD117FE49621).
1. Haga clic en **Guardar cambios**.
1. (Opcional) Reconstruya el índice del sitio provisional si desea obtener una vista previa de los resultados.

   Consulte [Configuración de un índice incremental de un sitio web provisional](../c-about-index-menu/c-about-incremental-index.md#task_46A367B0786C4C90BFFA5D3F95FD86C0).
1. (Opcional) En la página [!DNL SEO Search Results Word List], realice una de las acciones siguientes:

   * Haga clic en **[!UICONTROL History]** para revertir cualquier cambio que haya realizado.

      Consulte [Uso de la opción Historial](../t-using-the-history-option.md#task_70DD3F87A67242BBBD2CB27156F43002).

   * Haga clic **[!UICONTROL Live]**.

      Consulte [Visualización de la configuración de lanzamiento](../c-about-staging.md#task_401A0EBDB5DB4D4CA933CBA7BECDC10F).

   * Haga clic **[!UICONTROL Push Live]**.

      Consulte [Inserción de la configuración del escenario en directo](../c-about-staging.md#task_44306783B4C0408AAA58B471DAF2D9A4).

## Configuración de la lista de palabras de las páginas de navegación {#task_D7A1D765A92A4D6C94E672B3A86ECB5A}

Puede configurar la lista de páginas de navegación con las palabras y frases incluidas en títulos de páginas, descripciones y palabras clave.

**Para configurar una lista de palabras de las páginas de navegación**

1. En el menú del producto, haga clic en **[!UICONTROL Settings]** > **[!UICONTROL SEO]** > **[!UICONTROL Browse Pages]**.
1. En la página [!DNL SEO Browse Pages Word List], en los grupos [!DNL Title], [!DNL Description] y [!DNL Keywords] respectivos, configure las opciones que desee.

   Consulte la tabla de opciones en [Configuración de una lista de palabras de resultados de búsqueda](../c-about-settings-menu/c-about-seo.md#task_A459A3734EC04042BA52C81184251DD4).
1. (Opcional) Haga clic en **Vista previa de salida de muestra** para obtener una vista previa de los valores resultantes para los campos SEO que haya establecido.

   Consulte [Vista previa de las metaetiquetas de SEO que configuró](../c-about-settings-menu/c-about-seo.md#task_3F97949E193C4F92A104AD117FE49621).
1. Haga clic en **Guardar cambios**.
1. (Opcional) Reconstruya el índice del sitio provisional si desea obtener una vista previa de los resultados.

   Consulte [Configuración de un índice incremental de un sitio web provisional](../c-about-index-menu/c-about-incremental-index.md#task_46A367B0786C4C90BFFA5D3F95FD86C0).
1. (Opcional) En la página [!DNL SEO Browse Pages Word List], realice una de las acciones siguientes:

   * Haga clic en **[!UICONTROL History]** para revertir cualquier cambio que haya realizado.

      Consulte [Uso de la opción Historial](../t-using-the-history-option.md#task_70DD3F87A67242BBBD2CB27156F43002).

   * Haga clic **[!UICONTROL Live]**.

      Consulte [Visualización de la configuración de lanzamiento](../c-about-staging.md#task_401A0EBDB5DB4D4CA933CBA7BECDC10F).

   * Haga clic **[!UICONTROL Push Live]**.

      Consulte [Inserción de la configuración del escenario en directo](../c-about-staging.md#task_44306783B4C0408AAA58B471DAF2D9A4).

## Configuración de la lista de palabras de detalles de un elemento {#task_761538C519B34164BE189F13C39B2495}

Puede configurar la lista de palabras y frases detalladas que se incluyen en títulos de páginas, descripciones y palabras clave.

**Para configurar una lista de palabras de detalles de un elemento**

1. En el menú del producto, haga clic en **[!UICONTROL Settings]** > **[!UICONTROL SEO]** > **[!UICONTROL Item Detail Pages]**.
1. En la página [!DNL SEO Item Detail Word List], en los grupos [!DNL Title], [!DNL Description] y [!DNL Keywords] respectivos, configure las opciones que desee.

   Consulte la tabla de opciones en [Configuración de una lista de palabras de resultados de búsqueda](../c-about-settings-menu/c-about-seo.md#task_A459A3734EC04042BA52C81184251DD4).
1. (Opcional) Haga clic en **Vista previa de salida de muestra** para obtener una vista previa de los valores resultantes para los campos SEO que haya establecido.

   Consulte [Vista previa de las metaetiquetas de SEO que configuró](../c-about-settings-menu/c-about-seo.md#task_3F97949E193C4F92A104AD117FE49621).
1. Haga clic en **Guardar cambios**.
1. (Opcional) Reconstruya el índice del sitio provisional si desea obtener una vista previa de los resultados.

   Consulte [Configuración de un índice incremental de un sitio web provisional](../c-about-index-menu/c-about-incremental-index.md#task_46A367B0786C4C90BFFA5D3F95FD86C0).
1. (Opcional) En la página [!DNL SEO Item Detail Word List], realice una de las acciones siguientes:

   * Haga clic en **[!UICONTROL History]** para revertir cualquier cambio que haya realizado.

      Consulte [Uso de la opción Historial](../t-using-the-history-option.md#task_70DD3F87A67242BBBD2CB27156F43002).

   * Haga clic **[!UICONTROL Live]**.

      Consulte [Visualización de la configuración de lanzamiento](../c-about-staging.md#task_401A0EBDB5DB4D4CA933CBA7BECDC10F).

   * Haga clic **[!UICONTROL Push Live]**.

      Consulte [Inserción de la configuración del escenario en directo](../c-about-staging.md#task_44306783B4C0408AAA58B471DAF2D9A4).

## Vista previa de las metaetiquetas de SEO que configuró {#task_3F97949E193C4F92A104AD117FE49621}

Puede obtener una vista previa de los valores resultantes para los campos de SEO que haya establecido para ver qué se inserta donde los utiliza.

Los campos SEO pueden contener valores de campo incluidos, por lo que los resultados de la búsqueda pueden depender de la consulta de búsqueda que especifique.

**Para obtener una vista previa de las metaetiquetas de SEO que configuró**

1. En el menú del producto, haga clic en **[!UICONTROL Settings]** > **[!UICONTROL SEO]** > **[!UICONTROL Search Result Pages]**.
1. En la página SEO, haga clic en **Vista previa de salida de muestra**.
1. En la página [!DNL SEO Preview], en el campo [!DNL Enter sample query], escriba el término de consulta en el que desea buscar.
1. Haga clic en **Buscar**.

   Los campos Título, Descripción y Palabras clave resultantes muestran el contenido que se inserta en una página en función de la consulta de búsqueda que acaba de ingresar.
1. Haga clic en **Cerrar** para volver a la página principal de SEO.
