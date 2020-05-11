## Nomenclatura

- **/**: Si está al principio de la consulta, la etiqueta que se deberá de poner será la etiqueta padre. Si no es lo primero que se pone, indicará que es hijo de la etiqueta anterior.  
```/instituto/curso/alumno```: Con está consulta se nos mostrarían los alumnos que están dentro de un curso del instituto. Instituto sería la etiqueta padre, curso es hijo de instituto y alumno es hijo de curso, y curso y alumno son descendientes de instituto.  
La anterior consulta de XPath podría tener un XML de esta forma, pero puede haber un número ilimitado de descendientes.  
```
<instituto>
	<curso nombre="" duracion="">
		<alumno></alumno>
		<alumno></alumno>
	</curso>
	<curso nombre="">
		<alumno></alumno>
		<alumno></alumno>
	</curso>
</instituto>
```

- **//**: La doble barra indica descendencia, es decir, usando la anterior sentencia de XPath, sería equivalente a poner ```//alumno```. Pero hay que tener cuidado al usar esto porque es descendencia, con el anterior ejemplo de XML no habría ningún probblema, sin embargo //alumno muestra todos los alumnos independientemente de quienes sean sus antecesores, mientras que /instituto/curso/alumno muestra únicamente los alumnos que desciendenden de curso, y este a su vez de intituto.  

- **@atributo**: Se usa para indicar un atributo. Para seleccionar el nombre de todos los cursos, que es un atributo, pondríamos por la forma más corta ```//curso/@nombre```.  

-----

Una vez estemos en el nodo que queremos que nos muestre habrá que indicar que queremos que nos muestre, y para ello tenemos estas opciones:  

- **node()**: Muestra todos los nodos descendientes incluyendo el texto que pueda contener cada uno y los atributos. Si lo usamos sin ser descendiente de nada, es decir, poniendo simplemente ```node()``` mostrará todo el contenido del XML.  

- **text()**: Muestra solo el texto, si con el XML de antes ponemos ```//alumno/text()``` mostraría el texto que tuviera dentro, pero solo si alumno fuera un nodo final que contiene datos y no otros nodos.  
También hay que decir que se puede combinar con **//**, si ponemos ```//text()``` mostraría todos los textos del XML.  

- **\***: Muestra todos los elementos excepto los textos.  

- **@***: Muestra todos los valores de los atributos.  

- **comment()**: Se usa para mostrar los comentarios.  

Todos los elementos de esta lista se pueden usar junto con **//** para mostrar o todos los comentario, o todos los atributos junto con sus valores, o todos los textos o todos los nodos.  

-----

Pero para hacer consultas más **complejas** se necesitan hacer restricciones, y para esto se usan los **corchetes** para hacer predicados que restrijan la salida de datos.  

- **[@atributo]**: Muestra los nodos que tengan ese atributo. Con el XML de antes, si ponemos ```//curso[@duracion]``` solo mostrará el primer curso porque de los dos es el único que tiene el atributo de duración.  

- **[numero]**: Si indicamos un número mostrará el elemento que esté en esa posición, como podemos no saber el número de veces que aparece un elemento es fácil de que pase, para saber el último se usa la función **last()**.  
Para saber el primer alumno de cada curso se hará ```//curso/alumno[1]```.  
Para saber el último alumno de todos los que aparecen se deberá poner ```//alumno[last()]```.  
Para saber el penúltimo alumno de todos los que hay se puede hacer ```//alumno[last()-1]```

- **Los operadores**: En XPath se pueden usar operadores lógicos **and**, **or** y **not()**; aritméticos **+**, **-**, **\***, **div** y **mod**; y de comparación como **=**, **!=**, **<**, **>**, **<=** y **>=**.  
Van entre corchetes de esta forma; ```//alumno[@edad<=17]/nombre```, esto mostrará el nombre de los alumnos menores de edad.  

## Funciones útiles

- **sum(x)**: Tiene la función de sumar lo que pongamos entre paréntesis, si en el caso de que tuvieramos acceso a un XML de un cine y quisieramos saber la duración total de todas las películas que hay en cartelera, haríamo: ```sum(//horas)```, y nos devolvría e número total de horas.  

- **starts-with(x, "s")**: Se usa para buscar los elementos que comienzan por la cadena **s**. Si quisieramos saber la dirección de todos los alumnos que tienen el nombre *José* como primer nombre tendríamos que hacer: ```//alumno[starts-with(nombre, "José")]/direccion```.  

- **contains(x, "s")**: Si queremos buscar los elementos que contienen una palabra, pero que no necesariamente deben empezar con ella posemos usar esta función de la misma manera que stats-with.  

- **count(x)**: Se usa de la misma forma que **sum()**, solo que en vez de sumar cuenta, para contar el número de alumnos se haría: ```count(//alumno)```.  

- **position()**: Tiene la función de mostrar los elementos de una o varias posiciones, poneiendo [3] se podía ver el elemento de la tercera posición, pero para ver los tres primeros elementos se usa **position()**, ```//alumno[position()<=3]```.  

