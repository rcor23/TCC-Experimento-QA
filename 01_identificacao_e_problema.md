# Entrega 1: Identificação Básica, Contexto e Problema

## 1. Identificação básica

### 1.1 Título do experimento
Análise Comparativa de Eficácia na Detecção de Falhas: Cobertura de Código (Code Coverage) versus Teste de Mutação em Aplicações Java.

### 1.2 ID / código
EXP-QA-MUTATION-2025

### 1.3 Versão do documento e histórico de revisão
* **Versão atual:** v1.0
* **Histórico:**
    * 22/11/2025: Criação inicial do documento com definição de escopo e problema.

### 1.4 Datas
* **Criação:** 22/11/2025
* **Última atualização:** 22/11/2025

### 1.5 Autores
* **Nome:** Ryan Cristian Oliveira Rezende
* **Área:** Engenharia de Software / Qualidade de Software
* **Contato:** rcorezende@sga.pucminas.br

### 1.6 Responsável principal (PI / dono do experimento)
Ryan Cristian Oliveira Rezende

### 1.7 Projeto / produto / iniciativa relacionada
Trabalho de Conclusão de Curso (TCC) – Investigação sobre métricas de qualidade de software e confiabilidade de suítes de testes.

---

## 2. Contexto e problema

### 2.1 Descrição do problema / oportunidade
No desenvolvimento de software, a **Cobertura de Código (Code Coverage)** é usada por quase todas as empresas como a métrica padrão para medir a qualidade dos testes. É comum ver metas como "mínimo de 80% de cobertura" para aceitar um Pull Request.

O problema principal é a falsa sensação de segurança que isso gera: a chamada "ilusão de qualidade". É perfeitamente possível ter 100% de cobertura com testes fracos, que executam o código mas não têm asserções (assertions) capazes de pegar erros de lógica. Isso resulta em código que passa no CI/CD, mas quebra em produção.

A oportunidade deste trabalho é verificar na prática se o **Teste de Mutação (Mutation Testing)** — que insere bugs propositais no código para ver se os testes falham — é uma métrica mais honesta e confiável do que a cobertura tradicional.

### 2.2 Contexto organizacional e técnico
* **Ambiente:** O experimento será executado no meu notebook pessoal (Sistema Operacional Windows 10/11), garantindo isolamento de recursos durante a execução.
* **Stack Tecnológica:** Projetos open-source em **Java**.
* **Ferramentas:**
    * **JUnit 5:** Para rodar os testes unitários.
    * **JaCoCo:** Para medir a cobertura tradicional.
    * **PITest:** A ferramenta padrão de mercado para teste de mutação em Java (JVM).
* **Objeto de Estudo:** Módulos de lógica de negócios (Backend) com diferentes níveis de complexidade.

### 2.3 Trabalhos e evidências prévias
A literatura especializada em Engenharia de Software e Qualidade aponta que cobertura de código é necessária, mas não suficiente.
Estudos da área indicam que suítes de teste com alta cobertura frequentemente deixam passar falhas críticas. Por outro lado, o teste de mutação costuma ser mais eficaz em expor a fragilidade desses testes, embora seja menos usado na indústria por ser considerado "pesado" computacionalmente. A ideia é investigar esse trade-off.

### 2.4 Referencial teórico e empírico essencial
O experimento vai se basear em quatro conceitos principais:
1.  **Code Coverage (Cobertura):** Métrica simples que diz quantas linhas de código foram executadas.
2.  **Mutation Testing (Teste de Mutação):** Técnica que altera o código original (criando "mutantes", ex: trocando um `+` por `-`) para testar a qualidade do teste. Se o teste falhar, o mutante "morreu" (o que é bom).
3.  **Hipótese do Programador Competente:** A teoria de que a maioria dos bugs são erros simples de sintaxe ou lógica básica.
4.  **Efeito de Acoplamento:** A ideia de que testes capazes de pegar erros simples (mutantes) também acabam pegando erros complexos indiretamente.
