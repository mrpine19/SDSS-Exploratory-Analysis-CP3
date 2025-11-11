# CP3 - Análise Exploratória e Modelo Supervisionado para Previsão de Redshift

## Visão Geral do Projeto

Este projeto consiste em uma análise exploratória de dados (EDA) e a construção de um modelo de regressão para prever o **redshift** de objetos celestes (galáxias e quasares) utilizando o dataset **Stellar Classification Dataset - SDSS17**. O objetivo principal é demonstrar a capacidade de prever distâncias cosmológicas com base em propriedades fotométricas.

## Contextualização do Dataset e Redshift

O dataset **SDSS17** é derivado do *Sloan Digital Sky Survey*, um dos maiores levantamentos astronômicos. Ele contém informações sobre diversas propriedades de objetos celestes.

### O que é Redshift?

**Redshift (desvio para o vermelho)** é uma medida crucial em cosmologia que indica o quanto a luz de um objeto distante foi "esticada" para comprimentos de onda mais longos devido à expansão do universo. Em termos simples:
-   **Maior redshift = Objeto mais distante e mais antigo.**
-   É fundamental para mapear a estrutura do universo e entender a evolução de galáxias e quasares.

### Classes de Objetos no Dataset:
-   **GALAXY (Galáxia):** Sistemas de estrelas, gás, poeira e matéria escura.
-   **STAR (Estrela):** Corpos celestes luminosos. O redshift estelar é local e não cosmológico.
-   **QSO (Quasar - Objeto Quase Estelar):** Núcleos galácticos ativos extremamente luminosos, observáveis a grandes distâncias e com redshifts muito elevados.

### Features Principais:
-   **Variáveis Preditivas (Fotometria):** `u`, `g`, `r`, `i`, `z` (magnitudes em diferentes filtros de luz).
-   **Variável Alvo:** `redshift` (valor contínuo a ser previsto).
-   **Variável Categórica:** `class` (GALAXY, STAR, QSO).
-   **Variáveis Descartadas:** `obj_id`, `run_id`, `rerun_id`, `cam_col`, `field_id`, `spec_obj_id`, `plate`, `mjd`, `fiber_id`, `alpha`, `delta` (metadados e coordenadas sem poder preditivo para redshift).

## Metodologia

O projeto seguiu as seguintes etapas:

1.  **Limpeza Inicial:**
    *   Remoção de colunas de metadados e identificadores (`obj_id`, `run_id`, etc.).
    *   Padronização dos nomes das colunas para minúsculas.

2.  **Análise Descritiva e Tratamento de Dados:**
    *   Geração de estatísticas descritivas para as variáveis numéricas.
    *   Verificação de dados faltantes (o dataset não apresentou valores nulos).
    *   Análise visual da distribuição de `redshift` por classe (boxplot), revelando que estrelas possuem redshift próximo de zero.
    *   **Decisão:** Remoção da classe 'STAR' do dataset para focar na previsão de redshift cosmológico de galáxias e quasares.
    *   Análise de "outliers" no redshift para GALAXY e QSO. Concluiu-se que esses valores são observações legítimas de objetos distantes e foram mantidos.

3.  **Preparação para Modelagem:**
    *   Remoção das coordenadas celestes (`alpha`, `delta`), pois não contribuem para a previsão do redshift.
    *   Codificação da variável categórica `class` (one-hot encoding) para `class_qso`.
    *   Divisão do dataset em conjuntos de treino (80%) e teste (20%).
    *   Normalização dos dados numéricos usando `StandardScaler` (ajustado no treino e aplicado no teste).

4.  **Treinamento do Modelo:**
    *   Utilização de um modelo de **Regressão Linear** (`LinearRegression` do scikit-learn).

5.  **Avaliação do Modelo:**
    *   Cálculo das métricas de desempenho no conjunto de teste: R² (Coeficiente de Determinação), MAE (Erro Absoluto Médio) e RMSE (Raiz do Erro Quadrático Médio).

## Resultados e Conclusão

A questão central do projeto foi: **"É possível construir um modelo que utilize apenas as magnitudes fotométricas (`u`, `g`, `r`, `i`, `z`) para prever o valor contínuo do `redshift` de galáxias e quasares?"**

A resposta é **afirmativa**. O modelo de Regressão Linear demonstrou um **poder preditivo significativo**, validando a forte relação entre as propriedades de cor de um objeto celeste e sua distância cosmológica.