# Entrega 2: Escopo, GQM, Métricas e Riscos

## 3. Objetivos e Questões (GQM - Goal / Question / Metric)

### 3.1 Objetivo Geral (Goal Template)
**Analisar** a eficácia da métrica de Cobertura de Código (Code Coverage) comparada à Pontuação de Mutação (Mutation Score)
**Com o propósito de** avaliar a confiabilidade dos testes automatizados e identificar a "ilusão de qualidade"
**Sob a perspectiva** do Engenheiro de Software e Engenheiro de Qualidade (QA)
**No contexto de** projetos open-source em Java utilizando JUnit 5.

### 3.2 Tabela GQM (Objetivos Específicos, Questões e Métricas)

| Objetivos Específicos | Questões de Pesquisa (Q) | Métricas Associadas (M) |
| :--- | :--- | :--- |
| **O1. Analisar a correlação entre Cobertura e Robustez**<br>Verificar se existe relação direta entre cobrir linhas e matar mutantes. | **Q1.1** Projetos com alta cobertura de linha (>80%) apresentam pontuação de mutação proporcionalmente alta? | M1. Line Coverage (%)<br>M2. Mutation Score (%) |
| | **Q1.2** Existem casos de "falsos positivos" onde a cobertura é alta mas a pontuação de mutação é baixa (<50%)? | M1. Line Coverage (%)<br>M2. Mutation Score (%) |
| | **Q1.3** A cobertura de desvio (branch) é um preditor melhor para a pontuação de mutação do que a cobertura de linha? | M3. Branch Coverage (%)<br>M2. Mutation Score (%) |
| **O2. Avaliar o custo computacional (Trade-off)**<br>Comparar o impacto no tempo de execução entre as técnicas. | **Q2.1** Qual é a sobrecarga temporal (overhead) introduzida pelo teste de mutação em relação à execução normal dos testes? | M4. Test Execution Time (ms)<br>M5. Mutation Analysis Time (ms) |
| | **Q2.2** O tamanho do projeto (linhas de código) influencia exponencialmente o tempo de análise de mutação? | M6. Lines of Code (LOC)<br>M5. Mutation Analysis Time (ms) |
| | **Q2.3** O número de classes de teste impacta mais o tempo de cobertura ou o tempo de mutação? | M7. Number of Test Classes<br>M5. Mutation Analysis Time (ms) |
| **O3. Investigar a natureza dos defeitos não detectados**<br>Entender o que compõe a "falta de qualidade". | **Q3.1** Qual é a proporção de mutantes que sobrevivem mesmo em linhas cobertas pelos testes? | M8. Surviving Mutants Count<br>M1. Line Coverage (%) |
| | **Q3.2** A complexidade do código fonte (Ciclomática) aumenta a taxa de sobrevivência dos mutantes? | M9. Cyclomatic Complexity (Avg)<br>M8. Surviving Mutants Count |
| | **Q3.3** Testes que cobrem mais métodos tendem a matar mais mutantes? | M10. Methods Covered Count<br>M2. Mutation Score (%) |
| **O4. Analisar a eficiência da suíte de testes**<br>Verificar a densidade de eficácia dos testes criados. | **Q4.1** Qual a relação entre a quantidade de testes unitários e a pontuação de mutação obtida? | M11. Number of Test Methods<br>M2. Mutation Score (%) |
| | **Q4.2** A força da suíte de testes (Mutation Score) varia significativamente entre diferentes projetos analisados? | M2. Mutation Score (%)<br>M12. Project ID / Category |
| | **Q4.3** É viável estabelecer um limiar mínimo de Mutation Score para pipelines de CI/CD baseado nos dados coletados? | M5. Mutation Analysis Time (ms)<br>M2. Mutation Score (%) |

---

## 3.3 Definição das Métricas

Abaixo estão listadas todas as métricas utilizadas no GQM acima.

| ID | Nome da Métrica | Descrição | Unidade | Fonte de Coleta |
| :--- | :--- | :--- | :--- | :--- |
| **M1** | Line Coverage | Porcentagem de linhas de código executadas durante os testes. | Percentual (%) | Relatório JaCoCo |
| **M2** | Mutation Score | Porcentagem de mutantes mortos (detectados) sobre o total de mutantes gerados. | Percentual (%) | Relatório PITest |
| **M3** | Branch Coverage | Porcentagem de ramificações (if/else, switch) executadas. | Percentual (%) | Relatório JaCoCo |
| **M4** | Test Execution Time | Tempo total para rodar a suíte de testes padrão (JUnit). | Milissegundos (ms) | Log JUnit/Maven |
| **M5** | Mutation Analysis Time | Tempo total para rodar a análise de mutação completa. | Milissegundos (ms) | Log PITest |
| **M6** | Lines of Code (LOC) | Total de linhas de código fonte Java (não vazias/comentários) do projeto. | Inteiro (Un) | Plugin de Análise/Sonar |
| **M7** | Number of Test Classes | Quantidade de classes Java dedicadas a testes no projeto. | Inteiro (Un) | Análise Estática |
| **M8** | Surviving Mutants Count | Número absoluto de mutantes que não foram detectados pelos testes. | Inteiro (Un) | Relatório PITest |
| **M9** | Cyclomatic Complexity | Média da complexidade ciclomática (caminhos independentes) dos métodos. | Inteiro (Un) | Relatório JaCoCo/Metrics |
| **M10** | Methods Covered Count | Número absoluto de métodos que foram invocados durante os testes. | Inteiro (Un) | Relatório JaCoCo |
| **M11** | Number of Test Methods | Quantidade total de métodos anotados com `@Test`. | Inteiro (Un) | Análise Estática |
| **M12** | Project Category | Categoria do projeto (ex: Utils, Web, Lib) para agrupamento. | Nominal (Texto) | Classificação Manual |

