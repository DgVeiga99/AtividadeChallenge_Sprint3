# Atividade Challenge Enterprise - Sprint 3 (Continuação da Sprint 2)

## Introdução

Dando continuidade ao projeto da Sprint 2, esta nova etapa teve como foco a adaptação e validação do modelo preditivo para outra cultura agrícola: a **cana-de-açúcar**, utilizando os mesmos fundamentos analíticos e metodológicos aplicados anteriormente na cultura da laranja.

Foi mantido o foco no uso de dados públicos e sensoriamento remoto para prever a produtividade agrícola, agora com testes e treinos voltados à cultura da cana. A mudança da cultura permite avaliar a **generalização e escalabilidade** do modelo proposto.

---

## Dados utilizados

Os arquivos usados para a predição do modelo são:
- [NDVI da região](satveg_planilha.xlsx)
- [Produção de Cana-de-açúcar - 2023](Produção_2023_Sao_Paulo.xlsx)
- [Produção de Cana-de-açúcar - 2022](Produção_2022_Sao_Paulo.xlsx)
- [Produção de Cana-de-açúcar - 2021](Produção_2021_Sao_Paulo.xlsx)

## Pré-processamento e Integração dos Dados

Seguindo os mesmos critérios da fase anterior, foram realizadas as seguintes etapas:

- ✅ **Filtragem específica para a cultura da cana-de-açúcar** nos dados de produção dos anos de 2021, 2022 e 2023.
- ✅ Padronização temporal e geográfica com base em **Ano**, **Mês** e **Município**.
- ✅ Restrição do dataset de NDVI para o intervalo de 2021 a 2023, garantindo compatibilidade temporal.
- ✅ Integração dos dados em um **único dataset unificado**, com as variáveis: `Ano`, `Mês`, `Município`, `NDVI`, `Área colhida (ha)`, `Rendimento (kg/ha)`.

---

## Modelos e Avaliação

Assim como na Sprint 2, foram testados os seguintes algoritmos de regressão:

- **Random Forest Regressor**
- **Decision Tree Regressor**
- **K-Nearest Neighbors**
- **Regressão Linear**
- **SVM** (linear, polinomial e RBF)

### 🔎 Métricas utilizadas:
- R² (Coeficiente de Determinação)
- MAE (Erro Médio Absoluto)
- RMSE (Raiz do Erro Quadrático Médio)
- MAPE (Erro Percentual Absoluto Médio)

---

## Resultados

| Modelo               | R²     | MAE       | RMSE           | MAPE     |
|----------------------|--------|-----------|----------------|----------|
| **Random Forest**    | 0.9634 | 576.01    | 3.059.957,18   | 0.82%    |
| **Decision Tree**    | 0.9559 | 558.07    | 3.678.963,81   | 0.79%    |
| **KNN**              | 0.7026 | 3.659,17  | 24.789.631,35  | 3.88%    |
| Regressão Logística  | 0.6647 | 3.935,55  | 27.954.420,27  | 4.86%    |
| SVM (Polinomial)     | -0.1873| 7.911,20  | 98.980.847,46  | 9.49%    |
| SVM (RBF)            | -0.1898| 7.913,08  | 99.193.849,45  | 9.49%    |

- ✅ **Random Forest** se destacou como o melhor modelo, com alto poder preditivo (R² de 96.34%) e **erro percentual médio inferior a 1%**, evidenciando excelente capacidade de generalização para a cultura da cana-de-açúcar. É o modelo mais indicado para aplicações reais.
- ✅ **Decision Tree** também apresentou excelente desempenho, muito próximo ao do Random Forest. No entanto, sua tendência ao overfitting deve ser considerada em cenários mais complexos.
- ⚠️ **KNN** e **Regressão Logística** obtiveram desempenho intermediário, com erros ainda aceitáveis, mas significativamente maiores. Esses modelos podem ser úteis como referência ou base para ensaios mais simples.
- ❌ **SVM com kernel RBF e Polinomial** apresentaram **R² negativo**, o que indica que os modelos performaram pior que uma simples média. Além disso, os erros foram extremamente elevados, inviabilizando seu uso prático neste cenário.


---

## Conclusão

### 📈 Interpretação dos Resultados

Os modelos treinados demonstraram que o uso do **NDVI como variável preditiva da produtividade agrícola da cana-de-açúcar** é viável e eficiente. O **Random Forest** e o **Decision Tree** obtiveram **MAPE inferior a 1%**, indicando que o índice de vegetação por diferença normalizada (NDVI) se correlaciona fortemente com a produtividade da cultura em questão.

A performance superior desses dois modelos é justificada por sua capacidade de capturar **relações não-lineares complexas** entre as variáveis. Já modelos como SVM e Regressão Logística apresentaram desempenho inferior, com altos erros e baixa explicabilidade estatística (R² negativo), demonstrando limitação diante da natureza do problema.

### 📌 Análise de Cenários de Desempenho

Os modelos apresentaram **melhor desempenho nas regiões e períodos em que os valores de NDVI estavam mais estáveis e bem distribuídos**, refletindo uma vegetação saudável e uma coleta precisa. Por outro lado, o desempenho caiu em regiões onde:
- **A variabilidade climática foi alta** (ex. secas prolongadas, geadas inesperadas);
- **A coleta NDVI apresentou ruído**, seja por imagens de baixa qualidade ou frequências insuficientes;
- **Houve falhas na consistência dos dados produtivos municipais**, oriundos de bases públicas com preenchimento incompleto ou genérico.

### 🌦️ Fatores Externos que Podem Ter Influenciado a Produtividade

1. **Eventos climáticos extremos**, como secas severas e geadas, podem impactar drasticamente a produtividade e mascarar correlações NDVI-produtividade.
2. **Pragas e doenças agrícolas** não são capturadas diretamente pelo NDVI e podem comprometer a acurácia preditiva, principalmente em áreas com surtos localizados.
3. **Qualidade das imagens NDVI**: resoluções temporais ou espaciais baixas dificultam a identificação precisa de estágios fenológicos, afetando diretamente os modelos.

### 🧠 Sugestões de Melhoria para o Modelo de IA

- **Incorporação de novas variáveis**: adicionar variáveis meteorológicas (precipitação, temperatura, umidade relativa), dados de solo, histórico de uso agrícola e ocorrências fitossanitárias pode enriquecer o modelo.
- **Melhorar o tratamento das imagens NDVI**, realizando limpeza de outliers, interpolações de lacunas e agrupamento temporal por estágio da cultura.
- **Refinar o período de coleta**: o uso de médias mensais ou agregações por estágio de desenvolvimento da cana (brotamento, crescimento, maturação) pode melhorar a correlação com a produtividade real.

### ⚠️ Limitações da Análise

- **Tamanho amostral limitado** (apenas 3 anos de dados e um recorte geográfico específico) pode não capturar todas as variações interanuais e regionais.
- **Qualidade das bases públicas**: falhas nos registros municipais e na atualização dos dados podem comprometer a robustez estatística.
- **Modelos estatísticos supervisionados simples**: embora eficientes, não incorporam componentes espaciais ou temporais avançados, como séries temporais multivariadas ou redes neurais profundas.

---

Em suma, a metodologia aplicada mostrou-se **robusta e escalável**, com forte potencial de uso real no planejamento agrícola. A acurácia alcançada com modelos como Random Forest reforça a utilidade do NDVI, mas também evidencia a necessidade de **complementar a base de dados** e considerar **variáveis externas críticas** para um modelo ainda mais completo e resiliente às variações naturais do campo.

---
