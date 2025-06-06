## Imports e carregamento do dataset

```python
#Importando as Bibliotecas principais
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

from datetime import datetime
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans, DBSCAN
from sklearn.metrics import silhouette_score
from sklearn.decomposition import PCA
from sklearn.tree import DecisionTreeClassifier
from sklearn.inspection import permutation_importance

#Carregando os dados
df = pd.read_excel('dados/online_retail_II.xlsx', sheet_name='Year 2010-2011')

#Verificação inicial
print(df.head())
print(df.info())
```

![image](https://github.com/user-attachments/assets/8cfba239-dc97-4e03-82e8-7ff8b48319a4)


## Limpeza dos Dados

```python
#Remoção de nulos
df = df.dropna(subset=['Customer ID', 'InvoiceDate', 'Invoice', 'Quantity', 'Price'])

#Removendo valores negativos ou nulos
df = df[(df['Quantity'] > 0) & (df['Price'] > 0)]

#Calculando o valor total da compra
df['TotalAmount'] = df['Quantity'] * df['Price']

#Verificando se tem e caso tenha remove as duplicatas
df = df.drop_duplicates()

#Convertendo colunas que contém data para datatime
df['InvoiceDate'] = pd.to_datetime(df['InvoiceDate'])

#Verificando os dados depois da limpeza
print(df.describe())
```

![image](https://github.com/user-attachments/assets/3a1a461c-c25f-4f0d-ac61-f4dd45d2b415)

## Função para Calcular o RFM

```python
def calcular_rfm(df, data_col='InvoiceDate', cliente_col='Customer ID', valor_col='TotalAmount', data_base=None):
    if data_base is None:
        data_base = df[data_col].max() + pd.Timedelta(days=1)
    
    rfm = df.groupby(cliente_col).agg({
        data_col: lambda x: (data_base - x.max()).days,
        cliente_col: 'count',
        valor_col: 'sum'
    })
    
    rfm.columns = ['Recency', 'Frequency', 'Monetary']
    return rfm

rfm = calcular_rfm(df)
print(rfm.head())
```

![image](https://github.com/user-attachments/assets/61c30dfc-0b38-48ea-952a-7bb6c7435c4d)

## Score RFM (de 1 a 5)

```python
rfm['R_Score'] = pd.qcut(rfm['Recency'], 5, labels=[5,4,3,2,1])
rfm['F_Score'] = pd.qcut(rfm['Frequency'].rank(method='first'), 5, labels=[1,2,3,4,5])
rfm['M_Score'] = pd.qcut(rfm['Monetary'], 5, labels=[1,2,3,4,5])

rfm['RFM_Score'] = rfm['R_Score'].astype(str) + rfm['F_Score'].astype(str) + rfm['M_Score'].astype(str)
```

## Heatmap com o Score RFM

```python
rfm_heatmap = rfm.groupby(['R_Score','F_Score']).size().unstack()
sns.heatmap(rfm_heatmap, cmap="YlGnBu")
plt.title('Heatmap Recency x Frequency')
plt.show()
```

![image](https://github.com/user-attachments/assets/4e322e8a-1448-4232-9af1-29e637da6b00)

## Normalização para Clustering

```python
scaler = StandardScaler()
rfm_normalized = scaler.fit_transform(rfm[['Recency', 'Frequency', 'Monetary']])
```

## Utilizando o Método Elbow

```python
inertia = []
range_n = range(1, 11)
for k in range_n:
    kmeans = KMeans(n_clusters=k, random_state=42)
    kmeans.fit(rfm_normalized)
    inertia.append(kmeans.inertia_)

plt.plot(range_n, inertia, marker='o')
plt.xlabel('Número de Clusters')
plt.ylabel('Inércia')
plt.title('Método Elbow')
plt.grid()
plt.show()
```

![image](https://github.com/user-attachments/assets/b5411283-805a-4b00-b0b7-c1b2ca6cd1c4)

## Aplicando o K-Means e Silhouette Score

```python
kmeans = KMeans(n_clusters=4, random_state=42)
rfm['Cluster'] = kmeans.fit_predict(rfm_normalized)

sil_score = silhouette_score(rfm_normalized, rfm['Cluster'])
print(f"Silhouette Score: {sil_score:.2f}")
```

## Visualização 2D do PCA

```python
pca = PCA(n_components=2)
pca_result = pca.fit_transform(rfm_normalized)
rfm['PCA1'], rfm['PCA2'] = pca_result[:,0], pca_result[:,1]

sns.scatterplot(data=rfm, x='PCA1', y='PCA2', hue='Cluster', palette='tab10')
plt.title("Clusters de Clientes (PCA 2D)")
plt.show()
```

![image](https://github.com/user-attachments/assets/d24dc316-a5fa-42d3-9bc8-b18c65422b99)

## Visualização dos Clusters com Boxplots 

```python
for col in ['Recency', 'Frequency', 'Monetary']:
    sns.boxplot(x='Cluster', y=col, data=rfm)
    plt.title(f'{col} por Cluster')
    plt.show()
```

![image](https://github.com/user-attachments/assets/1a4b809d-717c-403f-83bb-21167aa405f7)

![image](https://github.com/user-attachments/assets/08f66019-b056-4733-bd28-bff7505599b3)

![image](https://github.com/user-attachments/assets/8da214cf-e47b-4733-a294-0f6b68c96381)

## Importância das Variáveis

```python
tree = DecisionTreeClassifier(max_depth=3)
tree.fit(rfm[['Recency','Frequency','Monetary']], rfm['Cluster'])

imp = permutation_importance(tree, rfm[['Recency','Frequency','Monetary']], rfm['Cluster'])
sns.barplot(x=imp.importances_mean, y=['Recency','Frequency','Monetary'])
plt.title("Importância das Variáveis")
plt.show()
```

![image](https://github.com/user-attachments/assets/e1a45c81-f483-453f-af81-83d23df9ff36)

## Aplicação de DBSCAN como comparação

```python
dbscan = DBSCAN(eps=0.8, min_samples=5)
rfm['Cluster_DBSCAN'] = dbscan.fit_predict(rfm_normalized)

sns.scatterplot(data=rfm, x="PCA1", y="PCA2", hue="Cluster_DBSCAN", palette="tab10")
plt.title("DBSCAN - Clusters")
plt.show()
```

![image](https://github.com/user-attachments/assets/3e8aaf66-118a-4493-9554-c13dd570a422)

## Tabela com Detalhes

```python
summary = rfm.groupby("Cluster").agg({
    "Recency": ["mean", "min", "max"],
    "Frequency": ["mean", "min", "max"],
    "Monetary": ["mean", "min", "max"],
    "Cluster": "count"
}).round(2)

summary.columns = ['_'.join(col) for col in summary.columns]
summary.rename(columns={"Cluster_count": "Num_Clientes"}, inplace=True)
display(summary)
```

![image](https://github.com/user-attachments/assets/c5f07540-fe9f-43f4-9e88-19394de82ad4)


