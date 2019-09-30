Esté documento corresponde al segundo proyecto.
xccxzcxzcxz
Primera sección.

En este documento se van a dar las pautas de limpieza del documento del segundo proyecto (Caribe).

Los documentos presentados para está limpieza son los siguientes:
- Documento base.csv
- Active Institutions in LMS_BA.xlsx
- Active Institutions in LMS_BH.xlsx
- Active Institutions in LMS_GY.xlsx
- Active Institutions in LMS_SU.xlsx
- Active Institutions in LMS_TT.xlsx
- Accounts Without Contacts-2019-08-21-14-21-30.xlsx
- Data Cleaning Process and Rules for CRM.pptx

**Nota: El Documento Base.csv, es el documento con la información que nos fue suministrada por salesforce desde el CMR**

El Documento Base.csv es de donde se va a extraer la información extra para cada LMS, las reglas son las siguientes:
- Los documentos LMS, tienen una "Sheet" que tiene un Active Inst LMS, en está "Sheet" hay varias columnas para trabajar, se debe buscar si la empresa de Name "X" y de Acronym "X" existe en el Documento base, y si es así cópiar la información extra del documento base para este active, sino, no hacer nada.

- Luego en el documento base comparar si hay algún contacto con 0 oportunidades y que en el documento Accounts Without Contacts-2019-08-21-14-21-30.xlsx, aparezca su ID, si es este el caso copiarlo a una lista diferente para eliminarlo de la base de datos.

- Por último los que no aplicaron a la primera acción ni aplicaron a la segunda (todo lo que sobró del documento, que aplique a los paises con los que se trabajó), mandarlo a otra lista para verificación.

Debido al tamaño de los archivos y a que la información no era la misma (podian variar hasta en un espacio), decidí ejecutar la limpieza de manera manual, usando la opcion de resaltar de excel, sin embargo seré lo más preciso posible y a futuro trataré de generar automatizaciones a estos procesos:

- Eliminé los espacios al final de las celdas de los Acronym de cada LMS con la opción de busqueda y reemplazo por columnas de excel, con ello pude lograr hacer comparativas de igualdad entre Acronym del documento base y Acronym de los LMS, al resaltar los elementos iguales, pude encontrar mas rapido lo que buscaba y pasar el contenido extra del documento base al LMS correspondiente (la automatizacion podria haber ayudado muchisimo) y esté será el documento final.

- Hice exactamente lo mismo con el documento Accounts Without Contacts-2019-08-21-14-21-30.xlsx, para así poder comparar los ID, sin embargo los ID tenian una versión recortada entonces además le agregue un filtro a excel para solo buscar por cada pais solicitado, si existía se creaba un documento aparte para llevarlo para eliminacion.

- Por ultimo, los que no entraban en LMS o no estaban en la lista de Without Contacts-2019-08-21-14-21-30.xlsx, se copiaban a una lista aparte, para su verificación.

Al finalizar el proceso los documentos entregados fueron los siguientes:
- To Delete Contacts Only ID.xlsx : Los ID de las cuentas que están por eliminar del CMR, por 0 oportunidades, no estar en el LMS y estar en la lista de Accounts without contacts.

- To Delete Contacts.xlsx : Contiene toda la información para validar si se van a eliminar o no.

- To Update Contacts.xlsx : Contiene toda la información que estaba en el LMS unida con la información que se tenía en el CMR.

- To verificate.xlsx : Aquí están todas las cuentas por verificar, pues no están en la lista Accounts without contacts, ni en LMS, son los que sobran del CMR en su primera hoja y en su segunda hoja están los que tienen oportunidades.

- Y en el archivo comprimido, está toda la información pero separada por países, los que son para hacer update y los que son para delete.


_______________________________________________________________________________________________________________
Segunda sección.

Luego de realizar la entrega, se solicitó que se cambiase el orden de las columnas del documento final y anexar una verificacion por Tipo de institución para este documento final, esta verificación esta estipulada en el siguiente documento de excel:

- Verificacion de sector.xlsx

Se hizo entrega de un documento de nombre:

- CCB Accounts  verificate.xlsx

El cual se debía corregir con la comparativa de los parametros del documento de verificación de sector para cambiar el contenido de las celdas correspondientes a las columnas de Institution Type, Institution Subtype y Public or Private, del documento final entregado en la primera sección.

Y el nuevo documento de salida fue:
- CCB Accounts  verificate (version 1).xlsx
