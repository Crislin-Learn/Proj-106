# Diccionario de Medidas DAX

## 01. Tickets

### Volume

#### Total Tickets

```DAX
Total Tickets =
COALESCE(
    COUNTROWS(FactTicketsTI),
    0
)
```

**Descripción:** Cantidad total de tickets registrados en el contexto de filtro actual.

#### Total Agents

```DAX
Total Agents =
COUNTROWS(DimAgent)
```

**Descripción:** Cantidad de agentes incluidos en el contexto de filtro actual.

#### Total Tickets All

```DAX
Total Tickets All =
CALCULATE(
    [Total Tickets],
    REMOVEFILTERS(FactTicketsTI)
)
```

**Descripción:** Cantidad total de tickets ignorando los filtros aplicados directamente sobre la tabla de hechos.

#### Total Tickets Selected

```DAX
Total Tickets Selected =
CALCULATE(
    [Total Tickets],
    ALLSELECTED(FactTicketsTI)
)
```

**Descripción:** Cantidad total de tickets considerando la selección realizada por el usuario y preservando el contexto externo del informe.

### Distribution

#### Positive Tickets

```DAX
Positive Tickets =
CALCULATE(
    [Total Tickets],
    DimSatisfaction[Status] = "Positivo"
)
```

**Descripción:** Cantidad de tickets cuya satisfacción está clasificada como positiva.

#### Positive Ticket %

```DAX
Positive Ticket % =
DIVIDE(
    [Positive Tickets],
    [Total Tickets],
    0
)
```

**Descripción:** Porcentaje de tickets clasificados como positivos respecto al total de tickets.

#### Problem Tickets

```DAX
Problem Tickets =
CALCULATE(
    [Total Tickets],
    DimTicketType[Type] = "Problema"
)
```

**Descripción:** Cantidad de tickets registrados con tipo Problema.

#### Problem Ticket %

```DAX
Problem Ticket % =
DIVIDE(
    [Problem Tickets],
    [Total Tickets],
    0
)
```

**Descripción:** Porcentaje de tickets registrados como problemas respecto al total de tickets.

### Productivity

#### Tickets per Agent

```DAX
Tickets per Agent =
DIVIDE(
    [Total Tickets],
    [Total Agents],
    0
)
```

**Descripción:** Promedio de tickets gestionados por agente en el contexto de filtro actual.

---

## 02. Satisfaction

### Average Satisfaction

```DAX
Average Satisfaction =
AVERAGEX(
    FactTicketsTI,
    RELATED(DimSatisfaction[Satisfaction])
)
```

**Descripción:** Promedio de satisfacción de los tickets en el contexto de filtro actual.

---

## 03. Performance

### Metrics

#### Average Resolution Duration

```DAX
Average Resolution Duration =
AVERAGEX(
    FactTicketsTI,
    RELATED(DimResolutionGroup[ResolutionDuration])
)
```

**Descripción:** Promedio de días requeridos para resolver los tickets en el contexto de filtro actual.

#### Selected Tickets per Agent

```DAX
Selected Tickets per Agent =
CALCULATE(
    [Tickets per Agent],
    ALLSELECTED(DimAgent)
)
```

**Descripción:** Promedio de tickets por agente considerando el conjunto de agentes seleccionado en el contexto actual del informe.

#### Ticket Variance per Agent

```DAX
Ticket Variance per Agent =
[Total Tickets]
    - [Selected Tickets per Agent]
```

**Descripción:** Diferencia entre los tickets gestionados por el agente y el promedio de tickets por agente dentro del conjunto seleccionado.

#### Ticket Performance Group

```DAX
Ticket Performance Group =
IF(
    [Ticket Variance per Agent] >= 0,
    "Above Average",
    "Below Average"
)
```

**Descripción:** Clasificación del agente según si su cantidad de tickets gestionados se encuentra por encima o por debajo del promedio del conjunto seleccionado.

### Ranking

#### Ticket Rank

```DAX
Ticket Rank =
RANKX(
    ALL(DimAgent),
    [Total Tickets],
    ,
    DESC,
    DENSE
)
```

**Descripción:** Posición del agente según la cantidad de tickets gestionados, de mayor a menor.

#### Satisfaction Rank

```DAX
Satisfaction Rank =
RANKX(
    ALL(DimAgent),
    [Average Satisfaction],
    ,
    DESC,
    DENSE
)
```

**Descripción:** Posición del agente según su promedio de satisfacción, de mayor a menor.

#### Resolution Rank

```DAX
Resolution Rank =
RANKX(
    ALL(DimAgent),
    [Average Resolution Duration],
    ,
    ASC,
    DENSE
)
```

**Descripción:** Posición del agente según su promedio de días de resolución, de menor a mayor.

#### Total Rank

```DAX
Total Rank =
[Ticket Rank]
    + [Satisfaction Rank]
    + [Resolution Rank]
```

**Descripción:** Puntaje total de ranking obtenido mediante la suma de las posiciones del agente en volumen de tickets, satisfacción y tiempo de resolución.

#### Final Rank

```DAX
Final Rank =
RANKX(
    ALL(DimAgent),
    [Total Rank],
    ,
    ASC,
    DENSE
)
```

**Descripción:** Posición final del agente según su puntaje total de desempeño; una posición menor representa un mejor resultado.

---

## Resumen de organización

| Display Folder | Medidas |
|---|---|
| `01. Tickets\Volume` | Total Tickets, Total Agents, Total Tickets All, Total Tickets Selected |
| `01. Tickets\Distribution` | Positive Tickets, Positive Ticket %, Problem Tickets, Problem Ticket % |
| `01. Tickets\Productivity` | Tickets per Agent |
| `02. Satisfaction` | Average Satisfaction |
| `03. Performance\Metrics` | Average Resolution Duration, Selected Tickets per Agent, Ticket Variance per Agent, Ticket Performance Group |
| `03. Performance\Ranking` | Ticket Rank, Satisfaction Rank, Resolution Rank, Total Rank, Final Rank |

**Total: 20 medidas.**
