---
description: Puede utilizar la actualización vertical para actualizar rápidamente partes del índice sin tener que procesar grandes cantidades de datos.
seo-description: Puede utilizar la actualización vertical para actualizar rápidamente partes del índice sin tener que procesar grandes cantidades de datos.
seo-title: Acerca de la actualización vertical
solution: Target
subtopic: Vertical Update
title: Acerca de la actualización vertical
topic: Index,Site search and merchandising
uuid: ded09e89-5a52-4e8c-a6f7-3e25b4191183
translation-type: tm+mt
source-git-commit: f21a3f7fe0aeaab517a5ca36da43594873b3e69a

---


# Acerca de la actualización vertical{#about-vertical-update}

Puede utilizar la actualización vertical para actualizar rápidamente partes del índice sin tener que procesar grandes cantidades de datos.

## Uso de actualización vertical {#concept_E65A70C9C2E04804BF24FBE1B3CAD899}

Un índice vertical tarda sólo segundos en funcionar y resulta útil en los sitios Web de comercio electrónico de gran capacidad que pueden tardar muchas horas en indexarse de forma completa o incremental.

Cuando se genera un índice vertical, se muestra la información de estado, como la hora de inicio, el tiempo transcurrido y los errores durante el proceso de indexación.

Puede detener o reiniciar el proceso de indexación vertical en cualquier momento.

Aunque el nuevo índice vertical actualiza el sitio web activo, los clientes pueden seguir buscando en el sitio con el índice actual.

>[!NOTE]
>
>Esta función no está habilitada de forma predeterminada en [!DNL Adobe Search&Promote]. Póngase en contacto con la asistencia técnica para activar la función para su uso.

Las actualizaciones verticales están diseñadas específicamente para utilizarse en cuentas de estilo de comercio electrónico [!DNL Adobe Search&Promote] que utilizan IndexConnector para proporcionar contenido para el índice de búsqueda. El caso de uso típico es aquel en el que el [!DNL Adobe Search&Promote] índice representa un catálogo de productos en el que se puede realizar una búsqueda y existe la necesidad de poder actualizar rápidamente los valores que cambian con frecuencia, como inventario disponible, disponibilidad o precio. Una actualización vertical es algo similar a un índice incremental, con la diferencia de que solo actualiza partes de cada documento, mientras que un índice incremental reemplaza documentos completos con versiones nuevas.

El término &quot;actualización vertical&quot; hace referencia a la noción de que un [!DNL Adobe Search&Promote] índice puede representarse como una tabla en columnas, con cada columna correspondiente a una definición de campo de [!DNL Adobe Search&Promote] metadatos y cada fila correspondiente a un documento. El proceso de actualización vertical reemplaza una o varias columnas sin necesidad de alterar el contenido de las demás columnas.

Cuando la fuente principal de contenido, una fuente IndexConnector, contiene todos los elementos de datos necesarios para crear el índice, la fuente de actualización vertical es un subconjunto de la fuente principal, que utiliza el mismo &quot;esquema&quot; de IndexConnector para definir los elementos de datos, pero que *solo* contiene los elementos de datos que deben actualizarse.

Como mínimo, la fuente Actualización vertical debe contener el elemento &quot;clave principal&quot; (como se identifica con la casilla de verificación Clave **** principal en la configuración de IndexConnector) y al menos un elemento de datos que se debe actualizar.

