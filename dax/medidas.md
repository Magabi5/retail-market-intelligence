# Medidas DAX principais

Este arquivo reúne as medidas utilizadas no projeto para análise de vendas, clientes, produtos e segmentação.

## Indicadores gerais

```DAX
Faturamento =
SUM(fat_vendas[VALOR_VENDA])
```

```DAX
Nº Vendas =
DISTINCTCOUNT(fat_vendas[ID_VENDA])
```

```DAX
Nº de clientes =
DISTINCTCOUNT(fat_vendas[ID_CLIENTE])
```

```DAX
Ticket Médio =
DIVIDE([Faturamento], [Nº Vendas])
```

```DAX
Itens Vendidos =
SUM(fat_vendas_produtos[QUANTIDADE])
```

```DAX
Faturamento Produtos =
SUM(fat_vendas_produtos[VALOR_VENDA_PRODUTO])
```

## Desconto

```DAX
Valor Preço Cheio =
SUMX(
    fat_vendas_produtos,
    fat_vendas_produtos[QUANTIDADE] *
    RELATED(dim_produtos[PRECO_TABELA])
)
```

```DAX
Valor Desconto =
[Valor Preço Cheio] - [Faturamento Produtos]
```

```DAX
Desconto Médio % =
DIVIDE([Valor Desconto], [Valor Preço Cheio])
```

## Comportamento do cliente

```DAX
Frequência Média =
DIVIDE([Nº Vendas], [Nº de clientes])
```

```DAX
Faturamento por Cliente =
DIVIDE([Faturamento], [Nº de clientes])
```

```DAX
Itens por Venda =
DIVIDE([Itens Vendidos], [Nº Vendas])
```

```DAX
Compras por Cliente =
DIVIDE([Nº Vendas], [Nº de clientes])
```

## Participação de produto

```DAX
Participação Produto % =
DIVIDE(
    [Faturamento Produtos],
    CALCULATE(
        [Faturamento Produtos],
        REMOVEFILTERS(dim_produtos)
    )
)
```

## Segmentação

Tabela auxiliar criada para consolidar frequência e valor por cliente:

```DAX
Perfil_Clientes =
SUMMARIZE(
    fat_vendas,
    fat_vendas[ID_CLIENTE],
    "Qtd_Compras", DISTINCTCOUNT(fat_vendas[ID_VENDA]),
    "Valor_Gasto", SUM(fat_vendas[VALOR_VENDA])
)
```

Quartis utilizados na análise:

```DAX
Q1 Frequência =
PERCENTILEX.INC(Perfil_Clientes, Perfil_Clientes[Qtd_Compras], 0.25)

Mediana Frequência =
PERCENTILEX.INC(Perfil_Clientes, Perfil_Clientes[Qtd_Compras], 0.50)

Q3 Frequência =
PERCENTILEX.INC(Perfil_Clientes, Perfil_Clientes[Qtd_Compras], 0.75)

Q1 Valor =
PERCENTILEX.INC(Perfil_Clientes, Perfil_Clientes[Valor_Gasto], 0.25)

Mediana Valor =
PERCENTILEX.INC(Perfil_Clientes, Perfil_Clientes[Valor_Gasto], 0.50)

Q3 Valor =
PERCENTILEX.INC(Perfil_Clientes, Perfil_Clientes[Valor_Gasto], 0.75)
```

Classificação final:

```DAX
Segmento =
SWITCH(
    TRUE(),
    Perfil_Clientes[Qtd_Compras] >= 3 &&
        Perfil_Clientes[Valor_Gasto] >= 1078.1, "Alto Valor",
    Perfil_Clientes[Qtd_Compras] >= 3 &&
        Perfil_Clientes[Valor_Gasto] < 1078.1, "Recorrente",
    Perfil_Clientes[Qtd_Compras] < 3 &&
        Perfil_Clientes[Valor_Gasto] >= 1078.1, "Alto Ticket",
    "Ocasional"
)
```

Para levar a classificação para a dimensão de clientes:

```DAX
Segmento =
LOOKUPVALUE(
    Perfil_Clientes[Segmento],
    Perfil_Clientes[ID_CLIENTE],
    dim_clientes[ID_CLIENTE]
)
```

## Observação

Os cortes da segmentação foram definidos com base na distribuição da base analisada:
- Mediana de frequência: 3 compras
- Mediana de valor acumulado: R$ 1.078,10
