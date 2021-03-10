<h1 align="center">Analise Exploratoria de Dados</h1>

<h1 align="center">
  <img src="https://github.com/leonvictorlima/Analise-exploratoria-compras/blob/main/imagens/1_PKXC0FeXQc5LVmqhJ8HnVg.png"  width="500"/>
</h1>

## Descrição do Projeto
Este arquivo se refere na aplicação de analise exploratória de dados utilizando a linguagem python. Este conjunto de dados foi extraído do repositório da UCI e adaptado para utilização neste exemplo.

A utilidade de linguagem de programação na extração de informações relevantes em grandes conjuntos de dados tornou-se cada vez mais útil com o seu grande volume e surgimento por causa do Big Data. A usabilidade e a aderência de suas flexibilidade como a linguagem de programação em python ou R são a chave para uma análise nesse novo e moderno mundo dos dados.

Neste exemplo, utilizamos um conjunto de dados de compras para aprender melhor sobre o perfil dos clientes, itens mais vendidos, informações demográficas por gênero, entre outros.

Espero que possa ser útil e um guia no aprendizado em análise de dados.


Tabela de conteúdos
=================
<!--ts-->
   * [Objetivo](#objetivo)
   * [Carregando os dados](#carregando-dados)
   * [Análise de compras](#analise-compras)
      * [Informações demográficas por gênero](#demografico-genero)
      * [Análise de compra por gênero](#compra-genero)
      * [Análise demográfica](#demografico)
      * [Top gastadores](#gastadores)
      * [Itens mais populares](#populares)
      * [Itens mais lucrativos](#lucrativos)
   * [Conclusão](#conclusao)
<!--te-->
1. [ Description. ](#desc)
2. [ Usage tips. ](#usage)

<a name="objetivo"></a>
## Objetivo

O objetivo deste módulo é efetuar análise de um conjunto de dados de clientes que efetuaram uma série de compras utilizando data munging. Esta analise poderá ser replicada em qualquer outro conjunto de dados, bem como seguir de guia para diferentes aplicações de exploração.

<a name="carregando-dados"></a>
## Carregando os dados

Para iniciar nossa jornada, é necessário efetuar o carregamento dos dados. Os mesmo estão disposníveis neste mesmo repositório com o título "dados_compras.json". Diferente de outras abordagens mais convencionais, estes dados estão em formato javascript object notation, ou json, e portanto, será utilizado a função do pandas para seu carregamento.

```python

# Declarando pacotes

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# lendo arquivo json pelo pandas

df = pd.read_json('dados_compras.json', orient='records')

```

<a name="analise-compras"></a>
## Análise de compras

Neste momento, é realizado uma análise sobre os dados carregados. A abordagem aqui é entender como estão distribuídos, os tipos de dados, suas variáveis, entre outros insights iniciais.


```python

# Observando os dados

df.head()

```

<h1 align="center">
  <img src="https://github.com/leonvictorlima/Analise-exploratoria-compras/blob/main/imagens/head.JPG"  width="500"/>
</h1>


```python
# Avaliando algumas métricas: quartil, média, valores máximos e mínimos e desvio padrão;

df.describe()

```
<h1 align="center">
  <img src="https://github.com/leonvictorlima/Analise-exploratoria-compras/blob/main/imagens/describe.JPG"  width="500"/>
</h1>

```python
infor_compradores = df.loc[:,['Sexo', 'Login', 'Idade']]
infor_compradores.head(6)

# Análise das compras em geral

#Plotando o número de compras total por ID;

print (df['Item ID'].value_counts().head(10).plot(kind='bar'))

```
<h1 align="center">
  <img src="https://github.com/leonvictorlima/Analise-exploratoria-compras/blob/main/imagens/compras.JPG"  width="500"/>
</h1>

```python

#Total de Itens;

total_itens = df['Nome do Item'].count()

print ('Número total de items vendidos: ',total_itens)

# Itens únicos:

item_unico = len(df['Item ID'].unique())

print ('Número total de items exclusivos: ',item_unico)
# Preço médio;

preco_medio = df['Valor'].mean()

print("O preço médio é %.2f" % preco_medio)

# Preço total:
preco_total = df['Valor'].sum()

print('O preço total = ', preco_total)

#####
# DataFrame para os resultados;

resultados = pd.DataFrame({'Número Itens exclusivos': item_unico, 
                           'Número de Compras': total_itens,
                          'Total de vendas': preco_total,
                          'Preco medio de vendas': [preco_medio]})
resultados


```
<a name="demografico-genero"></a>
## Informações demográficas por gênero

Aqui se inicia uma extração de valor maior dos dados. As informações por gênero é um fator importante para compreendermos os público de compras. A disponibilidade de certos produtos e sua produção leva em conta o público para o qual é direcionado. E qual o gênero que efetuam mais compras em nosso projeto?

```python
# Número de compradores por gênero e porcentagem
compradores_genero = infor_compradores['Sexo'].value_counts()
print(compradores_genero)
print('\n')

compradores_porcentagem = (compradores_genero/total_consumidores)*100
print(compradores_porcentagem)
print('\n')

# DataFrame

compradores_dados = pd.DataFrame({'Sexo': compradores_genero,
                                'Porcentagem %': compradores_porcentagem})

# Data

compradores_dados = compradores_dados.round(2)
compradores_dados ['Porcentagem %'] = compradores_dados['Porcentagem %'].map('{:,.2f}%'.format)

compradores_dados

```

<a name="compra-genero"></a>
## Análise de compra por gênero

Como relatado anteriomente, é importante uma avaliação em nossos dados sobre quais itens são mais buscados pelos nossos clientes e uma divisão sobre estes items mais buscados por gênero é de extremo valor. Os itens que são mais vendidos por gênero, a média de pagamento por item e o total geral. 

```python
# Criando as tabelas

# Total geral
total_genero_por_item = df.groupby(['Sexo']).sum()['Valor'].rename('Total de Vendas')

# Média Geral
media_preco_genero_por_item = df.groupby(['Sexo']).mean()['Valor'].rename('Média de preço')

# Contagem Geral
contagem_compras = df.groupby(['Sexo']).count()['Valor'].rename('Número de compras')

# Média Total individual dos Gêneros 
total_individual_genero = total_genero_por_item/compradores_dados['Sexo']


# DataFram

analise_genero = pd.DataFrame({'Número Compras': contagem_compras,
                               'Média Geral por item': media_preco_genero_por_item,
                               'Total geral por genero': total_genero_por_item,
                              'Media total individual por gênero Normalizado': total_individual_genero})

# Formatação dos dados


analise_genero = analise_genero.round(2)
analise_genero['Total geral por genero'] = analise_genero['Total geral por genero'].map('${:,.2f}'.format) 
analise_genero['Média Geral por item'] = analise_genero['Média Geral por item'].map('${:,.2f}'.format)
analise_genero['Media total individual por gênero Normalizado'] = analise_genero['Media total individual por gênero Normalizado'].map('${:,.2f}'.format) 
analise_genero

```

<a name="demografico"></a>
## Análise demográfica

Como não pensar em uma análise demográfica? Isto é um dos pontos mais relevantes e que muitas vezes não é levado em consideração em uma análise. Simples avaliações, fazem total diferença. Neste tópico, observamos que a média de idades está entre o range de 20 à 24 anos. Outras informações são observadas abaixo:

```python
# Cálculo de idade básico
idade_bin = [0, 9.99, 14.99, 19.99, 24.99, 29.99, 34.99, 39.99, 999]
label_idade = ['Menor de 10', '10 - 14', '15 - 19', '20 - 24',
               '25 - 29', '30 - 34', '35 - 39', 'Acima de 40']
df['Intervalo de Idades'] = pd.cut(
    df['Idade'], idade_bin, labels=label_idade)

# Cálculos

contagem_idade = df['Intervalo de Idades'].value_counts()

media_idade = df.groupby(['Intervalo de Idades']).mean()['Valor']

total_item_idade = df.groupby(['Intervalo de Idades']).sum()['Valor']

idade_porcent = (contagem_idade/total_consumidores)*100

#DataFrame resultados;

idade_dataframe = pd.DataFrame({'Contagem:': contagem_idade,
                      '%': idade_porcent,
                    'Media de Idade, unitário': media_idade,
                     'total_item_idade': total_item_idade})

# Data munging

idade_dataframe ['Media de Idade, unitário'] = idade_dataframe['Media de Idade, unitário'].map('${:,.2f}'.format)
idade_dataframe['total_item_idade'] = idade_dataframe['total_item_idade'].map('${:,.2f}'.format)
idade_dataframe['%'] = idade_dataframe['%'].map('{:,.2f}%'.format)
idade_dataframe.sort_index()

```

<a name="gastadores"></a>
## Top gastadores

Diante dos dados explorados até o momento, estamos chegando ao final da nossa análise e neste âmbito, uma dos itens importantes é observar os clientes que gastam mais na efetuação de compras:

<h1 align="center">
  <img src="https://github.com/leonvictorlima/Analise-exploratoria-compras/blob/main/imagens/gastadores.JPG"  width="500"/>
</h1>

<a name="populares"></a>
## Itens mais populares

E sobre as compras, abaixo temos a exibição dos itens mais populares:

```python

# Calculos básicos

usuario_total = df.groupby(['Nome do Item']).sum()['Valor'].rename('Valor total de compra')
usuario_media = df.groupby(['Nome do Item']).mean()['Valor'].rename('Valor médio de compra')
usuario_contagem = df.groupby(['Nome do Item']).count()['Valor'].rename('Numero de compras')


# Dataframe

usuario_dados = pd.DataFrame({'Valor total de compra': usuario_total,
                              'Valor médio de compra': usuario_media,
                              'Numero de compras': usuario_contagem})

# Data formatação

usuario_dados['Valor total de compra'] = usuario_dados['Valor total de compra'].map('${:,.2f}'.format)
usuario_dados['Valor médio de compra'] = usuario_dados['Valor médio de compra'].map('${:,.2f}'.format)
usuario_dados.sort_values('Numero de compras', ascending=False).head(5)

```

<h1 align="center">
  <img src="https://github.com/leonvictorlima/Analise-exploratoria-compras/blob/main/imagens/populares.JPG"  width="600"/>
</h1>

<a name="lucrativos"></a>
## Itens mais lucrativos

```python

# Calculos básicos

usuario_total = df.groupby(['Nome do Item']).sum()['Valor'].rename('Valor total de compras')
usuario_media = df.groupby(['Nome do Item']).mean()['Valor'].rename('Valor médio de compra')
usuario_contagem = df.groupby(['Nome do Item']).count()['Valor'].rename('Numero de compras')


# Dataframe

usuario_dados = pd.DataFrame({'Valor total de compras': usuario_total,
                              'Valor médio de compra': usuario_media,
                              'Numero de compras': usuario_contagem})

# Data formatação

usuario_dados['Valor total de compras'] = usuario_dados['Valor total de compras'].map('${:,.2f}'.format)
usuario_dados['Valor médio de compra'] = usuario_dados['Valor médio de compra'].map('${:,.2f}'.format)
usuario_dados.sort_values('Numero de compras', ascending=False).head(5)

```

<h1 align="center">
  <img src="https://github.com/leonvictorlima/Analise-exploratoria-compras/blob/main/imagens/lucrativos.JPG"  width="600"/>
</h1>

<a name="conclusao"></a>
## Conclusão
 
Após essa curta análise dos dados, pudemos observar o comportamento dos clientes e o padrão de consumo deles. A exploração de dados é uma das tarefas mais necessárias para conhecimento de mercado e entendimento do negócio que está sendo tratado, possibilidando uma compreensão melhor dos dados que estamos trabalhando. 

Espero que essa análise em python possa ser de grande valia e de guia para outras abordagens. 

O código em íntegra está disponível no meu repositório do #github, 
