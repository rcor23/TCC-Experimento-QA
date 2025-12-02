# Entrega 4: População, Protocolo e Plano de Análise
**Aluno:** Ryan Cristian Oliveira Rezende
**Data:** 01/12/2025

---

## 10. População, Sujeitos e Amostragem

### 10.1 População-alvo
A população deste estudo consiste em **projetos de software open-source desenvolvidos em Java**, hospedados publicamente na plataforma GitHub. O foco são bibliotecas (libs), frameworks leves e utilitários de backend que possuam suítes de testes automatizados.

### 10.2 Critérios de Inclusão e Exclusão
Para garantir a viabilidade técnica do experimento no meu ambiente local (Notebook pessoal), os projetos devem atender aos seguintes filtros:

* **Inclusão (Obrigatório):**
    * Linguagem predominante: Java (versão 8 a 21).
    * Sistema de Build: Maven (`pom.xml`) ou Gradle (`build.gradle`).
    * Framework de Teste: JUnit 4 ou JUnit 5.
    * Status do Build: O projeto deve compilar e passar nos testes atuais sem erros ("Build Passing").
* **Exclusão (Descartados):**
    * Projetos Android (devido à complexidade de emulação).
    * Projetos excessivamente grandes (mais de 50.000 linhas de código), pois inviabilizariam o tempo de execução do PITest.
    * Projetos sem testes unitários ou com testes que dependem de bancos de dados externos (Testes de Integração complexos).

### 10.3 Método de Amostragem e Tamanho
Será utilizada uma **Amostragem de Conveniência**.
Selecionarei entre **3 a 5 projetos** relevantes do GitHub (baseado em número de estrelas e atividade recente) que se encaixem nos critérios acima.
*Justificativa:* Como o teste de mutação é intensivo computacionalmente, uma amostra menor, porém analisada em profundidade, é mais adequada para o escopo deste TCC do que uma análise massiva superficial.

---

## 11. Instrumentação e Protocolo Operacional

### 11.1 Instrumentos de Coleta
1.  **Ambiente de Execução:** Notebook Pessoal (Windows 11, Intel Core i5, 16GB RAM).
2.  **IDE/Editor:** IntelliJ IDEA ou VS Code (para inspeção do código).
3.  **Ferramentas de Análise:**
    * **JaCoCo (via Maven Plugin):** Versão estável mais recente.
    * **PITest (via Maven Plugin):** Versão estável mais recente.
4.  **Planilha de Registro:** Microsoft Excel/Google Sheets para consolidar os dados extraídos dos relatórios HTML/XML.

### 11.2 Fluxograma do Experimento
O diagrama abaixo ilustra o passo a passo da operação para cada projeto selecionado.

```mermaid
graph TD
    A([Início]) --> B[Selecionar Repositório Java no GitHub]
    B --> C{Build e Testes<br>Passaram?}
    C -- Não --> D[Descartar Projeto]
    D --> B
    C -- Sim --> E[Configurar Plugin JaCoCo]
    E --> F[Executar: mvn clean test jacoco:report]
    F --> G[/Coletar Métrica:<br>% Cobertura e Tempo/]
    G --> H[Configurar Plugin PITest]
    H --> I[Executar: mvn org.pitest:pitest-maven:mutationCoverage]
    I --> J[/Coletar Métrica:<br>% Mutation Score e Tempo/]
    J --> K[Registrar Dados na Planilha]
    K --> L{Existem mais<br>projetos?}
    L -- Sim --> B
    L -- Não --> M([Fim da Coleta])
    
    style A fill:#f9f,stroke:#333,stroke-width:2px
    style M fill:#f9f,stroke:#333,stroke-width:2px
    style G fill:#bbf,stroke:#333,stroke-width:1px,stroke-dasharray: 5 5
    style J fill:#bbf,stroke:#333,stroke-width:1px,stroke-dasharray: 5 5
