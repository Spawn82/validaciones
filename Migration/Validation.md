En este documento se van a dar las pautas de limpieza del documento del primer proyecto (Migración).

Primera Sección.

Los documentos con los que se trabajo aqui, son los siguientes:

- Incomplete_Contacts_07_25_2019.csv
- Incomplete_Contacts_07_25_2019_invalid.csv
- Mailing Country - Values.xlsx
- Data Cleaning Rules for IDB Susbcriber file for Migraton.docx

Documentos anexos:
- Caracteres_Especiales_Identificados.xlsx

El documento base para trabajar era Incomplete_Contacts_07_25_2019.csvm, este documento presentaba los siguientes errores:

- En los campos correspondientes a las columnas FirstName y LastName, presentaba errores de compilacion de tipografia y datos errados (celdas vacias, con N/A, null, ??).

- En los campos de MailingAddress.country y MailingAddress.countryCode no todos tenian información correcta y otros estaban vacios.

¿Que se debia hacer con el documento?

- Eliminar el contenido de las celdas que en su contenido tuviesen:
	- Null (como fuese que estuviese escrito)
	- N/A (como fuese que estuviese escrito)
	- ?? o más

- Corregir los errores de tipografia (mostrados en el adjunto Caracteres_Especiales_Identificados.xlsx), en este documento se muestran las combinaciones de letras y caracter que representan a una letra.

- Corrección manual de los nombres (estar pegados(JAVIERMACIASTEJADA por Javier Macias Tejada)).
- Normalización de textos a Big Cap inicial.

- Si LastName tiene más de 2 palabras y FirstName esta vacio, poner la primera en FirstName y en LastName dejar la/s otra/s.

- Si FirstName tiene más de una palabra y LastName esta vacio dejar la primera en FirstName y mandar el resto a LastName.

- Si al final del Email en la columna Email tiene un TLD, verificar si existe en Mailing Country - Values.xlsx, y de ser asi reemplazar en la columna MailingAddress.country la celda por el pais del TLD y el MailingAddress.countryCode por el TLD sin el punto.

- Comparar los ID de los contactos luego de limpiar para extraer los que coinciden con los ID del documento Incomplete_Contacts_07_25_2019_invalid.csv, aquí hay algunos que deben ser eliminados, entonces si hace match eliminarlos del documento final.

Las verificaciones programables estaran en la seccion de Code, las que deben hacerse manualmente son las siguientes:

- Separar manualmente los nombres que vienen juntos en la columna FirstName y en LastName.

- Desde las celdas de excel usar la formula de =Nompropio("C2") y repetir hasta la última, para hacer un Big Cap inicial a los nombres, copiar y pegar valores sobre el valor inicial y borrar la columna que contiene la formula. Repetir con D2 completo (esta se puede programar para que se haga por Code, se actualizará pronto con la automatización de este proceso).

**Nota: Se debería ejecutar la limpieza por parte del Code antes de pasar a la limpieza manual.**

- Para determinar si la columna LastName o FirstName tienen más de una palabra, se puede usar una funcion que recorra la columna y haga un split por espacios (partir el contenido de la celda en una lista por espacios) y luego si la cantidad es mayor a 1, realizar una lista con los datos y hacer las validaciones, si LastName tiene más de 2 palabras y FirstName esta vacio, poner la primera en FirstName y en LastName dejar la/s otra/s, si FirstName tiene más de una palabra y LastName esta vacio dejar la primera en FirstName y mandar el resto a LastName (el Code que tengo por ahora es de UIPath, lo anexo, sin embargo esté estará pendiente a cambio para trabajar con VBA u otro lenguaje de programación más común).

<code>

	if
	firstNameParts.Count > 1 and string.IsNullOrWhiteSpace(row("LastName").ToString)
	firstNameParts(0).ToString
	(From ss In firstNameParts.AsEnumerable
		Select ss Where ss <> firstNameParts(0)).ToArray
		row("LastName").ToString.trim+" "+item
		
</code>

**Nota: este code es más complejo pues no se puede explicar todo ya que es un lenguaje bastante visual de trabajar, sin embargo se buscará corregir esto realizando este Code en un lenguaje mas comun.**

- Comparar los ID para ver si existe en la tabla principal y está en la tabla de datos de Incomplete_Contacts_07_25_2019_invalid.csv, en este caso eliminar la entrada.

<code>

	Assign
	MyRow = (From z In dt_compare.AsEnumerable
			Where z("Id").ToString.Equals(row("Id").ToString)
			Select z).FirstOrDefault
	if
		IsNothing(myRow)
			then
				add data row to finaldt
			else
				add data row to deletedt

</code>

