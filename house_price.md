# 1ª Parte: Análise de Dados Imobiliários

## Importação e Análise Inicial dos Dados
O conjunto de dados é importado a partir de um arquivo CSV. São realizadas as seguintes etapas de preparação inicial:
- Verificação das informações gerais dos dados.
- Identificação e remoção de duplicatas.
- Transformação da variável 'date' para o tipo 'datetime'.
- Transformação da variável 'waterfront' para binária, assumindo 'n' como 0 e 'y' como 1.

## Questão 1: Correlação entre 'price' e 'bedrooms'
Calculamos a matriz de correlação entre as variáveis numéricas e identificamos a correlação entre 'price' e 'bedrooms'. 
```
correlation_price_bedrooms = correlation_matrix.loc['price', 'bedrooms']
print("Correlação entre 'price' e 'bedrooms':", correlation_price_bedrooms)
```
`R.: Correlação entre 'price' e 'bedrooms': 0.299207216169182`

Em seguida, comparamos essa correlação com o conjunto completo de dados e com um subconjunto de casas com 'sqft_living' superior a 2000 pés quadrados.

```
# Filtrando o DataFrame para incluir apenas casas com 'sqft_living' superior a 2000
df_above_2000 = df_house[df_house['sqft_living'] > 2000]

# Calculando a correlação entre 'price' e 'bedrooms' para as casas acima de 2000 sqft_living
correlation_price_bedrooms_above_2000 = df_above_2000['price'].corr(df_above_2000['bedrooms'])

print("Correlação entre 'price' e 'bedrooms' para casas com sqft_living > 2000:", correlation_price_bedrooms_above_2000)
```
`R.: Correlação entre 'price' e 'bedrooms': 0.299207216169182`

Embora ambas as correlações sejam positivas, a diferença entre elas indica que a relação entre 'price' e 'bedrooms' é mais fraca quando consideramos apenas casas com uma área total 'sqft_living' superior a 2000 pés quadrados em comparação com o conjunto de dados completo.

## Questão 2: Correlação entre 'price' e 'sqft_living' com pelo menos 2 banheiros
Analisamos a correlação entre 'price' e 'sqft_living' considerando apenas casas com pelo menos dois banheiros.

```
# Filtrando o DataFrame para incluir apenas casas com pelo menos dois banheiros
df_at_least_2_bathrooms = df_house[df_house['bathrooms'] >= 2]

# Calculando a correlação entre 'price' e 'sqft_living' para as casas com pelo menos dois banheiros
correlation_price_sqft_living_at_least_2_bathrooms = df_at_least_2_bathrooms['price'].corr(df_at_least_2_bathrooms['sqft_living'])

print("Correlação entre 'price' e 'sqft_living' para casas com pelo menos dois banheiros:", correlation_price_sqft_living_at_least_2_bathrooms)
```
`R.: Correlação entre 'price' e 'sqft_living' para casas com pelo menos dois banheiros: 0.7067107107996193`

Correlação forte

## Questão 3: Influência da quantidade de banheiros na correlação entre 'sqft_living' e 'price'
Avaliamos como a quantidade de banheiros influencia na correlação entre 'sqft_living' e 'price', categorizando os banheiros em intervalos e observando a correlação para cada categoria.

No contexto imobiliário, é comum utilizar valores quebrados para descrever banheiros parcialmente equipados. Aqui está o que cada valor pode significar:

- Inteiros (ex: 1.00, 2.00, 3.00): Representam banheiros completos que incluem um vaso sanitário, uma pia, e um chuveiro ou banheira.
- Valores terminados em 0.5 (ex: 1.50, 2.50, 3.50): Representam um banheiro que possui um vaso sanitário e uma pia, mas não inclui um chuveiro ou banheira. São conhecidos como "meio banheiro".
- Valores terminados em 0.25 ou 0.75 (ex: 1.25, 2.75): Representam situações em que o banheiro tem uma combinação específica de facilidades. Por exemplo, 1.25 pode indicar um banheiro completo e um banheiro com apenas uma pia (sem vaso sanitário), ou uma combinação similar.

```
df_house['bathrooms'].unique()
```

`array([1.  , 2.25, 3.  , 2.  , 4.5 , 1.5 , 2.5 , 1.75, 2.75, 3.25, 4.  ,
       3.5 , 0.75, 4.75, 4.25, 3.75, 5.  , 0.  , 1.25, 0.5 , 5.5 , 5.25,
       6.75, 6.  , 5.75, 8.  , 7.5 , 7.75])`

