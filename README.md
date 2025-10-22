
# Rota 04 - Análise de Vendas e Engajamento (Amazon)

Projeto de portfólio (no contexto "Datalab") focado em Análise Exploratória de Dados (EDA) e Teste de Hipóteses para identificar os fatores que realmente impactam o engajamento e as avaliações de produtos no marketplace da Amazon.

---
## Contexto e Problema de Negócio

Este projeto foi desenvolvido no cenário da "Datalab", uma consultoria de dados que incentiva seus analistas a explorarem datasets alinhados aos seus interesses para demonstrar seu potencial analítico.

O dataset escolhido foi o **"Amazon Sales"**, que contém dados de mais de 1.000 produtos e suas respectivas avaliações.

**O Problema de Negócio:** Em um marketplace tão competitivo quanto a Amazon, entender o que impulsiona o engajamento do cliente é fundamental. Muitas empresas operam com base em suposições ("mitos") para definir suas estratégias de vendas.

As perguntas de negócio que guiaram esta análise foram:
* Será que descontos agressivos realmente resultam em clientes mais satisfeitos (melhores notas)?
* Produtos mais caros recebem mais ou menos avaliações?
* A qualidade do cadastro do produto (como ter uma imagem) impacta a disposição do cliente em avaliá-lo?

## Objetivos da Análise

O objetivo principal foi **validar ou refutar hipóteses** sobre o comportamento das avaliações de produtos na Amazon, usando análise estatística para gerar insights acionáveis.

As principais hipóteses testadas foram:
1.  Quanto maior o desconto (`discount_percentage`), melhor a nota (`rating`).
2.  Quanto mais avaliações (`rating_count`), melhor a nota (`rating`).
3.  Produtos de certas categorias (`categoria_principal`) recebem notas melhores.
4.  Produtos mais caros (`actual_price`) recebem mais avaliações (`rating_count`).
5.  A presença de imagem no anúncio (`status_imagem`) impacta o recebimento de avaliações.

## Tecnologias Utilizadas

* **Linguagem Principal:** Python
* **Bibliotecas de Análise:** Pandas (para ETL/Limpeza), Matplotlib, Seaborn (para Visualização).
* **Bibliotecas Estatísticas:** SciPy (para testes de hipóteses).
* **Ambiente de Desenvolvimento:** Google Colab.
* **Insumos:** Datasets `amazon_product.csv` e `amazon_review.csv`.

---
## Principais Insights e Conclusões

A análise estatística dos dados da Amazon revelou que muitas suposições comuns sobre preços e descontos estão incorretas. O fator mais decisivo para o engajamento do cliente foi, na verdade, a qualidade do cadastro do produto.

### Resultado Chave 1: Qualidade do Anúncio é Pré-Requisito

O insight mais impactante da análise foi que a qualidade do anúncio (especificamente, ter uma imagem) não é apenas um "fator", mas sim um **pré-requisito para o engajamento**.

* **Zero Engajamento:** Produtos **"Sem Imagem"** tiveram **0 (zero)** avaliações (`rating_count = 0`).
* **Validação Estatística:** A análise de Risco Relativo (RR) confirmou que a probabilidade de um produto "Sem Imagem" receber uma avaliação foi de 0.0%.
* **Implicação:** Esforços de marketing ou precificação são inúteis se o cadastro básico do produto não estiver completo. A ausência de imagem equivale à ausência de vendas/avaliações.

### Resultado Chave 2: Os "Mitos" do Preço e Desconto (Refutados)

As hipóteses mais comuns relacionadas a preço e desconto não foram validadas estatisticamente.

* **Mito: Descontos geram melhores notas.**
    * **Refutado:** Não há correlação estatística significativa entre `discount_percentage` e `rating`. Dar um desconto maior não resulta em um cliente mais satisfeito.
* **Mito: Preço alto ou baixo impacta a nota.**
    * **Refutado:** Não foi encontrada diferença estatística significativa no `rating` médio entre produtos "caros" e "baratos".
* **Mito: O número de avaliações afeta a nota média.**
    * **Refutado:** Não há correlação entre `rating_count` e `rating`. Produtos com muitas avaliações não têm, necessariamente, notas melhores ou piores.

### Recomendações Estratégicas (Para a "Datalab")

Se fôssemos apresentar esta análise à Amazon ou a um grande vendedor do marketplace, a recomendação estratégica seria:

1.  **Foco Imediato na Qualidade do Cadastro (Quality Control):** A prioridade de investimento deve ser em auditoria de marketplace para **eliminar anúncios sem imagem**, especialmente em categorias de alto valor como "Electronics", onde produtos caríssimos foram encontrados sem fotos.
2.  **Repensar a Estratégia de Descontos:** A empresa não deve "gastar" margem de lucro com descontos na expectativa de melhorar a satisfação ou a nota média, pois os dados mostram que esses fatores não estão correlacionados.

