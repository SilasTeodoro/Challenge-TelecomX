# 📈 Análise de Churn de Clientes - Challenge Telecom X - Alura

##  Sobre o Projeto  
Este projeto tem o objetivo de realizar as análises das razões que os clientes cancelaram (Churn) o serviço da TelecomX. Após uma análise aprofundada, notou-se alguns padrões demográficos, de tipos de contratos e métodos de pagamentos que possivelmente influenciou os clientes a cancelarem o serviço.

---

# Relatório Detalhado de Análise de Churn de Clientes

## 1. Introdução
Este relatório apresenta uma análise detalhada da taxa de *churn* (evasão de clientes) de uma empresa de telecomunicações, baseando-se no conjunto de dados `TelecomX_Data.json`. O objetivo principal é identificar os principais fatores que contribuem para a evasão de clientes, fornecendo *insights* acionáveis e recomendações para a retenção de clientes.

## 2. Limpeza e Tratamento de Dados
A fase de limpeza e tratamento de dados envolveu as seguintes etapas:
*   **Extração e Normalização**: Os dados, inicialmente em formato JSON aninhado, foram extraídos e normalizados em um único DataFrame (`df_normalized`) para facilitar a análise. Colunas aninhadas como 'customer', 'phone', 'internet' e 'account' foram expandidas.
*   **Verificação de Valores Nulos e Duplicados**: Foi verificado que não havia valores nulos evidentes inicialmente, nem linhas duplicadas. No entanto, uma verificação mais aprofundada revelou:
    *   11 valores em branco na coluna `Charges.Total`.
    *   224 valores em branco na coluna `Churn`.
*   **Tratamento de Tipos de Dados**:
    *   A coluna `Charges.Total` foi identificada como tipo 'object' e convertida para 'float'. Os valores em branco que resultaram em `NaN` durante a conversão foram preenchidos com a mediana da coluna para evitar distorções por valores extremos.
    *   Os valores em branco na coluna `Churn` foram substituídos por `NaN` e, posteriormente, preenchidos com a moda da coluna ('Não'), considerando que a maioria dos clientes não realiza *churn*. A coluna `Churn` foi convertida para tipo 'category'.
*   **Criação de Novas Features**: Uma nova coluna, `Contas_Diarias`, foi criada dividindo `Cobranca_Mensal` pela média de dias em um mês contando com o ano bissexto a cada quatro anos (30.4375) para uma perspectiva diária de gastos.
*   **Renomeação e Padronização de Colunas e Valores**: As colunas foram renomeadas para termos mais claros e em português (ex: `customerID` para `ID`, `gender` para `Genero`). Além disso, os valores nas colunas categóricas foram padronizados para termos em português (ex: 'Yes' para 'Sim', 'No' para 'Não', 'Female' para 'Feminino', 'Male' para 'Masculino').

## 3. Análise Exploratória de Dados
A análise exploratória revelou a distribuição geral do *churn* e a relação com diversas variáveis categóricas e numéricas.

### 3.1. Distribuição Geral de Churn
A análise inicial da coluna `Churn` mostrou que:
*   **5398 clientes** (74.28%) não realizaram *churn*.
*   **1869 clientes** (25.72%) realizaram *churn*.
Esta proporção indica que a evasão é um problema significativo, afetando aproximadamente um quarto da base de clientes. A visualização desta distribuição foi feita através de um gráfico de barras.
<img src="images/distribuicao de churn por cliente.png" width="500">