Por ejemplo, una fuente IndexConnector principal puede tener el aspecto siguiente (pero normalmente con muchos más elementos de datos):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<products>
<product>
  <id><![CDATA[123]]></id>
  <title><![CDATA[Title for product 123]]></title>
  <description><![CDATA[Description for product 123.]]></description>
  <price>3.99</price>
  <link><![CDATA[https://www.somewhere.com/products/123]]></link>
  <image><![CDATA[https://www.somewher.com/images/products/123.jpg]]></image>
  <label><![CDATA[PROD123]]></label>
  <inventory>100</inventory>
  <brand><![CDATA[brand123]]></brand>
  ...
</product>
<product>
...
</product>
</products>
```

Un requisito es poder actualizar rápidamente sólo los valores `<price>` y `<inventory>` , ya que estos valores pueden cambiar rápidamente en el sitio del cliente. Una fuente de actualización vertical podría tener el siguiente aspecto:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<products>
<product>
  <id><![CDATA[123]]></id>
  <price>3.50</price>
  <inventory>90</inventory>
</product>
<product>
...
</product>
</products>
```

Esta información se almacena generalmente en un archivo independiente en el servidor del cliente y la configuración &quot;Ruta de archivo vertical&quot; de IndexConnector apunta a este archivo. El proceso de actualización vertical lee este nuevo contenido y actualiza el [!DNL Adobe Search&Promote] índice existente, actualizando solo los valores para `<price>` y `<inventory>`, en este caso. Las búsquedas posteriores devuelven el contenido recién actualizado.

>[!NOTE]
En este ejemplo, los campos `<price>` y `<inventory>` Metadatos deberán haberse definido con la opción Campo **de actualización** vertical activada.

Consulte también [Acerca del control remoto para indizar](../c-about-index-menu/c-about-remote-control-for-indexing.md#concept_C79B322190E84106A434E5C6D4A4118F) y [agregar un nuevo campo](../c-about-settings-menu/c-about-metadata-menu.md#task_6DF188C0FC7F4831A4444CA9AFA615E5)de etiqueta meta.

## Configuración de una actualización vertical de un sitio web escalonado {#task_46A367B0786C4C90BFFA5D3F95FD86C0}

Puede configurar las fuentes del conector de índice que desee incluir en la actualización vertical especificando las direcciones URL.

**Para configurar una actualización vertical de un sitio web escalonado**

1. En el menú de producto, haga clic en **[!UICONTROL Index]** > **[!UICONTROL Vertical Update]** > **[!UICONTROL Configuration]**.
1. En el campo Actualizar direcciones URL de la **[!UICONTROL Vertical Update Configuration]** página, especifique las direcciones URL de las páginas que desea indexar.

   El robot de búsqueda solo actualiza los documentos identificados en los orígenes especificados del conector de índice.

   Este campo debe contener direcciones URL del conector de índice solo como en el ejemplo siguiente:

   `index:product_catalog`.
1. Haga clic **[!UICONTROL Save Changes]**.
1. (Opcional) Realice una de las siguientes acciones:

   * Haga clic en **[!UICONTROL History]** para revertir cualquier cambio que haya realizado.

      Consulte [Uso de la opción](../t-using-the-history-option.md#task_70DD3F87A67242BBBD2CB27156F43002)Historial.

   * Haga clic **[!UICONTROL Live]**.

      Consulte [Visualización de la configuración](../c-about-staging.md#task_401A0EBDB5DB4D4CA933CBA7BECDC10F)de lanzamiento.

   * Haga clic **[!UICONTROL Push Live]**.

      Consulte [Inserción de la configuración del escenario en directo](../c-about-staging.md#task_44306783B4C0408AAA58B471DAF2D9A4).

## Visualización del registro de actualización vertical de un sitio Web activo o en un sitio Web en etapas {#task_E668E1F1240C476DAA1CA783DC728232}

Cuando se completa una actualización vertical activa o una actualización vertical escalonada, puede ver el registro asociado para solucionar cualquier error que se haya producido.

No puede exportar archivos de registro de actualización vertical ni guardarlos. El archivo de registro permanece disponible para su visualización hasta que se produzca el siguiente índice.

**Para ver el registro de actualización vertical de un sitio web activo o en un sitio web escalonado**

1. En el menú del producto, realice una de las siguientes acciones:

   * Haga clic en **[!UICONTROL Index]** > **[!UICONTROL Vertical Update]** > **[!UICONTROL Live Log]**.

   * Haga clic en **[!UICONTROL Index]** > **[!UICONTROL Vertical Update]** > **[!UICONTROL Staged Log]**.

1. En la página de registro, en la parte superior o inferior, realice una de las siguientes acciones:

   * Utilice las opciones de navegación **[!UICONTROL First]****[!UICONTROL Prev]**, **[!UICONTROL Next]**, **[!UICONTROL Last]** o **[!UICONTROL Go to line]** para desplazarse por el registro.

   * Utilice las opciones de visualización **[!UICONTROL Errors only]**, **[!UICONTROL Wrap line]** o **[!UICONTROL Show]** para perfeccionar lo que ve.
