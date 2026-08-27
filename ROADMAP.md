# ROADMAP — Hackathon Jovens Talentos AI Builder 2026

## Resultado esperado

Entregar uma recomendação de investimento clara, reproduzível e sustentada pelos dados de Itapema, respondendo:

1. Qual é o melhor perfil de imóvel (tipologia, quartos e tipo de anúncio)?
2. Qual é a melhor localização em potencial de receita?
3. Quais características estão associadas às melhores receitas?
4. O que a Seazone deveria comprar hoje, por quê e com qual retorno estimado?
5. Os dados sustentam ou refutam a tese de studios/1 quarto no Centro?

A definição de sucesso inclui também: repositório público compreensível, análise reproduzível, sessões completas em `ai-log/`, recomendação escrita e vídeo de até 3 minutos com link público na primeira linha do `README.md`.

## Regra inegociável sobre os dados

`data/` é a fonte bruta e deve permanecer imutável durante todo o projeto. Nenhum notebook, script ou processo pode alterar, sobrescrever, renomear ou criar arquivos nessa pasta. Toda análise deve apenas ler os CSVs locais por caminhos relativos; dados tratados, tabelas, modelos e gráficos devem ser gravados em `outputs/`.

## Prioridades

Seguir a ponderação do desafio ao administrar o tempo:

- **45% — análise:** critérios coerentes, evidências, limitações e decisão defensável.
- **30% — uso de IA:** processo completo, iterativo e crítico registrado em texto.
- **25% — comunicação:** síntese, clareza e defesa da recomendação.

## Etapa 0 — Preparar um fluxo reproduzível

- [x] Ler o enunciado e transformar as cinco perguntas acima em critérios de aceite.
- [x] Usar caminhos relativos à raiz e os CSVs locais de `data/`, sem depender de URLs.
- [x] Definir e registrar ambiente, versões e dependências necessárias.
- [x] Manter todos os notebooks do projeto exclusivamente em `notebooks/`.
- [x] Planejar notebooks numerados, por exemplo: `01_auditoria`, `02_airbnb`, `03_vendas`, `04_recomendacao`.
- [x] Definir saídas finais em `outputs/`: tabelas, gráficos e resultados consolidados.
- [x] Confirmar por hashes e pelo diff do Git que `data/` permaneceu inalterada.

**Concluída quando:** outra pessoa consegue iniciar a análise a partir da raiz sem downloads ou caminhos pessoais.

### Ambiente e inicialização

