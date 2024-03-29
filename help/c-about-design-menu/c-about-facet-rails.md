---
description: Utilice Barra de faceta para reordenar grupos de facetas en una página web.
solution: Target
subtopic: Navigation
title: Acerca de Barra de faceta
topic-legacy: Design,Site search and merchandising
uuid: 6da2bd67-8c20-4955-9836-bc8ba88546c5
exl-id: 389b2f5e-c1aa-48d7-ab3e-c8a1d1e4ecb4
translation-type: tm+mt
source-git-commit: 7559f5f7437d46e3510d4659772308666425ec96
workflow-type: tm+mt
source-wordcount: '776'
ht-degree: 1%

---

# Acerca de Barra de faceta{#about-facet-rail}

Utilice Barra de faceta para reordenar grupos de facetas en una página web.

## Uso de Barra de faceta {#concept_1FDC8BCDFFC84A0889DA670F63D5F6DB}

Una faceta es una propiedad o característica, es una forma de categorizar generalmente los resultados de búsqueda. Por ejemplo, fabricante, precio y color podrían considerarse un grupo de facetas. Cada faceta puede tener varias restricciones o valores. Por ejemplo, si tiene el color como faceta, los &quot;valores de faceta&quot; pueden ser rojo, naranja, amarillo, verde, azul, índigo y violeta.

