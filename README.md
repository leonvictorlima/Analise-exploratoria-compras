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
   * [Carregando os dados e Avaliando](#carregando-dados)
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
## Carregando os dados e Avaliando

Para iniciar nossa jornada, é necessário efetuar o carregamento dos dados. Os mesmo estão disposníveis neste mesmo repositório com o título "dados_compras.json". Diferente de outras abordagens mais convencionais, estes dados estão em formato javascript object notation, ou json, e portanto, será utilizado a função do pandas para seu carregamento.

```python

# Declarando pacotes

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