```
# Definir os intervalos de categorização
bins = [0, 1, 2, 3, 4, 5, 6, float('inf')]

# Definir os rótulos para cada categoria
labels = ['Até 1', 'Até 2', 'Até 3', 'Até 4', 'Até 5', 'Até 6', 'Mais de 6']

# Aplicar a função cut para criar as categorias
df_house['bathroom_categories'] = pd.cut(df_house['bathrooms'], bins=bins, labels=labels, right=False)
```
| Quant. | Correlação |
| ------| ----------|
|Até 3    |    4717|
|Até 2     |   4164|
 |  Até 4    |     895
 |  Até 5     |    148
 |  Até 1      |    45
 |  Até 6       |   23
 |  Mais de 6    |   7 |  


```
# Calcular a correlação para cada categoria de banheiros
correlation_by_bathroom_category = df_house.groupby('bathroom_categories').apply(lambda x: x['sqft_living'].corr(x['price']))

print("Correlação entre 'sqft_living' e 'price' para cada categoria de banheiros:")
print(correlation_by_bathroom_category)

```

Correlação entre 'sqft_living' e 'price' para cada categoria de banheiros:
| Quant. | Correlação |
| ------| ----------|
| Até 1 |  0.785669 |
| Até 2 |  0.459896 |
| Até 3 |  0.569543 |
| Até 4 |  0.594484 |
| Até 5 |  0.660007 |
| Até 6 |  0.796831 |
|Mais de 6  |  0.842632|

```# Criar a matriz de scatter plots
sns.set(style="ticks")
g = sns.FacetGrid(df_house, col="bathroom_categories", col_wrap=4, height=4)
g.map(plt.scatter, "sqft_living", "price", alpha=0.5)
g.set_axis_labels("Área Total da Casa (sqft_living)", "Preço (price)")
plt.suptitle('Relação entre Área Total da Casa e Preço por Quantidade de Banheiros', y=1.02)
plt.tight_layout()
plt.show()
```
![image](https://github.com/nadinne94/correlacao-regressao/assets/129687299/1e4c2142-939b-4638-ac2b-ec2cb36cac3c)

```
# Definir os intervalos de categorização
bins = [0, 2, 3, float('inf')]

# Definir os rótulos para cada categoria
labels = [ 'Até 2', '3', '3+']

# Aplicar a função cut para criar as categorias
df_house['bathroom_categories2'] = pd.cut(df_house['bathrooms'], bins=bins, labels=labels, right=False)

# Exibir as contagens de cada categoria
print(df_house['bathroom_categories2'].value_counts().sort_index())
```

|Quant.| Correlação|
|-|-|
|Até 2 |0.467361|
|3|0.569543|
|3+|0.715869|

```
# Criar a matriz de scatter plots
sns.set(style="ticks")
g = sns.FacetGrid(df_house, col="bathroom_categories2", col_wrap=4, height=4)
g.map(plt.scatter, "sqft_living", "price", alpha=0.5)
g.set_axis_labels("Área Total da Casa (sqft_living)", "Preço (price)")
plt.suptitle('Relação entre Área Total da Casa e Preço por Quantidade de Banheiros', y=1.02)
plt.tight_layout()
plt.show()

```

![image](https://github.com/nadinne94/correlacao-regressao/assets/129687299/09aacd71-9e0c-414d-8135-0c7e86484181)


## Questão 4: Relação entre 'condition' e 'price' para casas com 'sqft_living' superior a 3000 pés quadrados
Examinamos a correlação entre 'condition' e 'price' considerando apenas casas com 'sqft_living' superior a 3000 pés quadrados.

## Questão 5: Correlação entre localização geográfica ('lat' e 'long') e 'price' para casas com pelo menos três quartos
Analisamos a correlação entre 'lat', 'long' e 'price' considerando apenas casas com pelo menos três quartos, além de visualizar essas relações por meio de um mapa de calor.

## Questão 6: Correlação entre 'waterfront' e 'price' usando ANOVA
Calculamos a correlação entre a variável categórica 'waterfront' e a variável numérica 'price' usando ANOVA (Analysis of Variance). Visualizamos essa relação por meio de gráficos de barras e boxplot, além de calcular o coeficiente de correlação r_pb.

### Resultados e Conclusões

Concluímos que a correlação entre o preço e o número de quartos é fraca, e essa relação se torna ainda mais fraca quando consideramos apenas casas com uma área total superior a 2000 pés quadrados. Por outro lado, identificamos uma correlação forte entre o preço e a área total da casa quando consideramos apenas casas com pelo menos dois banheiros. Além disso, observamos que quanto maior o número de banheiros, maior é a correlação entre a área total da casa e o preço.

## Conclusão

Este projeto forneceu uma visão abrangente das relações entre diferentes características das casas e seus preços na região de King County. Ao entender melhor essas relações, podemos tomar decisões mais informadas ao comprar ou vender imóveis, além de identificar características importantes que influenciam os preços das casas.
