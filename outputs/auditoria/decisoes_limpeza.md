# Decisões de preparação dos dados

## Princípios

- Os cinco CSVs de `data/` são somente leitura e tiveram seus hashes verificados antes e depois da auditoria.
- Conversões inválidas viram valores ausentes e são contabilizadas em `qualidade_dados.csv`.
- Campos textuais têm whitespace consecutivo normalizado para um espaço; o conteúdo lexical é preservado.
- Outliers são sinalizados; não são excluídos automaticamente nesta etapa.
- O bairro original é preservado; `suburb_key` remove acentos, espaços e pontuação para expor variações técnicas sem impor equivalências semânticas.
- Preço de diária e preço de venda continuam classificados como valores anunciados, não receita ou transação realizada.

## Chaves e snapshots

- Details: snapshot mais recente por `airbnb_listing_id`; 0 linha(s) sem a chave ficaram fora da base derivada.
- Hosts: snapshot mais recente por `owner_id`.
- Mesh: snapshot mais recente por `airbnb_listing_id`.
- VivaReal: snapshot mais recente por `listing_id`; 0 linha(s) sem a chave ficaram fora da base derivada.
- Price_AV: captura mais recente por `airbnb_listing_id` e `date`; 0 linha(s) sem chave/data ficaram fora da base temporal.
- A quantidade, a primeira e a última captura de cada diária foram preservadas em colunas auxiliares.
- Details possui 4441 anúncio(s) com latitude/longitude zeradas; as colunas analíticas de coordenadas usam Mesh, que tem cobertura completa após o join.

## Joins

- Details é a base-mãe do Airbnb. Mesh entra por `airbnb_listing_id` com cardinalidade 1:1 após snapshots.
- Hosts entra por `owner_id` com cardinalidade N:1 após snapshots.
- Price_AV permanece em base temporal separada e sua cobertura é registrada.
- VivaReal permanece separado; não existe join direto confiável por identificador com Airbnb.
- Joins são à esquerda para não excluir anúncios silenciosamente; flags indicam disponibilidade das fontes auxiliares.

## Limites para as próximas etapas

- Registros com preços inválidos permanecem nas bases com flags e deverão ser filtrados explicitamente ao calcular métricas.
- Nenhuma hipótese de ocupação foi criada.
- Nenhuma inferência causal ou recomendação de investimento foi realizada.