### 3.2. Churn por Variáveis Categóricas
A análise de *churn* por categorias chave destacou os seguintes pontos principais:<br>
*  **Idoso_60+**: Clientes idosos (`Idoso_60+ = Sim`) têm uma taxa de churn significativamente maior (40.27%) em comparação com clientes não idosos (22.89%).<br><img src="images/proporcao de churn por idoso60+.png" width="500">
*  **Possui_Parceiro/Possui_Dependentes**: Clientes sem parceiro (32.01%) e sem dependentes (30.34%) apresentam taxas de churn mais altas.<br><img src="images/Proporcao de churn por possui parceiro.png" width="500"> <img src="images/Proporcao de churn por possui dependentes.png" width="500"><br>
*  **Servico_Internet**: Clientes com serviço de internet "Fibra optica" têm a maior taxa de churn (40.56%), enquanto aqueles sem serviço de internet ('Não') têm a menor (7.15%).<br><img src="images/Proporcao de churn por servico de internet.png" width="500">
*  **Serviços Adicionais (OnlineSecurity, OnlineBackup, DeviceProtection, TechSupport)**: Clientes que *não* possuem serviços adicionais mostram taxas de *churn* mais elevadas. Por outro lado, aqueles que não têm serviço de internet ('Sem Servico de Internet') para estes itens têm uma taxa de churn muito menor, e clientes que *possuem* esses serviços têm taxas menores que a média global.<br><img src="images/Proporcao de churn por seguranca online.png" width="500"><img src="images/Proporcao de churn por backup online.png" width="500"><img src="images/Proporcao de churn por protecao dispositivo.png" width="500"><img src="images/Proporcao de churn por suporte tecnico.png" width="500">
*  **Contrato**: Clientes com contrato "Mensal" têm uma taxa de *churn* alarmantemente alta (41.32%) em comparação com contratos de "Um Ano" (10.93%) e "Dois Anos" (2.75%).<br><img src="images/Proporcao de churn por tipo de contrato.png" width="500">
*  **Fatura_Digital**: Clientes que optam pela "Fatura_Digital" (32.48%) têm uma taxa de *churn* maior do que aqueles que não a utilizam (15.87%).<br><img src="images/proporcao de churn por fatura digital.png" width="500">
*  **Metodo_Pagamento**: O método de pagamento "Cheque eletrônico" está associado à maior taxa de *churn* (43.80%).<br><img src="images/proporcao de churn por metodo pagamento.png" width="500"><br>

### 3.3. Churn por Variáveis Numéricas
A análise das variáveis numéricas (`Tempo_Contrato`, `Cobranca_Mensal`, `Cobranca_Total`, `Contas_Diarias`) em relação ao *churn* forneceu *insights* valiosos.

#### Sumário das Observações sobre Churn e Variáveis Numéricas
*   **Tempo_Contrato**: Clientes que não churnaram tendem a ter um tempo de contrato significativamente maior (média de 37 meses) em comparação com clientes que churnaram (média de 18 meses).<br><img src="images/Tendência de Churn por Tempo de Contrato.png" width="1000">
*   **Cobranca_Mensal**: Clientes que churnaram apresentam uma cobrança mensal média ligeiramente maior (74.44) do que clientes que não churnaram (61.35).<br><img src="images/distribuicao de cobranca mensal por churn.png" width="500">
*   **Cobranca_Total**: Clientes que não churnaram têm uma cobrança total média significativamente mais alta (2538.10) do que clientes que churnaram (1531.80).<br><img src="images/distribuicao de cobranca total por churn.png" width="500">
*   **Contas_Diarias**: Similar à 'Cobranca_Mensal', as `Contas_Diarias` também são ligeiramente maiores para clientes que churnaram (média de 2.45) em comparação com clientes que não churnaram (média de 2.02).<br><img src="images/distribuicao de contas diarias por churn.png" width="500">

**Em resumo:** Clientes que churnam tendem a ter um *tempo de contrato mais curto*, mas pagam *cobranças mensais e diárias ligeiramente mais altas*. Clientes leais (que não churnam) permanecem por mais tempo e, consequentemente, acumulam cobranças totais muito maiores. Isso sugere que fatores relacionados ao custo-benefício dos serviços mensais e a duração do relacionamento com a empresa são fortes indicadores de churn.