O ambiente foi verificado com Python 3.14.5. A partir da raiz, use:

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m jupyter lab
```

O primeiro ponto de entrada é `notebooks/00_setup.ipynb`. Ele apenas valida o ambiente e lê amostras dos cinco CSVs locais; não grava em `data/` nem inicia a auditoria analítica.

## Etapa 1 — Auditar os dados antes de analisar

- [x] Carregar os cinco CSVs com codificação, tipos e datas consistentes.
- [x] Produzir um inventário: linhas, colunas, período, nulos, duplicatas e valores inválidos.
- [x] Verificar granularidade e snapshots usando `aquisition_date`.
- [x] Validar chaves e cardinalidade antes dos joins:
  - `Details.airbnb_listing_id` com `Price_AV` e `Mesh_Ids_Data`;
  - `Details.owner_id` com `Hosts_ids.owner_id`;
  - `VivaReal` como mercado de venda separado, comparável por bairro e perfil, sem join direto com Airbnb.
- [x] Tratar repetições de captura em `Price_AV` sem contar a mesma diária mais de uma vez.
- [x] Examinar preços, quartos, coordenadas, bairros e avaliações para ausências e outliers.
- [x] Registrar decisões de limpeza sem alterar ou gerar arquivos dentro de `data/`.

**Saída:** tabela de qualidade dos dados e base analítica com uma linha por anúncio/perfil, acompanhada de métricas temporais de preço.

**Concluída quando:** cada coluna usada na recomendação tem significado, unidade, período e regra de tratamento documentados.

## Etapa 2 — Definir métricas e hipóteses

- [x] Definir uma proxy de diária média/mediana por anúncio, controlando período e data de captura.
- [x] Não chamar preço ofertado de receita realizada. Usar termos como **diária anunciada** e **potencial de receita**.
- [x] Escolher uma métrica comparável entre segmentos, exibindo também tamanho de amostra e dispersão.
- [x] Definir cenários conservador, base e otimista de ocupação; identificar ocupação como hipótese externa, não como fato da base.
- [x] Formalizar o retorno:

```text
receita_bruta_anual = diária_representativa × 365 × ocupação_assumida
receita_líquida_anual = receita_bruta_anual − custos_operacionais
retorno_líquido = receita_líquida_anual ÷ investimento_total
payback = investimento_total ÷ receita_líquida_anual
```

- [x] Explicitar compra, condomínio, IPTU, manutenção, administração, plataforma, mobília e demais custos incluídos ou omitidos.

**Concluída quando:** todas as fórmulas, proxies e hipóteses podem ser auditadas e recalculadas.

## Etapa 3 — Responder às perguntas analíticas

### 3.1 Melhor perfil

- [x] Comparar tipo de imóvel, tipo de anúncio, número de quartos e capacidade.
- [x] Mostrar diária/potencial de receita, dispersão e amostra por segmento.
- [x] Evitar eleger grupos pequenos ou dominados por outliers sem ressalvas.

### 3.2 Melhor localização

- [x] Padronizar bairros e validar coordenadas.
- [x] Comparar bairros de forma geral e dentro de perfis equivalentes.
- [x] Separar efeito de localização de diferenças de quartos, capacidade e tipo de imóvel.

### 3.3 Fatores associados ao desempenho

- [x] Investigar quartos, capacidade, tipo, avaliações, superhost, resposta, comodidades e localização.
- [x] Usar comparações segmentadas e, se útil, modelo explicativo simples e interpretável.
- [x] Tratar associação como associação; não afirmar causalidade sem desenho que a sustente.

**Saídas:** poucas tabelas e gráficos decisivos, cada um com título conclusivo, unidade, período, amostra e fonte.

## Etapa 4 — Testar explicitamente a tese do Centro

- [ ] Definir “compacto” antes do teste: studio ou imóvel com até 1 quarto.
- [ ] Comparar compactos do Centro com:
  - compactos de outros bairros;
  - imóveis maiores no Centro;
  - demais combinações com amostra suficiente.
- [ ] Avaliar diária, potencial de receita, preço de compra, retorno e sensibilidade — não apenas uma métrica isolada.
- [ ] Procurar evidências que contradigam a hipótese inicial.
- [ ] Dar um veredito direto: **sustentada**, **parcialmente sustentada** ou **refutada**, com condições e limitações.

**Concluída quando:** a tese recebe resposta inequívoca, apoiada por comparações justas.

## Etapa 5 — Construir a recomendação de compra e retorno

- [ ] Traduzir o segmento vencedor do Airbnb para imóveis comparáveis do VivaReal por bairro, tipologia e quartos.
- [ ] Usar preço de venda anunciado como referência de aquisição, deixando essa limitação explícita.
- [ ] Selecionar um perfil de compra principal e, no máximo, uma alternativa.
- [ ] Calcular investimento, receita potencial, retorno líquido e payback nos três cenários.
- [ ] Fazer sensibilidade para ocupação, diária, preço de compra e custos.
- [ ] Listar riscos: sazonalidade, vacância, qualidade dos anúncios, liquidez, regulação, condomínio e diferença entre oferta e transação.
- [ ] Encerrar com uma decisão executiva: **o que comprar, onde, faixa de preço e condições que fariam a decisão mudar**.

**Concluída quando:** um decisor consegue aprovar ou rejeitar a compra conhecendo evidências, premissas e riscos.

## Etapa 6 — Montar a entrega

- [ ] Salvar gráficos e tabelas finais em `outputs/`, sem depender de células executadas fora de ordem.
- [ ] Escrever a recomendação final no `README.md` ou em relatório claramente apontado por ele.
- [ ] Atualizar o `README.md` com:
  - link público do vídeo na primeira linha;
  - resumo executivo e posição sobre a tese;
  - estrutura do projeto;
  - instalação e execução;
  - fontes, metodologia, hipóteses e limitações.
- [ ] Exportar todas as sessões completas de IA para `ai-log/` em formato textual.
- [ ] Preparar vídeo de até 3 minutos cobrindo recomendação, raciocínio, uso da IA e próximos passos com mais uma semana.

## Etapa 7 — Controle de qualidade e envio

- [ ] Reiniciar o ambiente e executar a análise inteira na ordem documentada.
- [ ] Conferir se tabelas, gráficos e números do texto vêm da versão final executada.
- [ ] Revisar unidades, amostras, períodos, arredondamentos e coerência entre conclusão e evidência.
- [ ] Confirmar que dados brutos e `index.html` não foram modificados.
- [ ] Verificar que nenhum segredo, caminho local ou dado temporário foi versionado.
- [ ] Abrir repositório e vídeo em janela anônima; ambos devem estar públicos.
- [ ] Confirmar que o vídeo tem até 3 minutos e que seu link está na primeira linha do `README.md`.
- [ ] Revisar o repositório como avaliador: a resposta e a forma de reprodução devem ser encontradas em menos de dois minutos.
- [ ] Enviar uma única vez pelo formulário, dentro do prazo, e manter o repositório público até a data exigida.

## Sequência sugerida para um dia

| Bloco | Foco | Marco de saída |
|---|---|---|
| 1 | Setup e auditoria | Dados carregados e problemas conhecidos |
| 2 | Base analítica e métricas | Joins validados e métricas definidas |
| 3 | Perfil, localização e fatores | Três perguntas respondidas |
| 4 | Tese e retorno | Veredito e cenários calculados |
| 5 | Recomendação e visuais | História executiva fechada |
| 6 | README, vídeo e QA | Entrega pública, reproduzível e verificada |

Se o tempo apertar, priorizar uma recomendação simples e auditável com boas limitações, em vez de ampliar a análise sem conseguir validá-la.

## Checklist de conclusão do case

- [ ] As quatro perguntas da missão estão respondidas com números.
- [ ] A tese de compactos no Centro tem veredito explícito.
- [ ] A recomendação identifica perfil, localização, preço e retorno esperado.
- [ ] Proxies, hipóteses, custos, incertezas e riscos estão visíveis.
- [ ] A análise roda a partir da raiz usando os dados locais.
- [ ] O `README.md` explica como reproduzir e encontrar a resposta.
- [ ] O `ai-log/` contém as conversas completas em texto.
- [ ] O vídeo público está na primeira linha do `README.md` e tem até 3 minutos.
- [ ] Repositório e vídeo foram testados sem autenticação antes do envio.
