# Trabajo Práctico de Modelado Semántico 2022
## Integrantes
* Agustín García
* Bruno Agretti
* Juan Wiggenhauser
* Santiago Conelli

## Modelado
El modelado en Protegè se encuentra el archivo `TP-MS-conDatos.owl` (o `TP-MS-RDF.owl`, que es el archivo que utilizamos para cargar los datos a GraphQL), luego para los datos utilizados contamos con dos archivos: un excel modificado con los datos provistos por la cátedra (`BecasPlanillaModificada.xlsx`), y uno con ejemplos adicionales de forma tal que se puedan observar más casos de asignación de beca (`BecasPlanillaModificadaEjemplosExtras.xlsx`). Ambos archivos difieren del formato original entregado con el objetivo de procesar más fácilmente su contenido en el software de modelado.
Estos datos pueden ser importados utilizando un plugin de Protegè llamado Cellfie, el cuál provee una sintaxis para extraer los individuos en una hoja de cálculo. El script utilizado para esta extracción es: `transform-rules.json`, se encuentra subido en el repositorio.

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
![capturaAlumnos](https://user-images.githubusercontent.com/62401276/182728723-94385045-3195-4ab3-8249-d34dcdc720ae.png)

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
![capturaBecas](https://user-images.githubusercontent.com/62401276/182728696-a3eca04f-4eb7-4da6-8f88-b76f03f1dc77.png)

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
![capturaAprobadas](https://user-images.githubusercontent.com/62401276/182728652-181a493e-f8be-4fee-ab93-37e7d4ba4772.png)

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
![capturaRechazadas](https://user-images.githubusercontent.com/62401276/182728866-2895ab16-ecb5-4228-93a5-9924f2ef2209.png)


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
![capturaBecaTotal](https://user-images.githubusercontent.com/62401276/182728846-6f3df5cb-d268-41bd-992c-846a63aa04aa.png)


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
![capturaParciales](https://user-images.githubusercontent.com/62401276/182728756-cdde4706-5fd7-4a66-8a1f-5b557bd36bd9.png)


