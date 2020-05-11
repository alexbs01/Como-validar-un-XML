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

### Propiedades de los elementos

De las propiedades de los elementos de momento expliqué solo dos, *name* y *type*, pero hay más:

1. **name**: Se usa para atribuir una etiqueta del XML al elemento del Schema.  
2. **type**: Se utiliza para aplicar una restricción del tipo xs:integer o xs:string, o para referenciar a una restricción externa al elemento.  
3. **maxOccurs**: Establece un número máximo de ocurrencias de un elemento puesto que por defecto debe aparecer una vez, si a esta propiedad la ponemos *unbounded* significará que no tendrá límite.  
4. **minOccurs**: Establece un número mínimo de ocurrencias de un elemento, si ponemos que es cero, hacemos que sea opcional.  
5. **default**: Pone un valor por defecto al elemento en caso de que quede vacío en el XML.  
6. **fixed**: Asigna un valor fijo que no se puede cambiar, como es normal, esta opción no es comppatible con *default*.  
7. **ref**: Su valor es el nombre de otro elemento del Schema, y se le atribuyen las restricciones del elemento al que se está referenciando.  

### Los simpleType

Los simpleType son la forma de agrupar las características de un elemento del XML, ya sea una restricción, una lista, o una unión de ambas.  
Existen dos formas de declarar las características de un elemento mediante un simpleType, estas son, o dentro de la etiqueta element, o fuera.  

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

-----
**IMPORTANTE**: Para usar la segunda opción no se puede usar un dato predefinido de los que puse más arriba, en este caso hay que usar una palabra que lo identifique puesto que la restricción por ser un número o un texto se aplicará en el simpleType. Por ejemplo, para un DNI se podría poner type="tipoDNI" y que linkee con un simpleType que tenga un patrón para DNI.  

-----

#### Las restricciones en los simpleType

Las restricciones son el principal método para hacer que un Schema solo valide solo determinadas formas de XML. Una restricción tiene la forma de ```<xs:restriction base="tipoDeBase"> * * * </xs:restriction>``` y dentro de *restriction* pueden ir estas opciones:  
1. **length**: Establece una longitud fija para una cadena de caracteres.  
2. **minLength**:  Establece una longitud mínima para una cadena de texto.  
3. **maxLength**: Establece una longitud máxima para una cadena de texto.  
4. **pattern**:  Se usa para crear un patrón mediante expresiones regulares.  
5. **enumeration**:  Enumera posibles valores
6. **maxInclusive**: Pone un máximo al rango que se establece incluyendo el valor que se asigna.  
7. **maxExclusive**: Pone un máximo al rango que se establece excluyendo el valor que se asigna.  
8. **minInclusive**: Pone un mínimo al rango que se establece incluyendo el valor que se asigna.  
9. **minExclusive**: Pone un mínimo al rango que se establece excluyendo el valor que se asigna.  

Si quisieramos limitar la edad de las personas a un determinado rango, como por ejemplo entre 18 y 45 años haríamos lo siguiente. Lo pondré como si estuviera dentro del elemento.  

```xsd
<xs:element name="edad">
	<xs:simpleType>
		<xs:restriction base="xs:integer">
			<xs:minInclusive value="18"/>
			<xs:maxInclusive value="45"/>
		</xs:restriction>
	<xs:simpleType>
</xs:element>
```

-----
Si quisieramos establecer una longitud máxima de 30 caracteres para un nombre haríamos los siguiente. Esta vez lo pondré fuera del elemento.  

```xsd
<xs:element name="nombre" type="tipoNombre"/>

<xs:simpleType name="tipoNombre">
	<xs:restriction base="xs:string">
		<xs:maxLength value="30"/>
	</xs:restriction>
<xs:simpleType>

```

-----
Si tenemos que validar un datos como un DNI tendremos que usar un patrón.  

```xsd
<xs:element name="dni" type="tipoDNI"/>

<xs:simpleType name="tipoDNI">
	<xs:restriction base="xs:string">
		<xs:pattern value="[0-9]{8}[A-Z]"/>
	</xs:restriction>
<xs:simpleType>
```

#### Las listas
Una lista en un XML es la sucesión de valores dentro de la misma etiqueta que tienen esta forma:  

```xml
<edadesParticipantes>19 31 26 52 18 32 27 22 36 31 46</edadesParticipantes>
```

Aunque no sea la mejor forma de almacenar información, se puede hacer y también validar con un Schema. La anterior situación se podría solucionar...  

```xsd
<xs:element name="edadesParticipantes" type="tipoEdadesParticipantes"/>

<xs:simpleType name="tipoEdadesParticipantes">
	<xs:list itemType="xs:integer"/>
</xs:simpleType>
```

#### Las uniones
Las uniones tienen la función de unir varios tipos de simpleType, como puede ser el caso de en un campo tener un DNI. o si no se tiene, poder poner un texto que diga "No tiene DNI".  

