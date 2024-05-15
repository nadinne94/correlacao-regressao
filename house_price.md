# Documentação do Projeto: "Desafio Individual: Correlação e Regressão Simples/Multivariada"

## Introdução
Este projeto aborda análises de correlação e regressão simples/multivariada com base em um conjunto de dados imobiliários. O objetivo é compreender as relações entre diferentes variáveis e o preço das casas em um determinado mercado.

### Bibliotecas Utilizadas
- Pandas: Para manipulação de dados tabulares.
- Numpy: Para operações numéricas.
- Seaborn e Matplotlib: Para visualização de dados.
- Statsmodels e Scipy.stats: Para cálculos estatísticos.

### Preparação dos Dados
Inicialmente, importamos o conjunto de dados que contém informações sobre as casas, como preço, número de quartos, área total, entre outras. Realizamos uma análise inicial dos dados, verificando informações gerais e a presença de duplicatas. Também fizemos algumas transformações nos dados, como converter a variável de data para o tipo correto e transformar a variável categórica 'waterfront' em binária.# Desafio Individual: Correlação e Regressão Simples/Multivariada

### Análise de Correlação

Na primeira parte da análise, calculamos a matriz de correlação entre as variáveis numéricas do conjunto de dados. Isso nos permitiu entender as relações lineares entre as diferentes características das casas. Em seguida, focamos na correlação entre o preço das casas e o número de quartos, considerando também casas com uma área total superior a 2000 pés quadrados.

### Regressão Simples e Multivariada

Para investigar a relação entre o preço das casas e outras variáveis, realizamos análises específicas. Primeiramente, exploramos se há correlação entre o preço e a área total da casa, considerando apenas casas com pelo menos dois banheiros. Em seguida, investigamos como a quantidade de banheiros influencia na correlação entre a área total da casa e o preço. Isso foi feito categorizando o número de banheiros e calculando a correlação para cada categoria.

## 1ª Parte: Preparação dos Dados

### Importação e Análise Inicial dos Dados
O conjunto de dados é importado a partir de um arquivo CSV. São realizadas as seguintes etapas de preparação inicial:
- Verificação das informações gerais dos dados.
- Identificação e remoção de duplicatas.
- Transformação da variável 'date' para o tipo 'datetime'.
- Transformação da variável 'waterfront' para binária, assumindo 'n' como 0 e 'y' como 1.

## Questão 1: Correlação entre 'price' e 'bedrooms'
Calculamos a matriz de correlação entre as variáveis numéricas e identificamos a correlação entre 'price' e 'bedrooms'. Em seguida, comparamos essa correlação com o conjunto completo de dados e com um subconjunto de casas com 'sqft_living' superior a 2000 pés quadrados.

## Questão 2: Correlação entre 'price' e 'sqft_living' com pelo menos 2 banheiros
Analisamos a correlação entre 'price' e 'sqft_living' considerando apenas casas com pelo menos dois banheiros.

## Questão 3: Influência da quantidade de banheiros na correlação entre 'sqft_living' e 'price'
Avaliamos como a quantidade de banheiros influencia na correlação entre 'sqft_living' e 'price', categorizando os banheiros em intervalos e observando a correlação para cada categoria.

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