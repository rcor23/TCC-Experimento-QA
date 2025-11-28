# Entrega 3: Modelo Conceitual, Hipóteses e Desenho Experimental
**Aluno:** Ryan Cristian Oliveira Rezende
**Data:** 28/11/2025

---

## 7. Modelo Conceitual e Hipóteses

### 7.1 Modelo Conceitual
O modelo conceitual deste experimento baseia-se na premissa de que métricas de teste diferentes fornecem visões distintas sobre a qualidade do software.
Espera-se que a **Cobertura de Código** (Variável Independente 1) tenha uma correlação fraca ou moderada com o **Mutation Score** (Variável Dependente / Proxy de Robustez), indicando que a cobertura tradicional não é suficiente para prever a capacidade da suíte de testes de detectar falhas reais. Além disso, espera-se que o **Tempo de Execução** (Variável Dependente de Custo) seja significativamente maior para a análise de mutação.

**Esquema do Modelo:**
`[Código Fonte + Testes] -> (Técnica Aplicada) -> [Métricas Resultantes]`

* **Entrada:** Projetos Java (Open Source).
* **Fator (Técnica):** Análise de Cobertura (JaCoCo) vs. Análise de Mutação (PITest).
* **Saída:** % Cobertura, % Mutation Score, Tempo de Execução.

### 7.2 Hipóteses Formais
As hipóteses buscam responder se a cobertura de código é um preditor confiável da robustez dos testes.

#### Hipótese 1: Correlação entre Cobertura e Eficácia (Mutação)
* **H0 (Nula):** Existe uma correlação forte e positiva ($\rho \geq 0.8$) entre a Cobertura de Linha (Line Coverage) e a Pontuação de Mutação (Mutation Score).
    * *Interpretação:* Se H0 for verdadeira, cobertura alta já garante qualidade, tornando a mutação redundante.
* **H1 (Alternativa):** A correlação entre Cobertura de Linha e Pontuação de Mutação é fraca ou moderada ($\rho < 0.8$).
    * *Interpretação:* Se H1 for verdadeira, confirma-se a "ilusão de qualidade", onde alta cobertura não garante testes robustos.

#### Hipótese 2: Custo Computacional
* **H0 (Nula):** Não há diferença estatisticamente significativa no tempo de execução entre a coleta de cobertura e a análise de mutação.
* **H1 (Alternativa):** O tempo de execução da análise de mutação é significativamente superior (em ordens de magnitude) ao da coleta de cobertura.

---

## 8. Variáveis, Fatores, Tratamentos e Objetos de Estudo

### 8.1 Objetos de Estudo
Os objetos de estudo serão **projetos de software Open-Source escritos em Java**, hospedados no GitHub, que utilizem o framework JUnit 5 e ferramentas de build Maven ou Gradle. Serão selecionados projetos com diferentes tamanhos (LOC) e domínios (ex: bibliotecas utilitárias, parsers).

### 8.2 Tabela de Variáveis
Abaixo estão descritas todas as variáveis monitoradas no experimento.

| Tipo de Variável | Nome da Variável | Descrição | Escala / Unidade |
| :--- | :--- | :--- | :--- |
| **Independente** | Técnica de Avaliação | A abordagem utilizada para medir a qualidade dos testes. | Nominal (JaCoCo / PITest) |
| **Dependente** | Line Coverage | Percentual de linhas executadas pelos testes. | Razão (0-100%) |
| **Dependente** | Mutation Score | Percentual de mutantes "mortos" (detectados) pelos testes. | Razão (0-100%) |
| **Dependente** | Tempo de Execução | Tempo total gasto para rodar a análise completa. | Razão (milissegundos/segundos) |
| **Controle** | Tamanho do Projeto (LOC) | Quantidade de linhas de código do projeto, usada para normalizar comparações. | Inteiro (Linhas) |
| **Controle** | Complexidade Ciclomática | Complexidade média dos métodos analisados. | Inteiro |
| **Controle** | Versão do Java | Versão da JDK utilizada para garantir consistência. | Nominal (Ex: JDK 17) |

### 8.3 Tabela de Fatores e Tratamentos
O experimento possui um fator principal (a técnica de análise) com dois níveis (tratamentos).

| Fator | Tratamentos (Níveis) | Descrição do Tratamento |
| :--- | :--- | :--- |
| **Técnica de Análise de Qualidade** | **T1: Cobertura Tradicional** | Execução da suíte de testes monitorada pelo agente **JaCoCo**, gerando relatório de linhas e ramos cobertos. |
| | **T2: Teste de Mutação** | Execução da suíte de testes monitorada pelo **PITest**, com geração de mutantes padrão e verificação de sobrevivência. |

**Combinações:**
Como é um experimento de comparação direta (paired design), cada objeto de estudo passará por ambos os tratamentos.
* **Combinação 1:** Projeto X + Tratamento T1 (JaCoCo).
* **Combinação 2:** Projeto X + Tratamento T2 (PITest).

---

## 9. Desenho Experimental

### 9.1 Tipo de Desenho
O experimento seguirá um desenho **"One Factor with Two Treatments" (Um Fator com Dois Tratamentos)** em um arranjo de **Medidas Repetidas (Paired Design)**.
Isso significa que o **mesmo projeto** será submetido às duas técnicas (JaCoCo e PITest). Isso elimina a variabilidade entre projetos diferentes, permitindo comparar diretamente como as métricas se comportam sobre o mesmo código base.

### 9.2 Randomização e Alocação
* **Seleção dos Objetos:** A seleção dos projetos open-source será feita por **amostragem de conveniência** baseada em critérios técnicos (ter testes JUnit 5 rodando, build estável), buscando representar projetos comuns de backend.
* **Ordem de Execução:** Como as técnicas não interferem no código fonte original (apenas leitura ou geração de bytecode temporário), a ordem de execução (JaCoCo primeiro ou PITest primeiro) não gera viés de aprendizado, dispensando randomização de ordem.

### 9.3 Balanceamento
O desenho é naturalmente balanceado, pois 100% da amostra (todos os projetos selecionados) receberão ambos os tratamentos. Não haverá grupo que recebe apenas um tratamento.

### 9.4 Procedimento de Execução (Visão Macro)
Para cada projeto selecionado:
1.  Clone do repositório e verificação do build (`mvn clean test`).
2.  **Execução T1:** Configuração e rodagem do plugin JaCoCo. Coleta de métricas de cobertura e tempo.
3.  **Execução T2:** Configuração e rodagem do plugin PITest. Coleta de Mutation Score e tempo.
4.  Registro dos dados em planilha para análise comparativa.
