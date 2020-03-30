# Como validar un XML

## Índice
- [Que es un XML](#que-es-un-xml)
- [Que es un DTD](#que-es-un-dtd)
- [Donde se puede situar un DTD](#donde-se-puede-situar-un-dtd)
- [Declaración del contenido de los elementos](#declacración-del-contenido-de-los-elementos)
- [Declaración de los atributos](#declaración-de-atributos)
- [Ejemplo de validación con un DTD](#ejemplo-de-validación-con-un-dtd)

## Que es un XML
Un archivo XML es un documento que se creó para estructurar, almacenar y tranportar la información. Un XML es un texto acotado por etiquetas que establecemos nosotros.  
Un XML consta de una cabecera y un cuerpo, en la cabecera se hayan la versión, con que tipo de carácteres está escrito y la validación o ruta de la validación del XML. Y en el cuerpo se encuentra toda la información, además hay que indicar que un documento XML debe tener un elemento raíz que enmarque todo el cuerpo del documento, este elemento raíz no se puede repetir en niguna otra parte del archivo.  
Un archivo de este tipo, que se valida en el propio documento, tiene esta forma:  
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

## Donde se puede situar un DTD
Un documento DTD puede estar incluido en el propio XML, o en un archivo aparte.  
Si el DTD es un archivo exerno se pondrá:  
```dtd
<!DOCTYPE elementoRaíz SYSTEM "rutaDelDTD">
```
-----
O si está en el propio documento XML:
```
<!DOCTYPE elementoRaíz [
definiciónDelDTD
]>
```

### Declaración del contenido de los elementos
Los elementos de un XML van entre etiquetas, y con los siguientes atributos se puede definir el contenido del objeto en cuestion.  
Tienen la siguiente forma:
```dtd
<!ELEMENT nombreDelElemento (contenido)
```

- **Los paréntesis**: Las etiquetas que poseen subetiquetas dentro de ellas, llevan todos los elementos hijos entre paréntesis, por orden y con un carácter que indica la repetición o la opcionalidad entre dos o más elementos hijos.  
	- Los elementos hijos van separados por comas y por orden.  
	- Entre paréntesis y con **|** opcionalidad entre operadores.  
	- La **?** indica que el elemento puede estar o **0 o 1 vez**.  
	- El **\*** indica que el elemento se puede repetir entre **0 e infinitas veces**.  
	- El **+** indica que el elemento se debe poner al menos una vez, entre **1 e infinitas veces**.  
- **EMPTY**: Es para validar etiquetas sin contenido, es decir, es para etiquetas que se cierran en la propia etiqueta de apertura. También hay que señalar que se usa sin paréntesis.  
- **ANY**: Permite validar cualquier tipo de contenido, por lo que no se recomienda su uso.  
- **#PCDATA**: Es para un contenido de tipo textual.  

Este DTD validaría un XML que tenga un elemento raíz que contenga un número indeterminado de expositores y de estanterías, los expositores pueden tener o no un libro y pueden tener muchas revistas (como mínimo una). Y las estanterías pueden tener o no libros.  
En esta bibliteca puede haber expositores con una única revista y estánterías sin libros.  
```dtd
<!ELEMENT biblioteca (expositor*, estanteria*)>
		<!ELEMENT expositor (libro?, revista+)>
			<!ELEMENT libro (#PCDATA)>
			<!ELEMENT revista (#PCDATA)>
		<!ELEMENT estanteria (libro*)>
			<!ELEMENT libro (#PCDATA)>
```

### Declaración de atributos

Las etiquetas a parte tener un contenido, también pueden tener un atributo en la entiqueta de apertura.  
Un atributo tiene la siguiente forma:
```dtd
<!ATTLIST nombreDelElemento
	1_nombreDelAtributo tipo valor
	2_nombreDelAtributo tipo valor
>
```
El **tipo** de un atributo indica que puede haber dentro del atributo.  
- **CDATA**: Solo puede tener texto.  
- **NMTOKEN**: Puede contener texto y los carácteres de punto (**.**), guion (**-**), guion bajo (**_**) y los dos puntos (**:**).  
- **NMTOKENS**: Permite lo mismo que los del anterios, solo que tambíen acepta los espacios en blanco.  
- **La barra vertical**: Los atributos que tienen valores separados por una barra vertical, indican la opcionalidad entre ellos, si estos elementos contienen espacios en blanco hay que ponerlos entre comillas simples o dobles.  
- **ID**: El valor del atributo debe ser único, por lo que no se puede repetir en otro elemento. Corresponde con la *clave primaria* o *primary key* de una base de datos.  
- **IDREF**: El valor de este tipo debe corresponder con el valor de un elemento ID. Se corresponde con una *clave foránea* o *foreign key*.  
- **IDREFS**: El valor este tipo debe ser el de dos elementos ID. Corresponde con una clave compuesta que resulta de una *relación de muchos a muchos*.  

El **valor** de un atributo indica como debe ser el atributo.  
- **#REQUIRED**: Indica que el valor deber ser obligario, se debe poner para que el XML se pueda validar.
- **#IMPLIED**: Indica que el atributo se puede poner o no, tampoco tiene porque tener un valor por defecto.  
- **#FIXED**: El valor de ese atributo deberá ser siempre el mismo, no puede variar en nigún elemento.  
- **Valor por defecto**: Un valor por defecto se pone directamente con el valor entre comillas.  

## Ejemplo de validación con un DTD
Ahora con este ejemplo tendremos una visión mejor de como se valida un XML con un DTD. Este XML se valida con el DTD de abajo.  
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE lista SYSTEM "producto3.dtd">
<lista>
	<producto referencia="A1" color="AZUL">
		<nombre>Muñeca de trapo</nombre>
		<fabricante>
			<empresa pais="España">Juegolandia SA</empresa>
			<localidad>Valladolid</localidad>
		</fabricante>
		<precio cliente="final" moneda="dolar">60</precio>
		<precio cliente="distribuidor">40</precio>
	</producto>
	<producto referencia="A2">
		<nombre>Caballito de madera</nombre>
		<fabricante>
			<empresa>Juegolandia SA</empresa>
			<localidad>Valladolid</localidad>
		</fabricante>
		<precio cliente="final">18</precio>
		<precio cliente="distribuidor">12</precio>
	</producto>
	<producto referencia="A3">
		<nombre>Tren madera básico</nombre>
		<fabricante>
			<empresa>Natura SL</empresa>
		</fabricante>
		<precio cliente="final">80</precio>
		<precio cliente="distribuidor">60</precio>
	</producto>
</lista>
```

```dtd
<!ELEMENT lista (producto+)>
	<!ELEMENT producto (nombre, fabricante, precio, precio)>
		<!ATTLIST producto 
			referencia ID #REQUIRED
			color NMTOKEN #IMPLIED
		>
		<!ELEMENT nombre (#PCDATA)>
		<!ELEMENT fabricante (empresa, localidad?)>
			<!ELEMENT empresa (#PCDATA)>
				<!ATTLIST empresa 
					pais CDATA #IMPLIED>
			<!ELEMENT localidad (#PCDATA)>
		<!ELEMENT precio (#PCDATA)>
			<!ATTLIST precio 
				cliente (final|distribuidor) "Sin asignar"
				moneda (euro|dolar) "euro">
```

## Que es un Schema

Un **Schema** es un lenguaje de esquema que se utiliza para describir la estructura y las restricciones de los contenidos de un documento XML de forma precisa.  
La validación de un XML mediante un Schema consta de una doble validación puesto que el Schema a su vez se valida antes del validar al XML. La validación del Schema (que es un archivo .xsd), se hace poniendo las siguiente direscciones de Internet.

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

### Tipos de datos predefinidos

En Schemas, los datos predefinidos llevan el prefijo **xs:**, los más comunes son:  

1. **Para texto**: xs:string.  
2. **Para números**: xs:byte, xs:decimal, xs:double, xs:float, xs:integer, xs:long, xs:negativeInteger (<0), xs:nonNegativeInteger (>=0), xs:positiveInteger (>0) y xs:nonPositiveInteger (<=0).  
3. **Para fecha y hora**: xs:date, xs:dateTime, xs:duration, xs:gDay, xs:gMonth, xs:gMonthDay, xs:gYear, xs:gYearMonth y xs:time.  
4. **Para valores lógicos**: xs:boolean.  

Las fechas están con la forma *año-mes-día*, *dateTime* tiene la forma *año-mes-día**T**horas:minutos:segundos* y gMonthDay lleva dos guiones antes del mes, por lo que tendrá la forma *--mes-día*  

### Elementos simples

Como es normal, al empezar a aprender una cosa, se empieza por lo más fácil, que en este caso no coincide con el principo, por ahora solo hablaré de elementos de tipo simple, que son aquellos que no contienen ninguna etiqueta en su interior, solo contienen datos.  

Un elemento simple tiene la siguiente forma:  

```xsd
<xs:element name="nombreDelElemento" type="tipoDelElemento"/>
```

Donde el nombre del elemento es el nombre de la etiqueta correspondiente en el XML, y el tipo del elemento puede ser uno de los tipos de datos que puse arriba, o también se puede usar para aplicar otro tipo restricciones mediante un simpleType en otra parte del documento que también podrá ser usada por otra etiqueta del XML.  

### Formas de aplicar restricciones a los elementos

Las restricciones que se le aplican a los elementos de un XML pueden ser puestos dentro o fuera de la etiqueta ```xs:element``` teniendo las siguientes formas:  

```xsd
<xs:element name="nombreDelElemento">
	<xs:simpleType>
	*
	*
	*
	</xs:simpleType>
</xs:element>
```

```xsd
<xs:element name="nombreDelElemento" type="tipoDelElemento"/>

<xs:simpleType name="tipoDelElemento">
	*
	*
	*
</xs:simpleType>
```

Como podemos ver la segundo opción tiene el simpleType fuera de la etiqueta del elemento, en este caso, que es el que yo recomiendo se puede usar el mismo simpleType en caso de que se pueda reutilizar, además de que se pueden combinar unos con otros para crear restricciones más complejas.  
**IMPORTANTE: Para usar la segunda opción no se puede usar un dato predefinido de los que puse más arriba, en este caso hay que usar una palabra que lo identifique puesto que la restricción por ser un número o un texto se aplicará en el simpleType. Por ejemplo, para un DNI se podría poner type="tipoDNI" y que linkee con un simpleType que tenga un patrón para DNI.**  

### Propiedades de los elementos

De las propiedades de los elementos de momento expliqué solo dos, *name* y *type*, pero hay más:

1. **name**: Se usa para atribuir una etiqueta del XML al elemento del Schema.  
2. **type**: Se utiliza para aplicar una restricción del tipo xs:integer o xs:string, o para referenciar a una restricción externa al elemento.  
3. **maxOccurs**: Establece un número máximo de ocurrencias de un elemento puesto que por defecto debe aparecer una vez, si a esta propiedad la ponemos *unbounded* significará que no tendrá límite.  
4. **minOccurs**: Establece un número mínimo de ocurrencias de un elemento, si ponemos que es cero, hacemos que sea opcional.  
5. **default**: 


















