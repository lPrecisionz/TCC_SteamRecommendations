# Estudo de Algoritmos de Recomendação para Personalização de Conteúdo Aplicado em um *Dataset* de Jogos Digitais

Este repositório contém o código-fonte desenvolvido para o Trabalho de Conclusão de Curso (TCC) de **Pedro Henrique do Carmo Rosemiro**, apresentado ao curso de graduação em Sistemas de Informação da **Faculdade Professor Miguel Ângelo da Silva Santos (FeMASS)**, como requisito para obtenção do grau de Bacharel em Sistemas de Informação.

- **Autor:** Pedro Henrique do Carmo Rosemiro
- **Orientador:** Prof. Dr. Irineu A. Lima Neto
- **Instituição:** FeMASS — Macaé/RJ
- **Ano:** 2026

## Sobre o trabalho

O trabalho apresenta um estudo comparativo de algoritmos clássicos de recomendação aplicados a um conjunto de dados de jogos digitais da plataforma **Steam**, com o objetivo de avaliar diferentes abordagens para a personalização de conteúdo em ambientes digitais.

Foram implementados e comparados três algoritmos, contemplando desde abordagens personalizadas até uma estratégia não personalizada usada como linha de base:

- **Filtragem colaborativa baseada em similaridade de Pearson** (*user-based*): mede a correlação entre usuários e prevê notas a partir dos vizinhos mais similares, com predição centrada na média.
- **k-NN com similaridade do cosseno** (*item-based*): mede a similaridade entre jogos a partir do público que os jogou e recomenda itens vizinhos aos do histórico do usuário.
- **Recomendação por popularidade**: linha de base não personalizada, baseada na contagem de usuários por jogo.

A avaliação dos algoritmos baseou-se em quatro métricas: **tempo de execução**, **cobertura do catálogo**, **Precision@K** baseada em *holdout* e **concordância entre os algoritmos**.

## Conjunto de dados

- **Origem:** base pública obtida no Kaggle (TAMBER, 2017), contendo aproximadamente 200 mil interações entre usuários e jogos da Steam.
- **Sinal de interesse:** utilizou-se o **tempo jogado** como avaliação implícita; interações do tipo *purchase* (compra) foram descartadas por serem apenas binárias.
- O *dataset* não é redistribuído neste repositório. Para reproduzir os experimentos, baixe a base original na fonte citada e ajuste o caminho de leitura no notebook.

## Pré-processamento

O pipeline de preparação dos dados segue quatro etapas:

1. **Limpeza:** remoção de interações de compra, valores ausentes e registros duplicados.
2. **Transformação:** aplicação da transformação logarítmica `log(1 + x)` sobre o tempo jogado, gerando o atributo `rating`. Isso comprime a escala dos valores extremos sem descartá-los, evitando que usuários com milhares de horas dominem o cálculo de similaridade.
3. **Filtragem de esparsidade:** remoção de usuários com menos de 5 jogos e jogos com menos de 5 usuários, em duas passadas iterativas (processo em cascata).
4. **Construção da matriz usuário-item:** matriz esparsa em que cada linha é um usuário, cada coluna um jogo e cada célula o `rating` correspondente.

## Tecnologias utilizadas

- **Linguagem:** Python
- **Bibliotecas:** `pandas` (manipulação de dados e construção da matriz usuário-item), `numpy` (operações numéricas sobre vetores e matrizes), `scikit-learn` (implementação otimizada do k-NN) e `time` (medição do tempo de execução)
- **Ambiente:** Jupyter Notebook

## Parâmetros dos algoritmos

| Algoritmo | Parâmetros |
| --- | --- |
| Pearson (*user-based*) | Número de vizinhos (K) = 40; suporte mínimo de vizinhos = 3; filtro de similaridade > 0 (correlação positiva) |
| k-NN (*item-based*) | Número de vizinhos por jogo (K) = 10; métrica de similaridade = cosseno |
| Popularidade | Não personalizado (ranking global por número de usuários) |

## Como executar

> Recomenda-se o uso de um ambiente virtual (`venv` ou `conda`).

1. Clone o repositório:
   ```bash
   git clone <URL-do-repositorio>
   cd <nome-do-repositorio>
   ```

2. Instale as dependências:
   ```bash
   pip install -r requirements.txt
   ```
   Caso o arquivo `requirements.txt` não esteja presente, instale manualmente:
   ```bash
   pip install pandas numpy scikit-learn jupyter matplotlib
   ```

3. Baixe a base de dados da fonte citada (Kaggle) e ajuste o caminho do arquivo no notebook.

4. Abra o Jupyter Notebook e execute as células na ordem:
   ```bash
   jupyter notebook
   ```

## Resultados

Comparação consolidada das métricas obtidas sobre a mesma amostra de 30 usuários (Quadro 8 do TCC):

| Algoritmo | Tempo (ms) | Cobertura (%) | Jogos únicos | Precision@5 |
| --- | --- | --- | --- | --- |
| Pearson | 318,33 | 4,16 | 63 | 0,0267 |
| k-NN | 445,55 | 4,42 | 67 | 0,1067 |
| Popularidade | 0,44 | 0,86 | 13 | 0,1000 |

Principais conclusões:

- O **k-NN** obteve o melhor desempenho preditivo (Precision@5 = 0,1067) e a combinação mais equilibrada entre cobertura e precisão.
- O **Pearson** gerou recomendações coerentes com o perfil do usuário e foi o menos influenciado pela popularidade, ainda que com a menor precisão (0,0267).
- A **Popularidade**, embora extremamente eficiente, ofereceu baixa diversidade (cobertura de apenas 0,86%).
- A análise de concordância revelou **baixa sobreposição** entre as recomendações dos três algoritmos (no máximo 1,73 jogo em comum no Top-5, entre k-NN e Popularidade), evidenciando que métricas próximas não implicam recomendações semelhantes e que cada abordagem ocupa uma região distinta do espaço de recomendações.

Conclui-se que não há um algoritmo universalmente superior, sendo a escolha dependente dos objetivos da aplicação.

## Como citar

> ROSEMIRO, Pedro Henrique do Carmo. **Estudo de algoritmos de recomendação para personalização de conteúdo aplicado em um *dataset* de jogos digitais**. 2026. Trabalho de Conclusão de Curso (Bacharelado em Sistemas de Informação) — Faculdade Professor Miguel Ângelo da Silva Santos (FeMASS), Macaé, 2026.

## Licença

Defina aqui a licença do projeto (por exemplo, MIT). Caso não seja definida, todos os direitos são reservados ao autor.
