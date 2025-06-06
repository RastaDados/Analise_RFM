## O que é RFM?

O modelo RFM é uma técnica clássica de segmentação de clientes com base no comportamento de compra. Ele analisa 3 dimensões principais:

<table>
  <thead>
    <tr>
      <th>Sigla</th>
      <th>Nome</th>
      <th>Significado</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>R</td>
      <td>Recência</td>
      <td>Quantos dias se passaram desde a última compra do cliente. Quanto menor, melhor (o cliente está mais ativo).</td>
    </tr>
    <tr>
      <td>F</td>
      <td>Frequência</td>
      <td>Quantas vezes o cliente comprou no período analisado. Quanto maior, melhor.</td>
    </tr>
    <tr>
      <td>M</td>
      <td>Valor Monetário</td>
      <td>Quanto dinheiro o cliente gastou no total. Quanto maior, mais valioso ele é.</td>
    </tr>
  </tbody>
</table>

<hr>

## Objetivo do RFM:

- Identificar os melhores clientes, os que estão em risco, os que compraram uma vez e sumiram, etc.

<hr>

## Como calculei o RFM?

- Agrupei os dados pela coluna Customer ID.

Calculei:

- <b>Recência:</b> Hoje - Data da última compra
- <b>Frequência:</b> Quantidade de pedidos únicos
- <b>Valor Monetário:</b> Soma do valor total gasto

<hr>

## O que são Scores RFM?

Depois de calcular os valores brutos de R, F e M, criei pontuações de 1 a 5 para cada cliente, onde:

- 5 = Excelente em Recência/Frequência/Valor

- 1 = Ruim

<b>Exemplo:</b>

- Um cliente com R = 5, F = 5, M = 5 é um campeão.

- Um cliente com R = 1, F = 1, M = 1 é um cliente perdido.

<hr>

## O que é Clustering?

Clustering é um tipo de aprendizado de máquina não supervisionado que serve para agrupar elementos semelhantes. No meu caso, quero agrupar clientes com comportamento parecido com base nas métricas RFM.

<hr>

## O que é o algoritmo K-Means?

- O K-Means é um dos métodos mais usados de clustering. Ele:

- Escolhe K grupos (número de clusters).

- Agrupa os dados em torno de centros (médias).

- Repete até que os grupos fiquem estáveis.

#### Como saber qual é o melhor número de grupos (K)?

Usei duas técnicas:

#### Método Elbow 

- Testa vários valores de K e calcula o WSS (within-cluster sum of squares).

- Quanto menor o WSS, melhor. Mas chega um ponto que o ganho é pequeno.

- Esse ponto é o "cotovelo" que é a melhor escolha de K.

<b>Exemplo: </b>Imagine colocar roupa em malas. Se você usar muitas malas (K alto), as roupas cabem melhor. Mas usar 100 malas é um exagero, certo? O cotovelo mostra o equilíbrio ideal.

<hr>

## Silhouette Score

- Mede o quão bem os clientes estão agrupados.

- Varia de -1 a 1. Quanto mais próximo de 1, melhor o agrupamento.

<b>Exemplo: </b> Imagine grupos de amigos. Um Silhouette alto quer dizer que cada grupo está bem separado dos outros e tem pessoas parecidas entre si.

<hr>

## O que fiz com os clusters?

#### Depois de agrupar os clientes com K-Means (com base nos scores normalizados de R, F, M):

- Visualizei em 2D  os clusters.

#### Interpreteicada grupo, por exemplo:

- Cluster 0: Clientes fiéis, compram sempre.
- Cluster 2: Clientes que sumiram.
- Cluster 4: Compraram muito uma vez só.

<hr>

## Outras visualizações importantes:

- Distribuição de RFM por cluster.
- Gráficos de dispersão (scatter plot) para ver o comportamento entre recência, frequência e valor.
- Gráficos de barras e boxplots para entender o perfil de cada grupo.

## Automação e Escalabilidade

#### Crieri funções reutilizáveis para:

- Calcular RFM
- Aplicar K-Means
- Atualizar os clusters com novos dados

Isso permite que o time de TI integre ao CRM ou automatize os relatórios periodicamente.

<hr>

