# AVA1-AM-Spotify
Avaliação em dupla desenvolvida para a matéria de aprendizado de maquina - Rhian Mendes Souza e Pedro Henrique Carvalho Biondi
# 📊 Análise e Classificação de Artistas do Spotify por Popularidade

## 🎯 Objetivo do Projeto
Este projeto desenvolve um sistema completo para classificação de artistas musicais em faixas de popularidade baseadas no número de seguidores, utilizando técnicas de Machine Learning e análise de dados. O processo integra coleta de dados da API do Spotify, pré-processamento avançado, modelagem preditiva e visualização de insights.

## 🔍 Fluxo Completo do Projeto

### 1. Coleta de Dados (API Spotify)
- **Endpoint utilizado**: `/v1/artists`
- **Dados coletados**:
  - Informações básicas: `artist_id`, `artist_name`
  - Métricas de popularidade: `followers.total`, `popularity`
  - Categorização: `genres`, `type`
- **Estratégia de coleta**:
  - Busca por múltiplos gêneros musicais (87 gêneros diferentes)
  - Coleta em lotes de 50 artistas por requisição
  - Tratamento de erros e duplicados

### 2. Pré-processamento e Engenharia de Features
- **Transformações aplicadas**:
  - Normalização logarítmica: `log_followers = np.log1p(followers)`
  - Contagem de gêneros: `num_genres = len(genres)`
  - Flag para múltiplos gêneros: `has_multiple_genres = (num_genres > 1)`

- **Classificação em faixas**:
  ```python
  if followers > 150M: 'Ícones Globais'
  elif 100M-150M: 'Superestrelas'
  elif 50M-100M: 'Estrelas Internacionais'
  ...
  elif <50K: 'Descobertas Locais'
  ```

### 3. Modelagem Preditiva
- **Algoritmo selecionado**: Gaussian Naive Bayes
- **Features utilizadas**:
  1. Popularity (0-100)
  2. log_followers (transformação logarítmica)
  3. num_genres (quantidade de gêneros musicais)

- **Processo de treinamento**:
  - Divisão 70/30 (treino/teste)
  - Escalonamento MinMax (0-1)
  - Validação estratificada (mantendo proporção das classes)

### 4. Avaliação de Performance
**Métricas principais**:
- **Acurácia geral**: 94%
- **Precision/Recall por classe**:

| Faixa de Popularidade       | Precision | Recall | F1-Score | Suporte |
|-----------------------------|-----------|--------|----------|---------|
| Alta Popularidade           | 0.83      | 1.00   | 0.91     | 10      |
| Artistas de Nicho           | 0.98      | 0.96   | 0.97     | 328     |
| Descobertas Locais          | 0.99      | 0.97   | 0.98     | 230     |
| Emergentes Globais          | 0.75      | 0.88   | 0.81     | 43      |
| Estrelas Internacionais     | 0.75      | 0.75   | 0.75     | 8       |
| Mainstream Regional         | 0.97      | 0.93   | 0.95     | 402     |
| Nova Cena                   | 0.89      | 0.92   | 0.91     | 92      |
| Promissores                 | 0.84      | 0.93   | 0.88     | 149     |
| Superestrelas               | 0.00      | 0.00   | 0.00     | 2       |

**Principais insights**:
- Excelente performance para classes majoritárias (F1 > 0.95 para as 3 maiores)
- Dificuldade com classes raras (especialmente "Superestrelas")
- Boa generalização para novas amostras (acurácia consistente)

### 5. Visualização e Análise
**Gráficos gerados**:
1. **Distribuição de artistas por faixa**:
   - Mostra concentração em "Mainstream Regional" (27.2%)
   - Revela raridade de "Superestrelas" (0.1%)

2. **Relação Popularidade vs Seguidores**:
   - Clusterização natural por faixas
   - Identificação de outliers interessantes

3. **Matriz de Confusão**:
   - Revela principais confusões entre faixas adjacentes
   - Mostra precisão por classe de forma visual

## 📌 Limitações e Desafios
1. **Problemas identificados**:
   - **Desbalanceamento extremo**: 600× mais artistas na classe principal vs "Superestrelas"
   - **Sobreposição de features**: Faixas adjacentes têm características similares
   - **Dependência de dados externos**: Limitações da API do Spotify

2. **Soluções implementadas**:
   - Transformação logarítmica para normalizar seguidores
   - Agregação de classes minoritárias para análise
   - Cross-validation estratificado para avaliação justa
