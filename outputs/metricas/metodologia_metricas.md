# Metodologia de métricas e cenários

## Vocabulário

Os valores de `Price_AV` são **preços anunciados de diária**. Não representam reservas, ocupação ou receita realizada. Resultados anualizados são chamados de **potencial bruto anualizado**.

## Universo válido

- Entrada auditada: 59,040 combinações de anúncio/data.
- Saída válida: 58,531 combinações de anúncio/data.
- Anúncios com algum preço válido: 999 de 4,441.
- Elegíveis com pelo menos 7 datas: 991; com 14 datas: 959; com 30 datas: 859.
- Período das estadias: 2025-01-07 a 2025-04-20.
- Período das capturas utilizadas: 2025-01-06 13:22:07 a 2025-01-20 15:07:47.

### Funil de filtros

- `entrada_auditada`: 59,040 linha(s) após a regra; 0 removida(s) nesta etapa.
- `chave_e_data_validas`: 59,040 linha(s) após a regra; 0 removida(s) nesta etapa.
- `preco_anunciado_positivo`: 59,040 linha(s) após a regra; 0 removida(s) nesta etapa.
- `captura_nao_posterior_a_estadia`: 58,971 linha(s) após a regra; 69 removida(s) nesta etapa.
- `id_existente_em_airbnb`: 58,531 linha(s) após a regra; 440 removida(s) nesta etapa.
- Outliers não foram removidos; a mediana reduz sua influência.

## Métrica comparável

A métrica primária por anúncio é `diaria_mediana_anunciada`. Segmentos serão comparados pela mediana dessas medianas, para que anúncios com mais datas não recebam peso maior. Sempre devem acompanhar a métrica: número de anúncios, percentis 25/75 e cobertura da amostra.

O corte principal exige 14 datas válidas por anúncio. As flags de 7 e 30 datas permitem testar sensibilidade sem excluir registros da base-mãe.

## Cenários de ocupação

As ocupações de 40%, 55% e 70% são hipóteses de sensibilidade, não estimativas observadas em Itapema.

```text
noites_ocupadas_assumidas = 365 × ocupacao_assumida
potencial_bruto_anualizado = diaria_mediana_anunciada × noites_ocupadas_assumidas
```

A base cobre estadias somente entre 2025-01-07 e 2025-04-20; multiplicar sua mediana por um ano é uma extrapolação e não captura toda a sazonalidade.

## Fórmulas para a etapa de retorno

```text
investimento_total = preco_compra + custos_aquisicao + mobilia + reforma
custos_operacionais_anuais = custos_variaveis + condominio_anual + IPTU + manutencao + demais_custos_fixos
potencial_liquido_anual = potencial_bruto_anualizado - custos_operacionais_anuais
retorno_liquido = potencial_liquido_anual / investimento_total
payback = investimento_total / potencial_liquido_anual
```

Plataforma, administração, manutenção, condomínio, IPTU, mobília, reforma e custos de aquisição terão entradas separadas. Nenhum valor foi preenchido nesta etapa sem fonte ou hipótese aprovada.

## Limitações

- A cobertura de preços é parcial e pode não ser aleatória.
- Preço anunciado pode diferir do preço efetivamente pago.
- Não há ocupação observada, reservas ou receita realizada.
- Cenários não substituem validação de mercado nem análise completa de sazonalidade.
