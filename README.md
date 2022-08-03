# Trabajo Práctico de Modelado Semántico 2022
## Integrantes
* Agustín García
* Bruno Agretti
* Juan Wiggenhauser
* Santiago Conelli

## Modelado
El modelado en Protegè se encuentra el archivo X, luego para los datos utilizados contamos con dos archivos: un excel modificado con los datos provistos por la cátedra, y uno con ejemplos adicionales de forma tal que se puedan observar más casos de asignación de beca. Ambos archivos difieren del formato original entregado con el objetivo de procesar más fácilmente su contenido en el software de modelado.

## Video con demostración del modelado en Protegè
Durante la realización de la primer etapa subimos [este video](https://www.youtube.com/watch?v=Af2zAfW4mm0&t=202s&ab_channel=BrunoAgretti) con la intención de explicar a grandes rasgos los problemas encontrados y la solución propuesta.


## Queries
Antes que nada como prefijo para todas las queries se utiliza: ` PREFIX iri: <http://www.semanticweb.org/bruno/ontologies/2022/5/becas-alimentarias-grupo-agarconwi#> `

## Todos los alumnos registrados con sus atributos
```
select distinct ?nombreCompleto ?dni ?cantidadHermanos ?ingresos where {
	?x a iri:Alumno .
    ?x iri:nombreCompleto ?nombreCompleto .
    ?x iri:dni ?dni .
    ?x iri:cantidadHermanos ?cantidadHermanos .
    ?x iri:ingresosNetosCorregidos ?ingresos .
}
```
**NOTA:** Ingresos netos corregidos es el resultado de la suma de los ingresos de los padres menos los gastos por enfermedad en la familia.

## Todos los tipos de beca con sus atributos
```
select distinct ?beca ?ingresoInferior ?ingresoSuperior ?hermanosInferior ?hermanosSuperior where {
	?beca a iri:Beca .
    ?beca iri:ingresosCotaInferior ?ingresoInferior .
    ?beca iri:ingresosCotaSuperior ?ingresoSuperior .
    ?beca iri:hermanosCotaInferior ?hermanosInferior .
    ?beca iri:hermanosCotaSuperior ?hermanosSuperior .
}
```
**NOTA:** Las cotas hacen referencia a los mínimos y máximo de cada parámetro para acceder a una beca en particular. Es decir, dependiendo de los ingresos corregidos de la familia y de la cantidad de hermanos del candidato se lo asigna a una u otra beca. Tener los datos de esta forma permite modificarlo cuando sea necesario. 

## Seleccionar todos los alumnos con becas aprobadas
```
select distinct ?nombreCompleto ?dni ?tieneBeca where {{
	?x a iri:Alumno .
    ?x iri:nombreCompleto ?nombreCompleto .
    ?x iri:dni ?dni .
    ?x iri:tieneBeca ?tieneBeca .
    ?tieneBeca a iri:BecaTotal . 
} union {
    ?x a iri:Alumno .
    ?x iri:nombreCompleto ?nombreCompleto .
    ?x iri:dni ?dni .
    ?x iri:tieneBeca ?tieneBeca .
    ?tieneBeca a iri:BecaParcial .
}}
```

## Seleccionar todos los alumnos con becas rechazadas
```
select distinct ?nombreCompleto ?dni ?tieneBeca where { 
	?x a iri:Alumno .
    ?x iri:nombreCompleto ?nombreCompleto .
    ?x iri:dni ?dni .
    ?x iri:tieneBeca ?tieneBeca .
    ?tieneBeca a iri:BecaRechazada . 
}
``` 

## Alumnos con beca total aprobada
```
select distinct ?nombreCompleto ?dni ?tieneBeca where {
	?x a iri:Alumno .
    ?x iri:nombreCompleto ?nombreCompleto .
    ?x iri:dni ?dni .
    ?x iri:tieneBeca ?tieneBeca .
    ?tieneBeca a iri:BecaTotal . 
}
```

## Alumnos con beca parcial aprobada
```
select distinct ?nombreCompleto ?dni ?tieneBeca where {
	?x a iri:Alumno .
    ?x iri:nombreCompleto ?nombreCompleto .
    ?x iri:dni ?dni .
    ?x iri:tieneBeca ?tieneBeca .
    ?tieneBeca a iri:BecaParcial . 
}
```


