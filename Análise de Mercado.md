# Análise de Mercado - Segmentação RFM com Clustering

<hr>

## Objetivos da Análise

#### Problema de Negócio:

A empresa precisava entender melhor o comportamento de seus clientes para aprimorar estratégias de marketing, fidelização e retenção. Havia baixa personalização nas campanhas, dificultando a identificação de clientes de alto valor ou em risco de abandono.

#### Perguntas-Chave:

- Quem são nossos clientes mais valiosos?
- Quais clientes estão em risco de churn?
- Como segmentar nossa base para ações de marketing personalizadas?
- Quais padrões de compra existem?

<hr>

##  Indicadores-Chave de Desempenho (KPIs)

#### Recência (R)

- Dias desde a última compra do cliente.
- Quanto menor, mais recente é o cliente – sinal de engajamento atual.

#### Frequência (F)

- Quantidade de compras realizadas.
- Alta frequência indica clientes leais.

#### Valor Monetário (M)

- Soma total gasta pelo cliente.
- Reflete o valor financeiro agregado por cada cliente.

Esses KPIs foram normalizados e usados tanto para calcular o score do RFM quanto para a clusterização.

<hr>

## Interpretação das Visualizações

#### Distribuição das Métricas RFM

- <b>Histograma de Recência:</b> Clientes recentes são minoria, indicando que tem clientes  desengajados.
- <b>Frequência:</b> Muitos clientes compraram apenas 1 vez.
- <b>Valor Monetário:</b> Distribuição assimétrica, com poucos clientes de alto valor.

#### Gráfico de Dispersão RFM

Mostra agrupamentos naturais de clientes por valor, recência e frequência.

####  Método Elbow

- Indica o número ótimo de clusters ao observar a queda acentuada. Melhor valor de k encontrado: **4**.

####  Silhouette Score

Métrica de coesão dos clusters. Score de: aproximadamente 0.45 isso indica que tem agrupamentos razoavelmente bem definidos.

####  Gráfico de Clusters (2D)

<b>Segmentação clara de grupos com perfis distintos:</b>

- Clientes com alta frequência e valor.
- Clientes inativos de alto valor.
- Clientes novos, porém promissores.
- Clientes com baixo valor e engajamento.

#### Heatmap de Score RFM

- Visualiza a densidade dos clientes por score R, F e M – destacando os segmentos "Campeões" e "Em risco".

<hr>

## Insights

- <b>Campeões:</b> Clientes que compraram recentemente, com alta frequência e valor. Devem ser recompensados e usados como referência.
- <b>Clientes Fiéis:</b> Compram com frequência, mas precisam de incentivo para aumentar o preço médio.
- <b>Em Risco:</b> Clientes que gastaram muito, mas estão inativos. Ações de reengajamento precisam ser urgentes.
- <b>Recém-Chegados:</b> Potencial de fidelização.
- <b>Clientes de Baixo Valor:</b> Segmento para campanhas automatizadas ou apenas descontinuar.

<hr>

## Análise Segmentada

#### Cluster 0 – Campeões

- Alta frequência, baixo tempo desde última compra, alto gasto.
- Recomendação: Ofertas VIP, benefícios exclusivos.

#### Cluster 1 – Clientes Fiéis

- Compram frequentemente, mas com valor médio mais baixo.
- Recomendação: Upsell, bundles promocionais.

#### Cluster 2 – Em Risco

- Alto valor gasto no passado, mas alta recência.
- Recomendação: Reativação com cupons e incentivos urgentes.

#### Cluster 3 – Clientes de Baixo Engajamento

- Compras que não são frequentes e de baixo valor.
- Recomendação: Automatizar ou reduzir custos com esse público.

<hr>

## Recomendações que eu Sugiro

- Automatizar Segmentação de RFM mensalmente.
- Personalizar campanhas por cluster.
- Acompanhar KPIs de reengajamento (email, recompra, valor médio).
- Criar campanhas exclusivas para "Campeões" e "Em Risco".
- Implementar CRM com integração para classificação automática via API.

<hr>

## Resumo 

Esta análise segmentou a base de clientes em grupos com perfis distintos, utilizando o modelo RFM e clustering não supervisionado. Isso permitiu identificar claramente os clientes mais valiosos, os que estão em risco e aqueles que exigem menor esforço de marketing. As recomendações aqui listadas possuem alto potencial de aumentar a retenção, maximizar o retorno sobre campanhas e otimizar o relacionamento com o cliente.


