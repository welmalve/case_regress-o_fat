# Case Técnico - Previsão de Faturamento

## Introdução 

Este projeto foi desenvolvido como resolução do Case Técnico para uma vaga de Estágio de Machine Learning. Foi solicitada uma análise exploratória e o desenvolvimento de um modelo de Regressão. O conjunto de dados carrega uma série de informações sociodemográficas, e o faturamento da empresa, em cada bairro. 
Devido a pequena quantidade de dados, fiz a coleta de algumas informações adicionais na web. Do site da Wikipédia, coletei os seguintes dados por bairro: IDH, Subprefeitura, Região Administrativa e Área. Com isso, foi possível realizar algumas análises de público alvo mais acuradas, além de melhorar o desempenho do modelo. 
A entrega desse projeto está sendo feita de algumas maneiras. Este documento contém os principais insights do projeto, além da análise dos resultados do modelo de Machine Learning. Os notebooks - Webscraping, Projeto e Análise - estão todos na pasta 'notebooks'. O modelo final foi entregue em produção pelo Heroku. 
Esse documento serve como um guia do projeto. A descrição das etapas estão inclusas nas próximas sessões. 

## Índice 

*01.O Problema de Negócio


## 01. O Problema de Negócio 
Um cliente do ramo alimentício do Rio de Janeiro solicitou uma análise para poder entender melhor o seu público alvo. Do conjunto de dados, estão disponíveis algumas métricas sociogeograficas de cada bairro - Renda média, idade, população e domicilios. Além disso, estão disponíveis os seus respectivos faturamentos brutos. Estão disponíveis, ao total, 160 bairros para análise. 
Análise do Problema: 

•	Granularidade : Por bairro; 
•	Tipo de Modelo : Regressão. 

02. A Solução 
A solução do problema está entregue em dois formatos: 
•	Análise Exploratória dos Dados -> Fornecimento de Insights sobre as principais variáveis que impactam o faturamento do cliente. 
•	Modelo em Produção -> Modelo de Regressão em produção no Heroku. 

3.0. Conhecimentos 
Através da Análise Exploratória dos Dados, foi possível identificar alguns padrões sociodemográficos do público alvo do cliente. Algumas variáveis possuem um impacto sobre o problema em maior dimensão. As seguintes visões do problema foram analisadas: 

