---
description: Search&amp;Notas de la versión de Promote 8.9.8.
solution: Target
title: Search&amp;Promote 8.9.8 Notas de la versión (23/05/2013)
topic-legacy: Release Notes,Site search and merchandising
uuid: ff4bfc53-1d0e-4b7d-83ad-54c81d3f9769
exl-id: f4034289-e9cf-4cc4-97bf-2bc5769c043d
translation-type: tm+mt
source-git-commit: 7559f5f7437d46e3510d4659772308666425ec96
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 56%

---

# Notas de la versión 8.9.8 (23/05/2013){#search-promote-release-notes}

<table> 
 <thead> 
  <tr> 
   <th colname="col1" class="entry"> <p>Nueva función </p> </th> 
   <th colname="col2" class="entry"> <p>Descripción </p> </th> 
  </tr> 
 </thead>
 <tbody> 
  <tr> 
   <td colname="col1"> <p> Frases comunes: compatibilidad de coincidencia exacta </p> </td> 
   <td colname="col2"> <p> Las frases comunes contienen términos de dos o más palabras que se buscan como un todo (como "corte de arranque" o "techo de tanque") y no como partes separadas. Una frase común tiene un significado que es único y diferente de sus partes individuales. </p> <p> Usted mantiene un diccionario de frases comunes relacionadas con su negocio. Cuando un cliente efectúa una consulta de búsqueda que contiene varias palabras, se busca en el diccionario la coincidencia exacta.  </p> <p>Puede agregar, editar o eliminar frases comunes. También puede agrupar frases comunes de forma similar a los diccionarios de dominios. Por ejemplo, puede agrupar frases comunes por ropa, telas, bisutería, medidas, compras y frases generales. </p> <p>Consulte <a href="../c-about-linguistics-menu/c-about-common-phrases.md#concept_4946E53586DF492EAEB1B7F757FD440F" format="dita" scope="local"> Acerca de las frases comunes </a>. </p> </td> 
  </tr> 
 </tbody> 
</table>

**Correcciones y mejoras**

* El parámetro de búsqueda backend CGI `sp_date_range_#` no funcionaba para los campos definidos por el usuario.

   Consulte [Parámetros CGI de búsqueda back-end](../c-appendices/c-cgiparameters.md#reference_582E85C3886740C98FE88CA9DF7918E8).

* Al revertir la versión **[!UICONTROL History]** no se actualizaba el contenido del campo de puntos de entrada de la URL.

   Consulte [Uso de la opción Historial](../t-using-the-history-option.md#task_70DD3F87A67242BBBD2CB27156F43002).

   Consulte también [Acerca de los puntos de entrada de URL](../c-about-settings-menu/c-about-crawling-menu.md#concept_5D857E3B5C124E85BC0B5AE77A509573).

* La codificación JSON no gestionaba los caracteres codificados incorrectamente.
* Ahora se ha añadido compatibilidad que le permite publicar de forma remota un índice gradual.

   Consulte [Acerca del control remoto para la indexación](../c-about-index-menu/c-about-remote-control-for-indexing.md#concept_C79B322190E84106A434E5C6D4A4118F).