Consulte [Acerca de las facetas](../c-about-design-menu/c-about-facets.md#concept_FA912B3B41EE493DB2F492D188457FF5).

Utilice Barra de faceta para reordenar estos grupos de facetas en una página web. Por ejemplo, supongamos que tiene una sección de resultados de búsqueda en la parte izquierda de una página web. La sección mostraba, de arriba abajo, una faceta Categoría, una faceta Marca, una faceta Precio y una faceta Más popular. Con un carril de facetas, puede, por ejemplo, hacer que aparezca la faceta Más popular encima o debajo de la faceta Categoría .

El grupo de facetas que desea reordenar juntas pertenece a una etiqueta de raíl de faceta. Una faceta solo puede pertenecer a un carril de faceta. El carril de facetas es una etiqueta de plantilla de Presentación que incluye una sola representación de una faceta. Todas las facetas que pertenecen a este carril comparten esa representación de faceta única.

Consulte [Acerca de las plantillas](../c-about-design-menu/c-about-templates.md#concept_06EB481B14864E18A8AE2BCD1D6EF0B5) y la faceta en [Etiquetas de plantilla de presentación](../c-appendices/c-templates.md#reference_F1BBF616BCEC4AD7B2548ECD3CA74C64).

## Configuración de un carril de faceta {#task_561A8FF1CAD1402B9DD33E276BBC6A0E}

Puede añadir un carril de facetas para personalizar la capa de presentación. Los carriles de faceta proporcionan a los clientes una Búsqueda guiada que les permite explorar en profundidad los resultados de búsqueda en función del orden de las facetas en la página web.

<!-- 

t_configuring_facet_rail.xml

-->

Los cambios que realice en los carriles de faceta se pueden revertir con la función Historial.

**Configuración de un carril de faceta**

1. Antes de configurar un carril de facetas, asegúrese de que ya ha agregado una faceta y, como parte de esa tarea, especificó un nombre de raíl de faceta.

   Consulte [Adición de una nueva faceta](../c-about-design-menu/c-about-facets.md#task_FC07BFFA62CA4B718D6CBF4F2855C89B).
1. En el menú del producto, haga clic en **[!UICONTROL Design]** > **[!UICONTROL Navigation]** > **[!UICONTROL Facet Rail.]**
1. En la página [!DNL Facet Rail], seleccione las facetas que desee incluir en el carril de facetas y, a continuación, establezca la opción **[!UICONTROL Sort Facets Method]** en la lista desplegable.

   <!-- 
   r_facet_rail_options.xml
   -->

   | Función/Opción | Descripción |
   |--- |--- |
   | Barra de faceta Nombre | Identifica el nombre del carril de facetas.  El nombre del carril de facetas se crea en el momento de agregar la faceta.  Consulte [Adición de una nueva faceta](../c-about-design-menu/c-about-facets.md#task_FC07BFFA62CA4B718D6CBF4F2855C89B) |
   | Facetas incluidas | Una lista de facetas posibles que puede seleccionar para agregarlas al carril de facetas.  Si decide ordenar las facetas mediante `Custom`, el orden en que seleccione las facetas aquí determina el orden en que aparecen en el cuadro de texto `Custom Facet Order`. |
   | Método Sort Facets | Elija una de las tres opciones siguientes en la lista desplegable:<ul><li>`Alpha` Las facetas se ordenan en orden alfabético con respecto a sus nombres, incluidos los caracteres de puntuación.</li><li>`Alpha (not case sensitive)` Las facetas se ordenan en orden alfabético con respecto a sus nombres, ignorando las mayúsculas y minúsculas de los caracteres alfabéticos, incluyendo caracteres de puntuación. </li><li>`Alpha (alphanumeric only)` Las facetas se ordenan en orden alfabético con respecto a sus nombres, ignorando los caracteres de puntuación. </li><li>`Alpha (not case sensitive, alphanumeric only)` Las facetas se ordenan en orden alfabético con respecto a sus nombres, ignorando las mayúsculas y minúsculas de los caracteres alfabéticos e ignorando los caracteres de puntuación. </li><li>`Count` Las facetas se ordenan según el recuento. </li><li>`Custom` Abre el cuadro de  `Custom Facet Order` texto que permite definir el orden de las facetas introduciendo el nombre exacto de cada faceta. Las etiquetas de faceta que se queden afuera se eliminan de la lista `Custom Facet Order`.</li></ul> |
   | Orden de faceta personalizado | Esta opción solo está disponible si ha seleccionado `Custom` en la lista desplegable `Sort Facets Method`.  Permite enumerar nombres de facetas, ya sea uno por línea o todos en una línea y separados por coma. Si se definen las etiquetas de facetas, estas se muestran en la lista `Facets Included`, entre paréntesis.  No incluya etiquetas de facetas en el cuadro de texto `Custom Facet Order`.  Cuando selecciona o anula la selección de facetas en la lista `Facets Included`, el cuadro de texto `Custom Facet Order` se actualiza automáticamente. |

1. Haga clic **[!UICONTROL Save Changes]**.
1. (Opcional) En la página [!DNL Facet Rail], realice una de las siguientes acciones:

   * Haga clic en **[!UICONTROL History]** para revertir cualquier cambio que haya realizado.

      Consulte [Uso de la opción Historial](../t-using-the-history-option.md#task_70DD3F87A67242BBBD2CB27156F43002).

   * Haga clic **[!UICONTROL Live]**.

      Consulte [Visualización de la configuración de lanzamiento](../c-about-staging.md#task_401A0EBDB5DB4D4CA933CBA7BECDC10F).

   * Haga clic **[!UICONTROL Push Live]**.

      Consulte [Inserción de la configuración del escenario en directo](../c-about-staging.md#task_44306783B4C0408AAA58B471DAF2D9A4).

1. Edite la plantilla Presentación haciendo lo siguiente:

   * Cree una &quot;plantilla de faceta&quot; en la plantilla de presentación.
   * Rodee la &quot;plantilla de faceta&quot; con las etiquetas `<guided-facet-rail>` .

      Consulte Facetas en [Presentation template tags](../c-appendices/c-templates.md#reference_F1BBF616BCEC4AD7B2548ECD3CA74C64).

      Ejemplo:

      ```
      <guided-facet-rail>
        <guided-facet>
          <guided-facet-display-name/>
          ...
          </guided-facet>
        </guided-facet-rail>
      ```

      Estas etiquetas extraen una sección de la plantilla de presentación para convertirla en un patrón repetible para cada faceta del carril de la faceta. Cada faceta que pertenece al carril de facetas utiliza esta sección tallada para evaluar su salida. Solo puede aparecer una etiqueta `<guided-facet-rail>` en la plantilla de presentación final.

      Las siguientes etiquetas no necesitan el atributo `gsname` dentro de `<guided-facet-rail>` porque el valor se determina dinámicamente en el momento de la búsqueda y se sustituye correctamente:

      `<guided-facet>`
      `<guided-facet-display-name>`
      `<guided-facet-total-count>`
      `<guided-facet-undo-link>`
      `<guided-facet-undo-path>`
      `<guided-facet-behavior>`

   * Guarde la plantilla de presentación y colóquela en vivo.

      Consulte [Edición de una presentación o de una plantilla de transporte](../c-about-design-menu/c-about-templates.md#task_800E0E2265C34C028C92FEB5A1243EC3).