![MinMap_faturamento](https://user-images.githubusercontent.com/86089406/123551997-93abe580-d74a-11eb-9f14-dd16f2d7e8e4.png)

 
Alguns bairros se destacam no nível de receita. O gráfico a seguir elenca os dez principais bairros por ordem de faturamento bruto:

![top_10_bairros_faturamento](https://user-images.githubusercontent.com/86089406/123552059-cce45580-d74a-11eb-811f-54feb8a9c22f.png)

 
Ao colocar os dados médios dos dez principais bairros em faturamento, alguns padrões são verificados:

![padfat](https://user-images.githubusercontent.com/86089406/123552141-2d739280-d74b-11eb-95e0-e0832b947b0a.png)



Com o mapa de calor do faturamento dos bairros da cidade, é visível o padrão geográfico existente:

![map_calor](https://user-images.githubusercontent.com/86089406/123552205-82170d80-d74b-11eb-8a2a-af2672b5c5ee.png)

 
Os principais pontos de faturamento se concentram na Zona Sul e no Centro , além da Barra da Tijuca . O comércio na Zona Norte também possui um padrão médio interessante. Já o consumo na zona oeste é bem abaixo das demais localizações. 
Ao analisar a correlação entre as variáveis disponíveis, algumas se destacaram: 
## IDH x Faturamento 
O IDH possui alto impacto sobre o Faturamento. 

![idh_fat](https://user-images.githubusercontent.com/86089406/123552245-b12d7f00-d74b-11eb-83fa-4f69f783d75e.png)

 

## Renda Média x Faturamento 
A Renda Média Domiciliar também apresenta alta influência no Faturamento. 

![rendamédia_fat](https://user-images.githubusercontent.com/86089406/123552429-7a0b9d80-d74c-11eb-9fd2-bbc7d263deaf.png)


 
## Tipo de Domicílio x Faturamento 
O Faturamento possui boa correlação com o Tipo de Residência predominante no bairro. Bairros de padrão mais elevado apresentam altos níveis de receita para o comércio. 

![domic_fat](https://user-images.githubusercontent.com/86089406/123552457-9d364d00-d74c-11eb-997a-76630ea29273.png)

 

## Idade x Faturamento 
A Idade também demonstra alta correlação com o Faturamento. Percebe-se que bairros de idade média superior costumam ser mais rentáveis.

![idade_fat](https://user-images.githubusercontent.com/86089406/123552507-d1117280-d74c-11eb-9fef-c9d2efa2154b.png)



Perfil 
Dessa forma, é possível traçar um perfil médio do público alvo desse comércio: 

![pub_alvo](https://user-images.githubusercontent.com/86089406/123552538-fd2cf380-d74c-11eb-99b8-a8ffbbf58f5a.png)

 

4.0. Aplicação do Modelo de Machine Learning 
Por ser um problema com poucos dados, todo cuidado foi necessário para não prejudicar a performance do modelo. Valores irreais foram retirados. Alguns Outliers também tiveram que ser limitados. 
Além disso, os dados foram separados entre Treino e Teste na proporção 70/30. A escolha do modelo foi feita com o uso do CrossValidation . A métrica de escolha utilizada foi o MAPE (Mean Absolute Percentage Error). Ela toma como base o erro percentual de cada previsão individual, e calcula a média final. O modelo com o menor MAPE deverá ser o escolhido. 




O comparativo dos modelos utilizados ficou da seguinte maneira: 

![machine_img](https://user-images.githubusercontent.com/86089406/123552580-2e0d2880-d74d-11eb-9602-205ba23ba847.png)

 
Tanto a DecisionTreeRegressor quanto a RandomForestRegressor, dois modelos com base em árvores de decisão, tiveram larga vantagem sobre os demais. Como a Random Forest apresentou valores melhores, ela foi a escolhida como o modelo a ser utilizado nesse primeiro ciclo.

5.0. Performance do Modelo de Machine Learning 
Modelo escolhido, devemos melhorar seus Hiperparametros e avaliar a sua capacidade de generalização. 
Sobre os dados de Teste o modelo de Random Forest Regressor Aprensentou um valor de MAPE de 0.1051 . Ou seja, em média, o modelo erra 10.50% dos valores de faturamentos para dados nunca vistos antes. 
Também avaliei o modelo utilizando o CrossValidation , só que dessa vez em todo o Dataset. 
 
![machine_dataset](https://user-images.githubusercontent.com/86089406/123552616-509f4180-d74d-11eb-9cc0-00f53fa6b5b9.png)



O Modelo funciona de maneira satisfatória. Consegue aprender bon padrões dos dados de treinos. Mas também é capaz de generalizar para dados nunca vistos. Todas as nossas previsões seguiram as tendências dos faturamentos reais. Os erros existentes são mínimos.

 ![fat_pev real](https://user-images.githubusercontent.com/86089406/123552659-788ea500-d74d-11eb-8527-077d94e0e4d9.png)


6.0. Resultados de Negócio 
Visualizar os possíveis cenários é a melhor forma de traduzir o modelo para os resultados de negócio. Dessa forma, o cliente pode ter o valores previstos de faturamento e tomar decisões com base neles. 

![result_neg](https://user-images.githubusercontent.com/86089406/123552704-a542bc80-d74d-11eb-82fa-85c9131e30f9.png)


O modelo final foi entregue em produção. Através de um questionário com 12 questões, é possível fazer uma previsão de faturamento de um bairro. Esse repositório está acompanhado de um arquivo chamado API Tester. Nele é possível usar a API.

7.0. Conclusões 
Este primeiro ciclo do projeto foi um sucesso. Mesmo com uma pequena base de dados, foi possível entregar uma Análise Exploratória de Dados com Insights sobre o negócio do cliente. Também foi entregue um modelo em produção capaz de fazer previsões sobre novos bairros, de forma acurada. O objetivo final foi concluido: Apoiar a tomada de decisão do cliente. 

8.0. Lições Aprendidas 
Esse projeto foi desafiador. Como desenvolver um modelo bom com poucos dados foi o principal ponto de reflexão. Devido a isso, acabei me aprofundando no funcionamento de algumas técnicas de avaliação. A Análise Exploratória de Dados também agregou de forma positiva. Além de desenvolver algumas técnicas e conhecer novas bibliotecas, foi possível obter um maior conhecimento de negócio. 

                                                               https://www.linkedin.com/in/wellington-alves-662200165/