- Por último, lo menciono como manual, sin embargo tiene una automatizacion muy buena, y es el proceso de verificar los TLD, se puede extraer de la columna Email el TLD final con un Split por "." que traiga lo que quedé al final y lo comparé con los TLD de Mailing Country - Values.xlsx, si lo encuentra, que tome el TLD y lo agregue a la columna de MailingAddress.countryCode (si ya hay algo, que lo borre y escriba el encontrado) y le agregue el país del TLD a la columna MailingAddress.country (lo hice con UIPath, anexo el Code, sin embargo trataré de actualizarlo para que sea más común).

<code>

	Assign
	A = Split(row("Email").ToString,".").Last
	B = (From z In MailingCountryDT.AsEnumerable
		Where z("ISOCODE").ToString.ToLower.Equals(Split(row("Email").ToString,".").Last)
		Select z).FirstOrDefault
	If
		B IsNot Nothing
		then
			Secuence
				Assign
					row("MailingAddress.country") = myRow("COUNTRY").ToString
				Assign
					row("MailingAddress.countryCode") = myRow("ISOCODE".ToString)

</code>>

Seccion de Code:
Macro de VBA para eliminar null de las columnas FirstName y LastName donde selection tomará los valores entre C1 a C83527 y D1 a D83257, esto con el objetivo de solo eliminar null de la Columna FirstName y LastName y no borrar de la columna de Error o de otro lado que no sea necesario.

Además manualmente deben filtrarse algunos nombres que incluyen null en su estructura, por ejemplo: Amanullah o Faizanullah.

<code>

	Sub
 	   Range("C:D").Select
    	Range(Selection, Selection.End(xlDown)).Select
   	 	Range(Selection, Selection.End(xlDown)).Select
	   	 Selection.Replace What:="null", Replacement:="", LookAt:=xlPart, _
	    	    SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:=False, _
    		    ReplaceFormat:=False
    EndSub

</code>

Macro de VBA para eliminar n/a de las columnas FirstName y LastName donde selection tomará los valores entre C1 a C83527 y D1 a D83257.
<code>

    Sub
 	    Range("C:D").Select
    	Range(Selection, Selection.End(xlDown)).Select
   	 	Range(Selection, Selection.End(xlDown)).Select
	   	 Selection.Replace What:="n/a", Replacement:="", LookAt:=xlPart, _
	    	    SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:=False, _
    		    ReplaceFormat:=False
    EndSub

</code>

Macro de VBA para eliminar ??? de las columnas FirstName y LastName donde selection tomará los valores entre C1 a C83527 y D1 a D83257.

<code>

    Sub
 	    Range("C:D").Select
    	Range(Selection, Selection.End(xlDown)).Select
   	 	Range(Selection, Selection.End(xlDown)).Select
	   	 Selection.Replace What:="~?~?~?", Replacement:="", LookAt:=xlPart, _
	    	    SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:=False, _
    		    ReplaceFormat:=False
    EndSub

</code>

Para eliminar los caracteres extraños, este código debe repetirse para cada uno de las combinaciones de caracteres en donde dice ChangeCharacter.

<code>

    Sub
 	    Range("C:D").Select
    	Range(Selection, Selection.End(xlDown)).Select
   	 	Range(Selection, Selection.End(xlDown)).Select
	   	 Selection.Replace What:="ChangeCharacter", Replacement:="", LookAt:=xlPart, _
	    	    SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:=False, _
    		    ReplaceFormat:=False
    EndSub

</code>

Los documentos entregados al final fueron los siguientes:
- Final_Version_Contacts_Cleaned.csv
- Invalid_Contacts_To_Delete.xlsx

El Final, contiene los contactos limpiados y el invalid contacts, contiene los ID de los contactos que deben ser eliminados.

___________________________________________________________________________________________________________________

Segunda sección.

El documento Final_Version_Contacts_Cleaned.csv tuvo fallas en tipografia, estas fallas de tipografia fueron consignadas en el documento ContactsWithErrors_08152019.xlsx, los errores identificados fueron 3:

- Tipografia en el Email:
	- Nombres que fallaban como: Sánchez, João, Guimarães, que debido a una corrección automatica por excel de nombres con los parametros de tipografia, se habían cambiado por error en Email también.

- Datos ingresado en MailingAddress.country:
	- Se arregló con una re comparativa con los datos de Mailing Country - Values.xlsx, para que tuviese la misma sintaxis.

- Nombres muy largos:
	- Eran empresas o fundaciones que se habían pasado por alto en la primera limpieza (3 casos), se elimino la información de las celdas FirstName y LastName y con esto se solucionó.

Los documentos que fueron entregados para trabajar fueron:
- ContactsWithErrors_08152019.xlsx

El documento reparado tiene el nombre de:
- ContactsRepared.xlsx