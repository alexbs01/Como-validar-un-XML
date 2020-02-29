# Como validar un XML

## Índice

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
