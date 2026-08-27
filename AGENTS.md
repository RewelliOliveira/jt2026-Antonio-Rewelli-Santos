# AGENTS.md

## Objetivo

Produzir uma recomendação de investimento imobiliário defensável para a Seazone em Itapema (SC), baseada nos dados do repositório. A entrega deve responder: melhor perfil de imóvel, melhor localização em receita, fatores associados às melhores receitas e o que comprar com uma estimativa simples de retorno. Sempre concluir se os dados sustentam ou refutam a tese de compactos (studio/1 quarto) no Centro.

## Fontes e estrutura

- Trate `index.html` como enunciado de referência e `data/` como dados brutos imutáveis.
- Prefira os CSVs locais a downloads remotos para garantir reprodutibilidade.
- Use `notebooks/` para análises, `outputs/` para tabelas e gráficos finais, `references/` para material de apoio e `ai-log/` para sessões completas de IA em texto.
- Crie e mantenha todos os notebooks exclusivamente em `notebooks/`, usando somente caminhos relativos à raiz.
- Mantenha o `README.md` como porta de entrada da entrega: primeira linha com o vídeo, instruções de execução e localização da recomendação final.

## Regras de análise

- Execute tudo a partir da raiz e registre dependências, parâmetros e hipóteses necessárias para reproduzir os resultados.
- Antes de modelar, valide tipos, chaves, cardinalidade dos joins, duplicatas, ausências, datas de aquisição, unidades e outliers.
- Não trate preço anunciado de diária como receita realizada, nem preço de venda anunciado como transação. Identifique proxies e separe claramente fatos observados, estimativas e hipóteses.
- Toda conclusão deve informar métrica, período, unidade, tamanho da amostra e limitações. Para retorno, explicite fórmula, custos e cenários/sensibilidade; não invente ocupação sem justificativa.
- Compare alternativas relevantes e investigue evidência contrária, especialmente ao testar a tese dos compactos no Centro.

## Convenções de trabalho

- Escreva documentação e conclusões em português do Brasil, com nomes de variáveis claros e gráficos legíveis.
- Nunca altere, sobrescreva, renomeie ou gere arquivos dentro de `data/`. Trate a pasta como somente leitura; toda transformação deve ser reproduzível e toda saída derivada deve ficar em `outputs/`.
- Exporte sessões inteiras de IA para `ai-log/` em `.md`, `.txt` ou `.json`; não use capturas de tela nem selecione apenas trechos favoráveis.
- Antes de finalizar mudanças, execute as análises afetadas, confira caminhos relativos e registre no `README.md` como reproduzi-las.

Link do desafio: https://seazone-tech.github.io/jovens-talentos-2026-hackathon-data/

Link da base de dados: https://github.com/seazone-tech/jovens-talentos-2026-hackathon-data
