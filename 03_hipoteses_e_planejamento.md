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
Os objetos de estudo serão **projetos de software Open-Source escritos em Java**, hospedados no GitHub. O foco são bibliotecas de backend e utilitários que utilizem o framework JUnit 5 e ferramentas de build Maven ou Gradle.

### 8.2 Sujeitos / Participantes
**Não aplicável.** Este é um experimento *in vitro* (automatizado) que analisa artefatos de software. Não haverá participação de seres humanos (desenvolvedores ou testadores) na execução das tarefas, eliminando variáveis ligadas ao fator humano (cansaço, experiência, etc.).

### 8.3 Variáveis Independentes (Fatores)
O fator principal manipulado será a **Técnica de Análise de Qualidade**, com dois níveis categóricos:
1.  Análise Estrutural (Cobertura).
2.  Análise baseada em Falhas (Mutação).

### 8.4 Tratamentos (Condições Experimentais)
Cada projeto será submetido a dois tratamentos distintos:
* **Tratamento 1 (Controle/Padrão):** Execução da suíte de testes monitorada pelo agente **JaCoCo**, utilizando a configuração padrão de contagem de linhas e ramos.
* **Tratamento 2 (Experimental):** Execução da suíte de testes monitorada pelo **PITest**, utilizando o conjunto padrão de operadores de mutação (Defaults).

### 8.5 Variáveis Dependentes (Respostas)
| Variável | Definição | Unidade |
| :--- | :--- | :--- |
| **Line Coverage** | Percentual de linhas de código executadas. | % (0-100) |
| **Mutation Score** | Percentual de mutantes mortos sobre o total gerado. | % (0-100) |
| **Tempo de Execução** | Tempo total gasto pelo processo de build e análise. | Segundos (s) |

### 8.6 Variáveis de Controle
Fatores mantidos constantes ou monitorados para evitar ruído:
* **Versão do Java:** Todos os experimentos rodarão na mesma JDK (versão 17 ou 21) para evitar diferenças de performance da JVM.
* **Hardware:** O ambiente de execução será único (mesmo notebook) para não variar o tempo de processamento.

### 8.7 Variáveis de Confusão
* **Estilo de Teste:** Projetos que usam excesso de "Mocks" podem ter alta cobertura e comportamento imprevisível na mutação.
* **Mitigação:** A seleção dos projetos (Amostragem) buscará evitar repositórios que fujam do padrão de testes unitários clássicos.

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