## 4. Conclusões e Insights
A análise revelou que o perfil do cliente propenso ao *churn* é complexo, mas alguns padrões claros emergiram:
*   **Duração do Contrato:** Clientes com contratos mensais são significativamente mais propensos a churnar. Contratos mais longos demonstram uma forte relação com a retenção.
*   **Serviços de Internet e Adicionais:** A "Fibra Óptica" está associada a uma alta taxa de *churn*. A ausência de serviços de segurança (Segurança Online, Backup, Proteção de Dispositivo, Suporte Técnico) aumenta drasticamente a probabilidade de *churn*.
*   **Custos Mensais:** Clientes com cobranças mensais mais altas são um pouco mais propensos a churnar.
*   **Método de Pagamento:** O "Cheque eletrônico" é um método de pagamento associado a uma alta taxa de *churn*.
*   **Demografia:** Clientes idosos, sem parceiros e sem dependentes mostram maior propensão à evasão.

## 5. Recomendações
Com base nos *insights* obtidos, as seguintes recomendações são propostas para reduzir a taxa de *churn*:

1.  **Incentivar Contratos de Longo Prazo**:
    *   Oferecer descontos progressivos ou benefícios exclusivos para clientes que optam por contratos de 1 ou 2 anos.
    *   Criar programas de fidelidade que recompensem a permanência do cliente.
    *   Comunicar claramente os benefícios e a economia a longo prazo de contratos estendidos.

2.  **Melhorar a Proposta de Valor da Fibra Óptica**:
    *   Investigar os motivos específicos de *churn* para clientes de fibra óptica (qualidade do serviço, suporte, preço).
    *   Oferecer pacotes mais atraentes ou serviços adicionais gratuitos para clientes de fibra óptica.

3.  **Promover e Integrar Serviços de Segurança e Suporte**:
    *   Campanhas de marketing que destaquem os benefícios da Segurança Online, Backup, Proteção do Dispositivo e Suporte Técnico.
    *   Considerar a inclusão de serviços básicos de segurança ou suporte técnico em pacotes de internet.

4.  **Otimizar a Estratégia de Preços e Pacotes**:
    *   Reavaliar a estrutura de preços dos serviços, especialmente para pacotes com Cobranca Mensal mais alta.
    *   Oferecer opções de pacotes mais flexíveis ou personalizados.

5.  **Monitorar Clientes com "Cheque Eletrônico"**:
    *   Identificar se há problemas com o processo de "Cheque eletrônico", se este método de pagamento é preferido por um segmento de clientes mais propenso a *churn* ou analisar se este método já é ultrapassado e deveria ser substituído.
    *   Considerar incentivar outros métodos de pagamento mais estáveis e automáticos.

6.  **Programas Direcionados para Idosos e Clientes Sem Vínculos**:
    *   Desenvolver pacotes ou serviços específicos para clientes idosos, solteiros ou sem dependentes.
    *   Oferecer suporte ao cliente mais personalizado para esses segmentos.

Essas recomendações visam não apenas reduzir o *churn* existente, mas também construir um relacionamento mais sólido e duradouro com a base de clientes da TelecomX.

---

## 🛠️ Tecnologias Utilizadas

* ![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54) Linguagem principal.
* ![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white) Manipulação de dados.
* ![NumPy](https://img.shields.io/badge/numpy-%23013243.svg?style=for-the-badge&logo=numpy&logoColor=white) Cálculos numéricos.
* ![Matplotlib](https://img.shields.io/badge/Matplotlib-%23ffffff.svg?style=for-the-badge&logo=Matplotlib&logoColor=black) Visualizações estáticas.
* ![Seaborn](https://img.shields.io/badge/Seaborn-4D88FF?style=for-the-badge&logo=python&logoColor=white) Visualizações estatísticas refinadas.
* ![Google Colab](https://img.shields.io/badge/Google%20Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white) Ambiente de desenvolvimento.

---

## 📂 Estrutura do Repositório

* `images/`: Gráficos gerados para o relatório.
* `TelecomX_BR.ipynb`: Código completo e análises.
* `TelecomX_Data`: Base de dados [Dados Fictícios]
* `TelecomX_dicionario`: Estrutura dos Dados

---

**Desenvolvido por:** Silas Teodoro da Silva.<br>
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/SilasTeodoro)[![LinkedInd](https://img.shields.io/badge/LinkedIn-100000?style=for-the-badge&logo=github&logoColor=white)](https://www.linkedin.com/in/silas-teodoro/)
