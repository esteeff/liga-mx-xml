# Liga MX - XML y DTD

## Objetivo

Definir la estructura de un documento XML que permita representar los
resultados y estadísticas de los partidos de la Liga MX, y validarlo
mediante un DTD.

## Estructura del repositorio

```text
liga-mx-xml/
├── README.md
├── xml/
│   └── resultados.xml
└── dtd/
    └── resultados.dtd 

```

## Modelo de datos

Elemento raíz: jornada.

Atributos de jornada: fecha, liga y temporada.

Estructura:

```text
jornada
└── partido+
├── equipo_local
│   ├── nombre
│   ├── goles
│   └── goleador+
│       ├── jugador
│       └── minuto
├── equipo_visitante
│   ├── nombre
│   ├── goles
│   └── goleador+
│       ├── jugador
│       └── minuto
├── estadio
├── ciudad
└── arbitro
```

## Decisiones de diseño

- El elemento raíz jornada agrupa todos los partidos de una misma
fecha. Se usan atributos fecha, liga y temporada porque son datos
que describen a la jornada en su conjunto.
- Cada partido tiene un atributo id para identificarlo.
- equipo_local y equipo_visitante son elementos compuestos porque
contienen el nombre del equipo, sus goles y la lista de goleadores.
- Cada gol se representa con un elemento goleador, que contiene el
jugador y el minuto en que anotó.
- Se utiliza el operador + en partido y en goleador para permitir que
una jornada tenga uno o más partidos, y cada equipo uno o más
goleadores.
- Los datos cortos y descriptivos como fecha, liga, temporada e id se
representan como atributos. Los datos que forman parte del contenido
del partido se representan como elementos.

## Jornada representada

Se representan los partidos de la jornada del domingo 27 de septiembre
de 2026:

- Pumas UNAM 2 - 3 Atlético de San Luis
- León 2 - 1 FC Juárez
- Necaxa 2 - 4 América

## Validación

El documento XML se valida contra el DTD resultados.dtd.

Pruebas realizadas:

| Prueba | Resultado |
|--------|-----------|
| XML completo con los tres partidos | Válido |
| Eliminar el atributo id de un partido | No válido |
| Cambiar el orden de equipo_local y equipo_visitante | No válido |
| Eliminar todos los goleadores de un equipo | No válido |
| Quitar el atributo fecha de jornada | No válido |

## Conclusiones

- Un DTD permite validar la estructura del documento XML y garantizar
que los datos siguen un formato predecible.
- Los atributos se usan para describir el contexto general, como la
fecha de la jornada o el id del partido. Los elementos se usan para
el contenido variable, como los equipos o los goleadores.
- La cardinalidad partido+ permite representar jornadas con cualquier
número de partidos.
- La cardinalidad goleador+ permite representar equipos con cualquier
número de goles.
- Separar el XML y el DTD en carpetas distintas facilita la
organización del proyecto y su reutilización.




## XML Schema (XSD)

### Descripción

Además de la validación con DTD, el documento XML también puede
validarse mediante un XML Schema (XSD). El archivo se encuentra en:

```text
liga-mx-xml/
└── xsd/
    └── resultados.xsd
```

### Asociación con el XML
El archivo resultados.xml referencia al XSD mediante el atributo
xsi:noNamespaceSchemaLocation:

```text
<jornada xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="../xsd/resultados.xsd"
         fecha="2026-09-27" liga="Liga MX" temporada="Apertura 2026">
```

### Estructura definida en el XSD

```text
jornada
└── partido+
    ├── equipo_local (tipoEquipo)
    ├── equipo_visitante (tipoEquipo)
    ├── estadio
    ├── ciudad
    └── arbitro
El tipo tipoEquipo se define así:
```


```text
tipoEquipo
├── nombre
├── goles
└── goleador+
    ├── jugador
    └── minuto
```


### Atributos declarados
| Elemento | Atributo | Tipo | Obligatorio |
|----------|----------|------|-------------|
| jornada | fecha | xsd:date | Sí |
| jornada | liga | xsd:string | Sí |
| jornada | temporada | xsd:string | Sí |
| partido | id | xsd:string | Sí |



### Tipos de datos usados
| Tipo XSD | Se usa en | Por qué |
|----------|-----------|---------|
| xsd:string | nombre, jugador, minuto, estadio, ciudad, arbitro, liga, temporada, id | Son textos libres |
| xsd:date | fecha | Obliga al formato AAAA-MM-DD |
| xsd:nonNegativeInteger | goles | Número entero no negativo |


### Cardinalidades
| Elemento | Cardinalidad | Cómo se expresa en XSD |
|----------|--------------|------------------------|
| partido | uno o más | maxOccurs="unbounded" |
| goleador | uno o más | maxOccurs="unbounded" |
| equipo_local | exactamente uno | sin minOccurs ni maxOccurs |
| equipo_visitante | exactamente uno | sin minOccurs ni maxOccurs |
| estadio, ciudad, arbitro | exactamente uno | sin minOccurs ni maxOccurs |


### Pruebas de validación con XSD
| Prueba | Cambio realizado | Resultado |
|--------|------------------|-----------|
| XML completo | Sin cambios | Válido |
| Fecha no ISO | fecha="27-09-2026" | No válido |
| Goles negativos | goles="-1" | No válido |
| Sin id | eliminar id del partido | No válido |
| Orden invertido | visitante antes que local | No válido |
| Sin goleadores | eliminar todos los goleador | No válido |


### Ventajas del XSD frente al DTD
- Está escrito en XML, por lo que usa la misma sintaxis que el
documento que valida.

- Permite definir tipos de datos específicos como fechas y números,
lo que evita valores inválidos como goles negativos o fechas mal
formateadas.

- Permite restringir valores mediante enumeraciones, patrones y
rangos.

- Es más expresivo y más utilizado en proyectos modernos.