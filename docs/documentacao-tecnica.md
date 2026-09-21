# Documentação técnica resumida

## Objetivo

Transformar dados transacionais em informações acionáveis para apoiar decisões de vendas, distribuição de produtos e marketing.

## Estrutura dos dados

O modelo foi organizado em lógica próxima ao modelo estrela:

- `fat_vendas`: transações de venda;
- `fat_vendas_produtos`: itens de cada venda;
- `dim_clientes`: cadastro e localização dos clientes;
- `dim_filiais`: cadastro das filiais;
- `dim_produtos`: produtos e preço de tabela;
- `dim_calendario`: dimensão temporal criada no Power BI.

Relacionamentos principais:
- calendário 1:* vendas;
- clientes 1:* vendas;
- filiais 1:* vendas;
- vendas 1:* itens de venda;
- produtos 1:* itens de venda.

## Indicadores analisados

- Faturamento
- Nº de vendas
- Nº de clientes
- Ticket médio
- Itens vendidos
- Compras por cliente
- Faturamento por cliente
- Itens por venda
- Desconto médio

## Segmentação

A segmentação comportamental foi construída a partir de:

- frequência de compra;
- valor acumulado por cliente.

A distribuição foi analisada por quartis e a mediana foi utilizada como ponto de corte operacional.

Resultados:
- Q1 de frequência: 2 compras
- Mediana de frequência: 3 compras
- Q3 de frequência: 4 compras
- Q1 de valor: R$ 495,00
- Mediana de valor: R$ 1.078,10
- Q3 de valor: R$ 2.066,60

Perfis:
- Alto Valor
- Recorrente
- Alto Ticket
- Ocasional

## Principais achados

- Faturamento total: R$ 4,40 milhões
- 10.000 vendas
- 2.625 clientes
- Ticket médio: R$ 439,81
- 29.939 itens vendidos
- Batel lidera em faturamento
- Água Verde lidera em recorrência
- Cabral apresenta o maior ticket médio
- Curitiba concentra aproximadamente 77,2% da base de clientes
- Alto Valor representa 40,3% da base e 75,5% da receita

## Limitação promocional

O desconto foi estimado pela diferença entre preço de tabela e preço praticado.

Como a base não identifica campanhas específicas, exposição à comunicação ou grupos de controle, não é possível afirmar causalidade promocional. As recomendações devem ser interpretadas como hipóteses de teste.

## Recomendações

- Reter e proteger clientes de Alto Valor
- Estimular segunda compra dos Ocasionais
- Aumentar cesta média dos Recorrentes
- Estimular recompra dos clientes de Alto Ticket
- Orientar portfólio por perfil e desempenho da filial
- Testar promoções segmentadas e acompanhar resultado
