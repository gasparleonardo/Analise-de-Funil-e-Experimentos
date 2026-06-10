# Análise de Funil de Conversão e Otimização de Produto através de Testes A/B/A

## Descrição do Projeto
Este projeto foi desenvolvido com foco em Product Analytics e Growth para um aplicativo de e-commerce de produtos alimentares. O objetivo principal consistiu em mapear detalhadamente a jornada de compra dos clientes por meio da estruturação de um funil de conversão e, em seguida, avaliar cientificamente os resultados de um experimento de teste A/B/A. A análise determinou se a alteração das fontes visuais de toda a aplicação impactaria o comportamento dos utilizadores e as taxas de conversão, mitigando riscos financeiros antes da implementação definitiva em produção.

---

## Tecnologias e Ferramentas Utilizadas
* **Linguagem:** Python
* **Análise e Manipulação de Dados:** Pandas e NumPy
* **Análise Estatística e Validação:** SciPy (Módulo Stats) e Math
* **Visualização de Dados:** Matplotlib, Seaborn e Plotly (Gráficos Dinâmicos)
* **Ambiente de Desenvolvimento:** Jupyter Notebook

---

## Pipeline de Dados e Metodologia

### 1. Limpeza e Engenharia de Atributos (Data Prep)
* Processamento de uma base massiva contendo mais de 240.000 logs de eventos de utilizadores.
* Normalização e renomeação de colunas, eliminação de registos duplicados e tratamento de dados ausentes.
* Conversão de *timestamps* Unix originais para o formato nativo de data e hora (`datetime`).
* Criação de colunas derivadas para isolar componentes de data e hora para análises cronológicas específicas.

### 2. Análise de Consistência Temporal
* Construção de histogramas detalhados para avaliar a distribuição dos eventos ao longo do tempo.
* Identificação de uma assimetria nos dados (logs incompletos coletados de períodos anteriores), resultando na aplicação de um filtro cronológico estratégico para manter apenas os dados estáveis e completos (período de 1 a 7 de agosto).
* Avaliação do impacto da exclusão dos dados (descarte de apenas ~1% dos eventos, mantendo a amostragem estatisticamente robusta).

### 3. Modelagem do Funil de Vendas (User Journey)
* Identificação e contagem de frequência de todos os eventos principais do sistema: `MainScreenAppear` (Ecrã Principal), `OffersScreenAppear` (Ofertas), `CartScreenAppear` (Carrinho) e `PaymentScreenSuccessful` (Pagamento Concluído).
* Determinação da sequência lógica da jornada do utilizador.
* Cálculo da **taxa de conversão passo a passo** e identificação do principal gargalo de evasão do fluxo.
* Mensuração da percentagem total de utilizadores que iniciam o fluxo e concluem com sucesso a jornada de compra.

### 4. Análise e Validação de Testes Estatísticos (A/B/A)
* Divisão da base de dados em três grupos experimentais controlados por ID (`ExpId`): dois grupos de controlo com o design antigo (246 e 247) e um grupo de teste com as novas fontes (248).
* Execução de **Testes de Hipóteses de Proporção (Z-test)** com múltiplos níveis de significância ($\alpha = 0.05$ e $\alpha = 0.1$) para comparar a igualdade de proporções entre as amostras.
* Condução de 16 testes estatísticos sequenciais comparando os grupos de controlo entre si (validação do mecanismo de divisão A/A), e os grupos de controlo de forma isolada e combinada contra o grupo de teste (B).
* Aplicação da **Correção de Bonferroni** para ajustar o nível de significância e mitigar a ocorrência de falsos positivos (Erro Tipo I) devido às múltiplas comparações.

---

## Principais Insights & Impacto de Negócio
* **O Gargalo do Produto:** A análise do funil revelou que o maior vazamento de tráfego ocorre logo na primeira etapa: **38,1% dos utilizadores abandonam o aplicativo** ao passar do Ecrã Principal para o Ecrã de Ofertas. Após essa barreira, a retenção é alta (81% dos que veem ofertas avançam ao carrinho, e 95% dos que estão no carrinho concluem o pagamento).
* **Conversão Global:** Aproximadamente **47,7%** dos utilizadores que abrem o aplicativo completam toda a jornada e realizam uma compra.
* **Neutralidade do Experimento:** Os 16 testes estatísticos comprovaram que a variação nas taxas de conversão entre as novas fontes (Grupo B) e o design antigo (Grupos A1/A2) flutuou apenas entre 1% e 2%, mantendo um *p-valor* significativamente superior aos limites críticos ajustados.
* **Tomada de Decisão Baseada em Dados:** Ficou provado cientificamente que a alteração de design é **comercialmente neutra** (não prejudica nem alavanca as vendas). A diretoria recebeu a recomendação segura de que a implementação das fontes pode ser decidida puramente por critérios estéticos ou de performance técnica da equipa de Design, sem risco de queda no faturamento.

---

