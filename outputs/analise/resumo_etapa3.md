# Resultados da Etapa 3 — Airbnb em Itapema

## Escopo e métrica

- Universo: 4,441 anúncios; amostra principal: 959 anúncios com 14+ datas válidas.
- Estadias observadas: 2025-01-07 a 2025-04-20; capturas: 2025-01-06 a 2025-01-20.
- Métrica: mediana, entre anúncios, da diária mediana anunciada por anúncio, em R$/noite.
- O potencial bruto anualizado usa 365 dias e ocupação hipotética de 55%; não é receita realizada.
- Grupos com 20+ anúncios são ranqueáveis; 10–19 são exploratórios; menos de 10 não são ranqueados.

## Perfil

Entre os perfis combinados com amostra suficiente, **apartamento | 4+ quarto(s) | 7+ hóspede(s)** apresenta a maior diária mediana anunciada: **R$ 988/noite** (n=70, P25–P75 de R$ 800 a R$ 1,596). O potencial bruto anualizado no cenário-base é R$ 198,241, condicionado à ocupação assumida de 55%.

Segmentos pequenos não foram eleitos como vencedores. `listing_type` é a variável disponível para representar tipo de imóvel/anúncio; a base não fornece uma segunda classificação independente de tipologia.

## Localização

Na comparação bruta dos bairros ranqueáveis, **Meia Praia** tem a maior mediana, R$ 589/noite (n=607).

Na comparação padronizada entre apartamentos de 2 e 3 quartos — os estratos com pelo menos 10 observações em todos os bairros ranqueáveis — **Centro** lidera com diária padronizada de R$ 669/noite. Essa padronização reduz diferenças de composição, mas não identifica efeito causal de localização. O modelo multivariado adiciona controles de capacidade, banheiros, tipo, avaliações, anfitrião e comodidades.

As coordenadas analíticas dos 959 anúncios elegíveis foram validadas e vêm do Mesh, pois as coordenadas originais de Details eram inutilizáveis. Bairros abaixo do mínimo permanecem visíveis, sem ranking.

## Fatores associados

O modelo OLS usa `log(diária mediana anunciada)` e erros robustos HC3, com n=959 e R²=0.552. Condicionadas às demais variáveis, a maior associação positiva com intervalo de 95% que não cruza zero é **quartos faixa 4+** (+70.3%; IC95% +49.6% a +93.9%). A associação negativa de maior magnitude nas mesmas condições é **tipo agrupado outros** (-34.7%; IC95% -47.8% a -18.4%).

Taxa e tempo de resposta não puderam ser analisados: `response_rate_shown` e `response_time_shown` não possuem observações válidas na amostra. Comodidades quase universais têm pouco poder de comparação; as demais foram avaliadas como indicadores de presença no texto coletado.

Os coeficientes representam **associações condicionais**, não causalidade, e podem refletir qualidade não observada, padrão construtivo, vista, proximidade da praia e seleção dos anúncios com preços disponíveis.

## Outliers e sensibilidade

A amostra principal preserva todos os anúncios. A dupla mediana limita a influência de extremos. Como diagnóstico, P1–P99 corresponde a R$ 153–R$ 2,271/noite; anúncios fora desses limites foram retirados apenas da análise de sensibilidade.

- **tipo_anuncio:** líderes 7/14/30 datas e P1–P99 = apartamento / apartamento / apartamento / apartamento — estável.
- **numero_quartos:** líderes 7/14/30 datas e P1–P99 = 4+ / 4+ / 4+ / 4+ — estável.
- **capacidade_hospedes:** líderes 7/14/30 datas e P1–P99 = 7+ / 7+ / 7+ / 7+ — estável.
- **perfil_combinado:** líderes 7/14/30 datas e P1–P99 = apartamento | 4+ quarto(s) | 7+ hóspede(s) / apartamento | 4+ quarto(s) | 7+ hóspede(s) / apartamento | 4+ quarto(s) | 7+ hóspede(s) / apartamento | 4+ quarto(s) | 7+ hóspede(s) — estável.
- **bairro:** líderes 7/14/30 datas e P1–P99 = meiapraia / meiapraia / meiapraia / meiapraia — estável.

## Limitações e fronteira da decisão

- A cobertura de preços é parcial (959 de 4,441, ou 21.6%) e pode não ser aleatória.
- Preços são anunciados, não reservas nem receitas realizadas; não há ocupação observada.
- O período não cobre um ciclo anual completo, portanto o potencial anualizado não captura toda a sazonalidade.
- Avaliações, selos e comodidades podem ser consequência ou proxy de fatores não observados.
- Esta etapa compara perfil, localização e fatores. O teste formal dos compactos no Centro e o veredito da tese pertencem à Etapa 4.
