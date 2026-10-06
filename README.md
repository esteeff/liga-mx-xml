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