# Entrega 5: Avaliação de Validade (Ameaças e Mitigação)
**Aluno:** Ryan Cristian Oliveira Rezende
**Data:** 05/12/2025

---

## 13. Avaliação de Validade (Ameaças e Mitigação)

Todo experimento empírico está sujeito a ameaças que podem afetar a precisão dos resultados ou a capacidade de generalização. Abaixo, classifico as ameaças identificadas neste estudo segundo as quatro categorias de validade (Wohlin et al.).

### 13.1 Validade de Conclusão (Conclusion Validity)
Refere-se à capacidade de tirar conclusões estatísticas corretas sobre a relação entre o tratamento e o resultado.

* **Ameaça (Baixo Poder Estatístico):** Devido ao alto custo computacional do Teste de Mutação, a amostra será pequena (3 a 5 projetos). Isso impede a realização de testes estatísticos robustos (como Teste-T) com alta confiança.
    * **Mitigação:** O estudo focará em uma análise qualitativa e descritiva dos dados, em vez de buscar apenas significância estatística. Os "outliers" serão analisados caso a caso.

### 13.2 Validade Interna (Internal Validity)
Refere-se a fatores desconhecidos que podem influenciar o resultado sem que o pesquisador perceba.

* **Ameaça (Viés de Seleção / Instrumentação):** A escolha dos projetos open-source pode ser tendenciosa (ex: escolher projetos muito simples) ou os projetos podem falhar no build localmente devido a diferenças de ambiente (SO, versão do Java), gerando dados corrompidos.
    * **Mitigação:** Utilização de critérios de inclusão rígidos (build passando, uso de Maven padrão). Se um projeto falhar na configuração do PITest, ele será descartado e substituído, sem tentar "consertar" o código manualmente para não alterar o objeto de estudo.

### 13.3 Validade de Construto (Construct Validity)
Refere-se a se as métricas escolhidas realmente medem o que dizem medir (Qualidade/Robustez).

* **Ameaça (Mutantes Equivalentes):** O PITest pode gerar mutantes que são logicamente idênticos ao código original (ex: alterar um loop que não afeta o resultado final). O teste não mata esse mutante, e isso baixa a nota de Mutação injustamente.
    * **Mitigação:** Reconhecimento dessa limitação da ferramenta. Utilizaremos os operadores de mutação padrão do PITest, que são calibrados para minimizar (mas não eliminar) esse problema. Aceita-se a margem de erro como parte da técnica.

### 13.4 Validade Externa (External Validity)
Refere-se à capacidade de generalizar os resultados para a indústria como um todo.

* **Ameaça (Generalização Limitada):** Os resultados serão obtidos de bibliotecas Java de código aberto. Eles podem não representar a realidade de sistemas proprietários, microsserviços complexos ou sistemas legados de grandes empresas.
    * **Mitigação:** O escopo da conclusão será limitado explicitamente a "Projetos Java de Backend/Bibliotecas". Não faremos alegações de que o resultado se aplica a Frontend, Mobile ou outras linguagens.

---

### 13.5 Resumo das Ações de Mitigação
1.  Limitar o escopo das conclusões (não generalizar excessivamente).
2.  Seguir protocolo rígido de seleção de projetos (apenas builds estáveis).
3.  Utilizar configurações padrão ("defaults") das ferramentas para evitar manipulação de resultados.