---

## 4. Escopo e Contexto do Experimento

### 4.1 Escopo Funcional (Inclusões e Exclusões)

| Escopo | Descrição |
| :--- | :--- |
| **Incluído (In-Scope)** | 1. Análise de código fonte em linguagem **Java** (versões 11 a 21).<br>2. Projetos que utilizam **JUnit 5** para testes unitários.<br>3. Ferramentas de automação de build: **Maven** ou **Gradle**.<br>4. Uso do **JaCoCo** para métricas de cobertura clássica.<br>5. Uso do **PITest** para métricas de mutação.<br>6. Testes unitários de backend/lógica de negócio. |
| **Excluído (Out-Scope)** | 1. Testes de Interface de Usuário (Selenium, Frontend).<br>2. Testes de Integração que dependam de Banco de Dados externo ou APIs reais (para evitar instabilidade).<br>3. Projetos em outras linguagens (Kotlin, Python, etc).<br>4. Análise manual de código (Code Review). |

### 4.2 Contexto do Estudo
* **Tipo de Organização:** Acadêmica / Pesquisa Experimental.
* **Objeto:** Projetos Open-Source hospedados no GitHub com boa reputação (estrelas) e atividade recente.
* **Perfil:** Projetos de bibliotecas e utilitários, pois possuem lógica de negócio isolada, ideal para testes unitários puros.

---

## 5. Stakeholders e Impacto Esperado

### 5.1 Stakeholders Principais
1.  **Engenheiros de Software (Devs):** Interessados em saber se seus testes atuais são confiáveis.
2.  **Engenheiros de QA / SDETs:** Interessados em adotar novas ferramentas para garantir qualidade além da cobertura.
3.  **Gerentes de Engenharia / Tech Leads:** Interessados no custo-benefício (tempo de build vs. qualidade) da adoção de testes de mutação.

### 5.2 Impacto Esperado
* **No Processo:** Evidenciar que a métrica de cobertura sozinha é insuficiente para aprovar Pull Requests críticos.
* **No Produto:** Potencial redução de bugs em produção ao identificar "testes fracos" que precisam ser refatorados para matar mutantes.
* **Na Cultura:** Mudança de mindset de "quantidade de testes" para "qualidade/assertividade de testes".

---

## 6. Riscos de Alto Nível, Premissas e Critérios de Sucesso

### 6.1 Riscos de Alto Nível
1.  **Risco Técnico (Performance):** O PITest pode demorar excessivamente (horas) em projetos grandes, inviabilizando a coleta de dados em tempo hábil.
    * *Mitigação:* Limitar o escopo da mutação apenas para classes críticas ou usar projetos menores.
2.  **Risco de Compatibilidade:** Alguns projetos open-source podem falhar o build ao injetar o plugin do PITest devido a conflitos de dependências.
    * *Mitigação:* Selecionar uma lista maior de projetos candidatos (10+) para garantir que pelo menos 3 a 5 funcionem perfeitamente.
3.  **Risco de "Mutantes Equivalentes":** O PITest pode gerar mutantes que são logicamente equivalentes ao original e não podem ser mortos, distorcendo a métrica.
    * *Mitigação:* Aceitar uma margem de erro estatístico comum na literatura.

### 6.2 Premissas
1.  Os projetos selecionados no GitHub possuem suítes de testes funcionais (build passando no estado atual).
2.  O hardware disponível (notebook pessoal) tem memória RAM suficiente para rodar a análise de mutação.
3.  As ferramentas (JaCoCo e PITest) manterão compatibilidade com as versões do Java utilizadas nos projetos.

### 6.3 Critérios de Sucesso Globais
1.  **Coleta de Dados:** Conseguir executar o experimento completo em pelo menos **3 projetos** distintos.
2.  **Análise:** Gerar tabelas comparativas onde seja possível visualizar claramente a diferença entre % de Cobertura e % de Mutação.
3.  **Conclusão:** Ser capaz de responder às questões de pesquisa (Q1 a Q4) com base nos dados, confirmando ou refutando a hipótese de correlação.