```xsd
<xs:element name="dni" type="tipoDNI"/>

<xs:simpleType name="tipoDNI">
	<xs:union>
		<xs:simpleType>
			<xs:restriction base="xs:string">
				<xs:pattern value="[0-9]{8}[A-Z]"/>
			</xs:restriction>
		</xs:simpleType>
		<xs:simpleType>
			<xs:restriction base="xs:string">
				<xs:enumeration value="No tiene DNI"/>
			</xs:restriction>
		</xs:simpleType>
<xs:simpleType>
```

También está la posibilidad de unir simpleType que no estén dentro del ```xs:union``` usando el *memberTypes*, este atributo tambíen sirve para unir con un tipo predefinido que puse al principio.  

```xsd
<xs:element name="dni" type="tipoDNI"/>

<xs:simpleType name="tipoDNI">
	<xs:union memberTypes="tipoDNI noHayDNI">
<xs:simpleType>

<xs:simpleType name="tipoDNI">
	<xs:restriction base="xs:string">
		<xs:pattern value="[0-9]{8}[A-Z]"/>
	</xs:restriction>
</xs:simpleType>
		
<xs:simpleType name="noHayDNI">
	<xs:restriction base="xs:string">
		<xs:enumeration value="No tiene DNI"/>
	</xs:restriction>
</xs:simpleType>
```

### Los complexType
Los complexType son aquellos elementos que no contienen datos en su interior, contienen otros elementos que, o bien pueden ser complejos y contener más etiquetas en su interior, o simples y contener datos como los que estuvimos viendo hasta ahora.  
Para proclamar un elemento complejo se hace de la siguiente forma:  

```xsd
<xs:element name="aula">
	<xs:complexType>
	*
	*
	*
	</xs:complexType>
</xs:element>
```

Y dentro de *complexType* pueden ir tres etiquetas con sus respectivos usos.  
1. **xs:sequence**: Es el más utilizado ya que se usa para realizar una **secuencia de elementos ordenada**, esto quiere decir que los elementos del XML deberán ir en el orden que marcar el Schema.  
2. **xs:all**: Con este los elementos **pueden estar desordenados**, pero aparecerán como máximo una única vez, por lo que los elementos no pueden llevar *maxOccurs*.  
3. **xs:choice**: De todos los elementos que se puedan poner dentro de esta etiqueta **solo se podrá escoger uno**. Se puede combinar poniendole como atributo el maxOccurs para que se pueda escoger un atributo multiples veces.  

```xsd
<xs:element name="instituto">
	<xs:complexType>
		<xs:sequence>
			<xs:element name="aula">
				<xs:complexType>
					<xs:sequence>
						<xs:element name="numeroMesas"/>
						<xs:element name="numeroSillas"/>
					</xs:sequence>
				</xs:complexType>
			</xs:element>
			<xs:element name="profesor">
				<xs:complexType>
					<xs:all>
						<xs:element name="nombre"/>
						<xs:element name="email"/>
					</xs:all>
				</xs:complexType>
			</xs:element>
			<xs:element name="alumno">
				<xs:complexType>
					<xs:sequence>
						<xs:choice>
							<xs:element name="dni"/>
							<xs:element name="numeroPasaporte"/>
						</xs:choice>
					</xs:sequence>
				</xs:complexType>
			</xs:element>
		</xs:sequence>
	</xs:complexType>
</xs:element>
```
 
En este esquema el elemento *aula* tendrá dos elementos, *numeroMesas* y *numeroSillas* en ese orden. El elemento *profesor* tiene dos elementos hijos que pueden ir en cualquier orden o tambíen pueden no aparecer. Y el elemento *alumno* tiene dos elementos hijos de los cuales solo se podrá escoger uno de los dos.  

### Grupos de elementos

Los grupos de elementos permiten la agrupación de los mismos si estos se usan varios elementos complejos. Es decir, se crean varios elementos, cada uno con sus restricciones y posteriormente se podrá utilizar este grupo de elementos un número ilimitado de veces.  
Para crear un grupo, se hace lo siguiente:  

```xsd
<xs:group name="nombreDelGrupo">
*
*
*
</xs:group>
```

**Es importante recordar que dentro de *group* hay que poner una de las etiquetas que indicaban el orden de los elementos en el *complexType*, se puede poner *xs:sequence*, *xs:all* o bien *xs:choice*.**  

Para referir un grupo se tiene escribir ```<xs:group ref="nombreDelGrupo"/>``` dentro del elemento complejo.  

### Grupos de atributos

Un grupo de atributos tiene la misma función y también una forma muy similar de hacer que la de los grupos de elementos.  
Para crear un grupo de atributos y referenciarlo se hará lo siguiente.  

```xsd
<xs:attributeGroup name="nombreGrupo">
*
*
*
</xs:attributeGroup>

<xs:attributeGroup ref="nombreGrupo"/> 
```

### Ejercicio resuelto

Ahora en los dos siguientes links habrá un XML y un XSD que lo valide.  
[Enlace al XML](./cursoClaves.xml)  
[Enlace al XSD](./cursoClaves.xsd)  

