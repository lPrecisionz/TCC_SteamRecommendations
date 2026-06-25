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