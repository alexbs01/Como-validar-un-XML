# Como validar un XML

## Índice
- [Que es un XML](#que-es-un-xml)
- [Que es un DTD](#que-es-un-dtd)
	- [Donde se puede situar un DTD](./DTD.md#donde-se-puede-situar-un-dtd)
	- [Declaración del contenido de los elementos](./DTD.md#declacración-del-contenido-de-los-elementos)
	- [Declaración de los atributos](./DTD.md#declaración-de-atributos)
	- [Ejemplo de validación con un DTD](./DTD.md#ejemplo-de-validación-con-un-dtd)
- [Que es un Schema](./Schemas.md#Que-es-un-Schema)
	- [Tipos de datos predefinidos](./Schemas.md#Tipos-de-datos-predefinidos)
	- [Elementos simples](./Schemas.md#Elementos-simples)
	- [Propiedades de los elementos](./Schemas.md#Propiedades-de-los-elementos)
	- [Los simpleType](./Schemas.md#Los-simpleType)
		- [Las restricciones en los simpleType](./Schemas.md#Las-restricciones-en-los-simpleType)
		- [Las listas](./Schemas.md#Las-listas)
		- [Las uniones](./Schemas.md#Las-uniones)
	- [Los complexType](./Schemas.md#Los-complexType)
	- [Grupos de elementos](./Schemas.md#Grupos-de-elementos)
	- [Grupos de atributos](./Schemas.md#Grupos-de-atributos)
	- [Ejercicio resuelto](./Schemas.md#Ejercicio-resuelto)
- [Que es XPath](#Que-es-XPath)
	- [Como usar XPath](./XPath.md)


## Que es un XML
Un archivo XML es un documento que se creó para estructurar, almacenar y tranportar la información. Un XML es un texto acotado por etiquetas que establecemos nosotros.  
Un XML consta de una cabecera y un cuerpo, en la cabecera se hayan la versión, con que tipo de carácteres está escrito y la validación o ruta de la validación del XML. Y en el cuerpo se encuentra toda la información, además hay que indicar que un documento XML debe tener un elemento raíz que enmarque todo el cuerpo del documento, este elemento raíz no se puede repetir en niguna otra parte del archivo.  
Un archivo de este tipo, que se valida con un DTD en el propio documento, tiene esta forma:  
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE almacen SYSTEM "almacen.dtd">
<almacen>
	<objeto>
		<nombre></nombre>
		<precio moneda="EUR"></precio>
	</objeto>
	<objeto>
		<nombre></nombre>
		<precio moneda="DOL"></precio>
	</objeto>
</almacen>
```

## Que es un DTD
Un archivo DTD es la descripción de la estructura y sintaxis de un XML. Tiene la función de validar y de mantener su consistencia.  

## Que es un Schema

Un **Schema** es un lenguaje de esquema que se utiliza para describir la estructura y las restricciones de los contenidos de un documento XML de forma precisa.  
La validación de un XML mediante un Schema consta de una doble validación puesto que el Schema a su vez se valida antes del validar al XML. La validación del Schema (que es un archivo .xsd), se hace poniendo las siguientes direcciones de Internet.

```xsd
<?xml version="1.0" encoding="utf-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
xsi:noNamespaceSchemaLocation="http://www.w3.org/2001/XMLSchema.xsd"
>
	*
	*
	*
</xs:schema>
```

## Que es XPath

XPath es un lenguaje de consulta usado para seleccionar nodos de un documento XML, mostrar información o calcular valores de dichos documentos ya sean sumas, cuentas, etc.
El sistema de etiquetas de un XML se podría considerar como si fuera un sistema de archivos de un ordenador, en la que todas la etiquetas descienden del raíz. En XML la etiqueta raíz equivaldría al directorio **/** en *Linux* o a **C:/** en *Windows*.  