---
## Visualizações e Análises Chave

### 1. Mapa de Calor de Correlações
* **Descrição:** Matriz de correlação de Pearson entre as principais variáveis numéricas. Confirma visualmente a falta de correlação entre `rating` e as variáveis de preço ou desconto.

*![dash1-proj 04.jpg](dashboards_screenshots/dash1-proj 04.jpg)*

### 2. Distribuição de Produtos com ou sem Imagem
* **Descrição:** Gráfico de barras mostrando a proporção de produtos com e sem imagem. Essencial para contextualizar o principal insight do projeto.

*![Mapa de Calor - Correlação de Pearson](dashboards_screenshots/dash1-proj%2004.jpg)*

### 3. Boxplot da Variável "Rating"
* **Descrição:** Boxplot da variável `rating`. Importante para mostrar que, embora o método estatístico IQR tenha identificado "outliers", eles eram, na verdade, avaliações legítimas (ex: notas 2, 3), e que a grande maioria das notas se concentra entre 4.0 e 4.5.

*[COLE AQUI O PRINT: "Boxplot + Outliers de Rating" (página 14 da ficha)]*

---
## Técnicas Estatísticas Aplicadas (as "Fórmulas")

Para garantir que os insights não fossem baseados apenas em visualização, foram aplicados os seguintes testes estatísticos para validar (ou refutar) as hipóteses:

### 1. Correlação de Pearson
* **O que faz:** Mede a força e a direção de uma relação *linear* entre duas variáveis numéricas (varia de -1 a 1).
* **Onde foi usada:** Para testar as hipóteses de correlação entre:
    * `rating` vs. `discount_percentage` (Resultado: -0.01, sem correlação)
    * `rating` vs. `rating_count` (Resultado: -0.01, sem correlação)

### 2. Teste Kruskal-Wallis
* **O que faz:** Teste não paramétrico (não exige distribuição normal) usado para verificar se há diferença estatisticamente significativa entre as medianas de *três ou mais* grupos independentes.
* **Onde foi usada:** Para testar se a `categoria_principal` do produto tinha impacto na `rating` média. O teste falhou em rejeitar a hipótese nula, indicando que não há diferença significativa.

### 3. Teste Mann-Whitney U
* **O que faz:** Teste não paramétrico usado para verificar se há diferença significativa entre as medianas de *dois* grupos independentes.
* **Onde foi usada:** Para testar se o preço (categorizado como "caro" ou "barato") tinha impacto no `rating` médio. O teste falhou em rejeitar a hipótese nula.

### 4. Risco Relativo (RR)
* **O que faz:** Compara a probabilidade de um evento ocorrer em um grupo exposto (ex: "Sem Imagem") versus um grupo não exposto (ex: "Com Imagem").
* **Onde foi usada:** Para provar que a probabilidade de um produto "Sem Imagem" receber uma avaliação (`rating_count > 0`) era **zero**.

---
## Processo Técnico (ETL)

A fundação de qualquer análise confiável é um processo de limpeza de dados rigoroso.

* **Tratamento de Nulos:** Valores ausentes foram tratados de acordo com o contexto (ex: `rating_count` preenchido com 0; `about_product` com "sem descrição"; `img_link` com "sem link").
* **Tratamento de Duplicados:** Registros duplicados foram removidos com base nas chaves primárias (`product_id` e `user_id`), garantindo a unicidade.
* **Feature Engineering (Engenharia de Variáveis):**
    * A coluna `category` era complexa (ex: 'Eletrônicos|Computadores'). Foi criada a coluna `categoria_principal` contendo apenas a primeira categoria para simplificar a análise.
    * Foi criada a coluna `status_imagem` (Com/Sem Imagem) para segmentar a base.
* **União das Tabelas:** As tabelas `dados_produtos` e `dados_review` foram unidas usando um **LEFT JOIN** pela chave `product_id`, garantindo que produtos (mesmo aqueles sem nenhuma avaliação) fossem mantidos na análise.

---
## Como Rodar o Projeto

1.  Abrir o notebook (`.ipynb`) no Google Colab.
2.  Carregar os datasets `amazon_product.csv` e `amazon_review.csv`.
3.  Executar as células de limpeza e ETL (Pandas) para replicar o tratamento de dados.
4.  Executar as células de Análise Exploratória (Matplotlib/Seaborn) e Testes de Hipóteses (SciPy).

---
## Autor
**Leticia Gama de Souza** (e Ariana Brito)
[LinkedIn](https://www.linkedin.com/in/leticia-gama-code) | [GitHub](https://github.com/LeticiaGama-dev)
